### شرح للـ checkExistingSession

الـ function دي بتسخدم [()sessionExists](https://github.com/Ak-ram/To-Know/blob/main/BM/functions/sessionExists.md) عشان تشيك إذا كان في active session شغاله ولا لا.

- لو في : بتعمل navigate لصفحة التنبهه `session-alert/.`  وفي الحاله دي المستخدم لازم يسجل دخول مره تانيه.
- ولو مفيش : مش هتعمل حاجه.


```typescript
checkExistingSession() {
  if (this.sessionExists()) {
    this.router.navigate(['/session-alert']);
  }
}
```
