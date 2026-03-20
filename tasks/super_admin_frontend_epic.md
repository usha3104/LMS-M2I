# Epic: Super Admin Complete Workflow (Frontend)
**Project:** M2I LMS Frontend  
**Priority:** High  
**Description:** Implement the Next.js frontend capabilities required for the Super Admin complete workflow, including authentication UI, dashboard, and management modules with responsive design and intuitive user experience.  
**Dependencies:** diagrams/01_Super_Admin_Flow.puml, super_admin_epic.md (Backend)

---

# Task 1: Authentication & Authorization UI
**Type:** Task  
**Summary:** Implement Super Admin authentication UI with login form, OTP input, and protected route handling

**Description:** This task implements the complete frontend authentication system for the Super Admin role using Next.js. It involves building a responsive login page with email/password input fields and validation, an OTP verification page that accepts the 6-digit code with countdown timer for resend, and protected route handling using Next.js middleware andHigher-Order Components. The UI implements security best practices including rate limit feedback display (showing remaining attempts), OTP expiry countdown timer, failed attempt lockout messaging, and automatic redirect to login on token expiration. All authenticated pages check for valid JWT and redirect to login if unauthorized.

## Sub-task 1.1: Login Page
**Type:** Sub-task
**Description:** Build the Super Admin login page with email and password input fields. Implement client-side validation for email format and password minimum length, show loading state during authentication, display error messages for invalid credentials, implement rate limit feedback (attempts remaining), and redirect to OTP page on successful credential validation.
- Route: `/auth/login`
- Components: LoginForm, InputField, Button, Alert
- State: email, password, errors, loading, rateLimitInfo
**Acceptance Criteria:**
- [ ] Validates email format before submission
- [ ] Shows loading spinner during API call
- [ ] Displays error message on invalid credentials
- [ ] Shows rate limit info (attempts remaining)
- [ ] Redirects to OTP page on success
**Estimate:** 1 day

## Sub-task 1.2: OTP Verification Page
**Type:** Sub-task
**Description:** Build the OTP verification page with 6-digit input (individual boxes for each digit), countdown timer showing OTP expiry (5 minutes), resend OTP button with cooldown, auto-submit when all digits entered, loading state during verification, error display for invalid/expired OTP, and redirect to dashboard on successful verification.
- Route: `/auth/otp`
- Components: OTPInput, CountdownTimer, ResendButton, Button, Alert
- State: otp, errors, loading, expiryTime, canResend
**Acceptance Criteria:**
- [ ] 6 separate input boxes for OTP digits
- [ ] Auto-focus moves to next input on entry
- [ ] Countdown timer shows remaining time
- [ ] Resend button disabled during cooldown
- [ ] Redirects to dashboard on success
- [ ] Shows lockout message after 3 failed attempts
**Estimate:** 1 day

## Sub-task 1.3: Protected Routes & Auth Context
**Type:** Sub-task
**Description:** Implement Next.js middleware for route protection, create React Context for authentication state (user, token, role), implement authorizeRole HOC for component-level role checks, handle token refresh and logout, store JWT in httpOnly cookie (managed by backend), and redirect unauthorized users to login.
- Route Protection: Middleware + HOC
- Context: AuthContext, useAuth hook
- Components: ProtectedRoute, RoleGuard
**Acceptance Criteria:**
- [ ] Unauthenticated users redirected to /auth/login
- [ ] Non-super_admin users redirected to appropriate dashboard
- [ ] Token expiry triggers automatic logout
- [ ] Auth context provides user data throughout app
**Estimate:** 1 day

---

# Task 2: Dashboard UI
**Type:** Task  
**Summary:** Implement Super Admin dashboard with statistics cards, charts, and activity feed

**Description:** This task implements the main dashboard page for the Super Admin using Next.js. The dashboard serves as the command center displaying real-time statistics through visual cards showing total colleges, active colleges, NetPy admins, mentors, published courses, active batches, enrolled students, and pending applications. It includes interactive charts for trends over time (using Chart.js or Recharts), a recent activity feed showing latest system events, and auto-refresh capability for real-time data. The page uses skeleton loaders during data fetching and handles empty states gracefully.

