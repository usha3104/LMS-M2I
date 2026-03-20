# Epic: Super Admin Complete Workflow (Backend)
**Project:** M2I LMS Backend  
**Priority:** High  
**Description:** Implement the Express.js backend capabilities required for the Super Admin complete workflow, including authentication, dashboard access, and management modules.  
**Dependencies:** diagrams/01_Super_Admin_Flow.puml

---

# Task 1: Authentication & Authorization
**Type:** Task  
**Summary:** Implement Super Admin authentication with email/password and OTP, including JWT middleware

**Description:** This task implements the complete authentication and authorization system for the Super Admin role in the LMS backend. It involves building a secure two-factor authentication (2FA) flow using email/password for initial login followed by OTP verification. The task includes creating the login endpoint that validates credentials and generates time-limited OTPs, the OTP verification endpoint that validates the code and issues JWT tokens, and the JWT middleware that protects all subsequent API endpoints. The authentication system implements security best practices including rate limiting (max 5 login attempts per 15 minutes), OTP expiry (5 minutes), failed attempt tracking (max 3 attempts before lockout), and role-based authorization middleware. All authenticated endpoints require a valid JWT with the "super_admin" role claim.

## Sub-task 1.1: Super Admin Login Endpoint
**Type:** Sub-task
**Description:** Build Express.js endpoint for Super Admin login with email/password validation. This is the first step in the two-factor authentication process. The endpoint validates super admin credentials against the database, generates a unique OTP (One-Time Password) with 5-minute expiry, stores the OTP reference for verification, implements rate limiting to prevent brute force attacks (max 5 attempts per 15 minutes), and triggers OTP delivery via email or SMS.
- `POST /auth/super-admin/login`
- Input: `{ email, password }`
- Output: `{ message: "OTP sent", otpReference: "uuid" }`
- Generate OTP with 5-minute expiry
- Rate limit: max 5 attempts per 15 min
**Acceptance Criteria:**
- [ ] Returns 200 with OTP reference on valid credentials
- [ ] Returns 401 on invalid credentials
- [ ] Rate limiting applied
**Estimate:** 2 days

## Sub-task 1.2: OTP Verification & JWT Issuance
**Type:** Sub-task
**Description:** Build Express.js endpoint for OTP verification and JWT issuance. This completes the two-factor authentication by validating the OTP submitted by the user against the stored reference. On successful verification, a JWT (JSON Web Token) is generated with the user's ID, email, role as "super_admin", and permissions. The endpoint tracks failed attempts (max 3) and returns 429 (Too Many Requests) after lockout. The JWT payload includes: sub (userId), email, role: "super_admin", and permissions array.
- `POST /auth/super-admin/otp/verify`
- Input: `{ otpReference, otp }`
- Output: `{ token: "jwt", expiresIn: 3600 }`
- JWT payload: `{ sub: userId, email, role: "super_admin", permissions: [] }`
- Max 3 failed attempts before lockout
**Acceptance Criteria:**
- [ ] Returns 200 with JWT on valid OTP
- [ ] Returns 401 on invalid/expired OTP
- [ ] Returns 429 after 3 failed attempts
- [ ] JWT contains role claim `super_admin`
**Estimate:** 2 days

## Sub-task 1.3: JWT Authentication Middleware
**Type:** Sub-task
**Description:** Implement two essential Express.js middleware functions for request protection. The `authenticateJWT` middleware verifies the JWT token signature using the secret key, checks token expiration, decodes the payload, and attaches user information (userId, email, role, permissions) to the request object. The `authorizeRole` middleware accepts an array of allowed roles and verifies the user's role matches one of the allowed roles, returning 403 (Forbidden) if unauthorized. These middleware protect all subsequent Super Admin endpoints.
- Middleware: `authenticateJWT`
- Middleware: `authorizeRole(roles: string[])`
- Verify JWT signature and expiry
- Attach user info to request object
**Acceptance Criteria:**
- [ ] Valid JWT allows request through
- [ ] Invalid/expired JWT returns 401
- [ ] Role verification works correctly
**Estimate:** 1 day

