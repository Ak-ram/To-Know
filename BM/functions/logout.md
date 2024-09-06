## الـ login function

 الـ `logout` بتعمل تسجيل خروج للمستخدم وبتشيل كل البيانات المتعلقة بيه، سواء كان الخروج بسبب انتهاء الجلسة (session timeout) أو بسبب أن المستخدم نفسه ضغط على زر الـ logout.

### تقسيم الكود:

١. **إغلاق كل الـ overlays المفتوحة**:
   ```typescript
   this.overlayService.closeAll();
   ```
   - دي بتشيل أي نوافذ أو شاشات إضافية ظهرت فوق التطبيق الأساسي، وبتقفلها لما المستخدم يعمل تسجيل خروج.

٢. **التنقل للصفحة الرئيسية لو الخروج مش بسبب session timeout**:
   ```typescript
   if (!sessionTimeOut) this.navigateTo('/');
   ```
   - هنا لو الخروج مش بسبب انتهاء الجلسة (session timeout)، الكود بيرجع المستخدم لصفحة البداية أو الصفحة الرئيسية.

٣. **التعامل مع بيانات المستخدم**:
   ```typescript
   return this.userData$.pipe(
     take(1),
     filter((userData) => !!userData),
     switchMap(() => this.userApiGateway.logout()),
   ```
   - بيأخذ بيانات المستخدم الحالية من الـ `userData$`، وبيتأكد إن في بيانات (مش فاضية) باستخدام الـ `filter`.
   - بعدها، بيبعت طلب للـ API عن طريق `userApiGateway.logout()` عشان يعمل تسجيل خروج على مستوى السيرفر كمان.

٤. **مسح بيانات المستخدم**:
   ```typescript
   tap(() => {
     this.clearUserData();
     if (!sessionTimeOut) this.navigateTo('/');
   }),
   ```
   - هنا بيتم مسح كل البيانات الخاصة بالمستخدم من الـ storage أو الذاكرة المحلية عن طريق الـ `clearUserData` function.
   - بعد مسح البيانات، لو الجلسة مخلصتش بسبب timeout، بيرجع المستخدم مرة تانية للصفحة الرئيسية.

٥. **التعامل مع الأخطاء**:
   ```typescript
   catchError((err) => {
     this.clearUserData();
     if (!sessionTimeOut) this.navigateTo('/');
     return of(EMPTY);
   })
   ```
   - لو حصل أي خطأ أثناء عملية الـ logout، بيتم برضه مسح بيانات المستخدم وارجاعه للصفحة الرئيسية.
   - بيتم إرجاع `EMPTY` للتأكد إن الـ Observable بيتعامل مع الأخطاء وما بيكسرش الـ stream.

---

### ملخص:
- `logout()` بتعمل تسجيل خروج، بتمسح بيانات المستخدم، وبتنقله للصفحة الرئيسية. لو كان الخروج بسبب session timeout، ما بترجعوش تلقائيًا للصفحة الرئيسية. ولو حصل أي خطأ في العملية، برضه بتضمن إنه البيانات اتحذفت والـ logout تم.
