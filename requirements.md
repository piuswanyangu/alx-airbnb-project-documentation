✅ Task 5: Requirements Specification

📌 File:
requirements.md

# Airbnb Clone Backend: Requirements Specification

## 1. API Requirements

### Authentication API
| Method | Endpoint | Description | Auth |
|--------|----------|-------------|------|
| POST | /auth/register | Create user account | None |
| POST | /auth/login | Authenticate user | None |
| GET | /auth/profile | Get user profile | JWT |
| PUT | /auth/profile | Update profile | JWT |

### Property API (Host)
| Method | Endpoint | Description | Role |
|--------|----------|-------------|------|
| POST | /properties | Add new listing | Host |
| GET | /properties/:id | Get property | Public |
| PUT | /properties/:id | Update listing | Host |
| DELETE | /properties/:id | Remove listing | Host |

### Booking API
| Method | Endpoint | Description | Role |
|--------|----------|-------------|------|
| POST | /bookings | Create booking | Guest |
| GET | /bookings/:id | View booking | Auth |
| DELETE | /bookings/:id | Cancel booking | Auth |

### Payment API
| POST | /payments/checkout | Mock checkout | Guest |

### Review API
| POST | /reviews | Add review | Guest |
| GET | /reviews/:propertyId | View reviews | Public |

## 2. Data Validation Rules
- Email format validation
- Password minimum length: 8 characters
- Booking dates cannot overlap
- Listings must have title, price, location

## 3. Performance Requirements
- Requests respond within 200–500ms average
- Search results paginated

## 4. Security Requirements
- Bcrypt password hashing
- JWT authentication
- SQL Injection protection
- Role-based authorization
