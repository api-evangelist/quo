---
name: Manage Quo tasks
description: Create tasks linked to a phone number or conversation, assign users, and complete them.
api: Quo Public API v1
base_url: https://api.quo.com
auth: apiKey in Authorization header (no Bearer prefix)
operations:
  - createTask_v1
  - assignTask_v1
  - listTasks_v1
  - completeTask_v1
---

# Manage Quo tasks

Create and drive Quo tasks tied to conversations or phone numbers.

## Steps

1. **Create a task.** `POST /v1/tasks` (`createTask_v1`) with a title/description and **exactly one** of `phoneNumberId`, `conversationId`, or `activityId`.
2. **Assign it.** `POST /v1/tasks/{taskId}/assign` (`assignTask_v1`) with the user(s) to notify. Set/clear a due date with `changeTaskDueDate_v1` / `removeTaskDueDate_v1`.
3. **Track and close.** `GET /v1/tasks` (`listTasks_v1`) to monitor; `POST /v1/tasks/{taskId}/complete` (`completeTask_v1`) when done (or `reopenTask_v1` to reopen).

## Rules

- Auth: raw API key in the `Authorization` header (no `Bearer`).
- Providing more than one of `phoneNumberId` / `conversationId` / `activityId` is invalid (`400`).
- The Tasks API shipped 2026-06-08 with 13 endpoints; assignment, due-date, and conversation link/unlink are separate operations.
- Respect the 10 req/s per-key rate limit; back off on `429`.
