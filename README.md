Interview Scheduling System

This project is a Spring Boot–based Interview Scheduling System designed to automate the process of creating interview slots, allowing candidates to book interviews, and ensuring that no two candidates can book the same slot at the same time.

The system is built using Java, Spring Boot, and MySQL and follows Clean Architecture principles with proper error handling and concurrency control.

📌 Problem Statement

In many organizations, interview scheduling is handled manually, which leads to:

Double booking of interview slots

Poor visibility of interviewer availability

Difficulty in managing weekly interview limits

This project solves these problems by providing a backend system that:

Automatically generates interview slots

Allows candidates to book only one available slot

Prevents race conditions during booking

🎯 What This System Does

Interviewer provides availability

The interviewer defines:

Available days

Start time & end time

Slot duration

Maximum interviews allowed per week

System generates slots automatically

Based on interviewer availability

Slots are generated for the next 2 weeks

Candidate selects a slot

Candidates can view only available slots

Each candidate can book only one slot

Slot confirmation

Once booked, the slot status changes to BOOKED

The same slot cannot be booked again

Slot update & cancellation

Bookings can be updated or cancelled safely

🚀 Key Features Explained
✅ Automatic Slot Generation

The system automatically creates interview slots for the next two weeks using the interviewer’s availability and slot duration.

✅ Booking Management

Candidates can:

Book a slot

View their bookings

Update or cancel a booking

✅ Race Condition Handling

To prevent double booking, the system uses:

Optimistic Locking (@Version)

Database constraints

Transactional booking logic

This ensures that even if two users try to book the same slot at the same time, only one succeeds.

✅ Weekly Interview Limit

Each interviewer can define how many interviews can be conducted per week.
The system enforces this limit automatically.

✅ Cursor-Based Pagination

Available slots are fetched using cursor-based pagination, which:

Improves performance

Avoids duplicate or missing records

Works well for large datasets

✅ Clean Architecture

The project is structured into:

Controller layer (API handling)

Service layer (business logic)

Repository layer (database access)

Domain layer (entities)

This makes the code clean, testable, and maintainable.

🛠️ Technology Stack

Java 17

Spring Boot 3

Spring Data JPA

Hibernate

MySQL

Maven

JUnit 5 & Mockito

Lombok
