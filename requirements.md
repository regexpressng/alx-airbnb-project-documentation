# Airbnb Clone – Backend Architecture (With Input/Output)

## 1. User Authentication
- **Register**
  - `POST /api/auth/register`
  - **Input**
    ```json
    {
      "email": "user@example.com",
      "password": "SecurePass123!",
      "name": "John Doe"
    }
    ```
  - **Output (Success)**
    ```json
    {
      "message": "User registered successfully",
      "userId": "uuid"
    }
    ```
  - **Output (Failure)**
    ```json
    {
      "error": "Email already in use"
    }
    ```

- **Login**
  - `POST /api/auth/login`
  - **Input**
    ```json
    {
      "email": "user@example.com",
      "password": "SecurePass123!"
    }
    ```
  - **Output**
    ```json
    {
      "token": "jwt_token",
      "userId": "uuid"
    }
    ```

- **Profile**
  - `GET /api/auth/profile` → Retrieve profile (auth required)
  - `PUT /api/auth/profile` → Update profile
    - **Input**
      ```json
      {
        "name": "John Doe Updated",
        "avatar": "url_to_image"
      }
      ```
    - **Output**
      ```json
      {
        "message": "Profile updated successfully",
        "user": {...}
      }
      ```

---

## 2. Property Management
- **Add Property (Host)**
  - `POST /api/properties`
  - **Input**
    ```json
    {
      "title": "Cozy Apartment",
      "description": "2-bedroom apartment in Lagos",
      "location": "Lagos, Nigeria",
      "pricePerNight": 50,
      "amenities": ["WiFi", "AC"],
      "maxGuests": 3,
      "availability": [
        {"startDate": "2025-09-01", "endDate": "2025-09-15"}
      ]
    }
    ```
  - **Output**
    ```json
    {
      "message": "Property created successfully",
      "propertyId": "uuid"
    }
    ```

- **Edit Property (Host)**
  - `PUT /api/properties/:id`
  - **Input** (fields to update)
    ```json
    {
      "pricePerNight": 60,
      "amenities": ["WiFi", "AC", "Pool"]
    }
    ```
  - **Output**
    ```json
    {
      "message": "Property updated successfully",
      "propertyId": "uuid"
    }
    ```

- **Delete Property (Host)**
  - `DELETE /api/properties/:id`
  - **Output**
    ```json
    {
      "message": "Property deleted successfully"
    }
    ```

- **Get Property Details**
  - `GET /api/properties/:id`
  - **Output**
    ```json
    {
      "propertyId": "uuid",
      "title": "Cozy Apartment",
      "description": "2-bedroom apartment in Lagos",
      "location": "Lagos, Nigeria",
      "pricePerNight": 50,
      "amenities": ["WiFi", "AC"],
      "availability": [...]
    }
    ```

- **List/Search Properties**
  - `GET /api/properties?location=Lagos&maxGuests=2`
  - **Output**
    ```json
    [
      {
        "propertyId": "uuid",
        "title": "Cozy Apartment",
        "pricePerNight": 50,
        "location": "Lagos"
      }
    ]
    ```

---

## 3. Booking System
- **Create Booking**
  - `POST /api/bookings`
  - **Input**
    ```json
    {
      "propertyId": "uuid",
      "startDate": "2025-09-10",
      "endDate": "2025-09-15",
      "guests": 2
    }
    ```
  - **Output (Success)**
    ```json
    {
      "message": "Booking confirmed",
      "bookingId": "uuid",
      "status": "confirmed"
    }
    ```
  - **Output (Failure)**
    ```json
    {
      "error": "Property not available for selected dates"
    }
    ```

- **Get Booking Details**
  - `GET /api/bookings/:id`
  - **Output**
    ```json
    {
      "bookingId": "uuid",
      "propertyId": "uuid",
      "userId": "uuid",
      "status": "confirmed",
      "startDate": "2025-09-10",
      "endDate": "2025-09-15",
      "guests": 2
    }
    ```

- **Cancel Booking**
  - `DELETE /api/bookings/:id`
  - **Output**
    ```json
    {
      "message": "Booking canceled successfully",
      "status": "canceled"
    }
    ```

