# Log In as Junior Buyer

Authorization: No Auth

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
mutation LogIn(
    $storeHash: String!,
    $email: String!,
    $password: String!
) {
    login(
        loginData: {
            storeHash: $storeHash,
            email: $email,
            password: $password
        }
    ) {
        result {
            token
            user {
                id
                bcId
                firstName
                lastName
                email
            }
        }
    }
}
```

Variables:

```json
{
    "storeHash": "{{store_hash}}",
    "email": "{{junior_email}}",
    "password": "{{junior_password}}"
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


const result = pm.response.json().data?.login?.result;
pm.test('Response includes a token', () => {
    pm.expect(
        result?.token
    ).to.be.a('string');
});
pm.test('Response includes a user ID', () => {
    pm.expect(
        result?.user?.id
    ).to.be.a('string');
});
pm.test('Response includes a BC customer ID', () => {
    pm.expect(
        result?.user?.bcId
    ).to.be.a('number');
});
pm.test('Response includes a user email', () => {
    pm.expect(
        result?.user?.email
    ).to.be.a('string');
});

pm.environment.unset('b2b_storefront_token');
pm.environment.unset('b2b_logged_in_email');
pm.environment.unset('b2b_logged_in_id');
pm.environment.unset('bc_logged_in_id');

pm.environment.set('b2b_storefront_token', result?.token);
pm.environment.set('b2b_logged_in_email', result?.user?.email);
pm.environment.set('b2b_logged_in_id', parseInt(result?.user?.id));
pm.environment.set('bc_logged_in_id', result?.user?.bcId);
```

[Next](../../../Module03/01_Quotes/01_Quotes.md)