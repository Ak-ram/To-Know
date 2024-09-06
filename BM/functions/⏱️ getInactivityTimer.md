
![login](https://github.com/user-attachments/assets/89bfbfb8-0a37-447e-b341-1cf9a4eb29ae)

## شرح دالة `getInactivityTimer`

الـ `getInactivityTimer` دي وظيفتها إنها تتابع نشاط المستخدم على الصفحة. يعني لو المستخدم مبقاش متفاعل مع الصفحة لمدة معينة، نعرض له رسالة تحذير. لو فضل مش متفاعل لوقت أطول، نعتبر الجلسة انتهت ونسجل الخروج بشكل تلقائي.

### الخطوات بالتفصيل:

١. **تحديد الأحداث اللي هنتابعها**:

   ```typescript
   private inactivityTimerEvent: Array<any>[] = [
     [document, 'click'],
     [document, 'wheel'],
     [document, 'scroll'],
     [document, 'mousemove'],
     [document, 'keyup'],
     [window, 'resize'],
     [window, 'scroll'],
     [window, 'mousemove'],
     [window, 'reload'],
   ];

   let observableArray$: Observable<any>[] = [];
   this.inactivityTimerEvent.forEach((x) => {
     observableArray$.push(fromEvent(x[0], x[1]));
   });
   ```
   هنا بنحدد كل الأحداث اللي ممكن المستخدم يعملها على الصفحة زي (click)، (mousemove)، (scroll)، وغيرهم. بعد كده، بنحول كل حدث إلى `Observable` عشان نقدر نتابعه.

٢. **دمج كل الأحداث في Observable واحد**:

   ```typescript
   return merge(...observableArray$).pipe(...)
   ```
   بنستخدم `merge` لدمج كل الـ `Observables` اللي أنشأناها في واحد. يعني لو أي حدث من الأحداث دي حصل، هننفذ عملية معينة.

   المفروض نبدأ عد تنازلي لغاية ما نوصل لقيمة معينة وهي `AppConfigService.data.inactivityTimeMins * 2`. العد ده هيتوقف لو المستخدم عمل أي تفاعل على الصفحة. لو المستخدم وقف تفاعل لمده 30 ثانية، هنبدأ العد من جديد.

٣. **بدء التوقيت وتنبيهات الجلسة**:

   ```typescript
   startWith(0),
   tap(() => this.sessionWarning$.next(false)),
   ```
   نبدأ التفاعل بقيمة ابتدائية `0` (يعني لو في نشاط من البداية، بنبدأ العد). نبعث تنبيه بأن الجلسة مش هتنتهي لو حصل تفاعل على الصفحة.

٤. **مراقبة فترة عدم النشاط**:

   ```typescript
   private inactivityTime: number = AppConfigService.data.inactivityTimeMins * 2;

   switchMap((ev) => interval(1000 * 30).pipe(take(this.inactivityTime))),
   tap((res) => {
     if (res === this.inactivityTime - 2) {
       this.sessionWarning$.next(true);
     }
   }),
   skipWhile((x) => {
     return x != this.inactivityTime - 1;
   })
   ```
   - بعد كل حدث، نبدأ عد جديد كل 30 ثانية باستخدام `interval`.
   - لو المستخدم مفيش تفاعل، نراقب الوقت. لو قربنا من نهاية فترة عدم النشاط (مثلاً، 2 ثانية قبل النهاية)، نعرض رسالة تحذير.
   - نستخدم `skipWhile` لتجاهل القيم لحد ما نوصل لقيمة واحدة ثانية قبل نهاية فترة عدم النشاط.

### مثال توضيحي:

- **السيناريو**: فترة عدم النشاط هي 10 دقائق.
- **العمل**: الوظيفة تتابع تفاعل المستخدم على الصفحة. لو المستخدم مفيش تفاعل لمدة 9 دقائق و58 ثانية، تظهر له رسالة تحذير تقول "انت على وشك أن يتم تسجيل خروجك بسبب عدم النشاط".
- **إذا استمر عدم التفاعل**: بعد 10 دقائق، نعتبر الجلسة انتهت ونقوم بتسجيل الخروج أو إغلاق الجلسة.

