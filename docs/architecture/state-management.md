# State Management

## Purpose

State Management defines how application state is categorized, owned, updated, synchronized, and consumed within Exactly.

The purpose of this document is to establish a clear state management foundation before implementation begins.

This document focuses on:

- What state exists.
- Where state should live.
- Who owns state.
- Who may modify state.
- How state changes move through the application.
- How state interacts with components, data, intelligence, and APIs.
- How unnecessary duplicated state should be avoided.

This document defines architectural principles rather than selecting a specific state management library.

---

## Document Guide

| Section                                      | Purpose                                                   |
| -------------------------------------------- | --------------------------------------------------------- |
| Purpose                                      | Why State Management is needed                            |
| Relationship to Other Architecture Documents | Clarifies how State Management fits into the architecture |
| State Definition                             | Defines what state means within Exactly                   |
| State Management Principles                  | Principles governing state                                |
| State Categories                             | Defines major types of state                              |
| State Ownership                              | Defines where state should live                           |
| Local UI State                               | Defines component-level state                             |
| Shared Feature State                         | Defines state shared within a feature                     |
| Application State                            | Defines broader application state                         |
| Server and Data State                        | Defines externally sourced state                          |
| Persistent State                             | Defines state that survives sessions                      |
| Temporary State                              | Defines short-lived state                                 |
| State Lifecycle                              | Defines how state is created, changed, and removed        |
| State Mutation                               | Defines how state may change                              |
| State Synchronization                        | Defines how state remains consistent                      |
| Loading and Error State                      | Defines operational state                                 |
| Intelligence State                           | Defines AI-related state                                  |
| State and Components                         | Defines the relationship with components                  |
| State and Data Flow                          | Defines the relationship with information flow            |
| State and API Architecture                   | Defines the relationship with APIs                        |
| State Boundaries                             | Defines what belongs outside state management             |
| Avoiding Duplicate State                     | Defines how unnecessary duplication is prevented          |
| Future Expansion                             | Defines how state management may evolve                   |
| Architecture Boundaries                      | Defines what this document does not decide                |
| State Management Statement                   | Defines the long-term principle                           |

---

# Relationship to Other Architecture Documents

State Management is closely related to several other architecture documents.

| Document                     | Primary Question                                  |
| ---------------------------- | ------------------------------------------------- |
| `architecture-principles.md` | What principles govern the architecture?          |
| `folder-structure.md`        | Where do responsibilities live?                   |
| `component-strategy.md`      | How are responsibilities divided into components? |
| `data-flow.md`               | How does information move through the system?     |
| `state-management.md`        | Where does state live and how does it change?     |
| `api-architecture.md`        | How do system boundaries communicate?             |

The distinction is important.

### Data Flow

Data Flow answers:

> How does information move?

### State Management

State Management answers:

> Where does information that must be remembered by the application live, and how does it change over time?

State is therefore related to data flow but is not the same thing as data flow.

---

# State Definition

State is information whose current value affects the behavior, presentation, or operation of the application.

Examples include:

- Whether a dialog is open.
- What the user has entered.
- Which item is selected.
- Whether data is loading.
- Whether an operation failed.
- What information has already been retrieved.
- What context is currently active.
- What result is currently being displayed.

State changes over time.

Conceptually:

    State(t0)
        ↓
    User Action / System Event
        ↓
    State Transition
        ↓
    State(t1)

State management is therefore concerned with controlling these transitions in a predictable way.

---

# State Management Principles

Exactly follows these principles.

## 1. State Should Have One Clear Owner

Every important piece of state should have a clearly identifiable owner.

---

## 2. State Should Live at the Lowest Appropriate Level

State should not automatically be placed at global application level.

If state is only relevant to one component, it should remain local.

If state is shared by several components, it should move to the smallest common scope that can coordinate those components.

---

## 3. Avoid Unnecessary Global State

Global state should only be introduced when multiple independent parts of the application genuinely require access to the same state.

---

## 4. Avoid Duplicate Sources of Truth

The same information should not be independently maintained in multiple places unless there is a clear architectural reason.

---

## 5. Derived State Should Usually Be Derived