---

# Task 2: Dashboard
**Type:** Task  
**Summary:** Implement Super Admin dashboard access endpoint

**Description:** This task implements the main dashboard endpoint that serves as the Super Admin's command center for monitoring the entire LMS system. The dashboard provides real-time aggregated statistics including counts of total and active colleges, NetPy admins, mentors, published courses, active batches, enrolled students, and pending applications. It also provides a feed of recent activities across the platform to help the Super Admin stay informed about system events. This endpoint is protected by JWT authentication and role-based authorization, ensuring only authenticated Super Admins can access sensitive system statistics.

## Sub-task 2.1: Dashboard Endpoint
**Type:** Sub-task
**Description:** Implement the main dashboard endpoint that provides Super Admin with an overview of the entire LMS system. This endpoint aggregates data from multiple sources including total colleges (all and active), total NetPy admins, total mentors, total courses (published), total batches (active), total students, pending applications awaiting review, and recent activities across the platform. The data is used for quick decision-making and system monitoring.
- `GET /super-admin/dashboard`
- Protected by: JWT + role `super_admin`
- Output: `{ totalColleges, activeColleges, totalAdmins, totalMentors, totalCourses, totalBatches, totalStudents, pendingApplications, recentActivities }`
**Acceptance Criteria:**
- [ ] Returns summary statistics
- [ ] Protected by authentication
- [ ] Only accessible by super_admin role
**Estimate:** 1 day

---

# Task 3: College Management
**Type:** Task  
**Summary:** Implement college CRUD operations for Super Admin

**Description:** This task implements the complete college management functionality for Super Admins, enabling them to create, read, update, and manage colleges within the LMS. The Super Admin can create new colleges with unique codes, update college details such as contact information and website, and activate or deactivate college accounts. A critical feature is the cascade deactivation - when a college is deactivated, all associated NetPy Admins assigned to that college are automatically deactivated to maintain security. The task also includes a listing endpoint with pagination, status filtering, and search capabilities to easily find colleges by name or code. All operations are protected by JWT authentication and role authorization.

## Sub-task 3.1: Create College
**Type:** Sub-task
**Description:** Implement endpoint to create a new college in the LMS system. The Super Admin provides college details including name (required), unique code (required, used for identification), address, contact email, contact phone number, website URL, and established year. The system validates that the college code is unique (no duplicate codes allowed), validates required fields are present, sets default status to "active", and returns the created college with its ID. If college code already exists, returns 409 Conflict.
- `POST /super-admin/colleges`
- Protected by: JWT + role `super_admin`
- Input: `{ name, code, address, contactEmail, contactPhone, website, establishedYear }`
- Output: `{ id, name, code, status: "active" }`
- Validate unique code
**Acceptance Criteria:**
- [ ] Returns 201 with created college data
- [ ] Validates required fields
- [ ] Enforces unique college code
- [ ] Returns 409 if college code exists
**Estimate:** 1 day

## Sub-task 3.2: Update College Details
**Type:** Sub-task
**Description:** Implement endpoint to update existing college information. The Super Admin can update fields like name, address, contact email, contact phone, and website. All fields are optional in the update request. The endpoint validates that the college exists (returns 404 if not found), validates email format if email is being updated, and returns the updated college data with 200 OK status.
- `PUT /super-admin/colleges/:id`
- Protected by: JWT + role `super_admin`
- Input: `{ name?, address?, contactEmail?, contactPhone?, website? }`
**Acceptance Criteria:**
- [ ] Returns 200 with updated college
- [ ] Returns 404 if college not found
- [ ] Validates email format
**Estimate:** 1 day

