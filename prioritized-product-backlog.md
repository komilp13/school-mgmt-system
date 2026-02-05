# Prioritized Product Backlog

## 🏗️ Epic 1: Infrastructure & Core Architecture

*Goal: Establish the foundation for a scalable, full-stack application with a clear separation of concerns.*

- **Story 1.1: Backend Foundation (Node.js & Express/NestJS)**
  - **Description:** Set up a Node.js REST API with TypeScript. This includes configuring the project structure, environment variables (`.env`), and a middleware pipeline for logging (Winston/Morgan) and error handling.
- **Story 1.2: Database Schema & Migrations (PostgreSQL)**
  - **Description:** Initialize the PostgreSQL database using an ORM like Prisma or Sequelize. Create the initial migration scripts to establish core tables and relationships.
- **Story 1.3: Frontend Core (Next.js & Tailwind CSS)**
  - **Description:** Initialize a Next.js project using the App Router. Configure Tailwind CSS for styling and set up a global state management solution (e.g., Zustand or React Context).
- **Story 1.4: Containerization (Docker)**
  - **Description:** Create a `docker-compose.yml` file to orchestrate the Node.js API, PostgreSQL database, and a PGAdmin instance for local development.

------

## 👥 Epic 2: Authentication & Role-Based Access Control (RBAC)

*Goal: Ensure secure, tiered access for Admins, Teachers, Parents, and Students.*

- **Story 2.1: JWT-Based Authentication**
  - **Description:** Implement secure login/logout using JSON Web Tokens (JWT). Include password hashing (bcrypt) and refresh token logic in the Node.js API.
- **Story 2.2: Permission Middleware**
  - **Description:** Develop a server-side middleware that checks user roles against requested routes to ensure data confidentiality.
- **Story 2.3: User Profile Management**
  - **Description:** Build a Next.js interface for users to update their personal profiles and reset passwords via email verification.

------

## 🎒 Epic 3: Student Information System (SIS) & Admissions

*Goal: Centralize student lifecycle management from application to graduation.*

- **Story 3.1: Digital Admissions Pipeline**
  - **Description:** Build a public-facing Next.js form for new applicants to upload documents and submit demographics. Add an admin dashboard to track "Applied," "Interviewed," and "Enrolled" statuses.
- **Story 3.2: Comprehensive Student Records**
  - **Description:** Create a "Student 360" view displaying academic history, health records, emergency contacts, and disciplinary logs.
- **Story 3.3: (Added) Alumni Portal**
  - **Description:** A module to transition graduated students into an alumni database for networking and school event notifications.

------

## 🍎 Epic 4: Academic & Learning Management (LMS)

*Goal: Facilitate digital education delivery and automated grading.*

- **Story 4.1: Course & Curriculum Management**
  - **Description:** Allow administrators to define subjects, departments, and syllabi that teachers can then link to their classes.
- **Story 4.2: Digital Gradebook & Transcripts**
  - **Description:** Implement a marks-entry system with weighted averages. Add a feature to generate PDF transcripts and report cards with one click.
- **Story 4.3: Assignment & Quiz Engine**
  - **Description:** Enable teachers to upload digital materials and create online quizzes. Students can submit assignments, which teachers grade through a dedicated dashboard.

------

## 📅 Epic 5: Operations: Attendance & Timetables

*Goal: Manage the physical logistics of the school day.*

- **Story 5.1: Conflict-Free Timetable Generator**
  - **Description:** Build an algorithm that assigns teachers and rooms to time slots, preventing double-bookings and respecting teacher availability.
- **Story 5.2: Smart Attendance Tracking**
  - **Description:** Create a mobile-responsive UI for teachers to take attendance. Include child features for QR code scanning or RFID integration for automated entry logs.

------

## 💳 Epic 6: Financial & Operational Tools

*Goal: Handle school revenue, inventory, and logistics.*

- **Story 6.1: Automated Fee Invoicing & Stripe Integration**
  - **Description:** Set up a system to generate invoices based on grade-level fees. Integrate a payment gateway like Stripe or PayPal for parent payments.
