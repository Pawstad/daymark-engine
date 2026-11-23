# Daymark — Product Requirements Document (PRD)

**Document Version:** 1.0  
**Status:** Draft / Product Baseline  
**Product:** Daymark  
**Platform:** Web and Mobile  
**Last Updated:** 2026-11-23

---

# 1. Product Overview

## 1.1 Product Vision

Daymark is a personal habit and schedule management application designed to help individuals build consistent habits, manage their daily activities, and make meaningful progress through focused execution.

Daymark aims to help users prioritize what matters instead of overwhelming them with too many commitments.

The product follows a habit-first approach: users identify a small number of important habits, organize their schedules around daily activities, and use focused sessions to support consistent execution.

The long-term vision is to become a personal productivity companion that helps users build sustainable routines through structured planning, focused work, and measurable progress.

## 1.2 Product Description

Daymark combines three closely related capabilities:

1. **Habit Management** — Define and track recurring behaviors that users want to build.
2. **Schedule Management** — Organize activities and commitments based on time or deadlines.
3. **Focus Sessions** — Help users execute planned activities through a Pomodoro-based focus mechanism.

The application also provides reminders and progress monitoring to help users maintain consistency.

Unlike general-purpose task management applications, Daymark prioritizes habit development and focused execution rather than managing large lists of tasks.

The MVP intentionally limits active habits to one or two at a time to encourage users to focus on a small number of meaningful behavioral changes.

## 1.3 Target Users

Daymark targets individuals who want to improve their daily routines, personal productivity, and consistency.

Primary user segments:

| User Segment | Description | Main Needs |
|---|---|---|
| Students | Individuals balancing academic activities and personal development. | Build study routines, manage schedules, and maintain learning consistency. |
| Professionals | Individuals managing work responsibilities and personal goals. | Organize daily activities and establish sustainable routines. |
| Self-improvement Enthusiasts | Individuals who actively work on personal development. | Track habits, monitor progress, and improve consistency. |
| Beginners in Habit Building | Individuals who struggle to maintain routines. | Start with a manageable number of habits and avoid overwhelming commitments. |

Daymark is intended for users with different levels of productivity experience, from beginners to experienced users.

## 1.4 Core Product Principles

The product is built around the following principles:

**1. Habit-first**

Habits are the primary product focus. Other features should support habit execution and consistency.

**2. Focus over quantity**

Users should prioritize a small number of meaningful habits instead of creating too many commitments.

**3. Execution matters**

Planning alone is insufficient. Daymark provides focus sessions to support the actual execution of activities.

**4. Progress should be measurable**

Users should be able to understand their consistency and progress through historical records and visual indicators.

**5. Simple and approachable**

The product should avoid unnecessary complexity and reduce the cognitive burden of managing daily activities.

---

# 2. Product Goals and Objectives

## 2.1 Primary Goals

Daymark aims to:

1. Help users establish and maintain consistent habits.
2. Encourage users to prioritize one or two important habits.
3. Provide a simple way to organize daily schedules.
4. Support focused execution through Pomodoro sessions.
5. Provide visibility into habit consistency and personal progress.
6. Reduce friction between planning and executing daily activities.

## 2.2 Success Indicators

The MVP should establish measurable indicators to evaluate product effectiveness.

| Metric | Purpose |
|---|---|
| Habit Activation Rate | Measures how many users create and activate at least one habit. |
| Habit Completion Rate | Measures the proportion of scheduled habit occurrences completed. |
| Weekly Active Users | Measures recurring product engagement. |
| Focus Session Completion Rate | Measures how often users complete their intended focus sessions. |
| Habit Retention | Measures whether users continue tracking their habits over time. |
| Schedule Engagement | Measures how frequently users create and interact with schedules. |

Exact targets and measurement periods will be defined after the initial product validation.

---

# 3. Technology Stack

Daymark consists of three independently maintained repositories.

## 3.1 Repository Structure

| Repository | Responsibility | Technology |
|---|---|---|
| Backend | Business logic, REST API, authentication, persistence, background processing. | Go, PostgreSQL, Redis, River |
| Mobile | Cross-platform mobile application. | Flutter, Dart |
| Frontend | Web application. | React, Vite, TypeScript |

