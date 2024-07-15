
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

## Commands: 
### npm: 
 1. Uninstall Node:
```css
sudo rm -rf /usr/local/{bin/{node,npm},lib/node_modules/npm,lib/node,share/man/*/node.*}
```
