# Lab - Postman Company Management Workflow

## Introduction

In the previous lab, you created a workflow to simulate a customer login. Now you'll use that authentication process as you build out common storefront tasks for managing addresses and customers.

### Prerequisites

* A BigCommerce [sandbox store](https://docs.bigcommerce.com/developer/docs/start/about/sandboxes) or [trial store](https://www.bigcommerce.com/essentials/), or a full production store
* B2B Edition enabled in your store
* [Postman](https://www.postman.com/) or a similar API client
* An existing Postman environment, collection, and variables as configured in previous labs

### Collection Variable Setup

In the collection variables, make sure the following are set to desired values for the junior buyer flow:

* `junior_email`
* `junior_password`

## Address Exercises

Try the following requests in the **Addresses** folder.

For each request, verify that all tests succeed. Requests rely on the logged-in company context: **Enter** your company ID wherever the GraphQL variables use `<Company ID>`, or rely on the automated requests for this context. 

### Get Addresses

Runs the `addresses` query with pagination (`first` / `after`) for a company.

* **Enter** `<Company ID>` for `companyId`.
* **Observe** `pageInfo`, `totalCount`, and address fields on each edge `node`.

**AUTOMATION:** The automated version of the request should use `{{b2b_logged_in_company_id}}` for `companyId`.

### Create Address

Runs the `addressCreate` mutation to add a company address (shipping and billing flags, label, and so on).

* **Enter** `<Company ID>` and adjust the sample address fields as needed.
* **Verify** in the B2B Edition control panel that the address appears for the company.
* **Record** the new address `id` from `data.addressCreate.address` for **Update Address** and **Delete Address** (`addressId` is an integer in the mutation variables).

**AUTOMATION:** The automated version of the request ("Create Shipping Address") should use `{{b2b_logged_in_company_id}}` for `companyId`. The automated version of the request should save an `address_id` for the next steps.

### Update Address

Runs the `addressUpdate` mutation for an existing address.

* **Enter** `<Company ID>`, `<Address ID>`, and any fields you want to change (for example default shipping or billing flags).
* **Verify** the address updates in the control panel and in the response.

**AUTOMATION:** The automated version of the request should use `{{b2b_logged_in_company_id}}` and `{{address_id}}` for `companyId` and `addressId`.

### Delete Address

Runs the `addressDelete` mutation. Tests expect `data.addressDelete.message` to equal `Success`.

* **Enter** `<Company ID>` and `<Address ID>` for the address to remove.
* **Verify** the address is gone in the control panel.

**AUTOMATION:** The automated version of the request should use `{{b2b_logged_in_company_id}}` and `{{address_id}}`. The automated version of the request should clear `address_id` after a successful delete (so a stale ID is not reused).

## User Exercises

Try the following requests in the **Users** folder.

For each request, verify that all tests succeed.

### Get Users

Runs the `users` query with pagination for a company.

* **Enter** `<Company ID>` for `companyId`.
* **Observe** user fields such as `email`, `role`, `companyRoleId`, and `companyRoleName`.

**AUTOMATION:** The automated version of the request should use `{{b2b_logged_in_company_id}}` for `companyId`.

### Get User Roles

Runs the `companyRoles` query for a company’s B2B roles.

* **Enter** `<Company ID>` for `companyId`.
* **Observe** each role’s `id` and `name`.
* **Record** the `id` for the **Junior Buyer** role (as an integer in JSON) for **Create User**.

**AUTOMATION:** The automated version of the request should use `{{b2b_logged_in_company_id}}` for `companyId`. The request should save a `junior_role_id` from the role named **Junior Buyer**, in preparation for the next requests.

### Check User Email

Runs the `userEmailCheck` query. Tests expect `userType` to be a number included in `[1, 2]` (valid for inviting a new user with that email).

* **Replace** `<Email Address>` with the email you plan to use for **Create User**.

**AUTOMATION:** The automated version of the request should use `{{junior_email}}` for the email.

### Create User

Runs the `userCreate` mutation to add a company user (for example a junior buyer).

* **Enter** `<Company ID>`, `<Email Address>`, name fields, and `<Role ID>` from **Get User Roles**.
* **Verify** in the B2B Edition control panel that the user appears with the expected role.
* **Record** the new user `id` if you will use **Update User** or **Delete User**.

**AUTOMATION:** The automated version of the request ("Create Junior User") should use `{{b2b_logged_in_company_id}}`, `{{junior_email}}`, and `{{junior_role_id}}`.

#### After Creating the User

The user creation request does not set the BigCommerce customer's password. Before proceeding, use the control panel to set the customer's password, making sure to use the **same value** as your collection variable `junior_password`.

## Junior Buyer Login

After your Junior Buyer user exists, try toggling between the Admin and Junior Buyer logins.

* In **Registration/Login (Automated)**, run the "Junior Buyer Login" request.
* **Observe** the new environment variable values, which should include a new storefront token and new "logged_in" values.
* Try the **Create Shipping Address** request again to observe that your current logged-in user does not have permission.
* Switch back and forth between logins with the **Admin Login** and **Junior Buyer Login** requests.
