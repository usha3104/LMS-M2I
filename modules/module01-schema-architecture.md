# Module 01: Authentication & Onboarding - Schema Architecture

## 1. Database Schema

### 1.1 Users Collection
```
Table: users
├── id (UUID, PK)
├── email (VARCHAR, UNIQUE, NOT NULL)
├── username (VARCHAR, UNIQUE, NULL)
├── password_hash (VARCHAR, NOT NULL)
├── role (ENUM: super_admin, netpy_admin, college_admin, mentor, student, company)
├── role_id (UUID, FK → role-specific table)
├── is_active (BOOLEAN, DEFAULT true)
├── is_verified (BOOLEAN, DEFAULT false)
├── created_at (TIMESTAMP)
├── updated_at (TIMESTAMP)
├── last_login (TIMESTAMP)
└── failed_login_attempts (INT, DEFAULT 0)
```

### 1.2 User Roles Tables

```
Table: super_admins
├── id (UUID, PK)
├── user_id (UUID, FK → users.id)
├── full_name (VARCHAR)
├── phone (VARCHAR)
├── profile_image (VARCHAR)
└── created_at (TIMESTAMP)

Table: netpy_admins
├── id (UUID, PK)
├── user_id (UUID, FK → users.id)
├── full_name (VARCHAR)
├── phone (VARCHAR)
├── assigned_colleges (UUID[], FK → colleges.id)
├── profile_image (VARCHAR)
├── created_at (TIMESTAMP)
└── updated_at (TIMESTAMP)

Table: college_admins
├── id (UUID, PK)
├── user_id (UUID, FK → users.id)
├── college_id (UUID, FK → colleges.id)
├── full_name (VARCHAR)
├── phone (VARCHAR)
├── department (VARCHAR)
├── profile_image (VARCHAR)
├── created_at (TIMESTAMP)
└── updated_at (TIMESTAMP)

Table: mentors
├── id (UUID, PK)
├── user_id (UUID, FK → users.id)
├── full_name (VARCHAR)
├── phone (VARCHAR)
├── date_of_birth (DATE)
├── gender (ENUM: male, female, other)
├── profile_image (VARCHAR)
├── bio (TEXT)
├── linkedin_url (VARCHAR)
├── github_url (VARCHAR)
├── years_experience (INT)
├── current_company (VARCHAR)
├── current_designation (VARCHAR)
├── teaching_expertise (JSON)
├── certifications (JSON)
├── verification_status (ENUM: pending, verified, rejected)
├── verified_by (UUID, FK → users.id)
├── verified_at (TIMESTAMP)
├── created_at (TIMESTAMP)
└── updated_at (TIMESTAMP)

Table: students
├── id (UUID, PK)
├── user_id (UUID, FK → users.id)
├── application_id (UUID, FK → student_applications.id)
├── college_id (UUID, FK → colleges.id, NULLABLE)
├── batch_id (UUID, FK → batches.id, NULLABLE)
├── full_name (VARCHAR)
├── phone (VARCHAR)
├── date_of_birth (DATE)
├── gender (ENUM: male, female, other)
├── profile_image (VARCHAR)
├── aadhar_number (VARCHAR, ENCRYPTED)
├── address (JSON)
├── guardian_name (VARCHAR)
├── guardian_phone (VARCHAR)
├── application_path (ENUM: college_side, direct)
├── enrollment_date (DATE)
├── verification_status (ENUM: pending, verified, rejected)
├── created_at (TIMESTAMP)
└── updated_at (TIMESTAMP)

Table: companies
├── id (UUID, PK)
├── user_id (UUID, FK → users.id)
├── company_name (VARCHAR)
├── company_website (VARCHAR)
├── company_logo (VARCHAR)
├── industry (VARCHAR)
├── company_size (ENUM: startup, small, medium, large, enterprise)
├── description (TEXT)
├── contact_person_name (VARCHAR)
├── contact_person_email (VARCHAR)
├── contact_person_phone (VARCHAR)
├── verification_status (ENUM: pending, verified, rejected)
├── verified_by (UUID, FK → users.id)
├── verified_at (TIMESTAMP)
├── created_at (TIMESTAMP)
└── updated_at (TIMESTAMP)
```

### 1.3 Authentication Tables

```
Table: otp_verifications
├── id (UUID, PK)
├── user_id (UUID, FK → users.id)
├── otp_code (VARCHAR, 6 digits)
├── otp_type (ENUM: login, registration, password_reset)
├── expires_at (TIMESTAMP)
├── is_used (BOOLEAN, DEFAULT false)
├── created_at (TIMESTAMP)
└── ip_address (VARCHAR)

Table: sessions
├── id (UUID, PK)
├── user_id (UUID, FK → users.id)
├── session_token (VARCHAR, UNIQUE)
├── refresh_token (VARCHAR, UNIQUE)
├── ip_address (VARCHAR)
├── user_agent (VARCHAR)
├── device_info (JSON)
├── expires_at (TIMESTAMP)
├── created_at (TIMESTAMP)
└── last_activity (TIMESTAMP)

Table: two_factor_auth
├── id (UUID, PK)
├── user_id (UUID, FK → users.id)
├── secret_key (VARCHAR, ENCRYPTED)
├── is_enabled (BOOLEAN, DEFAULT false)
├── backup_codes (JSON)
├── created_at (TIMESTAMP)
└── updated_at (TIMESTAMP)

Table: password_resets
├── id (UUID, PK)
├── user_id (UUID, FK → users.id)
├── reset_token (VARCHAR, UNIQUE)
├── expires_at (TIMESTAMP)
├── is_used (BOOLEAN, DEFAULT false)
├── created_at (TIMESTAMP)
└── ip_address (VARCHAR)
```

