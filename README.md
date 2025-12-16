Automatic Interview Scheduling System
1. System Overview

The Automatic Interview Scheduling System is a backend REST API developed using Java, Spring Boot, and MySQL.
It enables interviewers to define their availability and allows candidates to book, update, or cancel interview slots.

The system is specifically designed to handle concurrent booking requests, ensuring that no two candidates can book the same interview slot at the same time, even under high load.

2. Architecture

The application follows Clean Architecture principles, ensuring clear separation of concerns, scalability, and testability.

Controller Layer
Handles HTTP requests, request validation, and response formatting.

Service Layer
Contains core business logic such as:

Slot generation

Booking rules

Weekly interview limit enforcement

Atomic rescheduling

Repository Layer
Uses Spring Data JPA to abstract database operations and manage persistence.

Domain Layer
Contains core entities such as Interviewer, Availability, InterviewSlot, and Booking, which directly map to the database schema.

3. Key Features & Implementation Details
A. Automatic Slot Generation

Logic:
Interviewers define their recurring weekly availability (for example, Mondays from 9 AM to 12 PM with 30-minute slots).
The system projects this availability 14 days into the future and generates individual InterviewSlot records.

Design Decision:
Slots are pre-generated and stored in the database instead of being calculated dynamically.

Benefit:

Fetching available slots becomes extremely fast

Reduces computation during API calls

Improves system scalability

B. Concurrency Handling (Race Conditions)

Problem:
Multiple candidates may attempt to book the same slot at the exact same time.

Solution:
The system uses Optimistic Locking through JPA’s @Version annotation.

How It Works:

Each slot has a version field

When two transactions try to book the same slot, only one succeeds

The second transaction fails automatically due to version mismatch

Trade-off Discussion:
Optimistic locking was chosen over pessimistic locking because:

Interview bookings are typically low-contention

It provides better throughput

It avoids unnecessary database row blocking

C. Atomic Booking & Rescheduling

Implementation:
Critical booking and rescheduling operations are wrapped using Spring’s @Transactional annotation.

Behavior:
For rescheduling:

The old slot is released

The new slot is booked

If either step fails, the entire transaction is rolled back.

Result:

No partial updates

Data integrity is always preserved

4. API Endpoints
Method	Endpoint	Description
POST	/api/v1/interviewers	Create interviewer and define availability
GET	/api/v1/slots/available	Fetch available slots for next 14 days
POST	/api/v1/bookings	Book an interview slot (race-condition safe)
PUT	/api/v1/bookings/{id}	Update an existing booking
DELETE	/api/v1/bookings/{id}	Cancel a booking
5. Database Schema Design

interviewers
Stores interviewer details and weekly interview limits.

availabilities
Stores recurring weekly availability rules.

interview_slots
Stores actual interview slots generated for the next 14 days.
Includes a version column for optimistic locking.

bookings
Stores candidate booking information and booking status.

6. Testing Strategy

Unit & Integration Testing were implemented using JUnit 5 and Mockito.

Concurrency Testing:
Multi-threaded tests simulate multiple candidates attempting to book the same slot simultaneously.

Result:

Exactly one booking succeeds

Remaining attempts fail safely without data corruption

7. Bonus Features & Enhancements
A. Cursor-Based Pagination

Implementation:
Cursor-based pagination is used instead of traditional offset pagination.

Trade-offs:

Pros:

Constant-time performance

Ideal for infinite scroll or “Load More” UIs

Cons:

Cannot jump directly to a specific page number

Acceptable trade-off for scheduling systems

B. Basic UI & Debouncing

UI:
A lightweight HTML + JavaScript interface is provided in
src/main/resources/static/index.html.

Debouncing:
A custom JavaScript debounce() function is implemented.

Purpose:

Prevents multiple rapid booking requests

Reduces unnecessary backend load

Avoids accidental double submissions

8. Conclusion

This project demonstrates:

Real-world backend system design

Safe concurrency handling

Clean architecture implementation

Scalable and maintainable API development

It is suitable for backend interviews, system design discussions, and production-ready evaluation tasks.
