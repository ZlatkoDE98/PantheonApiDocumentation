# Subjects API Documentation

## Overview
The Subjects API provides endpoints for managing and retrieving subject (business partner) data with various filtering and pagination options.

- **Base URL**: `/api/subjects`
- **Content-Type**: `application/json`

## Subject Object Schema
| Field      | Type    | Description                |
|------------|---------|----------------------------|
| acSubject  | string  | Unique identifier          |
| acName2    | string? | Subject name (optional)    |
| acAddress  | string  | Subject address            |
| acCode     | string  | Subject code               |
| acPhone    | string  | Subject phone number       |

## Endpoints

### 1. Get Paginated Subjects
Retrieves a paginated list of subjects with optional search functionality.

```http
GET /api/subjects
```

**Query Parameters**
| Parameter   | Type    | Required | Default | Max  | Description                    |
|------------|---------|----------|---------|------|---------------------------------|
| page       | integer | No       | 1       | -    | Page number                     |
| pageSize   | integer | No       | 10      | 30   | Subjects per page               |
| searchText | string  | No       | null    | -    | Text to search for in subjects  |

**Response**
```json
{
    "subjects": [
        {
            "acSubject": "SUB001",
            "acName2": "Sample Company",
            "acAddress": "123 Business St",
            "acCode": "SC001",
            "acPhone": "123-456-789"
        }
    ],
    "totalCount": 1,
    "currentPage": 1,
    "pageSize": 10
}
```

### 2. Get Subject by ID
Retrieves a specific subject by its identifier.

```http
GET /api/subjects/{acSubject}
```

**Parameters**
| Parameter  | Type   | Required | Description          |
|------------|--------|----------|----------------------|
| acSubject  | string | Yes      | Subject identifier   |

**Success Response**
```json
{
    "acSubject": "SUB001",
    "acName2": "Sample Company",
    "acAddress": "123 Business St",
    "acCode": "SC001",
    "acPhone": "123-456-789"
}
```


### 3. Get Filtered Subjects
Retrieves specified fields for all subjects.

```http
GET /api/subjects/filter?fields={fields}
```

**Query Parameters**
| Parameter | Type   | Required | Description                           |
|-----------|--------|----------|---------------------------------------|
| fields    | string | Yes      | Comma-separated list of field names (acSubject,acName2,acAddress,acCode,acPhone) |

**Example Request**
```http
GET /api/subjects/filter?fields=acSubject,acName2,acPhone
```

**Example Response**
```json
[
    {
        "acSubject": "SUB001",
        "acName2": "Sample Company",
        "acPhone": "123-456-789"
    }
]
```


### 4. Get Filtered Subject by ID
Retrieves specified fields for a specific subject.

```http
GET /api/subjects/filter/acsubject?acSubject={acSubject}&fields={fields}
```

**Query Parameters**
| Parameter  | Type   | Required | Description                              |
|------------|--------|----------|------------------------------------------|
| acSubject  | string | Yes      | Subject identifier                       |
| fields     | string | Yes      | Comma-separated list of available fields |

**Example Response**
```json
{
    "acSubject": "SUB001",
    "acPhone": "123-456-789"
}
```

### 5. Insert Subject
Creates a new subject record.

```http
POST /api/subjects
```

**Request Body**
```json
{
    "columnValues": {
        "acSubject": "SUB001",
        "acName2": "New Company",
        "acAddress": "456 Enterprise Ave",
        "acCode": "NC001",
        "acPhone": "987-654-321"
    }
}
```

**Success Response**
```json
{
    "id": "SUB001",
    "message": "Record inserted successfully"
}
```

### 6. Update Subject
Updates an existing subject record.

```http
PUT /api/subjects/acsubject?acSubject={acSubject}
```

**Request Body**
```json
{
    "columnValues": {
        "acName2": "Updated Company Name",
        "acPhone": "555-555-555"
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