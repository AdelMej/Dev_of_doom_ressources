# Stripe Connect -- Python Cheat Sheet

This document is a **practical, no-bullshit Stripe Connect cheat sheet**
for backend developers. Focus: **Stripe Connect (Express)**, Python
backend, webhooks, and payouts.

------------------------------------------------------------------------

## Mental Model (READ THIS FIRST)

**You create Stripe accounts.** Users do NOT bring their own Stripe
accounts.

Flow:

1.  Backend creates Stripe account
2.  User completes Stripe onboarding
3.  Stripe updates account state
4.  Webhooks notify your backend
5.  Your app decides if payouts are allowed

Webhooks are **notifications**, not discovery.

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

## Create a Stripe Connect Account

You MUST do this server-side.

``` python
acct = stripe.Account.create(
    type="express",
    country="FR",
    email=user.email
)

stripe_account_id = acct.id  # acct_*
```

Store `stripe_account_id` immediately in your DB.

------------------------------------------------------------------------

## Create Onboarding Link

This is what the user clicks.

``` python
link = stripe.AccountLink.create(
    account=stripe_account_id,
    type="account_onboarding",
    refresh_url="https://yourapp.com/stripe/refresh",
    return_url="https://yourapp.com/stripe/return"
)

onboarding_url = link.url
```

------------------------------------------------------------------------

## Important Account Fields (Trust THESE)

From `account.updated` webhook:

``` json
charges_enabled
payouts_enabled
requirements.currently_due
```

**Rule of thumb:** - `payouts_enabled == true` → coach can receive
money - Anything else → NO payouts

------------------------------------------------------------------------

## Stripe Webhooks (MANDATORY)

### Verify Webhook Signature

``` python
payload = request.data
sig_header = request.headers["Stripe-Signature"]

event = stripe.Webhook.construct_event(
    payload,
    sig_header,
    endpoint_secret
)
```

Never skip verification.

------------------------------------------------------------------------

## Handle account.updated Webhook

``` python
if event["type"] == "account.updated":
    account = event["data"]["object"]

    stripe_account_id = account["id"]
    charges_enabled = account["charges_enabled"]
    payouts_enabled = account["payouts_enabled"]

    update_stripe_status(
        stripe_account_id,
        charges_enabled,
        payouts_enabled
    )
```

Update your DB by `stripe_account_id`.

------------------------------------------------------------------------

## Minimal Database Model

``` sql
CREATE TABLE user_stripe_connect (
    user_id UUID PRIMARY KEY,
    stripe_account_id TEXT UNIQUE NOT NULL,
    charges_enabled BOOLEAN NOT NULL,
    payouts_enabled BOOLEAN NOT NULL,
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now()
);
```

------------------------------------------------------------------------

## Can a Coach Receive Payment?

``` python
def can_receive_payment(user_id):
    row = repo.get_stripe_status(user_id)

    return (
        row is not None
        and row.payouts_enabled is True
    )
```

Simple. No Stripe logic in domain code.

------------------------------------------------------------------------

## Create a Payment (Destination Charge)

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

------------------------------------------------------------------------

## Common Failure States

  Situation                              Meaning
  -------------------------------------- ------------------------
  payouts_enabled = false                KYC not completed
  requirements.currently_due not empty   Missing documents
  account.disabled_reason                Stripe blocked account

------------------------------------------------------------------------

## DO NOT DO THESE

❌ Discover accounts via webhooks\
❌ Let frontend create Stripe accounts\
❌ Store Stripe logic in domain code\
❌ Assume onboarding == payouts enabled

------------------------------------------------------------------------

## Express vs Standard (Short)

  Type       When
  ---------- -----------------------------------
  Express    You want simplicity (recommended)
  Standard   User manages Stripe dashboard
  Custom     Pain and compliance hell

Use **Express** unless you know why not to.

------------------------------------------------------------------------

## Stripe Truths (Memorize)

-   Stripe is authoritative
-   You own account creation
-   Webhooks are eventual
-   Capability flags are king
-   Audit decisions, not payloads

------------------------------------------------------------------------

## Minimal Required Webhooks

Enable ONLY:

-   `account.updated`
-   `capability.updated` (optional)

Less is more.

------------------------------------------------------------------------

## Final Advice

If Stripe feels confusing: - you're probably trying to be clever -
stop - store account id - trust `payouts_enabled`

Stripe works when you keep it boring.

------------------------------------------------------------------------

**End of cheat sheet**
