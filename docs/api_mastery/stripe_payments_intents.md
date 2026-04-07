---
title: Documenting Stripe Payment Intents API
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

Creates a PaymentIntent object, which initiates a ``client_secret`` key for the server to receive instead of sensitive financial details. 

```
RESPONSE:
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



















#### 2. GET v1/payment_intents/{id} - Retrieve a Payment Intent

#### 3. GET v1/payment_intents - List All Payment Intents

#### 4. POST v1/payment_intents/{id}/confirm - Confirm a Payment Intent 

#### 5. POST v1/payment_intents/{id}/cancel - Cancel a Payment Intent



## Payment Intent Statuses

## Error Handling 
### Create a Payment Intent

### Retrieve a Payment Intent

### List All Payment Intents

### Confirm a Payment Intent

### Cancel a Payment Intent

## Card Decline Codes

## Getting Started - Your First Payment Intent

## Testing 

## Glossary 
