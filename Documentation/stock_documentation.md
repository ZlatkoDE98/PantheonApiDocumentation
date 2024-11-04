# Stocks API Documentation

## Overview
The Stocks API provides endpoints for managing and retrieving stock/inventory data across different warehouses.

- **Base URL**: `/api/stocks`
- **Content-Type**: `application/json`

## Stock Object Schema
| Field       | Type      | Description                           |
|-------------|-----------|---------------------------------------|
| acWarehouse | string    | Warehouse identifier                  |
| acIdent     | string    | Item identifier                       |
| anStock     | decimal   | Current stock quantity                |
| anValue     | double    | Stock value                           |
| anLastPrice | double    | Last price                            |
| adTimeChg   | datetime? | Last modification time (optional)     |
| anReserved  | decimal   | Reserved quantity                     |
| adTimeIns   | datetime? | Creation time (optional)              |
| anUserIns   | int?      | User ID who created (optional)        |
| anUserChg   | int?      | User ID who modified (optional)       |
| anMinStock  | decimal   | Minimum stock level                   |
| anOptStock  | decimal   | Optimal stock level                   |
| anMaxStock  | decimal   | Maximum stock level                   |
| anQid       | int       | ID                                    |

## Endpoints

### 1. Get All Stocks
Retrieves all stock records.

```http
GET /api/stocks
```

**Response**
```json
[
    {
        "acWarehouse": "WH001",
        "acIdent": "ITEM001",
        "anStock": 100.00,
        "anValue": 1000.00,
        "anLastPrice": 10.00,
        "adTimeChg": "2024-01-01T12:00:00",
        "anReserved": 20.00,
        "adTimeIns": "2023-12-01T10:00:00",
        "anUserIns": 1,
        "anUserChg": 2,
        "anMinStock": 50.00,
        "anOptStock": 150.00,
        "anMaxStock": 200.00,
        "anQid": 1
    }
]
```

### 2. Get Stock by Warehouse and Item
Retrieves stock information for a specific item in a specific warehouse.

```http
GET /api/stocks/stock-ident?warehouse={warehouse}&ident={ident}
```

**Parameters**
| Parameter  | Type   | Required | Description          |
|------------|--------|----------|----------------------|
| warehouse  | string | Yes      | Warehouse identifier |
| ident      | string | Yes      | Item identifier      |


### 3. Get All Warehouses
Retrieves a list of all warehouses.

```http
GET /api/stocks/warehouses
```

### 4. Get Stock by Warehouse
Retrieves all stock records for a specific warehouse.

```http
GET /api/stocks/stock?warehouse={warehouse}
```

**Parameters**
| Parameter  | Type   | Required | Description          |
|------------|--------|----------|----------------------|
| warehouse  | string | Yes      | Warehouse identifier |

### 5. Get Stock by Item
Retrieves all stock records for a specific item across all warehouses.

```http
GET /api/stocks/ident?ident={ident}
```

**Parameters**
| Parameter | Type   | Required | Description     |
|-----------|--------|----------|-----------------|
| ident     | string | Yes      | Item identifier |

### 6. Update Stock
Updates stock information for a specific item in a specific warehouse.

```http
PUT /api/stocks/acwarehouse?acWarehouse={acWarehouse}&acIdent={acIdent}
```

**Parameters**
| Parameter   | Type   | Required | Description          |
|-------------|--------|----------|----------------------|
| acWarehouse | string | Yes      | Warehouse identifier |
| acIdent     | string | Yes      | Item identifier      |

**Request Body**
```json
{
    "columnValues": {
        "anStock": 150.00,
        "anReserved": 25.00,
        "anMinStock": 75.00,
        "anOptStock": 175.00,
        "anMaxStock": 250.00
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