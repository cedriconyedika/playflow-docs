# Frequently Asked Questions (FAQ)
**PlayFlow**

---

## General

### What is PlayFlow?

PlayFlow is a payment processing platform that allows businesses to accept payments, track transactions, and manage refunds from a single dashboard or via API.

---

### Who can use PlayFlow?

PlayFlow is designed for:
- Small businesses
- Freelancers
- Startups
- Developers building payment-enabled applications

---

### Do I need technical skills to use PlayFlow?

No. You can create payment links and manage transactions directly from the dashboard without writing any code.

However, developers can integrate PlayFlow using the API for more advanced use cases.

---

## Account & Setup

### How do I create an account?

- Go to the PlayFlow website
- Click **"Create Account"**
- Enter your details and verify your email

---

### I didn’t receive my verification email

- Check your spam/junk folder  
- Wait a few minutes  
- Click **"Resend Email"** on the login page  

---

### Can I change my email or business name?

Yes. Go to **Settings** in your dashboard and update your account details.

---

## Payments

### How do I accept payments?

You can:
- Create a payment link from your dashboard  
- Share it with your customers  
- Get paid when they complete the payment  

---

### What payment methods are supported?

PlayFlow supports:
- Debit and credit cards  
- Bank transfers (where available)  

---

### How long does it take to receive payments?

Payments are processed instantly, but settlement time to your bank account may vary depending on your provider.

---

### Why is my payment showing as "Pending"?

A payment may be pending if:
- The customer has not completed the payment  
- There is a temporary delay in processing  

**What to do:**
- Wait a few minutes  
- Ask the customer to confirm payment  

---

### Why did a payment fail?

Common reasons:
- Insufficient funds  
- Card declined  
- Network issues  

**What to do:**
- Ask the customer to try again  
- Suggest a different payment method  

---

## Transactions

### How do I check my transactions?

- Log in to your dashboard  
- Click **"Transactions"**  
- View, search, or filter payments  

---

### Can I download transaction history?

Yes. You can export your transactions from the dashboard (if enabled).

---

## Refunds

### How do I issue a refund?

- Go to **Transactions**  
- Select the payment  
- Click **"Refund"**  

---

### How long do refunds take?

Refunds are processed immediately on PlayFlow, but it may take a few business days for the customer to receive the funds depending on their bank.

---

### Can I issue partial refunds?

Yes, you can refund either the full amount or a portion of the payment.

---

## API & Developers

### Where can I find my API key?

- Go to **Settings → API Keys** in your dashboard  

---

### Why am I getting a 401 Unauthorized error?

This usually means:
- Your API key is missing or incorrect  
- You didn’t include the `Authorization` header  

---

### Why am I getting a 422 validation error?

Your request data is invalid.

Check:
- Required fields are included  
- Data types are correct  
- Amount is greater than 0  

---

### How do I verify a payment?

Use the **Verify Payment** endpoint with the `payment_id` returned when you created the payment.

---

## Security

### Is PlayFlow secure?

Yes. PlayFlow uses industry-standard security practices to protect transactions and user data.

---

### Should I share my API key?

No. Never share your API key publicly or expose it in frontend code.

---

## Troubleshooting

### The payment link is not working

- Ensure the link is correct  
- Check if the payment has already been completed  
- Try opening it in a different browser  

---

### I’m not seeing updated transactions

- Refresh your dashboard  
- Check your internet connection  
- Wait a few moments for real-time updates  

---

### What should I do if something isn’t working?

1. Check the error message  
2. Review the documentation  
3. Try again  
4. Contact support if the issue persists  

---

## Support

### How do I contact support?

```
support@playflow.com
```

---

## Final Tip

If you’re stuck, don’t guess—check the error message first. Most issues can be resolved quickly when you focus on the exact problem.

---
