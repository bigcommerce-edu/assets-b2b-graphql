# Get Invoice Checkout

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

Variables:

```json
{
    "invoiceId": {{invoice_id}},
    "amount": "{{invoice_amount}}",
    "currency": "{{currency_code}}"
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

const url = pm.response.json().data?.invoiceCreateBcCart?.result?.checkoutUrl;
pm.test(`Response includes checkout URL`, () => {
    pm.expect(
        url
    ).to.be.a('string');
});
```

[Next](../06_GetPaymentDetails/01_GetPayments.md)