
# Management Service

## Overview
This service is designed to manage orders for the Algonquin Pet Store. It retrieves orders from a RabbitMQ queue and exposes an API endpoint for fetching the list of orders.

## Features
- **Fetch Orders:** API to fetch orders from RabbitMQ queue.
- **RabbitMQ Integration:** Retrieves messages from RabbitMQ.
- **Express Server:** Hosts the service.

## Setup Instructions

### 1. Clone the repository
```bash
git clone https://github.com/Klamichhane738/management-service.git
cd management-service
```

### 2. Install dependencies
Make sure Node.js is installed, then run:
```bash
npm install
```

### 3. Configure environment variables
Create a `.env` file at the root of the project with the following content:
```bash
PORT=4000
RABBITMQ_URL=amqp://localhost
RABBITMQ_QUEUE=orders
```

### 4. Run the service
```bash
npm start
```

### 5. Testing API Endpoints
You can use Postman or the `.http` file provided to test the API.

## API Endpoints

### GET /api/orders
Fetches all orders from the RabbitMQ queue.

#### Sample Response:
```json
[
  {
    "id": 1,
    "item": "Book",
    "quantity": 3
  },
  {
    "id": 2,
    "item": "Pen",
    "quantity": 10
  }
]
```

## RabbitMQ Integration
Ensure RabbitMQ is running locally or remotely. The following environment variables are used:
- `RABBITMQ_URL`: The URL for connecting to RabbitMQ.
- `RABBITMQ_QUEUE`: The queue name to retrieve the orders.

## Deployment
### 1. Build and deploy the service to Azure App Service.
### 2. Ensure RabbitMQ is available with appropriate settings in Azure.

## License
ISC
