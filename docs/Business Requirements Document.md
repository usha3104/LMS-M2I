underBUSINESS REQUIREMENTS DOCUMENT (BRD)

NetPy Mentorship to Internship (M2i LMS)

Date: February 2026
Date: 
Prepared For: Founders, Designers, Developers, Product Managers, Colleges

Table of Contents
1. Executive Summary
2. System Overview
3. User Roles & Permission Matrix
4. Scope of System
5. Role-Based User Structure
6. Module 1: Governance & Role Management
7. Module 2: Course Management
8. Module 3: Batch Management
9. Module 4: Learning & Mentorship Management
10. Module 5: Progress Tracking & Evaluation
11. Module 6: Student Application & Screening
12. Module 7: Feedback & Performance Review
13. Module 8: Student Authentication & Dashboard
14. End-to-End User Workflows
15. Success Metrics

---

## 1. Executive Summary

### Purpose
The NetPy Mentorship to Internship (M2i LMS) is a multi-tenant SaaS platform designed to bridge the gap between academic learning and industry employment through structured mentorship and application-based screening.

It connects:
• Students (College-side & Direct Applicants)
• Colleges (Institutional Partners)
• Mentors (Trainers/Teachers)
• NetPy Admin Team (5-Tier Governance)
• Hiring Companies / Startups

into one unified, performance-driven mentorship ecosystem.

### Problem Statement
Currently:
• Students lack industry exposure and verified performance profiles
• Colleges struggle to track real learning outcomes
• Companies don't get job-ready interns with verified skills
• Application processes are fragmented and inefficient

M2I LMS solves this by creating a centralized mentorship-driven learning ecosystem with application-based screening and verified performance tracking.

### Key Objectives
1. Provide structured mentor-led learning with dual onboarding paths
2. Enable real-time academic transparency across 5-tier governance
3. Track individual student performance with observation phases
4. Create verified student performance profiles for hiring
5. Maintain governance with 5-tier administration hierarchy
6. Build application-based screening system for internship eligibility

---

## 2. System Overview

### What is M2i LMS?
M2i LMS is a cloud-based academic performance and internship management platform that:
• Onboards students through two paths (College-side & Direct Application)
• Screens applications through evaluation and interview process
• Organizes courses and batches with mentor assignment
• Tracks attendance and performance with individual focus
• Generates verified performance analytics
• Enables internship hiring visibility based on performance data

### Vision & Objectives
• Unified Ecosystem: Create a collaborative environment for Students, Mentors, Colleges, and Hiring Companies
• 5-Tier Administration: Implement structured governance (Super Admin → NetPy Admin → College Admin → Mentor → Student)
• Dual Onboarding Paths: Support both institutional (College-side) and individual (Direct) student registration with screening
• Application-Based Screening: Implement evaluation and interview process for student selection
• Verified Performance Profiles: Create trustworthy student profiles based on actual performance data
• Academic Transparency: Enable real-time tracking of attendance, progress reports, and batch execution

---

## 3. User Roles & Permission Matrix

### Role Definitions

| Role | Access Level | Key Permissions |
|-------|-------------|------------------|
| Super Admin | Full Control | Manage all colleges, admins, courses, system configuration |
| NetPy Admin | Assigned Colleges | Manage programs, batches, mentors, academic operations |
| College Admin | Read Only | View progress, attendance, reports for own college |
| Mentor | Academic Control | Manage attendance, assignments, feedback for assigned batches |
| Student | Limited | View courses, progress, submit assignments |
| Company | Hiring Access | View verified student profiles, post internships |

### Permission Matrix (Final)

