### شرح للـ checkExistingSession

الـ function دي بتشيك إذا كانت الـ session الحالية لسه شغالة. لو انتهت، بتوجه المستخدم لصفحة تنبهه إنه لازم يسجل دخول تاني.


```typescript
checkExistingSession() {
  if (this.sessionExists()) {
    this.router.navigate(['/session-alert']);
  }
}
```
