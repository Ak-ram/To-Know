
### شرح `processLoginResponse`

```typescript
processLoginResponse(resData: any, corporateId?: string) {
```

- الفنكشن بتبدأ بإنها تستقبل حاجتين: `resData` اللي هو الـ response اللي بيرجع من السيرفر بعد الـ login، و`corporateId` اللي هو اختياري (optional) وبيكون رقم الشركة لو المستخدم عنده أكتر من شركة مرتبطة بيه.

```typescript
  if (resData.body && resData.body.data) {
```

- هنا بنتأكد إن الـ response فيه بيانات (`body` و`data`) عشان نبدأ نشتغل عليهم.

```typescript
    this.widgetsConfig = [];
```

- بنفرغ الـ `widgetsConfig` اللي غالبًا بيحتوي على إعدادات الـ widgets الخاصة بالمستخدم.

```typescript
    const headers: Array<Header> = [];
    if (AppConfigService.data.headerConfig && AppConfigService.data.headerConfig.length > 0) {
      AppConfigService.data.headerConfig.forEach((x) => {
        if (resData.headers.has(x)) {
          headers.push({ key: x, value: resData.headers.get(x) });
        }
      });
    }
```

- هنا بنشوف إذا كان فيه إعدادات headers خاصة بالتطبيق (`headerConfig`)، وبعدين بنشيك على الـ headers اللي جت في الـ response، لو لقيناها بنضيفها في مصفوفة headers.

### استخراج وفك الـ token

```typescript
    const tokenInfo: any = this.jwtDecode(resData.body.data.token);
    const expirationTime = new Date(tokenInfo.exp * 1000).getTime().toString();
    const expirationDate = new Date(tokenInfo.exp * 1000).toString();
```

- هنا بنفك (decode) الـ token اللي جاي من الـ response باستخدام `jwtDecode`. الهدف من كده إننا نعرف معلومات زي وقت انتهاء الـ token (`exp`) عشان نقدر نتعامل معاه. بنحول الـ expiration time لتاريخ ووقت نقدر نستخدمهم لاحقًا في التطبيق.

### ضبط الـ token refresh interval

```typescript
    this.setTokenRefreshInterval(new Date(tokenInfo.exp * 1000));
```

- بنستخدم وقت انتهاء الـ token اللي استخرجناه عشان نضبط الفنكشن اللي بتحدث الـ token تلقائيًا قبل انتهاء صلاحيته.

### تكوين الـ User object

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

- هنا بنبني object جديد للمستخدم بناءً على البيانات اللي جت في الـ response. البيانات دي بتشمل اسم المستخدم، رقم الشركة، الـ token، صلاحيات المستخدم، الشركات اللي يقدر يتعامل معاها، وغيرها. 

### التعامل مع الـ corporateId

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

- لو المستخدم مرتبط بأكتر من شركة، هنا بنحدد أي شركة مرتبطة بالمستخدم بناءً على الـ `corporateId` اللي ممكن يكون موجود أو بنستخدم الشركة الافتراضية.

### تخزين بيانات المستخدم

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
  }
```

- في الآخر بنعمل update للـ observable (`user$`) عشان نخلي بيانات المستخدم متاحة في التطبيق، وبعدين بنخزن بيانات المستخدم والـ expiration time في الـ storage عشان نستخدمهم لاحقًا من غير ما نضطر نطلب البيانات تاني من السيرفر.

### الخلاصة:
الفنكشن دي بتعالج الـ login response عن طريق فك الـ token، ضبط وقت تجديده، تكوين بيانات المستخدم، وتخزينها في الـ storage عشان نقدر نستخدمها جوه التطبيق.
