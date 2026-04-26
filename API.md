# PlayFlow API Documentation
**Payment Processing API for Small Businesses**

---

## Overview

PlayFlow API enables small businesses to securely process payments, verify transactions, and issue refunds. It is designed to be simple, fast, and developer-friendly.

Key features:
- Secure payment processing
- Real-time transaction verification
- Easy refund handling
- Scalable for growing businesses

---

## Base URL

```
https://api.playflow.com/v1
```

---

## Authentication

PlayFlow uses API keys and Bearer tokens for authentication.

### API Key
You will receive an API key upon registering your application.

### Token Authentication
All requests must include a Bearer token in the headers:

```
Authorization: Bearer YOUR_SECRET_API_KEY
```

### Example Header

```http
GET /payments HTTP/1.1
Host: api.playflow.com
Authorization: Bearer sk_test_123456789
Content-Type: application/json
```

---

## Endpoints

---

### 1. Create Payment

Create a new payment transaction.

**Endpoint**
```
POST /payments
```

**Request Body**

```json
{
  "amount": 5000,
  "currency": "NGN",
  "customer": {
    "email": "customer@example.com",
    "name": "John Doe"
  },
  "reference": "txn_123456",
  "callback_url": "https://yourapp.com/callback"
}
```

**Response**

```json
{
  "status": "success",
  "message": "Payment created",
  "data": {
    "payment_id": "pay_987654",
    "amount": 5000,
    "currency": "NGN",
    "status": "pending",
    "payment_url": "https://pay.playflow.com/pay_987654",
    "created_at": "2026-04-26T10:00:00Z"
  }
}
```

---

### 2. Verify Payment

Verify the status of a payment.

**Endpoint**
```
GET /payments/{payment_id}/verify
```

**Path Parameter**
- `payment_id` (string) — Unique payment identifier

**Response**

```json
{
  "status": "success",
  "message": "Payment verified",
  "data": {
    "payment_id": "pay_987654",
    "amount": 5000,
    "currency": "NGN",
    "status": "successful",
    "paid_at": "2026-04-26T10:05:00Z",
    "customer": {
      "email": "customer@example.com"
    }
  }
}
```

---

### 3. Refund Payment

Issue a refund for a completed transaction.

**Endpoint**
```
POST /payments/{payment_id}/refund
```

**Request Body**

```json
{
  "amount": 5000,
  "reason": "Customer request"
}
```

**Response**

```json
{
  "status": "success",
  "message": "Refund processed",
  "data": {
    "refund_id": "ref_456789",
    "payment_id": "pay_987654",
    "amount": 5000,
    "status": "processed",
    "processed_at": "2026-04-26T11:00:00Z"
  }
}
```

---

## Error Codes

| Code | Message                     | Description                          |
|------|----------------------------|--------------------------------------|
| 400  | Bad Request                | Invalid request parameters           |
| 401  | Unauthorized              | Missing or invalid API key           |
| 403  | Forbidden                 | Access denied                        |
| 404  | Not Found                 | Resource not found                   |
| 409  | Conflict                  | Duplicate transaction reference      |
| 422  | Unprocessable Entity      | Validation error                     |
| 500  | Internal Server Error     | Something went wrong on the server   |

### Error Response Example

```json
{
  "status": "error",
  "message": "Invalid request",
  "error": {
    "code": 400,
    "details": "Amount must be greater than 0"
  }
}
```

---

## Rate Limits

To ensure fair usage, PlayFlow enforces rate limits on API requests.

- **Standard Plan:** 100 requests per minute
- **Burst Limit:** 120 requests per minute
- **Daily Limit:** 100,000 requests

### Rate Limit Headers

```http
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 85
X-RateLimit-Reset: 1714137600
```

### Rate Limit Exceeded Response

```json
{
  "status": "error",
  "message": "Rate limit exceeded",
  "error": {
    "code": 429,
    "details": "Too many requests. Try again later."
  }
}
```

---

## Best Practices

- Always store API keys securely
- Use unique transaction references
- Implement webhook handling for real-time updates
- Retry failed requests with exponential backoff

---

## Support

For integration support, contact:

```
support@playflow.com
```

---

## Versioning

Current API version: `v1`

Future updates will be versioned to ensure backward compatibility.

---
