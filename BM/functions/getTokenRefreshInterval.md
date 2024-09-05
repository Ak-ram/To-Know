
### شرح `getTokenRefreshInterval`

```typescript
getTokenRefreshInterval(): number {
  return (
    +this.storage.getItem(STORAGE_CONST.TOKEN_REFRESH_INTERVAL) ||
    AppConfigService.data.refreshTokenTimeInMinutes
  );
}
```

الفنكشن دي وظيفتها إنها ترجعلك مدة التجديد بتاعة الـ token.

١. **استرجاع القيمة من الـ storage**:
   - أول حاجة، الفنكشن بتحاول تجيب القيمة المخزنة في الـ storage باستخدام المفتاح `STORAGE_CONST.TOKEN_REFRESH_INTERVAL`. القيمة دي عادة بتكون نص (string) مش عدد (number). 
   - **`+`**: هنا، بنستخدم `+` لتحويل النص ده لعدد. يعني لو القيمة اللي جايه من الـ storage مثلاً "10"، هتحول لـ 10.

٢. **التعامل مع القيمة الافتراضية**:
   - لو القيمة اللي جبتها من الـ storage مش موجودة أو غير صحيحة (يعني لو بقت `NaN` بعد التحويل)، هنا بنستخدم **`||`** علشان نرجع القيمة الافتراضية.
   - **`AppConfigService.data.refreshTokenTimeInMinutes`**: دي القيمة الافتراضية اللي بتستخدم لو القيمة في الـ storage مش موجودة. القيمة دي تكون معمول لها إعداد في التطبيق، وبتكون بالدقائق.

### الخلاصة:

الفنكشن دي بتحاول تجيب مدة التجديد من الـ storage، ولو مش لقيت قيمة صحيحة، هترجعلك القيمة الافتراضية من إعدادات التطبيق. ده بيساعدك إنك تضمن إنك دايمًا عندك مدة تجديد صالحة للـ token.
