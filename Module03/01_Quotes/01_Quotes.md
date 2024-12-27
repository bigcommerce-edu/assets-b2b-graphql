# Quotes

## Obtaining Product Details

**Product search example:**

```
query SearchProducts(
    $productIds: [Int],
    $companyId: String
) {
    productsSearch(
        productIds: $productIds,
        companyId: $companyId
    ) {
        id
        name
        sku
        availability
        variants
        imageUrl
        inventoryLevel
        inventoryTracking
        availability
        optionsV3
        productUrl
    }
}
```

**Example variables:**

```json
{
    "productIds": [10,20],
    "companyId": "11122233"
}
```

**Example response:**

```json
{
    "data": {
        "productsSearch": [
            {
                "id": 10,
                "name": "Fancy Lamp",
                "sku": "FCY-LMP",
                "availability": "available",
                "variants": [
                    {
                        "variant_id": 15,
                        "product_id": 10,
                        "sku": "FCY-LMP",
                        "option_values": [],
                        "calculated_price": 150,
                        "image_url": "https://cdn11.bigcommerce.com/...",
                        "has_price_list": false,
                        "bulk_prices": [],
                        "purchasing_disabled": false,
                        "cost_price": 0,
                        "inventory_level": 50,
                        "bc_calculated_price": {
                            "as_entered": 150,
                            "tax_inclusive": 156,
                            "tax_exclusive": 150,
                            "entered_inclusive": false
                        }
                    }
                ],
                "imageUrl": "https://cdn11.bigcommerce.com/...",
                "inventoryLevel": 50,
                "inventoryTracking": "product",
                "optionsV3": [],
                "productUrl": "/fancy-lamp/"
            },
            {
                "id": 20,
                "name": "Awesome T-Shirt",
                "sku": "AWE-T",
                "availability": "available",
                "variants": [
                    {
                        "variant_id": 17,
                        "product_id": 20,
                        "sku": "AWE-T-SM-R",
                        "option_values": [
                            {
                                "id": 1000,
                                "label": "Red",
                                "option_id": 100,
                                "option_display_name": "Color"
                            },
                            {
                                "id": 2000,
                                "label": "Small",
                                "option_id": 101,
                                "option_display_name": "Size"
                            }
                        ],
                        ...
                    },
                    ...
                ],
                ...
                "optionsV3": [
                    {
                        "id": 100,
                        "product_id": 20,
                        "name": "Colors",
                        "display_name": "Color",
                        "type": "swatch",
                        "sort_order": 0,
                        "option_values": [
                            {
                                "id": 1000,
                                "label": "Red",
                                "sort_order": 1,
                                "value_data": {
                                    "colors": [
                                        "#ff0000"
                                    ]
                                },
                                "is_default": true
                            },
                            ...
                        ]
                    },
                    ...
                ],
                ...
            }
        ]
    }
}
```

**Example with search term:**

```
query SearchProducts(
    $search: String,
    $companyId: String
) {
    productsSearch(
        search: $search,
        companyId: $companyId
    ) {
        ...
    }
}
```

## Submitting a Quote Request

**Create quote example:**

```
mutation CreateQuote(
    $message: String,
    $subtotal: Decimal!,
    $discount: Decimal!,
    $grandTotal: Decimal!,
    $userEmail: String,
    $quoteTitle: String,
    $shippingAddress: ShippingAddressInputType!,
    $billingAddress: BillingAddressInputType!,
    $contactInfo: ContactInfoInputType!,
    $companyId: Int,
    $currency: CurrencyInputType!,
    $storeHash: String!,
    $productList: [ProductInputType]!
) {
    quoteCreate(
        quoteData: {
            message: $message,
            subtotal: $subtotal,
            discount: $discount,
            grandTotal: $grandTotal,
            userEmail: $userEmail,
            quoteTitle: $quoteTitle,
            shippingAddress: $shippingAddress,
            billingAddress: $billingAddress,
            contactInfo: $contactInfo,
            companyId: $companyId,
            currency: $currency,
            storeHash: $storeHash,
            productList: $productList
        }
    ) {
        quote {
            id
            createdAt
            quoteNumber
            quoteTitle
            quoteUrl
        }
    }
}
```

**Example variables:**

