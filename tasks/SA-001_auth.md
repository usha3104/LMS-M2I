# Subtask: Super Admin Authentication (Email/Password + OTP)

## Type
Sub-task

## Description
Build Express.js authentication flow for Super Admin: login with email/password, OTP generation/verification, JWT issuance, and failure handling.

## Requirements (from diagram)
- Super Admin Login
- Enter Email & Password
- OTP Authentication
- Authentication Successful? yes/no
- Authentication Failed → Retry Login

## Acceptance Criteria
- `POST /auth/super-admin/login` validates email/password and triggers OTP.
- `POST /auth/super-admin/otp/verify` validates OTP and issues JWT.
- Invalid credentials or OTP return appropriate error codes.
- JWT includes role claim `super_admin`.

## Notes
- OTP expiry and retry limits should be enforced.
