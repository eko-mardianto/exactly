# Folder Structure

## Purpose

Folder Structure defines the internal organization of the Exactly application.

It translates the major architectural responsibilities defined in `architecture-overview.md` into a clear application-level structure.

The purpose of this structure is to make responsibilities, dependencies, and boundaries easier to understand before major implementation begins.

This document defines the foundational application folder structure.

It does not define individual features, implementation details, or framework-specific conventions unless those decisions are established separately.

---

## Document Guide

| Section                    | Purpose                                                     |
| -------------------------- | ----------------------------------------------------------- |
| Purpose                    | Why the Folder Structure is needed                          |
| Application Structure      | The foundational structure of the application               |
| Folder Responsibilities    | The responsibility of each major application area           |
| Architectural Boundaries   | The boundaries that should remain between application areas |
| Dependency Direction       | The intended direction of dependencies                      |
| Structural Principles      | Principles governing folder organization                    |
| Future Expansion           | How the structure may evolve                                |
| Architecture Boundaries    | What is not defined by this document                        |
| Folder Structure Statement | The long-term structural principle                          |

---

## Application Structure

The application is organized around major responsibilities rather than individual features.

The foundational structure is:

    app/

    ├── presentation/
    │
    ├── application/
    │
    ├── domain/
    │
    ├── data/
    │
    └── intelligence/

Each major area represents a distinct architectural responsibility.

The structure is intended to support clear boundaries and controlled dependencies.

---

## Folder Responsibilities

### `presentation/`

The `presentation/` directory contains the user-facing presentation layer.

Its responsibility is to translate application state and results into an interface that users can understand and interact with.

Responsibilities include:

- User interface elements.
- Page-level presentation.
- Interaction handling.
- Presentation-specific state.
- User-facing feedback.
- Visual representation of application results.

The presentation layer should not contain core business rules or reasoning logic.

It should communicate with the application layer through defined interfaces.

---

### `application/`

The `application/` directory contains application-level coordination and use-case logic.

Its responsibility is to coordinate actions between the presentation layer and the underlying product capabilities.

Responsibilities may include:

- Use cases.
- Application workflows.
- Request coordination.
- Input validation at the application boundary.
- Coordination between domain, data, and intelligence capabilities.
- Application-level response preparation.

The application layer should coordinate responsibilities rather than become a container for unrelated business logic.

---

### `domain/`

The `domain/` directory contains core product concepts and rules.

Its responsibility is to represent the concepts that define how Exactly operates independently from presentation or infrastructure concerns.

Responsibilities may include:

- Core product entities.
- Domain concepts.
- Business rules.
- Domain constraints.
- Domain-level operations.
- Product-specific behavior.

The domain should remain as independent as practical from external services and presentation concerns.

---

### `data/`

The `data/` directory contains data access and persistence-related responsibilities.

Its responsibility is to provide controlled access to information required by the application.

Responsibilities may include:

- Data access.
- Persistence.
- Retrieval.
- Data transformation at the data boundary.
- External data sources.
- Storage adapters.

The data layer should isolate storage and external data implementation details from higher-level product logic where practical.

---

### `intelligence/`

The `intelligence/` directory contains AI and reasoning-related capabilities.

Its responsibility is to support Exactly's intelligence functions while remaining separate from presentation and general application coordination.

Responsibilities may include:

- Reasoning workflows.
- AI interaction.
- Evidence processing.
- Explanation generation.
- Intelligence-related transformations.
- Evaluation-related logic.
- AI provider integration boundaries.

The intelligence layer should not become the location for unrelated application logic.

AI capabilities should remain subject to appropriate validation and evaluation.

---

# Architectural Boundaries

The major application areas establish conceptual boundaries:

    presentation/
          ↓
    application/
          ↓
       domain/
       ↙     ↘
    data/   intelligence/

These boundaries describe responsibility and dependency direction.

They do not imply that every request must pass through every folder.

Specific flows may use only the layers required by the use case.

For example, a presentation interaction may invoke an application use case that requires domain logic and data access without involving the intelligence layer.

Likewise, an intelligence workflow may require data access without directly depending on presentation concerns.

---

## Dependency Direction

Dependencies should generally move toward more stable product responsibilities.

A simplified direction is:

    presentation
          ↓
    application
          ↓
       domain

External or implementation-specific concerns should remain behind appropriate boundaries.

Conceptually:

    presentation
          ↓
    application
          ↓
    domain
       ↙     ↘
    data   intelligence
       ↘     ↙
    External Services

This is a guiding dependency model rather than a requirement that every module depend directly on the next layer.

The purpose is to prevent higher-level product concepts from becoming tightly coupled to presentation or external implementation details.

---

## Dependency Principles

### Presentation Independence

Core product rules should not depend on presentation components.

The domain should not require knowledge of how information is displayed.

---

### Domain Independence

Domain concepts should remain as independent as practical from:

- UI frameworks.
- Storage implementations.
- External providers.
- AI providers.
- Infrastructure.

---

### External Dependency Isolation

External services should be accessed through appropriate boundaries.

