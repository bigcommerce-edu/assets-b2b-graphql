# Changelog: B2B GraphQL Labs

**Date:** 2026-05-27
**Comparison:** Changes from main branch
**Collection file:** `B2B GraphQL Labs.postman_collection.json`

## Registration/Login
> Covers both the "BC Registration/Login" and "BC Registration/Login (Automated)" folders, plus the renamed "B2B Registration/Login" and "B2B Registration/Login (Automated)" folders.

### Renamed Folders
- `Registration/Login` → `B2B Registration/Login` — request contents unchanged; added folder description clarifying that this flow uses the B2B GraphQL API and is an alternative to the BC registration/login flow.
- `Registration/Login (Automated)` → `B2B Registration/Login (Automated)` — request contents unchanged; added folder description clarifying that this flow uses the B2B GraphQL API and is an alternative to the BC registration/login flow.

### Added Folders

#### `BC Registration/Login`
New folder demonstrating the BigCommerce GraphQL Storefront API as an alternative to the B2B GraphQL registration/login flow. Folder description notes that this flow requires a private token for the main GQL Storefront API and includes a request to exchange a customer access token for a B2B user token.

Added requests:
- `BC REST Create Private Token` — POST to `/v3/storefront/api-token-private` creating a private storefront token with `Unauthenticated`, `Customer`, and `B2B` scopes; stores the token in the `private_storefront_token` environment variable.
- `BC GQL Register Customer` — `registerCustomer` mutation against the BC GraphQL Storefront API; asserts a numeric `entityId` is returned and includes error handling for `EmailAlreadyInUseError`, `AccountCreationDisabledError`, `CustomerRegistrationError`, and `ValidationError`.
- `BC GQL Customer Login` — `login` mutation returning `customer` (with `entityId`, `email`, `firstName`, `lastName`) and `customerAccessToken` (with `value` and `expiresAt`); asserts successful login, customer ID, email, and token are present.
- `BC GQL Register Company` — `registerCompany` mutation; requires `X-Bc-Customer-Access-Token` header; asserts a numeric `entityId` is returned.
- `B2B REST Get B2B Token` — POST to `/api/io/auth/customers/storefront` to exchange the BC customer access token for a B2B storefront token; sends `customerAccessToken` as an object with `expires_at` and `value`.
- `B2B GQL Customer Info` — `customerInfo` query against the B2B GraphQL API; asserts a numeric `userInfo.id` is returned.
- `B2B GQL User Company` — `userCompany` query against the B2B GraphQL API; asserts a string company `id` is returned.

#### `BC Registration/Login (Automated)`
New folder providing an automated version of the BC Registration/Login flow with environment variable wiring across requests.

Added requests:
- `BC REST Create Private Token` — same as standard; stores token in `private_storefront_token`.
- `BC GQL Register Customer` — uses `{{admin_email}}` and `{{admin_password}}` variables; stores the new customer ID in the `admin_customer_id` collection variable.
- `BC GQL Admin Login` — `login` mutation using `{{admin_email}}` / `{{admin_password}}`; stores `customer_access_token`, `customer_access_token_exp`, `b2b_logged_in_email`, and `bc_logged_in_id` environment variables.
- `BC GQL Junior Buyer Login` — `login` mutation using `{{junior_email}}` / `{{junior_password}}`; same environment variable wiring as admin login.
- `BC GQL Register Company` — uses `{{customer_access_token}}` for the `X-Bc-Customer-Access-Token` header.
- `B2B REST Get B2B Token` — uses `{{bc_logged_in_id}}`, `{{customer_access_token_exp}}`, and `{{customer_access_token}}` variables; stores the resulting B2B token in `b2b_storefront_token`.
- `B2B GQL Customer Info` — stores `userInfo.id` in `b2b_logged_in_id`.
- `B2B GQL User Company` — uses `{{b2b_logged_in_id}}` as the `userId` variable; stores company ID (parsed to int) in `b2b_logged_in_company_id`.
