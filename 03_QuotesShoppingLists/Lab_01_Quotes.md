# Lab - Postman Quote Workflow

## Introduction

In this lab, you'll explore the common storefront tasks involved with managing and submitting quotes. This will also give you an opportunity to observe the impact that different user roles have on GraphQL results.

### Prerequisites

* A BigCommerce [sandbox store](https://docs.bigcommerce.com/developer/docs/overview/sandboxes) or [trial store](https://www.bigcommerce.com/essentials/), or a full production store
* B2B Edition enabled in your store
* [Postman](https://www.postman.com/) or a similar API client
* An existing Postman environment, collection, and headers preset as configured in previous labs
* Minimal quote/payment settings as described below

### Required Quote/Payment Settings

The lab will involve logging into your storefront as a company user to place an order from a quote. The following minimal settings are required in the B2B Edition section of your control panel:

* In Settings > General, "Quotes" must be checked under "Feature Management."
* In Settings > Quotes, "Allow Quote Request for" should have "Company user" checked, and "Enable add to quote button" should be checked.
* In Settings > Checkout, the "Purchase Order Payment Method" should be enabled.
* The Payments settings for the company you will use for the storefront must not have been customized to exclude "PO" as an available payment option.

### User Login

Remember that you need an authorized token and "current company" values for all the requests in this exercise. 

* Run the **Admin Login** request or the **Junior Buyer Login** request in the **"Registration/Login"** folder to generate your current "logged in" token and record the logged-in user's information. (Try alternating between Junior Buyer and Admin to observe the effects on permissions.)
* Run the **User Company** request immediately after login to record information about the logged-in company.

## Quotes Exercises

Try the following requests in the **Quotes** folder.

For each request, verify that all tests succeed.

### Search Products

Runs the `productsSearch` query to load product and variant data (used to build line items for **Create Quote**).

* **Replace** `<Product 1 ID>` and `<Product 2 ID>` in the `productIds` array with real catalog product IDs from your store (the tests expect at least two products in the result).
* **Enter** `companyId` as a string matching your B2B company identifier.
* **Record** each product’s `id`, `sku`, `name`, `price`, and a `variantId`.
* **Sum** the total of all products' prices (multiplied by thte quantities you'll be adding to a new quote).

**AUTOMATION:** The automated version of the request should use `{{b2b_logged_in_company_id}}` for `companyId`. The request should save `product1_id`, `product1_sku`, `product1_price`, `product1_variant_id`, `product1_image_url`, `product1_name`, and the same fields for `product2_*`, plus `products_price_total` (line totals for quantity 10 per product) for **Create Quote**.

### Create Quote

Runs the `quoteCreate` mutation with addresses, contact info, currency, store hash, and a `productList`.

* **Align** `subtotal` and `grandTotal` with the sum of your line items (quantity × offered price).
* **Replace** placeholder emails, `<Company ID>`, `<Total Price>`, currency fields (`<Token>`, `<Code>`, and so on), and the single `productList` entry with values from **Search Products** (you can duplicate the object in JSON if you want two lines).
* **Verify** the new quote in the B2B Edition control panel or storefront quote experience, as appropriate for your store.

**AUTOMATION:** The automated version of the request should use `{{products_price_total}}` for `subtotal` and `grandTotal`, `{{b2b_logged_in_email}}` for buyer email fields, `{{b2b_logged_in_company_id}}` for `companyId`, the `product1_*` and `product2_*` variables for two line items, and the `currency_*` collection variables for the `currency` input. The request should save `quote_id`, `quote_uuid`, and `quote_created_at` for **Export Quote PDF**, **Add Message to Quote**, **Get Quote Checkout**, and **Get Quote**.

### Get Quotes

Runs the `quotes` connection query with `first`, `after`, and `orderBy` (for example newest first with `"-createdAt"`).

* **Observe** `pageInfo`, `totalCount`, and quote fields on each edge (totals, addresses, `productsList`, `quoteUrl`, and so on).
* **Note** that tests expect your user to have access to at least one quote; create one first if the count is zero.

### Export Quote PDF

Runs the `quoteFrontendPdf` mutation to obtain a URL for the quote PDF.

* **Enter** `quoteId` and `createdAt` from **Create Quote** (`createdAt` is the quote’s creation timestamp; it must pair correctly with the quote id).
* **Record** the URL in the response and use this to browse to the generated PDF.

**AUTOMATION:** The automated version of the request should use `{{quote_id}}` and `{{quote_created_at}}` from the previous **Create Quote** request.

### Add Message to Quote

Runs `quoteUpdate` with a `message` (buyer note) on an existing quote.

* **Enter** the quote numeric `id`, `storeHash`, and the `userEmail` for the acting user.
* **Observe** the returned quote fields such as `quoteNumber` and `quoteTitle`.

**AUTOMATION:** The automated version of the request should use `{{quote_id}}` from the previous **Create Quote** request and `{{b2b_logged_in_email}}` for `userEmail`.

### Get Quote Checkout

Runs the `quoteCheckout` mutation to retrieve checkout-related URLs and cart identifiers for a quote.

* **Enter** the quote `id` and `uuid` (both are now required).
* **Observe** `checkoutUrl`, `cartId`, and `cartUrl` in `data.quoteCheckout.quoteCheckout`.
* **Record** the `checkoutUrl` in the response and use this in a browser to initiate a checkout for the quote.

**AUTOMATION:** The automated version of the request should use `{{quote_id}}` and `{{quote_uuid}}` from the previous **Create Quote** request.

### Get Quote

Runs the `quote` query for a single quote by `id`, `uuid`, `storeHash`, and `date` (the quote’s creation context; use the same timestamp you use for the PDF export). The `uuid` is now required alongside `id` and `date`.

* **Enter** `id`, `uuid`, and `date` from **Create Quote**.
* **Observe** full quote detail including `status`, `productsList`, addresses, and `checkoutUrl`.

**AUTOMATION:** The automated version of the request should use `{{quote_id}}`, `{{quote_uuid}}`, and `{{quote_created_at}}` from the previous **Create Quote** request.