## 3.2 Backend Technology

| Area | Technology |
|---|---|
| Language | Go |
| Architecture | Modular Monolith |
| HTTP Router | Chi |
| API Architecture | REST API |
| API Specification | OpenAPI 3.x |
| API Documentation | Swagger UI |
| API Code Generation | oapi-codegen |
| Database | PostgreSQL |
| Database Driver | pgx |
| Query Generation | sqlc |
| Migration | Goose |
| Caching | Redis |
| Background Jobs | River |
| Authentication | Opaque Access and Refresh Tokens |
| Password Hashing | Argon2id |
| Containerization | Docker |
| Local Environment | Docker Compose |

## 3.3 Mobile Technology

| Area | Technology |
|---|---|
| Framework | Flutter |
| Language | Dart |
| State Management | Riverpod |
| HTTP Client | Dio |
| Routing | GoRouter |
| Secure Storage | flutter_secure_storage |
| Local Notifications | flutter_local_notifications |
| Model Generation | Freezed and json_serializable |

## 3.4 Frontend Technology

| Area | Technology |
|---|---|
| Framework | React |
| Build Tool | Vite |
| Language | TypeScript |
| CSS Framework | Tailwind CSS |
| Server State | TanStack Query |
| Routing | React Router |
| Form Management | React Hook Form |
| Validation | Zod |
| UI Components | shadcn/ui |
| Testing | Vitest, React Testing Library, Playwright |

## 3.5 Shared Development Infrastructure

| Area | Technology |
|---|---|
| Version Control | Git |
| Repository Hosting | GitHub |
| CI/CD | GitHub Actions |
| API Contract | OpenAPI |
| API Exploration | Swagger UI |
| Containerization | Docker |

Each repository must have its own development instructions, environment configuration, and CI workflow.

The backend repository owns the official API specification consumed by the mobile and frontend repositories.

---

# 4. Product Requirements

## 4.1 Functional Requirements

Functional requirements describe the capabilities that Daymark must provide.

### 4.1.1 User and Authentication

| ID | Requirement | Priority |
|---|---|---|
| FR-001 | Users must be able to register an account. | Must |
| FR-002 | Users must be able to authenticate and log out. | Must |
| FR-003 | Users must be able to maintain their account profile. | Must |
| FR-004 | The system must protect private user data through authentication and authorization. | Must |
| FR-005 | Users must be able to manage active login sessions. | Should |

### 4.1.2 Habit Management

| ID | Requirement | Priority |
|---|---|---|
| FR-006 | Users must be able to create a habit. | Must |
| FR-007 | Users must be able to assign a category to a habit. | Must |
| FR-008 | Users must be able to select from predefined habit categories. | Must |
| FR-009 | Users must initially be limited to a maximum of two active habits. | Must |
| FR-010 | Users must be able to define habit frequency. | Must |
| FR-011 | Users must be able to view their active habits. | Must |
| FR-012 | Users must be able to update their habit information. | Must |
| FR-013 | Users must be able to archive a habit. | Must |
| FR-014 | The system must preserve historical habit records after a habit is archived. | Must |
| FR-015 | Users must be able to view habit completion history. | Must |
| FR-016 | The system must support habit completion tracking based on defined completion criteria. | Must |

Initial habit categories may include:

- Learning
- Reading
- Fitness
- Health
- Personal Development
- Productivity
- Other

The category list should be extensible without requiring fundamental changes to the habit model.

### 4.1.3 Schedule Management

| ID | Requirement | Priority |
|---|---|---|
| FR-017 | Users must be able to create a schedule. | Must |
| FR-018 | Users must be able to define scheduled activities using time-based or deadline-based information. | Must |
| FR-019 | Users must be able to view their schedules. | Must |
| FR-020 | Users must be able to update schedules. | Must |
| FR-021 | Users must be able to delete schedules. | Must |
| FR-022 | Users must be able to associate a schedule with a habit when relevant. | Should |
| FR-023 | Users must be able to create standalone schedules without associating them with a habit. | Must |
| FR-024 | The system must validate schedule dates and time information. | Must |

