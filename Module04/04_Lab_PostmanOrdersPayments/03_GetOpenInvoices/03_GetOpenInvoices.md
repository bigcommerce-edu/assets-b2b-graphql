# Get Open Invoices

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
query GetInvoices(
    $limit: Int,
    $after: String,
    $orderBy: String,
    $status: [String]
) {
    invoices(
        first: $limit,
        after: $after,
        orderBy: $orderBy,
        status: $status
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

Variables:

```json
{
    "limit": 5,
    "after": null,
    "orderBy": "-createdAt",
    "status": ["0","1"]
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

const result = pm.response.json().data?.invoices;
pm.test('Total count includes at least one invoice', () => {
    pm.expect(
        result?.totalCount
    ).to.be.greaterThan(0);
});

let firstInvoice = null;
if (Array.isArray(result?.edges)) {
    firstInvoice = result.edges[0]?.node;
}
pm.test('Response includes at least one invoice with an ID and open balance', () => {
    pm.expect(
        firstInvoice?.id
    ).to.be.a('string');
    pm.expect(
        firstInvoice?.openBalance?.value
    ).to.be.a('string');
});

pm.collectionVariables.unset('invoice_id');
pm.collectionVariables.unset('invoice_amount');

pm.collectionVariables.set('invoice_id', firstInvoice?.id);
pm.collectionVariables.set('invoice_amount', firstInvoice?.openBalance?.value);
```

[Next](../04_ExportInvoiceCsvPdf/01_ExportOpenInvoices.md)