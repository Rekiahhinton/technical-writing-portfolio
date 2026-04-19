---
title: Stripe PaymentIntents API
---

### Conceptual Overview

Stripe is not just a payment API interface. It is a trust infrastructure that ensures both businesses and customers can interact with each other safely. The Payments Intent API perfectly demonstrates this idea. 

By creating a ``client_secret`` key when the business collects payment, the card credentials flow directly to Stripe allowing the customer to feel secure knowing the business server will never have access to their sensitive information. This eliminates the risk of fraud for the customer and relieves the business of maintaining PCI compliance requirements, diminishing the legal liability that comes with storing sensitive financial data. 

For customers, this security is transformative at the most critical point of contact - checkout. This reduces friction that may deter them from purchasing an item or interacting with the business. The added protection removes a trust barrier that could discourage customers, especially if the business is new and lacks an established reputation.

The Payment Intents API sits at the core of the business value of Stripe - creating trust and peace of mind by providing the means for businesses and customers to interact on digital platforms without apprehension.

## How Payment Intents Works

``Customer → Business Server → Stripe → Bank → Stripe → Business Server``

1. The business server creates a payment intent with a specific currency and value:

``POST /payment_intents``

2. Stripe receives and acknowledges the request before returning a ```` key.
  
3. The business website uses Stripe.js - Stripe's frontend JavaScript library - to collect the card credentials directly in the browser.
   
4. The credentials go to Stripe. The business server never sees them for security purposes.
   
5. Stripe confirms the payment and charges the card the value recorded in the payment intent.

6. The business server is notified about the payment success via a webhook, an automated HTTP request 
    Stripe sends to the business server when a payment event occurs.   

**Note:** The ``client_secret`` key is a randomized token assigned to each transaction the backend server receives upon checkout. The Stripe API also includes an idempotency key that prevents a ``client_secret`` key from being used in multiple transactions if a network error occurs.

## Base URL

``https://api.stripe.com/v1``

The ``v1`` refers to the API version. All endpoints in this document are relative to this base URL.

Stripe maintains ``v1`` as a stable base URL while managing more granular API updates through dated version headers. The examples in this document reflect the current Stripe API version.

## Authentication 
### Bearer Token Authentication

The Stripe API uses Bearer token authentication to manage access to protected resources.

The header contains the Bearer token: ``Authorization: Bearer <token>``

### Keys: Publishable vs. Secret 

```
Publishable Key: pk_test_...

Secret Key: sk_test_...
```

The primary difference between publishable and secret keys is level of confidentiality. Publishable keys are used on the client-side to manage authentication with JavaScript through the browser. Secret keys are only used for server-side integration. This key is never disclosed publicly because it protects highly sensitive information (financial details, passwords, etc.). The publishable key identifies the business to Stripe.js when collecting card credentials in the browser. The secret key authenticates all API requests to Stripe in the backend.

### Test Mode vs. Live Mode
```
Test mode:
pk_test_...   ← publishable test key
sk_test_...   ← secret test key

Live mode:
pk_live_...   ← publishable live key
sk_live_...   ← secret live key
```
Test mode processes calls with simulated data to safely build and test integrations without risk. Live mode processes real customer transactions with money. Never use live mode during development as it poses a security hazard. 

### How to Get Your API Keys

1. Log into your Stripe dashboard.
2. Click the **Developers** button in the lower left-hand corner.
3. Click **API Keys** in the pop-up.
4. Scroll down to **Standard Keys** and copy the secret key for API requests.

### How to Authenticate Your Requests 

```
curl https://api.stripe.com/v1/payment_intents \
  -H "Authorization: Bearer sk_test_yourkeyhere"
```

To authenticate in Postman: 
1. Click the **Authorization** tab.
2. Select **Bearer Token** from the **Type** dropdown.
3. Paste the secret key into the field.
   
```
Authorization tab
Type: Bearer Token
Token: sk_test_yourkeyhere
```
### Security Best Practices

1. Keep your secret key on the server-side and never exposing it to public access. Using the wrong key poses a great security risk.
2. Use test keys during development.
3. Regenerate your keys periodically.
4. Monitor your application for suspicious activity and respond immediately if breached.
5. Embed your secret key into your environment variables. Never store it on any version control system.

**Note:** Allowing your secret key to become public could make it possible for someone to make fraudulent API calls on your behalf. These calls could create charges, issue refunds, and access your customer data. If you accidentally expose your secret key, immediately roll your key in the Stripe dashboard to invalidate the compromised key and generate a new one.

## Endpoint Reference

#### 1. POST v1/payment_intents - Create a Payment Intent

##### Description

Creates a PaymentIntent object, which initiates a ``client_secret`` key for the server to receive instead of the customer's card information. This keeps the specific card numbers safe from misuse or fraud.

##### Parameters Table