Schedules represent planned activities and commitments, not a general-purpose task management system.

### 4.1.4 Focus Sessions and Pomodoro

| ID | Requirement | Priority |
|---|---|---|
| FR-025 | Users must be able to start a focus session. | Must |
| FR-026 | Users must be able to pause and resume an active focus session. | Must |
| FR-027 | Users must be able to stop an active focus session. | Must |
| FR-028 | The system must record focus session history. | Must |
| FR-029 | The system must record actual focus session duration. | Must |
| FR-030 | Users must be able to associate a focus session with a relevant habit or schedule. | Should |
| FR-031 | The system must support Pomodoro-based focus and break cycles. | Must |
| FR-032 | The system must distinguish completed sessions from interrupted sessions. | Must |
| FR-033 | The backend must remain authoritative for focus session state and validation. | Must |

Focus sessions support habit execution but are not equivalent to habit completion.

### 4.1.5 Habit Completion

| ID | Requirement | Priority |
|---|---|---|
| FR-034 | Users must be able to record habit completion. | Must |
| FR-035 | The system must validate whether completion criteria have been satisfied. | Must |
| FR-036 | The system must prevent duplicate completion records for the same habit occurrence where applicable. | Must |
| FR-037 | Users must be able to view their historical completion records. | Must |

### 4.1.6 Reminder and Notification

| ID | Requirement | Priority |
|---|---|---|
| FR-038 | Users must be able to receive reminders related to their habits and schedules. | Must |
| FR-039 | Users must be able to configure reminder preferences. | Should |
| FR-040 | The system must process scheduled reminders reliably. | Must |
| FR-041 | The system must avoid sending duplicate reminders for the same scheduled notification event. | Must |

The exact delivery mechanism for reminders will be finalized during implementation planning.

### 4.1.7 Progress Tracking

| ID | Requirement | Priority |
|---|---|---|
| FR-042 | Users must be able to view their habit completion history. | Must |
| FR-043 | Users must be able to view habit consistency over time. | Must |
| FR-044 | Users must be able to view focus session statistics. | Must |
| FR-045 | Users must be able to view relevant progress summaries. | Must |
| FR-046 | The system must provide a historical view of habit activity. | Must |

### 4.1.8 Overview and Dashboard

| ID | Requirement | Priority |
|---|---|---|
| FR-047 | Users must be able to view an overview of their active habits. | Must |
| FR-048 | Users must be able to view relevant daily schedules. | Must |
| FR-049 | Users must be able to view relevant focus and progress information. | Must |
| FR-050 | The dashboard must prioritize actionable information rather than overwhelming users with excessive data. | Must |

---

## 4.2 Non-Functional Requirements

### 4.2.1 Performance

- API responses should remain responsive under normal MVP usage.
- Database queries should be optimized for common user operations.
- Frequently accessed information may use caching when appropriate.
- Long-running or deferred operations should be handled through background jobs where necessary.

### 4.2.2 Security

- All private user data must require appropriate authorization.
- Passwords must be securely hashed.
- Authentication tokens must be securely generated, stored, and managed.
- Sensitive credentials must not be committed to source control.
- API endpoints must validate input and enforce ownership rules.
- Communication between clients and the backend must use HTTPS in production.

### 4.2.3 Reliability

- Important business operations must preserve data consistency.
- Database operations requiring atomicity must use transactions.
- Background jobs must support retry and idempotent processing.
- The application must handle network interruptions and failed requests gracefully.

### 4.2.4 Maintainability

- The backend must follow Modular Monolith architecture.
- Mobile and frontend applications must use feature-oriented organization.
- Business rules must remain consistent across clients.
- Code should be testable and documented.
- Dependencies should be kept manageable and actively maintained.

### 4.2.5 Usability

- The application should be simple and approachable.
- Users should be encouraged to prioritize one or two habits.
- Core actions should require minimal interaction.
- Progress information should be easy to understand.
- The interface should avoid unnecessary cognitive overload.

