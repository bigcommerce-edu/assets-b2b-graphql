# Register a BigCommerce Customer

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
mutation RegisterCustomer(
    $firstName: String!,
    $lastName: String!,
    $email: String!,
    $password: String!,
    $phone: String
) {
    customer {
        registerCustomer(
            input: {
                firstName: $firstName,
                lastName: $lastName,
                email: $email,
                password: $password,
                phone: $phone
            }
        ) {
            customer {
                entityId
                email
            }
            errors {
                ... on EmailAlreadyInUseError {
                    message
                }
                ... on AccountCreationDisabledError {
                    message
                }
                ... on EmailAlreadyInUseError {
                    message
                }
                ... on EmailAlreadyInUseError {
                    message
                }
            }
        }
    }
}
```

Variables:

```json
{
    "firstName": "John",
    "lastName": "Doe",
    "email": "{{admin_email}}",
    "password": "{{admin_password}}",
    "phone": "111-222-3333"
}
```

Post-response Script:
```javascript
pm.test('Response is not an error', () => {
    pm.response.to.not.be.error;
    pm.response.to.not.have.jsonBody("errors");
    pm.expect(pm.response.json().data?.customer?.registerCustomer?.errors).to.be.empty;
});
pm.test('Response is JSON with data', () => {
    pm.response.to.have.jsonBody("data");
});


const customer = pm.response.json().data?.customer?.registerCustomer?.customer;
pm.test('Response includes a customer ID', () => {
    pm.expect(
        customer?.entityId
    ).to.be.a('number');
});

pm.collectionVariables.unset('admin_customer_id');
pm.collectionVariables.set('admin_customer_id', customer?.entityId);
```

[Next](./03_CreateCompany.md)