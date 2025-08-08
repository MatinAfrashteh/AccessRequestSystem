# Access Request System

A **Java 21** & **Spring Boot**-based access request management system with three different authentication methods:
- **Custom OAuth2 Authorization Server**
- **Keycloak**
- **GitHub OAuth2 Client**

All services are containerized with **Docker** and orchestrated via **docker-compose**.

# 🐳 Running with Docker

docker-compose up -d --build
Then start the relevant containers from Docker Desktop or CLI.

---

# 📂 Project Structure:
```plaintext
AccessRequestSystem/
├── docker-compose.yml          # Orchestrates multi-container Docker applications.
├── auth-server/                # Authorization server service.
│   ├── Dockerfile              # Docker image build instructions for auth-server.
│   └── ... other files         # Source code and configuration for authentication.
└── main-app/                   # Main application service.
    ├── Dockerfile              # Docker image build instructions for main-app.
    └── ... other files         # Core business logic and APIs.
```
---

## 🚀 Overview

The **Main Application** allows users to request admin access and lets admins manage these requests.

# User Endpoints
- **Submit request**:  
  `POST http://localhost:8080/access/request`  
  Sends a request to become an admin.

- **View my requests**:  
  `GET http://localhost:8080/access/my-request`  
  Lists all requests submitted by the authenticated user.

# Admin Endpoints
- **View all requests**:  
  `GET http://localhost:8080/access/all-request`

- **Approve request**:  
  `PUT http://localhost:8080/access/requests/{id}/approve`

- **Reject request**:  
  `PUT http://localhost:8080/access/requests/{id}/reject`

---

## 🔑 Authentication Methods

### 1️⃣ Custom OAuth2 Authorization Server
- Implemented in `auth-server` (Spring Boot).
- To use it, set the **Spring profile** in `main-app/src/main/resources/application.properties`:
  ```properties
  spring.profiles.active=auth2Server

  ### Custom OAuth2 Authorization Server
Implemented in auth-server (Spring Boot).

Supports three OAuth2 grant types:

Authorization Code (default)

PKCE

Client Credentials

In this setup, the Authorization Code flow is selected by default, but you can switch to any of the other supported flows as needed.


### 2️⃣ Keycloak
Start the Keycloak container via docker-compose.

Steps:

Create a new Realm.

Create a Client.

Create Roles: USER and ADMIN.

Assign roles to the client.

Create a user account.

Set profile to jwt or opaque in application.properties.
If using opaque, enable Direct Access Grants.

### 3️⃣ GitHub OAuth2 Client
Set profile:

spring.profiles.active=auth2Client
Create an OAuth App in your GitHub Developer Settings:

Go to Developer settings → OAuth Apps → New OAuth App.

Set Homepage URL to your app's base URL (e.g., http://localhost:8080).

Set Authorization callback URL to your redirect endpoint (e.g., http://localhost:8080/login/oauth2/code/github).

Save and note down Client ID and Client Secret, then put them in application.properties.

# 🛠 Filters in Main Application
This project includes several Spring Security filters for demonstration purposes:

AuthorizationFilter: Ensures the user is authenticated before accessing restricted endpoints.

CsrfFilter: Protects against Cross-Site Request Forgery attacks.

IpBlockedFilter: Blocks requests from specific IP addresses (using mock data; disabled by default for demo purposes).

LoggerFilter: Logs incoming requests and responses.

RepeatRequestBlockerFilter: Prevents duplicate requests from being processed.

UserAgentFilter: Example filter to block certain browsers (e.g., Chrome).

🗄 Database
Main App: PostgreSQL

Auth Server: MySQL

📋 Requirements
Java 21
Maven
Docker & Docker Compose

