# Get Invoice Receipt Lines

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
query GetReceiptLines(
    $invoiceId: String,
    $limit: Int,
    $after: String
) {
    allReceiptLines(
        invoiceId: $invoiceId,
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

Variables:

```json
{
    "invoiceId": "{{invoice_id}}",
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

const result = pm.response.json().data?.allReceiptLines;
pm.test('Total count includes at least one receipt line', () => {
    pm.expect(
        result?.totalCount
    ).to.be.greaterThan(0);
});

let firstLine = null;
if (Array.isArray(result?.edges)) {
    firstLine = result.edges[0]?.node;
}
pm.test('Response includes at least one receipt line with an ID', () => {
    pm.expect(
        firstLine?.id
    ).to.be.a('string');
});
```

[Next](../../../Module05/01_SuperAdmins.md)