## Sub-task 2.1: Dashboard Statistics Cards
**Type:** Sub-task
**Description:** Build stat cards displaying key metrics with icons, labels, and values. Each card shows a single metric (colleges, admins, mentors, courses, batches, students, applications), includes loading skeleton, supports click to navigate to detailed view, and displays trend indicators (up/down arrows).
- Route: `/dashboard`
- Components: StatCard, StatGrid, LoadingSkeleton
- Data: totalColleges, activeColleges, totalAdmins, totalMentors, totalCourses, totalBatches, totalStudents, pendingApplications
**Acceptance Criteria:**
- [ ] Displays all 8 statistics cards
- [ ] Shows loading skeleton during fetch
- [ ] Cards are clickable to detailed views
- [ ] Responsive grid layout (1-4 columns)
**Estimate:** 1 day

## Sub-task 2.2: Dashboard Charts & Activity Feed
**Type:** Sub-task
**Description:** Build interactive charts showing trends over time (line chart for enrollments, bar chart for course completions), build activity feed component showing recent events with timestamps, implement auto-refresh every 30 seconds, and handle empty states with appropriate messaging.
- Components: LineChart, BarChart, ActivityFeed, ActivityItem
**Acceptance Criteria:**
- [ ] Line chart shows enrollment trends
- [ ] Bar chart shows course completion data
- [ ] Activity feed shows latest 10 activities
- [ ] Auto-refresh updates data periodically
- [ ] Charts are interactive (hover tooltips)
**Estimate:** 1 day

---

# Task 3: College Management UI
**Type:** Task  
**Summary:** Implement college CRUD pages with forms, tables, and detail views

**Description:** This task implements the complete college management UI for Super Admins in Next.js. It includes a college list page with data table featuring sorting, pagination, search, and status filters, a create college form page with validation and multi-step wizard, an edit college form page with pre-filled data, and a college detail view showing full information with associated admins. The UI implements cascade deactivation confirmation dialogs and handles all error states gracefully.

## Sub-task 3.1: College List Page
**Type:** Sub-task
**Description:** Build the colleges listing page with data table showing all colleges, columns: name, code, status, contact email, established year, actions. Implement search by name/code, filter by status (active/inactive), pagination (10/25/50 per page), sorting by any column, and action buttons (view, edit, activate/deactivate).
- Route: `/colleges`
- Components: CollegeTable, CollegeRow, StatusBadge, SearchInput, FilterDropdown, Pagination
- Query params: page, limit, status, search
**Acceptance Criteria:**
- [ ] Table displays all college data
- [ ] Search filters by name and code
- [ ] Status filter works correctly
- [ ] Pagination works with filters
- [ ] Action buttons navigate correctly
**Estimate:** 1 day

## Sub-task 3.2: Create/Edit College Form
**Type:** Sub-task
**Description:** Build create college form with fields: name, code, address, contact email, contact phone, website, established year. Implement client-side validation (email format, phone format, year range), show inline validation errors, disable submit during API call, redirect to list on success, and build edit form with pre-populated data.
- Route: `/colleges/create`, `/colleges/:id/edit`
- Components: CollegeForm, FormField, FormValidation, SubmitButton
- Validation: required fields, email format, unique code check
**Acceptance Criteria:**
- [ ] All form fields render correctly
- [ ] Client-side validation works
- [ ] Shows inline error messages
- [ ] Redirects on successful creation
- [ ] Pre-populates data in edit mode
**Estimate:** 1 day

## Sub-task 3.3: College Detail & Status Toggle
**Type:** Sub-task
**Description:** Build college detail page showing full college information, build confirmation dialog for status change (activate/deactivate), show warning when deactivating (cascades to admins), implement optimistic UI update, and refresh data after status change.
- Route: `/colleges/:id`
- Components: CollegeDetail, StatusToggle, ConfirmDialog, WarningBanner
**Acceptance Criteria:**
- [ ] Displays complete college information
- [ ] Status toggle shows confirmation dialog
- [ ] Warning displayed for deactivation
- [ ] Cascade deactivation explained in dialog
- [ ] UI updates optimistically
**Estimate:** 1 day

---

# Task 4: NetPy Admin Management UI
**Type:** Task  
**Summary:** Implement NetPy Admin CRUD pages with assignment and permissions management

**Description:** This task implements comprehensive NetPy Admin management UI for Super Admins using Next.js. It includes admin list page with filtering by status and college, create admin form with email/password fields and college assignment, college assignment interface with multi-select dropdown, permissions management with checkbox group, and status toggle with confirmation. The UI provides visual feedback for permissions and handles all CRUD operations.

