# Cloud Architecture Overview

This monorepo contains a React frontend, an Express API, and an in-memory SQLite data store. The diagrams below describe the system context and the request flow when a user creates a TODO item.

## System Context

```mermaid
flowchart LR
    user[User]
    frontend[React Frontend\npackages/frontend]
    api[Express API\npackages/backend]
    store[(In-Memory SQLite Store)]

    user -->|Uses browser UI| frontend
    frontend -->|HTTP /api/tasks| api
    api -->|SQL read/write| store
```

## Sequence Diagram: Creating a TODO

```mermaid
sequenceDiagram
    actor User
    participant Frontend as React Frontend
    participant API as Express API
    participant Store as In-Memory SQLite Store

    User->>Frontend: Enter title, description, and optional due date
    User->>Frontend: Submit create task form
    Frontend->>API: POST /api/tasks
    Note over Frontend,API: JSON payload includes title, description, and due_date
    API->>API: Validate title
    API->>Store: INSERT task record
    Store-->>API: Return inserted task id
    API->>Store: SELECT created task
    Store-->>API: Return created task
    API-->>Frontend: 201 Created with task JSON
    Frontend->>API: GET /api/tasks
    API->>Store: SELECT tasks ORDER BY due_date, created_at
    Store-->>API: Return task list
    API-->>Frontend: 200 OK with tasks JSON
    Frontend-->>User: Render updated TODO list
```