## Sub-task 3.3: Activate/Deactivate College
**Type:** Sub-task
**Description:** Implement endpoint to change college status between "active" and "inactive". When a college is deactivated, the system automatically cascades the deactivation to all associated NetPy Admins assigned to that college, preventing them from accessing the system. The endpoint validates college existence (returns 404 if not found), updates the college status, and performs cascade deactivation of NetPy Admins when setting status to "inactive".
- `PATCH /super-admin/colleges/:id/status`
- Protected by: JWT + role `super_admin`
- Input: `{ status: "active" | "inactive" }`
- Cascade: Deactivating college deactivates associated NetPy Admins
**Acceptance Criteria:**
- [ ] Returns 200 with updated status
- [ ] Returns 404 if college not found
- [ ] Cascade deactivates NetPy Admins
**Estimate:** 1 day

## Sub-task 3.4: List Colleges
**Type:** Sub-task
**Description:** Implement paginated endpoint to list all colleges with filtering capabilities. Supports pagination with page number and limit (default 10, max 100), optional status filter (active/inactive), and search functionality by college name or code. Returns an array of college objects with pagination metadata (total count, current page, total pages).
- `GET /super-admin/colleges`
- Protected by: JWT + role `super_admin`
- Query: `?page=1&limit=10&status=active&search=`
**Acceptance Criteria:**
- [ ] Returns paginated list
- [ ] Supports status filter
- [ ] Supports search by name/code
**Estimate:** 1 day

---

# Task 4: NetPy Admin Management
**Type:** Task  
**Summary:** Implement NetPy Admin CRUD and assignment operations

**Description:** This task implements comprehensive NetPy Admin management capabilities for Super Admins. NetPy Admins are secondary administrators who manage specific colleges within the LMS. The Super Admin can create new NetPy Admins with auto-generated passwords and welcome emails, assign one or more colleges to each admin for management, define granular permissions controlling what actions the admin can perform (such as viewing courses, managing batches, or viewing reports), and manage admin status (active/inactive). The task also includes a listing endpoint with filtering by status and assigned college. All operations enforce validation rules and maintain referential integrity with colleges.

## Sub-task 4.1: Create NetPy Admin
**Type:** Sub-task
**Description:** Implement endpoint to create a new NetPy Admin user. The Super Admin provides email (must be unique), name, phone number, optional list of assigned colleges, and optional permissions array. The system auto-generates a temporary password, validates email uniqueness (returns 409 if exists), creates the admin record, and triggers a welcome email with login credentials. The admin is created with "active" status by default.
- `POST /super-admin/admins`
- Protected by: JWT + role `super_admin`
- Input: `{ email, name, phone, assignedColleges: [], permissions: [] }`
- Auto-generate temporary password, send welcome email
**Acceptance Criteria:**
- [ ] Returns 201 with created admin data
- [ ] Validates unique email
- [ ] Sends welcome email
- [ ] Returns 409 if email exists
**Estimate:** 1 day

## Sub-task 4.2: Assign Colleges to Admin
**Type:** Sub-task
**Description:** Implement endpoint to assign one or more colleges to a NetPy Admin. The Super Admin provides an array of college IDs that the admin will manage. The endpoint validates all college IDs exist and are currently active (inactive colleges cannot be assigned), validates admin exists (returns 404 if not found), and returns the updated admin record with assigned colleges.
- `PATCH /super-admin/admins/:id/colleges`
- Protected by: JWT + role `super_admin`
- Input: `{ colleges: [collegeId1, collegeId2] }`
- Validate college IDs exist and are active
**Acceptance Criteria:**
- [ ] Returns 200 with updated assignments
- [ ] Returns 404 if admin not found
- [ ] Validates college existence
**Estimate:** 1 day

## Sub-task 4.3: Manage Admin Permissions
**Type:** Sub-task
**Description:** Implement endpoint to update permissions for a NetPy Admin. The Super Admin provides a permissions array with values from the predefined permission set (e.g., "view_courses", "manage_batches", "view_reports"). The endpoint validates admin exists (returns 404 if not found), validates all provided permissions against the predefined set, and returns the updated admin with new permissions.
- `PATCH /super-admin/admins/:id/permissions`
- Protected by: JWT + role `super_admin`
- Input: `{ permissions: ["view_courses", "manage_batches", "view_reports"] }`
- Validate against predefined permission set
**Acceptance Criteria:**
- [ ] Returns 200 with updated permissions
- [ ] Validates against predefined permission set
- [ ] Returns 404 if admin not found
**Estimate:** 1 day