### 1.4 Student Application Tables

```
Table: student_applications
├── id (UUID, PK)
├── application_number (VARCHAR, UNIQUE)
├── full_name (VARCHAR)
├── email (VARCHAR)
├── phone (VARCHAR)
├── date_of_birth (DATE)
├── gender (ENUM: male, female, other)
├── application_path (ENUM: college_side, direct)
├── college_id (UUID, FK → colleges.id, NULLABLE)
├── applied_at (TIMESTAMP)
├── status (ENUM: submitted, under_review, documents_pending, interview_scheduled, interview_completed, selected, rejected)
├── eligibility_status (ENUM: pending, eligible, ineligible)
├── eligibility_notes (TEXT)
├── document_verification_status (ENUM: pending, verified, rejected)
├── document_verified_by (UUID, FK → users.id)
├── document_verified_at (TIMESTAMP)
├── ai_score (DECIMAL(5,2))
├── ai_score_breakdown (JSON)
├── interview_id (UUID, FK → interviews.id, NULLABLE)
├── rejection_reason (TEXT)
├── rejection_email_sent (BOOLEAN, DEFAULT false)
├── created_at (TIMESTAMP)
└── updated_at (TIMESTAMP)

Table: application_documents
├── id (UUID, PK)
├── application_id (UUID, FK → student_applications.id)
├── document_type (ENUM: photo, aadhar, marksheet_10th, marksheet_12th, graduation_marksheet, college_id_card, other)
├── file_path (VARCHAR)
├── file_name (VARCHAR)
├── file_size (INT)
├── mime_type (VARCHAR)
├── is_verified (BOOLEAN, DEFAULT false)
├── verified_by (UUID, FK → users.id)
├── verified_at (TIMESTAMP)
├── rejection_reason (TEXT)
├── uploaded_at (TIMESTAMP)
└── created_at (TIMESTAMP)

Table: interviews
├── id (UUID, PK)
├── application_id (UUID, FK → student_applications.id)
├── interviewer_id (UUID, FK → users.id)
├── interview_type (ENUM: technical, behavioral, final)
├── scheduled_date (DATE)
├── scheduled_time (TIME)
├── duration_minutes (INT, DEFAULT 60)
├── meeting_link (VARCHAR)
├── status (ENUM: scheduled, completed, cancelled, no_show)
├── technical_score (INT, NULLABLE)
├── behavioral_score (INT, NULLABLE)
├── overall_feedback (TEXT)
├── result (ENUM: pending, selected, rejected)
├── completed_at (TIMESTAMP)
├── created_at (TIMESTAMP)
└── updated_at (TIMESTAMP)
```

### 1.5 College & Institution Tables

```
Table: colleges
├── id (UUID, PK)
├── college_code (VARCHAR, UNIQUE)
├── college_name (VARCHAR)
├── affiliation (VARCHAR)
├── university (VARCHAR)
├── address (JSON)
├── phone (VARCHAR)
├── email (VARCHAR)
├── website (VARCHAR)
├── logo (VARCHAR)
├── institution_type (ENUM: engineering, arts, science, commerce, polytechnic, other)
├── approved_programs (JSON)
├── status (ENUM: active, inactive, suspended)
├── verification_status (ENUM: pending, verified, rejected)
├── verified_by (UUID, FK → users.id)
├── verified_at (TIMESTAMP)
├── created_at (TIMESTAMP)
└── updated_at (TIMESTAMP)

Table: college_contacts
├── id (UUID, PK)
├── college_id (UUID, FK → colleges.id)
├── contact_type (ENUM: admin, principal, dean, hod, coordinator)
├── full_name (VARCHAR)
├── designation (VARCHAR)
├── email (VARCHAR)
├── phone (VARCHAR)
├── is_primary (BOOLEAN, DEFAULT false)
├── created_at (TIMESTAMP)
└── updated_at (TIMESTAMP)

Table: college_programs
├── id (UUID, PK)
├── college_id (UUID, FK → colleges.id)
├── program_name (VARCHAR)
├── program_code (VARCHAR)
├── department (VARCHAR)
├── duration_years (INT)
├── status (ENUM: active, inactive)
├── created_at (TIMESTAMP)
└── updated_at (TIMESTAMP)
```

### 1.6 Batch Tables

```
Table: batches
├── id (UUID, PK)
├── batch_code (VARCHAR, UNIQUE)
├── batch_name (VARCHAR)
├── college_id (UUID, FK → colleges.id, NULLABLE)
├── batch_type (ENUM: college_specific, open)
├── start_date (DATE)
├── end_date (DATE)
├── max_students (INT)
├── current_students (INT, DEFAULT 0)
├── status (ENUM: draft, open, in_progress, completed, cancelled)
├── created_by (UUID, FK → users.id)
├── created_at (TIMESTAMP)
└── updated_at (TIMESTAMP)
```

---

## 2. API Endpoints

### 2.1 Authentication Endpoints