| Function             | Super Admin | NetPy Admin | College Admin | Mentor        | Student  |
| ----------------------| -------------| -------------| ---------------| ---------------| ----------|
| System Configuration | ✓           | ✗           | ✗             | ✗             | ✗        |
| Create College       | ✓           | ✗           | ✗             | ✗             | ✗        |
| Edit College         | ✓           | Assigned    | ✗             | ✗             | ✗        |
| Create NetPy Admin   | ✓           | ✗           | ✗             | ✗             | ✗        |
| Create College Admin | ✓           | ✓           | ✗             | ✗             | ✗        |
| Create Mentor        | ✓           | ✓           | ✗             | ✗             | ✗        |
| Create Student       | ✓           | ✓           | Own College   | ✗             | ✗        |
| Create Course        | ✓           | ✓           | ✗             | ✗             | ✗        |
| Create Batch         | ✓           | ✓           | ✗             | ✗             | ✗        |
| Assign Mentor        | ✓           | ✓           | ✗             | ✗             | ✗        |
| Mark Attendance      | ✓           | ✓           | ✗             | ✓             | ✗        |
| Evaluate Students    | ✓           | ✓           | ✗             | ✓             | ✗        |
| Track Performance    | ✓           | ✓           | ✗             | ✓             | ✗        |
| View Reports         | ✓           | ✓           | ✓             | Assigned Only | Own Only |
| Manage Applications  | ✓           | ✓           | ✗             | ✗             | ✗        |
| Conduct Interviews   | ✓           | ✓           | ✗             | ✗             | ✗        |
| Select Candidates    | ✓           | ✓           | ✗             | ✗             | ✗        |

---

## 4. Scope of System

### In Scope
The system will provide a complete learning, screening, and internship management platform:
• 5-tier role-based access control with strict governance
• Dual student onboarding paths (College-side & Direct Application)
• Application-based screening with evaluation and interview process
• Centralized management of colleges, courses, batches, and users
• Student onboarding, mentor assignment, and batch scheduling
• Attendance tracking and continuous performance monitoring
• Individual and batch-level progress evaluation with observation phases
• Mentor-driven feedback and student support
• Read-only academic visibility for College Admins
• Verified student performance profiles for hiring
• Scalable structure to support multiple colleges and future expansion

### Out-of-Scope
The system will NOT include:
1. Guaranteed job placements (only internship visibility and opportunities)
2. Integration with external ERP/LMS systems in initial phase
3. Financial management features (fees, payroll, invoicing)
4. Advanced assessment engine (basic evaluation only)
5. Public student registration without screening

---

## 5. Role-Based User Structure

### 1. Super Admin (NetPy Company)
**Role Overview**
The Super Admin represents NetPy Company and serves as the highest authority on M2I platform with complete system-level access.

**Scope of Authority**
• College Management & User Management
• Course & Curriculum Management
• Batch Management & Academic Oversight
• Application Screening & Interview Process
• Reports & Analytics & System Configuration

**Key Responsibilities**
• Create and manage all user roles and permissions
• Approve colleges and monitor academic activities
• Create master courses and evaluation criteria
• Oversee application screening and selection process
• Monitor system-wide performance and KPIs
• Maintain platform policies and compliance

### 2. NetPy Admin (College Assigned)
**Role Overview**
Responsible for managing complete system of assigned college(s) including academic operations and application screening.

**Scope of Authority**
• College-Level Operational Management
• Course & Program Management
• Batch Management (yearly cycles)
• Student Management & Application Processing
• Performance Monitoring & Quality Control

**Key Responsibilities**
• Act as primary NetPy representative for assigned colleges
• Manage college-specific courses and batches
• Process and evaluate student applications
• Assign mentors and coordinate academic schedules
• Monitor attendance, assignments, and performance metrics

### 3. College Admin (Read-Only Role)
**Role Overview**
Observational stakeholder with full visibility but no modification rights for institutional transparency.

**Scope of Access**
• View-only access to student profiles and performance reports
• Attendance records and batch execution status
• Academic calendars and schedules
• Course progress and analytics dashboards

**Key Responsibilities**
• Monitor student progress and performance metrics
• Review batch-wise execution and completion status
• Ensure institutional academic transparency
• Coordinate with NetPy Admin for performance discussions

### 4. Mentor (Trainer / Teacher)
**Role Overview**
Responsible for training delivery, student engagement, and performance improvement within assigned batches.

**Scope of Authority**
• Training Delivery & Academic Planning
• Attendance & Engagement Management
• Performance Tracking & Student Mentorship
• Assignment & Project Management

**Key Responsibilities**
• Conduct live classes, workshops, and doubt-solving sessions
• Mark attendance and monitor participation levels
• Track assignment completion and project submissions
• Provide academic guidance and career-oriented mentoring
• Update student progress reports and evaluation records