## Sub-task 4.4: List NetPy Admins
**Type:** Sub-task
**Description:** Implement paginated endpoint to list all NetPy Admins with filtering. Supports pagination (page, limit), status filter (active/inactive), and filter by assigned college ID. Returns admin list with pagination metadata including total count, current page, total pages, and each admin's basic info, assigned colleges, and permissions.
- `GET /super-admin/admins`
- Protected by: JWT + role `super_admin`
- Query: `?page=1&limit=10&status=active&collegeId=`
**Acceptance Criteria:**
- [ ] Returns paginated list
- [ ] Supports status filter
- [ ] Supports filter by assigned college
**Estimate:** 1 day

---

# Task 5: Course Management
**Type:** Task  
**Summary:** Implement master course creation, modules, evaluation criteria, and publishing

**Description:** This task implements the complete master course management system for creating and publishing courses in the LMS. Master courses are the core educational content that can be assigned to batches. The workflow involves creating a course in "draft" status, adding modules with titles, descriptions, durations, and content, setting evaluation criteria with weighted components (quiz, assignment, exam) that must sum to 100%, and finally publishing the course to make it available for batch creation. The task ensures course quality by enforcing prerequisites - a course must have at least one module and evaluation criteria defined before it can be published. A listing endpoint supports filtering by status (draft/published) and category.

## Sub-task 5.1: Create Master Course
**Type:** Sub-task
**Description:** Implement endpoint to create a new master course in the LMS. The Super Admin provides course title (required), description, category, duration, and optional prerequisites (array of course IDs). The course is created with default status "draft" meaning it's not visible to students/mentors yet. Validates required fields are present, returns 201 with created course data including generated ID.
- `POST /super-admin/courses`
- Protected by: JWT + role `super_admin`
- Input: `{ title, description, category, duration, prerequisites: [] }`
- Default status: "draft"
**Acceptance Criteria:**
- [ ] Returns 201 with created course
- [ ] Validates required fields
- [ ] Default status is "draft"
**Estimate:** 1 day

## Sub-task 5.2: Define Course Modules
**Type:** Sub-task
**Description:** Implement endpoint to add modules to a draft course. Each module includes title, description, order (for sequencing), duration, and content (flexible JSON object for module materials). The endpoint validates the course exists, ensures course is in "draft" status (cannot add modules to published courses), supports adding multiple modules in one request, and supports reordering of existing modules by updating their order values.
- `POST /super-admin/courses/:id/modules`
- Protected by: JWT + role `super_admin`
- Input: `{ modules: [{ title, description, order, duration, content: {} }] }`
- Course must be in "draft" status
**Acceptance Criteria:**
- [ ] Returns 200 with modules added
- [ ] Validates course exists
- [ ] Only allowed when course is in draft
- [ ] Supports reordering modules
**Estimate:** 2 days

## Sub-task 5.3: Set Evaluation Criteria
**Type:** Sub-task
**Description:** Implement endpoint to define how student performance will be evaluated in a course. The Super Admin provides an array of criteria objects, each with type (quiz, assignment, exam, etc.) and weight (percentage). The system validates that all weights sum to exactly 100%, validates course exists, and returns 400 if weights are invalid. This criteria is used for calculating final grades.
- `PATCH /super-admin/courses/:id/evaluation`
- Protected by: JWT + role `super_admin`
- Input: `{ criteria: [{ type: "quiz", weight: 20 }, { type: "assignment", weight: 30 }, { type: "exam", weight: 50 }] }`
- Weights must sum to 100
**Acceptance Criteria:**
- [ ] Returns 200 with evaluation criteria
- [ ] Validates weights sum to 100
- [ ] Returns 400 if weights invalid
**Estimate:** 1 day

