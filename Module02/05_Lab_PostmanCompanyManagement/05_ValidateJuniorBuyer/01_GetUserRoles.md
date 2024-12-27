# Get User Roles

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
query GetUserRoles(
    $companyId: Int!
) {
    companyRoles(
        companyId: $companyId
    ) {
        edges {
            node {
                id
                name
            }
        }
    }
}
```

Variables:

```json
{
    "companyId": {{b2b_logged_in_company_id}}
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

const roles = pm.response.json().data?.companyRoles?.edges;
pm.test('Response includes roles', () => {
    pm.expect(
        roles?.length
    ).to.be.greaterThan(0);
});

pm.collectionVariables.unset('junior_role_id');
if (Array.isArray(roles)) {
    const juniorRole = roles.find(role => role?.node?.name === 'Junior Buyer');
    pm.test('Junior role found', () => {
        pm.expect(
            juniorRole?.node?.id
        ).to.be.a('string');
    });
    
    pm.collectionVariables.set('junior_role_id', parseInt(juniorRole?.node?.id));
}
```

[Next](./02_CollectionVariables.md)