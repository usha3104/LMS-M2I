# Module 01: Authentication, Governance & Onboarding Flow

## Purpose
This module defines secure authentication (OTP + password), role-based routing, and onboarding pipelines aligned to M2I governance and application screening.

## Scope
- Secure login and session management (OTP + password, optional 2FA)
- RBAC-based routing for existing users (5-tier governance)
- Dual student onboarding paths (College-side and Direct)
- Mentor and College onboarding with admin verification/approval
- Application screening, interview, selection, and digital onboarding

## Detailed Workflow
- User opens platform.
- Decision: Existing account?
  - Yes:
    - Login with email/username + password.
    - Send OTP.
    - Decision: OTP valid?
      - Yes:
        - Optional 2FA.
        - Session established with timeout handling.
        - Identify role (Super Admin / NetPy Admin / College Admin / Mentor / Student / Company).
        - Redirect to role-specific dashboard.
      - No:
        - Show error and retry OTP.
  - No (new user):
    - Decision: Select role?
      - Student:
        - Decision: Application path?
          - College-side application:
            - College distributes NetPy form.
            - Student submits to College Admin.
            - College Admin reviews and forwards to NetPy.
          - Direct application:
            - Student fills and submits form directly to NetPy.
        - Application processing:
          - Eligibility checks.
          - Document verification.
          - Application scoring (AI-assisted).
        - Interview phase:
          - Send interview invite.
          - Self-service scheduling.
          - Technical + behavioral interview.
        - Decision: Selected?
          - Yes:
            - Digital onboarding:
              - Secure account creation.
              - Profile configuration and security setup.
              - Assign batch (college-specific or general/open).
              - Dashboard access.
          - No:
            - Professional rejection handling.
      - Mentor:
        - Start mentor onboarding.
        - Enter personal information.
        - Add professional experience.
        - Add teaching expertise.
        - Upload certifications.
        - Admin verification.
        - Create mentor profile.
        - Redirect to Mentor Dashboard.
      - College:
        - Start college onboarding.
        - Enter institution details.
        - Add admin contact.
        - Submit program details.
        - Admin approval.
        - Create college profile.
        - Redirect to College Dashboard.
    - Optional: Company onboarding (if enabled) follows admin verification.

## Requirements and Decision Points
- Login decision for existing users vs new applicants.
- OTP validation decision during login.
- Role identification for existing users (5-tier governance + Company).
- Dual application path decision (College-side vs Direct).
- Screening checkpoints: eligibility, document verification, scoring.
- Interview scheduling and completion.
- Selection decision leading to digital onboarding.
- Batch assignment based on applicant type (college-specific or open).
- Admin verification/approval for mentor and college onboarding.