## Sub-task 5.4: Publish Course
**Type:** Sub-task
**Description:** Implement endpoint to publish a course, making it available for batch creation and student enrollment. The Super Admin sets publish to true. The endpoint enforces prerequisites: course must have at least one module defined and evaluation criteria must be set with weights summing to 100. Returns 400 if prerequisites not met. Once published, course status changes to "published" and modules/evaluation cannot be modified.
- `PATCH /super-admin/courses/:id/publish`
- Protected by: JWT + role `super_admin`
- Input: `{ publish: true }`
- Prerequisites: At least one module + evaluation criteria set
**Acceptance Criteria:**
- [ ] Returns 200 with published status
- [ ] Validates course has modules
- [ ] Validates course has evaluation criteria
- [ ] Returns 400 if prerequisites not met
**Estimate:** 1 day

## Sub-task 5.5: List Courses
**Type:** Sub-task
**Description:** Implement paginated endpoint to list master courses with filtering. Supports pagination (page, limit), status filter (draft/published), and category filter. Returns course list with pagination metadata including each course's basic info, module count, and whether evaluation criteria is set.
- `GET /super-admin/courses`
- Protected by: JWT + role `super_admin`
- Query: `?page=1&limit=10&status=published&category=`
**Acceptance Criteria:**
- [ ] Returns paginated list
- [ ] Supports status filter
- [ ] Supports category filter
**Estimate:** 1 day

---

# Task 6: Batch Management
**Type:** Task  
**Summary:** Implement batch creation, mentor assignment, student enrollment, and scheduling

**Description:** This task implements the complete batch management system for organizing student cohorts in the LMS. A batch represents a specific instance of a course with a defined timeline and participants. The workflow involves creating a batch for a published course with a defined start date, end date, and maximum student capacity, assigning an active mentor to lead the batch, enrolling approved applicants into the batch (with enforcement of capacity limits), and setting up the class schedule with specific time slots for each module. The scheduling feature validates that modules belong to the course and prevents overlapping sessions. A listing endpoint supports filtering by status and course.

## Sub-task 6.1: Create Batch
**Type:** Sub-task
**Description:** Implement endpoint to create a new batch for a published course. The Super Admin provides batch name (required), course ID (must exist and be published), start date, end date, and maximum student capacity. Validates course exists and is published, validates date range (end date must be after start date), and sets default status to "draft". Returns 201 with created batch data.
- `POST /super-admin/batches`
- Protected by: JWT + role `super_admin`
- Input: `{ name, courseId, startDate, endDate, maxStudents }`
- Default status: "draft"
**Acceptance Criteria:**
- [ ] Returns 201 with created batch
- [ ] Validates course exists
- [ ] Validates date range
**Estimate:** 1 day

## Sub-task 6.2: Assign Mentor to Batch
**Type:** Sub-task
**Description:** Implement endpoint to assign a mentor to a batch. The Super Admin provides the mentor ID. Validates mentor exists and has "active" status, validates batch exists (returns 404 if not found), and returns the updated batch with mentor assigned. Only one mentor can be assigned per batch.
- `PATCH /super-admin/batches/:id/mentor`
- Protected by: JWT + role `super_admin`
- Input: `{ mentorId }`
- Validate mentor exists and is active
**Acceptance Criteria:**
- [ ] Returns 200 with mentor assigned
- [ ] Validates mentor exists
- [ ] Returns 404 if batch not found
**Estimate:** 1 day

## Sub-task 6.3: Add Students to Batch
**Type:** Sub-task
**Description:** Implement endpoint to enroll approved applicants into a batch. The Super Admin provides an array of student IDs. Validates all students are approved applicants (status "approved"), validates batch is not full (enforces maxStudents limit, returns 400 if exceeded), and adds students to the batch. Students must have completed the application process and been approved before enrollment.
- `PATCH /super-admin/batches/:id/students`
- Protected by: JWT + role `super_admin`
- Input: `{ studentIds: [] }`
- Validate students are approved applicants
- Enforce maxStudents limit
**Acceptance Criteria:**
- [ ] Returns 200 with students added
- [ ] Validates students are approved
- [ ] Enforces maxStudents limit
- [ ] Returns 400 if batch full
**Estimate:** 1 day

