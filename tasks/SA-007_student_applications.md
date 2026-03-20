# Subtask: Student Applications

## Type
Sub-task

## Description
Build application review and decision flow endpoints for Super Admin.

## Requirements (from diagram)
- Review Applications
- Verify Documents
- Schedule Interview
- Approve / Reject Candidates

## Acceptance Criteria
- `GET /super-admin/applications` lists applications for review.
- `PATCH /super-admin/applications/:id/verify` updates document verification status.
- `PATCH /super-admin/applications/:id/interview` schedules interview.
- `PATCH /super-admin/applications/:id/decision` approves or rejects candidate.
- All endpoints require `super_admin` role.
