# LLD Preparation Strategy & Interview Questions

> **Source:** Reddit  
> **User:** u/user9908807

---

> **Note:** This contains a preparation plan and question list. Use ChatGPT to customize it according to the role (SDE-1/SDE-2) and company you're preparing for. You can also ask it to generate complete LLD solutions following this framework.

---

# How to Think About LLD

Think of LLD as:

> **"How would I model and organize this backend system cleanly?"**

**Not:**

- Perfect UML
- Every design pattern
- Enterprise architecture diagrams

## Your Goals

- Clean entities
- Clear relationships
- Extensibility
- Well-defined workflows
- Concurrency awareness

---

# LLD Interview Framework

Use this exact flow for every LLD interview question.

## 1. Clarify Requirements

Ask:

- What are the core features?
- Are there any scale or concurrency concerns?
- Is this a single-machine or distributed system?
- What is out of scope?

---

## 2. Identify Core Entities

Ask yourself:

> **"What are the nouns in this system?"**

### Examples

#### Uber

- Rider
- Driver
- Trip
- Vehicle

#### Booking System

- User
- Show
- Seat
- Reservation

#### Job Scheduler

- Job
- Worker
- Queue

---

## 3. Define Relationships

Examples:

- Driver **HAS-A** Vehicle
- Trip **HAS-A** Rider
- Reservation **BELONGS TO** User

> Keep the relationships simple and intuitive.

---

## 4. Add Interfaces / Strategies

Ask:

> **"What business logic may change later?"**

Examples:

- PricingStrategy
- MatchingStrategy
- RetryStrategy
- Filter

> This is often one of the strongest **SDE-2** signals.

---

## 5. Define the Main Workflow

Explain:

- How requests enter the system
- How processing happens
- State transitions
- Failure paths

---

## 6. Discuss Concurrency

Always mention:

- Race conditions
- Locks
- Atomic updates
- Thread safety

### Common Examples

- Double seat booking
- Two drivers accepting the same ride
- Multiple workers processing the same job

---

## 7. Discuss Extensibility

Ask:

> **"What feature might the product team ask for next?"**

Examples:

- New pricing algorithm
- New retry policy
- New notification channel

---

# LLD Topics to Know

Focus on concepts that repeatedly appear in interviews.

## OOP Fundamentals

- Encapsulation
- Abstraction
- Inheritance
- Polymorphism

---

## Relationships

- Association
- Aggregation
- Composition

---

## High ROI Design Patterns

- Factory
- Strategy
- Observer
- State

---

## Concurrency

- Locks
- Synchronization
- Thread Safety

---

## Common Building Blocks

- Scheduling
- Search & Filtering
- Reservations
- Queues
- Notifications

---

# LLD Question List

## High Priority

- Parking Lot
- Ride Sharing System (Uber/Lyft)
- Job Scheduler
- Unix File Search
- Movie Ticket / Seat Reservation System
- Amazon Locker System
- Meeting Room Scheduler

---

## Medium Priority

- Food Delivery System
- Hotel Reservation System
- Inventory Management System
- Event Processing Framework
- Car Rental System
- Notification Framework
- Payment Processing System
- Splitwise

---

## Low Priority

- Elevator System
- Library Management System
- ATM
- Vending Machine
- Logger Framework
- Download Manager
- Online Shopping Cart
- Airline Reservation System
- Chess
- Snake & Ladder

---

# LLD Interview Advice

- Start with requirements. A wrong assumption can derail the entire design.
- Spend most of your time on **entities**, **relationships**, and **workflows**. That's where most interview value lies.
- Don't force design patterns. Use them only when they solve a genuine extensibility problem.
- Always discuss concurrency whenever shared resources exist (e.g., seats, jobs, rides, inventory, lockers).
- Think about what business logic might change in the future and abstract only those parts.
- Keep the design simple. Interviewers generally prefer a clean design with sensible trade-offs over an overengineered solution.
- Communicate your thought process continuously instead of silently drawing diagrams.
- If you get stuck, return to the framework:

  ```text
  Requirements
      ↓
  Entities
      ↓
  Relationships
      ↓
  Strategies
      ↓
  Workflow
      ↓
  Concurrency
      ↓
  Extensibility
  ```

