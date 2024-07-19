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
