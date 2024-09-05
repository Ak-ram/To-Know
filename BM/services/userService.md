## ال processLoginResponse(resData:any,corporateId:string)

الـ function اللي اسمها `processLoginResponse` دي بتعمل إيه باختصار، بتتعامل مع الـ response اللي بيرجع بعد الـ login. خليني أشرحلك التفاصيل:

١. **التأكد من إن الـ response فيه بيانات**:
   ```typescript
   if (resData.body && resData.body.data) {
   ```
   هنا بتتأكد إن فيه بيانات جاية في الـ response من الـ server، تحديدًا في `body` و `body.data`.

٢. **تهيئة الـ headers**:
   ```typescript
   this.widgetsConfig = [];
   const headers: Array<Header> = [];
   if (AppConfigService.data.headerConfig && AppConfigService.data.headerConfig.length > 0) {
     AppConfigService.data.headerConfig.forEach((x) => {
       if (resData.headers.has(x)) {
         headers.push({ key: x, value: resData.headers.get(x) });
       }
     });
   }
   ```
   هنا بيعمل reset للـ `widgetsConfig` وبعدين بيعمل array فاضية اسمها `headers`. لو فيه إعدادات headers في التطبيق (`AppConfigService.data.headerConfig`)، بيبدأ يشيك على الـ headers اللي جاية في الـ response (`resData.headers`) ويخزنها في array لو موجودة.

٣. **فك الـ token واحتساب وقت انتهاء الصلاحية**:
   ```typescript
   const tokenInfo: any = this.jwtDecode(resData.body.data.token);
   const expirationTime = new Date(tokenInfo.exp * 1000).getTime().toString();
   const expirationDate = new Date(tokenInfo.exp * 1000).toString();
   ```
   بيستخدم `jwtDecode` عشان يفك الـ token ويعرف المعلومات اللي جواه (زي وقت الانتهاء). بعدها بيحسب وقت انتهاء الصلاحية (expiration time) وبيحوله لرقم أو تاريخ.

٤. **ضبط فترة تحديث الـ token**:
   ```typescript
   this.setTokenRefreshInterval(new Date(tokenInfo.exp * 1000));
   ```
   بيحدد فترة تحديث الـ token تلقائيًا بناءً على الوقت اللي فاضل لانتهاء صلاحيته.

٥. **إنشاء كائن المستخدم (User object)**:
   ```typescript
   const user = new User(
     resData.body.data.customerName,
     resData.body.data.corporateId,
     headers,
     resData.body.data.token,
     expirationDate,
     resData.body.data.authorized,
     this.patchCustomTranscationType(resData.body.data.transactionTypes),
     this.patchEnabledTransactionSubType(resData.body.data?.enabledTransactionSubType),
     resData.body.data.userType === 'BANKADMIN',
     resData.body.data.companies,
     resData.body.data.changePassword,
     resData.body.data.otpEnabled,
     resData.body.data.lastLogin,
     resData.body.data.firstName,
     resData.body.data.lastName,
     !!resData.body.data.inquiryUser,
     !!resData.body.data.internetBanking
   );
   ```
   هنا بيعمل object للمستخدم (user) بناءً على البيانات اللي جاية في الـ response زي الاسم، الـ token، وبيانات الشركات اللي المستخدم شغال معاها، وغيره.

٦. **تحديد الـ corporateId**:
   ```typescript
   this.userCifList = user.companies;
   let _corporateId: string = user.cifId;
   if (corporateId) {
     _corporateId = this.userCifList.findIndex((item) => item === corporateId) > -1 ? corporateId : user.cifId;
   }
   const cifId = this.userCifList.find((item) => item === _corporateId) || this.userCifList[0];
   user.cifId = cifId;
   this.userCif = user.cifId;
   ```
   لو فيه `corporateId` متبعت، بيشوف إذا كان الـ `corporateId` ده موجود ضمن الشركات المرتبطة بالمستخدم. لو موجود، بيستخدمه، لو مش موجود، بياخد الـ corporate ID الأساسي للمستخدم.

٧. **تحديث البيانات في الـ observable والـ storage**:
   ```typescript
   this.user$.next(user);
   this.storage.setItem({
     ...this._userDataIT,
     value: JSON.stringify(user),
   });
   this.storage.setItem({
     ...this._expirationTimeIT,
     value: expirationTime,
   });
   this.storage.setItem({
     ...STORAGE_CONST.ANNOUNCEMENT,
     value: 'Y',
   });
   ```
   - بيحدث الـ observable `user$` عشان يبعت بيانات المستخدم الجديدة.
   - بيخزن بيانات المستخدم ووقت انتهاء الصلاحية في الـ local storage.
   - وبيحط قيمة "Y" للإعلان في الـ storage.

باختصار، الـ function دي بتاخد الـ response اللي بيرجع بعد الـ login، تفك الـ token، تعمل كائن للمستخدم، وتخزن البيانات في الـ local storage و الـ observable عشان تستخدمها بعدين.
