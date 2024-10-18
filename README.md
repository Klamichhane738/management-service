# Assignment 1: Management-service for Algonquin Pet Store
In this project, we will develop the **management-service** for the Algonquin Pet Store application. This service will retrieve order messages from RabbitMQ and serve them to the **store-admin** app, which is used by employees to view customer orders.

The system follows a microservices architecture, consisting of:

- **store-front** (Vue.js) for customer interactions
- **order-service** (Node.js) for handling orders
- **product-service** (Rust/Python) for product details
- **RabbitMQ** for message brokering

Our goal is to integrate the **management-service** into this architecture, enabling efficient order retrieval for internal use in the **store-admin** app.


# Management Service

## Overview

The **Management Service** is designed to handle order management for the Algonquin Pet Store. It retrieves orders from a RabbitMQ queue and provides an API endpoint for fetching the list of orders.

## Features

- **Fetch Orders**: Provides an API to fetch orders from the RabbitMQ queue.
- **RabbitMQ Integration**: Integrates with RabbitMQ to retrieve messages from the `orders` queue.
- **Express Server**: Hosts the service using Express.js.

## Setup Instructions

Follow these steps to get the service up and running:

### 1. Clone the Repository

```bash
git clone https://github.com/Klamichhane738/management-service.git
cd management-service
```

### 2. Install Dependencies

Ensure that you have [Node.js](https://nodejs.org/) installed on your machine, then run:

```bash
npm install
```

### 3. Configure Environment Variables

Create a `.env` file in the root of the project with the following content:

```
PORT=4000
RABBITMQ_URL=amqp://localhost
RABBITMQ_QUEUE=orders
```

### 4. Run the Service

To start the service, run:

```bash
npm start
```

The service will start and listen on the specified port (default: 4000).

### 5. Testing API Endpoints

You can test the API endpoints using REST client extension for VS code using the provided `.http` file (`test-management-service.http`).

---

## API Endpoints

### **GET** `/api/orders`

Fetches all orders from the RabbitMQ queue.

#### Sample Response:

```json
[
    {
        "id": 1,
        "name": "Dog Food",
        "price": 19.99
    },
    {
        "id": 2,
        "name": "Cat Food",
        "price": 34.99
    },
    {
        "id": 3,
        "name": "Bird Seeds",
        "price": 10.99
    }
]
```

---

## RabbitMQ Setup (Local and Azure VM)

### Running RabbitMQ Locally

To run RabbitMQ locally, follow these steps:

1. **Install RabbitMQ**

   RabbitMQ can be installed on various platforms. Follow the official RabbitMQ installation guide based on your operating system. For Debian-based systems, use the following guide:

   [RabbitMQ Installation Guide for Debian](https://www.rabbitmq.com/docs/install-debian#apt-quick-start-cloudsmith)

   For other operating systems, check the [RabbitMQ Official Installation Documentation](https://www.rabbitmq.com/download.html).

2. **Start RabbitMQ**

   Once installed, start the RabbitMQ server with the following command:

   ```bash
   sudo systemctl start rabbitmq-server
   ```

3. **Enable the RabbitMQ Management Plugin**

   The RabbitMQ management plugin provides a web-based interface for managing queues, exchanges, and connections. Enable it using:

   ```bash
   sudo rabbitmq-plugins enable rabbitmq_management
   ```

4. **Restart RabbitMQ**

   To ensure that the management plugin is fully activated, restart the RabbitMQ server:

   ```bash
   sudo systemctl restart rabbitmq-server
   ```

5. **Create a New User for Remote Access (Optional)**

   For security purposes, it's recommended to create a new user for managing RabbitMQ:

   ```bash
   sudo rabbitmqctl add_user newuser newpassword
   sudo rabbitmqctl set_user_tags newuser administrator
   sudo rabbitmqctl set_permissions -p / newuser ".*" ".*" ".*"
   ```

6. **Access the Management Console**

   You can access the RabbitMQ Management Web UI via `http://localhost:15672`. Use the default credentials:

   - Username: `guest`
   - Password: `guest`

7. **AMQP Port**

   The default port for RabbitMQ's AMQP protocol (used by your app to communicate with RabbitMQ) is `5672`. Ensure that this port is open if you're running RabbitMQ locally and connecting to it from your app.

---

### Running RabbitMQ on Azure Virtual Machine

To run RabbitMQ on an Azure VM, follow these additional steps after creating a Linux-based VM:

1. **Create an Azure Virtual Machine**

   Use the Azure Portal or Azure CLI to create a Linux VM. Make sure to select a size and location that fits your needs.

2. **Install RabbitMQ on the VM**

   Once the VM is running, SSH into the VM and follow the same installation instructions as above to install RabbitMQ:

   ```bash
   sudo apt-get update
   sudo apt-get install rabbitmq-server -y
   ```

3. **Start RabbitMQ on the VM**

   Start RabbitMQ:

   ```bash
   sudo systemctl start rabbitmq-server
   ```

4. **Enable the RabbitMQ Management Plugin**

   Enable the management plugin:

   ```bash
   sudo rabbitmq-plugins enable rabbitmq_management
   sudo systemctl restart rabbitmq-server
   ```

5. **Configure Firewall Rules for Azure VM**

   Open the necessary ports for RabbitMQ on your Azure VM. You will need to open ports `5672` (for AMQP) and `15672` (for the management console).

   Use the Azure CLI or the portal to configure the Network Security Group (NSG) associated with your VM:

   ```bash
   az vm open-port --resource-group <your-resource-group> --name <your-vm-name> --port 5672
   az vm open-port --resource-group <your-resource-group> --name <your-vm-name> --port 15672
   ```

6. **Access the Management Console**

   After enabling the required ports, you can access the RabbitMQ Management Console by navigating to `http://<your-vm-ip>:15672`. Log in using the default credentials (`guest/guest`) or the new user you created.

---

## Deployment

To deploy this service to Azure App Service, follow these steps:

### 1. Build and Deploy

Ensure you have an Azure account and the [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) installed. Run the following command to deploy the service:

```bash
az webapp up --name <your-app-name> --resource-group <your-resource-group>
```

### 2. Ensure RabbitMQ Availability

For the service to function properly, RabbitMQ should be available with the appropriate settings in Azure. You can deploy RabbitMQ in Azure using a [RabbitMQ Docker image](https://hub.docker.com/_/rabbitmq) or as a [managed service](https://www.cloudamqp.com/).

---

## Additional Notes

- This service follows the 12-Factor principles for cloud-native application development.
- The provided `.http` file (`test-management-service.http`) can be used for quick API testing.
- Ensure proper security configurations when deploying to production environments, such as securing RabbitMQ connections and using environment variables for sensitive data.

