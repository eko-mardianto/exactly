# State Management

## Purpose

State Management defines how application state is categorized, owned, updated, synchronized, and consumed within Exactly.

The purpose of this document is to establish a clear state management foundation before implementation begins.

This document focuses on:

- What state exists.
- When information should become application state.
- Where state should live.
- Who owns state.
- Who may modify state.
- How state changes move through the application.
- How state interacts with components, data, intelligence, and APIs.
- How state transitions differ from side effects.
- How unnecessary duplicated state should be avoided.

This document defines architectural principles rather than selecting a specific state management library.

---

## Document Guide

| Section                                       | Purpose                                                                     |
| --------------------------------------------- | --------------------------------------------------------------------------- |
| Purpose                                       | Why State Management is needed                                              |
| Relationship to Other Architecture Documents  | Clarifies how State Management fits into the architecture                   |
| State Definition                              | Defines what state means within Exactly                                     |
| State Management Principles                   | Principles governing state                                                  |
| State Categories                              | Defines major types of state                                                |
| When Information Becomes State                | Defines when information should be retained as state                        |
| State Ownership                               | Defines where state should live                                             |
| Local UI State                                | Defines component-level state                                               |
| Shared Feature State                          | Defines state shared within a feature                                       |
| Application State                             | Defines broader application state                                           |
| Server and Data State                         | Defines externally sourced state and its distinction from application state |
| Persistent State                              | Defines state that survives sessions                                        |
| Temporary State                               | Defines short-lived state                                                   |
| State Lifecycle                               | Defines how state is created, changed, synchronized, and removed            |
| State Mutation                                | Defines how state may change                                                |
| State Transition and Side Effects             | Defines the difference between state changes and external effects           |
| State Synchronization                         | Defines how state remains consistent                                        |
| Intelligence State                            | Defines AI-related processing state                                         |
| State and Components                          | Defines the relationship with components                                    |
| State and Data Flow                           | Defines the relationship with information flow                              |
| State and API Architecture                    | Defines the relationship with APIs                                          |
| State Boundaries                              | Defines what belongs outside state management                               |
| Avoiding Duplicate State                      | Defines how unnecessary duplication is prevented                            |
| Future Expansion                              | Defines how state management may evolve                                     |
| Architecture Boundaries                       | Defines what this document does not decide                                  |
| Decision Criteria for Future State Technology | Defines how future technology decisions should be evaluated                 |
| Relationship to UX Foundation                 | Defines how state management supports the user experience                   |
| State Management Evolution                    | Defines how the architecture may evolve                                     |
| State Management Checklist                    | Provides a decision checklist                                               |
| State Management Statement                    | Defines the long-term principle                                             |

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

> What information must be remembered by the application, where does it live, and how does it change over time?

State is therefore related to data flow but is not the same thing as data flow.

---

# State Definition

State is information whose current value affects the behavior, presentation, or operation of the application and therefore needs to be retained beyond the immediate operation that produced or consumed it.

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

```text
State(t0)
    ↓
User Action / System Event
    ↓
State Transition
    ↓
State(t1)
```

State Management is therefore concerned with determining:

- What information needs to be remembered.
- Where it should be retained.
- Who owns it.
- Who may change it.
- How long it should exist.
- How it should remain consistent.

Not every piece of information that passes through the system needs to become state.

---

# State Management Principles

Exactly follows these principles.

## 1. State Should Have One Clear Owner

Every important piece of state should have a clearly identifiable owner.

The owner is responsible for defining how that state is initialized, changed, synchronized, and removed.

---

## 2. State Should Live at the Lowest Appropriate Level

State should not automatically be placed at global application level.

If state is only relevant to one component, it should remain local.

If state is shared by several components, it should move to the smallest common scope that can coordinate those components.

---

## 3. Avoid Unnecessary Global State

Global state should only be introduced when multiple independent parts of the application genuinely require access to the same state.

Frequent access does not automatically justify global ownership.

---

## 4. Avoid Duplicate Sources of Truth

The same conceptual information should not be independently maintained in multiple places unless there is a clear architectural reason.

Where multiple representations are required, their relationship should be explicit.

---

## 5. Derived State Should Usually Be Derived

If information can reliably be calculated from existing state, it should generally not be stored as an additional independent state value.

Conceptually:

```text
Source State
    ↓
Derived Value
```

rather than:

```text
Source State
    ↓
Separate Stored Value
```

This reduces synchronization problems.

---

## 6. State Changes Should Be Predictable

Important state transitions should occur through understandable actions or operations.

Hidden mutation should be avoided.

---

## 7. State and Data Ownership Should Be Distinguished

Information originating from a server, database, or external service does not automatically become application-owned state.

The architecture should distinguish:

- Information owned by an external source.
- Information temporarily retained by the application.
- Information owned and persisted by Exactly.
- Information derived from other state.

