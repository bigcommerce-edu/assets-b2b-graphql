# Get Orders

Authorization: Inherit auth from parent

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
query GetOrders(
    $limit: Int,
    $after: String,
    $orderBy: String
) {
    allOrders(
        first: $limit,
        after: $after,
        orderBy: $orderBy
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
                bcOrderId
                createdAt
                userId
                bcCustomerId
                companyId {
                    id
                    companyName
                }
                totalIncTax
                currencyCode
                items
                poNumber
                status
                shippingAddress
                shipments
                products
                customer
                companyName
                firstName
                lastName
            }
        }
    }
}
```

Variables:

```json
{
    "limit": 5,
    "after": null,
    "orderBy": "-createdAt"
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

const result = pm.response.json().data?.allOrders;
pm.test('Total count includes at least one order', () => {
    pm.expect(
        result?.totalCount
    ).to.be.greaterThan(0);
});

let firstOrder = null;
if (Array.isArray(result?.edges)) {
    firstOrder = result.edges[0]?.node;
}
pm.test('Response includes at least one order with an ID', () => {
    pm.expect(
        firstOrder?.id
    ).to.be.a('string');
});
```

[Next](../02_GetOrderedProducts/02_GetOrderedProducts.md)