# 🏥 **Telemedicine Platform – Requirement Gathering Document**

### **1. Overview**

The **Telemedicine Platform** aims to provide remote healthcare services to patients, doctors, and administrators through a secure, user-friendly web application.
It enables patients to consult doctors online, manage prescriptions and reports, while allowing doctors to handle appointments and digital prescriptions efficiently. The admin oversees user management, verification, and system configurations.

---

## **2. Stakeholders**

* **Patients** – Register, book appointments, upload reports, and attend consultations.
* **Doctors** – Manage profiles, schedules, patient records, and issue prescriptions.
* **Admin** – Verify doctors, manage system data (departments, medicines, etc.), and monitor system activity.

---

## **3. Functional Requirements**

### **3.1 Authentication & Authorization**

| ID   | Requirement               | Description                                                          |
| ---- | ------------------------- | -------------------------------------------------------------------- |
| FR-1 | User Registration & Login | Patients and doctors can register and log in securely.               |
| FR-2 | Role-based Access         | Different features and UI visibility for Admin, Doctor, and Patient. |
| FR-3 | JWT-based Authentication  | Use Spring Security + JWT for token-based session management.        |
| FR-4 | Admin Verification        | Admin verifies doctor credentials before activation.                 |

---

### **3.2 Doctor Module**

| ID    | Requirement             | Description                                                                    |
| ----- | ----------------------- | ------------------------------------------------------------------------------ |
| FR-5  | Profile Management      | Doctors can update personal info, department, specialization, and designation. |
| FR-6  | Appointment Schedule    | Doctors can set available time slots and patient capacity per day.             |
| FR-7  | View Patients & History | Doctors can view list of assigned patients and their medical history.          |
| FR-8  | Prescription Creation   | Doctors can generate or upload prescriptions in A4 PDF format.                 |
| FR-9  | Medicine/Test Selection | Doctors select medicines and tests from an extendable database.                |
| FR-10 | Video Consultation      | Conduct one-to-one secure online consultations (WebRTC or Zoom integration).   |

---

### **3.3 Patient Module**

| ID    | Requirement         | Description                                                        |
| ----- | ------------------- | ------------------------------------------------------------------ |
| FR-11 | Profile Management  | Patients can register, log in, and update profiles.                |
| FR-12 | Doctor Search       | Patients can search by department, specialization, or doctor name. |
| FR-13 | Appointment Booking | Patients can view doctor schedules and book available slots.       |
| FR-14 | Report Upload       | Patients upload medical reports (PDF, image).                      |
| FR-15 | Video Consultation  | Patients can join secure consultation sessions.                    |
| FR-16 | View Prescriptions  | Patients can view/download prescriptions and medical history.      |

---

### **3.4 Admin Module**

| ID    | Requirement         | Description                                                          |
| ----- | ------------------- | -------------------------------------------------------------------- |
| FR-17 | Manage Users        | Admin can manage patients and doctors.                               |
| FR-18 | Doctor Verification | Admin validates doctors’ credentials before activation.              |
| FR-19 | Manage Data         | Admin can manage departments, medicines, and report/test lists.      |
| FR-20 | View Statistics     | Admin can monitor appointments, active users, and system metrics.    |
| FR-21 | Audit Logs          | Track key activities (prescription updates, schedule changes, etc.). |

---

### **3.5 Appointment & Scheduling System**

| ID    | Requirement        | Description                                                                         |
| ----- | ------------------ | ----------------------------------------------------------------------------------- |
| FR-22 | Appointment Status | Appointments have statuses: PENDING, CONFIRMED, COMPLETED, CANCELLED.               |
| FR-23 | Limit Control      | Patients can book only if doctor’s slot limit isn’t exceeded.                       |
| FR-24 | Notifications      | Email/SMS alerts for confirmations, cancellations, reminders. *(Simulated in demo)* |

---

### **3.6 Prescription & Report Management**

| ID    | Requirement          | Description                                                                 |
| ----- | -------------------- | --------------------------------------------------------------------------- |
| FR-25 | Prescription History | Doctors and patients can view all past prescriptions.                       |
| FR-26 | Report Storage       | Uploaded reports stored securely and linked with patient record.            |
| FR-27 | Auto-generated PDF   | A4 printable PDF including doctor/patient info, date, medicines, and notes. |

---

### **3.7 Payments (Optional)**

| ID    | Requirement      | Description                                                |
| ----- | ---------------- | ---------------------------------------------------------- |
| FR-28 | Payment Record   | Maintain basic payment records (Cash or bKash simulation). |
| FR-29 | Consultation Fee | Each doctor can define consultation fees.                  |

---

### **3.8 Search, Filter & UI Enhancements**

| ID    | Requirement            | Description                                                             |
| ----- | ---------------------- | ----------------------------------------------------------------------- |
| FR-30 | Search & Filter        | Patients can filter doctors by specialization, department, or location. |
| FR-31 | Pagination & Sorting   | For doctor/patient/appointment lists.                                   |
| FR-32 | Availability Indicator | Show doctor’s online status or next available slot.                     |
| FR-33 | Multi-language Support | English/Bangla language toggle (UI level).                              |

---

## **4. Non-Functional Requirements**

| Category            | Requirement | Description                                                                                      |
| ------------------- |-------------| ------------------------------------------------------------------------------------------------ |
| **Performance**     | NFR-1       | The system should handle up to 1000 concurrent users without performance degradation.            |
| **Security**        | NFR-2       | All sensitive data (passwords, tokens) must be encrypted using industry standards (bcrypt, JWT). |
| **Usability**       | NFR-3       | The UI should be intuitive and responsive across desktop and mobile browsers.                    |
| **Maintainability** | NFR-4       | Codebase structured for easy updates, feature additions, and debugging.                          |
| **Reliability**     | NFR-5       | The system should handle unexpected shutdowns and resume normal operations.                      |
| **Data Integrity**  | NFR-6       | Database must enforce foreign key constraints and prevent inconsistent records.                  |
| **Auditability**    | NFR-7       | Maintain activity logs for admin auditing and issue tracking.                                    |
| **Availability**    | NFR-8       | System uptime target of 99% during working hours.                                                |

---


