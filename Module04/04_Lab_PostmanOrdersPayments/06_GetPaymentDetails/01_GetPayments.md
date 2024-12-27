# Get Payments

Authorization: Inherit auth from parent

Request method:
POST

URL:
```
https://api-b2b.bigcommerce.com/graphql
```

Headers:

| Header       | Value            |
|--------------|------------------|
| Accept       | application/json |
| Content-Type | application/json |

Body:

```
query GetPayments(
    $limit: Int,
    $after: String
) {
    invoicePayments(
        first: $limit,
        after: $after
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

Variables:

```json
{
    "limit": 5,
    "after": null
}
```

Post-response Script:

```javascript
pm.test('Response is not an error', () => {
    pm.response.to.not.be.error;
    pm.response.to.not.have.jsonBody("errors");
});
pm.test('Response is JSON with data', () => {
    pm.response.to.have.jsonBody("data");
});

const result = pm.response.json().data?.invoicePayments;
pm.test('Total count includes at least one payment', () => {
    pm.expect(
        result?.totalCount
    ).to.be.greaterThan(0);
});

let firstPayment = null;
if (Array.isArray(result?.edges)) {
    firstPayment = result.edges[0]?.node;
}
pm.test('Response includes at least one payment with an ID', () => {
    pm.expect(
        firstPayment?.id
    ).to.be.a('string');
});
```

[Next](./02_GetInvoiceReceiptLines.md)