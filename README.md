✅ AIRBNB CLONE BACKEND

Enterprise Full Documentation (Final Version)
Database: PostgreSQL
Architecture: Modular REST API
Notifications: Email only
Authentication: Email + Social OAuth (Google/Facebook)
Payment: Mocked Integration
Search: Filtering & Query Operations

────────────────────────────────────────

1. INTRODUCTION
1.1 Purpose

This Software Requirements Specification (SRS) describes the backend system for an Airbnb-like rental booking platform, defining user roles, functionalities, database and API specifications, and operational constraints.

1.2 Scope

The backend system provides:
• Secure authentication and authorization
• Property listing and hosting operations
• Search, filtering, and property discovery
• Booking lifecycle management
• Reviews and ratings
• Basic email notification system
• Admin management capabilities

Front-end is out of scope for this document.

1.3 Definitions
Term	Meaning
Host	A user who lists properties
Guest	A user who books properties
Listing	A rental property
Booking	A confirmed reservation
1.4 Target Users

• Guests
• Hosts
• Admins

2. SYSTEM OVERVIEW

The backend is a modular Node.js service exposing REST APIs to clients. PostgreSQL stores all persistent data. All operations enforce RBAC (Role-Based Access Control).

3. USER ROLES AND USER STORIES
3.1 Guest

• G01: Register and authenticate
• G02: Update profile and view bookings
• G03: Search properties by location, price, amenities, etc.
• G04: Book properties (mocked payment checkout)
• G05: Receive email booking confirmation
• G06: Submit reviews after stay

3.2 Host

• H01: Authenticate as Host
• H02: Create, update, and delete property listings
• H03: Manage availability calendars
• H04: View bookings on owned properties
• H05: Receive booking notifications by email

3.3 Admin

• A01: View and manage all users
• A02: Remove inappropriate listings
• A03: Monitor booking and system activity
• A04: Manage reported reviews

4. FUNCTIONAL REQUIREMENTS
4.1 Authentication & Authorization

• Users register with email/password or social login
• JWT-based authentication
• Strong password validation
• RBAC with Admin, Host, Guest levels

4.2 Property Management (Host Only)

• CRUD operations for listings
• Upload property media (object storage integration planned)
• Pricing and availability configuration

4.3 Search & Filtering

Supported filters:
• Location
• Price range
• Amenities
• Rating
• Property type
Includes pagination and result sorting.

4.4 Booking Management

• Prevent overlapping bookings
• Mock payment gateway integration
• Refund logic documented as stub
• Validation on stay duration and availability

4.5 Reviews & Ratings

• Guests review only after confirmed stay

5. NON-FUNCTIONAL REQUIREMENTS
Category	Requirement
Security	Encrypted passwords (bcrypt), secure JWT rotation
Availability	99 percent uptime target
Scalability	Modular architecture, index-optimized DB schema
Performance	Search response under 2 seconds under normal load
Maintainability	Service separation and clean folder structure
Auditability	Admin activity monitoring through logs
6. ENTITY RELATIONSHIP MODEL

(Text-Only Specification)

6.1 Tables and Attributes
1. users
Column	Type	Notes
id	SERIAL PK	
full_name	VARCHAR	
email	VARCHAR UNIQUE	
password_hash	TEXT	
role	ENUM('guest','host','admin')	
created_at	TIMESTAMP	
updated_at	TIMESTAMP	
2. properties
Column	Type
id	SERIAL PK
host_id	INT FK → users(id)
title	VARCHAR
description	TEXT
address	VARCHAR
price_per_night	DECIMAL
property_type	VARCHAR
created_at	TIMESTAMP
updated_at	TIMESTAMP
3. property_images
Column	Type
id	SERIAL PK
property_id	FK → properties(id)
image_url	TEXT
4. bookings
Column	Type
id	SERIAL PK
guest_id	FK → users(id)
property_id	FK → properties(id)
start_date	DATE
end_date	DATE
total_price	DECIMAL
status	ENUM('pending','confirmed','cancelled')
created_at	TIMESTAMP
5. reviews
Column	Type
id	SERIAL PK
guest_id	FK → users(id)
property_id	FK → properties(id)
rating	INT
comment	TEXT
created_at	TIMESTAMP
6.2 Relationships

• One Host has many Properties
• One Guest can have many Bookings
• One Property has many Bookings
• One Guest can leave many Reviews
• One Property has many Reviews

You will add the rendered ERD diagram as an image separately.

7. API DOCUMENTATION (REST)

Base URL:

/api/v1

7.1 Authentication Routes
Method	Endpoint	Access
POST	/auth/register	Public
POST	/auth/login	Public
POST	/auth/social-login	Public
GET	/auth/me	Authenticated
7.2 Property Routes (Host Only)
Method	Endpoint	Description
POST	/properties	Create listing
GET	/properties/host	Host listings
PUT	/properties/:id	Update listing
DELETE	/properties/:id	Remove listing
7.3 Public Property Search
Method	Endpoint
GET	/properties?location=&price_min=&price_max=&amenities=
7.4 Booking Routes
Method	Endpoint
POST	/bookings
GET	/bookings/me
PATCH	/bookings/:id/cancel
7.5 Reviews Routes
Method	Endpoint
POST	/reviews
GET	/properties/:id/reviews
7.6 Admin Routes
Method	Endpoint
GET	/admin/users
DELETE	/admin/properties/:id
8. BACKEND ARCHITECTURE & FOLDER STRUCTURE
src/
│
├── config/
├── controllers/
├── middleware/
├── models/
├── routes/
├── services/
├── utils/
└── app.js


Principles
• Controllers only handle HTTP request lifecycle
• Business rules abstracted to services
• Validation through middleware
• Database via models and query helpers

9. SECURITY & COMPLIANCE
Requirement	Implementation
Password safety	Bcrypt salted hashing
JWT protection	Expiry + refresh token strategy
Input validation	Joy/Zod schemas
Authorization	Role checks for restricted routes
SQL Injection prevention	Parameterized queries
10. EMAIL NOTIFICATIONS

Triggered events:
• Booking confirmation to Guest
• Booking notification to Host

Using lightweight queue or direct SMTP initially.

11. PAYMENT FLOW (MOCK)

Booking requests:
• System computes total price
• Marks status “pending”
• After mock payment, status becomes “confirmed”

Real provider integration possible in future.

12. FUTURE ROADMAP
Feature	Description
Real payment gateway	Stripe or PayPal
Image hosting	Cloudinary or S3
Real-time notifications	WebSockets or push
Multi-currency support	Exchange rate API
Host analytics dashboards	Revenue and load statistics