## Sub-task 6.4: Set Batch Schedule
**Type:** Sub-task
**Description:** Implement endpoint to define the class schedule for a batch. The Super Admin provides an array of schedule objects, each with dayOfWeek (1-7), startTime, endTime, and moduleId. Validates each module belongs to the batch's course, checks for overlapping sessions within the same batch, and stores the complete schedule. Multiple sessions can be scheduled per day.
- `PATCH /super-admin/batches/:id/schedule`
- Protected by: JWT + role `super_admin`
- Input: `{ schedule: [{ dayOfWeek: 1, startTime: "09:00", endTime: "11:00", moduleId }] }`
**Acceptance Criteria:**
- [ ] Returns 200 with schedule set
- [ ] Validates module belongs to course
- [ ] No overlapping sessions
**Estimate:** 1 day

## Sub-task 6.5: List Batches
**Type:** Sub-task
**Description:** Implement paginated endpoint to list batches with filtering. Supports pagination (page, limit), status filter (draft/active/completed), and course filter. Returns batch list with pagination metadata including each batch's name, course info, mentor, student count, and status.
- `GET /super-admin/batches`
- Protected by: JWT + role `super_admin`
- Query: `?page=1&limit=10&status=active&courseId=`
**Acceptance Criteria:**
- [ ] Returns paginated list
- [ ] Supports status filter
- [ ] Supports course filter
**Estimate:** 1 day

---

# Task 7: Student Applications
**Type:** Task  
**Summary:** Implement application review, document verification, interview scheduling, and approval workflow

**Description:** This task implements the complete student application processing workflow for the LMS admission process. The workflow involves listing applications with filtering by status and college, verifying applicant-submitted documents (ID proof, certificates, photos) and recording verification timestamps and notes, scheduling interviews with date/time, mode (video/in-person), and interview links, and making final approval/rejection decisions. On approval, a student record is automatically created and welcome credentials are sent. The system enforces that documents must be verified before an application can be approved, ensuring only qualified candidates are admitted. All decisions and reasons are logged for audit purposes.

## Sub-task 7.1: List Applications
**Type:** Sub-task
**Description:** Implement paginated endpoint to list student applications with filtering. Supports pagination (page, limit), status filter (pending/verified/interview_scheduled/approved/rejected), and college filter. Returns application list with pagination metadata including applicant details, applied college, status, and timestamps.
- `GET /super-admin/applications`
- Protected by: JWT + role `super_admin`
- Query: `?page=1&limit=10&status=pending&collegeId=`
**Acceptance Criteria:**
- [ ] Returns paginated list
- [ ] Supports status filter
- [ ] Supports college filter
**Estimate:** 1 day

## Sub-task 7.2: Verify Documents
**Type:** Sub-task
**Description:** Implement endpoint to verify applicant documents. The Super Admin marks documents as verified (boolean) and optionally provides notes. Records the verification timestamp (ISO 8601), stores verification notes, updates application status based on verification result, and returns the verification status. Documents typically include ID proof, educational certificates, photo, etc.
- `PATCH /super-admin/applications/:id/verify`
- Protected by: JWT + role `super_admin`
- Input: `{ verified: true, notes?: "All documents valid" }`
- Record verification timestamp
**Acceptance Criteria:**
- [ ] Returns 200 with verification status
- [ ] Records verification timestamp
- [ ] Stores verification notes
**Estimate:** 1 day

