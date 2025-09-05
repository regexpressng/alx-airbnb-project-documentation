# Airbnb Clone – Backend Architecture

## 1. User Authentication
- **Register**
  - `POST /api/auth/register`
  - Input: email, password, name
  - Output: userId
- **Login**
  - `POST /api/auth/login`
  - Output: JWT token
- **Profile**
  - `GET /api/auth/profile` → Retrieve profile (auth required)
  - `PUT /api/auth/profile` → Update profile (auth required)

---

## 2. Property Management
- **Manage Properties (Host)**
  - `POST /api/properties` → Add property
  - `PUT /api/properties/:id` → Edit property
  - `DELETE /api/properties/:id` → Delete property
- **View Properties**
  - `GET /api/properties/:id` → Property details
  - `GET /api/properties` → List / search properties

---

## 3. Booking System
- **Create & Manage Bookings**
  - `POST /api/bookings` → Create booking
  - `GET /api/bookings/:id` → Get booking details
  - `DELETE /api/bookings/:id` → Cancel booking
  - `GET /api/bookings/user/:userId` → List user bookings

---

## 4. Payment Integration
- **Checkout & Payouts**
  - `POST /api/payments/checkout` → Start payment
  - `GET /api/payments/status/:id` → Check payment status
  - `POST /api/payments/payout` → Release host payout

---

## 5. Reviews & Ratings
- **Guest Reviews**
  - `POST /api/reviews` → Add review
  - `GET /api/reviews/property/:id` → Get property reviews
- **Host Responses**
  - `POST /api/reviews/:id/respond` → Respond to review

---

## 6. Notifications System
- **Notifications**
  - `POST /api/notifications/send` → Send notification (email + in-app)
  - `GET /api/notifications/user/:id` → Get user notifications

---

## 7. Admin Dashboard
- **User Management**
  - `GET /api/admin/users` → List users
  - `PUT /api/admin/users/:id/deactivate` → Deactivate user
- **Property Management**
  - `GET /api/admin/properties` → List all properties
- **Booking Management**
  - `GET /api/admin/bookings` → List all bookings
- **Payment Management**
  - `GET /api/admin/payments` → List all payments
..