## شرح الـ sessionExists

الـ function دي بتستخدم عشان نعرف إذا كان الـ session ما زال شغال أو انتهى. يعني لو المستخدم ممكن يفضل يستخدم الـ session بتاعه ولا لازم يسجل دخول تاني.

#### تفاصيل الخطوات:

١. **استرجاع وقت انتهاء الـ session**:
   ```typescript
   const expirationDate = this.storage.getItem(this._expirationTimeIT);
   ```
هنا بنجيب وقت انتهاء الـ session من (`localStorage`). وال `this._expirationTimeIT` هو ال key اللي بنخزن فيه وقت انتهاء الـ session.

٢. **التحقق من صلاحية الـ session**:
   ```typescript
   if (
     expirationDate &&
     // handles browser and tab crashing scenarios
     +expirationDate + 1 * 60 * 1000 > new Date().getTime()
   )
     return true;
   return false;
   ```
   - لو كان فيه قيمة لوقت انتهاء الـ session، يعني فيه وقت مسجل.
   - بنقارن الوقت الحالي مع وقت انتهاء الـ session زائد دقيقة واحدة (1 * 60 * 1000 مللي ثانية). لو الوقت الحالي أقل من وقت انتهاء الـ session زائد دقيقة، يبقى الـ session لسه شغالة.
   - لو الوقت الحالي أكبر من وقت انتهاء الـ session زائد دقيقة، يبقى الـ session انتهت.

#### مثال توضيحي:

**السيناريو**: افترض إن وقت انتهاء الـ session المخزن هو 5 دقائق من دلوقتي.
  - لو حاول المستخدم يسجل دخول بعد 4 دقائق، الـ session لسه شغالة، وهترجع `true`.
  - لو حاول يسجل دخول بعد 6 دقائق، يبقى الـ session انتهت، وهترجع `false`.


الـ function دي بتساعدنا نعرف إذا كان المستخدم لسه يقدر يستخدم الـ session ولا لازم يسجل دخول تاني، وده مهم عشان نقدر ندير الـ sessions بشكل صحيح ونتجنب المشاكل اللي بتظهر لما الـ session تنتهي أو يحصل مشكلة في المتصفح.
