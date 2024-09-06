
## **الـ login function :**

```typescript
login(
  username: string,
  password: string,
  recaptchaToken?: string,
  campaign?:Campaign
): Observable<any> {
```

الـ function دي هي اللي بتقوم بعملية تسجيل الدخول، وبتاخد أربع متغيرات:

١. ال `username`: اسم المستخدم اللي هيدخله.

٢. ال `password`: كلمة السر اللي هيدخلها.

٣. ال `recaptchaToken`: لو فيه كود reCAPTCHA علشان يتأكد من إن المستخدم مش روبوت.

٤. ال `campaign`: بيمثل أي حملة تسويقية مرتبطة بتسجيل الدخول.**


---

### ** التآكد من عدم وجود session اخري :**

```typescript
this.checkExistingSession();
```
بتشوف لو فيه session موجودة بالفعل للمستخدم، وبتعمل إعادة توجيه (redirect) لو فيه session نشطة.
- 

### **عمل generate لرقم عشوائي:**

```typescript
const random = '' + Math.floor(100000 + Math.random() * 900000);
```
هنا بيتم تكوين رقم عشوائي مكوّن من 6 أرقام، بنستخدمه في التشفير بعد كده.

### **الحصول على public key  لتشفير بيانات المستخدم:**

```typescript
return this.encryptionRSAService
  .getRsaPublicKey(JSON.stringify({ username, password }))
  .pipe(
```
بنستخدم encryption service اسمها (RSA) علشان نجيب public key اللي هنشفر بيه اسم المستخدم وكلمة المرور.

### **إرسال بيانات الدخول للـ API:**

```typescript
    switchMap((userObject) =>
      this.userApiGateway.auth(userObject, random, recaptchaToken, campaign).pipe(
```
- بعد ما نجيب ال public key، بنبعت البيانات المشفرة ديه للـ API من خلال `auth` function اللي في `userApiGateway`.
- بنبعت مع البيانات المتشفرة الرقم العشوائي اللي طلعناه فوق (random) وكمان `recaptchaToken` لو موجود و`campaign`.

### **معالجة رد الـ API:**

```typescript
        tap((resData) => {
          this.processLoginResponse(resData);
        }),
```
- بعد ما الـ API يرد علينا، بننادي على function اسمها [processLoginResponse]([t](https://github.com/Ak-ram/To-Know/blob/main/BM/functions/processLoginResponse.md)) علشان نتعامل مع response بتاع الـ login.

### 7. **الحصول على تفاصيل CIF الخاصة بالمستخدم:**

```typescript
        switchMap((resLogin) => {
          return this.userApiGateway.getCifDetails(this.userCif).pipe(
```
بعد ما يسجل الدخول بنجاح، بنطلب تفاصيل إضافية عن المستخدم عن طريق `getCifDetails` اللي بترجع تفاصيل CIF (زي اسم الشركة وغيره).

### 8. **تحديث بيانات المستخدم:**

```typescript
            tap((res: any) => {
              this.corporateFullName = res.data.corporateFullName;
              this.soloCorporate = res.data.soloCorporate;
              this.disablePaymentLink = res.data.disablePaymentLink;
            }),
```
هنا بنقوم بتحديث بيانات المستخدم اللي رجعت من الـ API زي:
١. ال `corporateFullName`: اسم الشركة بالكامل.
٢. ال `soloCorporate`: هل المستخدم فردي أو تابع لشركة.
٣. ال `disablePaymentLink`: لو لينك الدفع مش شغال.


### 9. **إرجاع نتيجة الـ login:**

```typescript
            map((_) => resLogin)
```
في النهاية، بنرجع نتيجة تسجيل الدخول اللي حصلنا عليها من الـ API بعد تحديث بيانات المستخدم.

### 10. **الـ Observable النهائي:**

```typescript
          );
        })
      )
    );
```
الـ `Observable` اللي بيرجع بيكون فيه كل البيانات المتعلقة بتسجيل الدخول وتفاصيل المستخدم.