### 4.2.6 Compatibility

- Mobile applications must support the intended Flutter target platforms.
- The web application must support modern browsers.
- Both clients must consume the same backend API contract.

### 4.2.7 Data Integrity

- Historical habit and focus records must remain consistent.
- Important user operations must be validated.
- Duplicate business records must be prevented where appropriate.
- Archiving a habit must not unintentionally delete its historical information.

---

# 5. Non-Requirements

The following capabilities are explicitly excluded from the initial MVP.

| ID | Non-Requirement | Reason |
|---|---|---|
| NR-001 | AI companion or AI productivity assistant. | Not required to validate the core habit-first product. |
| NR-002 | Social networking and community features. | Outside the initial personal productivity scope. |
| NR-003 | Public habit sharing and social leaderboards. | Not part of the core product experience. |
| NR-004 | General-purpose task management. | Daymark focuses on habits and schedules rather than task lists. |
| NR-005 | Team collaboration and shared workspaces. | MVP targets individual users. |
| NR-006 | Complex project management. | Not aligned with the product's primary purpose. |
| NR-007 | Advanced gamification and reward systems. | Can be evaluated after product validation. |
| NR-008 | Third-party calendar synchronization. | Not necessary for the initial scheduling experience. |
| NR-009 | Advanced analytics and predictive insights. | Initial progress tracking should remain straightforward. |
| NR-010 | Wearable and health-platform integrations. | Not required for the initial product. |

These exclusions do not necessarily represent permanent product limitations. They may be considered in future product phases.

---

# 6. User Stories

User stories describe the expected user experience and the value delivered by each capability.

## 6.1 Authentication

**US-001 — Account Registration**

As a new user, I want to create an account so that I can use Daymark to manage my habits and schedules.

**US-002 — Login**

As a registered user, I want to log in securely so that I can access my personal data.

**US-003 — Logout**

As an authenticated user, I want to log out so that my account is protected when I stop using the application.

## 6.2 Habit Management

**US-004 — Create a Habit**

As a user, I want to create a habit with a name and category so that I can identify the behavior I want to build.

**US-005 — Limit Active Habits**

As a user, I want to focus on only one or two active habits so that I can avoid overwhelming myself with too many commitments.

**US-006 — Define Habit Frequency**

As a user, I want to define when a habit should be performed so that I can establish a consistent routine.

**US-007 — View Active Habits**

As a user, I want to see my active habits so that I can understand what I should focus on.

**US-008 — Archive a Habit**

As a user, I want to archive a habit that I no longer pursue so that my active habit list remains relevant while preserving its history.

**US-009 — Review Habit History**

As a user, I want to review previous habit completions so that I can understand my consistency over time.

## 6.3 Schedule Management

**US-010 — Create a Schedule**

As a user, I want to create a schedule so that I can organize my daily activities.

**US-011 — Associate Schedule with Habit**

As a user, I want to associate a schedule with a habit so that I can organize when I intend to perform that habit.

**US-012 — Create Standalone Schedule**

As a user, I want to create a schedule without associating it with a habit so that I can manage other personal commitments.

**US-013 — Review Daily Schedule**

As a user, I want to view my daily schedule so that I understand what activities are planned.

## 6.4 Focus Sessions

**US-014 — Start a Focus Session**

As a user, I want to start a Pomodoro session so that I can focus on executing an activity.

**US-015 — Pause and Resume**

As a user, I want to pause and resume my focus session so that I can handle interruptions without losing my session context.

**US-016 — Complete a Focus Session**

As a user, I want to complete a focus session so that my focused work is recorded.

**US-017 — Review Focus History**

As a user, I want to review previous focus sessions so that I can understand how much time I spend on focused activities.

## 6.5 Habit Completion

**US-018 — Mark Habit Completion**

As a user, I want to mark a habit as completed when its completion criteria are satisfied so that my progress is recorded.

**US-019 — Understand Habit Progress**

As a user, I want to see whether I have completed my planned habits so that I can maintain consistency.

## 6.6 Reminder

