## aesCbcEncrypt(message, random) 🔐
الـ **تشفير** في العموم هو زي تحويل الكلام أو البيانات لشكل تاني علشان نحمى المعلومات من أي حد مش مفروض يوصلها. في الكود ده، بنستخدم طريقة تشفير اسمها **AES** (اللي هي Advanced Encryption Standard) وبتشتغل بطريقة معينة اسمها **CBC** (Cipher Block Chaining).

### الكود:

```typescript
private aesCbcEncrypt(message, random): string {
  const key = 'BF1C6D7727' + random;
  const iv = '961E9031E2CAD84A';

  const keyHex = CryptoJS.enc.Utf8.parse(key);
  const ivHex = CryptoJS.enc.Utf8.parse(iv);
  const messageHex = CryptoJS.enc.Utf8.parse(message);
  const encrypted = CryptoJS.AES.encrypt(messageHex, keyHex, {
    iv: ivHex,
    mode: CryptoJS.mode.CBC,
    padding: CryptoJS.pad.Pkcs7,
  });
  return encrypted.toString();
}
```

### شرح الكود:

#### ١. **إنشاء المفتاح**:
    
```typescript
const key = 'BF1C6D7727' + random;
```

    
- **المفتاح**: ده زي كلمة السر اللي بنستخدمها علشان نشفر ونفك تشفير البيانات.
- **المفتاح هنا**: بيبدأ بسلسلة ثابتة `'BF1C6D7727'`، وبعدين بنضيف ليها قيمة عشوائية `random` علشان نعمل مفتاح أكتر أمانًا.
  #

#### ٢. **تحديد الـ Initialization Vector (IV)**:
```typescript
const iv = '961E9031E2CAD84A';
```
- **الـ IV**: ده قيمه بنستخدمها علشان نزيد أمان التشفير، ال IV ده لو:
  - متغير هيرجعلي encrupted key مختلف في كل مرة حتى لو البيانات نفسها لم تتغير.
  - ثابت (constant or hard coded) هيرجعلي نفس ال encrupted key  في كل مرة إذا كانت البيانات هي نفسها.
    
#### ٣. **تحويل النصوص لصيغة Hex**:
```typescript
const keyHex = CryptoJS.enc.Utf8.parse(key);
const ivHex = CryptoJS.enc.Utf8.parse(iv);
const messageHex = CryptoJS.enc.Utf8.parse(message);
```
- **التحويل**: النصوص (المفتاح، الـ IV، الرسالة) بنحولها لصيغة خاصة اسمها Hex علشان الـ CryptoJS تقدر تفهمها وتشتغل بها.
  

#### ٤. **تشفير الرسالة**:
```typescript
  const encrypted = CryptoJS.AES.encrypt(messageHex, keyHex, {
    iv: ivHex,
    mode: CryptoJS.mode.CBC,
    padding: CryptoJS.pad.Pkcs7,
  });
```
- **تشفير الرسالة**: بنستخدم خوارزمية AES علشان نشفر الرسالة.
- **الـ mode: CBC**: ده وضع للتشفير بيخلي التشفير أكتر أمانًا.
- **الـ padding: Pkcs7**: ده أسلوب لتعبئة الرسالة علشان يكون طولها مناسب للتشفير.

#### ٥. **إرجاع الرسالة المشفرة**:
```typescript
return encrypted.toString();
```
 في الآخر، بنعمل return للنص المشفر ده.



### مثال: 
```typescript
// A constant IV yields identical results for the same input.
   console.log('1',this.userService.aesCbcEncrypt('akram',212)); // mum1rnfOnVEiCEKQTQQ3Vw== 
   console.log('2',this.userService.aesCbcEncrypt('akram',212)); // mum1rnfOnVEiCEKQTQQ3Vw==

// Change random value to get different results:
   console.log('1',this.userService.aesCbcEncrypt('akram',212)); // mum1rnfOnVEiCEKQTQQ3Vw== 
   console.log('2',this.userService.aesCbcEncrypt('akram',22)); // wTV6RUFTWou9USeKXcZ4iA==
```

> [!note]
> For More Security, we can replace `random` with `IV`: `this.userService.aesCbcEncrypt(message,IV_Value)`


 
