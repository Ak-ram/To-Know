## Proxy Test Configuration:

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
