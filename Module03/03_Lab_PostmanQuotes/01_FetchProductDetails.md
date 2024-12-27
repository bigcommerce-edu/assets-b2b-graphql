# Search Products

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
    }
}
```

Variables:

```json
{
    "productIds": [<Product 1 ID>,<Product 2 ID>],
    "companyId": "{{company_id}}"
}
```

Post-response Script:
```javascript
const numberOfProducts = 2;
const productQty = 10;

pm.test('Response is not an error', () => {
    pm.response.to.not.be.error;
    pm.response.to.not.have.jsonBody("errors");
});
pm.test('Response is JSON with data', () => {
    pm.response.to.have.jsonBody("data");
});

const products = pm.response.json().data?.productsSearch ?? [];
pm.test(`Response includes ${numberOfProducts} products`, () => {
    pm.expect(
        products.length
    ).to.be.greaterThan(numberOfProducts - 1);
});

let index = 0;
const productsData = products.map(product => {
    index++;

    const variants = product?.variants ?? [];
    const firstVariant = variants[0] ?? {};

    const productData = {
        productId: product?.id,
        sku: firstVariant?.sku,
        price: firstVariant?.calculated_price,
        variantId: firstVariant?.variant_id,
        imageUrl: firstVariant?.image_url,
        name: product?.name,
    };

    pm.test(`Product ${index} has ID`, () => {
        pm.expect(productData.productId).to.be.a('number');
    });

    pm.test(`Product ${index} has a variant with SKU`, () => {
        pm.expect(productData.sku).to.be.a('string');
    });

    pm.test(`Product ${index} has a variant with price`, () => {
        pm.expect(productData.price).to.be.a('number');
    });

    pm.test(`Product ${index} has a variant with ID`, () => {
        pm.expect(productData.variantId).to.be.a('number');
    });

    pm.test(`Product ${index} has image URL`, () => {
        pm.expect(productData.imageUrl).to.be.a('string');
    });

    pm.test(`Product ${index} has name`, () => {
        pm.expect(productData.name).to.be.a('string');
    });

    return productData;
});

index = 0;
let priceTotal = 0;
productsData.forEach(productData => {
    index++;

    pm.collectionVariables.unset(`product${index}_id`);
    pm.collectionVariables.unset(`product${index}_sku`);
    pm.collectionVariables.unset(`product${index}_price`);
    pm.collectionVariables.unset(`product${index}_variant_id`);
    pm.collectionVariables.unset(`product${index}_image_url`);
    pm.collectionVariables.unset(`product${index}_name`);

    pm.collectionVariables.set(
        `product${index}_id`,
        productData.productId
    );
    pm.collectionVariables.set(
        `product${index}_sku`,
        productData.sku
    );
    pm.collectionVariables.set(
        `product${index}_price`,
        productData.price
    );
    pm.collectionVariables.set(
        `product${index}_variant_id`,
        productData.variantId
    );
    pm.collectionVariables.set(
        `product${index}_image_url`,
        productData.imageUrl
    );
    pm.collectionVariables.set(
        `product${index}_name`,
        productData.name
    );

    priceTotal += (productData.price * productQty);
});

pm.collectionVariables.unset('products_price_total');
pm.collectionVariables.set('products_price_total', priceTotal);
```

[Next](./02_SubmitQuoteRequest.md)