
## Corp: Proxy Test Configuration:

```json
{

  "/api/*": {
    "target": "https://corporate-cashcollection-uat.apps.openshiftnonprod.staff.banquemisr.bank/",
    "secure": false,
    "changeOrigin": true
  },
  "host": "0.0.0.0",
  "port": "40000"
}
```

## Commit Msg Format:

```plain
[Refactor: CRPCC-4291] Remove terms and conditions checkbox

Terms and conditions checkbox was removed from the payment link.
Changes Made:
- Eliminate the terms and conditions checkbox and its related form.
- Ensure all functionality previously dependent on the checkbox remains intact.

Issue: Payment Link Enhancements | Remove Terms & Conditions 
```
## Commands: 
### npm: 
 1. Uninstall Node:
```css
sudo rm -rf /usr/local/{bin/{node,npm},lib/node_modules/npm,lib/node,share/man/*/node.*}
```
