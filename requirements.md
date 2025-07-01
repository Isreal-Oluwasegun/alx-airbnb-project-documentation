# Requirements

##  Feature 1: User Authentication

### Functional Requirements:
- Users can register, log in, and manage their sessions.
- Role-based access (Guest, Host, Admin).

### API Endpoints:
| Method | Endpoint           | Description             |
|--------|--------------------|-------------------------|
| POST   | `/auth/register`   | Register a new account  |
| POST   | `/auth/login`      | Log in a user           |
| GET    | `/auth/logout`     | Log out current session |

### Input Format (POST /auth/register)
```json
{
  "email": "user@example.com",
  "password": "Password123!",
  "role": "guest"
}
```

### Output Format (Success Response)
```json
{
  "message": "User registered successfully.",
  "token": "jwt-token"
}
```

### Validation Rules:
- Email must be valid and unique
- Password: minimum 8 characters, alphanumeric
- Role must be one of: guest, host, admin

### Performance
- Response time ≤ 500ms
- Handle 200 concurrent registrations/login attempts

---

## Feature 2: Property Management

### Functional Requirements:
- Hosts can create, update, or delete property listings.

### API Endpoints:
| Method | Endpoint             | Description                     |
|--------|----------------------|---------------------------------|
| POST   | `/properties/`       | Add a new listing               |
| PUT    | `/properties/:id`    | Update a listing                |
| DELETE | `/properties/:id`    | Delete a listing                |
| GET    | `/properties/host/:id` | View listings by host ID        |

### nput Format (POST /properties)
```json
{
  "title": "Modern Loft",
  "location": "Lagos",
  "price": 150,
  "description": "Spacious and modern.",
  "available_from": "2025-07-10",
  "available_to": "2025-09-30"
}
```

### Requirements:
- Ensure host is authenticated before adding or modifying a property.
- Validate dates and price format.
- Images stored via a cloud provider (e.g., AWS S3 or Cloudinary).

---

## Feature 3: Booking System

### Functional Requirements:
- Guests can book available properties
- System checks for availability and prevents double bookings

### API Endpoints:
| Method | Endpoint              | Description                 |
|--------|-----------------------|-----------------------------|
| POST   | `/bookings/`          | Create a new booking        |
| GET    | `/bookings/:userId`   | View bookings by user       |
| DELETE | `/bookings/:bookingId`| Cancel a booking            |

### Input Format (POST /bookings)
```json
{
  "property_id": "abc123",
  "start_date": "2025-08-01",
  "end_date": "2025-08-07"
}
```

### Output Format
```json
{
  "message": "Booking confirmed.",
  "booking_id": "xyz789"
}
```

### Validation Rules:
- Date range must not overlap with existing bookings
- Start date must be ≥ today
- End date must be after start date
