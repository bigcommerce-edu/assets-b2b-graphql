# Create the Junior Buyer User

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
mutation CreateUser(
    $companyId: Int!,
    $email: String!,
    $firstName: String!,
    $lastName: String!,
    $companyRoleId: Int
) {
    userCreate(
        userData: {
            companyId: $companyId,
            email: $email,
            firstName: $firstName,
            lastName: $lastName,
            companyRoleId: $companyRoleId
        }
    ) {
        user {
            id
            firstName
            lastName
            email
            role
            companyRoleId
            companyRoleName
        }
    }
}
```

Variables:

```json
{
    "companyId": {{b2b_logged_in_company_id}},
    "email": "{{junior_email}}",
    "firstName": "Jack",
    "lastName": "Doe",
    "companyRoleId": {{junior_role_id}}
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

const user = pm.response.json().data?.userCreate?.user;
pm.test('Response includes user with ID', () => {
    pm.expect(
        user?.id
    ).to.be.a('string');
});
```

[Next](../07_LoginJuniorBuyer/07_LogInJuniorBuyer.md)