If information can reliably be calculated from existing state, it should generally not be stored as an additional independent state value.

Conceptually:

    Source State
        ↓
    Derived Value

rather than:

    Source State
        ↓
    Separate Stored Value

This reduces synchronization problems.

---

## 6. State Changes Should Be Predictable

Important state transitions should occur through understandable actions or operations.

Hidden mutation should be avoided.

---

## 7. External Data Should Not Automatically Become Application State

Data retrieved from an API, database, or external service should be treated according to its lifecycle and ownership.

Not every response needs to become persistent application state.

---

## 8. State Should Reflect Real Product Needs

State architecture should be driven by actual application requirements rather than by the capabilities of a chosen state management library.

---

## 9. State Should Be Easy to Understand

Developers should be able to determine:

- Where state lives.
- Who owns it.
- What can change it.
- Who consumes it.
- How long it exists.

---

## 10. Complexity Should Be Added Only When Necessary

Simple state should remain simple.

A sophisticated state architecture should only be introduced when product complexity justifies it.

---

# State Categories

Exactly recognizes several conceptual categories of state.

    State
    │
    ├── Local UI State
    │
    ├── Shared Feature State
    │
    ├── Application State
    │
    ├── Server / Data State
    │
    ├── Persistent State
    │
    ├── Temporary State
    │
    └── Intelligence / Processing State

These categories describe responsibility and lifecycle.

They do not necessarily require separate technologies.

---

# Local UI State

Local UI state belongs to a specific component or small presentation area.

Examples:

- Modal open/closed.
- Dropdown open/closed.
- Input focus.
- Temporary form values.
- Selected tab.
- Local animation state.
- Temporary visual preferences.

Conceptually:

    Component
        ↓
    Local UI State
        ↓
    Component

Local state should remain local when no other part of the application needs to know about it.

---

# Shared Feature State

Shared Feature State is state required by multiple components participating in the same feature.

Examples may include:

- Current feature selection.
- Multi-step workflow progress.
- Temporary feature context.
- Shared filters.
- Feature-specific interaction state.

Conceptually:

    Feature
       ↓
    Shared Feature State
       ↙       ↘
    Component  Component

The state should remain within the feature boundary where practical.

---

# Application State

Application state represents information required across broader areas of the application.

Examples may include:

- Current authenticated user.
- Application-level preferences.
- Current application context.
- Global notifications.
- Session-level configuration.

Application state should be introduced carefully.

Not every frequently used value is automatically application-wide state.

---

# Server and Data State

Server and Data State represents information whose primary source exists outside the presentation layer.

Examples include:

- Data retrieved from an API.
- Stored user information.
- Product information.
- Retrieved evidence.
- Historical records.
- Intelligence results retrieved from a service.

Conceptually:

    External Source
         ↓
    Data / API Boundary
         ↓
    Application
         ↓
    Presentation

Server and data state should be treated according to:

- Freshness.
- Ownership.
- Synchronization.
- Loading status.
- Error status.
- Cache requirements.

The exact technical mechanism is not defined here.

---

# Persistent State

Persistent state survives beyond the immediate lifecycle of a component or interaction.

Examples may include:

- User preferences.
- Account settings.
- Saved user information.
- Saved product data.
- Persistent application configuration.

Conceptually:

    Application
        ↓
    Persistence Boundary
        ↓
    Stored State
        ↓
    Future Session
        ↓
    Application

Persistent state should only be stored when there is a meaningful product requirement.

---

# Temporary State

Temporary state exists only for a limited interaction or workflow.

Examples include:

- Form input before submission.
- Temporary selections.
- Current processing context.
- Unsaved changes.
- Intermediate workflow state.

Temporary state should not automatically be persisted.

---

# Intelligence and Processing State

Exactly may require state associated with intelligence operations.

Examples include:

- Processing status.
- Current reasoning operation.
- Retrieved evidence.
- Intermediate results.
- Evaluation status.
- Completion state.
- Intelligence errors.

Conceptually:

    User Request
        ↓
    Intelligence Operation
        ↓
    Processing State
        ↓
    Result / Error
        ↓
    User-Facing State

