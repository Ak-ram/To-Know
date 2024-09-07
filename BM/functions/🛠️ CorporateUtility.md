The `CorporateUtility` class is a collection of static utility functions designed to handle common tasks in a web application. These tasks include:

- **Formatting and Parsing**: It offers methods to format numbers, dates, and strings, such as adding commas to large numbers or converting dates between different formats.
- **File Operations**: Functions to download files like PDFs, Excel sheets, or Blobs, and methods to check file sizes.
- **DOM Manipulation**: Methods to comment out script tags in HTML, open URLs in new tabs, and handle scroll behavior.
- **Data Handling**: Utilities for deep copying objects, generating query strings, and resetting form models.
- **Localization**: It supports translating dates into different languages (e.g., Arabic and English).
- **Observables**: A countdown timer utility using RxJS.

### 1. **formatNumber**

It formats a number into a string with commas separating the thousands and allows specifying decimal places.

```typescript
console.log(CorporateUtility.formatNumber(1000)); // Output: "1,000"
console.log(CorporateUtility.formatNumber(123456.789, 2)); // Output: "123,456.79"
```

---

### 2. **parseDate**

It parses a string into a `Date` object based on the provided date format.

```typescript
console.log(CorporateUtility.parseDate('01-01-2023', 'dd-MM-yyyy')); // Output: Sun Jan 01 2023
```

---

### 3. **formatDate**

It formats a `Date` object into a string based on the given format.

```typescript
console.log(CorporateUtility.formatDate(new Date(), 'yyyy-MM-dd')); // Output: "2024-09-07"
```

---

### 4. **isSameDate**

It checks if two dates are the same (ignores the time).

```typescript
const date1 = new Date('2023-09-07');
const date2 = new Date('2023-09-07');
console.log(CorporateUtility.isSameDate(date1, date2)); // Output: true
```

---

### 5. **isBeforeDate**

It checks if the first date is before the second date.

```typescript
console.log(CorporateUtility.isBeforeDate(new Date('2022-01-01'), new Date('2023-01-01'))); // Output: true
```

---

### 6. **isAfterDate**

It checks if the first date is after the second date.

```typescript
console.log(CorporateUtility.isAfterDate(new Date('2024-01-01'), new Date('2023-01-01'))); // Output: true
```

---

### 7. **formatDateString**

It converts a string from one date format to another.

```typescript
console.log(CorporateUtility.formatDateString('01-01-2023', 'dd-MM-yyyy', 'yyyy/MM/dd')); // Output: "2023/01/01"
```

---

### 8. **dateTranslate**

It formats a `Date` object and translates it based on the specified language (Arabic or English).

```typescript
console.log(CorporateUtility.dateTranslate(new Date(), 'yyyy-MM-dd', Language.EN)); // Output: "2024-09-07"
console.log(CorporateUtility.dateTranslate(new Date(), 'yyyy-MM-dd', Language.AR)); // Output: "٢٠٢٤-٠٩-٠٧"
```

---

### 9. **getMappedValue**

It finds an object in an array by a key-value pair and returns the value from another key.

```typescript
const arr = [{ id: 1, name: 'John' }, { id: 2, name: 'Jane' }];
console.log(CorporateUtility.getMappedValue(arr, 1, 'id', 'name')); // Output: "John"
```

---

### 10. **isArabic**

It checks if a given string contains Arabic characters.

```typescript
console.log(CorporateUtility.isArabic('مرحبا')); // Output: true
console.log(CorporateUtility.isArabic('Hello')); // Output: false
```

---

### 11. **countDown**

It creates an observable that counts down from a specified number of seconds.

```typescript
CorporateUtility.countDown(5).subscribe(x => console.log(x));
// Output: 5, 4, 3, 2, 1, 0 (one value per second)
```

---

### 12. **deepCopy**

It creates a deep copy of an object or array.

```typescript
const original = { name: 'John', address: { city: 'New York' } };
const copy = CorporateUtility.deepCopy(original);
console.log(copy); // Output: { name: "John", address: { city: "New York" } }
```

---

### 13. **generateQuery**

It generates a query string from an object.

```typescript
const params = { name: 'John', age: 30 };
console.log(CorporateUtility.generateQuery(params)); // Output: "name=John&age=30"
```

---

### 14. **modelReset**

It resets all fields of a model to their default values.

```typescript
const model = { name: 'John', age: 30, active: true };
CorporateUtility.modelReset(model);
console.log(model); // Output: { name: '', age: 0, active: false }
```

---

### 15. **downloadBlobFile**

It downloads a blob response as a file.

```typescript
// Assuming `blob` is a valid Blob object from an API response
CorporateUtility.downloadBlobFile(blob, 'file.txt').then(success => console.log(success)); // Output: true
```

---

### 16. **localCurrency**

It formats a value as local currency with an option to show the currency symbol.

```typescript
console.log(CorporateUtility.localCurrency(1234.56)); // Output: "1,234.56"
console.log(CorporateUtility.localCurrency(1234.56, true)); // Output: "$ 1,234.56" (assuming $ is the currency symbol)
```

---

### 17. **checkFileSizeByMb**

It checks if a file size (in bytes) exceeds a given maximum size in MB.

```typescript
console.log(CorporateUtility.checkFileSizeByMb(5 * 1024 * 1024, 10)); // Output: true (5MB < 10MB)
```

---

### 18. **downloadXls**

It exports data to an Excel file and triggers a download.

```typescript
const data = [{ id: 1, name: 'John' }, { id: 2, name: 'Jane' }];
CorporateUtility.downloadXls('test', data); // Downloads an Excel file named "test.xlsx"
```

---

### 19. **checkMinFileSizeByBytes**

It checks if a file size exceeds a minimum size in bytes.

```typescript
console.log(CorporateUtility.checkMinFileSizeByBytes(2000, 1000)); // Output: true
```

---

### 20. **checkMaxFileSizeByBytes**

It checks if a file size is within a given maximum size in bytes.

```typescript
console.log(CorporateUtility.checkMaxFileSizeByBytes(5000, 10000)); // Output: true
```

---

### 21. **commentScriptTag**

It comments out `<script>` tags in an HTML snippet.

```typescript
const htmlSnippet = '<div>Hello</div><script>alert("XSS")</script>';
console.log(CorporateUtility.commentScriptTag(htmlSnippet)); // Output: '<div>Hello</div> <!-- -->alert("XSS") -->'
```

---

### 22. **downloadPdf**

It downloads a specific container's content as a PDF.

```typescript
CorporateUtility.downloadPdf({ containerId: 'receiptContainer' }).then(() => console.log('Downloaded'));
```

---

### 23. **openInNewTab**

It opens a given URL in a new browser tab.

```typescript
CorporateUtility.openInNewTab('https://example.com');
```

---

### 24. **scrollToTop**

It scrolls the window to the top.

```typescript
CorporateUtility.scrollToTop();
```

---

### 25. **downloadXlsReport**

It downloads a transaction report as an Excel file.

```typescript
const report