```json
{
    "message": "I need this ASAP",
    "subtotal": 2250,
    "discount": 0,
    "grandTotal": 2250,
    "userEmail": "johndoe@example.com",
    "quoteTitle": "Fall Stocking Quote",
    "shippingAddress": {
        "country": "United States",
        "state": "Texas",
        "city": "Austin",
        "zipCode": "78726",
        "address": "123 Park Central",
        "firstName": "John",
        "lastName": "Doe",
        "addressLine1": "123 Park Central",
        "phoneNumber": "444-555-6666"
    },
    "billingAddress": {
        "country": "United States",
        "state": "Texas",
        "city": "Austin",
        "zipCode": "78726",
        "address": "123 Park Central",
        "firstName": "John",
        "lastName": "Doe",
        "addressLine1": "123 Park Central",
        "phoneNumber": "444-555-6666"
    },
    "contactInfo": {
        "name": "John Doe",
        "email": "johnswidgets@example.com",
        "companyName": "John's Widgets",
        "phoneNumber": "111-222-3333"
    },
    "companyId": 11122233,
    "currency": {},
    "storeHash": "pki...",
    "productList": [
        {
            "productId": 10,
            "sku": "FCY-LMP",
            "basePrice": 150,
            "discount": 0,
            "offeredPrice": 150,
            "quantity": 10,
            "variantId": 15,
            "imageUrl": "https://cdn11.bigcommerce.com/...",
            "productName": "Fancy Lamp",
            "options": []
        },
        {
            "productId": 20,
            "sku": "AWE-T-SM-R",
            "basePrice": 75,
            "discount": 0,
            "offeredPrice": 75,
            "quantity": 10,
            "variantId": 17,
            "imageUrl": "https://cdn11.bigcommerce.com/...",
            "productName": "Awesome T-Shirt",
            "options": []
        }
    ]
}
```

**Example response:**

```json
{
    "data": {
        "quoteCreate": {
            "quote": {
                "id": "334455",
                "createdAt": 1725560410,
                "quoteNumber": "QN000023",
                "quoteTitle": "Fall Stocking Quote",
                "quoteUrl": "https://mystore.com/#/quoteDetail/334455?date=1725560410"
            }
        }
    }
}
```

## Fetching Quote Details

**Get quotes example:**

```
query GetQuotes {
    quotes {
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
                quoteNumber
                quoteTitle
                createdBy
                createdByEmail
                expiredAt
                subtotal
                discount
                discountType
                discountValue
                shippingTotal
                taxTotal
                grandTotal
                totalAmount
                contactInfo
                trackingHistory
                notes
                legalTerms
                shippingMethod
                billingAddress
                shippingAddress
                salesRepInfo {
                    salesRepName
                    salesRepEmail
                }
                productsList {
                    productId
                    variantId
                    sku
                    basePrice
                    discount
                    offeredPrice
                    quantity
                    imageUrl
                    productName
                    options
                    notes
                    productUrl
                    type
                }
                quoteUrl
            }
        }
    }
}
```

**Example response:**

