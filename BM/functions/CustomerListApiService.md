### **CustomerListApiService**

The `CustomerListApiService` is a service responsible for interacting with the backend APIs to manage customer and supplier data.


#### **1. URL Configuration**

```typescript
  private cashCollectionApiUrl = this.cashCollectionApi.baseUrl;
  private profileUrl = `${this.cashCollectionApiUrl}/profile/:cif`;
  private inviteBuyerUrl = `${this.cashCollectionApiUrl}/buyer/:cif/invite-buyer`;
  private supplierListApi = `${this.cashCollectionApiUrl}/supplier/:cif`;
  private supplierProfileUrl = `${this.cashCollectionApiUrl}/supplier/:cif/:id/profile`;
  private supplierAddUrl = `${this.cashCollectionApiUrl}/supplier/:cif/add`;
  private buyerProfileUrl = `${this.cashCollectionApiUrl}/buyer/:cif/:id/profile`;
```

- **`cashCollectionApiUrl`**: consider the base URL for the API.
- **`profileUrl`, `inviteBuyerUrl`, `supplierListApi`, etc.**: Endpoints constructed using the base URL. `:cif` and `:id` are placeholders for dynamic parameters.

#### **2. Fetch Customer Profile Details**

```typescript
getProfileDetail$() {
  return this.apiCIFChangesHandlerService
    .apiCIFChangesHandler<IApiResponse<ICustomerProfile>>(
      (cif) => this.cashCollectionApi.getData(this.profileUrl, { cif }),
      'data'
    )
    .pipe(first());
}
```
this method is responsible to retrieve details of a customer's profile based on specific cif.

- **`this.cashCollectionApi.getData(this.profileUrl, { cif })`**: Makes a GET request to fetch profile details.
- **`'data'`**: Extracts the `data` property from the API response.
- **`pipe(first())`**: Completes the observable after emitting the first value.

#### **3. Invite a New Buyer**

```typescript
inviteNewBuyer(
  payload: INewBuyerFormData,
  header: { [key: string]: string }
): Observable<INewBuyerApiData> {
  return this.apiCIFChangesHandlerService
    .apiCIFChangesHandler<IApiResponse<INewBuyerApiData>>(
      (cif) =>
        this.cashCollectionApi.postData(
          this.inviteBuyerUrl,
          { cif },
          null,
          payload,
          header
        ),
      'data'
    )
    .pipe(first());
}
```
This mehod send an invitation to a new buyer.

- **`payload`**: Data sent to the API for inviting a new buyer.
- **`header`**: Additional headers for the API request.
- **`this.cashCollectionApi.postData`**: Makes a POST request to invite a new buyer.

#### **4. Get Supplier List**

```typescript
getSupplierList$(): Observable<ISupplierDetails[]> {
  return this.apiCIFChangesHandlerService.apiCIFChangesHandler<
    IApiResponse<ISupplierDetails[]>
  >(
    (cif) => this.cashCollectionApi.getData(this.supplierListApi, { cif }),
    'data'
  );
}
```
This method retrieves a list of suppliers.

- **`this.cashCollectionApi.getData`**: Fetches the list of suppliers from the API.

#### **5. Get Supplier Profile**

```typescript
getSupplierProfile(id: string): Observable<ICustomerProfile> {
  return this.apiCIFChangesHandlerService
    .apiCIFChangesHandler<IApiResponse<ICustomerProfile>>(
      (cif) =>
        this.cashCollectionApi.getData(this.supplierProfileUrl, { cif, id }),
      'data'
    )
    .pipe(first());
}
```
This method fetch specific supplier's profile.

- **`id`**: The unique identifier of the supplier.
- **`this.cashCollectionApi.getData`**: Makes a GET request to fetch the supplier's profile.

#### **6. Get Profile Logo**

```typescript
getProfileLogo(id: string, type: CCCustomerType) {
  return this.apiCIFChangesHandlerService
    .apiCIFChangesHandler(
      (cif) =>
        this.cashCollectionApi.getBlob(
          `${this.cashCollectionApiUrl}/${type}/${cif}/${id}/logo`
        ),
      undefined,
      true,
      true
    )
    .pipe(first());
}
```

This method retrieve the logo for a specific customer or supplier profile.

- **`type`**: Type of customer (e.g., supplier or buyer).
- **`this.cashCollectionApi.getBlob`**: Fetches a binary blob (image) for the logo.

#### **7. Add Supplier**

```typescript
addSupplier(payload: IAddSupplierPayload, header: { [key: string]: string }) {
  return this.apiCIFChangesHandlerService
    .apiCIFChangesHandler<IApiResponse<IAddSupplierResponse>>(
      (cif) =>
        this.cashCollectionApi.postData(
          this.supplierAddUrl,
          { cif },
          null,
          payload,
          header
        ),
      'data'
    )
    .pipe(first());
}
```
This method add a new supplier.

- **`payload`**: Data for adding the new supplier.
- **`header`**: Additional headers for the API request.
- **`this.cashCollectionApi.postData`**: Makes a POST request to add a new supplier.
