
## شرح setTokenRefreshInterval

```typescript
setTokenRefreshInterval(expirationDateTime: Date): void {
```

- الفنكشن دي بتستقبل معطى واحد هو `expirationDateTime`، وده بيكون تاريخ ووقت انتهاء صلاحية الـ token.

```typescript
  const diffInmilliSec = differenceInMilliseconds(
    expirationDateTime,
    new Date()
  );
```

- هنا، الفنكشن بتستخدم دالة `differenceInMilliseconds` (اللي من مكتبة `date-fns`) لحساب الفرق بين تاريخ انتهاء صلاحية الـ token (`expirationDateTime`) والوقت الحالي (`new Date()`). الفارق ده بيكون بالميلي ثانية.

```typescript
  const tokenRefreshInterval = Math.round(diffInmilliSec / (60 * 1000)) - 1;
```

- بعد كده، بتقسم الفرق (الـ `diffInmilliSec`) على 60,000 (اللي هي عدد الميلي ثانية في الدقيقة) لتحويله لدقائق. بعد كده، بتستخدم `Math.round` لتقريب الرقم لأقرب عدد صحيح. ثم بتخصم 1 من القيمة، عشان تدي وقت أقل بديقة واحدة لتجديد الـ token قبل انتهاء صلاحيته.

```typescript
  this.storage.setItem({
    ...STORAGE_CONST.TOKEN_REFRESH_INTERVAL,
    value: tokenRefreshInterval.toString(),
  });
```

- أخيرًا، بتخزن `tokenRefreshInterval` في الـ sesstion storage باستخدام مفتاح مخصص (`STORAGE_CONST.TOKEN_REFRESH_INTERVAL`). بتقوم بتحويل القيمة إلى سلسلة نصية (`toString`) لأن الـ storage عادة بتخزن القيم كنصوص.
  
<img width="100%" alt="Screenshot 2024-09-06 at 11 15 10 AM" src="https://github.com/user-attachments/assets/4fd6ba8a-a6b2-4a5d-a0ba-efbda912a20e">

### الخلاصة:


فنكشن `setTokenRefreshInterval` بتحدد الوقت الذي يحتاجه التطبيق لتجديد الـ token قبل انتهاء صلاحيته. الفنكشن بتحسب الفرق بين الوقت الحالي وتاريخ انتهاء صلاحية الـ token، وتحوله لدقائق، وتخزن قيمة التجديد في الـ session storage. الهدف من كده هو ضمان إن التطبيق يجدد الـ token بشكل مناسب قبل انتهاء صلاحيته.