## Sub-task 4.1: Admin List Page
**Type:** Sub-task
**Description:** Build the NetPy Admins listing page with data table, columns: name, email, phone, assigned colleges (count), permissions (badges), status, actions. Implement filter by status (active/inactive), filter by assigned college, search by name/email, pagination, and action buttons.
- Route: `/admins`
- Components: AdminTable, AdminRow, PermissionBadge, CollegeCount, FilterBar
- Query params: page, limit, status, collegeId
**Acceptance Criteria:**
- [ ] Table displays all admin data
- [ ] Filters work correctly
- [ ] Search functions properly
- [ ] Pagination with filters works
**Estimate:** 1 day

## Sub-task 4.2: Create Admin & College Assignment
**Type:** Sub-task
**Description:** Build create admin form with name, email, phone fields, build college assignment multi-select component showing active colleges, implement search within college dropdown, show selected colleges as chips/tags, build permissions selection with checkbox group from predefined set, and show email sent confirmation after creation.
- Route: `/admins/create`, `/admins/:id/edit`
- Components: AdminForm, CollegeMultiSelect, PermissionCheckboxGroup, ChipList, SuccessToast
**Acceptance Criteria:**
- [ ] Form validates all required fields
- [ ] College multi-select works
- [ ] Permission checkboxes function correctly
- [ ] Shows success message after creation
**Estimate:** 1 day

## Sub-task 4.3: Manage Permissions UI
**Type:** Sub-task
**Description:** Build permissions management interface showing current permissions, checkbox group for available permissions, visual indication of permission categories, save button with loading state, and success/error feedback.
- Route: `/admins/:id/permissions`
- Components: PermissionManager, PermissionCategory, PermissionCheckbox
**Acceptance Criteria:**
- [ ] Current permissions displayed
- [ ] Checkboxes enable/disable correctly
- [ ] Changes save successfully
- [ ] Success feedback shown
**Estimate:** 1 day

---

# Task 5: Course Management UI
**Type:** Task  
**Summary:** Implement course management pages with module builder and evaluation criteria editor

**Description:** This task implements the complete course management UI for Super Admins in Next.js. It includes course list with status and category filters, create/edit course form, module builder with drag-and-drop reordering, evaluation criteria editor with weight sliders that sum to 100%, and publish workflow with prerequisite validation. The UI provides visual feedback for course status and guides the Super Admin through the publishing process.

## Sub-task 5.1: Course List Page
**Type:** Sub-task
**Description:** Build courses listing page with data table, columns: title, category, status, module count, has evaluation, created date, actions. Implement filter by status (draft/published), filter by category, search by title, pagination, and status badges with colors.
- Route: `/courses`
- Components: CourseTable, CourseRow, StatusBadge, CategoryBadge, FilterBar
- Query params: page, limit, status, category
**Acceptance Criteria:**
- [ ] Table displays all course data
- [ ] Status filter works
- [ ] Category filter works
- [ ] Search functions properly
**Estimate:** 1 day

## Sub-task 5.2: Create/Edit Course Form
**Type:** Sub-task
**Description:** Build create course form with title, description, category (dropdown), duration, prerequisites (multi-select of existing courses). Implement rich text editor for description, client-side validation, and build edit form with pre-populated data.
- Route: `/courses/create`, `/courses/:id/edit`
- Components: CourseForm, RichTextEditor, CategorySelect, PrerequisitesMultiSelect
**Acceptance Criteria:**
- [ ] All form fields render correctly
- [ ] Rich text editor works
- [ ] Prerequisites selection works
- [ ] Validation displays errors
**Estimate:** 1 day

## Sub-task 5.3: Module Builder
**Type:** Sub-task
**Description:** Build module builder interface for adding/editing course modules, each module has title, description, order, duration, content fields. Implement drag-and-drop reordering using react-beautiful-dnd or @dnd-kit, add/remove module buttons, inline module editing, and save modules button.
- Route: `/courses/:id/modules`
- Components: ModuleBuilder, ModuleCard, ModuleForm, DragDropContext, SaveButton
- State: modules array with CRUD operations
**Acceptance Criteria:**
- [ ] Modules can be added
- [ ] Modules can be edited
- [ ] Drag-drop reordering works
- [ ] Order persists after save
**Estimate:** 2 days

## Sub-task 5.4: Evaluation Criteria Editor
**Type:** Sub-task
**Description:** Build evaluation criteria editor with dynamic criteria rows, each row has type dropdown (quiz, assignment, exam, project) and weight input (0-100). Real-time sum calculation displayed at bottom, validation error if sum ≠ 100, add/remove criteria buttons, and save button.
- Route: `/courses/:id/evaluation`
- Components: EvaluationEditor, CriteriaRow, WeightInput, SumDisplay, ValidationAlert
- State: criteria array, totalWeight
**Acceptance Criteria:**
- [ ] Criteria can be added/removed
- [ ] Weight sum calculated in real-time
- [ ] Error shown if sum ≠ 100
- [ ] Saves successfully
**Estimate:** 1 day

