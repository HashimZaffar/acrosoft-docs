# Swagger Pet Store API Manual

**Use case:** Register a user account, explore pets, and place an order

---

## Overview / Purpose

This guide explains how to use the **Swagger Pet Store** REST APIs to:

1. **Create a user account**
2. **Log in**
3. **Explore available pets**
4. **View pet details**
5. **Place an order for a pet**

All examples use the public demo API at:

```text
Base URL: https://petstore.swagger.io/v2
```

---

## Prerequisites

* Internet access to reach `https://petstore.swagger.io/v2`
* Any REST client:

  * cURL (command-line), or
  * Postman / similar API tool
* Basic understanding of:

  * HTTP methods: `GET`, `POST`
  * JSON request/response bodies

> **Note:** This is a demo environment. Data is not persistent or secure and should not contain real personal information.

---

## High-Level Flow

1. **Register a user** – `POST /user`
2. **Log in** – `GET /user/login`
3. **Find available pets** – `GET /pet/findByStatus?status=available`
4. **Retrieve details of a specific pet** – `GET /pet/{petId}`
5. **Place an order** – `POST /store/order`

---

## Step 1: Create a User Account

### Endpoint

```http
POST /user
```

### Purpose

Creates a new user in the Pet Store system.

### Request

**Headers**

```http
Content-Type: application/json
```

**Body (JSON)**

```json
{
  "id": 1,
  "username": "john_doe",
  "firstName": "John",
  "lastName": "Doe",
  "email": "john.doe@example.com",
  "password": "P@ssword123",
  "phone": "+1-202-555-0100",
  "userStatus": 1
}
```

### cURL Example

```bash
curl -X POST "https://petstore.swagger.io/v2/user" \
  -H "Content-Type: application/json" \
  -d '{
    "id": 1,
    "username": "john_doe",
    "firstName": "John",
    "lastName": "Doe",
    "email": "john.doe@example.com",
    "password": "P@ssword123",
    "phone": "+1-202-555-0100",
    "userStatus": 1
  }'
```

### Expected Response

```http
200 OK
```

Body is typically an empty or generic success message.

---

## Step 2: Log In the User

### Endpoint

```http
GET /user/login
```

### Purpose

Authenticates the user and returns a session token (in headers/body).

### Request

**Query Parameters**

* `username` – the username created in Step 1
* `password` – the password in plain text

**Example URL**

```text
GET /user/login?username=john_doe&password=P@ssword123
```

### cURL Example

```bash
curl -X GET "https://petstore.swagger.io/v2/user/login?username=john_doe&password=P@ssword123"
```

### Expected Response

```http
200 OK
Content-Type: application/json
X-Rate-Limit: <number>
X-Expires-After: <UTC timestamp>
```

Body:

```json
"logged in user session:123456789"
```

Keep this session active while calling further APIs in the same client.

---

## Step 3: Explore Available Pets

To browse pets, you will typically:

1. List pets by status
2. Inspect individual pet details

### 3.1 Find Pets by Status

#### Endpoint

```http
GET /pet/findByStatus
```

#### Purpose

Returns all pets that match a specific status.

#### Request

**Query Parameters**

* `status` – one or more of: `available`, `pending`, `sold`

Example: get all **available** pets:

```text
GET /pet/findByStatus?status=available
```

#### cURL Example

```bash
curl -X GET "https://petstore.swagger.io/v2/pet/findByStatus?status=available" \
  -H "Accept: application/json"
```

#### Example Response

```json
[
  {
    "id": 10,
    "category": {
      "id": 1,
      "name": "Dogs"
    },
    "name": "Rex",
    "photoUrls": [
      "https://example.com/rex.png"
    ],
    "tags": [
      {
        "id": 100,
        "name": "friendly"
      }
    ],
    "status": "available"
  }
]
```

> **Tip:** From the response list, choose the `id` of the pet you want to order (e.g., `10`).

---

### 3.2 Get Pet Details by ID

#### Endpoint

```http
GET /pet/{petId}
```

#### Purpose

Returns detailed information about one specific pet.

#### Path Parameter

* `petId` – the numeric ID of the pet (for example, `10`)

#### cURL Example

```bash
curl -X GET "https://petstore.swagger.io/v2/pet/10" \
  -H "Accept: application/json"
```

#### Example Response

```json
{
  "id": 10,
  "category": {
    "id": 1,
    "name": "Dogs"
  },
  "name": "Rex",
  "photoUrls": [
    "https://example.com/rex.png"
  ],
  "tags": [
    {
      "id": 100,
      "name": "friendly"
    }
  ],
  "status": "available"
}
```

Confirm that `status` is `available` before placing an order.

---

## Step 4: Place an Order for a Pet

### Endpoint

```http
POST /store/order
```

### Purpose

Creates a purchase order for the specified pet.

### Request

**Headers**

```http
Content-Type: application/json
Accept: application/json
```

**Body (JSON)**

```json
{
  "id": 1000,
  "petId": 10,
  "quantity": 1,
  "shipDate": "2025-11-26T06:29:12.182Z",
  "status": "placed",
  "complete": true
}
```

* `id` – order ID (client-defined for demo)
* `petId` – the pet’s ID from Step 3
* `quantity` – number of pets to order
* `shipDate` – ISO 8601 datetime string
* `status` – `"placed"` | `"approved"` | `"delivered"` (for demo, `"placed"` is typical)
* `complete` – `true` or `false`

### cURL Example

```bash
curl -X POST "https://petstore.swagger.io/v2/store/order" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -d '{
    "id": 1000,
    "petId": 10,
    "quantity": 1,
    "shipDate": "2025-11-26T06:29:12.182Z",
    "status": "placed",
    "complete": true
  }'
```

### Example Response

```json
{
  "id": 1000,
  "petId": 10,
  "quantity": 1,
  "shipDate": "2025-11-26T06:29:12.182Z",
  "status": "placed",
  "complete": true
}
```

---

## Optional: Check the Order Status

### Endpoint

```http
GET /store/order/{orderId}
```

### Path Parameter

* `orderId` – ID used in the order request (e.g., `1000`)

### cURL Example

```bash
curl -X GET "https://petstore.swagger.io/v2/store/order/1000" \
  -H "Accept: application/json"
```

You’ll get the order record with `status` and `quantity`.

---

## Troubleshooting / Notes

| Issue                                    | Possible Cause                            | Suggested Action                                                   |
| ---------------------------------------- | ----------------------------------------- | ------------------------------------------------------------------ |
| `400 Invalid username/password supplied` | Typo or mismatch in credentials           | Recheck `username` and `password` values sent to `/user/login`.    |
| `404 Pet not found`                      | `petId` does not exist or was removed     | Re-run `GET /pet/findByStatus` to get a valid `petId`.             |
| `400 Invalid Order`                      | Missing or incorrect fields in order JSON | Ensure `petId`, `quantity`, and `status` are provided and valid.   |
| Empty / unexpected data                  | Demo data reset or changed                | Treat Pet Store as a sample; re-create sample user, re-query pets. |

