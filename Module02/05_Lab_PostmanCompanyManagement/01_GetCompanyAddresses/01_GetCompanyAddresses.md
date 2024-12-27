# Get Company Addresses

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
query GetAddresses(
    $companyId: Int!,
    $limit: Int,
    $after: String
) {
    addresses(
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
                firstName
                lastName
                isShipping
                isDefaultShipping
                isBilling
                isDefaultBilling
                addressLine1
                addressLine2
                city
                state
                stateCode
                country
                countryCode
                zipCode
                phoneNumber
                label
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

const result = pm.response.json().data?.addresses;
pm.test('Total count includes at least one address', () => {
    pm.expect(
        result?.totalCount
    ).to.be.greaterThan(0);
});

let firstAddress = null;
if (Array.isArray(result?.edges)) {
    firstAddress = result.edges[0]?.node;
}
pm.test('Response includes at least one address with an ID', () => {
    pm.expect(
        firstAddress?.id
    ).to.be.a('string');
});
```

[Next](../02_CreateShippingAddress/02_CreateShippingAddress.md)