Intelligence processing state should be handled carefully because it may represent incomplete or intermediate information.

---

# State Ownership

Every meaningful state value should have an identifiable owner.

A simplified ownership model is:

| State Type                    | Typical Owner                       |
| ----------------------------- | ----------------------------------- |
| UI-only state                 | Component                           |
| Shared feature state          | Feature / Application boundary      |
| Application state             | Application layer                   |
| Server data                   | Data / Application boundary         |
| Persistent state              | Data / Persistence boundary         |
| Intelligence processing state | Intelligence / Application boundary |
| User decision                 | User                                |

Ownership determines:

- Who may update the state.
- Where validation occurs.
- How state is persisted.
- How state is synchronized.
- Which components may consume it.

---

# State Ownership Rule

The owner of state should be the smallest architectural scope capable of coordinating all required consumers.

For example:

    One Component
        ↓
    Local State

If multiple components need it:

    Shared Feature
        ↓
    Feature State

If unrelated application areas need it:

    Application
        ↓
    Application State

This prevents unnecessary global state.

---

# State Mutation

State should change through intentional operations.

Conceptually:

    Current State
         ↓
    Action / Event
         ↓
    State Transition
         ↓
    New State

The exact implementation may vary.

The architectural principle remains:

> State should change for an identifiable reason.

---

# State Mutation Boundaries

A component should not directly modify state it does not own.

Conceptually:

    Component
        ↓
    Defined Action
        ↓
    State Owner
        ↓
    State Update

This keeps ownership clear.

---

# Immutable State Principles

Where practical, state transitions should avoid uncontrolled mutation of shared state.

Changes should produce predictable results.

This supports:

- Debugging.
- Testing.
- Change tracking.
- Predictable rendering.
- Easier reasoning about state transitions.

The exact implementation mechanism is not defined here.

---

# State Lifecycle

State should have an understandable lifecycle.

Conceptually:

    Create
      ↓
    Initialize
      ↓
    Read
      ↓
    Update
      ↓
    Synchronize
      ↓
    Clear / Replace
      ↓
    Destroy

Not every state type follows every stage.

The lifecycle depends on the nature of the state.

---

# Initialization

State should be initialized from an appropriate source.

Possible sources include:

- Default values.
- User input.
- Existing application context.
- Persistent storage.
- API responses.
- Retrieved data.
- Intelligence operations.

Initialization should not create unnecessary duplicated state.

---

# State Synchronization

When the same conceptual information exists across boundaries, synchronization must be intentional.

Examples include:

    Server Data
        ↕
    Application State
        ↕
    Presentation

Synchronization may be required when:

- Server data changes.
- User changes local data.
- Another operation modifies shared information.
- Persistent data is restored.
- An intelligence operation completes.

---

# Source of Truth

Each important piece of information should have a primary source of truth.

For example:

    Persistent User Preference
            ↓
        Data Source
            ↓
      Application State
            ↓
       Presentation

The presentation layer should not independently become a competing source of truth.

---

# Derived State

Derived state is information that can be calculated from existing state.

Example:

    Selected Items
          ↓
    Calculate Total
          ↓
    Display Total

The calculated total does not necessarily need to exist as independently stored state.

This reduces synchronization risk.

---

# Loading State

Loading state represents an operation that has begun but has not yet produced a final result.

Conceptually:

    Idle
      ↓
    Loading
      ↓
    Success
      or
    Error

The user interface should communicate loading states clearly.

---

# Processing State

Some Exactly operations may require more than a simple loading state.

For example:

    Idle
      ↓
    Preparing
      ↓
    Retrieving Information
      ↓
    Processing
      ↓
    Reasoning
      ↓
    Generating Explanation
      ↓
    Complete

Where meaningful, processing state may communicate progress or system activity to the user.

The system should not expose unnecessary internal implementation details merely for the sake of showing progress.

---

# Error State

Errors should be represented as meaningful state where the user or application needs to respond to them.

Conceptually:

    Operation
       ↓
    Failure
       ↓
    Error State
       ↓
    User Feedback
       ↓
    Recovery / Retry / Exit

