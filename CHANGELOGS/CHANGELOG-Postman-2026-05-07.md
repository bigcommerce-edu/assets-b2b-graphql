# Changelog: B2B GraphQL Labs

**Date:** 2026-05-07
**Comparison:** Changes from main branch
**Collection file:** `B2B GraphQL Labs.postman_collection.json`

---

## Global Changes

### Collection Variables

- **Added:** `quote_uuid` (initial value: empty)

---

## Quotes

> Covers both the "Quotes" and "Quotes (Automated)" folders.

### Modified Requests

#### `Create Quote`

- **GraphQL query updated** *(both)* — added `uuid` field to the quote selection set
- **Test scripts updated** *(standard only)* — removed lines that unset and set the `quote_id` and `quote_created_at` collection variables
- **Test scripts updated** *(automated only)* — added `pm.collectionVariables.unset('quote_uuid')` and `pm.collectionVariables.set('quote_uuid', quote?.uuid)` to capture the new `uuid` field from the response

#### `Get Quotes`

- **GraphQL query updated** *(both)* — added `uuid` field to the quote selection set

#### `Get Quote`

- **GraphQL query updated** *(both)* — added `$uuid: String!` variable declaration; updated `$date: String!` to `$date: String!,` (trailing comma); added `uuid: $uuid` as a query argument
- **Variables updated** *(standard only)* — added `uuid` field with placeholder value `<Quote UUID>`; updated `date` entry to include trailing comma formatting
- **Variables updated** *(automated only)* — added `uuid` field referencing `{{quote_uuid}}` collection variable; updated `date` entry to include trailing comma formatting

#### `Get Quote Checkout`

- **GraphQL query updated** *(both)* — added `$uuid: String!` variable declaration; updated `$storeHash: String!` to `$storeHash: String!,` (trailing comma); added `uuid: $uuid` as a query argument
- **Variables updated** *(standard only)* — added `uuid` field with placeholder value `<Quote UUID>`; updated `storeHash` entry to include trailing comma formatting
- **Variables updated** *(automated only)* — added `uuid` field referencing `{{quote_uuid}}` collection variable; updated `storeHash` entry to include trailing comma formatting

---

## Registration / Login

> Covers both the "Registration / Login" and "Registration / Login (Automated)" folders.

### Modified Requests

#### `Create Company`

- **GraphQL query updated** *(both)* — added `$customerEmail: String!,` variable declaration; added `customerEmail: $customerEmail,` as an argument to the mutation input
- **Variables updated** *(standard only)* — added `customerEmail` field with placeholder value `<Customer Email>`
- **Variables updated** *(automated only)* — added `customerEmail` field referencing `{{admin_email}}` collection variable
