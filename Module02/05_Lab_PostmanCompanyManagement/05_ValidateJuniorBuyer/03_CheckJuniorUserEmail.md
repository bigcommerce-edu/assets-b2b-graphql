# Check Junior User Email

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
query CheckUserEmail(
    $email: String!
) {
    userEmailCheck(
        email: $email
    ) {
        userType
    }
}
```

Variables:

```json
{
    "email": "{{junior_email}}"
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

const userType = pm.response.json().data?.userEmailCheck?.userType;
pm.test('User type exists', () => {
    pm.expect(
        userType
    ).to.be.a('number');
});
pm.test('Email is valid for new user', () => {
    pm.expect(
        [1, 2]
    ).to.include(userType);
});
```

[Next](../06_CreateJuniorBuyerUser/06_CreateJuniorBuyerUser.md)