# Export Open Invoices

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
mutation InvoicesExport(
    $status: [Int]
) {
    invoicesExport(
        invoiceFilterData: {
            status: $status
        }
    ) {
        url
    }
}
```

Variables:

```json
{
    "status": [0,1]
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

const url = pm.response.json().data?.invoicesExport?.url;
pm.test(`Response includes order with ID`, () => {
    pm.expect(
        url
    ).to.be.a('string');
});
```

[Next](./02_GetInvoicePdf.md)