---
title: Documenting Stripe Payment Intents API
---

### Conceptual Overview

Stripe is not just a payment API interface. It is a trust infrastructure that ensures both businesses and customers can interact with each other safely. The Payments Intent API perfectly demonstrates this idea. 

By creating a ``client_secret`` key when the business collects payment, the card credentials flow directly to Stripe allowing the customer to feel secure knowing the business server will never have access to their sensitive information. This eliminates the risk of fraud for the customer and relieves the business of maintaining PCI compliance requirements, diminishing the legal liability that comes with storing sensitive financial data. 

For customers, this security is transformative at the most critical point of contact - checkout. This reduces friction that may deter them from purchasing an item or interacting with the business. The added protection removes a trust barrier that could discourage customers, especially if the business is new and lacks an established reputation.

The Payment Intents API sits at the core of the business value of Stripe - creating trust and peace of mind by providing the means for businesses and customers to interact on digital platforms without apprehension.

## How Payment Intents Works

## Base URL

``POST https://api.stripe.com/v1/payment_intents``

## Authentication 

## Endpoint Reference

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