### 5. Student (Learner)
**Role Overview**
Core users with two distinct onboarding paths and complete learning access.

**A. College-Side Student**
• Apply/enroll through their college with verification
• College Admin and NetPy Admin can monitor academic activities
• Automatically mapped to college-specific batches
• Full access to learning and mentorship features

**B. Direct Student (Individual Applicant)**
• Apply directly through application and screening process
• Assigned to general/open batches after selection
• Complete access to learning features after screening
• Isolated from college-level monitoring

**Student Features & Responsibilities**
• Access to live classes and doubt sessions
• Participation in live projects and assignments
• Individual progress tracking within batches
• Feedback system for mentors and courses
• Dashboard visibility for personal progress and attendance

---

## 6. Module 1: Governance & Role Management

### Purpose
Implement a structured Role-Based Access Control (RBAC) system with a 5-tier governance hierarchy.

### Governance Hierarchy
```
Super Admin (NetPy Head)
↓
NetPy Admin (College Assigned)
↓
College Admin (View-Only)
↓
Batch Trainer (Mentor)
↓
Student
```

### Key Features
• Strict hierarchical authority with least privilege enforcement
• Scoped college-level and batch-level access
• No cross-college or cross-batch access
• All governance actions logged with audit trails
• Session invalidation on role changes
• Direct Student isolation for individual applicants

---

## 7. Module 2: Course Management

### Purpose
Create and manage structured learning programs with standardized evaluation criteria.

### Course Creation Process
1. **Super Admin Creates Master Course**
   - Defines course template for use across colleges
   - Sets duration, modules, assignments, projects
   - Configures evaluation criteria (attendance %, assignment %, project %)

2. **Course Assignment**
   - Assign to specific colleges or make available globally
   - Save as draft or publish for batch creation
   - Maintain consistency in academic delivery standards

3. **Course Configuration**
   - Define curriculum structure and learning outcomes
   - Set prerequisites and difficulty levels
   - Upload learning materials and resources

---

## 8. Module 3: Batch Management

### Purpose
Organize students into structured learning groups under specific courses with proper mentor allocation.

### Batch Types
• **College-Specific Batch (Private)**
  - Accessible only to students of particular college
  - Monitored by College Admin (read-only)

• **General / Open Batch (Public)**
  - Accessible to Direct Students and multiple colleges
  - Monitored by NetPy Admin and assigned Mentor

### Batch Lifecycle
```
Created → Active → Under Evaluation → Completed → Archived
```

### Internal Process
1. Admin selects course and creates batch
2. Assigns mentor with expertise matching
3. Sets start/end dates and schedules
4. Adds students (college-side or direct)
5. System auto-generates batch ID and calendar
6. Activates attendance and performance tracking

---

## 9. Module 4: Learning & Mentorship Management

### Purpose
Ensure structured, mentor-led learning execution within each batch.

### Learning Flow
```
Batch Activated → Live Classes → Assignments → Projects → Evaluation → Feedback
```

### Mentor Capabilities
• Mark and manage attendance
• Upload class recordings and materials
• Assign assignments and projects
• Update student progress scores
• Schedule doubt-clearing sessions
• Provide qualitative feedback
• Monitor student engagement levels

### Student Engagement
• Access to live classes and recordings
• Submit assignments and projects
• Schedule meetings with mentors
• Track personal progress and performance

---

## 10. Module 5: Progress Tracking & Evaluation

### Purpose
Monitor, measure, and analyze student performance at individual and batch levels.

### Tracking Phases

**Initial 2-Month Observation Phase (Individual-Focused)**
• Learning speed and consistency tracking
• Attendance and submission patterns
• Adaptability to new concepts
• Participation and engagement levels
• Early identification of slow learners

**Post-Observation Phase (Individual + Batch Tracking)**
• Individual student scoring maintained
• Batch-wise performance metrics generated
• Comparative analysis and healthy competition
• Identification of strong/weak batches

### Personalized Improvement Paths
• High performers: Advanced projects and enrichment resources
• Consistent learners: Standard flow continuation
• Low performers: Extra sessions, additional assignments, focused guidance