| Method | Endpoint | Description | Access |
|--------|----------|-------------|--------|
| POST | /api/v1/auth/register | Register new user | Public |
| POST | /api/v1/auth/login | Login with credentials | Public |
| POST | /api/v1/auth/logout | Logout and invalidate session | Auth |
| POST | /api/v1/auth/otp/send | Send OTP for verification | Public |
| POST | /api/v1/auth/otp/verify | Verify OTP | Public |
| POST | /api/v1/auth/otp/resend | Resend OTP | Public |
| POST | /api/v1/auth/2fa/enable | Enable 2FA | Auth |
| POST | /api/v1/auth/2fa/disable | Disable 2FA | Auth |
| POST | /api/v1/auth/2fa/verify | Verify 2FA code | Public |
| POST | /api/v1/auth/password/reset | Request password reset | Public |
| POST | /api/v1/auth/password/reset/confirm | Confirm password reset | Public |
| GET | /api/v1/auth/me | Get current user | Auth |
| POST | /api/v1/auth/session/refresh | Refresh session token | Auth |

### 2.2 Student Application Endpoints

| Method | Endpoint | Description | Access |
|--------|----------|-------------|--------|
| POST | /api/v1/applications | Submit new application | Public |
| GET | /api/v1/applications/:id | Get application details | Applicant/Admin |
| PUT | /api/v1/applications/:id | Update application | Applicant |
| POST | /api/v1/applications/:id/documents | Upload documents | Applicant |
| POST | /api/v1/applications/:id/eligibility/check | Check eligibility | Admin |
| POST | /api/v1/applications/:id/documents/verify | Verify documents | Admin |
| POST | /api/v1/applications/:id/score | AI score application | Admin |
| POST | /api/v1/applications/:id/interview/schedule | Schedule interview | Admin |
| POST | /api/v1/applications/:id/interview/complete | Complete interview | Admin |
| POST | /api/v1/applications/:id/select | Select applicant | Admin |
| POST | /api/v1/applications/:id/reject | Reject applicant | Admin |
| GET | /api/v1/applications | List applications | Admin |
| GET | /api/v1/applications/export | Export applications | Admin |

### 2.3 Onboarding Endpoints

| Method | Endpoint | Description | Access |
|--------|----------|-------------|--------|
| POST | /api/v1/onboarding/student | Create student profile (post-selection) | Admin |
| PUT | /api/v1/onboarding/student/:id | Complete student profile | Student |
| POST | /api/v1/onboarding/student/:id/batch/assign | Assign student to batch | Admin |
| POST | /api/v1/onboarding/mentor | Submit mentor application | Public |
| PUT | /api/v1/onboarding/mentor/:id | Update mentor profile | Mentor |
| POST | /api/v1/onboarding/mentor/:id/verify | Verify mentor | Admin |
| POST | /api/v1/onboarding/college | Submit college application | Public |
| PUT | /api/v1/onboarding/college/:id | Update college profile | College Admin |
| POST | /api/v1/onboarding/college/:id/approve | Approve college | Admin |
| GET | /api/v1/onboarding/status/:token | Check onboarding status | Public |

### 2.4 College Management Endpoints

| Method | Endpoint | Description | Access |
|--------|----------|-------------|--------|
| POST | /api/v1/colleges | Create college | Super Admin |
| GET | /api/v1/colleges | List colleges | Admin |
| GET | /api/v1/colleges/:id | Get college details | Admin |
| PUT | /api/v1/colleges/:id | Update college | Super Admin |
| DELETE | /api/v1/colleges/:id | Delete college | Super Admin |
| POST | /api/v1/colleges/:id/verify | Verify college | Admin |
| POST | /api/v1/colleges/:id/contacts | Add college contact | Admin |
| PUT | /api/v1/colleges/:id/contacts/:contactId | Update contact | Admin |
| POST | /api/v1/colleges/:id/programs | Add college program | Admin |

---

## 3. Data Models (TypeScript Interfaces)

### 3.1 User & Authentication Models

```typescript
enum UserRole {
  SUPER_ADMIN = 'super_admin',
  NETPY_ADMIN = 'netpy_admin',
  COLLEGE_ADMIN = 'college_admin',
  MENTOR = 'mentor',
  STUDENT = 'student',
  COMPANY = 'company'
}

interface User {
  id: string;
  email: string;
  username?: string;
  role: UserRole;
  roleId: string;
  isActive: boolean;
  isVerified: boolean;
  createdAt: Date;
  updatedAt: Date;
  lastLogin?: Date;
  failedLoginAttempts: number;
}

interface LoginRequest {
  email: string;
  password: string;
}

interface LoginResponse {
  user: User;
  accessToken: string;
  refreshToken: string;
  requiresOTP: boolean;
  requires2FA: boolean;
}

interface OTPVerification {
  id: string;
  userId: string;
  otpCode: string;
  otpType: 'login' | 'registration' | 'password_reset';
  expiresAt: Date;
  isUsed: boolean;
  createdAt: Date;
  ipAddress: string;
}

interface Session {
  id: string;
  userId: string;
  sessionToken: string;
  refreshToken: string;
  ipAddress: string;
  userAgent: string;
  deviceInfo: DeviceInfo;
  expiresAt: Date;
  createdAt: Date;
  lastActivity: Date;
}

interface TwoFactorAuth {
  id: string;
  userId: string;
  secretKey: string;
  isEnabled: boolean;
  backupCodes: string[];
  createdAt: Date;
  updatedAt: Date;
}
```

### 3.2 Student Application Models

