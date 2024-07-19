# `PaymentLinkApiService`

An Angular service that provides various API methods for managing payment links. This service interacts with multiple endpoints to perform actions such as retrieving transaction fees, submitting link requests, uploading files, and fetching data related to payment links.

<details>
<pre>

```ts
import {
  BM_LEGACY_QUEUABLE_HEADER_KEY,
  BM_LEGACY_QUEUABLE_HEADER_VALUE,
} from '@bmonline/legacy/bm-legacy-services';
import {
  ICorporateRecentData,
  IGenerateLinkFormData,
  IGenerateLinkSubmissionModel,
  IPaymentLinkApiRes,
} from '../../../shared/models/payment-link-model';
import { Injectable, inject } from '@angular/core';
import { Observable, first, map } from 'rxjs';

import { ApiCIFChangesHandlerService } from './api-cif-change-handler.service';
import { CCRequestStatus } from '../../../shared/enum';
import { CashCollectionApiService } from './cash-collection-api.service';
import { IApiResponse } from '@bmcorp/corp-core';

@Injectable({ providedIn: 'root' })
export class PaymentLinkApiService {
  public apiCIFChangesHandlerService = inject(ApiCIFChangesHandlerService);

  private cashCollectionApiUrl = this.cashCollectionApi.baseUrl;
  private paymentLinkUrl = `${this.cashCollectionApiUrl}/payment-link`;
  private uploadFileURL = `${this.cashCollectionApiUrl}/payment-link/:cif/:requestId/upload`;
  private postLinkRequestApi = `${this.cashCollectionApiUrl}/payment-link/:cif/submit`;
  private paymentRequestListApi = `${this.cashCollectionApiUrl}/payment-link/:cif/requests/:requestStatus`;
  private getRequestDetailsApi = `${this.cashCollectionApiUrl}/payment-link/:cif/:requestId`;
  private downloadAttachmentApi = `${this.cashCollectionApiUrl}/payment-link/:cif/:requestId/download/:fileName`;

  constructor(private cashCollectionApi: CashCollectionApiService) {}

  getTransactionFees$(
    amount: number,
    cif: string
  ): Observable<{ fees: number }> {
    return this.cashCollectionApi
      .getData(`${this.paymentLinkUrl}/${cif}/fees?amount=${amount}`)
      .pipe(map((res: IApiResponse<any>) => res.data));
  }

  postLinkRequest(
    cif: string,
    payload: IGenerateLinkFormData,
    otp: string
  ): Observable<IGenerateLinkSubmissionModel> {
    const routeParams = { cif };
    return this.cashCollectionApi
      .postData(this.postLinkRequestApi, routeParams, null, payload, { otp })
      .pipe(map((res: IApiResponse<IGenerateLinkSubmissionModel>) => res.data));
  }

  uploadFileRequest$(cif: string, requestId: string, payload: FormData) {
    const routeParms = { cif, requestId };
    return this.cashCollectionApi.postData(
      this.uploadFileURL,
      routeParms,
      null,
      payload
    );
  }

  getCorporateRecentData$(cif: string): Observable<ICorporateRecentData> {
    const headers = {
      [BM_LEGACY_QUEUABLE_HEADER_KEY]: BM_LEGACY_QUEUABLE_HEADER_VALUE,
    };

    return this.cashCollectionApi
      .getData(`${this.paymentLinkUrl}/${cif}/recent-data`, null, null, headers)
      .pipe(map((res: IApiResponse<ICorporateRecentData>) => res.data));
  }

  getPaymentLinkRequestList$(
    requestStatus: string,
    cif: string
  ): Observable<IPaymentLinkApiRes[]> {
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

  getRequestDetails(
    requestId: string,
    cif: string
  ): Observable<IPaymentLinkApiRes> {
    const routeParms = { cif, requestId };
    return this.cashCollectionApi
      .getData(this.getRequestDetailsApi, routeParms)
      .pipe(map((res: IApiResponse<any>) => res.data));
  }

  getRequestAttachments(requestId: string, fileName: string, ciff: string) {
    return this.apiCIFChangesHandlerService
      .apiCIFChangesHandler((cif) =>
        this.cashCollectionApi.getBlob(
          `${this.cashCollectionApiUrl}/payment-link/${cif}/${requestId}/download/${fileName}`
        )
      )
      .pipe(first());
  }
}


```
  
