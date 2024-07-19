# CCSideNavService

This service provide data about current user & list of side nav bar items.

It emits: 

  - `getLeftMenu()` : return json obj contains all items of sidenav bar
  - `hasCldTransactionType` : determine if the user has CLD or not
  - `isAuthorized` : determine if the user is Auth
  - `isInquiryUser` : determne if the user is inquiry
  - `requiresApprovalCountSub` : ..............

> [!note]
>
> **Current user data is important to determine it's permissions [ which tabs should be enabled or disabled as not all users have the same accessability to navbar items.]**

# JSON Object

Running `CCSideNavSerice.getLeftMenu()` will return object like this:




![jsoncrack com](https://github.com/user-attachments/assets/72b56848-de9f-4edd-a265-d95dfb483c9b)



```json
[
    {
        "id": "collections",
        "title": "Collections",
        "titleTranslationKey": "common.navmenu.collection",
        "imageName": "bm-bg-duotone_distributor_inactive",
        "activeImageName": "bm-bg-duotone_distributor_active",
        "redirectTo": "cash",
        "isActive": false,
        "isShown": true,
        "extraProps": [
            "cash"
        ],
        "subItems": [
            {
                "id": "cc-introduction",
                "title": "Introduction",
                "titleTranslationKey": "leftNav.introduction",
                "imageName": "bm-icon bm-maskImage-global_text_snippet bm-text-natural-500 bm-bg-contain",
                "activeImageName": "bm-icon bm-maskImage-global_text_snippet bm-text-primary-100 bm-bg-contain",
                "redirectTo": "dashboard",
                "isActive": false,
                "isRoot": true,
                "subItems": []
            },
            {
                "id": "create-profile",
                "title": "Profile",
                "titleTranslationKey": "leftNav.profile",
                "imageName": "bm-icon bm-maskImage-global_account_circle bm-text-natural-500 bm-bg-contain",
                "activeImageName": "bm-icon bm-maskImage-global_account_circle bm-text-primary-100 bm-bg-contain",
                "redirectTo": "create-profile",
                "isActive": false,
                "isRoot": true,
                "isDisabled": true,
                "subItems": []
            },
            {
                "id": "cc-collect",
                "title": "Collect",
                "titleTranslationKey": "leftNav.collect",
                "imageName": "bm-maskImage-global_bank_account_recipient bm-icon bm-text-natural-500 bm-bg-contain",
                "activeImageName": "bm-maskImage-global_bank_account_recipient bm-icon bm-text-primary-100 bm-bg-contain",
                "redirectTo": "",
                "isActive": false,
                "isRoot": true,
                "isDisabled": true,
                "extraProps": [
                    "request-to-pay",
                    "payment-link"
                ],
                "subItems": [
                    {
                        "id": "cc-request-to-pay",
                        "title": "Request to pay",
                        "titleTranslationKey": "leftNav.requestToPay",
                        "imageName": "bm-icon bm-maskImage-global_receipt_long bm-text-natural-500 bm-bg-contain",
                        "activeImageName": "bm-icon bm-maskImage-global_receipt_long bm-text-primary-100 bm-bg-contain",
                        "redirectTo": "request-to-pay",
                        "isActive": false,
                        "isDisabled": true,
                        "isRoot": true,
                        "subItems": [
                            {
                                "id": "cc-new-request-to-pay-link",
                                "title": "New Request To Pay",
                                "titleTranslationKey": "leftNav.newRequestToPay",
                                "imageName": "bm-icon bm-maskImage-global_new_request bm-text-natural-500 bm-bg-contain",
                                "activeImageName": "bm-icon bm-maskImage-global_new_request bm-text-primary-100 bm-bg-contain",
                                "redirectTo": "request-to-pay/add",
                                "isActive": false,
                                "isDisabled": true,
                                "isRoot": false,
                                "subItems": []
                            },
                            {
                                "id": "cc-collection-request-link",
                                "title": "Collection Requests",
                                "titleTranslationKey": "leftNav.collectionRequests",
                                "imageName": "bm-icon bm-maskImage-global_document_cash_banknote_certificate bm-text-natural-500 bm-bg-contain",
                                "activeImageName": "bm-icon bm-maskImage-global_document_cash_banknote_certificate bm-text-primary-100 bm-bg-contain",
                                "redirectTo": "request-to-pay/unpaid-requests",
                                "isActive": false,
                                "isRoot": false,
                                "isDisabled": true,
                                "subItems": [],
                                "extraProps": [
                                    "unpaid-requests",
                                    "partial-requests"
                                ]
                            },
                            {
                                "id": "cc-submitted-for-approval-link",
                                "title": "Submitted For Approval",
                                "titleTranslationKey": "leftNav.ccSubmittedForApproval",
                                "imageName": "bm-icon bm-maskImage-global_submitted_for_approval bm-text-natural-500 bm-bg-contain",
                                "activeImageName": "bm-icon bm-maskImage-global_submitted_for_approval bm-text-primary-100 bm-bg-contain",
                                "redirectTo": "request-to-pay/submitted-for-approval",
                                "isActive": false,
                                "isRoot": false,
                                "isDisabled": true,
                                "subItems": [],
                                "extraProps": [
                                    "submitted-for-approval"
                                ]
                            },
                            {
                                "id": "cc-requires-approval-link",
                                "title": "Requires Approval",
                                "titleTranslationKey": "leftNav.ccRequiresApprovals",
                                "imageName": "bm-icon bm-maskImage-global_hourglass_empty bm-text-natural-500 bm-bg-contain",
                                "activeImageName": "bm-icon bm-maskImage-global_hourglass_empty bm-text-primary-100 bm-bg-contain",
                                "redirectTo": "request-to-pay/requires-approval",
                                "isActive": false,
                                "isRoot": false,
                                "isDisabled": true,
                                "subItems": [],
                                "notificationCount": {
                                    "source": {
                                        "closed": false,
                                        "currentObservers": null,
                                        "observers": [],
                                        "isStopped": false,
                                        "hasError": false,
                                        "thrownError": null,
                                        "_value": 0
                                    }
                                },
                                "extraProps": [
                                    "requires-approval"
                                ]
                            },
                            {
                                "id": "cc-rejected-requests-link",
                                "title": "Rejected Requests",
                                "titleTranslationKey": "leftNav.rejectedRequests",
                                "imageName": "bm-icon bm-maskImage-global_rejected_requests bm-text-natural-500 bm-bg-contain",
                                "activeImageName": "bm-icon bm-maskImage-global_rejected_requests bm-text-primary-100 bm-bg-contain",
                                "redirectTo": "request-to-pay/rejected-requests",
                                "isActive": false,
                                "isRoot": false,
                                "isDisabled": true,
                                "subItems": [],
                                "extraProps": [
                                    "rejected-requests"
                                ]
                            },
                            {
                                "id": "cc-collection-history-link",
                                "title": "Collection History",
                                "titleTranslationKey": "leftNav.collectionHistory",
                                "imageName": "bm-icon bm-maskImage-global_clock bm-text-natural-500 bm-bg-contain",
                                "activeImageName": "bm-icon bm-maskImage-global_clock bm-text-primary-100 bm-bg-contain",
                                "redirectTo": "request-to-pay/history",
                                "isActive": false,
                                "isDisabled": true,
                                "isRoot": false,
                                "subItems": [],
                                "extraProps": [
                                    "history"
                                ]
                            }
                        ]
                    },
                    {
                        "id": "cc-payment-link",
                        "title": "Payment link",
                        "titleTranslationKey": "leftNav.paymentLink",
                        "imageName": "bm-icon bm-maskImage-global_link bm-text-natural-500 bm-bg-contain",
                        "activeImageName": "bm-icon bm-maskImage-global_link bm-text-primary-100 bm-bg-contain",
                        "redirectTo": "payment-link",
                        "isActive": false,
                        "isRoot": true,
                        "isDisabled": true,
                        "subItems": [
                            {
                                "id": "cc-new-payment-link",
                                "title": "New Payment link",
                                "titleTranslationKey": "leftNav.newPaymentLink",
                                "imageName": "bm-icon bm-maskImage-global_add_link bm-text-natural-500 bm-bg-contain",
                                "activeImageName": "bm-icon bm-maskImage-global_add_link bm-text-primary-100 bm-bg-contain",
                                "redirectTo": "payment-link/generate-link",
                                "isActive": false,
                                "isDisabled": true,
                                "isRoot": false,
                                "subItems": []
                            },
                            {
                                "id": "cc-pending-requests-link",
                                "title": "Pending Requests",
                                "titleTranslationKey": "leftNav.pendingRequests",
                                "imageName": "bm-icon bm-maskImage-global_waiting_for_approval bm-text-natural-500 bm-bg-contain",
                                "activeImageName": "bm-icon bm-maskImage-global_waiting_for_approval bm-text-primary-100 bm-bg-contain",
                                "redirectTo": "payment-link/payment-link-requests",
                                "isActive": false,
                                "isRoot": false,
                                "isDisabled": true,
                                "subItems": [],
                                "extraProps": [
                                    "payment-link-requests"
                                ]
                            },
                            {
                                "id": "cc-requests-history-link",
                                "title": "Payment Requests History",
                                "titleTranslationKey": "leftNav.requestsHistory",
                                "imageName": "bm-icon bm-maskImage-global_clock bm-text-natural-500 bm-bg-contain",
                                "activeImageName": "bm-icon bm-maskImage-global_clock bm-text-primary-100 bm-bg-contain",
                                "redirectTo": "payment-link/payment-link-history",
                                "isActive": false,
                                "isDisabled": true,
                                "isRoot": false,
                                "subItems": [],
                                "extraProps": [
                                    "payment-link-history"
                                ]
                            }
                        ]
                    }
                ]
            },
            {
                "id": "cc-pay",
                "title": "Pay",
                "titleTranslationKey": "leftNav.pay",
                "imageName": "bm-maskImage-global_bank_account_sender bm-icon bm-text-natural-500 bm-bg-contain",
                "activeImageName": "bm-maskImage-global_bank_account_sender bm-icon bm-text-primary-100 bm-bg-contain",
                "redirectTo": "",
                "isRoot": true,
                "isActive": false,
                "isMobileOnly": false,
                "extraProps": [
                    "distributors",
                    "request-history"
                ],
                "subItems": [
                    {
                        "id": "distributers",
                        "title": "Payment Requests",
                        "titleTranslationKey": "leftNav.payPaymentRequests",
                        "imageName": "bm-maskImage-global_document_cash_banknote bm-icon bm-text-natural-500 bm-bg-contain",
                        "activeImageName": "bm-maskImage-global_document_cash_banknote bm-icon bm-text-primary-100 bm-bg-contain",
                        "redirectTo": "distributors",
                        "isActive": false,
                        "isMobileOnly": false,
                        "isRoot": false,
                        "subItems": []
                    },
                    {
                        "id": "history",
                        "title": "Requests History",
                        "titleTranslationKey": "leftNav.payRequestsHistory",
                        "imageName": "bm-maskImage-global_clock bm-icon bm-text-natural-500 bm-bg-contain",
                        "activeImageName": "bm-maskImage-global_clock bm-icon bm-text-primary-100 bm-bg-contain",
                        "redirectTo": "request-history",
                        "isActive": false,
                        "isMobileOnly": false,
                        "isRoot": false,
                        "subItems": []
                    }
                ]
            },
            {
                "id": "customer-list",
                "title": "Customer List",
                "titleTranslationKey": "leftNav.customerList",
                "imageName": "bm-icon bm-maskImage-global_lan bm-text-natural-500 bm-bg-contain",
                "activeImageName": "bm-icon bm-maskImage-global_lan bm-text-primary-100 bm-bg-contain",
                "redirectTo": "customer-list",
                "isActive": false,
                "isRoot": true,
                "isDisabled": false,
                "subItems": []
            },
            {
                "id": "orders",
                "title": "Orders",
                "titleTranslationKey": "leftNav.orders",
                "imageName": "bm-icon bm-maskImage-global_markunread_mailbox bm-text-natural-500 bm-bg-contain",
                "activeImageName": "bm-icon bm-maskImage-global_markunread_mailbox bm-text-primary-100 bm-bg-contain",
                "redirectTo": "orders",
                "isActive": false,
                "isDisabled": false,
                "isRoot": true,
                "extraProps": [
                    "orders",
                    "my-orders",
                    "buyer-orders",
                    "products"
                ],
                "subItems": [
                    {
                        "id": "my-orders",
                        "title": "My Orders",
                        "titleTranslationKey": "leftNav.myOrders",
                        "imageName": "bm-maskImage-global_lan bm-icon bm-text-natural-500 bm-bg-contain",
                        "activeImageName": "bm-maskImage-global_lan bm-icon bm-text-primary-100 bm-bg-contain",
                        "redirectTo": "orders/my-orders",
                        "isActive": false,
                        "isRoot": false,
                        "isDisabled": false,
                        "subItems": [],
                        "extraProps": [
                            "my-orders"
                        ]
                    },
                    {
                        "id": "buyer-orders",
                        "title": "Buyer Orders",
                        "titleTranslationKey": "leftNav.buyerOrders",
                        "imageName": "bm-maskImage-global_article bm-icon bm-text-natural-500 bm-bg-contain",
                        "activeImageName": "bm-maskImage-global_article bm-icon bm-text-primary-100 bm-bg-contain",
                        "redirectTo": "orders/buyer-orders",
                        "isActive": false,
                        "isDisabled": true,
                        "isRoot": false,
                        "subItems": [],
                        "extraProps": [
                            "buyer-orders"
                        ]
                    },
                    {
                        "id": "products",
                        "title": "Products List",
                        "titleTranslationKey": "leftNav.productsList",
                        "imageName": "bm-maskImage-global_products bm-icon bm-text-natural-500 bm-bg-contain",
                        "activeImageName": "bm-maskImage-global_products bm-icon bm-text-primary-100 bm-bg-contain",
                        "redirectTo": "products/products-list",
                        "isActive": false,
                        "isRoot": false,
                        "isDisabled": true,
                        "subItems": [],
                        "extraProps": [
                            "products"
                        ]
                    }
                ]
            }
        ]
    }
]  
```
