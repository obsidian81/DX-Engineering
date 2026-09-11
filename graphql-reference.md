# ⚡ GraphQL API Reference

Welcome to the GraphQL API reference. Unlike traditional REST APIs with fixed endpoints, our GraphQL API exposes a single endpoint that allows you to query exactly the data you need.

* **Production Endpoint**: `https://api.example.com/graphql`
* **Protocol**: HTTP POST / WebSockets (for Subscriptions)
* **Authentication**: Bearer Token in `Authorization` header

---

## 🔍 Common Query Examples

### 1. Fetch User Profile with Nested Orders

Retrieve a specific user along with their 5 most recent orders in a single network request:

```graphql
query GetUserProfile {
  user(id: "usr_987654321") {
    id
    name
    email
    role
    createdAt
    orders(limit: 5) {
      id
      status
      totalAmount
      createdAt
    }
  }
}
```

```json
{
  "data": {
    "user": {
      "id": "usr_987654321",
      "name": "Alex Mercer",
      "email": "alex.mercer@example.com",
      "role": "CUSTOMER",
      "createdAt": "2026-01-15T08:30:00Z",
      "orders": [
        {
          "id": "ord_11223344",
          "status": "DELIVERED",
          "totalAmount": 4999,
          "createdAt": "2026-02-10T14:22:00Z"
        }
      ]
    }
  }
}
```

```graphql
mutation CreateNewUser($input: CreateUserInput!) {
  createUser(input: $input) {
    id
    name
    email
    role
    createdAt
  }
}
```

```json
{
  "input": {
    "name": "Jane Doe",
    "email": "jane.doe@example.com",
    "role": "DEVELOPER"
  }
}
```

```graphql
mutation UpdateOrder($input: UpdateOrderStatusInput!) {
  updateOrderStatus(input: $input) {
    id
    status
  }
}
```

```json
{
  "input": {
    "orderId": "ord_11223344",
    "status": "SHIPPED"
  }
}
```json
{
  "input": {
    "orderId": "ord_11223344",
    "status": "SHIPPED"
  }
}
```

```json
{
  "errors": [
    {
      "message": "User not found with ID: usr_invalid",
      "locations": [{ "line": 2, "column": 3 }],
      "path": ["user"],
      "extensions": {
        "code": "NOT_FOUND",
        "timestamp": "2026-09-02T12:00:00Z"
      }
    }
  ],
  "data": {
    "user": null
  }
}
```