```json
{
    "data": {
        "quotes": {
            "pageInfo": {
                "hasNextPage": true,
                "hasPreviousPage": false,
                "startCursor": "YXJy...",
                "endCursor": "YXJy..."
            },
            "totalCount": 15,
            "edges": [
                {
                    "node": {
                        "id": "334455",
                        "createdAt": 1725560410,
                        "quoteNumber": "QN000023",
                        "quoteTitle": "Fall Stocking Quote",
                        "createdBy": "Jack Doe",
                        "createdByEmail": "johndoe@example.com",
                        "expiredAt": 1728173605,
                        "subtotal": "2250.0000",
                        "discount": "112.5000",
                        "discountType": 1,
                        "discountValue": "5.0000",
                        "shippingTotal": "50.0000",
                        "taxTotal": "77.5000",
                        "grandTotal": "2137.5000",
                        "totalAmount": "2265.0000",
                        "contactInfo": {
                            "name": "John Doe",
                            "email": "johndoe@example.com",
                            "companyName": "John's Widgets",
                            "phoneNumber": "111-222-3333"
                        },
                        "trackingHistory": [
                            {
                                "date": 1725560410,
                                "role": "Customer: John Doe",
                                "message": "I need this ASAP"
                            },
                            {
                                "date": 1725668005,
                                "role": "Sales rep: admin@mystore.com",
                                "message": "I'm pleased to be able to offer a 5% discount for this quote!"
                            }
                        ],
                        "notes": "Applied our one-time Early Bird discount",
                        "legalTerms": "Please note items from a discounted order cannot be returned.",
                        "shippingMethod": {
                            "id": 111,
                            "cost": 50,
                            "type": "custom",
                            "b2bId": 50,
                            "isCustom": true,
                            "description": "Ground",
                            "shippingTotal": 50
                        },
                        "billingAddress": {
                            "city": "Austin",
                            "label": "",
                            "state": "Texas",
                            "address": "123 Park Central",
                            "country": "United States",
                            "zipCode": "78726",
                            "lastName": "Doe",
                            "addressId": "",
                            "apartment": "",
                            "firstName": "John",
                            "stateCode": "TX",
                            "countryCode": "US",
                            "phoneNumber": "444-555-6666"
                        },
                        "shippingAddress": {
                            "city": "Austin",
                            "label": "",
                            "state": "Texas",
                            "address": "123 Park Central",
                            "country": "United States",
                            "zipCode": "78726",
                            "lastName": "Doe",
                            "addressId": "",
                            "apartment": "",
                            "firstName": "John",
                            "stateCode": "TX",
                            "countryCode": "US",
                            "phoneNumber": "444-555-6666"
                        },
                        "salesRepInfo": {
                            "salesRepName": "Jack Foo",
                            "salesRepEmail": "admin@mystore.com"
                        },
                        "productsList": [
                            {
                                "productId": "10",
                                "variantId": 15,
                                "sku": "FCY-LMP",
                                "basePrice": "150.0000",
                                "discount": "5.0000",
                                "offeredPrice": "142.5000",
                                "quantity": 10,
                                "imageUrl": "https://cdn11.bigcommerce.com/...",
                                "productName": "Fancy Lamp",
                                "options": [],
                                "notes": "",
                                "productUrl": "/fancy-lamp/",
                                "type": "physical"
                            },
                            ...
                        ],
                        "quoteUrl": "https://mystore.com/#/quoteDetail/334455?date=1725560410"
                    }
                },
                ...
            ]
        }
    }
}
```

**Get quote example:**

```
query GetQuote(
    $id: Int!,
    $storeHash: String!,
    $date: String!
) {
    quote(
        id: $id,
        storeHash: $storeHash,
        date: $date
    ) {
        ...
    }
}
```

**Export quote PDF example:**

```
mutation ExportQuotePdf(
    $createdAt: Int!,
    $quoteId: Int!,
    $storeHash: String!
) {
    quoteFrontendPdf(
        createdAt: $createdAt,
        lang: "en",
        quoteId: $quoteId,
        storeHash: $storeHash
    ) {
        url
    }
}
```

**Example response:**

```json
{
    "data": {
        "quoteFrontendPdf": {
            "url": "https://s3.us-west-2.amazonaws.com/bundleb2b-v2.0-quote-prod/My_Store%3AQN000023.pdf"
        }
    }
}
```

## Sending Quote Messages

**Quote message example:**

```
mutation AddQuoteMessage(
    $id: Int!,
    $storeHash: String!,
    $userEmail: String!,
    $message: String
) {
    quoteUpdate(
        id: $id,
        quoteData: {
            storeHash: $storeHash,
            userEmail: $userEmail,
            message: $message
        }
    ) {
        quote {
            createdAt
            quoteNumber
            quoteTitle
            quoteUrl
        }
    }
}
```

**Example variables:**

```json
{
    "id": 334455,
    "storeHash": "pki...",
    "userEmail": "johndoe@example.com",
    "message": "Can you give us your best quote on Express shipping?"
}
```

## Converting a Quote to an Order

**Quote checkout example:**

```
mutation GetQuoteCheckout(
    $id: Int!,
    $storeHash: String!
) {
    quoteCheckout(
        id: $id,
        storeHash: $storeHash
    ) {
        quoteCheckout {
            checkoutUrl
            cartId
            cartUrl
        }
    }
}
```

**Example response:**

```json
{
    "data": {
        "quoteCheckout": {
            "quoteCheckout": {
                "checkoutUrl": "https://mystore.com/cart.php...",
                "cartId": "521a...",
                "cartUrl": "https://mystore.com/cart.php..."
            }
        }
    }
}
```

[Next](../02_ShoppingLists/02_ShoppingLists.md)
