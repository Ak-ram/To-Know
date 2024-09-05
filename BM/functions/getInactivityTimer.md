### شرح getInactivityTimer

ال function دي هدفها تتبع نشاط المستخدم على الصفحة. لو المستخدم متفاعلش مع الصفحة لمدة معينة، بنعرض ليه رسالة تحذير ونستني شويه لو معملش تفاعل بنعتبر الجلسة انتهت ونعمله logout تلقائي.


#### الخطوات بالتفصيل:

١. **جمع الأحداث التي نراقبها**:
   
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
هنا بنجمع كل ال events اللي ممكن يستخدمها المستخدم على الصفحة، زي (click)، (mousemove)، (scroll)، إلخ. 
ال `inactivityTimerEvent` هي قائمة تحتوي على هذه ال events دي (مثلاً، `[document, 'click']`).

٢. **دمج كل ال events دي في Observable واحد**:
   ```typescript
   return merge(...observableArray$).pipe(...)
   ```
نستخدم `merge` لدمج جميع الـ `Observables` اللي أنشأناها من الأحداث في Observable واحد. يعني لو أي من هذه الأحداث حدث، هنتابع التفاعل.

٣. **بدء التوقيت وتنبيهات الجلسة**:
   ```typescript
   startWith(0),
   tap(() => this.sessionWarning$.next(false)),
   ```
نبدأ التفاعل بقيمة ابتدائية `0` (يعني لو في نشاط منذ البداية، بنبدأ بالعدّ). 
نبعث تنبيه بأن الجلسة مش هتنتهي، لو حصل تفاعل على الصفحة.

٤. **مراقبة فترة عدم النشاط**:
   ```typescript
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
   - بعد كل حدث، نبدأ عدّ من جديد كل 30 ثانية (باستخدام `interval`).
   - لو لم يتفاعل المستخدم، نراقب الوقت. لو قربنا من نهاية فترة عدم النشاط (مثلاً، 2 ثانية قبل النهاية)، نعرض رسالة تحذير.
   - نستخدم `skipWhile` لتجاهل القيم لحد ما نوصل لقيمة واحدة ثانية قبل نهاية فترة عدم النشاط.

#### مثال توضيحي:

- **السيناريو**: فترة عدم النشاط هي 10 دقائق.
- **العمل**: الوظيفة تتبع التفاعل على الصفحة. لو المستخدم مفيش تفاعل لمدة 9 دقائق و58 ثانية، تظهر له رسالة تحذير، تعني "انت على وشك أن يتم تسجيل خروجك بسبب عدم النشاط".
- **إذا استمر عدم التفاعل**: بعد 10 دقائق، نعتبر الجلسة انتهت ونقوم بإجراءات مناسبة مثل تسجيل الخروج أو إغلاق الجلسة.