</pre>
</details>



## It Provides these Methods

### 1. `getTransactionFees$`

Fetches transaction fees for a given amount and CIF (Customer Information File).

**Parameters:**
- `amount: number` - The transaction amount.
- `cif: string` - The CIF.

**Returns:**
- `Observable<{ fees: number }>` - An observable containing the transaction fees.

**Usage:**

```typescript
getTransactionFees$(amount: number, cif: string): Observable<{ fees: number }> {
  return this.cashCollectionApi
    .getData(`${this.paymentLinkUrl}/${cif}/fees?amount=${amount}`)
    .pipe(map((res: IApiResponse<any>) => res.data));
}
```

### 2. `postLinkRequest`

Submits a new payment link request.

**Parameters:**
- `cif: string` - The CIF.
- `payload: IGenerateLinkFormData` - The form data for generating the link.
- `otp: string` - One-time password for validation.

**Returns:**
- `Observable<IGenerateLinkSubmissionModel>` - An observable containing the result of the link submission.

**Usage:**

```typescript
postLinkRequest(cif: string, payload: IGenerateLinkFormData, otp: string): Observable<IGenerateLinkSubmissionModel> {
  const routeParams = { cif };
  return this.cashCollectionApi
    .postData(this.postLinkRequestApi, routeParams, null, payload, { otp })
    .pipe(map((res: IApiResponse<IGenerateLinkSubmissionModel>) => res.data));
}
```

### 3. `uploadFileRequest$`

Uploads a file associated with a payment link request.

**Parameters:**
- `cif: string` - The CIF.
- `requestId: string` - The request ID.
- `payload: FormData` - The file data to upload.

**Returns:**
- `Observable<any>` - An observable for the file upload response.

**Usage:**

```typescript
uploadFileRequest$(cif: string, requestId: string, payload: FormData) {
  const routeParms = { cif, requestId };
  return this.cashCollectionApi.postData(
    this.uploadFileURL,
    routeParms,
    null,
    payload
  );
}
```

### 4. `getCorporateRecentData$`

Retrieves recent corporate data for a given CIF.

**Parameters:**
- `cif: string` - The CIF.

**Returns:**
- `Observable<ICorporateRecentData>` - An observable containing recent corporate data.

**Usage:**

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

### 5. `getPaymentLinkRequestList$`

Fetches a list of payment link requests based on the request status.

**Parameters:**
- `requestStatus: string` - The status of the requests (e.g., 'pending', 'history').
- `cif: string` - The CIF.

**Returns:**
- `Observable<IPaymentLinkApiRes[]>` - An observable containing a list of payment link requests.

**Usage:**

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

### 6. `getRequestDetails`

Fetches details for a specific payment link request.

**Parameters:**
- `requestId: string` - The request ID.
- `cif: string` - The CIF.

**Returns:**
- `Observable<IPaymentLinkApiRes>` - An observable containing the request details.

**Usage:**

```typescript
getRequestDetails(requestId: string, cif: string): Observable<IPaymentLinkApiRes> {
  const routeParms = { cif, requestId };
  return this.cashCollectionApi
    .getData(this.getRequestDetailsApi, routeParms)
    .pipe(map((res: IApiResponse<any>) => res.data));
}
```

### 7. `getRequestAttachments`

Downloads attachments related to a payment link request.

**Parameters:**
- `requestId: string` - The request ID.
- `fileName: string` - The name of the file to download.
- `cif: string` - The CIF.

**Returns:**
- `Observable<Blob>` - An observable containing the file blob.

**Usage:**

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

