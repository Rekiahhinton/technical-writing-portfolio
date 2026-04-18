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

2. Stripe receives and acknowledges the request before returning a ``client_secret`` key.
  
3. The business website uses Stripe.js - Stripe's frontend JavaScript library - to collect the card credentials directly in the browser.
   
4. The credentials go to Stripe. The business server never sees them for security purposes.
   
5. Stripe confirms the payment and charges the card the value recorded in the payment intent.

6. The business server is notified about the payment success via a webhook, an automated HTTP request 
    Stripe sends to the business server when a payment event occurs.   

**Note:** The ``client_secret`` key is a randomized token assigned to each transaction that the business server receives upon checkout. The Stripe API also includes an idempotency key that prevents a ``client_secret`` key from being used in multiple transactions if a network error occurs.

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

| Name | Type | Required | Description |
| ---- | ---- | ---- | ---- |
| ``id`` | string | Required | The identifier of the PaymentIntent specific to each PaymentIntent. |
| ``object`` | string | Required |
| ``amount`` | integer | Required |


















#### 2. GET v1/payment_intents/{id} - Retrieve a Payment Intent

##### Description

Returns the ``client_secret`` key of a PaymentIntent object. 

##### Parameters Table

| Name | Type | Required | Description |
|---- | ---- | ---- | ---- |
| ``client_secret`` | string | Required | The |

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

| Name | Type | Required | Description |
| ---- | ---- | ---- | ---- |
| id | string | Required | The identifier of the PaymentIntent specific to each PaymentIntent. |
| object | string | Required |
| amount | integer | Required |

#### 3. GET v1/payment_intents - List All Payment Intents

##### Description 

Returns a list of most recent PaymentIntents up to a default of 10.

##### Parameters Table

| Parameter | Type | Required | Description |
| ---- | ---- | ---- | ---- |
| ``limit`` | integer | Optional | Sets a maximum number of PaymentIntents the call will return. |
| ``customer`` | string | Optional | ID of the customer that has PaymentIntents. |
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

| Name | Type | Required | Description |
| ---- | ---- | ---- | ---- |
| ``id`` | string | Required | The identifier of the PaymentIntent specific to each PaymentIntent. |
| ``object`` | string | Required | The type of object Stripe returned. For this endpoint, the value is always ''payment intent''. |
| ``amount`` | integer | Required | The value the PaymentIntent expects to collect for the transaction, represented by a positive integer in the smallest currency unit. e.g., ``1000`` cents to charge $10.00 usd. The value is ``0`` when creating the PaymentIntent. |
| ``amount_received`` | integer | Required | Monetary value the PaymentIntent successfully collects from the customer's payment method. |
| ``client_secret`` | string | Required | A unique token attached to this PaymentIntent for the purpose of obscuring sensitive financial details. Each PaymentIntent receives only one token. Never expose or log this value. |
| ``currency`` | string | Required | The [ISO currency code](https://www.iso.org/iso-4217-currency-codes.html), represented by three lowercase letters. |
| ``description`` | string | Optional | A string of text attached to the PaymentIntent, potentially to display to users. |
| ``payment_method_types`` | array of strings | Required | An array of payment method types eligible to complete this PaymentIntent, such as ``card``, ``klarna``, or ``affirm``. |
| ``status`` | string | Required | The current state of the PaymentIntent. Common values include ``requires_payment_method``, ``requires_confirmation``, ``processing``, and ``succeeded``. |

 
#### 4. POST v1/payment_intents/{id}/confirm - Confirm a Payment Intent 

##### Description 

Indicates that the customer intends to pay with the current or provided payment method. A successful confirmation initiates a charge to the payment method. 

##### Parameters Table 

| Name | Type | Required | Description |
| ---- | ---- | ---- | ---- |
| ``payment_method`` | string | Required | The ID of the payment method attached to this confirmation. |
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

| Name | Type | Required | Description |
| ---- | ---- | ---- | ---- |
| ``id`` | 
| ``object`` |
| ``amount`` |


#### 5. POST v1/payment_intents/{id}/cancel - Cancel a Payment Intent

##### Description

Indicates that the PaymentIntent is no longer functional and cannot be applied to any additional charges. Any operations made on the PaymentIntent fail and return an error. 

The PaymentIntent can only be cancelled in one of the following states: ``requires_payment_method``, ``requires_capture``, ``requires_confirmation``, ``requires_action``, or rarely ``processing``.

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

| Name | Type | Required | Description |
| ---- | ---- | ---- | ---- |



## Payment Intent Statuses

## Error Handling 

### Overview

Errors happen, which is why we must know how to handle them. This section lists possible error scenarios for each endpoint and offers some useful tips for successfully solving them according to error type.

| Error Type | Description |
| ---- | ---- | 
| ``api_error`` | Indicates disruptions with the API itself (e.g. Stripe's servers are down.) This is unlikely to occur. |
| ``card_error`` |  | Indicates a problem with the payment method. This is the error that is most likely to occur. |
| ``idempotency error`` | the |
| ``invalid_request_error`` | the |

### Create a Payment Intent

### Retrieve a Payment Intent

### List All Payment Intents

### Confirm a Payment Intent

### Cancel a Payment Intent

## Card Decline Codes

## Getting Started - Your First Payment Intent

## Testing 

## Glossary 

| Term | Definition |
| ---- | ---- |
| 3D Secure | A security protocol that adds an extra authentication step for online card payments, reducing fraud by verifying the cardholder's identity directly with the issuer. |
| Bearer Token | An opaque string, such as a JSON Web Token (JWT), that acts as a digital key granting access to protected API resources to whoever holds it. Generally used with OAuth 2.0. |
| Charge | The withdrawal transaction applied to the payment method for the PaymentIntent. |
| ``client_secret`` | A unique key generated with each PaymentIntent. Used to complete a payment from the frontend of a browser without exposing sensitive financial details.| 
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








