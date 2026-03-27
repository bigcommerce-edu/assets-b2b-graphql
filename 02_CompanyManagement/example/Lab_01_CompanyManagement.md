# Lab - Postman Company Management Workflow

### Prerequisites

* A BigCommerce [sandbox store](https://developer.bigcommerce.com/docs/start/about/sandboxes) or [trial store](https://www.bigcommerce.com/essentials/), or a full production store
* B2B Edition enabled in your store
* [Postman](https://www.postman.com/) or a similar API client
* An existing Postman environment and collection as configured in previous labs

## Company Exercises

Try the following requests in the "Companies" folder.

For each request, verify that all tests succeed.

### Create Company

* **Verify** the new company exists in the B2B Edition control panel, in the "Approved" status.
* **Record** the `companyId` value from the API response for use in the next requests.

**AUTOMATION:** The automated version of the request should save `company_id` in the collection variables.

### Get Companies

* **Observe** the company response data.
* **Try out** the `companyStatus` query param to filter on status "0" (Unapproved).

**AUTOMATION:** The automated version of the request should already be filtered on "Unapproved" status and save a `pending_company_id` collection variable for the approval step.

### Get Company

* **Enter** the previously captured company ID for the `:company_id` path variable.
* **Observe** the company response data.

**AUTOMATION:** The automated version of the request should have `:company_id` automatically populated.

### Update Company

* **Enter** a company ID for the `:company_id` path variable.
* **Edit** the Body details as you please.
* **Verify** company data is updated accordingly.

**AUTOMATION:** The automated version of the request should have `:company_id` automatically populated.

### Delete Company

* **Enter** a company ID for the `:company_id` path variable.
* **Verify** the company has been deleted.

**AUTOMATION:** The automated version of the request should have `:company_id` automatically populated. Make sure to use the automated version only if you want to delete the company you created earlier.

### Approve Company

If you don't already have unapproved companies in your store, **browse** to your B2B-enabled storefront, make sure you are logged out, and **register** a new company account.

* **Enter** a company ID for the `:company_id` path variable.
* **Verify** in the B2B Edition control panel that the company is now "Approved."

**AUTOMATION:** The automated version of the request should have `:company_id` automatically populated by the `pending_company_id` from "Get Companies."

## Address Exercises

Try the following requests in the "Addresses" folder.

For each request, verify that all tests succeed.

### Create Address

* **Enter** a valid company ID in the Body tab.
* **Verify** in the B2B Edition control panel that the address exists for the company.
* **Record** the `addressId` in the API response for the next requests.

**AUTOMATION:** The automated version of the request should have `companyId` in the body automatically populated based on your previous "Create Company" and should save `address_id` in the collection variables.

### Get Addresses / Get Company Addresses

* **Try out** filtering on the various query params, such as `companyId` for a single company's addresses.
* **Verify** returned results are as expected.

**AUTOMATION:** The automated version of the request should be filtered based on the previous "Create Company" request.

### Get Address

* **Enter** the previously captured address ID for the `:address_id` path variable.
* **Observe** the address response data.

**AUTOMATION:** The automated version of the request should have `:address_id` automatically populated.

## User Exercises

Try the following requests in the "Users" folder.

For each request, verify that all tests succeed.

### Get Users / Get Company Users

* **Try out** filtering on the various query params, such as `companyId` for a single company's users.
* **Verify** returned results are as expected.

**AUTOMATION:** The automated version of the request should be filtered based on the previous "Create Company" request.

### Get Roles

Use "Get Roles" to find the ID of the "Junior Buyer" role in preparation for creating a user with this role.

* **Observe** the various roles in the response.
* **Record** the `id` of the role with the name "Junior Buyer."

**AUTOMATION:** The automated version of the request should save `junior_role_id` in the collection variables.

### Create Junior Buyer User

* **Enter** a `companyId` in the Body.
* **Enter** the previously captured "Junior Buyer" `id` as the value of `companyRoleId` in the Body.
* **Verify** that the new user is created with the "Junior Buyer" role.
* **Record** the `userId` from the API response.

**AUTOMATION:** The automated version of the request should have `companyId` and `companyRoleId` automatically populated and should save `user_id` in the collection variables.

### Get User

* **Enter** the previously captured user ID for the `:user_id` path variable.
* **Observe** the user response data.

**AUTOMATION:** The automated version of the request should have `:user_id` automatically populated.
