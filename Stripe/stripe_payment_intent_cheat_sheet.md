# Stripe Payment Intents -- Python Cheat Sheet

This document is a **practical Stripe Payment Intents cheat sheet** for
backend developers. Focus: **Payments API**, server-side control, and
clean domain separation.

------------------------------------------------------------------------

## Mental Model (READ FIRST)

**PaymentIntent = payment state machine owned by Stripe**

You: - create it - confirm it - react to its state

You do NOT: - calculate final state yourself - trust frontend signals -
skip webhooks

------------------------------------------------------------------------

## Install & Setup

``` bash
pip install stripe
```

``` python
import stripe

stripe.api_key = "sk_test_..."
```

------------------------------------------------------------------------

## What is a PaymentIntent?

A PaymentIntent represents: - **how much** - **in which currency** -
**who gets paid** - **what the current payment state is**

It survives retries, SCA, failures, and network issues.

------------------------------------------------------------------------

## Create a PaymentIntent (Server-Side)

``` python
intent = stripe.PaymentIntent.create(
    amount=5000,  # cents
    currency="eur",
    payment_method_types=["card"]
)
```

Return **only**:

``` python
intent.client_secret
```

to the frontend.

------------------------------------------------------------------------

## PaymentIntent States (Important)

  Status                    Meaning
  ------------------------- --------------------
  requires_payment_method   waiting for card
  requires_confirmation     ready to confirm
  requires_action           3DS / SCA required
  processing                payment in flight
  succeeded                 money captured
  canceled                  payment aborted

Trust these. Nothing else.

------------------------------------------------------------------------

## Confirm a PaymentIntent (Frontend)

Frontend confirms using `client_secret`.

Backend never handles card details.

------------------------------------------------------------------------

## Destination Charge (Marketplace / Coach Payment)

Paying a connected account:

``` python
stripe.PaymentIntent.create(
    amount=5000,
    currency="eur",
    payment_method_types=["card"],
    transfer_data={
        "destination": stripe_account_id
    }
)
```

Optional platform fee:

``` python
application_fee_amount=500
```

------------------------------------------------------------------------

## IMPORTANT: Check Payout Capability FIRST

``` python
if not coach_can_receive_payment(user_id):
    raise PaymentForbidden()
```

Never rely on Stripe failure to enforce business rules.

------------------------------------------------------------------------

## Webhooks You MUST Handle

Enable: - `payment_intent.succeeded` - `payment_intent.payment_failed`

------------------------------------------------------------------------

## Verify Webhook Signature

``` python
event = stripe.Webhook.construct_event(
    payload,
    sig_header,
    endpoint_secret
)
```

Never skip this.

------------------------------------------------------------------------

## Handle payment_intent.succeeded

``` python
if event["type"] == "payment_intent.succeeded":
    intent = event["data"]["object"]

    amount = intent["amount"]
    intent_id = intent["id"]

    mark_payment_as_paid(intent_id, amount)
```

------------------------------------------------------------------------

## Handle payment_intent.payment_failed

``` python
if event["type"] == "payment_intent.payment_failed":
    intent = event["data"]["object"]

    error = intent["last_payment_error"]
    mark_payment_as_failed(intent["id"], error)
```

------------------------------------------------------------------------

## Idempotency (CRITICAL)

Stripe retries webhooks.

Your DB logic MUST be idempotent.

Example: - unique constraint on `payment_intent_id` - ignore duplicates

------------------------------------------------------------------------

## Minimal Payment Table

``` sql
CREATE TABLE app.payments (
    id UUID PRIMARY KEY,
    stripe_payment_intent_id TEXT UNIQUE NOT NULL,
    amount INTEGER NOT NULL,
    currency TEXT NOT NULL,
    status TEXT NOT NULL,
    created_at TIMESTAMPTZ NOT NULL DEFAULT now()
);
```

------------------------------------------------------------------------

## DO NOT DO THESE

❌ Create charges directly\
❌ Trust frontend success callbacks\
❌ Skip webhooks\
❌ Mix Stripe logic into domain rules

------------------------------------------------------------------------

## PaymentIntent vs Charge (REMEMBER)

-   **Charge** = Stripe internal
-   **PaymentIntent** = what you control

Always use PaymentIntents.

------------------------------------------------------------------------

## Common Errors & Meaning

  Error                     Meaning
  ------------------------- --------------------
  card_declined             User card rejected
  authentication_required   3DS required
  insufficient_funds        Bank refused

------------------------------------------------------------------------

## Refund a Payment

``` python
stripe.Refund.create(
    payment_intent=payment_intent_id
)
```

Refunds are separate state transitions.

------------------------------------------------------------------------

## Stripe Truths

-   Stripe owns payment state
-   Webhooks are eventual
-   Your DB must be idempotent
-   Frontend is untrusted
-   Backend is king

------------------------------------------------------------------------

## Final Advice

If payments feel complex: - you're probably skipping state - or trusting
the frontend - or not using webhooks

PaymentIntents solve this. Use them.

------------------------------------------------------------------------

**End of cheat sheet**
