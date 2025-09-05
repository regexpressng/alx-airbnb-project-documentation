# Airbnb Clone – Backend Requirements

This document details the **technical and functional requirements** for key backend features.  
The focus is on **User Authentication**, **Property Management**, and **Booking System**.

---

## 1. User Authentication

### Functional Requirements
- Users should be able to register with email and password.
- Users should be able to log in using valid credentials.
- Users should remain logged in via JWT tokens.
- OAuth login (Google, Facebook) optional but supported.

### API Endpoints
- `POST /api/auth/register` → Register a new user  
- `POST /api/auth/login` → Authenticate a user and issue JWT  
- `GET /api/auth/profile` → Retrieve user profile (requires authentication)  
- `PUT /api/auth/profile` → Update profile info  

### Input / Output Specifications
**Register (POST /auth/register)**  
- **Input:**  
  ```json
  {
    "email": "user@example.com",
    "password": "SecurePass123!",
    "name": "John Doe"
  }
