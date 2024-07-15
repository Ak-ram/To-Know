# Mac Hell

## VS Code: [Insufficient permissions](https://stackoverflow.com/questions/51674627/insufficient-permissions-in-vscode)

**Problem:** 

Each time I try to edit a file in VS Code that I don't own (such as a file in a cloned repository), I encounter an "Insufficient Permission" issue when saving the file.

**Solution:**

```terminal
sudo chown -R username directory_name
```

**Description:**
  - sudo – admin rights must be used since we are dealing with a folder that belongs to another user 
  - chown – the command for changing ownership 
  - -R – the recursive switch to make sure all child objects get the same ownership changes 
  - username – the new owner of the folder 
  - directory_name – the directory to be modified

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
