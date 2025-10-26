✅ Task 0: Features and Functionalities Documentation

📌 File:
features-and-functionalities/README.md

# Airbnb Clone Backend: Features and Functionalities

## 1. User Management
- User registration (email + password)
- Social login (Google)
- Secure password hashing
- Email verification
- Edit and update profile
- Role based access (Guest, Host, Admin)

## 2. Property Listings Management (Host Only)
- Create property listing
- Upload images
- Update property details
- Delete listing
- Set availability calendar

## 3. Search and Filtering (Guest)
- Search by location
- Filter by:
  - Price range
  - Number of guests
  - Amenities
  - Ratings
- Pagination support

## 4. Booking Management
- Check availability
- Create a reservation
- Cancel reservation
- Booking statuses:
  - Pending
  - Confirmed
  - Cancelled
  - Completed

## 5. Payment Processing (Mocked)
- Collect payment on booking
- Payment confirmation response
- Payment failure messaging

## 6. Reviews and Ratings
- Guests review properties
- Ratings 1–5
- Hosts view reviews

## 7. Notification System
- Email notifications for:
  - Booking confirmation
  - Booking cancellation
  - Payment result

## 8. Admin Capabilities
- View users
- View listings
- Deactivate fraud accounts
- Remove inappropriate listings


✅ Commit this before proceeding.

✅ Task 1: Use Case Diagram

📌 File:
use-case-diagram/README.md
Image: use-case-diagram.png

Instruction for Draw.io

Include actors:

Guest

Host

Admin

Payment System (External)

Include core interactions:

Register/Login

Manage Listings (Host)

Search Properties (Guest)

Create Booking

Process Payment

Manage Users (Admin)

✅ After exporting PNG, embed this text in README.md:

# Use Case Diagram
This diagram illustrates actors and their interactions with core system features.

![Use Case Diagram](./use-case-diagram.png)

✅ Task 2: User Stories

📌 File:
user-stories/user-stories.md

✅ Already approved structure:



✅ Add README.md that just links to this file.

✅ Task 3: Data Flow Diagram

📌 Folder + File:
data-flow-diagram/data-flow.png
Text: data-flow-diagram/README.md

Elements to include in Draw.io

📌 Processes:

Auth Service

Listing Service

Booking Service

Payment System (external)

Notification Service

📌 Data Stores:

Users

Properties

Bookings

Payments

Reviews

📌 Flow examples:
Guest → Search Request → System → Listings DB → Response

✅ README.md content:

# Data Flow Diagram

The Data Flow Diagram illustrates how data interacts between users, backend services, and the database.

![Data Flow Diagram](./data-flow.png)

✅ Task 4: Flowchart (Choose User Registration Process)

📌 Folder + File:
flowcharts/data-flow-diagram.png
Text: flowcharts/README.md

Flowchart steps:

User submits registration form

Validate input

Hash password

Store in Users DB

Send verification email

Return success response

✅ README.md content:

# User Registration Flowchart

This flowchart shows the sequence of system actions during user registration.

![Flowchart](./data-flow-diagram.png)

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