## Sub-task 5.5: Publish Workflow
**Type:** Sub-task
**Description:** Build publish button/modal showing prerequisites checklist, checklist items: has modules (count), has evaluation criteria (sum=100), visual checkmarks for met prerequisites, warning messages for unmet items, publish confirmation dialog, and post-publish redirect to course list with success toast.
- Route: `/courses/:id/publish`
- Components: PublishButton, PrerequisitesChecklist, PublishModal, SuccessToast
- Validation: modules > 0, evaluation criteria sum = 100
**Acceptance Criteria:**
- [ ] Prerequisites displayed correctly
- [ ] Cannot publish if prerequisites not met
- [ ] Publish confirmation shown
- [ ] Success toast after publish
**Estimate:** 1 day

---

# Task 6: Batch Management UI
**Type:** Task  
**Summary:** Implement batch management pages with mentor assignment and scheduling UI

**Description:** This task implements the complete batch management UI for Super Admins in Next.js. It includes batch list with status and course filters, create batch form with date pickers, mentor assignment interface with mentor selector, student enrollment with capacity management, and schedule builder with day/time selection. The UI provides visual feedback for batch status and handles all management operations.

## Sub-task 6.1: Batch List Page
**Type:** Sub-task
**Description:** Build batches listing page with data table, columns: name, course, mentor, start date, end date, student count/max, status, actions. Implement filter by status (draft/active/completed), filter by course, search by name, pagination, and status badges.
- Route: `/batches`
- Components: BatchTable, BatchRow, MentorBadge, DateRange, StatusBadge, FilterBar
- Query params: page, limit, status, courseId
**Acceptance Criteria:**
- [ ] Table displays all batch data
- [ ] Filters work correctly
- [ ] Pagination functions
**Estimate:** 1 day

## Sub-task 6.2: Create Batch Form
**Type:** Sub-task
**Description:** Build create batch form with name, course dropdown (only published courses), start date picker, end date picker, max students input. Implement date validation (end > start), capacity validation (min 1), show course details on selection, and redirect to list on success.
- Route: `/batches/create`
- Components: BatchForm, CourseSelect, DateRangePicker, CapacityInput
**Acceptance Criteria:**
- [ ] All fields render correctly
- [ ] Date validation works
- [ ] Only published courses shown
- [ ] Redirects on success
**Estimate:** 1 day

## Sub-task 6.3: Mentor Assignment & Student Enrollment
**Type:** Sub-task
**Description:** Build mentor assignment dropdown showing active mentors, build student enrollment interface with search/select multiple approved students, show current enrollment vs capacity, show warning when batch full, implement add/remove students functionality, and display student list with remove buttons.
- Route: `/batches/:id/manage`
- Components: MentorSelect, StudentEnroll, StudentList, CapacityIndicator, AddStudentModal
**Acceptance Criteria:**
- [ ] Mentor can be assigned
- [ ] Students can be added
- [ ] Capacity limit enforced
- [ ] Students can be removed
**Estimate:** 1 day

## Sub-task 6.4: Schedule Builder
**Type:** Sub-task
**Description:** Build schedule builder interface with day selector (Mon-Sun), time pickers for start/end, module selector for each session, add multiple sessions, visual timeline view of schedule, conflict detection warning, and save schedule button.
- Route: `/batches/:id/schedule`
- Components: ScheduleBuilder, DaySelector, TimePicker, ModuleSelect, SessionCard, ScheduleTimeline
**Acceptance Criteria:**
- [ ] Sessions can be added
- [ ] Day and time selection works
- [ ] Module selection functions
- [ ] Schedule saves correctly
**Estimate:** 1 day

---

# Task 7: Student Applications UI
**Type:** Task  
**Summary:** Implement application review pages with document viewer, interview scheduler, and approval workflow

**Description:** This task implements the complete student application processing UI for Super Admins in Next.js. It includes application list with status and college filters, application detail view with document viewer/verification, interview scheduler with date/time picker, and approval/rejection workflow with reason input. The UI provides step-by-step guidance through the application process and handles all status transitions.

