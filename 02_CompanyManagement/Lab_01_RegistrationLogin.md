# Lab - Postman Registration/Login Workflow

## Introduction

In this lab, you'll create a workflow in Postman to simulate the common storefront tasks of registering a company and logging in.

This will include building in automation to capture and re-use values from API responses, much in the same way that the storefront application would track a user's session.

### Prerequisites

* A BigCommerce [sandbox store](https://developer.bigcommerce.com/docs/start/about/sandboxes) or [trial store](https://www.bigcommerce.com/essentials/), or a full production store
* B2B Edition enabled in your store
* [Postman](https://www.postman.com/) or a similar API client

### Collection Variable Setup

In the collection variables, make sure the following are set to a desired value:

* `admin_email`
* `admin_password`

## Registration and Login Exercises

Try the following requests in the **Registration/Login** folder.

For each request, verify that all tests succeed.

### Create Customer

Runs the `customerCreate` mutation to register a BigCommerce customer (with password) on your channel.

* **Replace** the placeholders in the GraphQL variables: `<Email>` and `<Password>`.
* **Observe** the created customer’s `id` and `email` in the response.
* **Record** the customer `id` for **Create Company** (`customerId` is a string in the mutation variables).

**AUTOMATION:** The automated version of the request should use `{{admin_email}}` and `{{admin_password}}` from variables. The automated version of the request should save a `admin_customer_id` for the next step.

### Create Company

Runs the `companyCreate` mutation to register a B2B company for that customer.

* **Enter** the customer `id` from **Create Customer** as `customerId` (as a string in JSON).
* **Adjust** company and address fields in the variables if you like.
* **Verify** in the B2B Edition control panel that the company appears (for example, pending or approved depending on your store settings).

**AUTOMATION:** The automated version of the request should use `{{admin_customer_id}}` for `customerId`.

### Login

Runs the `login` mutation with email and password to obtain a storefront session token and user profile.

* **Use** the same email and password as in **Create Customer**.
* **Observe** `data.login.result.token` and `data.login.result.user` (`id`, `bcId`, `email`, etc.).

**AUTOMATION:** The automated version of the request ("Admin Login") should use `{{admin_email}}` and `{{admin_password}}`. The request should save `b2b_storefront_token`, `b2b_logged_in_email`, `b2b_logged_in_id`, and `bc_logged_in_id` in the environment for **User Company** and other requests that use the collection Bearer auth.

**The automated request will simplify all future requests by prepping the storefront token that will be automatically used for authentication.**

### User Company

Runs the `userCompany` query for the company associated with the logged-in B2B user.

* **Set** the GraphQL variable `userId` to the B2B user `id` from the **Login** response (integer). The `id` field under `data.login.result.user` is the B2B user id.
* **Observe** company fields such as `companyName`, `companyStatus`, and address data.

**AUTOMATION:** The automated version of the request should use `{{b2b_logged_in_id}}` for `userId`. The request should save a `b2b_logged_in_company_id` for future examples.
