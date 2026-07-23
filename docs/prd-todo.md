# Product Requirements Document (PRD) - TODO App Upgrade

## 1. Overview

Upgrade the basic TODO app so users can manage deadlines and urgency without increasing product complexity or introducing backend changes. The MVP focuses on adding due dates, priorities, and date-based filters while keeping storage local only. Post-MVP work can improve task visibility and ordering, and several requested capabilities remain explicitly out of scope.

---

## 2. MVP Scope

- Add an optional `dueDate` field for each task using ISO `YYYY-MM-DD` format.
- Add a `priority` field with allowed values `P1`, `P2`, and `P3`.
- Default `priority` to `P3` when it is not provided.
- Add filter views for `All`, `Today`, and `Overdue`.
- In the `All` filter, completed tasks should still be shown.
- In the `Today` and `Overdue` filters, only incomplete tasks should be shown.
- Keep storage local only.
- Do not introduce backend changes or external storage.
- Require `title` for each task.
- Ignore invalid `dueDate` values and treat them as absent.

---

## 3. Post-MVP Scope

- Visually highlight overdue tasks.
- Add sorting with the following order:
- overdue tasks first
- then priority from `P1` to `P3`
- then due date in ascending order
- tasks without a due date last

---

## 4. Out of Scope

- Notifications.
- Recurring tasks.
- Multi-user support.
- Keyboard navigation.
- External storage.