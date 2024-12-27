# Payments and Receipts

## Fetching Payments

**Get invoice payments example:**

```
query GetPayments {
    invoicePayments {
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
                createdAt
                totalCode
                totalAmount
                payerName
                processingStatus
                companyId
                companyName
                paymentReceiptSet {
                    edges {
                        node {
                            id
                            createdAt
                            customerId
                            totalCode
                            totalAmount
                            payerName
                            paymentId
                            paymentType
                            receiptLineSet {
                                edges {
                                    node {
                                        id
                                        createdAt
                                        customerId
                                        receiptId
                                        paymentId
                                        invoiceId
                                        invoiceNumber
                                        amount
                                        paymentType
                                    }
                                }
                            }
                        }
                    }
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
        "invoicePayments": {
            "pageInfo": {
                "hasNextPage": true,
                "hasPreviousPage": false,
                "startCursor": "YXJy...",
                "endCursor": "YXJy..."
            },
            "totalCount": 25,
            "edges": [
                {
                    "node": {
                        "id": "223344",
                        "createdAt": 1726078850,
                        "totalCode": "USD",
                        "totalAmount": "500.0000",
                        "payerName": "John Doe",
                        "processingStatus": 3,
                        "companyId": "1112233",
                        "companyName": "John's Widgets",
                        "paymentReceiptSet": {
                            "edges": [
                                {
                                    "node": {
                                        "id": "334455",
                                        "createdAt": 1726078850,
                                        "customerId": 1112233,
                                        "totalCode": "USD",
                                        "totalAmount": "500.0000,
                                        "payerName": "John Doe",
                                        "paymentId": 223344,
                                        "paymentType": "Offline",
                                        "receiptLineSet": {
                                            "edges": [
                                                {
                                                    "node": {
                                                        "id": "445566",
                                                        "createdAt": 1726078850,
                                                        "customerId": 1112233,
                                                        "receiptId": 334455,
                                                        "paymentId": 223344,
                                                        "invoiceId": 556679,
                                                        "invoiceNumber": "0556679",
                                                        "amount": {
                                                            "code": "USD",
                                                            "value": "500.0000"
                                                        },
                                                        "paymentType": "Offline"
                                                    }
                                                }
                                            ]
                                        }
                                    }
                                }
                            ]
                        }
                    }
                },
                {
                    "node": {
                        "id": "223343",
                        ...
                        "paymentReceiptSet": {
                            "edges": [
                                {
                                    "node": {
                                        "id": "334454",
                                        ...
                                        "totalAmount": "175.0000",
                                        ...
                                        "paymentId": 223343,
                                        "paymentType": "visa ending in 1111",
                                        "receiptLineSet": {
                                            "edges": [
                                                {
                                                    "node": {
                                                        "id": "445565",
                                                        ...
                                                        "receiptId": 334454,
                                                        "paymentId": 223343,
                                                        "invoiceId": 5556677,
                                                        "invoiceNumber": "05556677",
                                                        "amount": {
                                                            "code": "USD",
                                                            "value": "100.0000"
                                                        },
                                                        "paymentType": "visa ending in 1111"
                                                    }
                                                },
                                                {
                                                    "node": {
                                                        "id": "445564",
                                                        ...
                                                        "receiptId": 334454,
                                                        "paymentId": 223343,
                                                        "invoiceId": 5556678,
                                                        "invoiceNumber": "05556678",
                                                        "amount": {
                                                            "code": "USD",
                                                            "value": "75.0000"
                                                        },
                                                        "paymentType": "visa ending in 1111"
                                                    }
                                                }
                                            ]
                                        }
                                    }
                                }
                            ]
                        }
                    }
                },
                ...
            ]
        }
    }
}
```

**Get invoice payment request:**

```
query GetPayment(
    $paymentId: Int!
) {
    invoicePayment(
        paymentId: $paymentId
    ) {
        id
        createdAt
        totalCode
        totalAmount
        ...
        paymentReceiptSet {
            edges {
                node {
                    id
                    createdAt
                    customerId
                    totalCode
                    totalAmount
                    ...
                    receiptLineSet {
                        edges {
                            node {
                                id
                                createdAt
                                customerId
                                receiptId
                                paymentId
                                invoiceId
                                ...
                            }
                        }
                    }
                }
            }
        }
    }
}
```

## Fetching Receipt Lines

**Get receipt lines example:**

```
query GetReceiptLines(
    $invoiceId: String
) {
    allReceiptLines(
        invoiceId: $invoiceId
    ) {
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
                createdAt
                customerId
                receiptId
                paymentId
                invoiceId
                invoiceNumber
                amount
                paymentStatus
                paymentType
            }
        }
    }
}
```

**Example variables:**

```json
{
    "invoiceId": "5556677"
}
```

**Example response:**

```json
{
    "data": {
        "allReceiptLines": {
            "pageInfo": {
                "hasNextPage": false,
                "hasPreviousPage": false,
                "startCursor": "YXJy...",
                "endCursor": "YXJy..."
            },
            "totalCount": 1,
            "edges": [
                {
                    "node": {
                        "id": "445565",
                        "createdAt": 1725979982,
                        "customerId": 1112233,
                        "receiptId": 334454,
                        "paymentId": 223343,
                        "invoiceId": 5556677,
                        "invoiceNumber": "05556677",
                        "amount": {
                            "code": "USD",
                            "value": "100.0000"
                        },
                        "paymentStatus": 2,
                        "paymentType": "visa ending in 1111"
                    }
                }
            ]
        }
    }
}
```

**Get receipt line example:**

```
query GetReceiptLine(
    $id: Int!
) {
    receiptLine(
        id: $id
    ) {
        id
        createdAt
        customerId
        receiptId
        paymentId
        invoiceId
        invoiceNumber
        amount
        paymentStatus
        paymentType
    }
}
```

[Next](../04_Lab_PostmanOrdersPayments/01_GetOrders/01_GetOrders.md)
