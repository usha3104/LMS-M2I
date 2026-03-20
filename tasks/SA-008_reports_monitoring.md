# Subtask: Reports & Monitoring

## Type
Sub-task

## Description
Implement reporting and monitoring endpoints for Super Admin.

## Requirements (from diagram)
- View Reports
- Track Student Performance
- Monitor Batch Progress

## Acceptance Criteria
- `GET /super-admin/reports` returns report summaries.
- `GET /super-admin/reports/students` returns student performance metrics.
- `GET /super-admin/reports/batches` returns batch progress metrics.
- All endpoints require `super_admin` role.
