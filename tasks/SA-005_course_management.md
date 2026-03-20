# Subtask: Course Management (Master Courses)

## Type
Sub-task

## Description
Implement master course management for Super Admin.

## Requirements (from diagram)
- Create Master Course
- Define Modules
- Set Evaluation Criteria
- Publish Course

## Acceptance Criteria
- `POST /super-admin/courses` creates a master course.
- `POST /super-admin/courses/:id/modules` defines course modules.
- `PATCH /super-admin/courses/:id/evaluation` sets evaluation criteria.
- `PATCH /super-admin/courses/:id/publish` publishes the course.
- All endpoints require `super_admin` role.
