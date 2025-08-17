# Spring Authorization Server - Custom Implementation

This repository contains a **custom Spring Authorization Server** implementation to handle OAuth 2.1 flows, token customization, and secure API-to-API communication.  
The project demonstrates various grant types, token formats, and integration with a database for user authentication.

---

## 📌 Features Implemented

### 1. **Custom Authorization Server**
- Implemented a fully functional **Authorization Server** using [Spring Authorization Server](https://spring.io/projects/spring-authorization-server).
- The server is responsible for issuing OAuth 2.1 tokens, validating clients, and managing authentication flows.

---

### 2. **Setup of Spring Authorization Server**
- Integrated **Spring Authorization Server** into a Spring Boot application.
- Configured `AuthorizationServerConfiguration` with:
  - OAuth 2 endpoints (`/oauth2/authorize`, `/oauth2/token`)
  - Token introspection & revocation endpoints
  - Registered security filters and CORS configuration.

---

### 3. **Client Credentials Grant (API-to-API Communication)**
- Created **client credentials** inside the Auth Server for **service-to-service authentication**.
- Example:  
clientId: service-client
clientSecret: service-secret
grantType: client_credentials
scope: service.read, service.write

- Useful for microservices communicating without end-user context.

---

### 4. **OAuth2 Token Customization**
- Customized **access tokens** to include:
- Additional claims (roles, permissions, tenant info)
- Custom token expiration settings
- Used `OAuth2TokenCustomizer<JwtEncodingContext>` to inject claims dynamically.

---

### 5. **Auth Code & PKCE Grant Type Clients**
- Registered clients for **Authorization Code Flow with PKCE**:
- Used for public clients (e.g., SPAs, mobile apps)
- PKCE ensures secure exchange of authorization codes
- Example:  
clientId: spa-client
grantType: authorization_code
redirectUri: http://localhost:8080/login/oauth2/code/spa-client
scopes: openid, profile, email

---

### 6. **User Authentication with Database**
- Integrated a **custom `UserDetailsService`** to authenticate end users from a **database**.
- Passwords stored securely using **BCrypt hashing**.
- Supports role-based authentication for resource access.

---

### 7. **Auth Code & PKCE Flow Demo**
- Implemented a working demo of:
1. User logs in via browser
2. Authorization Server issues an **authorization code**
3. Client exchanges code + PKCE verifier for an **access token**
4. Token used to call secured APIs.

---

### 8. **Refresh Token Flow Demo**
- Enabled **Refresh Token** support to get new access tokens without re-authenticating.
- Configured token validity periods for **short-lived access tokens** and **longer-lived refresh tokens**.

---

### 9. **Opaque Tokens Support**
- Demonstrated token issuance as **opaque tokens** instead of JWTs.
- Opaque tokens require **introspection endpoint** (`/oauth2/introspect`) to validate.
- Useful when token content should not be exposed to clients.

---

## 🚀 Getting Started

### **Prerequisites**
- Java 21
- Maven 3.5+
- MySQL (for user authentication)
- Git

---

### **Clone the Repository**
```bash
git clone https://github.com/Sangramjit786/Spring-Auth-Server-MS.git
cd Spring-Auth-Server-MS

Configure Database:
Update application.properties:
```
spring.application.name=${AS_NAME:authserver}

server.port= ${AS_SERVER_PORT:9000}
logging.level.org.springframework.security=${SPRING_SECURITY_LOG_LEVEL:TRACE}

spring.datasource.url=jdbc:mysql://${DATABASE_HOST:localhost}:${DATABASE_PORT:3306}/${DATABASE_NAME:eazybank}
spring.datasource.username=${DATABASE_USERNAME:root}
spring.datasource.password=${DATABASE_PASSWORD:root}
spring.jpa.show-sql=${JPA_SHOW_SQL:true}
spring.jpa.properties.hibernate.format_sql=${HIBERNATE_FORMAT_SQL:true}

logging.pattern.console = ${LOGPATTERN_CONSOLE:%green(%d{HH:mm:ss.SSS}) %blue(%-5level) %red([%thread]) %yellow(%logger{15}) - %msg%n}
```

Run the Application:
```
mvn spring-boot:run
```

Authorization Server will be available at:
http://localhost:9000

📚 API Endpoints
Endpoint	Method	Description
```
/oauth2/authorize	GET	Authorization endpoint
/oauth2/token	POST	Token endpoint
/oauth2/introspect	POST	Token introspection for opaque tokens
/oauth2/revoke	POST	Revoke access or refresh tokens
/userinfo	GET	User information endpoint (OIDC)
````

🔑 OAuth 2.1 Flows Supported
```
Client Credentials (Service-to-Service)
Authorization Code with PKCE (Public Clients)
Refresh Token (Silent Re-authentication)
Opaque Tokens (Token Introspection)
```
