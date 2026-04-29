# Lab - Postman Registration/Login Workflow (Env Setup)

### Prerequisites

* A BigCommerce [sandbox store](https://docs.bigcommerce.com/developer/docs/start/about/sandboxes) or [trial store](https://www.bigcommerce.com/essentials/), or a full production store
* B2B Edition enabled in your store
* [Postman](https://www.postman.com/) or a similar API client

### Environment Setup

1. **Log in** to your BigCommerce store control panel and **record** the store hash for your BigCommerce store. You can find this in the URL of your control panel, which is in the format _https://store-{store hash}.mybigcommerce.com_.
2. **Create** a new Postman environment with the following variables. (Be sure to save the environment after adding the variables.)


| Variable Name | Value |
| --- | --- |
| store_hash | Your store hash |
| storefront_channel_id | The channel ID of the storefront you will be working with.If you are on a single-storefront implementation, the default channel ID will always be 1. |

3. **Create** a new collection and give it an appropriate name, such as "B2B GraphQL."
4. **Select** your new environment from the environment drop-down at the top of the Postman interface.


The correct value must be selected from the environment drop-down whenever you send a request, in order to ensure the appropriate variables are used.

### Import Collection

Import the [B2B GraphQL Labs collection](../B2B%20GraphQL%20Labs.postman_collection.json).