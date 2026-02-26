# 🚀 Gateway API Project

## 📖 Project Overview

This project is a simple **API Gateway** built using **Django REST Framework**.  
It routes requests to multiple microservices, provides dynamic **Swagger documentation** for each service, and performs **health checks** to monitor service status.  

Think of it as the **central hub 🧩** that keeps all your microservices connected and healthy!

---

## ✨ Features

- 🔀 **Request Proxying** – Route requests to different microservices based on URL paths  
- 📄 **Dynamic Swagger Docs** – Fetch and display API docs from each service in real-time  
- 💓 **Health Checks** – Monitor the status of all services  
- ⚙️ **Configurable Services** – Add or update service endpoints in a single settings file  
- 🐳 **Docker & Docker Compose** – Easy deployment and management of all services  

---

## 🛠️ API Proxy & Health Check Overview

This gateway acts as the **central entry point** between clients (frontend) and microservices.  

### 1️⃣ Proxy Requests & Display Service Docs

The `ProxyDocsAPIView` class dynamically fetches Swagger documentation from the target service and returns it to the client.  

Benefits:  
- Clients never need to connect directly to individual services 🌐  
- Centralized documentation for all microservices 📚  
- Update service URLs only in gateway settings 🔧  

### 2️⃣ Health Checks

The `HealthCheckAPIView` class monitors the status of all services and reports to clients or monitoring systems.  

Benefits:  
- Quickly detect active/inactive services ⚡  
- Improve reliability by handling failing services 🛡️  
- Ensure the overall system is running smoothly ✅  

### 3️⃣ Frontend Integration

Frontend should only communicate via the gateway.  
**Never send requests directly to microservices** – this ensures **security, simplicity, and manageability**.  

#### API Endpoints for Frontend

| API Name               | HTTP Method | Path                 | Description |
|------------------------|------------|--------------------|-------------|
| 📝 Get Service Docs    | GET        | `/docs/{service}/`  | Fetch dynamic Swagger docs for a service (`{service}` = `user`, `order`, etc.). Useful for rendering docs or checking service features. |
| 💓 System Health Check | GET        | `/health/`          | Returns the health status of all services. Can be polled periodically to monitor system status. |

---

### ⚠️ Important Frontend Notes

1. **Send all requests through the gateway**  
   Example: `http://gateway-url/docs/order/` for the `order` service.

2. **Retrieve service documentation**  
   Call `GET /docs/{service}/` to dynamically fetch the Swagger docs.

3. **Check service health**  
   Call `GET /health/` to monitor all services and display status.

4. **Error Handling**  
   - Service not defined in `SERVICE_MAP` → 404 response ❌  
   - Service unavailable → 503 response ⚠️  
   - Frontend should handle these gracefully and notify users.

5. **Authentication & Security**  
   - Include a valid token or auth method for all requests 🔑  
   - No direct microservice requests allowed for security reasons 🛡️

---

### 🔍 Example Request

- Fetch Swagger docs for the `user` service:

```http
GET http://gateway-url/docs/user/
Authorization: Bearer <token>
```

---

### 📌 Summary

With this API Gateway:  
- Single **entry point** for all microservices 🏛️  
- **Improved security, monitoring, and management** 🔒  
- Simplified frontend integration 🎯  
- Scalable and maintainable system architecture 📈  

This is a **standard architecture for microservice systems** and a solid foundation for large, reliable applications.

---

## 🗂️ Project Structure

```
├── db.sqlite3
├── gateway
│   ├── asgi.py
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
├── manage.py
└── proxy
    ├── apps.py
    ├── urls.py
    └── views.py
├── Dockerfile
├── docker-compose.yml
└── run.sh
```

---

## ⚡ Usage Guide

### Prerequisites

- Docker 🐳 & Docker Compose  
- Configure your services in `gateway/settings.py` under `SERVICE_MAP`:

```python
SERVICE_MAP = {
    "auth": "http://auth-service:8000",
    "user": "http://user-service:8001",
    # other services
}
```
