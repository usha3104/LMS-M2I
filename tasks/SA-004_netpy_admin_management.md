# Subtask: NetPy Admin Management

## Type
Sub-task

## Description
Implement NetPy Admin management capabilities for Super Admin.

## Requirements (from diagram)
- Create NetPy Admin
- Assign Colleges
- Manage Admin Permissions

## Acceptance Criteria
- `POST /super-admin/admins` creates a NetPy Admin user.
- `PATCH /super-admin/admins/:id/colleges` assigns colleges.
- `PATCH /super-admin/admins/:id/permissions` updates admin permissions.
- All endpoints require `super_admin` role.