```typescript
enum ApplicationPath {
  COLLEGE_SIDE = 'college_side',
  DIRECT = 'direct'
}

enum ApplicationStatus {
  SUBMITTED = 'submitted',
  UNDER_REVIEW = 'under_review',
  DOCUMENTS_PENDING = 'documents_pending',
  INTERVIEW_SCHEDULED = 'interview_scheduled',
  INTERVIEW_COMPLETED = 'interview_completed',
  SELECTED = 'selected',
  REJECTED = 'rejected'
}

enum EligibilityStatus {
  PENDING = 'pending',
  ELIGIBLE = 'eligible',
  INELIGIBLE = 'ineligible'
}

enum DocumentVerificationStatus {
  PENDING = 'pending',
  VERIFIED = 'verified',
  REJECTED = 'rejected'
}

interface StudentApplication {
  id: string;
  applicationNumber: string;
  fullName: string;
  email: string;
  phone: string;
  dateOfBirth: Date;
  gender: 'male' | 'female' | 'other';
  applicationPath: ApplicationPath;
  collegeId?: string;
  appliedAt: Date;
  status: ApplicationStatus;
  eligibilityStatus: EligibilityStatus;
  eligibilityNotes?: string;
  documentVerificationStatus: DocumentVerificationStatus;
  documentVerifiedBy?: string;
  documentVerifiedAt?: Date;
  aiScore?: number;
  aiScoreBreakdown?: AIScoreBreakdown;
  interviewId?: string;
  rejectionReason?: string;
  rejectionEmailSent: boolean;
  createdAt: Date;
  updatedAt: Date;
}

interface AIScoreBreakdown {
  academicScore: number;
  experienceScore: number;
  skillsScore: number;
  overallScore: number;
  strengths: string[];
  weaknesses: string[];
  recommendation: 'strong_candidate' | 'average_candidate' | 'weak_candidate';
}

interface ApplicationDocument {
  id: string;
  applicationId: string;
  documentType: DocumentType;
  filePath: string;
  fileName: string;
  fileSize: number;
  mimeType: string;
  isVerified: boolean;
  verifiedBy?: string;
  verifiedAt?: Date;
  rejectionReason?: string;
  uploadedAt: Date;
  createdAt: Date;
}

enum DocumentType {
  PHOTO = 'photo',
  AADHAR = 'aadhar',
  MARKSHEET_10TH = 'marksheet_10th',
  MARKSHEET_12TH = 'marksheet_12th',
  GRADUATION_MARKSHEET = 'graduation_marksheet',
  COLLEGE_ID_CARD = 'college_id_card',
  OTHER = 'other'
}
```

### 3.3 Interview Models

```typescript
enum InterviewType {
  TECHNICAL = 'technical',
  BEHAVIORAL = 'behavioral',
  FINAL = 'final'
}

enum InterviewStatus {
  SCHEDULED = 'scheduled',
  COMPLETED = 'completed',
  CANCELLED = 'cancelled',
  NO_SHOW = 'no_show'
}

enum InterviewResult {
  PENDING = 'pending',
  SELECTED = 'selected',
  REJECTED = 'rejected'
}

interface Interview {
  id: string;
  applicationId: string;
  interviewerId: string;
  interviewType: InterviewType;
  scheduledDate: Date;
  scheduledTime: string;
  durationMinutes: number;
  meetingLink: string;
  status: InterviewStatus;
  technicalScore?: number;
  behavioralScore?: number;
  overallFeedback?: string;
  result: InterviewResult;
  completedAt?: Date;
  createdAt: Date;
  updatedAt: Date;
}
```

### 3.4 Mentor Models

```typescript
enum VerificationStatus {
  PENDING = 'pending',
  VERIFIED = 'verified',
  REJECTED = 'rejected'
}

interface Mentor {
  id: string;
  userId: string;
  fullName: string;
  phone: string;
  dateOfBirth: Date;
  gender: 'male' | 'female' | 'other';
  profileImage?: string;
  bio?: string;
  linkedinUrl?: string;
  githubUrl?: string;
  yearsExperience: number;
  currentCompany?: string;
  currentDesignation?: string;
  teachingExpertise: TeachingExpertise[];
  certifications: Certification[];
  verificationStatus: VerificationStatus;
  verifiedBy?: string;
  verifiedAt?: Date;
  createdAt: Date;
  updatedAt: Date;
}

interface TeachingExpertise {
  subject: string;
  expertiseLevel: 'beginner' | 'intermediate' | 'expert';
  yearsTeaching: number;
}

interface Certification {
  name: string;
  issuer: string;
  issueDate: Date;
  expiryDate?: Date;
  credentialId?: string;
  credentialUrl?: string;
}
```

### 3.5 College Models

