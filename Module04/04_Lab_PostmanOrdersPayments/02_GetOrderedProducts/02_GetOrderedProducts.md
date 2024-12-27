# Get Ordered Products

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
query GetOrderedProducts(
    $limit: Int,
    $after: String,
    $orderBy: String
) {
    orderedProducts(
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
                productId
                productName
                sku
                variantSku
                variantId
                optionList
                optionSelections
                orderedTimes
                firstOrderedAt
                lastOrderedAt
                imageUrl
                productUrl
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
    "orderBy": "-lastOrderedAt"
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

const result = pm.response.json().data?.orderedProducts;
pm.test('Total count includes at least one product', () => {
    pm.expect(
        result?.totalCount
    ).to.be.greaterThan(0);
});

let firstProduct = null;
if (Array.isArray(result?.edges)) {
    firstProduct = result.edges[0]?.node;
}
pm.test('Response includes at least one product with an ID', () => {
    pm.expect(
        firstProduct?.id
    ).to.be.a('string');
});
```

[Next](../03_GetOpenInvoices/03_GetOpenInvoices.md)