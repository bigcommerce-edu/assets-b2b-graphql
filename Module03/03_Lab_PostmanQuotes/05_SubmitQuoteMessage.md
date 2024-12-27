# Add Message to Quote

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
mutation AddQuoteMessage(
    $id: Int!,
    $storeHash: String!,
    $userEmail: String!,
    $message: String
) {
    quoteUpdate(
        id: $id,
        quoteData: {
            storeHash: $storeHash,
            userEmail: $userEmail,
            message: $message
        }
    ) {
        quote {
            createdAt
            quoteNumber
            quoteTitle
            quoteUrl
        }
    }
}
```

Variables:

```json
{
    "id": {{quote_id}},
    "storeHash": "{{store_hash}}",
    "userEmail": "{{b2b_logged_in_email}}",
    "message": "Can you give us your best quote on Express shipping?"
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

const quote = pm.response.json().data?.quoteUpdate?.quote;
pm.test('Response includes quote with quote number', () => {
    pm.expect(
        quote?.quoteNumber
    ).to.be.a('string');
});
```

[Next](./06_CheckOutFromQuote.md)