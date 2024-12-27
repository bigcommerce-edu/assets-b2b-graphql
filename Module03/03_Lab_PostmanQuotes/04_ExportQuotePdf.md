# Export Quote PDF

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
mutation ExportQuotePdf(
    $createdAt: Int!,
    $lang: String!,
    $quoteId: Int!,
    $storeHash: String!
) {
    quoteFrontendPdf(
        createdAt: $createdAt,
        lang: $lang,
        quoteId: $quoteId,
        storeHash: $storeHash
    ) {
        url
    }
}
```

Variables:

```json
{
    "createdAt": {{quote_created_at}},
    "lang": "en",
    "quoteId": {{quote_id}},
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

const quoteUrl = pm.response.json().data?.quoteFrontendPdf?.url;
pm.test('Response includes URL', () => {
    pm.expect(
        quoteUrl
    ).to.be.a('string');
});
```

[Next](./05_SubmitQuoteMessage.md)