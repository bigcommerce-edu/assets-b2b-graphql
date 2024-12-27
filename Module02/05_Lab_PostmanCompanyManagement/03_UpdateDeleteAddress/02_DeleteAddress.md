# Delete Address

Authorization: 
Inherit auth from parent

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
mutation DeleteAddress(
    $companyId: Int!,
    $addressId: Int!
) {
    addressDelete(
        companyId: $companyId,
        addressId: $addressId
    ) {
        message
    }
}
```

Variables:

```json
{
    "companyId": {{b2b_logged_in_company_id}},
    "addressId": {{address_id}}
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

const message = pm.response.json().data?.addressDelete?.message
pm.test('Operation was successful', () => {
    pm.expect(
        message
    ).to.eq('Success');
});

pm.collectionVariables.unset('address_id');
```

[Next](../04_GetUsers/04_GetUsers.md)