# API Basics Documentation

## Introduction
This API provides a comprehensive suite of endpoints for managing business operations including inventory management, order processing, subject (business partner) management, and stock control. The API is designed to be RESTful and returns responses in JSON format.

## Base Information
- **API Server URL**: `http://89.216.106.5:5095`
- **API Version**: v1
- **Default Content-Type**: `application/json`


## Available API Modules
The API consists of four main modules:

1. **Items API** (`/api/items`)
   - Manage product/item information
   - Handle pricing and inventory details
   - Support filtered queries and field selection

2. **Orders API** (`/api/orders`)
   - Create and manage order documents
   - Track order status and history
   - Handle order items and relationships

3. **Stocks API** (`/api/stocks`)
   - Monitor inventory levels across warehouses
   - Track stock movements and reservations
   - Manage warehouse information

4. **Subjects API** (`/api/subjects`)
   - Manage business partner information
   - Handle customer and supplier data
   - Support filtered queries and field selection

## Common Response Formats

### Success Responses
Successful responses typically return either:
- The requested data in JSON format
- A confirmation message with affected records
- HTTP 204 (No Content) for successful operations without response data

Example success response:
```json
{
    "id": "123",
    "message": "Operation completed successfully"
}
```

### Error Responses
All API endpoints use a consistent error response format:

```json
{
    "message": "Human-readable error description",
    "error": "Technical error message",
    "innerError": "Database or system error details",
    "stackTrace": "Stack trace (development environment only)"
}
```


## Pagination
APIs that return lists support pagination through common query parameters:
- `page`: Page number (default: 1)
- `pageSize`: Number of items per page (default: 10, max: 30)

Example paginated response:
```json
{
    "items": [...],
    "totalCount": 100,
    "currentPage": 1,
    "pageSize": 10
}
```

## Field Filtering
Many endpoints support field filtering using the `fields` query parameter:
```
GET /api/{endpoint}/filter?fields=field1,field2,field3
```

## Making Requests
Example of making a request to the API:

```bash
# Get all items
curl http://89.216.106.5:5095/api/items

# Get specific item with field filtering
curl http://89.216.106.5:5095/api/items/filter?fields=acIdent,acName,anSalePrice
```

## Additional Resources
For detailed information about specific endpoints, refer to the following documentation:
- [Items API Documentation](item_documentation.md)
- [Orders API Documentation](order_documentation.md)
- [Stocks API Documentation](stock_documentation.md)
- [Subjects API Documentation](subject_documentation.md)