```typescript
enum InstitutionType {
  ENGINEERING = 'engineering',
  ARTS = 'arts',
  SCIENCE = 'science',
  COMMERCE = 'commerce',
  POLYTECHNIC = 'polytechnic',
  OTHER = 'other'
}

enum CollegeStatus {
  ACTIVE = 'active',
  INACTIVE = 'inactive',
  SUSPENDED = 'suspended'
}

interface College {
  id: string;
  collegeCode: string;
  collegeName: string;
  affiliation: string;
  university: string;
  address: Address;
  phone: string;
  email: string;
  website?: string;
  logo?: string;
  institutionType: InstitutionType;
  approvedPrograms: Program[];
  status: CollegeStatus;
  verificationStatus: VerificationStatus;
  verifiedBy?: string;
  verifiedAt?: Date;
  createdAt: Date;
  updatedAt: Date;
}

interface Address {
  street: string;
  city: string;
  state: string;
  pincode: string;
  country: string;
}

interface Program {
  name: string;
  code: string;
  department: string;
  durationYears: number;
}

interface CollegeContact {
  id: string;
  collegeId: string;
  contactType: 'admin' | 'principal' | 'dean' | 'hod' | 'coordinator';
  fullName: string;
  designation: string;
  email: string;
  phone: string;
  isPrimary: boolean;
  createdAt: Date;
  updatedAt: Date;
}
```

### 3.6 Batch Models

```typescript
enum BatchType {
  COLLEGE_SPECIFIC = 'college_specific',
  OPEN = 'open'
}

enum BatchStatus {
  DRAFT = 'draft',
  OPEN = 'open',
  IN_PROGRESS = 'in_progress',
  COMPLETED = 'completed',
  CANCELLED = 'cancelled'
}

interface Batch {
  id: string;
  batchCode: string;
  batchName: string;
  collegeId?: string;
  batchType: BatchType;
  startDate: Date;
  endDate: Date;
  maxStudents: number;
  currentStudents: number;
  status: BatchStatus;
  createdBy: string;
  createdAt: Date;
  updatedAt: Date;
}
```

---

## 4. Authentication Flow

### 4.1 Login Flow

```
┌─────────────────────────────────────────────────────────────────┐
│                      LOGIN FLOW                                  │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  ┌──────────┐                                                  │
│  │  User    │                                                  │
│  │Enters    │                                                  │
│  │Credentials                                                  │
│  └────┬─────┘                                                  │
│       │ POST /api/v1/auth/login                                │
│       ▼                                                        │
│  ┌─────────────┐                                               │
│  │  Validate   │──Invalid──▶ Show Error                        │
│  │  Credentials│                                               │
│  └──────┬──────┘                                               │
│         │ Valid                                                │
│         ▼                                                      │
│  ┌─────────────────┐                                           │
│  │ Check if OTP    │──Yes───▶ Send OTP ──▶ Return requiresOTP  │
│  │ Required?        │         to client                        │
│  └────────┬────────┘                                           │
│           │ No                                                  │
│           ▼                                                     │
│  ┌─────────────────┐                                           │
│  │ Check if 2FA    │──Yes───▶ Return requires2FA               │
│  │ Enabled?        │         to client                        │
│  └────────┬────────┘                                           │
│           │ No                                                  │
│           ▼                                                     │
│  ┌─────────────────┐                                           │
│  │ Create Session  │                                           │
│  │ Generate Tokens │                                           │
│  └────────┬────────┘                                           │
│           │                                                    │
│           ▼                                                    │
│  ┌─────────────────┐                                           │
│  │ Identify User   │                                           │
│  │ Role            │                                           │
│  └────────┬────────┘                                           │
│           │                                                    │
│           ▼                                                    │
│  ┌─────────────────────────────────────┐                       │
│  │         Return                       │                       │
│  │  • User Object                      │                       │
│  │  • Access Token                     │                       │
│  │  • Refresh Token                    │                       │
│  │  • Role-based Redirect URL          │                       │
│  └─────────────────────────────────────┘                       │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

### 4.2 OTP Verification Flow

```
┌─────────────────────────────────────────────────────────────────┐
│                    OTP VERIFICATION FLOW                        │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  Client                                                          │
│    │                                                              │
│    │ POST /api/v1/auth/otp/send                                  │
│    ▼                                                              │
│  ┌────────────────────┐                                          │
│  │ Generate 6-digit   │                                          │
│  │ OTP                │                                          │
│  └─────────┬──────────┘                                          │
│            │                                                      │
│            ▼                                                      │
│  ┌────────────────────┐                                          │
│  │ Store OTP in DB    │                                          │
│  │ (expires in 5 min) │                                          │
│  └─────────┬──────────┘                                          │
│            │                                                      │
│            ▼                                                      │
│  ┌────────────────────┐                                          │
│  │ Send OTP via       │                                          │
│  │ Email/SMS          │                                          │
│  └─────────┬──────────┘                                          │
│            │                                                      │
│    ────────┴──────────                                            │
│    │                   │                                          │
│    ▼                   ▼                                          │
│  Client              Server                                      │
│  Enter OTP           POST /api/v1/auth/otp/verify                │
│    │                   │                                          │
│    │                   ▼                                          │
│    │            ┌──────────────┐                                  │
│    │            │ Validate OTP │                                  │
│    │            │ • Not expired│                                  │
│    │            │ • Not used   │                                  │
│    │            │ • Correct    │                                  │
│    │            └──────┬───────┘                                  │
│    │                   │                                          │
│    └──────┬────────────┘                                          │
│           │ Valid                                                 │
│           ▼                                                      │
│    ┌─────────────────┐                                           │
│    │ Mark OTP as     │                                           │
│    │ used            │                                           │
│    └────────┬────────┘                                           │
│             │                                                    │
│             ▼                                                    │
│    ┌─────────────────┐                                           │
│    │ Create/Return   │                                           │
│    │ Session Tokens  │                                           │
│    └─────────────────┘                                           │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

### 4.3 Role-Based Redirect Mapping

