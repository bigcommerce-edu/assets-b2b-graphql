# Lab - Postman Order and Payment Workflow

## Introduction

In the previous lab, you built an API workflow to submit a quote from a storefront user and then initiate a checkout with that quote, ultimately completing an order. Now you'll complete a common purchasing workflow with tasks to view a company's orders, initiate a checkout for invoice payment, and inspect existing payments.

### Prerequisites

* A BigCommerce [sandbox store](https://docs.bigcommerce.com/developer/docs/start/about/sandboxes) or [trial store](https://www.bigcommerce.com/essentials/), or a full production store
* B2B Edition enabled in your store
* [Postman](https://www.postman.com/) or a similar API client
* An existing Postman environment, collection, and headers preset as configured in previous labs
* Minimal invoice settings as described below

### Required Invoice Settings

The lab will involve logging into your storefront as a company user and placing an order to pay an invoice. The following minimal settings are required in the B2B Edition section of your control panel:

* In Settings > General, "Invoices" must be checked under "Feature management."
* In Settings > Invoice, at least one valid option must be enabled under "Payment methods for paying invoices."

If you don't have a valid payment method option available in the invoice settings, make sure you have enabled a method other than Check/Purchase Order in the main Settings > Payments section of the BigCommerce control panel.

### User Login

Remember that you need an authorized token and "current company" values for all the requests in this exercise. 

* Run the **Admin Login** request or the **Junior Buyer Login** request in the **"Registration/Login"** folder to 
generate your current "logged in" token and record the logged-in user's information. (Try alternating between 
Junior Buyer and Admin to observe the effects on permissions.)
* Run the **User Company** request immediately after login to record information about the logged-in company.

### Collection Variable Setup

In the collection variables, make sure the following are set:

* `currency_code`
* `currency_exchange_rate`
* `currency_token`
* `currency_decimal_token`
* `currency_decimal_places`
* `currency_token_location`
* `currency_thousands_token`

These are already set to default values in the imported collection. Or run **Get Currencies** in the **"Currency (Automated)"** folder to retrieve the values for your store.

### Existing Order

You should have an existing order for your logged-in company. If you followed the previous Quote exercises, 
you'll likely have an order from having completed a checkout from a quote.

## Order Exercises

Try the following requests in the **Orders** folder.

For each request, verify that all tests succeed.

### Get Orders

Runs the `allOrders` connection query with `first`, `after`, and `orderBy`.

* **Observe** `pageInfo`, `totalCount`, and each order’s `id`, `bcOrderId`, status, totals, addresses, and nested product or customer data.
* **Note** that tests expect at least one order visible to the current user.

**AUTOMATION:** The automated version of the request should save `order_id` (B2B order `id` from the first record) and `bc_order_id` for **Get Order**.

### Get Ordered Products

Runs the `orderedProducts` connection for products the company has purchased, ordered by recency.

* **Observe** SKU, variant, order counts, and timestamps on each line.
* **Note** that tests expect at least one ordered product.

### Get Order

Runs the `order` query for a single order by BigCommerce order id.

* **Enter** the numeric BigCommerce order id for `id` (use `bcOrderId` from **Get Orders**).
* **Observe** totals, line items, addresses, `invoiceId`, and `orderHistoryEvent`.

**AUTOMATION:** The automated version of the request should use `{{bc_order_id}}` if you previously ran **Get Orders**.

## Invoice Exercises

Try the following requests in the **Invoices** folder.

**Before** trying these exercises, make sure you have generated at least one invoice for the company your login user belongs to.

For each request, verify that all tests succeed.

### Get Invoices

Runs the `invoices` connection with optional `status` filters (string codes such as `"0"` in the sample body).

* **Adjust** `status` to match the invoice states you want to list.
* **Observe** balances, invoice numbers, due dates, and order references.
* **Record** an `invoiceId` and open balance amount for **Get Invoice PDF**, **Get Invoice Checkout**, and **Get Invoice**.

**AUTOMATION:** The automated version of the request (**Get Open Invoices**) should use status values `["0","1"]` to filter on all open invoices and should save `invoice_id` and `invoice_amount` (from the first invoice’s `openBalance.value`) for the rest of the automated invoice requests.

### Export Invoices

Runs the `invoicesExport` mutation to obtain a download URL for a filtered export.

* **Set** the `status` array to the numeric status codes you want (for example open or unpaid states).
* **Observe** `data.invoicesExport.url`.

**AUTOMATION:** The automated version of the request (**Export Open Invoices**) should use `status` `[0, 1]` to export all open invoices.

### Get Invoice PDF

Runs the `invoicePdf` mutation with `isPayNow: true` to return a PDF URL.

* **Enter** `invoiceId` for the target invoice.

**AUTOMATION:** The automated version of the request should use `{{invoice_id}}` from the previous **Get Open Invoices** request.

### Get Invoice Checkout

Runs the `invoiceCreateBcCart` mutation to build a BigCommerce cart and return a checkout URL for paying invoice line items.

* **Enter** `invoiceId`, pay `amount` as a string, and `currency` (for example the invoice currency code).
* **Observe** `checkoutUrl` and `cartId` under `result`.
* **Record** `checkoutUrl` and open it in a browser to initiate making payment on the invoice.

**AUTOMATION:** The automated version of the request should use `{{invoice_id}}`, `{{invoice_amount}}`, and `{{currency_code}}` from your existing variables.

### Get Invoice

Runs the `invoice` query for one invoice by id.

* **Enter** `invoiceId`.

**AUTOMATION:** The automated version of the request should use `{{invoice_id}}` from the previous **Get Open Invoices** request.

## Payments and Receipts Exercises

Try the following request in the **Payments/Receipts** folder.

For each request, verify that all tests succeed.

### Get Invoice Receipt Lines

Runs the `allReceiptLines` connection, optionally filtered by `invoiceId` (string in the request variables).

* **Set** `invoiceId` to a specific invoice if you want lines for that invoice only, or adjust as needed to match your data.
* **Observe** amounts, `paymentStatus`, `paymentType`, and links to `receiptId` and `paymentId`.

**AUTOMATION:** The automated version of the request should use `{{invoice_id}}` for `invoiceId`, from the previous **Get Open Invoices** request.