The application should avoid allowing provider-specific implementation details to spread unnecessarily throughout the system.

---

### Controlled Intelligence Dependency

AI and reasoning capabilities should be treated as a distinct architectural responsibility.

The rest of the application should interact with intelligence capabilities through clear interfaces rather than depending directly on provider-specific implementation details.

---

## Structural Principles

### Organize by Responsibility

Top-level application folders should represent meaningful architectural responsibilities.

They should not exist merely because a framework or coding convention commonly uses them.

---

### Keep Boundaries Clear

A folder should have a clear reason to exist.

Responsibilities should not overlap unnecessarily between major architectural areas.

---

### Avoid Premature Feature Structure

Individual features should not determine the entire application architecture before the major system boundaries are understood.

Feature organization may evolve within these boundaries.

---

### Avoid Excessive Nesting

Folders should only be introduced when they provide meaningful organizational value.

The structure should remain discoverable.

---

### Prefer Explicit Dependencies

Important dependencies should be understandable from the structure and interfaces.

Hidden coupling should be avoided.

---

### Keep Infrastructure Replaceable

Technology-specific implementation should remain isolated where practical.

Replacing an external provider or infrastructure component should not require unnecessary changes throughout the application.

---

## Feature Expansion

As Exactly grows, features may be introduced within the established architectural boundaries.

For example:

    presentation/
    ├── ...
    └── features/

    application/
    ├── ...
    └── use-cases/

    domain/
    ├── ...
    └── ...

    data/
    ├── ...
    └── ...

    intelligence/
    ├── ...
    └── ...

These examples are illustrative only.

Specific feature structures should be defined when actual product requirements require them.

---

## Future Expansion

The architecture may eventually require additional boundaries.

Potential future areas may include:

- Authentication.
- Authorization.
- Observability.
- Background processing.
- Integrations.
- Infrastructure.
- Configuration.
- Evaluation.
- Analytics.

These should only become dedicated architectural areas when their responsibilities justify the additional structure.

The project should avoid creating empty or speculative folders solely for anticipated future requirements.

---

## Relationship to Repository Structure

The repository-level structure defined in `repository-structure.md` remains the higher-level boundary.

The relationship is:

    exactly/
    │
    ├── docs/
    │
    ├── app/
    │   ├── presentation/
    │   ├── application/
    │   ├── domain/
    │   ├── data/
    │   └── intelligence/
    │
    └── public/

`repository-structure.md` defines where the application exists within the repository.

This document defines how the application is organized internally.

The two documents should remain complementary rather than overlapping.

---

## Relationship to Architecture Overview

The folder structure translates the major responsibilities defined in `architecture-overview.md`.

| Architecture Overview | Folder Structure                        |
| --------------------- | --------------------------------------- |
| User Interface        | `presentation/`                         |
| Application Layer     | `application/`                          |
| Product Logic         | `domain/`                               |
| Data Layer            | `data/`                                 |
| Intelligence Layer    | `intelligence/`                         |
| External Services     | Accessed through appropriate boundaries |

The mapping is conceptual.

Implementation details may evolve without changing the underlying architectural responsibilities.

---

## Relationship to Architecture Principles

The folder structure applies the principles defined in `architecture-principles.md`.

### Modularity

Each major folder represents a meaningful architectural responsibility.

### Separation of Concerns

Presentation, application coordination, domain logic, data access, and intelligence capabilities remain distinguishable.

### Simplicity

Only meaningful architectural boundaries are established.

### Maintainability

Responsibilities are organized so that future contributors can understand where different types of logic belong.

### Scalability

The structure can expand through additional modules and features without requiring immediate restructuring of the entire application.

### Security

Sensitive responsibilities and external boundaries can be isolated appropriately.

### Testability

Clear boundaries make individual responsibilities easier to verify.

### Documentation

The structure is explicitly documented so that architectural intent remains understandable.

---

## Architecture Boundaries

This document does not define:

- Specific framework conventions.
- Exact component names.
- Exact feature folders.
- Exact file names inside each folder.
- Database schema.
- API endpoint structure.
- API protocol.
- State management implementation.
- Authentication implementation.
- Authorization implementation.
- AI model selection.
- AI provider selection.
- Deployment structure.
- Infrastructure configuration.
- Testing framework.
- CI/CD configuration.

These decisions belong to later architecture documentation or Architecture Decision Records.

---

## Folder Structure Evolution

The folder structure may evolve when the system's responsibilities become better understood.

Structural changes should be evaluated against:

- Existing architectural boundaries.
- Dependency direction.
- Maintainability.
- Simplicity.
- Actual product requirements.

New folders should be introduced because a responsibility exists, not merely because a future responsibility is possible.

Major structural changes should be documented when they materially affect the architecture.

---

## Folder Structure Statement

Exactly's application structure should make responsibilities clear before implementation becomes complex.

The structure should separate presentation, application coordination, product logic, data access, and intelligence capabilities while keeping unnecessary complexity out of the system.

The folder structure should evolve with the product without losing the architectural boundaries that make the system understandable.

> **Organize the code so that responsibility is clear before complexity grows.**