This distinction is particularly important for Server and Data State.

---

## 8. External Data Should Not Automatically Become Application State

Data retrieved from an API, database, or external service should be treated according to its lifecycle and ownership.

Not every response needs to become persistent application state.

Some information may be:

- Used once.
- Temporarily retained.
- Cached.
- Derived into another representation.
- Persisted because the product requires it.

---

## 9. State Should Reflect Real Product Needs

State architecture should be driven by actual application requirements rather than by the capabilities of a chosen state management library.

---

## 10. State Should Be Easy to Understand

Developers should be able to determine:

- Where state lives.
- Who owns it.
- What can change it.
- Who consumes it.
- How long it exists.
- Whether it is application-owned or externally owned.

---

## 11. Complexity Should Be Added Only When Necessary

Simple state should remain simple.

A sophisticated state architecture should only be introduced when product complexity justifies it.

---

## 12. State Transitions Should Be Separated From Side Effects

A state transition changes application state.

A side effect interacts with something outside the state being changed.

For example:

```text
User Action
    ↓
State Transition
    ↓
New State
```

is different from:

```text
User Action
    ↓
Side Effect
    ↓
API / Database / External Service
    ↓
Result
    ↓
State Transition
```

State management should define how state changes.

It should not become the uncontrolled owner of every side effect.

---

# State Categories

Exactly recognizes several conceptual categories of state.

```text
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
```

These categories describe responsibility and lifecycle.

They do not necessarily require separate technologies.

A single piece of information may move between categories during its lifecycle when its ownership or persistence requirements change.

---

# When Information Becomes State

Not every piece of information that passes through Exactly should become application state.

Information should generally become state when its value needs to be remembered because it affects future behavior, presentation, coordination, or user interaction.

Conceptually:

```text
Information
    ↓
Does it need to be remembered?
    ↓
   ┌───────────────┐
   │               │
  No              Yes
   │               │
Use and discard   State
                   ↓
              Assign Owner
                   ↓
              Define Lifecycle
```

Examples of information that may become state include:

- Current user selections.
- Active workflow progress.
- Retrieved information required for later interaction.
- Current processing status.
- Current result.
- Temporary user input.
- Persistent user preferences.

Information may not need to become state when it is:

- Used only once.
- Immediately transformed into another value.
- Available from an authoritative source when needed.
- Fully derivable from existing state.
- Relevant only to an operation that has already completed.

The decision should therefore not be:

> Can this information technically be stored?

The decision should be:

> Does the application need to remember this information, and if so, for what purpose and for how long?

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

```text
Component
    ↓
Local UI State
    ↓
Component
```

Local state should remain local when no other part of the application needs to know about it.

Local UI state should not be promoted to shared or application state merely because a state management mechanism makes that technically convenient.

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

```text
Feature
   ↓
Shared Feature State
   ↙       ↘
Component  Component
```

The state should remain within the feature boundary where practical.

Shared Feature State should be promoted to broader application state only when its consumers genuinely extend beyond the feature boundary.

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

The defining characteristic is broader coordination across otherwise independent application areas.

---

# Server and Data State

Server and Data State represents information whose primary source or authority exists outside the presentation layer.

Examples include:

- Data retrieved from an API.
- Stored user information.
- Product information.
- Retrieved evidence.
- Historical records.
- Intelligence results retrieved from a service.

Conceptually:

```text
External Source
     ↓
Data / API Boundary
     ↓
Application
     ↓
Presentation
```

Server and Data State should be distinguished from Application State.

### Server / Data State

Server / Data State is primarily owned by an external or persistent source.

Its important characteristics may include:

- Freshness.
- Synchronization.
- Retrieval status.
- Cache requirements.
- External ownership.
- Persistence outside the immediate application runtime.

### Application State

Application State is primarily owned by the running application and exists to coordinate application behavior.

Its characteristics may include:

- Current workflow context.
- Application-level preferences.
- Active selections.
- Session-level coordination.
- UI-independent application behavior.

The application may temporarily retain server data for performance, interaction, or coordination.

However, retaining server data locally does not automatically transfer authoritative ownership to the application.

Conceptually:

```text
Server / Persistent Source
          ↓
     Server Data
          ↓
   Application Retention
          ↓
   Application Consumers
```

The source of truth should remain clear.

---

# Persistent State

Persistent state survives beyond the immediate lifecycle of a component, interaction, or session.

Examples may include:

- User preferences.
- Account settings.
- Saved user information.
- Saved product data.
- Persistent application configuration.

Conceptually:

```text
Application
    ↓
Persistence Boundary
    ↓
Stored State
    ↓
Future Session
    ↓
Application
```

Persistent state should only be stored when there is a meaningful product requirement.

Persistence should not be introduced merely because information can be stored.

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

