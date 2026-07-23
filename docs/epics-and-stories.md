# TODO App Epics and Stories

Based on the requirements in `docs/prd-todo.md` and following the structure in `docs/templates/epic-and-stories-template.md`.

## MVP Epics

- Epic: Extend Task Metadata
  - Story: Capture optional due dates on tasks
    - Acceptance Criteria:
    - Users can create a task without a due date.
    - Users can create a task with a due date in `YYYY-MM-DD` format.
    - Users can edit an existing task and add, change, or clear its due date.
    - Tasks with valid due dates display the saved due date in the task list.
    - Technical Requirements:
    - Continue using the due date input in `/packages/frontend/src/TaskForm.js` and keep it bound to the task payload.
    - Preserve create and edit save handling in `/packages/frontend/src/App.js` for task submissions.
    - Persist due dates through the existing task endpoints in `/packages/backend/src/app.js`.
    - Keep stored due date values aligned to ISO `YYYY-MM-DD`.
  - Story: Add task priority with default `P3`
    - Acceptance Criteria:
    - Users can assign `P1`, `P2`, or `P3` when creating a task.
    - If a priority is not provided, the saved task defaults to `P3`.
    - Users can edit an existing task and change its priority.
    - Saved tasks return the assigned priority when fetched.
    - Technical Requirements:
    - Add priority input handling to `/packages/frontend/src/TaskForm.js` and include the field in requests sent from `/packages/frontend/src/App.js`.
    - Update task rendering in `/packages/frontend/src/TaskList.js` so priority data is available for display and later sorting.
    - Extend the task model in `/packages/backend/src/app.js` to store `priority` with allowed values `P1`, `P2`, and `P3` and default `P3`.
    - Update the existing POST and PUT handlers in `/packages/backend/src/app.js` to accept and return `priority`.

- Epic: Enforce MVP Task Validation
  - Story: Require task titles before save
    - Acceptance Criteria:
    - A task cannot be created without a non-empty title.
    - A task cannot be updated to an empty or whitespace-only title.
    - Users receive validation feedback when a title is invalid.
    - The API rejects invalid create and update requests.
    - Technical Requirements:
    - Keep client-side required-title validation in `/packages/frontend/src/TaskForm.js`.
    - Preserve server-side title validation in the POST and PUT handlers in `/packages/backend/src/app.js`.
    - Add or extend API tests in `/packages/backend/__tests__/tasks.test.js` for invalid title scenarios.
  - Story: Ignore invalid due date values
    - Acceptance Criteria:
    - Invalid due date values are treated as absent.
    - Invalid due dates do not block saving a task with a valid title and priority.
    - Tasks with invalid due date input do not display malformed dates in the UI.
    - Technical Requirements:
    - Validate and normalize due date values in `/packages/frontend/src/TaskForm.js` before save and when loading edit state.
    - Add backend handling in `/packages/backend/src/app.js` so invalid due date values are stored as `null`.
    - Add API coverage in `/packages/backend/__tests__/tasks.test.js` for invalid due date handling.

- Epic: Filter Tasks by Date Status
  - Story: Add `All`, `Today`, and `Overdue` task views
    - Acceptance Criteria:
    - Users can switch between `All`, `Today`, and `Overdue` views.
    - `All` shows all tasks regardless of completion status.
    - `Today` shows only incomplete tasks due on the current date.
    - `Overdue` shows only incomplete tasks with due dates before the current date.
    - Technical Requirements:
    - Add filter state and UI controls in `/packages/frontend/src/App.js` or `/packages/frontend/src/TaskList.js`.
    - Extend the fetch flow in `/packages/frontend/src/TaskList.js` to apply the selected filter.
    - Implement filter support through the existing `/api/tasks` endpoint in `/packages/backend/src/app.js`.
    - Define date comparisons consistently against ISO `YYYY-MM-DD` values to avoid timezone drift.
  - Story: Exclude completed tasks from `Today` and `Overdue`
    - Acceptance Criteria:
    - Completed tasks remain visible in `All`.
    - Completed tasks are hidden in `Today`.
    - Completed tasks are hidden in `Overdue`.
    - Toggling a task to completed updates its visibility correctly in the active filter.
    - Technical Requirements:
    - Reuse the existing completion toggle flow in `/packages/frontend/src/TaskList.js` and `/packages/backend/src/app.js`.
    - Ensure the active filter is reapplied after completion updates refresh the task list.
    - Add API tests in `/packages/backend/__tests__/tasks.test.js` for completion-aware filtering behavior.

- Epic: Preserve Local-Only Storage
  - Story: Keep task management within the current local application boundary
    - Acceptance Criteria:
    - The MVP does not introduce external storage.
    - The MVP does not introduce multi-user or remote synchronization features.
    - The MVP remains operable within the current local frontend and backend project structure.
    - Technical Requirements:
    - Keep implementation within the existing React frontend in `/packages/frontend` and Express backend in `/packages/backend`.
    - Do not add external databases, third-party storage providers, or new backend services.
    - Limit backend changes to the current task API in `/packages/backend/src/app.js`.

## Post-MVP Epics

- Epic: Improve Urgency Visibility
  - Story: Highlight overdue tasks in the list
    - Acceptance Criteria:
    - Incomplete overdue tasks are visually distinct from non-overdue tasks.
    - Completed tasks are not highlighted as overdue.
    - Overdue highlighting remains visible in any view where overdue tasks appear.
    - Technical Requirements:
    - Add overdue-state styling logic to `/packages/frontend/src/TaskList.js`.
    - Compute overdue state from saved due date and completion status without changing the API contract.
    - Keep the styling compatible with the existing Material UI presentation in `/packages/frontend/src/App.js` and `/packages/frontend/src/TaskList.js`.

- Epic: Sort Tasks by Urgency
  - Story: Order tasks by overdue status, priority, due date, and undated fallback
    - Acceptance Criteria:
    - Overdue tasks appear before non-overdue tasks.
    - Within the same overdue group, tasks are sorted by priority from `P1` to `P3`.
    - Within the same priority group, tasks are sorted by due date ascending.
    - Tasks without due dates appear after tasks with due dates.
    - Technical Requirements:
    - Replace the current due-date-first ordering in `/packages/backend/src/app.js` with the Post-MVP sort rules, or apply the same ordering consistently in `/packages/frontend/src/TaskList.js`.
    - Sort priorities using explicit business order rather than lexical comparison.
    - Add API tests in `/packages/backend/__tests__/tasks.test.js` to verify the full mixed-data sort order.

## Out of Scope

- Notifications
- Recurring tasks
- Multi-user support
- Keyboard navigation
- External storage
