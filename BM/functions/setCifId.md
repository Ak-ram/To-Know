## ال setCifId() function: 

### 1. **استخراج بيانات المستخدم المخزنة**:
```typescript
const userData = JSON.parse(this.storage.getItem(this._userDataIT));
```
- هنا بنجيب بيانات المستخدم من المخزن المحلي (`local storage`) ونعملهم `parse` من صيغة JSON لـ Object عشان نقدر نتعامل معاهم.
- لو البيانات مش موجودة، الفنكشن بترجع `null`.

---

### 2. **التأكد من وجود بيانات المستخدم**:
```typescript
if (!userData) {
  return of(null);
}
```
- لو الـ `userData` اللي جاي من الـ storage فاضية، الفنكشن بتطلع وتستخدم `of(null)` عشان ترجع قيمة فارغة من نوع `Observable`.

---

### 3. **جلب تفاصيل CIF**:
```typescript
return this.userApiGateway.getCifDetails(cifId).pipe(
```
- هنا بيتم الاتصال بـ API عشان يجيب تفاصيل الـ CIF (Customer Information File) بناءً على الـ `cifId` اللي اتبعت للـ function.
  
---

### 4. **تخزين بيانات الـ CIF وتحديث الخصائص**:
```typescript
tap((res: IApiResponse<{ corporateFullName: string; soloCorporate: boolean; disablePaymentLink: boolean }>) => {
  this.corporateFullName = res.data.corporateFullName;
  this.soloCorporate = res.data.soloCorporate;
  this.disablePaymentLink = res.data.disablePaymentLink;
  this.userCif = cifId;
```
- هنا بتخزن معلومات مهمة زي:
  - ال `corporateFullName`: اسم الشركة بالكامل.
  - ال `soloCorporate`: هل الشركة دي قائمة بذاتها أو جزء من مجموعة.
  - ال `disablePaymentLink`: هل الـ Payment Link معطل.
- كمان بتحدّث الـ `userCif` بالـ `cifId` الجديد.

---

### 5. **تحديث بيانات المستخدم**:
```typescript
const user = new User(
  userData.customerName,
  cifId,
  userData.headers,
  userData._token,
  userData.tokenExpirationDate,
  userData.authorized,
  userData.transactionTypes,
  this.patchEnabledTransactionSubType(transactionSubTypes),
  userData.isAdmin,
  userData.companies,
  userData.changePassword,
  userData.otpEnabled,
  userData.lastLogin,
  userData.firstName,
  userData.lastName,
  userData.inquiryUser
);
```
- هنا بيتم تكوين Object جديد للمستخدم اسمه `user` باستخدام البيانات اللي كانت موجودة مع تحديث الـ `cifId` وتعديل الـ `transactionSubTypes` باستخدام الفنكشن `patchEnabledTransactionSubType`.

---

### 6. **تخزين بيانات المستخدم المحدثة**:
```typescript
this.storage.setItem({
  ...this._userDataIT,
  value: JSON.stringify(user),
});
```
- بعد تكوين بيانات المستخدم الجديدة، بيتم تخزينها في المخزن المحلي (`local storage`).

---

### 7. **تحديث Observable المستخدم**:
```typescript
this.user$.next(user);
```
- بيتم تحديث الـ `user$` Observable عشان أي مكان في التطبيق بيعتمد عليه يقدر يشوف التغييرات اللي حصلت.

---

### ال **CIF و SoloCorporate**:

١- ال CIF : هو رقم تعريف العميل (Customer Information File)، وبيستخدم في البنوك أو الشركات عشان يربط كل المعاملات والتفاصيل المتعلقة بالعميل تحت رقم موحد.

٢- ال SoloCorporate : ده نوع من العملاء اللي بيمثل كيان منفرد لشركة معينة، وبيتم التعامل معاه بشكل مختلف عن العملاء العاديين.