Errors should not silently disappear.

---

# Empty State

An empty state is different from an error.

Examples:

- No saved information.
- No search results.
- No evidence available.
- No previous activity.

The application should distinguish:

    No Data
       ≠
    Loading
       ≠
    Error

This distinction is important for user understanding.

---

# Success State

A successful operation should produce a meaningful state representing completion.

Conceptually:

    Processing
       ↓
    Success
       ↓
    Result Available
       ↓
    Presentation

Success state should not imply that the result is necessarily correct.

It means the operation completed successfully.

---

# Intelligence State

Intelligence operations may require specialized state.

A conceptual model is:

    Idle
      ↓
    Requested
      ↓
    Gathering Context
      ↓
    Retrieving Evidence
      ↓
    Processing
      ↓
    Reasoning
      ↓
    Explanation
      ↓
    Complete

Possible alternative:

    Any Processing State
          ↓
        Error
          ↓
      Recovery

The exact state machine for intelligence features should be defined when specific workflows are implemented.

---

# State and Components

Components consume state according to their responsibility.

Conceptually:

    State Owner
        ↓
    Defined State
        ↓
    Component
        ↓
    Presentation

Components should not automatically own every piece of state they display.

A component may:

- Read state.
- Trigger an action.
- Display state.
- Maintain local state.

But it should not become responsible for state outside its scope.

---

# State and Component Strategy

The Component Strategy defines how responsibilities are divided into components.

State Management defines how those components interact with state.

The relationship is:

    Component Strategy
          ↓
    Defines component responsibility
          ↓
    State Management
          ↓
    Defines state ownership
          ↓
    Component
          ↓
    Reads / triggers state changes

This separation prevents state architecture from becoming an accidental consequence of component structure.

---

# State and Data Flow

Data Flow answers:

> How does information move?

State Management answers:

> What information must be remembered, where is it held, and how does it change?

Conceptually:

    Data Flow
        ↓
    Information arrives
        ↓
    State Management
        ↓
    Information is retained / transformed / synchronized
        ↓
    Presentation

Not every piece of data moving through the system needs to become long-lived state.

---

# State and API Architecture

API responses may become application state when the product requires the information to remain available for later interactions.

Conceptually:

    API Request
        ↓
    API Response
        ↓
    Application
        ↓
    State Decision
       ↙       ↘
    Use Once   Store / Cache
                 ↓
                State

The API architecture will define the communication mechanism.

State Management defines whether and how the resulting information is retained.

---

# State and Persistence

Persistent state should have an explicit reason to exist.

Before persisting state, the system should consider:

- Is persistence required?
- What is the retention period?
- Is the information sensitive?
- Who owns it?
- When should it be synchronized?
- When can it be removed?

Persistence decisions should not be made solely because storage is available.

---

# State and User Decisions

Exactly should distinguish between:

    System State
        ↓
    System Result
        ↓
    User Understanding
        ↓
    User Judgment
        ↓
    User Decision

The user's final decision is not application state that Exactly should attempt to control.

The system may remember user-provided decisions when there is a legitimate product requirement, but the decision itself remains a user-owned concept.

---

# Duplicate State

Duplicated state occurs when the same conceptual information is independently stored in multiple locations.

Example:

    API Data
       ↓
    Store A
       ↓
    Component State
       ↓
    Store B

This can create synchronization problems.

The preferred approach is:

    Single Source of Truth
            ↓
       Derived Views
            ↓
       Components

---

# When Duplication May Be Justified

Duplication may be appropriate when:

- A deliberate cache exists.
- A temporary editing copy is required.
- A transformation is intentionally materialized.
- Performance requirements justify it.
- Historical state must be preserved.
- Different lifecycles require separate representations.

When duplication exists, the relationship between the copies should be explicit.

---

# State Reset

State should be reset when its lifecycle ends.

Examples include:

- Leaving a feature.
- Completing a workflow.
- Logging out.
- Changing user context.
- Starting a new analysis.
- Discarding temporary information.

Reset behavior should be intentional.

---

# State Security

State may contain sensitive information.

State architecture should therefore consider:

