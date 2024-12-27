# Get Users

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
query GetUsers(
    $companyId: Int!,
    $limit: Int,
    $after: String
) {
    users(
        companyId: $companyId,
        first: $limit,
        after: $after
    ) {
        pageInfo {
            hasNextPage
            hasPreviousPage
            startCursor
            endCursor
        }
        totalCount
        edges {
            node {
                id
                bcId
                firstName
                lastName
                email
                phone
                role
                companyRoleId
                companyRoleName
            }
        }
    }
}
```

Variables:

```json
{
    "companyId": {{b2b_logged_in_company_id}},
    "limit": 5,
    "after": null
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

const result = pm.response.json().data?.users;
pm.test('Total count includes at least one user', () => {
    pm.expect(
        result?.totalCount
    ).to.be.greaterThan(0);
});

let firstUser = null;
if (Array.isArray(result?.edges)) {
    firstUser = result.edges[0]?.node;
}
pm.test('Response includes at least one user with an ID', () => {
    pm.expect(
        firstUser?.id
    ).to.be.a('string');
});
```

[Next](../05_ValidateJuniorBuyer/01_GetUserRoles.md)