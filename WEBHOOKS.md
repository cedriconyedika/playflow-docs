# Webhooks Guide
**PlayFlow API**

---

## Overview

Webhooks allow PlayFlow to notify your application in real-time when important events occur, such as successful payments or refunds.

Instead of constantly polling the API, your system can listen for updates automatically.

---

## How Webhooks Work

1. An event occurs (e.g. payment is successful)
2. PlayFlow sends an HTTP POST request to your webhook URL
3. Your server receives and processes the event

---

## Setting Up Webhooks

### Step 1: Create a Webhook Endpoint

You need a publicly accessible URL that can receive POST requests.

Example:

```
https://yourapp.com/webhooks/playflow
```

---

### Step 2: Register Your Webhook URL

In your PlayFlow dashboard:

- Go to **Settings → Webhooks**
- Add your webhook URL
- Save changes

---

### Step 3: Handle Incoming Requests

Your server must:
- Accept POST requests
- Parse JSON payloads
- Return a `200 OK` response quickly

---

## Webhook Events

PlayFlow sends webhooks for the following events:

| Event Name            | Description                          |
|----------------------|--------------------------------------|
| payment.success      | Payment completed successfully       |
| payment.failed       | Payment failed                       |
| payment.pending      | Payment initiated but not completed  |
| refund.processed     | Refund completed                     |

---

## Webhook Payload Example

### payment.success

```json
{
  "event": "payment.success",
  "data": {
    "payment_id": "pay_987654",
    "amount": 5000,
    "currency": "NGN",
    "status": "successful",
    "reference": "txn_123456",
    "customer": {
      "email": "customer@example.com"
    },
    "paid_at": "2026-04-26T10:05:00Z"
  }
}
```

---

## Verifying Webhook Signatures

To ensure requests are coming from PlayFlow, verify the webhook signature.

### Header

```
x-playflow-signature: <signature>
```

---

### How It Works

- PlayFlow signs each webhook payload using your secret key
- You must compute a hash and compare it with the signature

---

### Example (Node.js)

```javascript
const crypto = require("crypto");

function verifySignature(payload, signature, secret) {
  const hash = crypto
    .createHmac("sha256", secret)
    .update(payload)
    .digest("hex");

  return hash === signature;
}
```

---

## Example Webhook Handler (Node.js)

```javascript
const express = require("express");
const app = express();

app.use(express.json());

app.post("/webhooks/playflow", (req, res) => {
  const event = req.body;

  switch (event.event) {
    case "payment.success":
      console.log("Payment successful:", event.data);
      break;

    case "payment.failed":
      console.log("Payment failed:", event.data);
      break;

    case "refund.processed":
      console.log("Refund processed:", event.data);
      break;

    default:
      console.log("Unhandled event:", event.event);
  }

  res.sendStatus(200);
});

app.listen(3000, () => {
  console.log("Webhook server running on port 3000");
});
```

---

## Retry Behavior

If your server does not respond with a `200 OK`, PlayFlow will retry the webhook.

### Retry Policy

- Retries up to 5 times  
- Exponential backoff between retries  

---

## Best Practices

- Respond with `200 OK` quickly (under 5 seconds)
- Process webhook events asynchronously
- Store event data for auditing
- Use idempotency (avoid processing the same event twice)
- Always verify webhook signatures

---

## Common Issues

---

### Webhook Not Receiving Events

- Ensure your endpoint is publicly accessible  
- Check firewall or server restrictions  
- Verify correct webhook URL  

---

### Duplicate Events

Webhooks may be sent more than once.

**Solution:**
- Use the `payment_id` or `reference` to detect duplicates  
- Ignore already processed events  

---

### Invalid Signature

- Ensure you are using the correct secret key  
- Verify the raw request body (not modified JSON)  

---

## Testing Webhooks

You can test webhooks by:

- Using tools like ngrok to expose your local server  
- Triggering test payments from the dashboard  

---

## Security Notes

- Never expose your webhook secret  
- Always verify signatures before processing events  
- Use HTTPS for your webhook endpoint  

---

## Support

```
support@playflow.com
```

---
