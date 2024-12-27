# Create a GraphQL Storefront Token

Request method:
POST

URL:
```
https://api.bigcommerce.com/stores/{{store_hash}}/v3/storefront/api-token
```

Headers:

| Header       | Value            |
|--------------|------------------|
| Accept       | application/json |
| Content-Type | application/json |
| authToken    | {{b2b_v3_token}} |

Body:

```json
{
    "channel_id": {{storefront_channel_id}},
    "expires_at": 2147483647
}
```

Post-response Script:
```javascript
pm.test("Response is not an error", () => {
    pm.response.to.not.be.error;
    pm.response.to.not.have.jsonBody("errors");
});
pm.test("Response is JSON with data", () => {
    pm.response.to.have.jsonBody("data");
});
pm.test("Response includes token", () => {
    pm.expect(pm.response.json().data?.token).to.be.a('string');
});

const token = pm.response.json().data?.token;
if (token) {
    pm.environment.unset("storefront_token");
    pm.environment.set("storefront_token", token);
}
```

[Next](./04_BCStoreSettings.md)
