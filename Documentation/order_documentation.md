# Order API Documentation

## Overview
The Orders API provides endpoints for managing and retrieving order documents with pagination support and item management capabilities.

- **Base URL**: `/api/orders`
- **Content-Type**: `application/json`

## Order Object Schema
| Field          | Type     | Description                    |
|----------------|----------|--------------------------------|
| acDocType      | string   | Document type                  |
| acReceiver     | string   | Receiver subject code          |
| acIssuer       | string   | Issuer subject code            |
| acReceiverStock| string   | Receiver warehouse code        |
| acIssuerStock  | string   | Issuer warehouse code          |
| adDate         | datetime | Date of the document           |
| acDoc1         | string   | External document 1            |
| adDateDoc1     | datetime | External document 1 date       |
| acDoc2         | string?  | External document 2 (optional) |
| adDateDoc2     | datetime | External document 2 date       |
| orderItems     | array    | List of order items            |

## Endpoints

### 1. Get Paginated Orders
Retrieves a paginated list of orders.

```http
GET /api/orders
```

**Query Parameters**
| Parameter | Type    | Required | Default | Max | Description        |
|-----------|---------|----------|---------|-----|--------------------|
| page      | integer | No       | 1       | -   | Page number        |
| pageSize  | integer | No       | 10      | 30  | Items per page     |

**Response**
```json
{
    "orders": [
        {
            "acDocType": "ORDER",
            "acReceiver": "REC001",
            "acIssuer": "ISS001",
            "acReceiverStock": "WH001",
            "acIssuerStock": "WH002",
            "adDate": "2024-01-01T00:00:00Z",
            "acDoc1": "DOC123",
            "adDateDoc1": "2024-01-01T00:00:00Z",
            "orderItems": [...]
        }
    ],
    "totalCount": 100,
    "currentPage": 1,
    "pageSize": 10
}
```

### 2. Get Order by ID
Retrieves a specific order document by its key.

```http
GET /api/orders/{acKey}
```

**Parameters**
| Parameter | Type   | Required | Description      |
|-----------|--------|----------|------------------|
| acKey     | string | Yes      | Order identifier |


### 3. Create Order
Creates a new order document.

```http
POST /api/orders/CreateOrder
```

**Request Body**
```json
{
    "acDocType": "0100",
    "acReceiver": "REC001",
    "acIssuer": "ISS001",
    "adDate": "2024-01-01T00:00:00Z",
    "orderItems": [
        "acIdent": "Ident 01",
        "anQty" : 3
    ]
}
```

**Success Response**
```json
{
    "acKey": "2401000000004"
}
```


### 4. Update Order
Updates an existing order document.

```http
PUT /api/orders/acKey?acKey={acKey}
```

**Parameters**
| Parameter | Type   | Required | Description      |
|-----------|--------|----------|------------------|
| acKey     | string | Yes      | Order identifier |

**Request Body**
```json
{
    "columnValues": {
        "acDocType": "3000",
        "adDate": "2024-01-02T00:00:00Z"
    }
}
```

**Success Response**
```json
{
    "affectedRows": 1
}
```


### 5. Delete Order
Deletes an order document.

```http
DELETE /api/orders/{acKey}
```

**Parameters**
| Parameter | Type   | Required | Description      |
|-----------|--------|----------|------------------|
| acKey     | string | Yes      | Order identifier |

**Success Response**
- `204 No Content`


### 6. Update Order Status
Updates the status of an order document.

```http
PATCH /api/orders/{acKey}/status
```

**Parameters**
| Parameter | Type   | Required | Description      |
|-----------|--------|----------|------------------|
| acKey     | string | Yes      | Order identifier |

**Request Body**
```json
"Ponuda"
```

**Success Response**
- `204 No Content`


### 7. Get Order Status
Retrieves the status of an order document.

```http
GET /api/orders/{acKey}/status
```

**Parameters**
| Parameter | Type   | Required | Description      |
|-----------|--------|----------|------------------|
| acKey     | string | Yes      | Order identifier |

**Success Response**
```json
{
    "acStatus": "Ponuda"
}
```

### 8. Get Order Items
Retrieves all items associated with a specific order.

```http
GET /api/orders/{acKey}/items
```

**Parameters**
| Parameter | Type   | Required | Description      |
|-----------|--------|----------|------------------|
| acKey     | string | Yes      | Order identifier |

**Success Response**
```json
[
    {
        // Order item details
    }
]
```

## Error Response Format
When an error occurs, the API returns error details in the following format:

```json
{
    "message": "Error description",
    "error": "Technical error message",
    "innerError": "Database or system error details",
    "stackTrace": "Stack trace for debugging (only in development)"
}
```