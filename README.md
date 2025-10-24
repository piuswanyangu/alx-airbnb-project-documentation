Features & Functionalities — Airbnb Clone (Backend)
Project summary

This document enumerates the features and functional requirements for the Airbnb Clone backend. The system is a modular, service-oriented REST API using PostgreSQL for persistence. The documentation emphasizes clarity, acceptance criteria, and developer-ready detail suitable for ALX submission and professional review.

Table of contents

Project goals and scope

Architecture overview

Modules (services) and responsibilities

Feature list by actor (Guest, Host, Admin)

Core API endpoints (summary)

Data model overview (tables and key fields)

Validation & business rules (high-value rules)

Mocked payment flow (design & acceptance)

Non-functional requirements (security, performance, testing)

Deliverables & file placement

Next steps

1. Project goals and scope

Goal: Produce a production-oriented backend blueprint that fully documents features, data flows, API surface, and constraints for an Airbnb-like marketplace.

Scope (MVP):

Guests and Hosts features (Hosts limited to managing their own listings and bookings).

PostgreSQL relational schema.

Authentication with email/password and OAuth (Google, Facebook).

Mocked Payment Processing (endpoints and simulated flows).

Email notification support.

Admin capabilities for monitoring and management (documented).

Enterprise-grade documentation level.

2. Architecture overview

Style: Modular, service-structured REST API (single deployable app or multiple logical modules within one repo).

Primary architectural layers (per module):

Router → Controller → Service → Repository/Model → Database

Cross-cutting middleware: Authentication, Authorization (RBAC), Input Validation, Error Handling, Request Logging, Rate Limiting

Modules (logical services):

Auth Service (email + OAuth, JWT)

Users Service (profiles, roles)

Properties Service (host listings)

Search Service (filters, pagination)

Bookings Service (availability, reservations)

Payments Service (mocked integration)

Reviews Service (ratings & responses)

Notifications Service (email)

Admin Service (management dashboards & endpoints)

3. Modules and responsibilities
Auth Service

Register (guest/host)

Login (email/password) + OAuth callbacks

Issue/refresh JWT

Password reset flow (email token)

Role-based claims in JWT

Users Service

Profile read/update (name, bio, contact, avatar)

Role management (guest, host, admin)

View user's bookings (guest) and listings/bookings (host)

Properties Service

Create, read, update, delete listings (host-only)

Upload images (integration points: S3/Cloudinary — documented)

Mark availability windows and blackout dates

Amenities list and property metadata

Search Service

Search by location (city, coordinates), price range, guest capacity, amenities

Sorting (price, rating, distance)

Pagination and filtering

Bookings Service

Create booking (date validation, double-book prevention)

Booking status lifecycle: pending, confirmed, canceled, completed

Host acceptance flow (if required)

Cancellation and refund policy (documented rules)

Payments Service (Mocked)

Simulate payment intent creation

Simulate payment confirmation and webhook-like callbacks

Payout simulation to host account (documented)

Support for multiple currencies (design-level only)

Reviews Service

Create/read reviews (guest -> property)

Host response to reviews

Link reviews to completed bookings

Notifications Service

Email notifications for bookings, cancellations, payment events

Template structure and events registry

Admin Service

User/listing/booking/payment queries

Moderation actions (suspend user, remove listing)

Basic reporting endpoints (counts, recent activity)

4. Feature list by actor
Guest

Register / Login (email, OAuth)

Search properties

View property details and host profile

Create booking request / complete booking (with mocked payment)

View and cancel bookings per cancellation policy

Leave review after completed stay

Receive email notifications

Host

Register / Login (email, OAuth)

Create and manage listings (CRUD)

Upload listing images

Set/calendar availability and pricing

Accept or reject bookings (optional host acceptance model)

View bookings for own listings

Receive payouts (simulated)

Respond to reviews

Receive email notifications

Admin

Authenticate with admin role

Manage users, listings, bookings, payments

Moderate reviews and listings

Access system-level reports (user counts, booking metrics)

Trigger refunds or adjustments (simulated)

5. Core API endpoints (summary)

Note: This is a high-level map. Full endpoint paths, query parameters, payload examples and response schemas will be in the requirements.md (Task 5).

Auth

POST /auth/register — register (role: guest|host)

POST /auth/login — email/password login

GET /auth/oauth/:provider/callback — OAuth callback

POST /auth/refresh — refresh token

POST /auth/password-reset — request reset

POST /auth/password-reset/confirm — confirm reset

Users

GET /users/:id

PUT /users/:id

GET /users/:id/bookings

GET /users/:id/listings (host)

Properties (Hosts)

POST /properties

GET /properties/:id

PUT /properties/:id

DELETE /properties/:id

GET /properties — search endpoint with filters & pagination

Bookings

POST /bookings — create booking

GET /bookings/:id

PUT /bookings/:id — update (guest/host actions)

DELETE /bookings/:id — cancel

GET /users/:id/bookings — guest or host view

Payments (Mocked)

POST /payments/create-intent — create payment intent (mock)