Its lifecycle should be explicitly tied to the interaction or operation that requires it.

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

```text
User Request
    ↓
Intelligence Operation
    ↓
Processing State
    ↓
Evaluation
    ↓
Result / Error
    ↓
User-Facing State
```

Intelligence processing state should be handled carefully because it may represent incomplete or intermediate information.

---

# Intelligence State Lifecycle

An intelligence operation may conceptually progress through:

```text
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
Evaluation
  ↓
Explanation
  ↓
Complete
```

An operation may instead enter an error state from an appropriate processing stage:

```text
Any Processing State
      ↓
    Error
      ↓
  Recovery
```

The exact state machine for intelligence features should be defined when specific workflows are implemented.

---

# Intelligence Evaluation and Result

Intelligence processing should not be treated as equivalent to a final product result.

Conceptually:

```text
Intelligence Processing
        ↓
   Intermediate Output
        ↓
     Evaluation
        ↓
Validated / Qualified Result
        ↓
     Explanation
        ↓
   User-Facing Result
```

Evaluation may consider, where appropriate:

- Evidence availability.
- Result completeness.
- Confidence or uncertainty.
- Consistency with product rules.
- Relevance to the requested context.
- Whether the result is suitable for presentation.

The exact evaluation mechanism belongs to the relevant Intelligence and Application architecture.

The important state-management principle is that:

> Processing state, intermediate intelligence output, evaluated result, and user-facing result should not be assumed to be the same state.

---

# State Ownership

Every meaningful state value should have an identifiable owner.

A simplified ownership model is:

| State Type                       | Typical Owner                       |
| -------------------------------- | ----------------------------------- |
| UI-only state                    | Component                           |
| Shared feature state             | Feature / Application boundary      |
| Application state                | Application layer                   |
| Server data                      | Data / External source              |
| Temporarily retained server data | Application / Data boundary         |
| Persistent state                 | Data / Persistence boundary         |
| Intelligence processing state    | Intelligence / Application boundary |
| User decision                    | User                                |

Ownership determines:

- Who may update the state.
- Where validation occurs.
- How state is persisted.
- How state is synchronized.
- Which components may consume it.
- Which source remains authoritative.

---

# State Ownership Rule

The owner of state should be the smallest architectural scope capable of coordinating all required consumers.

For example:

```text
One Component
    ↓
Local State
```

If multiple components need it:

```text
Shared Feature
    ↓
Feature State
```

If unrelated application areas need it:

```text
Application
    ↓
Application State
```

If an external system is authoritative:

```text
External / Persistent Source
    ↓
Server / Data State
```

This prevents unnecessary global state and keeps ownership understandable.

---

# State Mutation

State should change through intentional operations.

Conceptually:

```text
Current State
     ↓
Action / Event
     ↓
State Transition
     ↓
New State
```

The exact implementation may vary.

The architectural principle remains:

> State should change for an identifiable reason.

---

# State Transition and Side Effects

A state transition and a side effect are related but distinct.

## State Transition

A state transition changes the state managed by the relevant owner.

```text
Current State
     ↓
Action
     ↓
State Transition
     ↓
New State
```

Examples include:

- Changing the selected tab.
- Marking an operation as processing.
- Storing the current workflow step.
- Updating a temporary form value.

## Side Effect

A side effect interacts with something outside the state transition itself.

Examples include:

- Calling an API.
- Writing to a database.
- Calling an AI provider.
- Sending a message.
- Reading from persistent storage.
- Triggering analytics.

Conceptually:

```text
Action
   ↓
Side Effect
   ↓
External Result
   ↓
State Transition
```

or:

```text
User Action
     ↓
Application Operation
     ├── Side Effect
     │      ↓
     │ External Result
     │      ↓
     └── State Transition
              ↓
           New State
```

State management should coordinate the resulting state.

It should not blur the distinction between changing state and performing external operations.

---

# State Mutation Boundaries

A component should not directly modify state it does not own.

Conceptually:

```text
Component
    ↓
Defined Action
    ↓
State Owner
    ↓
State Update
```

This keeps ownership clear.

Components may request changes through defined actions or operations, while the appropriate state owner remains responsible for applying the transition.

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

```text
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
```

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

```text
Server Data
    ↕
Application Retention
    ↕
Presentation
```

Synchronization may be required when:

- Server data changes.
- User changes local data.
- Another operation modifies shared information.
- Persistent data is restored.
- An intelligence operation completes.

The synchronization mechanism should preserve a clear source of truth.

---

# Source of Truth

Each important piece of information should have a primary source of truth.

For example:

```text
Persistent User Preference
        ↓
    Data Source
        ↓
  Application State
        ↓
   Presentation
```

The presentation layer should not independently become a competing source of truth.

For server-owned information:

