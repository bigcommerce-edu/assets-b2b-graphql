# Register a BigCommerce Customer

Authorization - No Auth

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
mutation CreateCustomer(
    $storeHash: String!,
    $email: String!,
    $password: String,
    $firstName: String!,
    $lastName: String!,
    $phone: String,
    $channelId: Int
) {
    customerCreate(
        customerData: {
            storeHash: $storeHash,
            email: $email,
            firstName: $firstName,
            lastName: $lastName,
            phone: $phone,
            originChannelId: $channelId,
            channelIds: [$channelId],
            authentication: {
                forcePasswordReset: false,
                newPassword: $password
            }
        }
    ) {
        customer {
            id
            email
        }
    }
}
```

Variables:

```json
{
    "storeHash": "{{store_hash}}",
    "email": "{{admin_email}}",
    "password": "{{admin_password}}",
    "firstName": "John",
    "lastName": "Doe",
    "phone": "111-222-3333",
    "channelId": {{storefront_channel_id}}
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


const customer = pm.response.json().data?.customerCreate?.customer;
pm.test('Response includes a customer ID', () => {
    pm.expect(
        customer?.id
    ).to.be.a('number');
});

pm.collectionVariables.unset('admin_customer_id');
pm.collectionVariables.set('admin_customer_id', customer?.id);
```

[Next](./03_CreateCompany.md)