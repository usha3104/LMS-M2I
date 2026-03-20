# Subtask: Super Admin Dashboard Access

## Type
Sub-task

## Description
Implement dashboard access endpoints and authorization for authenticated Super Admins.

## Requirements (from diagram)
- Access Super Admin Dashboard
- Return to Dashboard

## Acceptance Criteria
- `GET /super-admin/dashboard` returns summary data needed by UI.
- Endpoint is protected by JWT and role `super_admin`.