- **Story 6.2: Library Inventory & Circulation**
  - **Description:** Build a catalog where students can search for books. Include automated logic to flag overdue items and calculate fines.
- **Story 6.3: (Added) Laboratory & Asset Tracking**
  - **Description:** A child feature of Inventory Management to track specific high-value assets like lab equipment or laptops.

------

## 🧪 Epic 7: Quality Assurance & Testing

*Goal: Ensure system reliability across the entire tech stack.*

- **Story 7.1: Unit Testing (Jest)**
  - **Description:** Implement unit tests for core business logic in the Node.js API (e.g., fee calculation logic, GPA math).
- **Story 7.2: Integration Testing (Supertest)**
  - **Description:** Test API endpoints to ensure the REST API correctly interacts with the PostgreSQL database.
- **Story 7.3: End-to-End (E2E) Testing (Playwright/Cypress)**
  - **Description:** Script critical user paths in the Next.js frontend, such as "Parent pays fee" or "Teacher submits grades," to prevent regressions.
- **Story 7.4: (Added) Load Testing**
  - **Description:** Use tools like k6 to simulate high traffic during peak times (e.g., report card release day) to ensure the Node.js API remains responsive.

---

---

## 🏗️ Epic 1: Infrastructure & Core

These endpoints handle the "plumbing" of the system.

- **`GET /api/v1/health`**: Performs a deep health check, verifying the connection to the **PostgreSQL** database and the status of background workers.
- **`GET /api/v1/config`**: Provides the **Next.js** frontend with necessary public environment configurations (e.g., supported locales, theme settings).

------

## 👥 Epic 2: Auth & Role-Based Access Control (RBAC)

Focuses on secure identity management.

- **`POST /api/v1/auth/login`**: Accepts credentials; returns a short-lived **JWT** and sets a `HttpOnly` refresh cookie.
- **`POST /api/v1/auth/refresh`**: Exchanges a refresh token for a new JWT to maintain session persistence.
- **`GET /api/v1/users/me`**: Retrieves the authenticated user's profile and their specific **RBAC** permissions for frontend UI rendering.
- **`PATCH /api/v1/users/profile`**: Allows users to update their personal details and security settings.

------

## 🎒 Epic 3: Student Information System (SIS) & Admissions

Managing the core entities of the school.

- **`POST /api/v1/admissions/apply`**: A public endpoint for prospective students to submit applications and upload documents.
- **`GET /api/v1/admissions/applications`**: Admin-only; lists applications with filtering by status (e.g., "Pending", "Interviewed").
- **`GET /api/v1/students/:id`**: Returns a comprehensive **Student 360** view, including demographics, health history, and attendance.
- **`POST /api/v1/students/:id/incidents`**: Logs disciplinary notes or behavioral merits.

------

## 🍎 Epic 4: Academic & Learning Management (LMS)

Endpoints for the digital classroom.

- **`GET /api/v1/courses`**: Lists available courses; teachers see their assigned classes, students see their enrolled ones.
- **`POST /api/v1/assessments`**: Allows teachers to create quizzes or upload homework tasks.
- **`POST /api/v1/grades/batch`**: Enables teachers to input grades for an entire class at once.
- **`GET /api/v1/reports/transcripts/:studentId`**: Generates a PDF transcript based on the **PostgreSQL** grade records.

------

## 📅 Epic 5: Operations: Attendance & Timetables

Managing the daily schedule.

- **`GET /api/v1/timetables/:role/:id`**: Fetches the weekly schedule for a specific user or classroom.
- **`POST /api/v1/attendance/mark`**: Records attendance for a specific session; supports bulk entry or individual QR/RFID scans.
- **`GET /api/v1/attendance/analytics`**: Provides trends on student/staff presence for administrative dashboards.

------

## 💳 Epic 6: Financial & Operational Tools

Handling transactions and school assets.

- **`GET /api/v1/finance/invoices`**: Parents see their child's outstanding fees; Admins see global revenue status.
- **`POST /api/v1/finance/checkout`**: Initiates a payment session (e.g., **Stripe Checkout**) for specific invoice items.
- **`GET /api/v1/library/books`**: Searchable catalog for the school library with real-time "Available/Reserved" status.
- **`PATCH /api/v1/library/checkout/:bookId`**: Updates the book status and records the borrower's ID in **PostgreSQL**.