| Role | Redirect URL | Dashboard Route |
|------|--------------|------------------|
| SUPER_ADMIN | /dashboard/super-admin | /dashboard/super-admin |
| NETPY_ADMIN | /dashboard/netpy-admin | /dashboard/netpy-admin |
| COLLEGE_ADMIN | /dashboard/college/:id | /dashboard/college |
| MENTOR | /dashboard/mentor | /dashboard/mentor |
| COMPANY | /dashboard/company | /dashboard/company |
| STUDENT | /dashboard/student | /dashboard/student |

---

## 5. Onboarding Flows

### 5.1 Student Onboarding Flow

```
┌─────────────────────────────────────────────────────────────────┐
│                STUDENT ONBOARDING FLOW                          │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │              APPLICATION PHASE                           │  │
│  └──────────────────────────────────────────────────────────┘  │
│                                                                  │
│  Start                                                           │
│    │                                                             │
│    ▼                                                             │
│  ┌───────────────────┐                                          │
│  │ Select Role:      │                                          │
│  │ Student           │                                          │
│  └────────┬──────────┘                                          │
│           │                                                      │
│           ▼                                                      │
│  ┌───────────────────┐                                          │
│  │ Select Path?      │                                          │
│  └────────┬──────────┘                                          │
│       │       │                                                  │
│       ▼       ▼                                                  │
│  ┌───────┐ ┌───────┐                                            │
│  │College│ │Direct │                                            │
│  │-side  │ │Apply  │                                            │
│  └──┬───┘ └───┬───┘                                            │
│     │         │                                                 │
│     ▼         ▼                                                 │
│  ┌─────────────────────────┐                                   │
│  │ Submit Application Form  │                                   │
│  │ • Personal Details       │                                   │
│  │ • Academic Details       │                                   │
│  │ • Contact Info           │                                   │
│  └────────────┬────────────┘                                    │
│               │                                                  │
│               ▼                                                  │
│  ┌─────────────────────────┐                                     │
│  │ Upload Documents        │                                     │
│  │ • Photo                 │                                     │
│  │ • Aadhar                │                                     │
│  │ • Marksheets            │                                     │
│  │ • College ID (if applicable)                                │
│  └────────────┬────────────┘                                    │
│               │                                                  │
│  ─────────────┴─────────────                                    │
│  │                                                             │
│  ▼                                                             │
│  ┌─────────────────────────┐                                    │
│  │ SCREENING PHASE         │                                    │
│  └─────────────────────────┘                                    │
│               │                                                  │
│               ▼                                                  │
│  ┌───────────────────────┐                                      │
│  │ Eligibility Check     │────Ineligible──▶ Rejection          │
│  │ • Academic Criteria   │                                      │
│  │ • Age Requirements    │                                      │
│  └───────────┬───────────┘                                      │
│              │ Eligible                                          │
│              ▼                                                   │
│  ┌───────────────────────┐                                      │
│  │ Document Verification │────Rejected──▶ Request Resubmission│
│  │ • Verify all docs     │                                      │
│  └───────────┬───────────┘                                      │
│              │ Verified                                          │
│              ▼                                                   │
│  ┌───────────────────────┐                                      │
│  │ AI Scoring            │                                      │
│  │ • Academic Score      │                                      │
│  │ • Experience Score    │                                      │
│  │ • Skills Assessment   │                                      │
│  └───────────┬───────────┘                                      │
│              │                                                  │
│              ▼                                                  │
│  ┌─────────────────────────┐                                    │
│  │ INTERVIEW PHASE         │                                    │
│  └─────────────────────────┘                                    │
│              │                                                  │
│              ▼                                                  │
│  ┌───────────────────────┐                                      │
│  │ Send Interview Invite │                                      │
│  │ (Email + SMS)          │                                      │
│  └───────────┬───────────┘                                      │
│              │                                                  │
│              ▼                                                  │
│  ┌───────────────────────┐                                      │
│  │ Self-service Schedule │                                      │
│  │ • Select Date/Time    │                                      │
│  │ • Get Meeting Link    │                                      │
│  └───────────┬───────────┘                                      │
│              │                                                  │
│              ▼                                                  │
│  ┌───────────────────────┐                                      │
│  │ Conduct Interview     │                                      │
│  │ • Technical           │                                      │
│  │ • Behavioral          │                                      │
│  └───────────┬───────────┘                                      │
│              │                                                  │
│              ▼                                                  │
│  ┌───────────────────────┐                                      │
│  │ Interview Result      │────Rejected──▶ Rejection Handling   │
│  │ • Selected/Rejected   │                                      │
│  └───────────┬───────────┘                                      │
│              │ Selected                                          │
│              ▼                                                  │
│  ┌─────────────────────────┐                                    │
│  │ DIGITAL ONBOARDING      │                                    │
│  └─────────────────────────┘                                    │
│              │                                                  │
│              ▼                                                  │
│  ┌───────────────────────┐                                      │
│  │ Create User Account   │                                      │
│  │ • Generate Credentials│                                      │
│  │ • Set Temporary Pass  │                                      │
│  └───────────┬───────────┘                                      │
│              │                                                  │
│              ▼                                                  │
│  ┌───────────────────────┐                                      │
│  │ Profile Configuration │                                      │
│  │ • Complete Profile    │                                      │
│  │ • Setup Security      │                                      │
│  └───────────┬───────────┘                                      │
│              │                                                  │
│              ▼                                                  │
│  ┌───────────────────────┐                                      │
│  │ Assign Batch          │                                      │
│  │ • College Batch       │                                      │
│  │ • Open Batch          │                                      │
│  └───────────┬───────────┘                                      │
│              │                                                  │
│              ▼                                                  │
│  ┌───────────────────────┐                                      │
│  │ Enable Dashboard     │                                      │
│  │ Access                │                                      │
│  └───────────┬───────────┘                                      │
│              │                                                  │
│              ▼                                                  │
│          Complete                                                │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

### 5.2 Mentor Onboarding Flow

```
┌─────────────────────────────────────────────────────────────────┐
│                MENTOR ONBOARDING FLOW                           │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  Start                                                           │
│    │                                                             │
│    ▼                                                             │
│  ┌───────────────────┐                                          │
│  │ Select Role:      │                                          │
│  │ Mentor            │                                          │
│  └────────┬──────────┘                                          │
│           │                                                      │
│           ▼                                                      │
│  ┌───────────────────┐                                          │
│  │ Personal Info    │                                          │
│  │ • Full Name      │                                          │
│  │ • DOB            │                                          │
│  │ • Gender         │                                          │
│  │ • Contact        │                                          │
│  └────────┬──────────┘                                          │
│           │                                                      │
│           ▼                                                      │
│  ┌───────────────────┐                                          │
│  │ Professional      │                                          │
│  │ Experience        │                                          │
│  │ • Current Company │                                          │
│  │ • Designation     │                                          │
│  │ • Years Exp       │                                          │
│  │ • LinkedIn/GitHub│                                          │
│  └────────┬──────────┘                                          │
│           │                                                      │
│           ▼                                                      │
│  ┌───────────────────┐                                          │
│  │ Teaching Expertise│                                         │
│  │ • Subjects        │                                          │
│  │ • Expertise Level │                                          │
│  │ • Teaching Years  │                                          │
│  └────────┬──────────┘                                          │
│           │                                                      │
│           ▼                                                      │
│  ┌───────────────────┐                                          │
│  │ Upload            │                                          │
│  │ Certifications    │                                          │
│  │ • Certificate     │                                          │
│  │ • Issuer          │                                          │
│  │ • Credential URL  │                                          │
│  └────────┬──────────┘                                          │
│           │                                                      │
│           ▼                                                      │
│  ┌───────────────────┐                                          │
│  │ Submit Application│                                         │
│  └────────┬──────────┘                                          │
│           │                                                      │
│           ▼                                                      │
│  ┌───────────────────┐                                          │
│  │ Admin Verification│                                          │
│  │ • Verify Documents│                                          │
│  │ • Check Background│                                         │
│  │ • Validate Exp    │                                          │
│  └────────┬──────────┘                                          │
│           │                                                      │
│      ┌────┴────┐                                                 │
│      ▼         ▼                                                 │
│  ┌───────┐ ┌───────┐                                            │
│  │Verified│ │Rejected│                                           │
│  └───┬───┘ └───┬───┘                                            │
│      │         │                                                 │
│      ▼         ▼                                                 │
│  ┌─────────────────┐                                            │
│  │ Create Mentor   │──▶ Notification Sent                       │
│  │ Profile         │                                            │
│  └────────┬────────┘                                            │
│           │                                                      │
│           ▼                                                      │
│  ┌─────────────────┐                                            │
│  │ Redirect to     │                                            │
│  │ Mentor Dashboard│                                            │
│  └─────────────────┘                                            │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

