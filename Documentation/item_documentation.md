# Items API Documentation

## Overview
The Items API provides endpoints for managing and retrieving item data with various filtering and pagination options.

- **Base URL**: `/api/items`
- **Content-Type**: `application/json`

## Item Object Schema
| Field        | Type    | Description               |
|-------------|---------|----------------------------|
| acIdent     | string  | Unique identifier          |
| acName      | string? | Item name (optional)       |
| acCode      | string  | Item code                  |
| anBuyPrice  | double  | Purchase price             |
| anSalePrice | decimal | Sale price                 |
| anRebate    | double  | Rebate amount              |

## Endpoints

### 1. Get Paginated Items
Retrieves a paginated list of items with optional search functionality.

```http
GET /api/items
```

**Query Parameters**
| Parameter  | Type    | Required | Default | Max  | Description                    |
|------------|---------|----------|---------|------|--------------------------------|
| page       | integer | No       | 1       | -    | Page number                    |
| pageSize   | integer | No       | 10      | 30   | Items per page                 |
| searchText | string  | No       | null    | -    | Text to search for in items    |

**Response**
```json
{
    "items": [
        {
            "acIdent": "ITEM001",
            "acName": "Sample Item",
            "acCode": "SI001",
            "anBuyPrice": 100.00,
            "anSalePrice": 150.00,
            "anRebate": 10.00
        }
    ],
    "totalCount": 1,
    "currentPage": 1,
    "pageSize": 10
}
```

### 2. Get Item by ID
Retrieves a specific item by acIdent.

```http
GET /api/items/{acIdent}
```

**Parameters**
| Parameter | Type   | Required | Description        |
|-----------|--------|----------|--------------------|
| acIdent   | string | Yes      | Item identifier    |

**Success Response**
```json
{
    "acIdent": "ITEM001",
    "acName": "Sample Item",
    "acCode": "SI001",
    "anBuyPrice": 100.00,
    "anSalePrice": 150.00,
    "anRebate": 10.00
}
```

### 3. Get Filtered Items
Retrieves specified fields for all items.

```http
GET /api/items/filter?fields={fields}
```

**Query Parameters**
| Parameter | Type   | Required | Description                           |
|-----------|--------|----------|---------------------------------------|
| fields    | string | Yes      | Comma-separated list of field names (acIdent,acName,acCode,anBuyPrice,anSalePrice,anRebate) |

**Example Request**
```http
GET /api/items/filter?fields=acIdent,acName,anSalePrice
```

**Example Response**
```json
[
    {
        "acIdent": "ITEM001",
        "acName": "Sample Item",
        "anSalePrice": 150.00
    }
]
```

### 4. Get Filtered Item by ID
Retrieves specified fields for a specific item.

```http
GET /api/items/filter/acident?acIdent={acIdent}&fields={fields}
```

**Query Parameters**
| Parameter | Type   | Required | Description                              |
|-----------|--------|----------|------------------------------------------|
| acIdent   | string | Yes      | Item    identifier                       |
| fields    | string | Yes      | Comma-separated list of available fields |

**Example Response**
```json
{
    "acIdent": "ITEM001",
    "anSalePrice": 150.00
}
```

### 5. Get Item Prices

#### Single Item Prices
```http
GET /api/items/prices/acident?acIdent={acIdent}
```

**Example Response**
```json
{
    "acIdent": "ITEM001",
    "anBuyPrice": 100.00,
    "anSalePrice": 150.00,
    "anRebate": 10.00
}
```

#### All Items Prices
```http
GET /api/items/prices
```

#### Detailed Prices
```http
GET /api/items/all-prices
```

### 6. Insert Item
Creates a new item record.

```http
POST /api/items
```

**Request Body**
```json
{
    "columnValues": {
        "acIdent": "ITEM001",
        "acName": "New Item",
        "acCode": "NI001",
        "anBuyPrice": 100.00,
        "anSalePrice": 150.00,
        "anRebate": 10.00
    }
}
```

**Success Response**
```json
{
    "id": "ITEM001",
    "message": "Record inserted successfully"
}
```

### 7. Update Item
Updates an existing item record.

```http
PUT /api/items/acident?acIdent={acIdent}
```

**Request Body**
```json
{
    "columnValues": {
        "acName": "Updated Item",
        "anSalePrice": 160.00
    }
}
```

**Success Response**
```json
{
    "affectedRows": 1
}
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