## Sub-task 7.3: Schedule Interview
**Type:** Sub-task
**Description:** Implement endpoint to schedule an interview for an applicant. The Super Admin provides scheduled date/time (must be in the future), interview mode (video/in-person), and optional interview link for video calls. Validates the scheduled date is in the future, updates application status to "interview_scheduled", creates interview record, and sends notification (email/SMS) to the applicant with interview details.
- `PATCH /super-admin/applications/:id/interview`
- Protected by: JWT + role `super_admin`
- Input: `{ scheduledAt: "2024-01-15T10:00:00Z", interviewMode: "video", interviewLink?: "" }`
- Send notification to applicant
**Acceptance Criteria:**
- [ ] Returns 200 with interview details
- [ ] Validates future date
- [ ] Sends notification to applicant
**Estimate:** 1 day

## Sub-task 7.4: Approve/Reject Application
**Type:** Sub-task
**Description:** Implement endpoint to make final decision on an application. The Super Admin provides decision ("approved" or "rejected") and optional reason. Validates documents have been verified before approval (returns 400 if not verified), creates student record if approved, updates application status accordingly, sends welcome email with login credentials on approval, sends rejection notification on rejection, and stores decision reason for audit.
- `PATCH /super-admin/applications/:id/decision`
- Protected by: JWT + role `super_admin`
- Input: `{ decision: "approved" | "rejected", reason?: "Met all criteria" }`
- If approved: Create student record, send welcome email
**Acceptance Criteria:**
- [ ] Returns 200 with decision
- [ ] Creates student record on approval
- [ ] Sends notification on decision
- [ ] Returns 400 if documents not verified
**Estimate:** 2 days

---

# Task 8: Reports & Monitoring
**Type:** Task  
**Summary:** Implement reporting endpoints for overview, student performance, and batch progress

**Description:** This task implements comprehensive reporting and monitoring capabilities for the Super Admin to analyze system performance and student progress. The reporting system includes three main components: dashboard reports providing system-wide overview statistics including total students, active batches, and course completion rates along with recent activity logs; student performance reports with filtering by batch and course, showing individual attendance, grades, and progress metrics; and batch progress reports with filtering by college and status, showing completion percentages, average attendance, and upcoming schedules. These reports enable data-driven decision making and help identify areas requiring attention.

## Sub-task 8.1: Dashboard Reports
**Type:** Sub-task
- `GET /super-admin/reports`
- Protected by: JWT + role `super_admin`
- Output: `{ overview: { totalStudents, activeBatches, completionRate }, recentActivity: [] }`
**Acceptance Criteria:**
- [ ] Returns overview statistics
- [ ] Returns recent activity log
**Estimate:** 1 day

## Sub-task 8.2: Student Performance Reports
**Type:** Sub-task
- `GET /super-admin/reports/students`
- Protected by: JWT + role `super_admin`
- Query: `?batchId=&courseId=&page=1&limit=10`
- Output: `{ data: [{ studentId, name, attendance, grades, progress }] }`
**Acceptance Criteria:**
- [ ] Returns student performance data
- [ ] Supports batch filter
- [ ] Supports course filter
**Estimate:** 2 days

## Sub-task 8.3: Batch Progress Reports
**Type:** Sub-task
- `GET /super-admin/reports/batches`
- Protected by: JWT + role `super_admin`
- Query: `?collegeId=&status=active`
- Output: `{ data: [{ batchId, name, progress, attendance, upcomingSchedule }] }`
**Acceptance Criteria:**
- [ ] Returns batch progress data
- [ ] Supports college filter
- [ ] Shows completion percentage
**Estimate:** 2 days

---

# Task 9: Mentor Management
**Type:** Task  
**Summary:** Implement mentor profile creation, college/batch assignment, permissions, and activation management

**Description:** This task implements comprehensive mentor management capabilities for Super Admins to manage the teaching staff in the LMS. Mentors are instructors assigned to colleges and batches who deliver course content to students. The workflow involves creating mentor profiles with professional details (name, email, phone, qualification, experience) and auto-generating login credentials, assigning mentors to specific colleges for administrative purposes, assigning mentors to specific batches where they will conduct classes, defining granular permissions controlling what actions mentors can perform (view students, update attendance, update marks, view reports), and activating or deactivating mentor accounts. A deactivated mentor cannot access the system. The listing endpoint supports filtering by status, college, and batch.

