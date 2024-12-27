# Get BigCommerce Store Settings

Authorization - Bearer Token:
{{storefront_token}}

Request method:
POST

URL:
```
https://store-{{store_hash}}-{{storefront_channel_id}}.mybigcommerce.com/graphql
```

Headers:

| Header       | Value            |
|--------------|------------------|
| Accept       | application/json |
| Content-Type | application/json |

Body:

```
{
    site {
        settings {
            storeName
            storeHash
        }
    }
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

[Next](../../Module02/01_CompaniesAndAddresses/01_CompaniesAndAddresses.md)