- **List User Bookings**
  - `GET /api/bookings/user/:userId`
  - **Output**
    ```json
    [
      {
        "bookingId": "uuid",
        "propertyTitle": "Cozy Apartment",
        "status": "confirmed",
        "startDate": "2025-09-10",
        "endDate": "2025-09-15"
      }
    ]
    ```

---

## 4. Payment Integration
- **Start Payment / Checkout**
  - `POST /api/payments/checkout`
  - **Input**
    ```json
    {
      "bookingId": "uuid",
      "amount": 250,
      "currency": "USD",
      "paymentMethod": "stripe"
    }
    ```
  - **Output**
    ```json
    {
      "paymentId": "uuid",
      "status": "pending"
    }
    ```

- **Check Payment Status**
  - `GET /api/payments/status/:id`
  - **Output**
    ```json
    {
      "paymentId": "uuid",
      "status": "completed"
    }
    ```

- **Release Host Payout**
  - `POST /api/payments/payout`
  - **Input**
    ```json
    {
      "bookingId": "uuid",
      "hostId": "uuid",
      "amount": 200
    }
    ```
  - **Output**
    ```json
    {
      "message": "Payout released",
      "status": "success"
    }
    ```

---

## 5. Reviews & Ratings
- **Add Review**
  - `POST /api/reviews`
  - **Input**
    ```json
    {
      "bookingId": "uuid",
      "rating": 5,
      "comment": "Great stay!"
    }
    ```
  - **Output**
    ```json
    {
      "reviewId": "uuid",
      "message": "Review submitted"
    }
    ```

- **Get Property Reviews**
  - `GET /api/reviews/property/:id`
  - **Output**
    ```json
    [
      {
        "reviewId": "uuid",
        "userName": "John Doe",
        "rating": 5,
        "comment": "Great stay!"
      }
    ]
    ```

- **Respond to Review (Host)**
  - `POST /api/reviews/:id/respond`
  - **Input**
    ```json
    {
      "response": "Thank you for your feedback!"
    }
    ```
  - **Output**
    ```json
    {
      "message": "Response submitted successfully"
    }
    ```

---

## 6. Notifications System
- **Send Notification**
  - `POST /api/notifications/send`
  - **Input**
    ```json
    {
      "userId": "uuid",
      "type": "booking_confirmation",
      "message": "Your booking is confirmed"
    }
    ```
  - **Output**
    ```json
    {
      "message": "Notification sent successfully"
    }
    ```

- **Get User Notifications**
  - `GET /api/notifications/user/:id`
  - **Output**
    ```json
    [
      {
        "type": "booking_confirmation",
        "message": "Your booking is confirmed",
        "read": false,
        "timestamp": "2025-09-01T10:00:00Z"
      }
    ]
    ```

---

## 7. Admin Dashboard
- **List Users**
  - `GET /api/admin/users`
  - **Output**
    ```json
    [
      {
        "userId": "uuid",
        "name": "John Doe",
        "email": "user@example.com",
        "status": "active"
      }
    ]
    ```

- **Deactivate User**
  - `PUT /api/admin/users/:id/deactivate`
  - **Output**
    ```json
    {
      "message": "User deactivated successfully"
    }
    ```

- **List All Properties**
  - `GET /api/admin/properties`
  - **Output**
    ```json
    [
      {
        "propertyId": "uuid",
        "title": "Cozy Apartment",
        "hostName": "John Doe",
        "status": "active"
      }
    ]
    ```

- **List All Bookings**
  - `GET /api/admin/bookings`
  - **Output**
    ```json
    [
      {
        "bookingId": "uuid",
        "propertyTitle": "Cozy Apartment",
        "guestName": "Jane Doe",
        "status": "confirmed"
      }
    ]
    ```

- **List All Payments**
  - `GET /api/admin/payments`
  - **Output**
    ```json
    [
      {
        "paymentId": "uuid",
        "bookingId": "uuid",
        "amount": 250,
        "status": "completed"
      }
    ]
    ```
