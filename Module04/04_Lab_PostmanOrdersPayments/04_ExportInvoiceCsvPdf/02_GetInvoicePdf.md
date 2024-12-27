# Get Invoice PDF

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

Variables:

```json
{
    "invoiceId": {{invoice_id}}
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

const url = pm.response.json().data?.invoicePdf?.url;
pm.test(`Response includes PDF URL`, () => {
    pm.expect(
        url
    ).to.be.a('string');
});
```

[Next](../05_InitiateInvoiceCheckout/05_InitiateInvoiceCheckout.md)