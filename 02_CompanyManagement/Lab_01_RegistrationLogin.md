# Lab - Postman Registration/Login Workflow

## Introduction

In this lab, you'll create a workflow in Postman to simulate the common storefront tasks of registering a company and logging in.

This will include building in automation to capture and re-use values from API responses, much in the same way that the storefront application would track a user's session.

### Prerequisites

* A BigCommerce [sandbox store](https://docs.bigcommerce.com/developer/docs/overview/sandboxes) or [trial store](https://www.bigcommerce.com/essentials/), or a full production store
* B2B Edition enabled in your store
* [Postman](https://www.postman.com/) or a similar API client

### Collection Variable Setup

In the collection variables, make sure the following are set to a desired value:

* `admin_email`
* `admin_password`

## Registration and Login Exercises

This lab covers two alternative registration/login flows: the **B2B Registration/Login** flow, which uses the B2B GraphQL API directly, and the **BC Registration/Login** flow, which uses the BigCommerce GraphQL Storefront API and exchanges the resulting session for a B2B token.

Try the requests below in each corresponding folder. For each request, verify that all tests succeed.

### B2B Registration/Login

#### Create Customer

Runs the `customerCreate` mutation to register a BigCommerce customer (with password) on your channel.

* **Replace** the placeholders in the GraphQL variables: `<Email>` and `<Password>`.
* **Observe** the created customer’s `id` and `email` in the response.
* **Record** the customer `id` for **Create Company** (`customerId` is a string in the mutation variables).

**AUTOMATION:** The automated version of the request should use `{{admin_email}}` and `{{admin_password}}` from variables. The automated version of the request should save a `admin_customer_id` for the next step.

#### Create Company

Runs the `companyCreate` mutation to register a B2B company for that customer.

* **Enter** the customer `id` from **Create Customer** as `customerId` (as a string in JSON).
* **Enter** the customer email from **Create Customer** as `customerEmail` (required alongside `customerId`).
* **Adjust** company and address fields in the variables if you like.
* **Verify** in the B2B Edition control panel that the company appears (for example, pending or approved depending on your store settings).

**AUTOMATION:** The automated version of the request should use `{{admin_customer_id}}` for `customerId` and `{{admin_email}}` for `customerEmail`.

#### Login

Runs the `login` mutation with email and password to obtain a storefront session token and user profile.

* **Use** the same email and password as in **Create Customer**.
* **Observe** `data.login.result.token` and `data.login.result.user` (`id`, `bcId`, `email`, etc.).

**AUTOMATION:** The automated version of the request ("Admin Login") should use `{{admin_email}}` and `{{admin_password}}`. The request should save `b2b_storefront_token`, `b2b_logged_in_email`, `b2b_logged_in_id`, and `bc_logged_in_id` in the environment for **User Company** and other requests that use the collection Bearer auth.

**The automated request will simplify all future requests by prepping the storefront token that will be automatically used for authentication.**

#### User Company

Runs the `userCompany` query for the company associated with the logged-in B2B user.

* **Set** the GraphQL variable `userId` to the B2B user `id` from the **Login** response (integer). The `id` field under `data.login.result.user` is the B2B user id.
* **Observe** company fields such as `companyName`, `companyStatus`, and address data.

**AUTOMATION:** The automated version of the request should use `{{b2b_logged_in_id}}` for `userId`. The request should save a `b2b_logged_in_company_id` for future examples.

### BC Registration/Login

This flow uses the main BigCommerce GraphQL Storefront API instead of the B2B GraphQL API, then exchanges the resulting customer session for a B2B token. It's an alternative to the **B2B Registration/Login** flow above.

#### BC REST Create Private Token

Creates a private Storefront API token (REST Admin API) with `Unauthenticated`, `Customer`, and `B2B` scopes.

* **Observe** the returned `data.token` value.

**AUTOMATION:** The automated version of the request stores the token in the `private_storefront_token` environment variable, used as the Bearer token for subsequent BC GQL requests in this folder.

#### BC GQL Register Customer

Runs the `registerCustomer` mutation (BigCommerce GraphQL Storefront API) to register a customer.

* **Replace** the placeholders in the GraphQL variables: `<Email>` and `<Password>`.
* **Observe** the created customer's `entityId` in the response.

**AUTOMATION:** The automated version of the request should use `{{admin_email}}` and `{{admin_password}}` from variables, and save the returned `entityId` as `admin_customer_id`.

#### BC GQL Customer Login

Runs the `login` mutation (BigCommerce GraphQL Storefront API) with email and password.

* **Use** the same email and password as in **BC GQL Register Customer**.
* **Observe** `data.login.customer` (`entityId`, `email`, etc.) and `data.login.customerAccessToken` (`value`, `expiresAt`).

**AUTOMATION:** The automated version of the request ("BC GQL Admin Login") should use `{{admin_email}}` and `{{admin_password}}`, and save `customer_access_token`, `customer_access_token_exp`, `b2b_logged_in_email`, and `bc_logged_in_id` in the environment. A parallel "BC GQL Junior Buyer Login" request performs the same steps with `{{junior_email}}` and `{{junior_password}}`.

#### BC GQL Register Company

Runs the `registerCompany` mutation (BigCommerce GraphQL Storefront API) to register a company for the logged-in customer.

* **Set** the `X-Bc-Customer-Access-Token` header to the `customerAccessToken.value` from **BC GQL Customer Login**.
* **Adjust** company and address fields in the variables if you like.
* **Observe** the created company's `entityId` in the response.

**AUTOMATION:** The automated version of the request should use `{{customer_access_token}}` for the `X-Bc-Customer-Access-Token` header.

#### B2B REST Get B2B Token

Exchanges the BigCommerce customer access token for a B2B storefront token, using the [Get a B2B Storefront Token](https://docs.bigcommerce.com/developer/api-reference/rest/b2b/management/authentication/post-auth-customers-storefront) REST endpoint.

* **Enter** the BigCommerce customer ID, channel ID, and the `customerAccessToken` (`value` and `expires_at`) from **BC GQL Customer Login**.
* **Observe** the returned `data.token` array; the first element is the B2B storefront token.

**AUTOMATION:** The automated version of the request should use `{{bc_logged_in_id}}`, `{{customer_access_token}}`, and `{{customer_access_token_exp}}`, and save the resulting token as `b2b_storefront_token`.

**The automated request will simplify all future requests by prepping the B2B storefront token that will be automatically used for authentication, the same way the B2B Registration/Login flow's Login request does.**

#### B2B GQL Customer Info

Runs the `customerInfo` query (B2B GraphQL API) to retrieve the B2B user ID for the logged-in customer, since the BigCommerce Storefront API login flow doesn't return this ID directly.

* **Observe** `data.customerInfo.userInfo.id`.

**AUTOMATION:** The automated version of the request should save `data.customerInfo.userInfo.id` as `b2b_logged_in_id`.

#### B2B GQL User Company

Runs the `userCompany` query (B2B GraphQL API) for the company associated with the logged-in user, same as in the **B2B Registration/Login** flow.

* **Set** the GraphQL variable `userId` to the B2B user `id` from **B2B GQL Customer Info**.
* **Observe** company fields such as `companyName`, `companyStatus`, and address data.

**AUTOMATION:** The automated version of the request should use `{{b2b_logged_in_id}}` for `userId`, and save the company `id` (parsed to an integer) as `b2b_logged_in_company_id`.