```text
Server / Persistent Source
        ↓
     Server Data
        ↓
  Local Representation
        ↓
   Presentation
```

The local representation may improve interaction or performance, but the authoritative source should remain identifiable.

---

# Derived State

Derived state is information that can be calculated from existing state.

Example:

```text
Selected Items
      ↓
Calculate Total
      ↓
Display Total
```

The calculated total does not necessarily need to exist as independently stored state.

This reduces synchronization risk.

Derived values should generally be recalculated from their source state unless there is a deliberate reason to materialize them.

---

# Loading State

Loading state represents an operation that has begun but has not yet produced a final result.

Conceptually:

```text
Idle
  ↓
Loading
  ↓
Success
  or
Error
```

The user interface should communicate loading states clearly.

---

# Processing State

Some Exactly operations may require more than a simple loading state.

For example:

```text
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
```

Where meaningful, processing state may communicate progress or system activity to the user.

The system should not expose unnecessary internal implementation details merely for the sake of showing progress.

---

# Error State

Errors should be represented as meaningful state where the user or application needs to respond to them.

Conceptually:

```text
Operation
   ↓
Failure
   ↓
Error State
   ↓
User Feedback
   ↓
Recovery / Retry / Exit
```

Errors should not silently disappear.

The error state should communicate enough information for the appropriate application or user response without exposing unnecessary internal implementation details.

---

# Empty State

An empty state is different from an error.

Examples:

- No saved information.
- No search results.
- No evidence available.
- No previous activity.

The application should distinguish:

```text
No Data
   ≠
Loading
   ≠
Error
```

This distinction is important for user understanding.

---

# Success State

A successful operation should produce a meaningful state representing completion.

Conceptually:

```text
Processing
   ↓
Success
   ↓
Result Available
   ↓
Presentation
```

Success state should not imply that the result is necessarily correct.

It means the operation completed successfully.

For intelligence operations, successful completion may still be followed by evaluation before the result is considered suitable for presentation.

---

# State and Components

Components consume state according to their responsibility.

Conceptually:

```text
State Owner
    ↓
Defined State
    ↓
Component
    ↓
Presentation
```

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

```text
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
```

This separation prevents state architecture from becoming an accidental consequence of component structure.

---

# State and Data Flow

Data Flow answers:

> How does information move?

State Management answers:

> What information must be remembered, where is it held, and how does it change?

Conceptually:

```text
Data Flow
    ↓
Information arrives
    ↓
State Management
    ↓
Information is retained / transformed / synchronized
    ↓
Presentation
```

Not every piece of data moving through the system needs to become long-lived state.

Some information may move through the system without being retained.

---

# State and API Architecture

API responses may become application state when the product requires the information to remain available for later interactions.

Conceptually:

```text
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
```

The API architecture will define the communication mechanism.

State Management defines whether and how the resulting information is retained.

An API response should not automatically become global application state.

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

```text
System State
    ↓
System Result
    ↓
User Understanding
    ↓
User Judgment
    ↓
User Decision
```

The user's final decision is not application state that Exactly should attempt to control.

The system may remember user-provided decisions when there is a legitimate product requirement, but the decision itself remains a user-owned concept.

---

# Duplicate State

Duplicated state occurs when the same conceptual information is independently stored in multiple locations.

Example:

```text
API Data
   ↓
Store A
   ↓
Component State
   ↓
Store B
```

This can create synchronization problems.

The preferred approach is:

```text
Single Source of Truth
        ↓
   Derived Views
        ↓
   Components
```

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

The architecture should be able to answer:

> Which representation is authoritative?

and:

> How and when are the representations synchronized?

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
- Intelligence processing states.
- Intelligence evaluation states.

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
- Treating every server response as global application state.
- Combining state transitions with uncontrolled side effects.

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
11. Is the source of truth clear?
12. Does changing this state require a side effect?
13. If a side effect is required, where does that responsibility belong?
14. If this is intelligence-related, is it processing state, evaluated state, or final result state?

If these questions cannot be answered clearly, the state design should be reconsidered before implementation.

---

# State Management Statement

Exactly should manage state according to clear ownership, appropriate lifecycle, and a single understandable source of truth.

Information should become state only when the application needs to remember it for future behavior, presentation, coordination, or interaction.

State should remain at the smallest appropriate scope, distinguish application-owned state from externally owned server and data state, and change through intentional transitions.

State transitions should remain conceptually separate from side effects, while synchronization between state and external systems should occur through defined architectural boundaries.

Intelligence-related state should distinguish processing, evaluation, and resulting information rather than treating all intermediate output as final state.

The architecture should avoid unnecessary global state, duplicated state, hidden mutation, unnecessary synchronization, and premature complexity.

> **Keep state where it belongs, change it deliberately, preserve its source of truth, and make its ownership clear.**
