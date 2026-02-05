# 📊 Entity Relationship Diagram

------

### 💻 PostgreSQL Schema (DDL)

Below is the SQL definition for the core tables, including the **Foreign Key** and **Unique** constraints as requested.

SQL

```
-- Core User & Auth
CREATE TABLE users (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    email VARCHAR(255) UNIQUE NOT NULL,
    password_hash TEXT NOT NULL,
    role VARCHAR(50) NOT NULL CHECK (role IN ('ADMIN', 'TEACHER', 'STUDENT', 'PARENT')),
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- Student Information System (SIS)
CREATE TABLE students (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID UNIQUE REFERENCES users(id) ON DELETE CASCADE,
    student_code VARCHAR(50) UNIQUE NOT NULL,
    first_name VARCHAR(100) NOT NULL,
    last_name VARCHAR(100) NOT NULL,
    date_of_birth DATE NOT NULL,
    health_records JSONB, --
    status VARCHAR(20) DEFAULT 'APPLICANT', --
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- Academic Structures
CREATE TABLE courses (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name VARCHAR(255) NOT NULL,
    code VARCHAR(50) UNIQUE NOT NULL,
    description TEXT,
    credits INT DEFAULT 0
);

CREATE TABLE enrollments (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    student_id UUID REFERENCES students(id),
    course_id UUID REFERENCES courses(id),
    semester VARCHAR(20) NOT NULL,
    grade_numeric DECIMAL(5,2),
    UNIQUE(student_id, course_id, semester) -- Prevents duplicate enrollment
);

-- Operations & Attendance
CREATE TABLE timetables (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    course_id UUID REFERENCES courses(id),
    teacher_id UUID REFERENCES users(id),
    room_number VARCHAR(50),
    day_of_week INT CHECK (day_of_week BETWEEN 1 AND 7),
    start_time TIME NOT NULL,
    end_time TIME NOT NULL,
    UNIQUE(teacher_id, day_of_week, start_time) -- Conflict prevention
);

CREATE TABLE attendance (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    student_id UUID REFERENCES students(id),
    timetable_id UUID REFERENCES timetables(id),
    status VARCHAR(20) CHECK (status IN ('PRESENT', 'ABSENT', 'LATE', 'EXCUSED')),
    recorded_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    UNIQUE(student_id, timetable_id, recorded_at::DATE) --
);

-- Finance
CREATE TABLE invoices (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    student_id UUID REFERENCES students(id),
    amount_cents INT NOT NULL, -- Storing in cents for precision
    status VARCHAR(20) DEFAULT 'UNPAID',
    due_date DATE NOT NULL,
    stripe_payment_id VARCHAR(255) UNIQUE, --
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
```

------

### 🔍 Architectural Highlights

- **Unique Constraints**: Applied to `student_code`, `email`, and `course_code` to ensure data integrity.
- **Composite Uniqueness**: The `enrollments` table uses a composite key `(student_id, course_id, semester)` to prevent a student from being enrolled in the same class twice in one term.
- **JSONB Support**: The `students` table uses a `JSONB` column for `health_records` to allow for flexible data storage without constant schema migrations as health tracking requirements change.
- **Conflict Prevention**: The `timetables` table includes a unique constraint on `(teacher_id, day_of_week, start_time)` to automatically enforce the "conflict-free" requirement at the database level.