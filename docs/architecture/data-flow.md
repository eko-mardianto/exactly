# Data Flow

## Purpose

Data Flow defines how information moves through the Exactly system.

The purpose of this document is to establish a shared understanding of how user input, application processing, product logic, data, intelligence, evidence, reasoning, explanations, and results move between architectural boundaries.

This document translates the high-level architecture defined in `architecture-overview.md` into a conceptual information-flow model.

The focus is on the movement and transformation of information rather than specific implementation technologies.

---

## Document Guide

| Section                                      | Purpose                                                   |
| -------------------------------------------- | --------------------------------------------------------- |
| Purpose                                      | Why the Data Flow document is needed                      |
| Relationship to Other Architecture Documents | Clarifies the role of Data Flow within the architecture   |
| Data Flow Principles                         | Principles governing information movement                 |
| High-Level Data Flow                         | The primary system flow                                   |
| Core Information Flow                        | How information moves through Exactly                     |
| Input Flow                                   | How user input enters the system                          |
| Application Flow                             | How requests are coordinated                              |
| Domain Flow                                  | How product rules process information                     |
| Data Flow                                    | How stored and external information participates          |
| Intelligence Flow                            | How AI and reasoning capabilities participate             |
| Evidence Flow                                | How supporting information is handled                     |
| Reasoning Flow                               | How information is transformed into reasoning             |
| Explanation Flow                             | How reasoning becomes understandable to users             |
| Result Flow                                  | How processed information returns to the user             |
| Error Flow                                   | How failures move through the system                      |
| External Service Flow                        | How external services participate                         |
| Data Ownership                               | Which layer is responsible for information                |
| Trust and Responsibility Boundaries          | Where system responsibility ends and user judgment begins |
| Flow Boundaries                              | What this document does not define                        |
| Data Flow Evolution                          | How the flow may evolve                                   |
| Data Flow Statement                          | The long-term principle                                   |

---

# Relationship to Other Architecture Documents

Data Flow complements the other Phase 3 architecture documents.

| Document                     | Primary Question                                   |
| ---------------------------- | -------------------------------------------------- |
| `architecture-principles.md` | What principles govern the architecture?           |
| `repository-structure.md`    | Where do major project areas exist?                |
| `architecture-overview.md`   | What are the major system boundaries?              |
| `folder-structure.md`        | Where are application responsibilities organized?  |
| `component-strategy.md`      | How are responsibilities divided into components?  |
| `data-flow.md`               | How does information move through the system?      |
| `state-management.md`        | How is application state managed?                  |
| `api-architecture.md`        | How do system boundaries communicate through APIs? |

Data Flow therefore focuses specifically on **movement, transformation, and return of information**.

---

# Data Flow Principles

Exactly's data flow follows several principles.

## 1. Information Should Have a Clear Path

Important information should move through understandable architectural boundaries.

The origin, transformation, and destination of important information should remain traceable.

---

## 2. Responsibilities Should Remain Separated

Data should not move directly between unrelated architectural responsibilities simply because doing so is technically possible.

Appropriate boundaries should be respected.

---

## 3. Information Should Be Transformed Deliberately

When information changes meaning or structure, the transformation should occur at an appropriate architectural boundary.

---

## 4. Evidence Should Remain Distinguishable

Evidence, interpretation, reasoning, and final presentation should not be treated as the same type of information.

Exactly should preserve meaningful distinctions between them.

---

## 5. AI Output Should Not Automatically Become Truth

AI-generated information should be treated as a system-generated result that may require evaluation, supporting evidence, contextualization, or explanation.

The data flow should therefore support appropriate validation before information is presented as meaningful guidance.

---

## 6. User Judgment Remains Outside the System

Exactly may process and explain information.

The final decision remains with the user.

---

## 7. External Dependencies Should Remain Bounded

External services should participate through defined boundaries rather than becoming uncontrolled parts of the internal application flow.

---

## 8. Data Flow Should Remain Understandable

The system should avoid unnecessary transformations and indirect paths that make information difficult to trace.

---

# High-Level Data Flow

The simplest representation of the Exactly information flow is:

    User Input
        ↓
    Presentation
        ↓
    Application
        ↓
    Domain / Intelligence / Data
        ↓
    Result
        ↓
    Presentation
        ↓
    User

However, this model is intentionally simplified.

Exactly's core product flow is better represented as:

    User Input
        ↓
    Presentation
        ↓
    Application
        ↓
    Understand / Process
        ↓
    ┌──────────────┬──────────────┬──────────────┐
    │              │              │
    Domain        Data       Intelligence
    │              │              │
    └──────────────┴──────────────┘
                   ↓
            Evidence / Reasoning
                   ↓
              Explanation
                   ↓
                 Result
                   ↓
              Presentation
                   ↓
                  User

This represents the conceptual information flow of Exactly.