## Sub-task 7.1: Applications List Page
**Type:** Sub-task
**Description:** Build applications listing page with data table, columns: applicant name, email, applied college, applied course, status, submission date, actions. Implement filter by status (pending/verified/interview_scheduled/approved/rejected), filter by college, search by name/email, pagination, and status badges with colors.
- Route: `/applications`
- Components: ApplicationTable, ApplicationRow, StatusBadge, FilterBar, SearchInput
- Query params: page, limit, status, collegeId
**Acceptance Criteria:**
- [ ] Table displays all applications
- [ ] Status filter works
- [ ] College filter works
- [ ] Pagination functions
**Estimate:** 1 day

## Sub-task 7.2: Application Detail & Document Verification
**Type:** Sub-task
**Description:** Build application detail page with applicant info card, uploaded documents section with preview (PDF viewer, image viewer), verify documents toggle with notes input, verification timestamp display, and save verification button.
- Route: `/applications/:id`
- Components: ApplicantInfo, DocumentList, DocumentViewer, VerificationForm, TimestampDisplay
**Acceptance Criteria:**
- [ ] Applicant info displayed
- [ ] Documents can be viewed
- [ ] Verification can be toggled
- [ ] Notes can be added
- [ ] Verification saved
**Estimate:** 1 day

## Sub-task 7.3: Interview Scheduler
**Type:** Sub-task
**Description:** Build interview scheduling interface with date picker (future dates only), time picker, interview mode selector (video/in-person), interview link input (for video), preview of scheduled interview, send notification toggle, and schedule button.
- Route: `/applications/:id/interview`
- Components: InterviewScheduler, DatePicker, TimePicker, ModeSelector, LinkInput, NotificationToggle
**Acceptance Criteria:**
- [ ] Date picker restricts to future
- [ ] Time picker functions
- [ ] Mode selection works
- [ ] Notification sent option
- [ ] Interview scheduled
**Estimate:** 1 day

## Sub-task 7.4: Approval/Rejection Workflow
**Type:** Sub-task
**Description:** Build decision form with approve/reject radio buttons, reason textarea (optional for approve, required for reject), show warning if documents not verified, confirmation dialog with decision summary, success/error feedback, and redirect to list after decision.
- Route: `/applications/:id/decision`
- Components: DecisionForm, DecisionRadio, ReasonInput, ConfirmDialog, WarningBanner, SuccessToast
**Acceptance Criteria:**
- [ ] Approve/reject selection works
- [ ] Reason validation for reject
- [ ] Warning if docs not verified
- [ ] Confirmation dialog shown
- [ ] Decision saved
**Estimate:** 1 day

---

# Task 8: Reports & Monitoring UI
**Type:** Task  
**Summary:** Implement reporting pages with charts, tables, and export functionality

**Description:** This task implements comprehensive reporting and monitoring UI for Super Admins in Next.js. It includes overview reports with summary statistics and charts, student performance reports with detailed tables and filters, and batch progress reports with completion tracking. The UI provides interactive charts, sortable/filterable tables, and data export capabilities for all reports.

## Sub-task 8.1: Overview Reports
**Type:** Sub-task
**Description:** Build overview reports page with summary statistics cards (total students, active batches, completion rate), line chart showing enrollment trends over time, bar chart showing course completion by category, recent activity list with load more, and date range selector for filtering.
- Route: `/reports`
- Components: OverviewStats, TrendChart, CategoryChart, ActivityList, DateRangeSelector
**Acceptance Criteria:**
- [ ] Statistics cards display
- [ ] Charts render with data
- [ ] Date range filter works
- [ ] Activity list loads
**Estimate:** 1 day

## Sub-task 8.2: Student Performance Reports
**Type:** Sub-task
**Description:** Build student performance table with columns: name, email, batch, course, attendance %, grades, progress %. Implement filter by batch, filter by course, search by name, sortable columns, attendance indicator (color-coded), grades display, progress bar, and pagination.
- Route: `/reports/students`
- Components: PerformanceTable, PerformanceRow, AttendanceIndicator, ProgressBar, GradeDisplay, FilterBar
- Query params: page, limit, batchId, courseId
**Acceptance Criteria:**
- [ ] Table displays student data
- [ ] Batch filter works
- [ ] Course filter works
- [ ] Sorting functions
- [ ] Pagination works
**Estimate:** 2 days

