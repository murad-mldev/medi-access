# High-Level System Design – Telemedicine Platform

### 1. Architectural Overview

The system follows a **Monolithic Modular Architecture**, where all core modules reside within a single deployable application but are organized into **independent functional modules** with well-defined boundaries.
Each module communicates internally through **service calls or event publishing**, and all share the same **relational database**.

---

### 2. Core Architectural Style

| Layer                  | Description                                                                                                                              |
| ---------------------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| **Presentation Layer** | Web and mobile clients interact with the backend through RESTful APIs over HTTPS.                                                        |
| **Application Layer**  | Contains business services grouped by domain (Auth, Appointment, Video, etc.). Handles request orchestration and transaction management. |
| **Domain Layer**       | Defines business logic, models, and entity relationships.                                                                                |
| **Data Access Layer**  | Responsible for persistence, communicating with a relational database using repositories.                                                |
| **Integration Layer**  | Handles communication with external systems (email/SMS, payment gateways, file storage, STUN/TURN servers).                              |

---

### 3. High-Level Component Interaction

```
        ┌─────────────────────┐
        │     User Interface  │
        │         (Web)       │
        └──────────┬──────────┘
                   │ REST API Calls
                   ▼
        ┌────────────────────────────┐
        │ DoctorConnectBD Backend    │  (Monolithic Modular App)
        │────────────────────────────│
        │                            │
        │ 1. Authentication Module   │───► User DB (verify/login)
        │                            │
        │ 2. User Management Module  │───► Doctor/Patient Tables
        │                            │
        │ 3. Appointment Module      │───► Appointment & Schedule Tables
        │                            │
        │ 4. Prescription Module     │───► Prescription Records / Files
        │                            │
        │ 5. File Storage Module     │───► File System / Cloud Storage
        │                            │
        │ 6. Video/Consultation Mod. │───► STUN/TURN Server
        │                            │
        │ 7. Notification Module     │───► Email/SMS Gateway
        │                            │
        │ 8. Admin Module            │───► Audit / Verification Data
        │                            │
        └──────────┬─────────────────┘
                   │
                   ▼
        ┌────────────────────────────┐
        │    Relational Database     │
        │       (PostgreSQL)         │
        └────────────────────────────┘
```

---

### 4. Component Responsibilities & Interactions

#### **1. Authentication & Authorization**

* Handles login, signup, and session management.
* Validates credentials and issues access tokens.
* Shares user identity with other modules via session context.
* Interfaces:

    * Input: `LoginRequest`, `SignupRequest`
    * Output: Authentication token, user info
    * Interacts with: User Management Module, Database (Users table)

---

#### **2. User Management Module**

* Manages doctor and patient profiles.
* Doctors submit credentials for verification.
* Admin module can verify doctors.
* Interfaces:

    * Input: Profile creation/update requests
    * Output: Profile details, verification status
    * Interacts with: Auth Module, Admin Module, Database

---

#### **3. Appointment Management Module**

* Provides scheduling, booking, and cancellation.
* Validates slot availability and prevents double bookings.
* Sends notifications upon successful booking.
* Interfaces:

    * Input: Booking requests
    * Output: Appointment confirmation or failure message
    * Interacts with: Notification Module, Database (Appointment tables)

---

#### **4. Prescription Management Module**

* Enables doctors to create prescriptions post-consultation.
* Stores digital prescriptions and generates downloadable files.
* Interfaces:

    * Input: Prescription data from doctor
    * Output: Stored prescription record / downloadable link
    * Interacts with: File Storage Module, Database

---

#### **5. File Storage & Report Module**

* Manages uploads of medical reports, prescriptions, and verification documents.
* Stores files in local or cloud storage.
* Provides secure, role-based file access.
* Interfaces:

    * Input: Multipart uploads
    * Output: File metadata / access link
    * Interacts with: Database, Storage System

---

#### **6. Video Consultation Module**

* Manages 1:1 consultation sessions.
* Handles signaling for WebRTC/Meet API and user session validation.
* Uses external STUN/TURN server for peer connectivity.
* Interfaces:

    * Input: Session create/join requests
    * Output: Connection tokens / room info
    * Interacts with: Appointment Module, Auth Module, Meet API

---

#### **7. Notification Module**

* Sends emails or SMS notifications for appointment confirmations, reminders, and verifications.
* Triggered by domain events (e.g., booking confirmation).
* Interfaces:

    * Input: Event payloads (e.g., appointment.created)
    * Output: Email/SMS sent
    * Interacts with: Appointment Module, External Notification API

---

#### **8. Admin Module**

* Provides operational control: doctor verification, user management, system monitoring.
* Can view and manage audit logs.
* Interfaces:

    * Input: Admin commands
    * Output: Updated system state
    * Interacts with: User Module, Audit DB

---

### 5. Data Flow Example – Appointment Booking

1. **Patient UI** → sends booking request → `/appointments`
2. **Appointment Module**

    * Validates authentication (Auth Module)
    * Checks doctor availability (DB query)
    * Reserves slot and creates record
    * Publishes event `appointment.created`
3. **Notification Module**

    * Receives event → sends email/SMS confirmation
4. **Database**

    * Stores appointment data and related entities
5. **Patient UI**
    * Receives confirmation response

---

### 6. Database & Storage Design

* **Database Type:** Relational Database (PostgreSQL)
* **Reason:** Strong consistency required for transactions (booking, payment, verification)
* **Schema Highlights:**

    * `users`, `doctors`, `patients`
    * `appointments`, `schedules`
    * `prescriptions`, `reports`
    * `audit_logs`
* **Storage:**

    * Files and prescriptions stored in file system / cloud storage (e.g., S3)
    * References stored as URLs or file keys in DB

---

### 7. Communication Patterns

* **Internal Communication:** In-process service calls (monolith modules)
* **External Communication:** REST APIs, WebSocket (video signaling), SMTP/SMS (notifications)
* **Data Exchange Format:** JSON for APIs, multipart/form-data for uploads

---

### 9. Non-Functional Highlights

| Aspect              | Approach                                                                      |
| ------------------- | ----------------------------------------------------------------------------- |
| **Performance**     | Database indexing, async tasks for long operations (e.g., notifications, PDF) |
| **Scalability**     | Horizontal scaling with multiple monolith instances                           |
| **Availability**    | Single DB cluster, stateless backend                                          |
| **Security**        | Role-based access control, token auth, HTTPS enforced                         |
| **Reliability**     | ACID transactions for booking                                                 |
| **Maintainability** | Modular code structure; separation by domains                                 |

---
