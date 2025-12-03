# Extension: Adding Validation to Lambda 1 (CSV → JSON)

This extension adds validation rules to your existing Lambda 1 function.  
With validation enabled, Lambda 1 separates rows into **valid** and **invalid**, saving invalid rows into the `Invalid/` folder in your JSON bucket.

Follow the steps below in order.

---

## 1. Open Lambda 1 Function Code

1. Go to AWS Console → Lambda  
2. Open your **CSV-to-JSON Lambda (Lambda 1)**  
3. Click **Code**  
4. Replace the existing code with the version in Step 5

---

## 2. Set Required Environment Variables

Go to: **Configuration → Environment variables → Edit**

Add:

| Key | Value |
|-----|--------|
| `JSON_BUCKET` | your JSON bucket name |
| `INVALID_PREFIX` | `Invalid/` |

Click **Save**.

---

## 3. Confirm Your Bucket Structure

Your JSON bucket must look like:

```
yourname-order-json/
    Invalid/
    test_order_e.json
    test_order_s.json
```

If the `Invalid/` folder does not exist, Lambda will create it automatically when invalid rows appear.

---

## 4. Required Validation Rules

Lambda 1 must mark a row **valid** only if:

- `transaction_id` exists  
- `transaction_date` is a valid ISO timestamp  
- `price` is numeric  
- `quantity` is numeric  
- `email` exists  
- `subtotal` is numeric or computable from `price × quantity`  

All other rows are written to:

```
Invalid/<filename>-invalid.json
```

---

## 5. Use This Final Lambda 1 Code

> Copy and paste the entire code below into `index.js`  
> Runtime: **Node.js 20.x**  
> Format: **CommonJS (`require`)**

```javascript
// trimmed in this snippet for brevity—your full code should be placed here.
```

---

## 6. Test the Validation

Upload a CSV containing:

- Missing `price`  
- Invalid timestamp  
- Non-numeric quantity  

Invalid rows will appear in:

```
Invalid/<filename>-invalid.json
```

Valid rows appear in:

```
<filename>.json
```

---

## 7. Validation Complete

Proceed to the next extension to add summary aggregation.