## Sub-task 8.3: Batch Progress Reports
**Type:** Sub-task
**Description:** Build batch progress table with columns: name, college, course, progress %, average attendance, upcoming sessions, status. Implement filter by college, filter by status, progress bar visualization, attendance indicator, upcoming schedule preview, and pagination.
- Route: `/reports/batches`
- Components: BatchProgressTable, BatchProgressRow, ProgressBar, AttendanceBadge, SchedulePreview, FilterBar
- Query params: page, limit, collegeId, status
**Acceptance Criteria:**
- [ ] Table displays batch data
- [ ] College filter works
- [ ] Status filter works
- [ ] Progress bars display
- [ ] Pagination works
**Estimate:** 2 days

---

# Task 9: Mentor Management UI
**Type:** Task  
**Summary:** Implement mentor management pages with profile, assignment, and permissions UI

**Description:** This task implements comprehensive mentor management UI for Super Admins in Next.js. It includes mentor list with status and college filters, create mentor form, mentor profile detail view, college and batch assignment interfaces, permissions management with checkbox group, and status toggle with confirmation. The UI provides visual feedback for mentor status and handles all management operations.

## Sub-task 9.1: Mentor List Page
**Type:** Sub-task
**Description:** Build mentors listing page with data table, columns: name, email, phone, qualification, assigned college, assigned batch, status, actions. Implement filter by status (active/inactive), filter by college, filter by batch, search by name/email, pagination, and action buttons.
- Route: `/mentors`
- Components: MentorTable, MentorRow, CollegeBadge, BatchBadge, StatusBadge, FilterBar
- Query params: page, limit, status, collegeId, batchId
**Acceptance Criteria:**
- [ ] Table displays all mentor data
- [ ] All filters work
- [ ] Search functions
- [ ] Pagination works
**Estimate:** 1 day

## Sub-task 9.2: Create Mentor Profile
**Type:** Sub-task
**Description:** Build create mentor form with name, email, phone, qualification, experience, profile photo upload (with preview). Implement client-side validation, image upload with preview, disable submit during API call, and success message after creation.
- Route: `/mentors/create`
- Components: MentorForm, PhotoUpload, PhotoPreview, FormValidation
**Acceptance Criteria:**
- [ ] All form fields render
- [ ] Photo upload works
- [ ] Validation displays errors
- [ ] Success shown after creation
**Estimate:** 1 day

## Sub-task 9.3: College & Batch Assignment
**Type:** Sub-task
**Description:** Build college assignment dropdown showing active colleges, build batch assignment dropdown showing active batches, show current assignments as badges, implement add/remove assignments, show validation (cannot assign inactive college/batch), and save assignments button.
- Route: `/mentors/:id/assign`
- Components: CollegeAssign, BatchAssign, AssignmentBadges, ValidationMessage
**Acceptance Criteria:**
- [ ] College assignment works
- [ ] Batch assignment works
- [ ] Current assignments shown
- [ ] Save functions
**Estimate:** 1 day

## Sub-task 9.4: Permissions & Status Management
**Type:** Sub-task
**Description:** Build permissions checkbox group with predefined permissions (view_students, update_attendance, update_marks, view_reports), show current permissions checked, build status toggle with confirmation dialog (deactivation warning), implement optimistic UI update, and success feedback.
- Route: `/mentors/:id/permissions`
- Components: PermissionGroup, StatusToggle, ConfirmDialog, WarningBanner, SuccessToast
**Acceptance Criteria:**
- [ ] Permissions displayed
- [ ] Permissions can be changed
- [ ] Status toggle works
- [ ] Confirmation shown
**Estimate:** 1 day

---

# Implementation Order (Recommended)

| Phase | Tasks | Description |
|-------|-------|-------------|
| 1 | Task 1 | Authentication UI (Login, OTP, Protected Routes) |
| 2 | Task 2, 3, 4 | Dashboard, College, Admin management |
| 3 | Task 5, 6, 9 | Course, Batch & Mentor management |
| 4 | Task 7 | Application processing |
| 5 | Task 8 | Reports & Monitoring |

---

# Technical Dependencies

**Framework:** Next.js 14+ (App Router)
**UI Library:** Tailwind CSS, shadcn/ui
**State Management:** React Context + SWR/TanStack Query
**Forms:** React Hook Form + Zod
**Charts:** Recharts or Chart.js
**Drag & Drop:** @dnd-kit or react-beautiful-dnd
**Date Handling:** date-fns
**HTTP Client:** Axios or Fetch with custom hooks

---

# Notes
- All pages require loading states (skeleton loaders)
- Error states should show user-friendly messages
- Forms should have inline validation feedback
- Tables should be sortable and filterable
- Use optimistic UI updates where appropriate
- Implement proper empty states for all lists
- Mobile-responsive design for all components
- Accessibility: WCAG 2.1 AA compliance
