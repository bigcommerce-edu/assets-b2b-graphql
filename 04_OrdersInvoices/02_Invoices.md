# Invoices

## Fetching Invoices

**Get invoices example:**

```
query GetInvoices {
    invoices {
        pageInfo {
            hasNextPage
            hasPreviousPage
            startCursor
            endCursor
        }
        totalCount
        edges {
            node {
                id
                invoiceNumber
                createdAt
                customerId
                dueDate
                status
                orderNumber
                purchaseOrderNumber
                details
                pendingPaymentCount
                originalBalance {
                    code
                    value
                }
                openBalance {
                    code
                    value
                }
            }
        }
    }
}
```

**Example response:**

```json
{
    "data": {
        "invoices": {
            "pageInfo": {
                "hasNextPage": true,
                "hasPreviousPage": false,
                "startCursor": "YXJy...",
                "endCursor": "YXJy..."
            },
            "totalCount": 12,
            "edges": [
                {
                    "node": {
                        "id": "5556677",
                        "invoiceNumber": "05556677",
                        "createdAt": 1724276491,
                        "customerId": "1112233",
                        "dueDate": 1726868491,
                        "status": 1,
                        "orderNumber": "120",
                        "purchaseOrderNumber": "12345",
                        "details": {
                            "type": "StandardInvoiceDetails",
                            "header": {
                                "cost_lines": [
                                    {
                                        "amount": {
                                            "code": "USD",
                                            "value": "341.0000"
                                        },
                                        "description": "Subtotal"
                                    },
                                    ...
                                ],
                                "order_date": 1724273249,
                                "billing_address": {
                                    ...
                                },
                                "customer_fields": {},
                                "shipping_addresses": [
                                    {
                                        ...
                                    }
                                ]
                            },
                            "details": {
                                "shipments": [
                                     ...
                                 ],
                                "line_items": [
                                     ...
                                 ]
                            }
                        },
                        "pendingPaymentCount": 0,
                        "originalBalance": {
                            "code": "USD",
                            "value": "325.5200"
                        },
                        "openBalance": {
                            "code": "USD",
                            "value": "325.5200"
                        }
                    }
                },
                ...
            ]
        }
    }
}
```

**Get invoice example:**

```
query GetInvoice(
    $invoiceId: Int!
) {
    invoice(
        invoiceId: $invoiceId
    ) {
        id
        invoiceNumber
        createdAt
        customerId
        dueDate
        ...
    }
}
```

## Downloading Invoices

**Export CSV example:**

```
mutation InvoicesExport {
    invoicesExport{
        url
    }
}
```

**Example response:**

```json
{
    "data": {
        "invoicesExport": {
            "url": "https://s3-us-west-2.amazonaws.com/bundleb2b-v3.0-invoice-prod/invoice-export-pki...-1726003486.csv"
        }
    }
}
```

**Filtered example:**

```
mutation InvoicesExport(
    $beginDate: Int,
    $endDate: Int
) {
    invoicesExport(
        invoiceFilterData: {
            beginDateAt: $beginDate,
            endDateAt: $endDate
        }
    ) {
        url
    }
}
```

**Example variables:**

```json
{
    "beginDate": 1704088800,
    "endDate": 1735711199
}
```

**Invoice PDF example:**

```
mutation GetInvoicePdf(
    $invoiceId: Int!
) {
    invoicePdf(
        invoiceId: $invoiceId,
        isPayNow: true
    ) {
        url
    }
}
```

**Example response:**

```json
{
    "data": {
        "invoicePdf": {
            "url": "https://s3-us-west-2.amazonaws.com/bundleb2b-v2.0-quote-prod/My_Store_05556677_ec01cb3a-....pdf"
        }
    }
}
```

## Initiating Invoice Checkout

**Create cart example:**

```
mutation GetInvoiceCheckout(
    $invoiceId: Int!,
    $amount: String!,
    $currency: String!
) {
    invoiceCreateBcCart(
        bcCartData: {
            lineItems: [
                {
                    invoiceId: $invoiceId,
                    amount: $amount
                }
            ],
            currency: $currency,
            details: {}
        }
    ) {
        result {
            checkoutUrl
            cartId
        }
    }
}
```

**Example variables:**

```json
{
    "invoiceId": 5556677,
    "amount": "325.5200",
    "currency": "USD"
}
```

**Example response:**

```json
{
    "data": {
        "invoiceCreateBcCart": {
            "result": {
                "checkoutUrl": "https://mystore.com/cart.php?...",
                "cartId": "550a97..."
            }
        }
    }
}
```

**Multiple invoice example:**

```
mutation GetInvoiceCheckout(
    $invoiceLines: [InvoiceLineItemsInputType]!,
    $currency: String!
) {
    invoiceCreateBcCart(
        bcCartData: {
            lineItems: $invoiceLines,
            currency: $currency,
            details: {}
        }
    ) {
        result {
            checkoutUrl
            cartId
        }
    }
}
```

**Example variables:**

```json
{
    "invoiceLines": [
        {
            "invoiceId": 5556677,
            "amount": "100.0000"
        },
        {
            "invoiceId": 5556678,
            "amount": "75.0000"
        }
    ],
    "currency": "USD"
}
```
