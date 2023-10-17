
# Graviton Cloud API Documentation

### Overview

This documentation provides information about the Graviton Cloud RESTful Object Database HTTP API endpoints.

## Table of Contents

1. [GET `/api/v1/object`](#1-get-apiv1object)
   - [Request](#request)
   - [Response](#response)
   - [Example Requests](#example-requests)
   - [Notes](#notes)
2. [PUT `/api/v1/object`](#2-put-apiv1object)
   - [Request](#request-1)
   - [Response](#response-1)
   - [Example Requests](#example-requests-1)
   - [Notes](#notes-1)
3. [POST `/api/v1/object`](#3-post-apiv1object)
   - [Request](#request-2)
   - [Response](#response-2)
   - [Example Requests](#example-requests-2)
   - [Notes](#notes-2)
4. [DELETE `/api/v1/object`](#4-delete-apiv1object)
   - [Request](#request-3)
   - [Response](#response-3)
   - [Example Requests](#example-requests-3)
   - [Notes](#notes-3)

---

## 1. GET `/api/v1/object`

Retrieve an object based on the specified path.

### Request

#### Headers

- `x-api-key`: Your API key (UUID format).

#### Parameters

- `path` (query string, required): The path to the desired object. Example: `/path/to/object`

### Response

#### Success (200 OK)

Returns the object at the specified path.

```json
{
    "status": "success",
    "data": {
        // ... object data
    }
}
```

#### Errors

- **400 Bad Request**: Missing or invalid `path` parameter.

```json
{
    "status": "error",
    "message": "Missing or invalid path parameter."
}
```

- **401 Unauthorized**: Missing or invalid API key.

```json
{
    "status": "error",
    "message": "Unauthorized. Please provide a valid API key."
}
```

- **404 Not Found**: The specified object was not found.

```json
{
    "status": "error",
    "message": "Object not found."
}
```

### Example Requests

#### Using cURL:

```bash
curl -X GET "https://api.gravitoncloud.com/api/v1/object?path=/path/to/object" \
     -H "x-api-key: your-uuid-formatted-api-key"
```

#### Using Node.js with Axios:

First, ensure you have `axios` installed:

```bash
npm install axios
```

Then, use the following script:

```javascript
const axios = require('axios');

// Define the base URL and endpoint
const BASE_URL = 'https://api.gravitoncloud.com';
const ENDPOINT = '/api/v1/object';

// Define your query parameter and API key
const path = '/path/to/object';
const API_KEY = 'your-uuid-formatted-api-key'; // Replace with your actual API key

// Make the GET request
axios.get(`${BASE_URL}${ENDPOINT}`, {
    params: {
        path: path
    },
    headers: {
        'x-api-key': API_KEY
    }
})
.then(response => {
    console.log('Success:', response.data);
})
.catch(error => {
    console.error('Error:', error.response ? error.response.data : error.message);
});
```

### Notes

- Ensure the path is properly URL encoded if it contains special characters.
- Ensure the provided `x-api-key` is in a valid UUID format.
- If the object is not found, the path is invalid, or the API key is missing/incorrect, an error will be returned.

---

### 2. PUT `/api/v1/object`

Update or create an object at the specified path.

#### Request

##### Headers

- `x-api-key`: Your API key (UUID format).

##### Parameters

- `path` (query string, required): The path to the object you want to update or create. Example: `/path/to/object`

##### Body

- JSON payload containing the object details. Structure and fields will depend on the specific object's schema.

### Response

#### Success (200 OK or 201 Created)

If the object is updated:
```json
{
    "status": "success",
    "message": "Object updated successfully."
}
```

If a new object is created:
```json
{
    "status": "success",
    "message": "Object created successfully."
}
```

#### Errors

- **400 Bad Request**: Missing or invalid `path` parameter or invalid JSON payload.

```json
{
    "status": "error",
    "message": "Missing or invalid data."
}
```

- **401 Unauthorized**: Missing or invalid API key.

```json
{
    "status": "error",
    "message": "Unauthorized. Please provide a valid API key."
}
```

### Example Requests

#### Using cURL:

```bash
curl -X PUT "https://api.gravitoncloud.com/api/v1/object?path=/path/to/object" \
     -H "x-api-key: your-uuid-formatted-api-key" \
     -H "Content-Type: application/json" \
     -d '{
         "field1": "value1",
         "field2": "value2"
         // ... other fields ...
     }'
```

#### Using Node.js with Axios:

Ensure you have `axios` installed:

```bash
npm install axios
```

Then, use the following script:

```javascript
const axios = require('axios');

// Define the base URL, endpoint, query parameter, API key, and payload
const BASE_URL = 'https://api.gravitoncloud.com';
const ENDPOINT = '/api/v1/object';
const path = '/path/to/object';
const API_KEY = 'your-uuid-formatted-api-key'; // Replace with your actual API key
const payload = {
    field1: 'value1',
    field2: 'value2'
    // ... other fields ...
};

// Make the PUT request
axios.put(`${BASE_URL}${ENDPOINT}`, payload, {
    params: {
        path: path
    },
    headers: {
        'x-api-key': API_KEY
    }
})
.then(response => {
    console.log('Success:', response.data);
})
.catch(error => {
    console.error('Error:', error.response ? error.response.data : error.message);
});
```

### Notes

- Ensure the path is properly URL encoded if it contains special characters.
- Ensure the provided `x-api-key` is in a valid UUID format.
- Provide a valid JSON payload in the body of the PUT request.
- If the object's path exists, the data will be updated; otherwise, a new object will be created at the specified path.

---

## 3. POST `/api/v1/object`

Create a new object at the specified path. If an object already exists at the specified path, the request will fail.

### Request

#### Headers

- `x-api-key`: Your API key (UUID format).

#### Parameters

- `path` (query string, required): The path for the new object. Example: `/path/to/object`

#### Body

- JSON payload containing the object details.

### Response

#### Success (201 Created)

The object was successfully created.

```json
{
    "status": "success",
    "message": "Object created successfully."
}
```

#### Errors

- **400 Bad Request**: Missing or invalid `path` parameter or invalid JSON payload.

```json
{
    "status": "error",
    "message": "Missing or invalid data."
}
```

- **401 Unauthorized**: Missing or invalid API key.

```json
{
    "status": "error",
    "message": "Unauthorized. Please provide a valid API key."
}
```

- **409 Conflict**: An object already exists at the specified path.

```json
{
    "status": "error",
    "message": "Conflict. Object already exists at the specified path."
}
```

### Example Requests

#### Using cURL:

```bash
curl -X POST "https://api.gravitoncloud.com/api/v1/object?path=/path/to/object" \
     -H "x-api-key: your-uuid-formatted-api-key" \
     -H "Content-Type: application/json" \
     -d '{
         "field1": "value1",
         "field2": "value2"
         // ... other fields ...
     }'
```

#### Using Node.js with Axios:

Ensure you have `axios` installed:

```bash
npm install axios
```

Then, use the following script:

```javascript
const axios = require('axios');

// Define the base URL, endpoint, query parameter, API key, and payload
const BASE_URL = 'https://api.gravitoncloud.com';
const ENDPOINT = '/api/v1/object';
const path = '/path/to/object';
const API_KEY = 'your-uuid-formatted-api-key'; // Replace with your actual API key
const payload = {
    field1: 'value1',
    field2: 'value2'
    // ... other fields ...
};

// Make the POST request
axios.post(`${BASE_URL}${ENDPOINT}`, payload, {
    params: {
        path: path
    },
    headers: {
        'x-api-key': API_KEY
    }
})
.then(response => {
    console.log('Success:', response.data);
})
.catch(error => {
    console.error('Error:', error.response ? error.response.data : error.message);
});
```

### Notes

- Ensure the path is properly URL encoded if it contains special characters.
- Provide a valid JSON payload in the body of the POST request.
- If an object already exists at the specified path, the POST request will return a conflict error.

--- 

## 4. DELETE `/api/v1/object`

Delete an object at the specified path.

### Request

#### Headers

- `x-api-key`: Your API key (UUID format).

#### Parameters

- `path` (query string, required): The path of the object to be deleted. Example: `/path/to/object`

### Response

#### Success (200 OK)

The object was successfully deleted.

```json
{
    "status": "success",
    "message": "Object deleted successfully."
}
```

#### Errors

- **400 Bad Request**: Missing or invalid `path` parameter.

```json
{
    "status": "error",
    "message": "Missing or invalid path."
}
```

- **401 Unauthorized**: Missing or invalid API key.

```json
{
    "status": "error",
    "message": "Unauthorized. Please provide a valid API key."
}
```

- **404 Not Found**: The specified object was not found.

```json
{
    "status": "error",
    "message": "Object not found."
}
```

### Example Requests

#### Using cURL:

```bash
curl -X DELETE "https://api.gravitoncloud.com/api/v1/object?path=/path/to/object" \
     -H "x-api-key: your-uuid-formatted-api-key"
```

#### Using Node.js with Axios:

Ensure you have `axios` installed:

```bash
npm install axios
```

Then, use the following script:

```javascript
const axios = require('axios');

// Define the base URL, endpoint, and query parameter
const BASE_URL = 'https://api.gravitoncloud.com';
const ENDPOINT = '/api/v1/object';
const path = '/path/to/object';
const API_KEY = 'your-uuid-formatted-api-key'; // Replace with your actual API key

// Make the DELETE request
axios.delete(`${BASE_URL}${ENDPOINT}`, {
    params: {
        path: path
    },
    headers: {
        'x-api-key': API_KEY
    }
})
.then(response => {
    console.log('Success:', response.data);
})
.catch(error => {
    console.error('Error:', error.response ? error.response.data : error.message);
});
```

### Notes

- Ensure the path is properly URL encoded if it contains special characters.
- Ensure the provided `x-api-key` is in a valid UUID format.
- If the object is not found or the path is invalid, an error will be returned.

---