POST /payments/confirm — confirm payment (mock)

POST /payments/webhook — simulated webhook endpoints (for state changes)

Reviews

POST /properties/:id/reviews

GET /properties/:id/reviews

POST /reviews/:id/response — host response

Admin (role-protected)

GET /admin/users

GET /admin/listings

GET /admin/bookings

POST /admin/moderate/listing/:id — remove/suspend

6. Data model overview (key tables / columns)
Users

id (PK), email, password_hash, full_name, role (guest|host|admin), avatar_url, bio, created_at, updated_at, oauth_provider, oauth_id

Properties

id (PK), host_id (FK->users.id), title, description, address, city, country, lat, lng, price_per_night, max_guests, amenities (JSONB), images (JSONB), created_at, updated_at, is_active

Availability (or BookableDates)

id, property_id, date, is_available, created_at

Bookings

id (PK), property_id (FK), guest_id (FK), host_id (FK), start_date, end_date, total_amount, currency, status (pending|confirmed|canceled|completed), payment_status, created_at

Payments (Mocked ledger)

id, booking_id, amount, currency, status (pending|succeeded|failed|refunded), provider_reference (nullable), created_at

Reviews

id, property_id, booking_id, guest_id, rating (1-5), comment, created_at

Admin/Audit logs (optional)

id, admin_id, action, target_type, target_id, details, created_at

7. Validation & business rules (high-value)
Authentication & User

Email must be RFC-5322 compliant.

Password minimum: 8 characters, at least one number and one special character.

Unique email constraint.

OAuth users created with oauth_provider and oauth_id and do not require password.

Properties

Price per night must be > 0.

Max guests >= 1.

At least one image required for published listing.

Host must own the listing to edit/delete.

Bookings

Booking date ranges are inclusive of start_date and exclusive of end_date (documented).

No overlapping confirmed bookings for the same property (atomic date range check).

Minimum stay policy (configurable per property).

Cancellation rules: guest can cancel up to X days before for full refund (configured globally).

Payments (mocked)

Payment intent reserved on POST /payments/create-intent.

Booking status transitions:

pending (awaiting payment) → confirmed (payment succeeded) → completed

canceled can be triggered by host, guest, or admin (documented rules for refund)

Mock payment provider responses emulate success/failure/webhook signatures.

8. Mocked payment flow (design & acceptance)

Design: Payment service exposes an API that mimics Stripe behavior:

Guest creates booking → server creates booking with status = pending.

Client calls POST /payments/create-intent with booking id and amount.

Server returns a client_secret (mock) and stores a payment_intent record.

Client calls POST /payments/confirm to simulate completing payment (or a webhook triggers succeeded).

On success: payment_status = succeeded, booking status = confirmed, send email notifications.

On failure: payment_status = failed, booking remains pending (or is auto-canceled after timeout).

Acceptance criteria:

Payment endpoints return deterministic mock responses for successful and failed flows.

The system updates booking and payment records consistently.

Email notifications sent on payment success and failure.

9. Non-functional requirements

Security

Passwords hashed with bcrypt.

JWTs signed with a strong secret; refresh tokens stored securely.

Input validation for all endpoints.

Rate limiting for auth and payments endpoints.

Performance & Scalability

Indexing on frequently filtered fields (city, price, host_id).

Caching layer recommended (Redis) for search results and session caching (design documented).

Horizontal scaling possible due to modular services.

Reliability

Centralized error handling (structured error responses).

Idempotency keys for payments and booking creation endpoints.

Testing

Unit tests for services and controllers (Jest / Mocha).

Integration tests for API endpoints (Supertest).

Mocking strategy for third-party services (email, payments).

Observability

Request logging with correlation IDs.

Error logs, and basic metrics exposure endpoints.

10. Deliverables & file placement

Place the following files in your repository exactly as shown:

alx-airbnb-project-documentation/
├─ features-and-functionalities/
│  └─ README.md         ← this file
├─ use-case-diagram/
│  └─ README.md
│  └─ use-case.png      ← Draw.io export
├─ user-stories/
│  └─ user-stories.md
│  └─ README.md
├─ data-flow-diagram/
│  └─ data-flow.png     ← Draw.io export
├─ flowcharts/
│  └─ registration-flow.png
│  └─ booking-flow.png
├─ requirements.md      ← API endpoint specs, validation rules (Task 5)
└─ .github/
   └─ workflows/        ← optional CI for tests & lint


Git commands (example)

git add features-and-functionalities/README.md
git commit -m "docs: add features and functionalities for modular Airbnb clone"
git push origin main

11. Next steps (task sequence)

The next artifacts to produce (ready for commit) are:

Use Case Diagram (Draw.io) — use-case-diagram/use-case.png + README.md

User Stories — user-stories/user-stories.md (≥ 5 user stories)

Data Flow Diagram — data-flow-diagram/data-flow.png

Flowchart(s) — pick key process (booking) and produce flowchart PNG under flowcharts/

requirements.md — full API endpoint specs, request/response schemas, validation rules, sample cURL calls