------

## 🧪 Epic 7: Quality Assurance & Testing

While not "endpoints" in the traditional sense, these involve specialized API testing hooks.

- **`POST /api/v1/test/seed`**: (Development Only) A restricted endpoint used by **Playwright/Cypress** to reset the database to a known state before E2E tests run.
- **`GET /api/v1/test/metrics`**: Exposes internal performance metrics to load-testing tools like **k6**.

------

### 🗄️ Database Architecture Note

Since you are using **PostgreSQL**, I recommend implementing a **Soft Delete** pattern for these endpoints (using a `deleted_at` timestamp) to ensure that accidental deletions of critical student or financial records can be recovered.



---

---



### Priority 1: Epic 1: Core Infrastructure Setup and Deployment

*The foundational technical work to establish the Next.js, PostgreSQL, and Vercel environment.*

- **Story 1.1: Initialize Next.js Project and GitHub Repo**
  - **Description:** As a developer, I want to initialize a new Next.js project and push it to a new GitHub repository, so that we have the codebase foundation and version control in place.
- **Story 1.2: Set up PostgreSQL Database**
  - **Description:** As a developer, I want to provision a PostgreSQL database instance (e.g., via Vercel Storage or an external provider), so that we have a secure, persistent data store for the application.
- **Story 1.3: Configure Vercel Deployment Pipeline**
  - **Description:** As a DevOps engineer, I want to connect the GitHub repository to Vercel and configure automatic deployments, so that every push to the main branch triggers a live update.
- **Story 1.4: Implement Initial Data Model (Prisma)**
  - **Description:** As a backend developer, I want to integrate an ORM (like Prisma) and define basic user schemas (Admin, Teacher, Student), so that we can interact with the database using clean code.

------

### Priority 2: Epic 2: User Authentication & Role Management

*Enables secure access and different permissions for various user types.*

- **Story 2.1: User Registration**
  - **Description:** As a new user, I want to register for an account using my email and password, so that I can securely access the system.
- **Story 2.2: User Login & Session Management**
  - **Description:** As a registered user, I want to log in and maintain a secure session, so that I don't have to re-enter credentials frequently.
- **Story 2.3: Password Reset Functionality**
  - **Description:** As a user who forgot my password, I want to securely reset it via a link sent to my email, so that I can regain access to my account.
- **Story 2.4: Role-Based Route Protection**
  - **Description:** As an administrator, I want to ensure certain pages (e.g., admin dashboard) are only accessible by users with the `admin` role, so that sensitive data and functionality are protected.

------

### Priority 3: Epic 3: Student Information System (SIS) - Basic Module

*Focuses on core student data management, which is a primary functional requirement.*

- **Story 3.1: Admin Views All Students**
  - **Description:** As an admin, I want to view a list of all enrolled students, so that I can oversee the student population.
- **Story 3.2: Admin Creates New Student Record**
  - **Description:** As an admin, I want to create a new student record manually, so that I can enroll a new student into the system.
- **Story 3.3: Parent Views Child's Basic Info**
  - **Description:** As a parent, I want to log in and see my child’s basic profile information, so that I can confirm their details are correct.

------

### Priority 4: Epic 4: Quality Assurance & Testing

*Ensures the system is stable, functional, and secure. This runs alongside the other epics but is prioritized here to emphasize setting up testing early.*

- **Story 4.1: End-to-End Testing Framework Setup**
  - **Description:** As a QA engineer, I want to set up an E2E testing framework (e.g., Cypress or Playwright), so that we can automate testing of core user flows.
- **Story 4.2: Unit & Integration Test Implementation**
  - **Description:** As a developer, I want to write unit and integration tests for core API routes (e.g., login, create student), so that we ensure code changes don't break existing functionality.
- **Story 4.3: Manual UAT for Core User Journeys**
  - **Description:** As a QA tester, I want to manually verify the core user journeys (registration, login, admin student view) on staging, so that we confirm all acceptance criteria are met before production release.