# 🚖 Rider App

A simple ride-hailing app where a rider can sign up, get a price estimate for their destination, and track their distance remaining during the trip.

---

## 📁 Table of Contents

- [Vision](#vision)
- [User Stories](#user-stories)
- [Wireframes](#wireframes)
- [Data Model](#data-model)
- [API Design](#api-design)
- [Acceptance Criteria](#acceptance-criteria)
- [Tech Stack](#tech-stack)
- [Out of Scope (MVP)](#out-of-scope-mvp)

---

## 🎯 Vision

### Goal
Build a lightweight ride-hailing web app focused purely on the rider experience.

### Problem It Solves
A rider needs a simple way to:
1. Sign up and create an account
2. Enter a destination and see a price before committing
3. Track how far they are from their destination during the trip

### Who Is It For
Riders who want a no-frills, fast experience to book and track a ride.

### What Does Success Look Like
- A rider can go from sign up → price estimate → in-trip tracking in under 2 minutes
- The app works smoothly on a mobile browser

---

## 👤 User Stories

| #  | As a rider I want to...                        | So that...                              | Priority    |
|----|------------------------------------------------|-----------------------------------------|-------------|
| 1  | Sign up with my personal details               | I can create an account and use the app | 🔴 Must Have |
| 2  | Log in with my email and password              | I can access my account                 | 🔴 Must Have |
| 3  | Type in my destination                         | The app knows where I want to go        | 🔴 Must Have |
| 4  | See a price estimate before I confirm          | I know what I'll pay before committing  | 🔴 Must Have |
| 5  | Confirm or cancel my ride                      | I stay in control of my decision        | 🔴 Must Have |
| 6  | See how far I am from my destination mid-trip  | I know when I'll arrive                 | 🔴 Must Have |
| 7  | See my past trips                              | I can review my ride history            | 🟡 Should Have |
| 8  | Save a payment method                          | I don't have to re-enter it every time  | 🟡 Should Have |

---

## 🖼️ Wireframes

### Screen Flow

```
[Sign Up] → [Home / Enter Destination] → [Price Estimate] → [In-Trip View]
```

---

### Screen 1 — Sign Up

```
--------------------------
        RIDER APP
--------------------------
  First Name: [        ]
  Last Name:  [        ]
  Email:      [        ]
  Phone:      [        ]
  Password:   [        ]

       [  Sign Up  ]

  Already have an account? Log In
--------------------------
```

---

### Screen 2 — Home / Enter Destination

```
--------------------------
   Where do you want to go?

   [  Enter Destination  ]

         [ Get Price ]
--------------------------
```

---

### Screen 3 — Price Estimate

```
--------------------------
   Destination:
   123 Main Street

   Estimated Price: $12.50
   Estimated Time:  18 mins
   Distance:        12.3 km

    [ Confirm Ride ]  [ Cancel ]
--------------------------
```

---

### Screen 4 — In-Trip View

```
--------------------------
   🗺️  [ Map View ]

   Destination: 123 Main Street

   Distance Remaining: 8.2 km
   ETA: 11 mins

   Status: On the way 🚗
--------------------------
```

---

## 🗄️ Data Model

### 👤 User
Stores who the rider is. This data rarely changes.

| Field         | Type     | Notes                  |
|---------------|----------|------------------------|
| id            | UUID     | Primary key            |
| first_name    | String   |                        |
| last_name     | String   |                        |
| email         | String   | Unique                 |
| phone_number  | String   |                        |
| address       | String   | Home address           |
| password_hash | String   | Never store plain text |
| created_at    | DateTime |                        |

---

### 🚗 Trip
Stores destination and distance. Linked to the User.

| Field              | Type     | Notes                              |
|--------------------|----------|------------------------------------|
| id                 | UUID     | Primary key                        |
| rider_id           | UUID     | Foreign key → User                 |
| destination        | String   |                                    |
| distance_remaining | Float    | Updates in real time               |
| estimated_price    | Decimal  |                                    |
| status             | Enum     | `pending` / `active` / `completed` |
| created_at         | DateTime |                                    |

> 📌 Order history = all trips WHERE rider_id = current user. No separate table needed.

---

### 💳 Payment
Stores payment method. Linked to the User.

| Field          | Type    | Notes                        |
|----------------|---------|------------------------------|
| id             | UUID    | Primary key                  |
| rider_id       | UUID    | Foreign key → User           |
| card_last_four | String  | Never store full card number |
| card_type      | String  | Visa, Mastercard, etc.       |
| billing_zip    | String  |                              |
| is_default     | Boolean |                              |

> 📌 Use Stripe to handle card storage — never store raw card data yourself.

---

### Relationships

```
USER
 │
 ├──── TRIP (one user → many trips)
 │         └── destination
 │             distance_remaining
 │             status
 │             estimated_price
 │
 └──── PAYMENT (one user → many payment methods)
```

---

## 🔌 API Design

### Base URL
```
https://api.riderapp.com/v1
```

### Authentication
All endpoints except `/auth/*` require a JWT token in the header:
```
Authorization: Bearer <token>
```

---

### Auth

#### POST `/auth/signup`
Create a new rider account.

**Request Body:**
```json
{
  "first_name": "John",
  "last_name": "Doe",
  "email": "john@example.com",
  "phone_number": "555-1234",
  "address": "123 Home St",
  "password": "securepassword"
}
```

**Response:**
```json
{
  "message": "Account created successfully",
  "user_id": "uuid-1234"
}
```

---

#### POST `/auth/login`
Log in and receive a token.

**Request Body:**
```json
{
  "email": "john@example.com",
  "password": "securepassword"
}
```

**Response:**
```json
{
  "token": "jwt-token-here",
  "user_id": "uuid-1234"
}
```

---

### Trip

#### POST `/trip/estimate`
Takes a destination and returns a price estimate.

**Request Body:**
```json
{
  "destination": "123 Main Street, Detroit, MI"
}
```

**Response:**
```json
{
  "destination": "123 Main Street, Detroit, MI",
  "estimated_price": 12.50,
  "estimated_time_mins": 18,
  "distance_km": 12.3
}
```

---

#### POST `/trip/start`
Confirms and starts the trip.

**Request Body:**
```json
{
  "destination": "123 Main Street, Detroit, MI",
  "estimated_price": 12.50
}
```

**Response:**
```json
{
  "trip_id": "uuid-5678",
  "status": "active",
  "message": "Trip started"
}
```

---

#### GET `/trip/{trip_id}/status`
Returns real-time distance remaining for an active trip.

**Response:**
```json
{
  "trip_id": "uuid-5678",
  "status": "active",
  "distance_remaining_km": 8.2,
  "eta_mins": 11
}
```

---

#### GET `/trip/history`
Returns all past trips for the logged-in rider.

**Response:**
```json
[
  {
    "trip_id": "uuid-5678",
    "destination": "123 Main Street",
    "price_paid": 12.50,
    "status": "completed",
    "created_at": "2024-01-15T10:30:00Z"
  }
]
```

---

## ✅ Acceptance Criteria

### Sign Up
- ✔️ Rider can create an account with first name, last name, email, phone, address, and password
- ✔️ Duplicate emails are rejected with a clear error message
- ✔️ Password is never stored in plain text

### Log In
- ✔️ Rider can log in with email and password
- ✔️ Invalid credentials return an error message
- ✔️ Successful login returns a session token

### Price Estimate
- ✔️ Rider can type a destination and receive a price estimate
- ✔️ Estimate shows price, distance, and ETA
- ✔️ Rider can confirm or cancel before any charge occurs

### In-Trip View
- ✔️ Rider sees distance remaining after trip is confirmed
- ✔️ Distance remaining updates in real time
- ✔️ ETA is displayed alongside distance

---

## 🛠️ Tech Stack

| Layer    | Technology              | Reason                          |
|----------|-------------------------|---------------------------------|
| Frontend | React                   | Component-based, mobile-friendly|
| Backend  | Node.js + Express       | Fast, simple REST API           |
| Database | PostgreSQL              | Relational, handles our model well |
| Maps     | Google Maps API         | Distance, routing, ETA          |
| Auth     | JWT                     | Stateless, simple               |
| Payments | Stripe                  | Secure card handling            |
| Hosting  | AWS / Vercel            | Scalable, easy to deploy        |

---

## 🚫 Out of Scope (MVP)

These features are **intentionally excluded** from the first version:

- ❌ Driver management and driver app
- ❌ Real payment processing
- ❌ Push notifications
- ❌ Ratings and reviews
- ❌ Ride scheduling
- ❌ Multiple stops
- ❌ Native iOS / Android app

---

*Last updated: 2024*