It is not an implementation-specific request pipeline.

---

# Core Information Flow

The core information journey can be represented as:

    1. User Input
          ↓
    2. Application Request
          ↓
    3. Context Understanding
          ↓
    4. Data / Domain / Intelligence Processing
          ↓
    5. Evidence
          ↓
    6. Reasoning
          ↓
    7. Explanation
          ↓
    8. Result
          ↓
    9. Presentation
          ↓
    10. User Understanding
          ↓
    11. User Decision

The final step is intentionally outside the system.

Exactly supports the user's understanding.

Exactly does not own the user's decision.

---

# Input Flow

User interaction begins at the presentation boundary.

    User
      ↓
    Presentation
      ↓
    User Input
      ↓
    Application

User input may include:

- Questions.
- Information supplied by the user.
- Preferences.
- Context.
- Requests for explanation.
- Requests for analysis.
- Decisions or criteria the user wants to evaluate.

The presentation layer is responsible for collecting and presenting the input.

The application layer is responsible for interpreting the input as an application operation.

---

# Input Validation

Input should be evaluated at the appropriate boundaries.

Conceptually:

    User Input
        ↓
    Presentation Validation
        ↓
    Application Validation
        ↓
    Domain Validation
        ↓
    Processing

Not every input requires every validation layer.

Validation should occur where the relevant responsibility exists.

Presentation validation should primarily protect the user experience.

Application validation should protect application operations.

Domain validation should protect product rules.

---

# Application Flow

The Application Layer coordinates the request.

Conceptually:

    User Input
        ↓
    Application
        ↓
    Identify Operation
        ↓
    Gather Required Context
        ↓
    Coordinate Required Capabilities
        ↓
    Produce Application Result

The Application Layer should not become a replacement for every underlying responsibility.

Its primary role is coordination.

---

# Context Flow

Before Exactly can process complex information, relevant context may need to be established.

Conceptually:

    User Input
        ↓
    Context
       ↙ ↘
    User   Existing Information
       ↘ ↙
    Application Context
        ↓
    Processing

Context may include:

- User-provided information.
- Existing product information.
- Relevant historical information.
- Retrieved information.
- Previous interaction state.
- Task-specific requirements.

The exact definition of context depends on the product capability being executed.

---

# Domain Flow

The Domain Layer applies product-level concepts and rules.

Conceptually:

    Application
        ↓
    Domain
        ↓
    Product Rules
        ↓
    Domain Result
        ↓
    Application

Domain processing may determine:

- What information is valid.
- What product rules apply.
- What relationships exist between concepts.
- What operations are allowed.
- What product-level result should be produced.

Domain logic should remain independent from presentation concerns.

---

# Data Flow

The Data Layer provides information required by application and domain operations.

Conceptually:

    Application / Domain
          ↓
       Data Request
          ↓
        Data Layer
         ↙      ↘
    Stored Data  External Data
         ↘      ↙
        Retrieved Information
               ↓
        Application / Domain

Data may include:

- User information.
- Product information.
- Stored context.
- Historical information.
- Retrieved information.
- Supporting evidence.

The exact persistence mechanism is not defined by this document.

---

# Data Retrieval

When information is required, the system may retrieve it from an appropriate source.

Conceptually:

    Request
      ↓
    Data Boundary
      ↓
    Identify Source
      ↓
    Retrieve Information
      ↓
    Validate / Transform
      ↓
    Return Information

Retrieved information should remain distinguishable from information generated by Exactly itself.

---

# Intelligence Flow

The Intelligence Layer supports AI-assisted reasoning and information processing.

Conceptually:

    Application
        ↓
    Intelligence Request
        ↓
    Context
        ↓
    Relevant Information
        ↓
    Intelligence Processing
        ↓
    AI / Reasoning Result
        ↓
    Evaluation / Processing
        ↓
    Application

The Intelligence Layer may use external AI services.

However, external AI providers remain outside the internal system boundary.

---

# Intelligence and External Services

The conceptual flow is:

    Intelligence Layer
          ↓
    External Service Boundary
          ↓
    AI / External Provider
          ↓
    External Result
          ↓
    Intelligence Layer
          ↓
    Evaluation / Transformation

The Intelligence Layer should isolate provider-specific implementation details from the rest of the application where practical.

---

# Evidence Flow

Evidence represents information that supports an explanation, interpretation, or reasoning process.

Conceptually:

    Data / External Information
            ↓
          Evidence
            ↓
      Evidence Processing
            ↓
    Relevant Supporting Information
            ↓
        Reasoning

Evidence should remain distinguishable from conclusions.

Exactly should avoid presenting generated interpretations as though they were raw evidence.

---

# Evidence Classification

Information may conceptually fall into different categories:

    Source Information
          ↓
    Retrieved / Stored Information
          ↓
    Processed Information
          ↓
    Interpretation
          ↓
    Reasoning
          ↓
    Explanation