---

## 11. Module 6: Student Application & Screening

### Purpose
Manage student application, screening, and onboarding process with assessment-based selection.

### Application Paths

**College-side Application**
• NetPy provides digital forms to registered colleges
• Colleges distribute forms to students
• Students fill complete details and submit to college
• College admin reviews and submits to NetPy
• NetPy evaluates and processes applications

**Direct Application**
• Students access NetPy portal directly
• Fill complete application forms online
• Submit directly to NetPy for evaluation
• No college intermediary involved

### Screening Process
1. **Application Processing**
   - Automated eligibility checks
   - Document verification
   - Application scoring with AI assistance

2. **Interview Phase**
   - Send interview invitations via email
   - Self-service scheduling system
   - Technical and behavioral assessment

3. **Selection Decision**
   - Final committee review
   - Selection package generation
   - Professional rejection handling

4. **Digital Onboarding**
   - Secure account creation
   - Profile configuration and security setup
   - Dashboard access and community integration

---

## 12. Module 7: Feedback & Performance Review

### Purpose
Collect and analyze feedback from all stakeholders for continuous improvement.

### Feedback Collection

**Student → Mentor Feedback**
• Teaching quality and clarity
• Support and doubt resolution
• Availability and responsiveness

**Student → Course Feedback**
• Content relevance and difficulty
• Practical usefulness and structure
• Suggestions for improvement

**Mentor → Student Feedback**
• Strengths and improvement areas
• Performance and engagement assessment
• Personalized development recommendations

### Feedback Processing
• Aggregate anonymous feedback data
• Calculate metrics and sentiment analysis
• Generate improvement reports
• Implement actionable changes

---

## 13. Module 8: Student Authentication & Dashboard

### Purpose
Provide secure access and personalized dashboard experience for students.

### Authentication Process
• Email/username and password validation
• Two-factor authentication (optional)
• Session management with timeout handling
• Password reset and account recovery

### Dashboard Features
• Personal progress reports and attendance history
• Upcoming classes, meetings, and calendar events
• Assigned projects and resources tracking
• Mentor remarks and performance feedback
• Quick access actions (join class, submit assignment)

---

## 14. End-to-End User Workflows

### Student Lifecycle
```
Application → Screening → Interview → Selection → Onboarding → Learning → Evaluation → Internship Eligible
```

### Batch Lifecycle
```
Created → Active → Monitoring → Completed → Performance Archived
```

### Application Lifecycle
```
Submitted → Verification → Assessment → Interview → Selected/Rejected → Account Created → Activated
```

### Complete Student Journey
1. **Application Phase**: College-side or Direct application with form submission
2. **Screening Phase**: Document verification, eligibility check, interview scheduling
3. **Selection Phase**: Interview evaluation, decision making, offer generation
4. **Onboarding Phase**: Account creation, profile setup, dashboard access
5. **Learning Phase**: Batch assignment, mentor interaction, progress tracking
6. **Evaluation Phase**: Performance assessment, feedback generation, profile building
7. **Internship Phase**: Verified profile visibility, company applications, hiring

---

## 15. Success Metrics

### Academic Performance Metrics
• Student completion rate (>90% target)
• Average attendance rate (>85% target)
• Assignment submission rate (>95% target)
• Mentor-student engagement score
• Performance improvement percentage

### Operational Metrics
• Application processing time (<48 hours)
• Interview scheduling efficiency
• Onboarding completion rate
• System uptime and availability
• User satisfaction scores

### Business Impact Metrics
• College adoption rate
• Direct application conversion
• Verified student profile generation
• Internship placement rate
• Company satisfaction with hired interns

### Governance Metrics
• Role-based access compliance
• Audit trail completeness
• Security incident rate
• Data integrity maintenance
• Cross-college access prevention

---

## Final Outcome

The NetPy M2i LMS platform establishes:
• A structured 5-tier governance hierarchy
• Dual student onboarding paths with screening
• Verified performance-based evaluation system
• Transparent academic tracking and reporting
• Secure authentication and role-based access
• Complete audit compliance and governance
• Industry-ready student profiles for hiring


