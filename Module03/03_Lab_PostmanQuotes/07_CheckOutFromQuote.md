# Get Quote Checkout

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
mutation GetQuoteCheckout(
    $id: Int!,
    $storeHash: String!
) {
    quoteCheckout(
        id: $id,
        storeHash: $storeHash
    ) {
        quoteCheckout {
            checkoutUrl
            cartId
            cartUrl
        }
    }
}
```

Variables:

```json
{
    "id": {{quote_id}},
    "storeHash": "{{store_hash}}"
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

const checkoutUrl = pm.response.json().data?.quoteCheckout?.quoteCheckout?.checkoutUrl;
pm.test(`Response includes checkout URL`, () => {
    pm.expect(
        checkoutUrl
    ).to.be.a('string');
});
```

[Next](../../Module04/01_Orders/01_Orders.md)