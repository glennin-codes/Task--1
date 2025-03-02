# Microservices API Project

A Node.js microservices project with MongoDB and Redis, containerized with Docker.

## Prerequisites

- [Docker](https://docs.docker.com/get-docker/)
- [Docker Compose](https://docs.docker.com/compose/install/)

## Getting Started

### Running the Application

1. Clone the repository:
   ```
   git clone https://github.com/glennin-codes/Task--1.git
   cd  Task--1
   ```
or download this zip file
```
cd into task--1

```

2. Start the application:
   ```
   docker-compose up -d --build
   ```
   This command builds the Docker images and starts all services in detached mode.

3. Check if all containers are running:
   ```
   docker-compose ps
   ```

4. Access the API at:
   ```
   http://localhost:8080
   ```

### Stopping the Application

To stop and remove all containers:
```
docker-compose down
```

To stop and remove all containers including volumes:
```
docker-compose down -v
```

## API Documentation

### Health Check
- **URL**: `/`
- **Method**: `GET`
- **Response**: `{"status":"API is running"}`

### Redis Operations

#### Add Key-Value Pair
- **URL**: `/redis`
- **Method**: `POST`
- **Body**:
  ```json
  {
    "key": "example_key",
    "value": "example_value"
  }
  ```
- **Success Response**: `201 Created`
  ```json
  {
    "message": "Key-value pair added successfully",
    "key": "example_key",
    "value": "example_value"
  }
  ```
- **Error Response**: `400 Bad Request` or `500 Internal Server Error`

#### Get Value by Key
- **URL**: `/redis/:key`
- **Method**: `GET`
- **URL Params**: `key=[string]`
- **Success Response**: `200 OK`
  ```json
  {
    "key": "example_key",
    "value": "example_value"
  }
  ```
- **Error Response**: `404 Not Found` or `500 Internal Server Error`

### MongoDB Operations

#### Add Document
- **URL**: `/mongodb`
- **Method**: `POST`
- **Body**:
  ```json
  {
    "name": "Item Name",
    "description": "Item Description"
  }
  ```
- **Success Response**: `201 Created`
  ```json
  {
    "message": "Document added successfully",
    "item": {
      "_id": "60d21b4667d0d8992e610c85",
      "name": "Item Name",
      "description": "Item Description",
      "createdAt": "2023-03-01T00:00:00.000Z"
    }
  }
  ```
- **Error Response**: `400 Bad Request` or `500 Internal Server Error`

#### Get Document by ID
- **URL**: `/mongodb/:id`
- **Method**: `GET`
- **URL Params**: `id=[string]`
- **Success Response**: `200 OK`
  ```json
  {
    "item": {
      "_id": "60d21b4667d0d8992e610c85",
      "name": "Item Name",
      "description": "Item Description",
      "createdAt": "2023-03-01T00:00:00.000Z"
    }
  }
  ```
- **Error Response**: `404 Not Found` or `500 Internal Server Error`

#### Get All Documents
- **URL**: `/mongodb`
- **Method**: `GET`
- **Success Response**: `200 OK`
  ```json
  {
    "items": [
      {
        "_id": "60d21b4667d0d8992e610c85",
        "name": "Item Name",
        "description": "Item Description",
        "createdAt": "2023-03-01T00:00:00.000Z"
      },
      {
        "_id": "60d21b4667d0d8992e610c86",
        "name": "Another Item",
        "description": "Another Description",
        "createdAt": "2023-03-01T00:00:00.000Z"
      }
    ]
  }
  ```
- **Error Response**: `500 Internal Server Error`

## Testing API Endpoints

You can test the API endpoints using:

### Using cURL

#### Health Check
```bash
curl http://localhost:8080
```

#### Add Key to Redis
```bash
curl -X POST http://localhost:8080/redis \
  -H "Content-Type: application/json" \
  -d '{"key":"test_key","value":"test_value"}'
```

#### Get Key from Redis
```bash
curl http://localhost:8080/redis/test_key
```

#### Add Document to MongoDB
```bash
curl -X POST http://localhost:8080/mongodb \
  -H "Content-Type: application/json" \
  -d '{"name":"Test Item","description":"This is a test item"}'
```

#### Get All Documents from MongoDB
```bash
curl http://localhost:8080/mongodb
```

### Using Postman or Similar Tools

1. Import the following requests:
   - GET `http://localhost:8080/` - Health check
   - POST `http://localhost:8080/redis` with JSON body
   - GET `http://localhost:8080/redis/:key`
   - POST `http://localhost:8080/mongodb` with JSON body
   - GET `http://localhost:8080/mongodb/:id`
   - GET `http://localhost:8080/mongodb`

## Environment Variables

The application uses the following environment variables which are already set in the docker-compose.yml file:

- `PORT`: The port on which the API server runs
- `REDIS_HOST`: Hostname for Redis
- `REDIS_PORT`: Port for Redis
- `MONGO_URI`: MongoDB connection URI