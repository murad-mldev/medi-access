### **Non-Functional Requirements**

#### **Performance**

* The system must maintain consistent performance, with average API response time under 2 seconds under normal load.
* It should support at least 1000 concurrent active users as a baseline and be horizontally scalable to handle higher loads without significant degradation.

#### **Scalability**

* The system should be designed with a modular architecture to allow future expansion and scaling.
* Components should be loosely coupled to enable independent deployment and scaling.

#### **Security**

* All sensitive information such as passwords and tokens must be encrypted using secure standards (e.g., bcrypt for passwords, JWT for tokens).
* Role-based access control must ensure that users can only access authorized modules and data.
* The system should prevent common vulnerabilities such as SQL injection, XSS, and CSRF attacks.

#### **Usability**

* The user interface should be intuitive, consistent, and responsive across desktop and mobile browsers.
* Navigation should be simple, with clear error messages and helpful feedback to users.

#### **Maintainability**

* The codebase should be modular and well-documented for easy maintenance and feature extension.
* Configuration parameters should be externalized for environment flexibility.

#### **Reliability**

* The system should recover gracefully from unexpected shutdowns and resume normal operations without data loss.
* Error handling should be implemented throughout to ensure consistent system behavior.

#### **Data Integrity**

* The database must enforce referential integrity and prevent inconsistent records.
* All create, update, and delete operations should be transactional to maintain data accuracy.

#### **Auditability**

* The system should log key activities such as prescription updates, appointment modifications, and schedule changes.
* Admins should be able to review logs for transparency and debugging.

#### **Availability**

* The system should maintain at least 99% uptime during working hours.
* Planned downtime should be communicated in advance.