**US-020 — Receive Habit Reminder**

As a user, I want to receive reminders for my habits so that I do not easily forget my intended activities.

**US-021 — Configure Reminder**

As a user, I want to configure my reminders so that notifications fit my daily routine.

## 6.7 Progress and Dashboard

**US-022 — View Progress**

As a user, I want to see my habit completion history so that I can understand my progress.

**US-023 — View Consistency**

As a user, I want to see how consistently I perform my habits so that I can identify patterns in my behavior.

**US-024 — View Focus Statistics**

As a user, I want to see my focus session statistics so that I can evaluate my focused execution.

**US-025 — View Dashboard Overview**

As a user, I want to see my habits, schedules, and relevant progress information in one place so that I can quickly understand my current activities.

---

# 7. Product Scope

## 7.1 MVP Scope

The MVP focuses on validating the core Daymark experience.

| Feature | MVP Scope | Description |
|---|---|---|
| Authentication | Included | Registration, login, logout, protected access. |
| Habit Management | Included | Create, update, archive, categorize, and track habits. |
| Habit Limitation | Included | Maximum of two active habits. |
| Habit Frequency | Included | Define recurring habit schedules. |
| Schedule Management | Included | Create, view, update, and delete schedules. |
| Focus Sessions | Included | Pomodoro-based focus execution. |
| Habit Completion | Included | Record and validate habit completion. |
| Reminder | Included | Basic habit and schedule reminders. |
| Progress Tracking | Included | Completion history and consistency. |
| Dashboard | Included | Overview of habits, schedules, and progress. |
| Mobile Application | Included | Core Daymark experience on Flutter. |
| Web Application | Included | Core Daymark experience on React. |

## 7.2 Future Scope

The following features may be evaluated after MVP validation.

**Phase 2 — Product Enhancement**

- Advanced habit analytics.
- More flexible habit configuration.
- Calendar integrations.
- Advanced reminder customization.
- Improved productivity insights.
- Additional personalization.

**Future Product Direction**

- AI Companion.
- Personalized habit recommendations.
- AI-powered productivity insights.
- Intelligent routine suggestions.
- Predictive behavioral insights.

Future features must be evaluated based on actual user needs and product validation.

## 7.3 Platform Scope

Daymark will have three independently maintained repositories:

1. Backend repository.
2. Mobile repository.
3. Frontend repository.

Both mobile and web applications consume the same backend API.

The backend owns business rules, authentication, data persistence, and authoritative application state.

---

# 8. Do's and Don'ts

These guidelines establish product and development boundaries.

## 8.1 Do's

**Product and UX**

- Prioritize habit development over generic task management.
- Encourage users to start with one or two active habits.
- Keep core user interactions simple.
- Make progress understandable and meaningful.
- Ensure focus sessions support real activity execution.
- Preserve historical records when habits are archived.
- Design a consistent experience between web and mobile.

**Engineering**

- Follow the approved technology stack.
- Maintain clear separation between business logic and infrastructure.
- Use OpenAPI as the shared API contract.
- Keep frontend and mobile behavior consistent with backend business rules.
- Validate important operations on the backend.
- Write tests based on requirements and expected behavior.
- Protect user data and authentication credentials.
- Keep the three repositories independently maintainable.

## 8.2 Don'ts

**Product and UX**

- Do not encourage users to create too many active habits.
- Do not turn Daymark into a general-purpose task management application.
- Do not overload the dashboard with unnecessary information.
- Do not equate completing a Pomodoro session with automatically completing a habit.
- Do not introduce AI functionality into the MVP.
- Do not introduce social features without validating their product value.

**Engineering**

- Do not implement business rules exclusively in the frontend or mobile application.
- Do not duplicate business logic independently across clients.
- Do not introduce microservices prematurely.
- Do not create unnecessary abstractions or dependencies.
- Do not store sensitive credentials in source control.
- Do not bypass backend authorization checks.
- Do not change valid tests simply to accommodate incorrect implementation behavior.
- Do not treat background job execution as guaranteed exactly-once processing.

---

# 9. Assumptions and Dependencies

