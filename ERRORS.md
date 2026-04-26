# Error Handling Guide
**PlayFlow API**

---

## Overview

Errors are a normal part of working with any API. PlayFlow provides structured and predictable error responses to help you quickly identify and resolve issues.

Every error response includes:
- A status (`error`)
- A message (human-readable summary)
- An error object (detailed information)

---

## Error Response Format

All errors follow this structure:

```json
{
  "status": "error",
  "message": "Short description of the error",
  "error": {
    "code": 400,
    "type": "invalid_request",
    "details": "Amount must be greater than 0"
  }
}
```

### Fields Explained

| Field   | Type   | Description |
|--------|--------|-------------|
| status | string | Always `"error"` for failed requests |
| message | string | High-level explanation |
| error.code | number | HTTP status code |
| error.type | string | Error category |
| error.details | string | Specific issue description |

---

## Error Categories

---

### 1. Authentication Errors

Occurs when there is an issue with your API key or authorization.

#### Example

```json
{
  "status": "error",
  "message": "Unauthorized",
  "error": {
    "code": 401,
    "type": "authentication_error",
    "details": "Invalid or missing API key"
  }
}
```

#### Causes

- Missing `Authorization` header  
- Incorrect API key  
- Expired or revoked key  

#### How to Fix

- Ensure you include:
  ```
  Authorization: Bearer YOUR_API_KEY
  ```
- Double-check your API key
- Regenerate your key if needed

---

### 2. Validation Errors

Occurs when request data is incorrect or incomplete.

#### Example

```json
{
  "status": "error",
  "message": "Invalid request",
  "error": {
    "code": 422,
    "type": "validation_error",
    "details": "Currency must be NGN, USD, or EUR"
  }
}
```

#### Common Causes

- Missing required fields  
- Invalid data types  
- Unsupported currency  
- Negative or zero amount  

#### How to Fix

- Validate input before sending requests  
- Follow API request schema exactly  
- Use proper data formats  

---

### 3. Resource Errors

Occurs when a requested resource does not exist.

#### Example

```json
{
  "status": "error",
  "message": "Not Found",
  "error": {
    "code": 404,
    "type": "resource_error",
    "details": "Payment ID not found"
  }
}
```

#### Causes

- Incorrect `payment_id`  
- Resource has been deleted  
- Typo in endpoint  

#### How to Fix

- Verify the resource ID  
- Ensure the resource exists  
- Double-check endpoint URLs  

---

### 4. Conflict Errors

Occurs when a request conflicts with existing data.

#### Example

```json
{
  "status": "error",
  "message": "Conflict",
  "error": {
    "code": 409,
    "type": "conflict_error",
    "details": "Duplicate transaction reference"
  }
}
```

#### Causes

- Reusing a transaction reference  
- Duplicate request submission  

#### How to Fix

- Use unique references (e.g. timestamp-based)  
- Avoid retrying the same request without changes  

---

### 5. Rate Limit Errors

Occurs when you exceed allowed request limits.

#### Example

```json
{
  "status": "error",
  "message": "Rate limit exceeded",
  "error": {
    "code": 429,
    "type": "rate_limit_error",
    "details": "Too many requests. Try again later."
  }
}
```

#### How to Fix

- Reduce request frequency  
- Implement retry logic with delays  
- Use exponential backoff  

---

### 6. Server Errors

Occurs when something goes wrong on PlayFlow’s servers.

#### Example

```json
{
  "status": "error",
  "message": "Internal server error",
  "error": {
    "code": 500,
    "type": "server_error",
    "details": "An unexpected error occurred"
  }
}
```

#### What to Do

- Retry the request after a short delay  
- Log the error for debugging  
- Contact support if the issue persists  

---

## Retry Strategy (Best Practice)

Not all errors should be retried.

### Safe to Retry

- 500 (Server errors)  
- 503 (Service unavailable)  
- 429 (Rate limit exceeded)  

### Do NOT Retry

- 400 (Bad request)  
- 401 (Unauthorized)  
- 422 (Validation errors)  

---

## Example: Retry with Exponential Backoff (Node.js)

```javascript
async function retryRequest(fn, retries = 3, delay = 1000) {
  try {
    return await fn();
  } catch (error) {
    if (retries === 0) throw error;

    console.log(`Retrying in ${delay}ms...`);
    await new Promise((res) => setTimeout(res, delay));

    return retryRequest(fn, retries - 1, delay * 2);
  }
}
```

---

## Debugging Checklist

Before contacting support, check:

- API key is correct and active  
- Request body matches API schema  
- Endpoint URL is correct  
- Network connection is stable  
- You are within rate limits  

---

## Logging Recommendations

Always log:
- Request payload  
- Response body  
- Error message and code  

This helps you debug issues faster and track failures in production.

---

## Support

If you continue to experience issues:

```
support@playflow.com
```

---
