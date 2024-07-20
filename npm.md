## 1. Uninstall Node:
```css
sudo rm -rf /usr/local/{bin/{node,npm},lib/node_modules/npm,lib/node,share/man/*/node.*}
```


## 2. Clone errors


After cloning the repo and trying to run `npm ci` you get this error: 

```
Error: `npm ci` Can Only Install Packages When Your `package.json` and `package-lock.json` Are in Sync
```


This occurs when `npm ci` detects that `package.json` and `package-lock.json` are not in sync. This discrepancy typically happens when there are version mismatches or when `package-lock.json` has become outdated compared to `package.json`.

#### **Solution**

To resolve this issue, you need to ensure that the versions specified in `package.json` are consistent with those in `package-lock.json`. Follow these steps:

1. **Identify the Mismatch**

   Check the versions of the packages listed in `package.json` and compare them with the versions in `package-lock.json`. A common cause is mismatched versions of internal or external packages.

2. **Update Package Versions**

   Adjust the versions in `package.json` to match the versions that are causing issues. For example, if you’re facing issues with `@bmonline` packages, update the versions as follows:

   - **Remove the existing versions** from `package.json`:

     ```json
     "@bmonline/assets": "~0.1.298",
     "@bmonline/components": "~0.1.298",
     "@bmonline/core": "~0.1.298",
     "@bmonline/legacy": "~0.1.298",
     "@bmonline/theme": "~0.1.298",
     "@bmonline/features": "~0.1.298",
     ```

   - **Add the consistent versions** to `package.json`:

     ```json
     "@bmonline/assets": "0.1.290",
     "@bmonline/components": "0.1.290",
     "@bmonline/core": "0.1.290",
     "@bmonline/legacy": "0.1.290",
     "@bmonline/theme": "0.1.290",
     "@bmonline/features": "0.1.290",
     ```

3. **Reinstall Packages**

   After updating `package.json`, run the following command to regenerate `package-lock.json` and install the packages:

   ```bash
   sudo npm ci
   ```

