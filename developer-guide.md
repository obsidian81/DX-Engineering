# 🚀 Developer Integration Guide & SDK Quickstart

Welcome to the Developer Integration Guide. This resource provides step-by-step instructions, runnable code samples, and production resilience patterns to help you integrate our platform into your applications seamlessly.

---

## 🔑 1. Authentication & Security

Our APIs use **OAuth 2.0 Bearer Tokens** to authenticate requests. All API calls must be made over HTTPS and include your secret API key in the `Authorization` request header:

```http
Authorization: Bearer YOUR_API_KEY
```bash
npm install axios dotenv
```

```javascript
import axios from 'axios';
import dotenv from 'dotenv';

dotenv.config();

const API_KEY = process.env.API_KEY;
const BASE_URL = '[https://api.example.com/v1](https://api.example.com/v1)';

const client = axios.create({
  baseURL: BASE_URL,
  headers: {
    'Authorization': `Bearer ${API_KEY}`,
    'Content-Type': 'application/json',
  },
});

async function getProfileAndCreateOrder() {
  try {
    // 1. Fetch user profile
    const userResponse = await client.get('/users/usr_987654321');
    console.log('User Profile:', userResponse.data);

    // 2. Create a new order with an Idempotency Key
    const orderResponse = await client.post('/orders', {
      customerId: userResponse.data.id,
      amount: 4999,
      currency: 'USD'
    }, {
      headers: {
        'Idempotency-Key': `ik_${Date.now()}`
      }
    });

    console.log('Order Created:', orderResponse.data);
  } catch (error) {
    console.error('API Error:', error.response?.data || error.message);
  }
}

getProfileAndCreateOrder();
```

```bash
pip install requests python-dotenv
```

```python
import os
import time
import requests
from dotenv import load_dotenv

load_dotenv()

API_KEY = os.getenv("API_KEY")
BASE_URL = "[https://api.example.com/v1](https://api.example.com/v1)"

headers = {
    "Authorization": f"Bearer {API_KEY}",
    "Content-Type": "application/json"
}

def create_order():
    try:
        # 1. Fetch user profile
        user_res = requests.get(f"{BASE_URL}/users/usr_987654321", headers=headers)
        user_res.raise_for_status()
        user = user_res.json()
        print("User Profile:", user)

        # 2. Post order with Idempotency Key
        idempotency_key = f"ik_{int(time.time())}"
        order_payload = {
            "customerId": user["id"],
            "amount": 4999,
            "currency": "USD"
        }
        
        request_headers = {**headers, "Idempotency-Key": idempotency_key}
        order_res = requests.post(f"{BASE_URL}/orders", json=order_payload, headers=request_headers)
        order_res.raise_for_status()
        
        print("Order Created:", order_res.json())

    except requests.exceptions.RequestException as e:
        print("API Error:", e)

if __name__ == "__main__":
    create_order()
    ```
    ```javascript
async function fetchWithRetry(url, retries = 3, delay = 1000) {
  try {
    return await client.get(url);
  } catch (error) {
    if (error.response?.status === 429 && retries > 0) {
      console.warn(`Rate limited. Retrying in ${delay}ms...`);
      await new Promise((resolve) => setTimeout(resolve, delay));
      return fetchWithRetry(url, retries - 1, delay * 2);
    }
    throw error;
  }
}
```