- Focus on demonstrating **structured thinking**, not perfect UML diagrams or every design pattern you've learned.

---

# Key Takeaway

> **Most candidates fail LLD interviews because of a lack of structure, not a lack of knowledge.**
>
> A clear, organized approach often matters more than the final design itself.

# HLD Preparation Strategy & Interview Questions

> **Source:** Reddit  
> **User:** u/user9908807

---

> **Note:** This contains a preparation plan and question list. Use ChatGPT to customize it according to the role (SDE-1/SDE-2) and company you're preparing for. You can also ask it to generate complete HLD solutions following this framework.

---

# How to Think About HLD

Think of HLD as:

> **"How would I build this system simply and then scale this system for many users?"**

**Not:**

- Buzzword dumping
- Distributed systems thesis
- Every cloud service ever created

## Your Goals

- Structured thinking
- Scalability awareness
- Reasonable trade-offs

---

# HLD Interview Framework

Use this exact flow for every HLD interview question.

---

## 1. Clarify Requirements

Ask:

- What are the core features?
- What is the read/write ratio?
- Are there any real-time requirements?
- What are the consistency requirements?
- What is the expected scale?

---

## 2. Estimate Scale

Keep the estimates rough.

Examples:

- Daily Active Users (DAU)
- Queries Per Second (QPS)
- Storage requirements
- Bandwidth requirements

> The goal is to demonstrate awareness, not perfect calculations.

---

## 3. Define APIs

Examples:

```http
POST /orders
POST /messages
GET  /notifications
```

Simple REST APIs are sufficient.

---

## 4. Draw the Core Architecture

A typical architecture consists of:

- Client
- Load Balancer
- Services
- Cache
- Database
- Queue

---

## 5. Choose the Database

Ask yourself:

> **"What type of data am I storing?"**

Examples:

- **SQL** → Transactions
- **NoSQL** → Feeds / Chats
- **Redis** → Cache
- **Object Storage** → Files

Always explain **why** you chose a particular storage solution.

---

## 6. Discuss the Scaling Strategy

Focus on:

- Caching
- Replication
- Sharding
- Queues
- Asynchronous Workers

> This is typically the **highest ROI** section of an HLD interview.

---

## 7. Deep Dive into One Challenge

Choose **one** interesting engineering problem and discuss it in depth.

Examples:

- Seat locking
- Message ordering
- Ride matching
- Notification fanout

---

## 8. Reliability

Always mention:

- Retries
- Dead Letter Queue (DLQ)
- Idempotency
- Monitoring

---

## 9. Discuss Trade-offs

Always compare alternatives.

Examples:

- SQL vs NoSQL
- Consistency vs Availability
- Simplicity vs Scalability

---

# HLD Topics to Know

Focus on concepts that repeatedly appear in interviews.

---

## Core Components

- Load Balancers
- Caches
- Databases
- Queues
- Object Storage

---

## Scaling

- Replication
- Sharding
- Asynchronous Processing

---

## Reliability

- Retries
- Dead Letter Queue (DLQ)
- Idempotency

---

## Real-Time Systems

- WebSockets
- Long Polling

---

## Common Themes

- Notifications
- Search
- Scheduling
- Reservations
- Geospatial Systems

---

# HLD Question List

## High Priority

- URL Shortener
- BookMyShow
- Rate Limiter
- Notification System
- Chat System
- E-Commerce Platform
- Uber
- File Storage System

---

## Medium Priority

- Food Delivery
- Package Tracking
- Search Autocomplete
- Queue Processing System
- Distributed Job Scheduler
- Search System
- Logging & Monitoring System
- API Gateway
- Distributed Cache

---

## Low Priority

- Google Docs
- Recommendation System
- Social Feed / News Feed
- Video Streaming Platform
- Web Crawler
- Analytics / Event Tracking System

---

# Final Advice

Don't try to become a distributed systems expert before your interviews.

Most candidates benefit far more from:

- A repeatable framework
- Clear communication
- Clean diagrams
- Reasonable trade-offs

than from memorizing every architecture pattern or distributed systems concept.

---

# Key Takeaway

> **Strong HLD interviews are less about knowing every technology and more about demonstrating structured system design thinking.**
>
> Start simple, justify your decisions, discuss trade-offs, and evolve the design as scale increases.
