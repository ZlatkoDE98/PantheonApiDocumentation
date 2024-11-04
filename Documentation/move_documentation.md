# Move API Documentation

## Overview
The Move API provides endpoints for managing and retrieving move documents, which handle the transfer of items between warehouses.

- **Base URL**: `/api/move`
- **Content-Type**: `application/json`

## Move Object Schema
| Field          | Type     | Description                    |
|----------------|----------|----------------------------    |
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
| moveItems      | array    | List of move items             |

## Endpoints

### 1. Get All Moves
Retrieves all move documents.

```http
GET /api/move
```

**Response**
```json
[
    {
        "acDocType": "TRANSFER",
        "acReceiver": "REC001",
        "acIssuer": "ISS001",
        "acReceiverStock": "WH001",
        "acIssuerStock": "WH002",
        "adDate": "2024-01-01T00:00:00Z",
        "acDoc1": "DOC123",
        "adDateDoc1": "2024-01-01T00:00:00Z",
        "moveItems": [...]
    }
]
```

### 2. Get Move by ID
Retrieves a specific move document by its key.

```http
GET /api/move/{acKey}
```

**Parameters**
| Parameter | Type   | Required | Description     |
|-----------|--------|----------|-----------------|
| acKey     | string | Yes      | Move identifier |

**Error Responses**
- `404 Not Found` - Move document not found

### 3. Create Move
Creates a new move document.

```http
POST /api/move/CreateMove
```

**Request Body**
```json
{
    "acDocType": "TRANSFER",
    "acReceiver": "REC001",
    "acIssuer": "ISS001",
    "acReceiverStock": "WH001",
    "acIssuerStock": "WH002",
    "adDate": "2024-01-01T00:00:00Z",
    "acDoc1": "DOC123",
    "adDateDoc1": "2024-01-01T00:00:00Z",
    "acDoc2": null,
    "adDateDoc2": null,
    "moveItems": []
}
```

**Success Response**
```json
{
    "acKey": "MOVE001"
}
```

### 4. Update Move
Updates an existing move document.

```http
PUT /api/move/acKey?acKey={acKey}
```

**Parameters**
| Parameter | Type   | Required | Description     |
|-----------|--------|----------|-----------------|
| acKey     | string | Yes      | Move identifier |

**Request Body**
```json
{
    "columnValues": {
        "acDocType": "TRANSFER_UPDATE",
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


### 5. Delete Move
Deletes a move document.

```http
DELETE /api/move/{acKey}
```

**Parameters**
| Parameter | Type   | Required | Description     |
|-----------|--------|----------|-----------------|
| acKey     | string | Yes      | Move identifier |

**Success Response**
- `204 No Content`

**Error Response**
- `404 Not Found` - Move document not found

### 6. Update Verify Status
Updates the verification status of a move document.

```http
PATCH /api/move/{acKey}/verify-status
```

**Parameters**
| Parameter | Type   | Required | Description     |
|-----------|--------|----------|-----------------|
| acKey     | string | Yes      | Move identifier |

**Request Body**
```json
"NEW_STATUS"
```

**Success Response**
- `204 No Content`

**Error Responses**
- `400 Bad Request` - Invalid status
- `500 Internal Server Error` - Server error

### 7. Get Verify Status
Retrieves the verification status of a move document.

```http
GET /api/move/{acKey}/verify-status
```

**Parameters**
| Parameter | Type   | Required | Description     |
|-----------|--------|----------|-----------------|
| acKey     | string | Yes      | Move identifier |

**Success Response**
```json
{
    "acVerifyStatus": "VERIFIED"
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