The system should preserve meaningful distinctions between these categories where practical.

The exact data model for these categories will be defined separately when required.

---

# Reasoning Flow

Reasoning represents the transformation of available information into a structured interpretation.

Conceptually:

    Context
       +
    Evidence
       +
    Product Rules
       +
    Intelligence
       ↓
    Reasoning
       ↓
    Structured Interpretation

Reasoning should not be treated as an unexplained black box where transparency is important to the user experience.

Where appropriate, the system should retain enough information to explain how a result was produced.

---

# Explanation Flow

Explanation transforms processed information and reasoning into something understandable to the user.

Conceptually:

    Evidence
       +
    Reasoning
       +
    Context
       ↓
    Explanation
       ↓
    User-Facing Result

Explanation should prioritize:

- Clarity.
- Context.
- Appropriate evidence.
- Understandable reasoning.
- Appropriate uncertainty.
- User comprehension.

Explanation is therefore not simply a visual rendering of an AI response.

It is a product responsibility.

---

# Result Flow

The result moves back through the application boundary toward the presentation layer.

Conceptually:

    Processing
        ↓
    Result
        ↓
    Application
        ↓
    Presentation
        ↓
    User

The result may contain:

- Information.
- Evidence.
- Reasoning.
- Explanation.
- Warnings.
- Uncertainty.
- Recommended next steps for further understanding.

The exact content depends on the use case.

---

# User Understanding Flow

Exactly's information flow has a deliberate final conceptual stage:

    Result
      ↓
    Presentation
      ↓
    User Understanding
      ↓
    User Judgment
      ↓
    User Decision

The last two steps occur outside the system.

Exactly may support them, but does not control them.

---

# Trust Boundary

A critical boundary exists between:

    System Output
         ↓
    User Understanding
         ↓
    User Judgment
         ↓
    User Decision

Exactly should not imply that a generated result is automatically the correct decision.

The system should support informed judgment rather than replace it.

---

# Error Flow

Errors should move through appropriate architectural boundaries.

Conceptually:

    Failure
      ↓
    Responsible Layer
      ↓
    Error Classification
      ↓
    Application Boundary
      ↓
    User-Facing State
      ↓
    Presentation
      ↓
    User

Errors may originate from:

- Invalid input.
- Domain rules.
- Data retrieval.
- External services.
- Intelligence processing.
- Infrastructure.
- Unexpected system behavior.

The presentation layer should communicate appropriate user-facing feedback without needing to understand every internal implementation detail.

---

# Partial Result Flow

Some operations may produce incomplete or intermediate information.

Conceptually:

    Request
      ↓
    Processing
      ↓
    Partial Information
      ↓
    Evaluation
      ↓
    User-Facing State

The system should not present incomplete information as complete merely because some output is available.

The treatment of partial results depends on the specific product workflow.

---

# External Service Flow

External services remain outside Exactly's core system boundary.

Conceptually:

    Exactly
       ↓
    Integration Boundary
       ↓
    External Service
       ↓
    External Response
       ↓
    Validation / Processing
       ↓
    Exactly

Examples may include:

- AI providers.
- External information sources.
- Authentication providers.
- Communication services.
- Other third-party integrations.

External responses should not automatically bypass internal processing and appear directly to users.

---

# Data Ownership

Each architectural area should have a clear responsibility for the information it creates or manages.

Conceptually:

| Information            | Primary Responsibility                                                |
| ---------------------- | --------------------------------------------------------------------- |
| User Input             | Presentation / Application                                            |
| Product Rules          | Domain                                                                |
| Persistent Information | Data                                                                  |
| AI Processing          | Intelligence                                                          |
| Evidence               | Data / Intelligence depending on origin and processing                |
| Reasoning              | Intelligence / Domain                                                 |
| Explanation            | Application / Intelligence / Presentation depending on responsibility |
| Final User Decision    | User                                                                  |

These boundaries are conceptual and may be refined as specific product requirements are implemented.

---

# Data Transformation

Information may change form as it moves through the system.

A simplified transformation is:

    Raw Input
        ↓
    Structured Input
        ↓
    Context
        ↓
    Retrieved Information
        ↓
    Evidence
        ↓
    Reasoning
        ↓
    Explanation
        ↓
    User-Facing Result

Each transformation should have a clear purpose.

The system should avoid transformations that add complexity without meaningful value.

---

# Data Integrity

Information should remain accurate and traceable through transformations where practical.

The system should avoid unnecessary loss of:

- Source context.
- Evidence.
- Meaning.
- Uncertainty.
- Provenance.

When information is transformed, the system should preserve relevant context required for responsible interpretation.

---

# Data Sensitivity

Sensitive information should be handled according to the appropriate security and privacy requirements.

