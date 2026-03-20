# Subtask: College Management

## Type
Sub-task

## Description
Create Express.js endpoints and services for Super Admin to manage colleges.

## Requirements (from diagram)
- Create College
- Edit College Details
- Activate / Deactivate College

## Acceptance Criteria
- `POST /super-admin/colleges` creates a college.
- `PUT /super-admin/colleges/:id` updates college details.
- `PATCH /super-admin/colleges/:id/status` activates/deactivates a college.
- All endpoints require `super_admin` role.