## Sub-task 9.1: Create Mentor Profile
**Type:** Sub-task
- `POST /super-admin/mentors`
- Protected by: JWT + role `super_admin`
- Input: `{ name, email, phone, qualification, experience, profilePhoto? }`
- Auto-generate temporary password, send welcome email
**Acceptance Criteria:**
- [ ] Returns 201 with created mentor data
- [ ] Validates unique email
- [ ] Sends welcome email with login credentials
- [ ] Returns 409 if email exists
**Estimate:** 1 day

## Sub-task 9.2: Assign College to Mentor
**Type:** Sub-task
- `PATCH /super-admin/mentors/:id/college`
- Protected by: JWT + role `super_admin`
- Input: `{ collegeId }`
- Validate college exists and is active
**Acceptance Criteria:**
- [ ] Returns 200 with college assigned
- [ ] Validates college exists
- [ ] Returns 404 if mentor not found
**Estimate:** 1 day

## Sub-task 9.3: Assign Batch to Mentor
**Type:** Sub-task
- `PATCH /super-admin/mentors/:id/batch`
- Protected by: JWT + role `super_admin`
- Input: `{ batchId }`
- Validate batch exists and is active
**Acceptance Criteria:**
- [ ] Returns 200 with batch assigned
- [ ] Validates batch exists
- [ ] Returns 404 if mentor not found
**Estimate:** 1 day

## Sub-task 9.4: Manage Mentor Permissions
**Type:** Sub-task
- `PATCH /super-admin/mentors/:id/permissions`
- Protected by: JWT + role `super_admin`
- Input: `{ permissions: ["view_students", "update_attendance", "update_marks", "view_reports"] }`
- Validate against predefined permission set
**Acceptance Criteria:**
- [ ] Returns 200 with updated permissions
- [ ] Validates against predefined permission set
- [ ] Returns 404 if mentor not found
**Estimate:** 1 day

## Sub-task 9.5: Activate/Deactivate Mentor
**Type:** Sub-task
- `PATCH /super-admin/mentors/:id/status`
- Protected by: JWT + role `super_admin`
- Input: `{ status: "active" | "inactive" }`
**Acceptance Criteria:**
- [ ] Returns 200 with updated status
- [ ] Returns 404 if mentor not found
- [ ] Deactivated mentor cannot login
**Estimate:** 1 day

## Sub-task 9.6: List Mentors
**Type:** Sub-task
- `GET /super-admin/mentors`
- Protected by: JWT + role `super_admin`
- Query: `?page=1&limit=10&status=active&collegeId=&batchId=`
**Acceptance Criteria:**
- [ ] Returns paginated list
- [ ] Supports status filter
- [ ] Supports college filter
- [ ] Supports batch filter
**Estimate:** 1 day

---

# Implementation Order (Recommended)

| Phase | Tasks | Description |
|-------|-------|-------------|
| 1 | Task 1 | Authentication & Authorization |
| 2 | Task 2, 3, 4 | Dashboard, College, Admin management |
| 3 | Task 5, 6, 9 | Course, Batch & Mentor management |
| 4 | Task 7 | Application processing |
| 5 | Task 8 | Reports & Monitoring |

---

# Technical Dependencies

**Database Models:** SuperAdmin, College, NetPyAdmin, MasterCourse, CourseModule, Batch, Student, Application, Interview, Report, Mentor

**Services:** AuthenticationService, CollegeService, AdminService, CourseService, BatchService, MentorService, ApplicationService, ReportService, EmailService, NotificationService

**Middleware:** authenticateJWT, authorizeRole, validateRequest, errorHandler, rateLimiter

---

# Notes
- Error format: `{ error: { code, message, details? } }`
- Timestamps: ISO 8601
- Dates: YYYY-MM-DD
- Pagination: page=1, limit=10, max=100
- All write operations logged for audit