Data flow should consider:

- What information is collected.
- Why it is required.
- Where it is stored.
- Where it is transmitted.
- Which external services receive it.
- How long it is retained.

Specific security architecture and privacy requirements will be documented separately when necessary.

---

# Flow Observability

Important information flows should be observable enough to support:

- Debugging.
- Monitoring.
- Evaluation.
- Error analysis.
- Performance analysis.
- System improvement.

Observability should not require exposing sensitive user information unnecessarily.

---

# Data Flow and User Experience

Data flow should support the established UX foundation.

The technical movement of information should enable:

- Clear user journeys.
- Predictable user flows.
- Understandable navigation.
- Meaningful feedback.
- Appropriate loading states.
- Clear evidence presentation.
- Understandable explanations.

Technical implementation should not create unnecessary friction in the user experience.

---

# Data Flow and Component Strategy

Components interact with data flow through defined architectural boundaries.

Conceptually:

    Component
        ↓
    Application Operation
        ↓
    Domain / Data / Intelligence
        ↓
    Result
        ↓
    Application
        ↓
    Component

Presentation components should not bypass these boundaries simply to retrieve or modify information directly.

---

# Data Flow and State Management

Data flow and state management are related but different concepts.

Data Flow answers:

> How does information move?

State Management answers:

> Where does application state live, and how does it change?

State management will therefore be defined separately in:

    docs/architecture/state-management.md

The Data Flow document should remain focused on information movement.

---

# Data Flow and API Architecture

Data Flow describes conceptual information movement.

API Architecture will later define how system boundaries communicate technically.

For example:

    Data Flow
        ↓
    User Request
        ↓
    Application
        ↓
    Intelligence
        ↓
    Result

API Architecture may later define:

    HTTP Request
        ↓
    Endpoint
        ↓
    Application Service
        ↓
    Intelligence Service
        ↓
    Response

The API layer is therefore an implementation mechanism for certain parts of the broader data flow.

---

# Complete Conceptual Flow

The complete high-level Exactly information flow can be summarized as:

    User
      ↓
    User Input
      ↓
    Presentation
      ↓
    Application
      ↓
    Context
      ↓
    ┌─────────────────────────────────────┐
    │                                     │
    ↓                                     ↓
    Domain                              Data
    │                                     │
    │                                     ↓
    │                              Retrieved Information
    │                                     │
    └────────────────┬────────────────────┘
                     ↓
              Intelligence
                     ↓
             Evidence Processing
                     ↓
                Reasoning
                     ↓
                Explanation
                     ↓
                  Result
                     ↓
               Application
                     ↓
                Presentation
                     ↓
                   User
                     ↓
             Understanding
                     ↓
                 Judgment
                     ↓
                 Decision

The final stages remain under the user's responsibility.

---

# Alternative Flow

Not every user request requires the complete flow.

For example, a simple information display may follow:

    User
      ↓
    Presentation
      ↓
    Application
      ↓
    Data
      ↓
    Result
      ↓
    Presentation
      ↓
    User

An intelligence-heavy workflow may follow:

    User
      ↓
    Presentation
      ↓
    Application
      ↓
    Data
      ↓
    Intelligence
      ↓
    Evidence
      ↓
    Reasoning
      ↓
    Explanation
      ↓
    Result
      ↓
    Presentation
      ↓
    User

The architecture should support both simple and complex flows without forcing unnecessary processing into every operation.

---

# Flow Boundaries

This document intentionally does not define:

- Specific API endpoints.
- Specific HTTP methods.
- Specific request and response schemas.
- Database schemas.
- Exact database queries.
- Exact AI prompts.
- AI model selection.
- External provider selection.
- State management implementation.
- UI component implementation.
- Exact event architecture.
- Authentication implementation.
- Authorization implementation.
- Infrastructure architecture.
- Deployment architecture.
- Monitoring implementation.

These decisions belong to subsequent architecture documents or Architecture Decision Records.

---

# Data Flow Evolution

The data flow may evolve as the product becomes more concrete.

Changes may be required when:

- New product capabilities are introduced.
- New information sources are added.
- Intelligence capabilities change.
- External dependencies change.
- Data ownership changes.
- Security requirements change.
- Performance requirements reveal a better flow.

Changes that materially affect architectural boundaries should be documented appropriately.

---

# Data Flow Statement

Exactly should move information through clear and meaningful boundaries.

User input should be transformed into structured context, processed through the appropriate combination of domain logic, data, and intelligence capabilities, supported by evidence where appropriate, transformed into understandable reasoning and explanation, and returned to the user through the presentation layer.

The architecture should preserve the distinction between information, evidence, reasoning, explanation, and user judgment.

The system should help users understand.

It should not take ownership of their decisions.

> **Move information clearly, preserve its meaning, and return understanding to the user.**
