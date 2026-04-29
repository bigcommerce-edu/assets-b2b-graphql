# Shopping Lists

## Create a Shopping List

**Create list example:**

```
mutation CreateShoppingList(
    $name: String!,
    $description: String!,
    $status: Int!
) {
    shoppingListsCreate(
        shoppingListData: {
            name: $name,
            description: $description,
            status: $status
        }
    ) {
        shoppingList {
            id
            name
            description
            status
        }
    }
}
```

## Adding Items to a List

**Create items example:**

```
mutation AddShoppingListItems(
    $items: [ShoppingListsItemsInputType]!,
    $shoppingListId: Int!
) {
    shoppingListsItemsCreate(
        items: $items
        shoppingListId: $shoppingListId
    ) {
        shoppingListsItems {
            id
            productId
            variantId
            quantity
        }
    }
}
```

**Example variables:**

```json
{
    "shoppingListId": {{shopping_list_id}},
    "items": [
        {
            "productId": 10,
            "variantId": 15,
            "quantity": 10,
            "productNote": "Highest priority"
        },
        ...
    ]
}
```

**Update items example:**

```
mutation UpdateShoppingListItem(
    $itemId: Int!,
    $shoppingListId: Int!,
    $itemData: ShoppingListsItemsUpdateInputType!
) {
    shoppingListsItemsUpdate(
        itemId: $itemId,
        shoppingListId: $shoppingListId,
        itemData: $itemData
    ) {
        shoppingListsItem {
            id
            productId
            variantId
            quantity
        }
    }
}
```

## Fetching List Details

**Get shopping lists example:**

```
query GetShoppingLists {
    shoppingLists {
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
                createdAt
                name
                description
                status
                customerInfo {
                    firstName
                    lastName
                    userId
                    email
                    role
                }
                isOwner
                products {
                    edges {
                        node {
                            id
                            productId
                            variantId
                            quantity
                            productSku
                            productNote
                        }
                    }
                }
            }
        }
    }
}
```

**Example response:**

```json
{
    "data": {
        "shoppingLists": {
            "pageInfo": {
                "hasNextPage": true,
                "hasPreviousPage": false,
                "startCursor": "YXJy...",
                "endCursor": "YXJy..."
            },
            "totalCount": 7,
            "edges": [
                {
                    "node": {
                        "id": "987654",
                        "createdAt": 1767160800,
                        "name": "Fall Re-stock",
                        "description": "These items are needed for the fall re-stock.",
                        "status": 30,
                        "customerInfo": {
                            "firstName": "Jack",
                            "lastName": "Doe",
                            "userId": 1234568,
                            "email": "jackdoe@example.com",
                            "role": "2"
                        },
                        "isOwner": false,
                        "products": {
                            "edges": [
                                {
                                    "node": {
                                        "id": "567890123",
                                        "productId": 10,
                                        "variantId": 15,
                                        "quantity": 10,
                                        "productSku": "FCY-LMP",
                                        "productNote": "Highest priority"
                                    }
                                },
                                ...
                            ]
                        }
                    }
                },
                ...
            ]
        }
    }
}
```

**Get shopping list request:**

```
query GetShoppingList(
    $shoppingListId: Int!
) {
    shoppingList(
        id: $shoppingListId
    ) {
        id
        createdAt
        name
        description
        status
        customerInfo {
            firstName
            lastName
            userId
            email
            role
        }
        isOwner
        grandTotal
        totalDiscount
        totalTax
        products {
            edges {
                node {
                    id
                    productId
                    variantId
                    quantity
                    productName
                    baseSku
                    variantSku
                    basePrice
                    discount
                    tax
                    productUrl
                    primaryImage
                    productNote
                }
            }
        }
    }
}
```

## Changing a List's Status

**Update status example:**

```
mutation SubmitShoppingList(
    $shoppingListId: Int,
    $name: String!,
    $description: String!,
    $status: Int!
) {
    shoppingListsUpdate(
        id: $shoppingListId,
        shoppingListData: {
            name: $name,
            description: $description,
            status: $status
        }
    ) {
        shoppingList {
            id
            name
            status
        }
    }
}
```

**Submit for approval example:**

```
mutation SubmitShoppingList(
    $shoppingListId: Int,
    ...
) {
    shoppingListsUpdate(
        id: $shoppingListId,
        shoppingListData: {
            ...
            status: 40
        }
    ) {
        ...
    }
}
```

**Approve list example:**

```
mutation SubmitShoppingList(
    $shoppingListId: Int,
    ...
) {
    shoppingListsUpdate(
        id: $shoppingListId,
        shoppingListData: {
            ...
            status: 0
        }
    ) {
        ...
    }
}
```