- Whether information should exist in client state.
- Whether it should be persisted.
- Whether it should be transmitted.
- Whether it should be logged.
- Whether it should be exposed to components that do not require it.

Sensitive state should follow the appropriate security and privacy requirements.

---

# State Observability

State transitions should be observable enough to support:

- Debugging.
- Testing.
- Error analysis.
- Performance analysis.
- Product evaluation.

Observability should avoid exposing sensitive information unnecessarily.

---

# State Testing

State behavior should be testable according to its responsibility.

Testing may include:

- Initial state.
- State transitions.
- User actions.
- Loading states.
- Error states.
- Reset behavior.
- Synchronization.
- Persistence behavior.
- Derived state.

The specific testing framework is outside the scope of this document.

---

# State Complexity

State complexity should reflect actual product complexity.

The project should avoid:

- Global state for local problems.
- Multiple stores for the same responsibility.
- Deeply nested state without clear ownership.
- Unnecessary synchronization.
- Storing values that can be derived.
- Persisting information without a product requirement.

Simple state should remain simple.

---

# State Scalability

As Exactly grows, state architecture may require additional structures.

Possible future needs include:

- Feature-level stores.
- Server-state caching.
- Event-driven state transitions.
- Offline state.
- Background processing state.
- Collaborative state.
- Real-time synchronization.
- Advanced workflow state.

These should be introduced only when actual product requirements justify them.

---

# Future Expansion

Future state categories may emerge as the product evolves.

Potential examples include:

- Collaboration State.
- Notification State.
- Analytics State.
- Offline State.
- Synchronization State.
- Evaluation State.
- Administrative State.

These categories should not be formalized until their responsibilities are understood.

---

# Architecture Boundaries

This document does not define:

- A specific state management library.
- A specific frontend framework.
- Exact store implementation.
- Exact state object schemas.
- Exact API response structures.
- Database schemas.
- Caching technology.
- Persistence technology.
- Authentication implementation.
- Authorization implementation.
- AI model implementation.
- Infrastructure architecture.

These decisions belong to later implementation documentation or Architecture Decision Records.

---

# Decision Criteria for Future State Technology

When a specific state management technology becomes necessary, the decision should be evaluated against:

- Simplicity.
- Maintainability.
- Predictability.
- Performance.
- Type safety.
- Testability.
- Developer experience.
- Server-state requirements.
- Application complexity.
- Integration with the existing architecture.

The technology should serve the state architecture rather than determine it.

---

# Relationship to UX Foundation

State Management should support the UX foundation established during Phase 2 and Sprint M4.

State should enable:

- Clear user flows.
- Predictable interactions.
- Meaningful feedback.
- Appropriate loading states.
- Clear error states.
- Consistent navigation behavior.
- Preservation of relevant user context.

Technical state management should remain invisible to the user wherever possible.

The user should experience a coherent product rather than the complexity of the underlying state architecture.

---

# State Management Evolution

The state architecture may evolve as Exactly becomes more concrete.

Changes should be evaluated against:

- State ownership.
- Source of truth.
- Lifecycle.
- Synchronization requirements.
- Product complexity.
- Performance.
- Security.
- Maintainability.

Major state architecture changes should be documented when they materially affect system behavior or architectural boundaries.

---

# State Management Checklist

Before introducing new state, ask:

1. Does this information actually need to be state?
2. Who owns it?
3. Where should it live?
4. Is it local or shared?
5. Is it temporary or persistent?
6. Is it server-owned or application-owned?
7. Can it be derived instead?
8. Does another state already represent the same information?
9. Who is allowed to change it?
10. What happens when its lifecycle ends?

If these questions cannot be answered clearly, the state design should be reconsidered before implementation.

---

# State Management Statement

Exactly should manage state according to clear ownership, appropriate lifecycle, and a single understandable source of truth.

State should exist only when it provides meaningful value, remain at the smallest appropriate scope, and change through intentional transitions.

The architecture should avoid unnecessary global state, duplicated state, hidden mutation, and premature complexity.

> **Keep state where it belongs, change it deliberately, and make its ownership clear.**