## 9.1 Product Assumptions

1. Daymark primarily targets individual users rather than teams.
2. Users benefit from starting with a limited number of active habits.
3. One or two active habits are sufficient to establish the initial habit-building experience.
4. Users need both habit management and schedule management.
5. Pomodoro sessions can help users execute planned activities.
6. Progress visibility can encourage users to maintain consistency.
7. Web and mobile applications should share the same business rules.
8. Users have access to an internet connection for synchronization with the backend.

## 9.2 Technical Assumptions

1. PostgreSQL is the primary source of truth.
2. Redis is used for caching and supporting infrastructure.
3. River handles persistent background jobs.
4. The backend exposes a REST API.
5. OpenAPI defines the API contract.
6. Swagger UI provides interactive API documentation.
7. The backend is maintained as a Modular Monolith.
8. Mobile and web applications communicate with the backend through the same API.
9. The three repositories are maintained independently but follow shared product and API contracts.

## 9.3 Dependencies

| Dependency | Purpose | Impact if Unavailable |
|---|---|---|
| PostgreSQL | Primary application data storage. | Core application operations cannot function. |
| Redis | Caching and supporting infrastructure. | Cache-dependent functionality may degrade or require fallback behavior. |
| River | Persistent background job processing. | Deferred operations and scheduled backend jobs cannot run normally. |
| Backend API | Shared business logic and data access. | Mobile and web applications cannot operate normally. |
| OpenAPI Specification | Shared API contract. | Client and backend integration becomes harder to maintain. |
| GitHub | Source control and collaboration. | Development and CI/CD workflows are affected. |
| GitHub Actions | Automated validation and CI/CD. | Automated quality checks and deployment workflows are affected. |
| Push Notification Provider | Server-triggered mobile notifications, if selected. | Remote push reminders cannot be delivered. |

## 9.4 Open Product Decisions

The following decisions must be finalized before the relevant features enter detailed implementation.

| ID | Decision | Current Direction | Status |
|---|---|---|---|
| OD-001 | Habit completion criteria | Based on configurable habit targets or criteria. | To be finalized |
| OD-002 | Pomodoro duration | Standard Pomodoro defaults are under consideration. | To be finalized |
| OD-003 | Break duration | Short and long break configuration. | To be finalized |
| OD-004 | Habit frequency | Daily, selected weekdays, and/or weekly targets. | To be finalized |
| OD-005 | Schedule and habit relationship | A schedule can be standalone or associated with a habit. | Direction established |
| OD-006 | Interrupted focus session | Preserve incomplete session history and actual duration. | Direction established |
| OD-007 | Habit lifecycle | Archive old habits and preserve their historical records. | Direction established |
| OD-008 | Reminder delivery | Local notifications and/or server-triggered push notifications. | To be finalized |
| OD-009 | Notification provider | FCM/APNs or another suitable delivery mechanism. | To be finalized |
| OD-010 | Active habit limit | Maximum of two active habits. | Approved |

These open decisions must not be silently converted into implementation assumptions. Their business rules should be documented before implementation of the affected features.

---

# 10. Product Acceptance Principles

The MVP can be considered ready for initial product validation when:

1. Users can register and securely access their accounts.
2. Users can create and manage up to two active habits.
3. Users can organize activities through schedules.
4. Users can execute focus sessions using Pomodoro.
5. Users can record and review habit completions.
6. Users can receive relevant reminders through the selected notification mechanism.
7. Users can understand their habit consistency and progress.
8. Users can access the core experience through web and mobile.
9. User data remains isolated and protected.
10. Core business rules behave consistently across all clients.

---

# 11. Final Product Direction

Daymark is a habit-first personal productivity application.

The MVP is intentionally focused on helping users:

- Choose a small number of meaningful habits.
- Organize their activities.
- Execute activities through focused sessions.
- Track habit completion.
- Understand their consistency and progress.

The product should not attempt to solve every productivity problem at once.

Its initial value comes from helping users move from intention to consistent action.

**Build fewer features, make the core experience reliable, and validate whether Daymark genuinely helps users build sustainable habits.**