### 5.3 College Onboarding Flow

```
┌─────────────────────────────────────────────────────────────────┐
│                COLLEGE ONBOARDING FLOW                          │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  Start                                                           │
│    │                                                             │
│    ▼                                                             │
│  ┌───────────────────┐                                          │
│  │ Select Role:      │                                          │
│  │ College           │                                          │
│  └────────┬──────────┘                                          │
│           │                                                      │
│           ▼                                                      │
│  ┌───────────────────┐                                          │
│  │ Institution Info  │                                          │
│  │ • College Name    │                                          │
│  │ • Code            │                                          │
│  │ • Affiliation     │                                          │
│  │ • University      │                                          │
│  │ • Address         │                                          │
│  └────────┬──────────┘                                          │
│           │                                                      │
│           ▼                                                      │
│  ┌───────────────────┐                                          │
│  │ Admin Contact     │                                          │
│  │ • Contact Person  │                                          │
│  │ • Designation     │                                          │
│  │ • Email           │                                          │
│  │ • Phone           │                                          │
│  └────────┬──────────┘                                          │
│           │                                                      │
│           ▼                                                      │
│  ┌───────────────────┐                                          │
│  │ Program Details   │                                          │
│  │ • Programs Offered│                                          │
│  │ • Departments     │                                          │
│  │ • Duration        │                                          │
│  └────────┬──────────┘                                          │
│           │                                                      │
│           ▼                                                      │
│  ┌───────────────────┐                                          │
│  │ Submit Application│                                          │
│  └────────┬──────────┘                                          │
│           │                                                      │
│           ▼                                                      │
│  ┌───────────────────┐                                          │
│  │ Admin Approval    │                                          │
│  │ • Verify Documents│                                          │
│  │ • Check Affiliation│                                         │
│  │ • Validate Programs│                                        │
│  └────────┬──────────┘                                          │
│           │                                                      │
│      ┌────┴────┐                                                 │
│      ▼         ▼                                                 │
│  ┌───────┐ ┌───────┐                                            │
│  │Approved│ │Rejected│                                           │
│  └───┬───┘ └───┬───┘                                            │
│      │         │                                                 │
│      ▼         ▼                                                 │
│  ┌─────────────────┐                                            │
│  │ Create College  │──▶ Notification Sent                       │
│  │ Profile         │                                            │
│  └────────┬────────┘                                            │
│           │                                                      │
│           ▼                                                      │
│  ┌─────────────────┐                                            │
│  │ Create College  │                                            │
│  │ Admin Account   │                                            │
│  └────────┬────────┘                                            │
│           │                                                      │
│           ▼                                                      │
│  ┌─────────────────┐                                            │
│  │ Redirect to     │                                            │
│  │ College Dashboard│                                           │
│  └─────────────────┘                                            │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## 6. Role-Based Access Control (RBAC)

### 6.1 Permission Matrix

| Permission | Super Admin | NetPy Admin | College Admin | Mentor | Student | Company |
|------------|:------------:|:------------:|:-------------:|:------:|:-------:|:-------:|
| **Authentication** ||||||
| Manage own profile | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| Change own password | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| Enable/Disable 2FA | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| View audit logs | ✓ | ✗ | ✗ | ✗ | ✗ | ✗ |
| **User Management** ||||||
| Create NetPy Admin | ✓ | ✗ | ✗ | ✗ | ✗ | ✗ |
| Create College Admin | ✓ | ✓ | ✗ | ✗ | ✗ | ✗ |
| Create Mentor | ✓ | ✓ | ✗ | ✗ | ✗ | ✗ |
| Create Student | ✓ | ✓ | ✓ | ✗ | ✗ | ✗ |
| View all users | ✓ | ✓ | ✓ | ✗ | ✗ | ✗ |
| Manage user roles | ✓ | ✗ | ✗ | ✗ | ✗ | ✗ |
| **College Management** ||||||
| Create College | ✓ | ✗ | ✗ | ✗ | ✗ | ✗ |
| Approve College | ✓ | ✓ | ✗ | ✗ | ✗ | ✗ |
| View College | ✓ | ✓ | ✓ | ✓ | ✓ | ✗ |
| Manage College | ✓ | ✓ | ✗ | ✗ | ✗ | ✗ |
| **Application Management** ||||||
| View all applications | ✓ | ✓ | ✓ | ✗ | ✗ | ✗ |
| Review applications | ✓ | ✓ | ✓ | ✗ | ✗ | ✗ |
| Schedule interviews | ✓ | ✓ | ✓ | ✓ | ✗ | ✗ |
| Conduct interviews | ✓ | ✓ | ✓ | ✓ | ✗ | ✗ |
| Select/Reject applicants | ✓ | ✓ | ✓ | ✗ | ✗ | ✗ |
| Submit own application | ✗ | ✗ | ✗ | ✗ | ✓ | ✗ |
| **Batch Management** ||||||
| Create Batch | ✓ | ✓ | ✗ | ✗ | ✗ | ✗ |
| Assign Students | ✓ | ✓ | ✓ | ✗ | ✗ | ✗ |
| View Batch | ✓ | ✓ | ✓ | ✓ | ✓ | ✗ |
| **Reporting** ||||||
| View all reports | ✓ | ✓ | ✗ | ✗ | ✗ | ✗ |
| View college reports | ✓ | ✓ | ✓ | ✗ | ✗ | ✗ |
| View own performance | ✗ | ✗ | ✗ | ✓ | ✓ | ✗ |
| **Company Features** ||||||
| View student profiles | ✗ | ✗ | ✗ | ✗ | ✗ | ✓ |
| Post internships | ✗ | ✗ | ✗ | ✗ | ✗ | ✓ |

---

## 7. Security Considerations

### 7.1 Password Requirements
- Minimum 8 characters
- At least one uppercase letter
- At least one lowercase letter
- At least one number
- At least one special character
- Passwords stored using bcrypt with salt rounds ≥ 12

### 7.2 Session Management
- Access token expiry: 15 minutes
- Refresh token expiry: 7 days
- Session timeout: 30 minutes of inactivity
- Maximum concurrent sessions: 3

### 7.3 OTP Configuration
- OTP length: 6 digits
- OTP validity: 5 minutes
- Maximum OTP attempts: 3
- OTP resend cooldown: 60 seconds

### 7.4 Rate Limiting
- Login attempts: 5 per minute
- OTP requests: 3 per minute
- API requests: 100 per minute (authenticated)
- Public API requests: 20 per minute

### 7.5 Audit Logging
All authentication and onboarding events are logged:
- Login attempts (success/failure)
- OTP sent/verified
- Password changes
- Role changes
- Application status changes
- Profile updates

---

## 8. State Machines

### 8.1 Application Status Flow

```
SUBMITTED → UNDER_REVIEW → DOCUMENTS_PENDING → INTERVIEW_SCHEDULED → INTERVIEW_COMPLETED
     ↓              ↓                 ↓                   ↓                   ↓
  REJECTED      REJECTED          REJECTED            REJECTED            SELECTED/REJECTED
```

### 8.2 Mentor Verification Flow

```
PENDING → VERIFIED
    ↓
 REJECTED
```

### 8.3 College Verification Flow

```
PENDING → VERIFIED
    ↓
 REJECTED
```
