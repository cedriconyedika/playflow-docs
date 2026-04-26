# Quick Start Guide
**Integrate PlayFlow API in Node.js (5 Minutes)**

---

## Prerequisites

Before you begin, make sure you have:

- Node.js (v16 or higher)
- npm or yarn installed
- A PlayFlow API key  
- Basic knowledge of JavaScript

---

## Installation

Create a new Node.js project:

```bash
mkdir playflow-demo
cd playflow-demo
npm init -y
```

Install required dependency:

```bash
npm install axios
```

---

## Authentication Setup

PlayFlow uses Bearer Token authentication.

Create a `.env` file in your project root:

```
PLAYFLOW_API_KEY=sk_test_123456789
```

Install dotenv to load environment variables:

```bash
npm install dotenv
```

---

## First API Request

We’ll create a simple script to initialize a payment.

Create a file called `index.js`:

```javascript
require("dotenv").config();
const axios = require("axios");

const API_URL = "https://api.playflow.com/v1";

async function createPayment() {
  try {
    const response = await axios.post(
      `${API_URL}/payments`,
      {
        amount: 5000,
        currency: "NGN",
        customer: {
          email: "customer@example.com",
          name: "John Doe",
        },
        reference: "txn_" + Date.now(),
        callback_url: "https://yourapp.com/callback",
      },
      {
        headers: {
          Authorization: `Bearer ${process.env.PLAYFLOW_API_KEY}`,
          "Content-Type": "application/json",
        },
      }
    );

    console.log("Payment Created:");
    console.log(response.data);
  } catch (error) {
    console.error("Error creating payment:");
    console.error(
      error.response ? error.response.data : error.message
    );
  }
}

createPayment();
```

---

## Run the Script

Execute the script using:

```bash
node index.js
```

---

## Expected Output

If successful, you should see a response like this:

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

## What’s Next?

- Open the `payment_url` in your browser to complete the payment
- Use the **Verify Payment** endpoint to confirm status
- Store transaction details in your database

---

## Common Mistakes

-  Missing or incorrect API key  
-  Not loading `.env` properly  
-  Invalid request body (e.g. negative amount)  
-  Forgetting to handle errors  

---

## Need Help?

```
support@playflow.com
```

---
