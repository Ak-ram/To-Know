# PaymentLinkApiService

This service provide methods to deal with paymentlink

---

### 1. Getting Transaction Fees

The `getTransactionFees$` method is used to retrieve transaction fees based on a given payment amount. It constructs an API endpoint using the `cif` (customer identifier) and the payment `amount`. Then, it uses the `getData` method from the `cashCollectionApi` service to make an HTTP request. Once the data is returned, the method maps the response to extract the `fees` field and returns an observable with this information.

```typescript
getTransactionFees$(amount: number, cif: string): Observable<{ fees: number }> {
  return this.cashCollectionApi
    .getData(`${this.paymentLinkUrl}/${cif}/fees?amount=${amount}`)
    .pipe(map((res: IApiResponse<any>) => res.data));
}
```

---

### 2. Posting a Payment Link Request

The `postLinkRequest` method allows you to submit a payment link request. It takes the `cif`, `payload` (which contains the form data), and an `otp` (one-time password). This method builds the route parameters using the `cif`, and then calls `postData` to send the `payload` and `otp` to the API. The response is mapped to extract the submitted link request data, which is then returned as an observable.

```typescript
postLinkRequest(cif: string, payload: IGenerateLinkFormData, otp: string): Observable<IGenerateLinkSubmissionModel> {
  const routeParams = { cif };
  return this.cashCollectionApi
    .postData(this.postLinkRequestApi, routeParams, null, payload, { otp })
    .pipe(map((res: IApiResponse<IGenerateLinkSubmissionModel>) => res.data));
}
```

---

### 3. Uploading a File for a Payment Link Request

The `uploadFileRequest$` method handles uploading a file for a specific payment link request. It constructs the route parameters using both the `cif` and `requestId`. The file is passed in the `payload`, which is a `FormData` object. The method calls `postData` to send the file data to the API.

```typescript
uploadFileRequest$(cif: string, requestId: string, payload: FormData) {
  const routeParms = { cif, requestId };
  return this.cashCollectionApi.postData(this.uploadFileURL, routeParms, null, payload);
}
```

---

### 4. Getting Recent Corporate Data

The `getCorporateRecentData$` method fetches recent corporate data for a given customer. It includes custom headers, which are used for legacy queuable handling. The method calls `getData` to request the corporate data for the specified `cif`, maps the response to extract the required data, and returns it as an observable.

```typescript
getCorporateRecentData$(cif: string): Observable<ICorporateRecentData> {
  const headers = {
    [BM_LEGACY_QUEUABLE_HEADER_KEY]: BM_LEGACY_QUEUABLE_HEADER_VALUE,
  };

  return this.cashCollectionApi
    .getData(`${this.paymentLinkUrl}/${cif}/recent-data`, null, null, headers)
    .pipe(map((res: IApiResponse<ICorporateRecentData>) => res.data));
}
```

---

### 5. Getting a List of Payment Link Requests

The `getPaymentLinkRequestList$` method is used to retrieve a list of payment link requests. The `requestStatus` can be either pending or history, which is handled through a switch case that modifies the route parameters accordingly. It uses `getData` to make the request and returns an observable with a list of payment link requests.

```typescript
getPaymentLinkRequestList$(requestStatus: string, cif: string): Observable<IPaymentLinkApiRes[]> {
  let routeParms = { cif, requestStatus: null };

  switch (requestStatus) {
    case CCRequestStatus.PAYMENT_LINK_REQUESTS:
      routeParms = { ...routeParms, requestStatus: 'pending' };
      break;
    case CCRequestStatus.PAYMENT_LINK_HISTORY:
      routeParms = { ...routeParms, requestStatus: 'history' };
      break;
  }
  return this.cashCollectionApi
    .getData(this.paymentRequestListApi, routeParms)
    .pipe(map((res: IApiResponse<IPaymentLinkApiRes[]>) => res.data));
}
```

---

### 6. Getting Request Details

The `getRequestDetails` method is designed to fetch detailed information about a specific payment link request. It uses the `requestId` and `cif` to construct the route parameters, then calls `getData` to fetch the detailed data from the backend. The method maps the response and returns an observable containing the details.

```typescript
getRequestDetails(requestId: string, cif: string): Observable<IPaymentLinkApiRes> {
  const routeParms = { cif, requestId };
  return this.cashCollectionApi
    .getData(this.getRequestDetailsApi, routeParms)
    .pipe(map((res: IApiResponse<any>) => res.data));
}
```

---

### 7. Downloading Attachments of a Payment Link Request

The `getRequestAttachments` method downloads an attachment associated with a specific payment link request. It constructs the download URL using the `cif`, `requestId`, and `fileName`. The `getBlob` method is used to fetch the file, and the observable completes once the download is finished.

```typescript
getRequestAttachments(requestId: string, fileName: string, cif: string) {
  return this.apiCIFChangesHandlerService
    .apiCIFChangesHandler((cif) =>
      this.cashCollectionApi.getBlob(
        `${this.cashCollectionApiUrl}/payment-link/${cif}/${requestId}/download/${fileName}`
      )
    )
    .pipe(first());
}
```

---

In summary, the `PaymentLinkApiService` manages payment link-related operations such as posting requests, uploading files, fetching recent data, retrieving request lists, getting request details, and downloading attachments. Each method is responsible for interacting with the backend via the `cashCollectionApi` service, processing API responses, and returning observables that handle the results of these operations asynchronously.