| Parameter | Type | Required | Description |
| ---- | ---- | ---- | ---- |
| ``amount`` | integer | Required | The value the PaymentIntent expects to collect for the transaction, represented by a positive integer in the smallest currency unit. e.g., 1000 cents to charge $10.00 usd. |
| ``currency`` | integer | Required | The [ISO currency code](https://www.iso.org/iso-4217-currency-codes.html), represented by three lowercase letters. |
| ``description`` | string | Optional | A string of text attached to the PaymentIntent, potentially to display to users. |
| ``customer`` | string | Optional | An ID assigned to the Customer, if necessary to the transaction. |

##### Example curl request:

```
curl --location --request POST "https://api.stripe.com/v1/payment_intents?amount=10000&currency=usd&description=Your%20transaction%20is%20secure\!" --header "Authorization: Bearer sk_test_your_Bearer_token" --data ""
```
**Note:** This format is compatible with Windows devices. Linux and Mac devices use single quotes and the carot symbol (``^``) to nest data.

##### Example JSON Response:
```
{
    "id": "pi_3TJCpTILrYoCRvlE0Msv9vBA",
    "object": "payment_intent",
    "amount": 10000,
    "amount_capturable": 0,
    "amount_details": {
        "tip": {}
    },
    "amount_received": 0,
    "application": null,
    "application_fee_amount": null,
    "automatic_payment_methods": {
        "allow_redirects": "always",
        "enabled": true
    },
    "canceled_at": null,
    "cancellation_reason": null,
    "capture_method": "automatic_async",
    "client_secret": "pi_3TJCpTILrYoCRvlE0Msv9vBA_secret_RRY7chZOsMHq7CVHNKivCjaay",
    "confirmation_method": "automatic",
    "created": 1775480675,
    "currency": "usd",
    "customer": null,
    "customer_account": null,
    "description": "Your transaction is secure!",
    "excluded_payment_method_types": null,
    "last_payment_error": null,
    "latest_charge": null,
    "livemode": false,
    "metadata": {},
    "next_action": null,
    "on_behalf_of": null,
    "payment_method": null,
    "payment_method_configuration_details": {
        "id": "pmc_1TIw4vILrYoCRvlETa4kE8w3",
        "parent": null
    },
    "payment_method_options": {
        "affirm": {},
        "amazon_pay": {
            "express_checkout_element_session_id": null
        },
        "card": {
            "installments": null,
            "mandate_options": null,
            "network": null,
            "request_three_d_secure": "automatic"
        },
        "cashapp": {},
        "klarna": {
            "preferred_locale": null
        },
        "link": {
            "persistent_token": null
        }
    },
    "payment_method_types": [
        "card",
        "klarna",
        "link",
        "affirm",
        "cashapp",
        "amazon_pay"
    ],
    "processing": null,
    "receipt_email": null,
    "review": null,
    "setup_future_usage": null,
    "shipping": null,
    "source": null,
    "statement_descriptor": null,
    "statement_descriptor_suffix": null,
    "status": "requires_payment_method",
    "transfer_data": null,
    "transfer_group": null
}
```
##### Response Field Definitions

**Note:** The following definitions include primary response fields. Additional fields are conditionally present depending on payment method configuration.

| Name | Type | Description |
| ---- | ---- | ---- |
| ``id`` | string | The identifier of the PaymentIntent specific to each PaymentIntent. |
| ``object`` | string | The type of object Stripe returned. For this endpoint, the value is always ''payment intent''.
| ``currency`` | string | The [ISO currency code](https://www.iso.org/iso-4217-currency-codes.html), represented by three lowercase letters. |
| ``amount`` | integer | The value the PaymentIntent expects to collect for the transaction, represented by a positive integer in the smallest currency unit. e.g., ``1000`` cents to charge $10.00 usd. The value is ``0`` when creating the PaymentIntent.
| ``description`` | string | A string of text attached to the PaymentIntent, potentially to display to users.
| ``status`` | string | The current state of the PaymentIntent. Common values include ``requires_payment_method``, ``requires_confirmation``, ``processing``, and ``succeeded``. |
| ``confirm`` | boolean | An optional setting to confirm the PaymentIntent immediately. Set to ``true`` to combine the creation and confirmation of a PaymentIntent. |
| ``customer`` | string | The ID of the customer this PaymentIntent belongs to, if one exists. | 
| ``receipt_email`` | string | An optional setting for sending a receipt message to the customer's email address. |
| ``client_secret`` | string | A unique token attached to this PaymentIntent for the purpose of obscuring sensitive financial details. Each PaymentIntent receives only one token. Never expose or log this value. |


#### 2. GET v1/payment_intents/{id} - Retrieve a Payment Intent

##### Description

Returns the ``client_secret`` key of a PaymentIntent object. 

##### Parameters Table

| Name | Type | Required | Description |
|---- | ---- | ---- | ---- |
| ``client_secret`` | string | Required | A unique token attached to this PaymentIntent for the purpose of obscuring sensitive financial details. Each PaymentIntent receives only one token. Never expose or log this value. |

##### Example curl Request

```
curl --location "https://api.stripe.com/v1/payment_intents/pi_3TJCpTILrYoCRvlE0Msv9vBAclient_secret=pi_3TJCpTILrYoCRvlE0Msv9vBA_secret_RRY7chZOsMHq7CVHNKivCjaay" --header "Authorization: Bearer sk_test_your_bearer_token"
```

##### Example JSON Response

```
{
    "id": "pi_3TJCpTILrYoCRvlE0Msv9vBA",
    "object": "payment_intent",
    "amount": 10000,
    "amount_capturable": 0,
    "amount_details": {
        "tip": {}
    },
    "amount_received": 0,
    "application": null,
    "application_fee_amount": null,
    "automatic_payment_methods": {
        "allow_redirects": "always",
        "enabled": true
    },
    "canceled_at": null,
    "cancellation_reason": null,
    "capture_method": "automatic_async",
    "client_secret": "pi_3TJCpTILrYoCRvlE0Msv9vBA_secret_RRY7chZOsMHq7CVHNKivCjaay",
    "confirmation_method": "automatic",
    "created": 1775480675,
    "currency": "usd",
    "customer": null,
    "customer_account": null,
    "description": "Your transaction is secure!",
    "excluded_payment_method_types": null,
    "last_payment_error": null,
    "latest_charge": null,
    "livemode": false,
    "metadata": {},
    "next_action": null,
    "on_behalf_of": null,
    "payment_method": null,
    "payment_method_configuration_details": {
        "id": "pmc_1TIw4vILrYoCRvlETa4kE8w3",
        "parent": null
    },
    "payment_method_options": {
        "affirm": {},
        "amazon_pay": {
            "express_checkout_element_session_id": null
        },
        "card": {
            "installments": null,
            "mandate_options": null,
            "network": null,
            "request_three_d_secure": "automatic"
        },
        "cashapp": {},
        "klarna": {
            "preferred_locale": null
        },
        "link": {
            "persistent_token": null
        }
    },
    "payment_method_types": [
        "card",
        "klarna",
        "link",
        "affirm",
        "cashapp",
        "amazon_pay"
    ],
    "processing": null,
    "receipt_email": null,
    "review": null,
    "setup_future_usage": null,
    "shipping": null,
    "source": null,
    "statement_descriptor": null,
    "statement_descriptor_suffix": null,
    "status": "requires_payment_method",
    "transfer_data": null,
    "transfer_group": null
}
```

##### Response Field Definitions

**Note:** The following definitions include primary response fields. Additional fields are conditionally present depending on payment method configuration.

| Name | Type | Description |
| ---- | ---- | ---- |
| ``id`` | string | The identifier of the PaymentIntent specific to each PaymentIntent. |
| ``object`` | string | The type of object Stripe returned. For this endpoint, the value is always ''payment intent''.
| ``currency`` | string | The [ISO currency code](https://www.iso.org/iso-4217-currency-codes.html), represented by three lowercase letters. |
| ``amount`` | integer | The value the PaymentIntent expects to collect for the transaction, represented by a positive integer in the smallest currency unit. e.g., ``1000`` cents to charge $10.00 usd. The value is ``0`` when creating the PaymentIntent.
| ``status`` | string | The current state of the PaymentIntent. Common values include ``requires_payment_method``, ``requires_confirmation``, ``processing``, and ``succeeded``. |
| ``client_secret`` | string | A unique token attached to this PaymentIntent for the purpose of obscuring sensitive financial details. Each PaymentIntent receives only one token. Never expose or log this value. |



#### 3. GET v1/payment_intents - List All Payment Intents

##### Description 

Returns a list of most recent PaymentIntents up to a default of 10.

##### Parameters Table

| Parameter | Type | Required | Description |
| ---- | ---- | ---- | ---- |
| ``limit`` | integer | Optional | Sets a maximum number of PaymentIntents the call will return. |
| ``customer`` | string | Optional | ID of the customer the PaymentIntents are assigned. |
| ``customer_account`` | string | Optional | ID of the customer account that has PaymentIntents. |


##### Example curl Request

```
curl --location "https://api.stripe.com/v1/payment_intents?limit=2" --header "Authorization: Bearer sk_test_your_bearer_token"
```

##### Example JSON Response

```
{
    "object": "list",
    "data": [
        {
            "id": "pi_3TJb7eILrYoCRvlE1By25F2M",
            "object": "payment_intent",
            "amount": 10000,
            "amount_capturable": 0,
            "amount_details": {
                "tip": {}
            },
            "amount_received": 0,
            "application": null,
            "application_fee_amount": null,
            "automatic_payment_methods": {
                "allow_redirects": "always",
                "enabled": true
            },
            "canceled_at": null,
            "cancellation_reason": null,
            "capture_method": "automatic_async",
            "client_secret": "pi_3TJb7eILrYoCRvlE1By25F2M_secret_AFVGiIShOUcML9aBs5pOHkOyg",
            "confirmation_method": "automatic",
            "created": 1775574058,
            "currency": "usd",
            "customer": null,
            "customer_account": null,
            "description": "Your transaction is secure\\!",
            "excluded_payment_method_types": null,
            "last_payment_error": null,
            "latest_charge": null,
            "livemode": false,
            "metadata": {},
            "next_action": null,
            "on_behalf_of": null,
            "payment_method": null,
            "payment_method_configuration_details": {
                "id": "pmc_1TIw4vILrYoCRvlETa4kE8w3",
                "parent": null
            },
            "payment_method_options": {
                "affirm": {},
                "amazon_pay": {
                    "express_checkout_element_session_id": null
                },
                "card": {
                    "installments": null,
                    "mandate_options": null,
                    "network": null,
                    "request_three_d_secure": "automatic"
                },
                "cashapp": {},
                "klarna": {
                    "preferred_locale": null
                },
                "link": {
                    "persistent_token": null
                }
            },
            "payment_method_types": [
                "card",
                "klarna",
                "link",
                "affirm",
                "cashapp",
                "amazon_pay"
            ],
            "processing": null,
            "receipt_email": null,
            "review": null,
            "setup_future_usage": null,
            "shipping": null,
            "source": null,
            "statement_descriptor": null,
            "statement_descriptor_suffix": null,
            "status": "requires_payment_method",
            "transfer_data": null,
            "transfer_group": null
        },
        {
            "id": "pi_3TJCpTILrYoCRvlE0Msv9vBA",
            "object": "payment_intent",
            "amount": 10000,
            "amount_capturable": 0,
            "amount_details": {
                "tip": {}
            },
            "amount_received": 0,
            "application": null,
            "application_fee_amount": null,
            "automatic_payment_methods": {
                "allow_redirects": "always",
                "enabled": true
            },
            "canceled_at": null,
            "cancellation_reason": null,
            "capture_method": "automatic_async",
            "client_secret": "pi_3TJCpTILrYoCRvlE0Msv9vBA_secret_RRY7chZOsMHq7CVHNKivCjaay",
            "confirmation_method": "automatic",
            "created": 1775480675,
            "currency": "usd",
            "customer": null,
            "customer_account": null,
            "description": "Your transaction is secure!",
            "excluded_payment_method_types": null,
            "last_payment_error": null,
            "latest_charge": null,
            "livemode": false,
            "metadata": {},
            "next_action": null,
            "on_behalf_of": null,
            "payment_method": null,
            "payment_method_configuration_details": {
                "id": "pmc_1TIw4vILrYoCRvlETa4kE8w3",
                "parent": null
            },
            "payment_method_options": {
                "affirm": {},
                "amazon_pay": {
                    "express_checkout_element_session_id": null
                },
                "card": {
                    "installments": null,
                    "mandate_options": null,
                    "network": null,
                    "request_three_d_secure": "automatic"
                },
                "cashapp": {},
                "klarna": {
                    "preferred_locale": null
                },
                "link": {
                    "persistent_token": null
                }
            },
            "payment_method_types": [
                "card",
                "klarna",
                "link",
                "affirm",
                "cashapp",
                "amazon_pay"
            ],
            "processing": null,
            "receipt_email": null,
            "review": null,
            "setup_future_usage": null,
            "shipping": null,
            "source": null,
            "statement_descriptor": null,
            "statement_descriptor_suffix": null,
            "status": "requires_payment_method",
            "transfer_data": null,
            "transfer_group": null
        }
    ],
    "has_more": true,
    "url": "/v1/payment_intents"
}
```

##### Response Field Definitions

Note: The following definitions include primary response fields. Additional fields are conditionally present depending on payment method configuration.

| Name | Type | Description |
| ---- | ---- | ---- |
| ``id`` | string | The identifier of the PaymentIntent specific to each PaymentIntent. |
| ``object`` | string | The type of object Stripe returned. For this endpoint, the value is always ''list'' **Note:** The object of each PaymentIntent listed will have the value of ``payment_intent``. |
| ``currency`` | string | The [ISO currency code](https://www.iso.org/iso-4217-currency-codes.html), represented by three lowercase letters. |
| ``amount`` | integer | The value the PaymentIntent expects to collect for the transaction, represented by a positive integer in the smallest currency unit. e.g., ``1000`` cents to charge $10.00 usd. The value is ``0`` when creating the PaymentIntent. |
| ``status`` | string | The current state of the PaymentIntent. Common values include ``requires_payment_method``, ``requires_confirmation``, ``processing``, and ``succeeded``. |
| ``client_secret`` | string | A unique token attached to this PaymentIntent for the purpose of obscuring sensitive financial details. Each PaymentIntent receives only one token. Never expose or log this value. |
| ``url`` | string | The endpoint path used to generate the list of PaymentIntents. |

 
#### 4. POST v1/payment_intents/{id}/confirm - Confirm a Payment Intent 

##### Description 

Indicates that the customer intends to pay with the current or provided payment method. A successful confirmation initiates a charge to the payment method. 

##### Parameters Table 

| Name | Type | Required | Description |
| ---- | ---- | ---- | ---- |
| ``payment_method`` | string | Required | The opaque reference ID of the payment method typically created by Stripe.js on the client side. **Note:** When calling this endpoint in test mode, you must manually create the payment method.|
| ``return_url ``| string | Required | A HTTPS redirect URL for sending the customer after payment confirmation. |
| ``receipt_email`` | string | Required | An email address for sending a confirmation email to the customer after payment confirmation. |
| ``setup_future_usage`` | enum | Required | Determines whether to save the payment method. Enum values include ``on_session``, which indicates the customer is present during the payment confirmation, and ``off_session``, which indicates the customer is not present during the payment confirmation. The ``off_session`` enum is useful for monthly subscription payments. |   


##### Example curl Request

```
curl --location "https://api.stripe.com/v1/payment_intents/pi_3TLYfPILrYoCRvlE0lj6Xh53/confirm" --header "Content-Type: application/x-www-form-urlencoded" --header "Authorization: Bearer sk_test_your_bearer_token" --data-urlencode "return_url=https://google.com"
```

##### Example JSON Response

```
{
    "id": "pi_3TLYfPILrYoCRvlE0lj6Xh53",
    "object": "payment_intent",
    "amount": 2000,
    "amount_capturable": 0,
    "amount_details": {
        "tip": {}
    },
    "amount_received": 2000,
    "application": null,
    "application_fee_amount": null,
    "automatic_payment_methods": {
        "allow_redirects": "always",
        "enabled": true
    },
    "canceled_at": null,
    "cancellation_reason": null,
    "capture_method": "automatic_async",
    "client_secret": "pi_3TLYfPILrYoCRvlE0lj6Xh53_secret_VnRfFeluaWHVQRnOI3HvXROJ8",
    "confirmation_method": "automatic",
    "created": 1776041275,
    "currency": "usd",
    "customer": null,
    "customer_account": null,
    "description": null,
    "excluded_payment_method_types": null,
    "last_payment_error": null,
    "latest_charge": "ch_3TLYfPILrYoCRvlE0KQfoPGh",
    "livemode": false,
    "managed_payments": {
        "enabled": false
    },
    "metadata": {},
    "next_action": null,
    "on_behalf_of": null,
    "payment_method": "pm_1TLYdKILrYoCRvlEbS42JYLi",
    "payment_method_configuration_details": {
        "id": "pmc_1TIw4vILrYoCRvlETa4kE8w3",
        "parent": null
    },
    "payment_method_options": {
        "amazon_pay": {
            "express_checkout_element_session_id": null
        },
        "card": {
            "installments": null,
            "mandate_options": null,
            "network": null,
            "request_three_d_secure": "automatic"
        },
        "cashapp": {},
        "klarna": {
            "preferred_locale": null
        },
        "link": {
            "persistent_token": null
        }
    },
    "payment_method_types": [
        "card",
        "klarna",
        "link",
        "cashapp",
        "amazon_pay"
    ],
    "processing": null,
    "receipt_email": null,
    "review": null,
    "setup_future_usage": null,
    "shipping": null,
    "source": null,
    "statement_descriptor": null,
    "statement_descriptor_suffix": null,
    "status": "succeeded",
    "transfer_data": null,
    "transfer_group": null
}
```

##### Response Field Definitions

Note: The following definitions include primary response fields. Additional fields are conditionally present depending on payment method configuration.

| Name | Type | Description |
| ---- | ---- | ---- |
| ``id`` | string | The identifier of the PaymentIntent specific to each PaymentIntent. |
| ``object`` | string | The type of object Stripe returned. For this endpoint, the value is always ''payment intent''. |
| ``currency`` | string | The [ISO currency code](https://www.iso.org/iso-4217-currency-codes.html), represented by three lowercase letters. |
| ``amount`` | integer | The value the PaymentIntent expects to collect for the transaction, represented by a positive integer in the smallest currency unit. e.g., ``1000`` cents to charge $10.00 usd. The value is ``0`` when creating the PaymentIntent. |
| ``description`` | string | A string of text attached to the PaymentIntent, potentially to display to users. |
| ``status`` | string | The current state of the PaymentIntent. Common values include ``requires_payment_method``, ``requires_confirmation``, ``processing``, and ``succeeded``. |
| ``client_secret`` | string | A unique token attached to this PaymentIntent for the purpose of obscuring sensitive financial details. Each PaymentIntent receives only one token. Never expose or log this value. |
| ``amount_received`` | integer | The monetary value successfully captured from the ``payment_method``. The value of ``amount_received`` is always ``0`` before the payment has been collected. |
| ``payment_method`` | string | The opaque reference ID of the payment method typically created by Stripe.js on the client side. This is not the card numbers from the customer's payment method. **Note:** When calling this endpoint in test mode, you must manually create the payment method. |
| ``payment_method_types`` | array of strings | The options of payment methods available to the customer. Possible values: ``card``, ``klarna``, ``link``, ``cashapp``, or ``amazon_pay``. |


#### 5. POST v1/payment_intents/{id}/cancel - Cancel a Payment Intent

##### Description

Indicates that the PaymentIntent is no longer functional and cannot be applied to any additional charges. Any operations made on the PaymentIntent fail and return an error. 

The PaymentIntent can only be canceled in one of the following states: ``requires_payment_method``, ``requires_capture``, ``requires_confirmation``, ``requires_action``, or rarely ``processing``.

##### Parameters Table

| Name | Type | Required | Description |
| ---- | ---- | ---- | ---- |
| ``cancellation_reason`` | string | Optional | The reason for cancelling the PaymentIntent. Possible values: ``duplicate``, ``fraudulent``, ``requested_by_customer``, or ``abandoned`` |

##### Example curl Request

```
curl --location --request POST "https://api.stripe.com/v1/payment_intents/pi_3TNMYgILrYoCRvlE159KaAai/cancel" --header "Authorization: Bearer sk_test_your_bearer_token"
```


##### Example JSON Response

```
{
    "id": "pi_3TNMYgILrYoCRvlE159KaAai",
    "object": "payment_intent",
    "amount": 10000,
    "amount_capturable": 0,
    "amount_details": {
        "tip": {}
    },
    "amount_received": 0,
    "application": null,
    "application_fee_amount": null,
    "automatic_payment_methods": {
        "allow_redirects": "always",
        "enabled": true
    },
    "canceled_at": 1776472241,
    "cancellation_reason": null,
    "capture_method": "automatic_async",
    "client_secret": "pi_3TNMYgILrYoCRvlE159KaAai_secret_khZDpcsFJabWZE0kEYmkTjRgu",
    "confirmation_method": "automatic",
    "created": 1776471386,
    "currency": "usd",
    "customer": null,
    "customer_account": null,
    "description": "Your transaction is secure!",
    "excluded_payment_method_types": null,
    "last_payment_error": null,
    "latest_charge": null,
    "livemode": false,
    "managed_payments": {
        "enabled": false
    },
    "metadata": {},
    "next_action": null,
    "on_behalf_of": null,
    "payment_method": null,
    "payment_method_configuration_details": {
        "id": "pmc_1TIw4vILrYoCRvlETa4kE8w3",
        "parent": null
    },
    "payment_method_options": {
        "affirm": {},
        "amazon_pay": {
            "express_checkout_element_session_id": null
        },
        "card": {
            "installments": null,
            "mandate_options": null,
            "network": null,
            "request_three_d_secure": "automatic"
        },
        "cashapp": {},
        "klarna": {
            "preferred_locale": null
        },
        "link": {
            "persistent_token": null
        }
    },
    "payment_method_types": [
        "card",
        "klarna",
        "link",
        "affirm",
        "cashapp",
        "amazon_pay"
    ],
    "processing": null,
    "receipt_email": null,
    "review": null,
    "setup_future_usage": null,
    "shipping": null,
    "source": null,
    "statement_descriptor": null,
    "statement_descriptor_suffix": null,
    "status": "canceled",
    "transfer_data": null,
    "transfer_group": null
}
```

##### Response Field Definitions

Note: The following definitions include primary response fields. Additional fields are conditionally present depending on payment method configuration.

| Name | Type | Description |
| ---- | ---- | ---- |
| ``id`` | string | The identifier of the PaymentIntent specific to each PaymentIntent. |
| ``object`` | string | The type of object Stripe returned. For this endpoint, the value is always ''payment intent''.
| ``currency`` | string | The [ISO currency code](https://www.iso.org/iso-4217-currency-codes.html), represented by three lowercase letters. |
| ``amount`` | integer | The value the PaymentIntent expects to collect for the transaction, represented by a positive integer in the smallest currency unit. e.g., ``1000`` cents to charge $10.00 usd. The value is ``0`` when creating the PaymentIntent.
| ``description`` | string | A string of text attached to the PaymentIntent, potentially to display to users.
| ``status`` | string | The current state of the PaymentIntent. You can only cancel a PaymentIntent when the status has one of these values: ``requires_payment_method``, ``requires_capture``, ``requires_confirmation``, or ``processing``. |
| ``client_secret`` | string | A unique token attached to this PaymentIntent for the purpose of obscuring sensitive financial details. Each PaymentIntent receives only one token. Never expose or log this value. |
| ``cancellation_reason`` | string | The reason for cancelling the PaymentIntent. Possible values: ``duplicate``, ``fraudulent``, ``requested_by_customer``, or ``abandoned``. This value is ``null`` if no reason is manually added.|


## Payment Intent Statuses

## Error Handling 

### Overview

Stripe signals errors with HTTP statuses. The ``2xx`` HTTP responses indicate the API returned a successful response. The ``4xx``responses indicate an error occurred and the request could not be completed. The ``5xx`` responses indicate the request could not complete because of an internal server error. This response is less likely to occur.

The error message always follows a particular JSON structure which includes certains fields in the object: ``code``, ``type``, ``message``, ``param``, and ``doc_url``. Understanding these fields and the corresponding data within can help decode error messages and correct your requests sooner. 

| Error Type | Description |
| ---- | ---- | 
| ``api_error`` | Indicates disruptions with the API itself (e.g. Stripe's servers are down.) This is unlikely to occur. |
| ``card_error`` | Indicates a problem with the payment method. This is the error that is most likely to occur. |
| ``idempotency_error`` | Occurs when an ``idempotency-key`` is used on another request with different parameters than the first. |
| ``invalid_request_error`` | Occurs when the request contains invalid parameters. E.g., missing parameters, invalid values, unknown values. | 

This section lists possible error scenarios for each endpoint and offers some useful tips for solving them according to the error type.

### Create a Payment Intent

#### 400 Bad Request

This error occurs when the request is missing required parameters.

```
{
    "error": {
        "code": "parameter_missing",
        "doc_url": "https://stripe.com/docs/error-codes/parameter-missing",
        "message": "Missing required param: currency.",
        "param": "currency",
        "request_log_url": "https://dashboard.stripe.com/acct_1TIw4KILrYoCRvlE/test/workbench/logs?object=req_lKZob95PYelojc",
        "type": "invalid_request_error"
    }
}
```

To fix this error, refer to the ``param`` field for information on which parameters are missing. The Create a PaymentIntent endpoint requires the ``amount`` and ``currency`` parameters.

This example error message shows ``currency`` as the missing parameter. 

#### 401 Unauthorized

This error occurs when the request does not include a valid API key to authorize access to the PaymentIntents resource. 

```
{
    "error": {
        "message": "You did not provide an API key. You need to provide your API key in the Authorization header, using Bearer auth (e.g. 'Authorization: Bearer YOUR_SECRET_KEY'). See https://stripe.com/docs/api#authentication for details, or we can help at https://support.stripe.com/.",
        "type": "invalid_request_error"
    }
}
```

To fix this error, configure the authorization with the proper token to gain access the PaymentIntents resource. The ``curl`` request should show your bearer token in the command. 

#### 404 Not Found

This error occurs when Stripe does not recognize the URL. This is usually due to a typo. This error occurred in this particular example because a trailing slash was appended at the end of the URL.

```
    "error": {
        "message": "Unrecognized request URL (POST: /v1/payment_intents/). If you are trying to list objects, remove the trailing slash. If you are trying to retrieve an object, make sure you passed a valid (non-empty) identifier in your code. Please see https://stripe.com/docs or we can help at https://support.stripe.com/.",
        "type": "invalid_request_error"
    }
}
```

To fix this error, check the URL for any spelling mistakes or trailing slashes. The ``message`` field always contains useful information for debugging the error. 

### Retrieve a Payment Intent

#### 404 Not Found

This error occurs when Stripe does not recognize the URL. In this case, the ``client_secret`` key is passed into the URL instead of the PaymentIntent ID.

```
{
    "error": {
        "message": "You passed a string that looks like a client secret as the PaymentIntent ID, but you must provide a valid PaymentIntent ID. Please use the value in the `id` field of the PaymentIntent.",
        "param": "intent",
        "request_log_url": "https://dashboard.stripe.com/acct_1TIw4KILrYoCRvlE/test/workbench/logs?object=req_qXc0qoGMmN6RrW",
        "type": "invalid_request_error"
    }
}
```

The ``message`` field in this example reports the problem and provides relevant information on passing the PaymentIntent ID. Replace the ``client_secret`` key with the PaymentIntent ID to fix the error. 

The ``request_log_url`` field also provides a link to the developer log for this error message to examine the error further. 


### List All Payment Intents

#### 400 Bad Request

This error occurs when a PaymentIntents ID is passed into the parameters instead of customer ID.

```
{
    "error": {
        "code": "resource_missing",
        "doc_url": "https://stripe.com/docs/error-codes/resource-missing",
        "message": "No such customer: 'pi_83249u9fdf89suf89389u42'",
        "param": "customer",
        "request_log_url": "https://dashboard.stripe.com/acct_1TIw4KILrYoCRvlE/test/workbench/logs?object=req_62plfPg2ypuQgQ",
        "type": "invalid_request_error"
    }
}
```

To fix this error, pass a valid customer ID beginning with ``cus``. Also, be sure the ID does not belong to another Stripe account as this will also throw a resource error. 

The List All PaymentIntents endpoint has limited error scenarios because it has no required parameters.

### Confirm a Payment Intent

#### 400 Bad Request

This error occurs due to missing parameters. Once a Confirm request is sent, it tries to initiate a transaction. However, if the PaymentIntent accepts payment methods, it may lead to a browser redirect. This necessitates a ``return_url`` address for the customer to return to after completing the online payment flow. If a ``return_url`` is not provided before sending the request, Stripe throws an error:

```
{
    "error": {
        "message": "This PaymentIntent is configured to accept payment methods enabled in your Dashboard. Because some of these payment methods might redirect your customer off of your page, you must provide a `return_url`. If you don't want to accept redirect-based payment methods, set `automatic_payment_methods[enabled]` to `true` and `automatic_payment_methods[allow_redirects]` to `never` when creating Setup Intents and Payment Intents.",
        "payment_intent": {
            "id": "pi_3TJ1tVILrYoCRvlE06CjkdFv",
            "object": "payment_intent",
            "amount": 2000,
            "amount_capturable": 0,
            "amount_details": {
                "tip": {}
            },
            "amount_received": 0,
            "application": null,
            "application_fee_amount": null,
            "automatic_payment_methods": {
                "allow_redirects": "always",
                "enabled": true
            },
            "canceled_at": null,
            "cancellation_reason": null,
            "capture_method": "automatic_async",
            "client_secret": "pi_3TJ1tVILrYoCRvlE06CjkdFv_secret_SOph7sl7q0uCe33mtH5DVAACO",
            "confirmation_method": "automatic",
            "created": 1775438641,
            "currency": "usd",
            "customer": null,
            "customer_account": null,
            "description": null,
            "excluded_payment_method_types": null,
            "last_payment_error": null,
            "latest_charge": null,
            "livemode": false,
            "managed_payments": {
                "enabled": false
            },
            "metadata": {},
            "next_action": null,
            "on_behalf_of": null,
            "payment_method": null,
            "payment_method_configuration_details": {
                "id": "pmc_1TIw4vILrYoCRvlETa4kE8w3",
                "parent": null
            },
            "payment_method_options": {
                "amazon_pay": {
                    "express_checkout_element_session_id": null
                },
                "card": {
                    "installments": null,
                    "mandate_options": null,
                    "network": null,
                    "request_three_d_secure": "automatic"
                },
                "cashapp": {},
                "klarna": {
                    "preferred_locale": null
                },
                "link": {
                    "persistent_token": null
                }
            },
            "payment_method_types": [
                "card",
                "klarna",
                "link",
                "cashapp",
                "amazon_pay"
            ],
            "processing": null,
            "receipt_email": null,
            "review": null,
            "setup_future_usage": null,
            "shipping": null,
            "source": null,
            "statement_descriptor": null,
            "statement_descriptor_suffix": null,
            "status": "requires_payment_method",
            "transfer_data": null,
            "transfer_group": null
        },
        "request_log_url": "https://dashboard.stripe.com/acct_1TIw4KILrYoCRvlE/test/workbench/logs?object=req_oa7DPWVFTn4Di6",
        "type": "invalid_request_error"
    }

```

Fix this error by completing the steps in the ``message`` field. Assign a ``return_url`` in the Params box or reconfigure your settings for ``automatic_payment_methods`` in Setup Intents and PaymentIntents.  

**Note**: Stripe returns the entire PaymentIntent object for this error. This additional information is useful for viewing the state of the PaymentIntent and finding the bottleneck. The response reports that this PaymentIntent has the status "requires_payment_method." Before the PaymentIntent can be confirmed and the transaction initiated, a payment method must be provided.

#### 400 Bad Request

A Bad Request error can also occur if a Confirm request is sent for a PaymentIntent that has already been confirmed:

```
{
    "error": {
        "code": "payment_intent_unexpected_state",
        "doc_url": "https://stripe.com/docs/error-codes/payment-intent-unexpected-state",
        "message": "You cannot confirm this PaymentIntent because it has already succeeded after being previously confirmed.",
        "payment_intent": {
            "id": "pi_3TLYfPILrYoCRvlE0lj6Xh53",
            "object": "payment_intent",
            "amount": 2000,
            "amount_capturable": 0,
            "amount_details": {
                "tip": {}
            },
            "amount_received": 2000,
            "application": null,
            "application_fee_amount": null,
            "automatic_payment_methods": {
                "allow_redirects": "always",
                "enabled": true
            },
            "canceled_at": null,
            "cancellation_reason": null,
            "capture_method": "automatic_async",
            "client_secret": "pi_3TLYfPILrYoCRvlE0lj6Xh53_secret_VnRfFeluaWHVQRnOI3HvXROJ8",
            "confirmation_method": "automatic",
            "created": 1776041275,
            "currency": "usd",
            "customer": null,
            "customer_account": null,
            "description": null,
            "excluded_payment_method_types": null,
            "last_payment_error": null,
            "latest_charge": "ch_3TLYfPILrYoCRvlE0KQfoPGh",
            "livemode": false,
            "managed_payments": {
                "enabled": false
            },
            "metadata": {},
            "next_action": null,
            "on_behalf_of": null,
            "payment_method": "pm_1TLYdKILrYoCRvlEbS42JYLi",
            "payment_method_configuration_details": {
                "id": "pmc_1TIw4vILrYoCRvlETa4kE8w3",
                "parent": null
            },
            "payment_method_options": {
                "amazon_pay": {
                    "express_checkout_element_session_id": null
                },
                "card": {
                    "installments": null,
                    "mandate_options": null,
                    "network": null,
                    "request_three_d_secure": "automatic"
                },
                "cashapp": {},
                "klarna": {
                    "preferred_locale": null
                },
                "link": {
                    "persistent_token": null
                }
            },
            "payment_method_types": [
                "card",
                "klarna",
                "link",
                "cashapp",
                "amazon_pay"
            ],
            "processing": null,
            "receipt_email": null,
            "review": null,
            "setup_future_usage": null,
            "shipping": null,
            "source": null,
            "statement_descriptor": null,
            "statement_descriptor_suffix": null,
            "status": "succeeded",
            "transfer_data": null,
            "transfer_group": null
        },
        "request_log_url": "https://dashboard.stripe.com/acct_1TIw4KILrYoCRvlE/test/workbench/logs?object=req_GxFlJAseVaYlfG",
        "type": "invalid_request_error"
    }
}
```

The ``message`` field explains that the PaymentIntent has already been confirmed. The JSON response also shows the status of the PaymentIntent is "succeeded," which indicates the PaymentIntent has already processed a previous confirm request. 

Fix this error by refraining from sending another confirm request on a PaymentIntent that has already succeeded.

### Cancel a Payment Intent

#### 400 Bad Request

This error occurs when the Cancel request includes unnecessary parameters. The Cancel a PaymentIntent endpoint does not accept ``amount`` or ``currency`` as parameters, only ``cancellation_reason``; therefore, the request is invalid and returns an error:

```
{
    "error": {
        "code": "parameter_unknown",
        "doc_url": "https://stripe.com/docs/error-codes/parameter-unknown",
        "message": "Received unknown parameters: amount, currency",
        "param": "amount",
        "payment_intent": {
            "id": "pi_3TNMYgILrYoCRvlE159KaAai",
            "object": "payment_intent",
            "amount": 10000,
            "amount_capturable": 0,
            "amount_details": {
                "tip": {}
            },
            "amount_received": 0,
            "application": null,
            "application_fee_amount": null,
            "automatic_payment_methods": {
                "allow_redirects": "always",
                "enabled": true
            },
            "canceled_at": 1776472241,
            "cancellation_reason": null,
            "capture_method": "automatic_async",
            "client_secret": "pi_3TNMYgILrYoCRvlE159KaAai_secret_khZDpcsFJabWZE0kEYmkTjRgu",
            "confirmation_method": "automatic",
            "created": 1776471386,
            "currency": "usd",
            "customer": null,
            "customer_account": null,
            "description": "Your transaction is secure!",
            "excluded_payment_method_types": null,
            "last_payment_error": null,
            "latest_charge": null,
            "livemode": false,
            "managed_payments": {
                "enabled": false
            },
            "metadata": {},
            "next_action": null,
            "on_behalf_of": null,
            "payment_method": null,
            "payment_method_configuration_details": {
                "id": "pmc_1TIw4vILrYoCRvlETa4kE8w3",
                "parent": null
            },
            "payment_method_options": {
                "affirm": {},
                "amazon_pay": {
                    "express_checkout_element_session_id": null
                },
                "card": {
                    "installments": null,
                    "mandate_options": null,
                    "network": null,
                    "request_three_d_secure": "automatic"
                },
                "cashapp": {},
                "klarna": {
                    "preferred_locale": null
                },
                "link": {
                    "persistent_token": null
                }
            },
            "payment_method_types": [
                "card",
                "klarna",
                "link",
                "affirm",
                "cashapp",
                "amazon_pay"
            ],
            "processing": null,
            "receipt_email": null,
            "review": null,
            "setup_future_usage": null,
            "shipping": null,
            "source": null,
            "statement_descriptor": null,
            "statement_descriptor_suffix": null,
            "status": "canceled",
            "transfer_data": null,
            "transfer_group": null
        },
        "request_log_url": "https://dashboard.stripe.com/acct_1TIw4KILrYoCRvlE/test/workbench/logs?object=req_Dg958GNaOoJEeb",
        "type": "invalid_request_error"
    }
}
```

Fix this error by removing the unknown ``amount`` and ``currency`` parameters listed in the ``message``field. 

#### 400 Bad Request

This error occurs when Stripe receives a Cancel request for a PaymentIntent that has already succeeded. The PaymentIntent cannot be canceled after previously confirming and succeeding. 

```
{
    "error": {
        "code": "payment_intent_unexpected_state",
        "doc_url": "https://stripe.com/docs/error-codes/payment-intent-unexpected-state",
        "message": "You cannot cancel this PaymentIntent because it has a status of succeeded. Only a PaymentIntent with one of the following statuses may be canceled: requires_payment_method, requires_capture, requires_reauthorization, requires_confirmation, requires_action, processing.",
        "payment_intent": {
            "id": "pi_3TLYfPILrYoCRvlE0lj6Xh53",
            "object": "payment_intent",
            "amount": 2000,
            "amount_capturable": 0,
            "amount_details": {
                "tip": {}
            },
            "amount_received": 2000,
            "application": null,
            "application_fee_amount": null,
            "automatic_payment_methods": {
                "allow_redirects": "always",
                "enabled": true
            },
            "canceled_at": null,
            "cancellation_reason": null,
            "capture_method": "automatic_async",
            "client_secret": "pi_3TLYfPILrYoCRvlE0lj6Xh53_secret_VnRfFeluaWHVQRnOI3HvXROJ8",
            "confirmation_method": "automatic",
            "created": 1776041275,
            "currency": "usd",
            "customer": null,
            "customer_account": null,
            "description": null,
            "excluded_payment_method_types": null,
            "last_payment_error": null,
            "latest_charge": "ch_3TLYfPILrYoCRvlE0KQfoPGh",
            "livemode": false,
            "managed_payments": {
                "enabled": false
            },
            "metadata": {},
            "next_action": null,
            "on_behalf_of": null,
            "payment_method": "pm_1TLYdKILrYoCRvlEbS42JYLi",
            "payment_method_configuration_details": {
                "id": "pmc_1TIw4vILrYoCRvlETa4kE8w3",
                "parent": null
            },
            "payment_method_options": {
                "amazon_pay": {
                    "express_checkout_element_session_id": null
                },
                "card": {
                    "installments": null,
                    "mandate_options": null,
                    "network": null,
                    "request_three_d_secure": "automatic"
                },
                "cashapp": {},
                "klarna": {
                    "preferred_locale": null
                },
                "link": {
                    "persistent_token": null
                }
            },
            "payment_method_types": [
                "card",
                "klarna",
                "link",
                "cashapp",
                "amazon_pay"
            ],
            "processing": null,
            "receipt_email": null,
            "review": null,
            "setup_future_usage": null,
            "shipping": null,
            "source": null,
            "statement_descriptor": null,
            "statement_descriptor_suffix": null,
            "status": "succeeded",
            "transfer_data": null,
            "transfer_group": null
        },
        "request_log_url": "https://dashboard.stripe.com/acct_1TIw4KILrYoCRvlE/test/workbench/logs?object=req_423GwXfX2uV0m0",
        "type": "invalid_request_error"
    }
}
```

Only PaymentIntents that have not yet succeeded can be canceled. Valid status for cancelable PaymentIntents include: ``requires_payment_method``, ``requires_confirmation``, and ``requires_capture``.
 
## Card Decline Codes

When initiating a transaction, the customer's payment method might decline. A developer must known how to resolve these errors effectively proceed with the transaction. Utilizing the table below, you can learn why particular card decline codes appear and how to take the next step in resolving them.

| Card Decline Code | Description | Actionable Steps |
| ---- | ---- | ---- |
| ``authentication_required`` | the | the |
| ``card_declined``| The payment method has not succeeded. When a payment method declines, the corresponding error message will also include a ``decline_code`` attribute that gives the specific reason the payment method declined. | Three |
| ``duplicate_transaction`` | 
| ``expired_card`` | the | the |
| ``fraudulent`` | stealing | is wrong. |
| ``insufficient_funds`` | broke | bitch. |
| ``invalid_cvv`` | wrong | number. |
| ``processing_error`` | stop | working. |

## Getting Started - Your First Payment Intent

## Testing 

## Glossary 

| Term | Definition |
| ---- | ---- |
| 3D Secure | A security protocol that adds an extra authentication step for online card payments, reducing fraud by verifying the cardholder's identity directly with the issuer. |
| Bearer Token | An opaque string, such as a JSON Web Token (JWT), that acts as a digital key granting access to protected API resources to whoever holds it. Generally used with OAuth 2.0. |
| Charge | The withdrawal transaction applied to the payment method for the PaymentIntent. |
| ``client_secret`` | A unique token attached to this PaymentIntent for the purpose of obscuring sensitive financial details. Each PaymentIntent receives only one token. Never expose or log this value. | 
| Idempotency Key | An API key that guarantees each transaction only uses a single PaymentIntent and does not risk applying multiple charges to the same order.|
| ISO Currency Code | The three-letter code standard used internationally to identify almost 300 different currencies. The currencies are represented both alphabetically and numerically. E.g., The US dollar is represented by the code USD.|
| Live Mode | The production environment where real, actionable transactions occur. |
| PaymentIntent | A Stripe API object that tracks the lifecycle of a customer's checkout order. The PaymentIntent is created as an intention to collect payment by issuing a ``client_secret`` key.|
| Publishable Key | A public, safe-to-share API key used in frontend code to identify the account user. |
| PCI Compliance | A set of mandatory security standards designed to ensure all companies that accept, process, store, or transmit credit card information maintain a secure environment, protecting against data breaches and fraud. |
| Redirect-based Payment Method | A checkout event in which the customer's browser is sent to a third-party site (bank or processor) to authorize transactions and returned to the merchant site upon completion (e.g., bank transfers, PayPal, Klarna). |
| Secret Key | A private, confidential API key used in backend code to identify the account user. This key should be refreshed regularly and never shared or exposed in public. |
| Test Mode | The sandbox environment where simulated transactions occur. Live data is inaccessible in test mode.| 
| Webhook | A lightweight, event-driven HTTP callback that allows one application to send real-time data to another automatically when a specific event occurs. |








