# Subtask: Batch Management

## Type
Sub-task

## Description
Implement batch management capabilities for Super Admin.

## Requirements (from diagram)
- Create Batch
- Assign Mentor
- Add Students
- Set Schedule

## Acceptance Criteria
- `POST /super-admin/batches` creates a batch.
- `PATCH /super-admin/batches/:id/mentor` assigns mentor.
- `PATCH /super-admin/batches/:id/students` adds students.
- `PATCH /super-admin/batches/:id/schedule` sets schedule.
- All endpoints require `super_admin` role.
