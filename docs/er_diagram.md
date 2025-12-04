```
erDiagram
    USER {
    UUID id PK
    string name
    string email
    string password
    string role  // ADMIN, DOCTOR, PATIENT
    string phone
    boolean verified
    datetime created_at
    }

    DOCTOR {
        UUID id PK
        UUID user_id FK
        string department_id FK
        string specialization
        string designation
        text qualifications
        string license_number
        boolean approved
        datetime created_at
    }

    PATIENT {
        UUID id PK
        UUID user_id FK
        date date_of_birth
        string gender
        string blood_group
        text address
        datetime created_at
    }

    DEPARTMENT {
        UUID id PK
        string name
        text description
    }

    SCHEDULE {
        UUID id PK
        UUID doctor_id FK
        date date
        time start_time
        time end_time
        integer slot_limit
        integer booked_count
    }

    APPOINTMENT {
        UUID id PK
        UUID doctor_id FK
        UUID patient_id FK
        UUID schedule_id FK
        string status  // PENDING, CONFIRMED, COMPLETED, CANCELLED
        datetime appointment_time
        decimal fee
        datetime created_at
    }

    PRESCRIPTION {
        UUID id PK
        UUID appointment_id FK
        UUID doctor_id FK
        UUID patient_id FK
        text notes
        string pdf_path
        datetime created_at
    }

    PRESCRIPTION_MEDICINE {
        UUID id PK
        UUID prescription_id FK
        UUID medicine_id FK
        string dosage
        string duration
        string instructions
    }

    PRESCRIPTION_TEST {
        UUID id PK
        UUID prescription_id FK
        UUID test_id FK
        string instructions
    }

    MEDICINE {
        UUID id PK
        string name
        string type
        string manufacturer
        text description
    }

    TEST {
        UUID id PK
        string name
        text description
    }

    REPORT {
        UUID id PK
        UUID patient_id FK
        string file_name
        string file_path
        string file_type
        datetime uploaded_at
    }

    PAYMENT {
        UUID id PK
        UUID appointment_id FK
        decimal amount
        string method  // e.g., Cash, bKash, etc.
        string status  // INITIATED, COMPLETED, FAILED
        datetime created_at
    }

    NOTIFICATION {
        UUID id PK
        UUID user_id FK
        string type  // EMAIL, SMS, SYSTEM
        string message
        boolean read
        datetime created_at
    }

    AUDIT_LOG {
        UUID id PK
        UUID user_id FK
        string action
        string entity
        string entity_id
        datetime timestamp
        text details
    }

    %% Relationships
    USER ||--o{ DOCTOR : "is"
    USER ||--o{ PATIENT : "is"
    DOCTOR }o--|| DEPARTMENT : "belongs to"
    DOCTOR ||--o{ SCHEDULE : "defines"
    SCHEDULE ||--o{ APPOINTMENT : "books"
    PATIENT ||--o{ APPOINTMENT : "requests"
    DOCTOR ||--o{ APPOINTMENT : "receives"
    APPOINTMENT ||--|| PRESCRIPTION : "generates"
    PRESCRIPTION ||--o{ PRESCRIPTION_MEDICINE : "contains"
    PRESCRIPTION ||--o{ PRESCRIPTION_TEST : "includes"
    MEDICINE ||--o{ PRESCRIPTION_MEDICINE : "is part of"
    TEST ||--o{ PRESCRIPTION_TEST : "is part of"
    PATIENT ||--o{ REPORT : "uploads"
    APPOINTMENT ||--|| PAYMENT : "has"
    USER ||--o{ NOTIFICATION : "receives"
    USER ||--o{ AUDIT_LOG : "performs"
```