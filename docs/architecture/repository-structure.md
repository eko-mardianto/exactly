# Repository Structure

## Purpose

Repository Structure defines the foundational organization of the Exactly repository.

It establishes clear boundaries between documentation, application code, public assets, and project-level configuration.

The structure is intended to make the repository understandable and maintainable while providing a stable foundation for future development.

This document defines the repository-level structure only.

Detailed application architecture will be defined separately during the System Architecture phase.

---

## Document Guide

| Section                 | Purpose                                                      |
| ----------------------- | ------------------------------------------------------------ |
| Purpose                 | Why the Repository Structure is needed                       |
| Repository Structure    | The foundational repository organization                     |
| Documentation Structure | How project documentation is organized                       |
| Application Structure   | The boundary for application code                            |
| Root Files              | Files that define or support the project at repository level |
| Structural Principles   | Principles that guide repository organization                |
| Repository Boundaries   | What is not defined by this document                         |
| Repository Statement    | The long-term repository organization principle              |

---

## Repository Structure

The Exactly repository uses the following foundational structure:

exactly/

├── docs/

│ ├── architecture/

│ ├── branding/

│ ├── decisions/

│ ├── developer/

│ ├── history/

│ ├── product/

│ ├── research/

│ ├── sprints/

│ └── ux/

│

├── app/

├── public/

│

├── README.md

├── LICENSE

└── .gitignore

This structure establishes the primary boundaries of the repository without prescribing the internal architecture of the application.

---

## Documentation Structure

The `docs/` directory contains the project's documented knowledge.

Each documentation area has a distinct responsibility.

### `docs/architecture/`

Contains documentation related to the technical structure and architectural direction of Exactly.

Examples include:

- Architecture Principles
- Repository Structure
- Architecture Overview
- Data Flow
- Component Strategy
- Other system architecture documentation

Detailed architecture documents may be added as the project progresses.

---

### `docs/branding/`

Contains documentation related to Exactly's brand identity and communication foundation.

Examples include:

- Brand Foundation
- Brand Language
- Brand Guidelines
- Other approved brand documentation

---

### `docs/decisions/`

Contains Architecture Decision Records and other significant documented decisions.

This directory provides traceability for decisions that materially affect the project.

---

### `docs/developer/`

Contains documentation intended to help developers understand and work with the project.

Examples may include:

- Development setup
- Development workflow
- Contribution guidance
- Technical conventions
- Development standards

---

### `docs/history/`

Contains the historical record of major project milestones and evolution.

This directory provides long-term context about how Exactly developed over time.

---

### `docs/product/`

Contains documentation related to product strategy and product definition.

Examples include:

- Product Foundation
- Product Positioning
- Product Scope
- Product Principles
- Other product-level documentation

---

### `docs/research/`

Contains research and supporting evidence used to inform product, design, technical, or strategic decisions.

Research should remain distinguishable from approved product decisions.

---

### `docs/sprints/`

Contains sprint-level planning and completion documentation.

Sprint documents describe how the project progresses toward the direction established by the roadmap.

---

### `docs/ux/`

Contains user experience documentation.

Examples include:

- Information Architecture
- User Journey
- User Flow
- Navigation Model
- Screen Hierarchy
- Interaction Principles

These documents establish the UX foundation before detailed interface design begins.

---

## Application Structure

The `app/` directory contains the application itself.

At the repository foundation level, `app/` represents the primary boundary for application implementation.

The internal structure of `app/` is intentionally not finalized by this document.

Application-level architecture will be defined during the System Architecture phase.

This prevents the repository structure from prematurely determining implementation details.

---

## Public Structure

The `public/` directory contains public assets required by the application.

The internal organization of this directory may evolve according to implementation requirements.

Its existence establishes a repository-level boundary for assets intended to be publicly accessible by the application.

---

## Root Files

Root-level files provide project-wide configuration, identification, licensing, and documentation.

### `README.md`

Provides the primary entry point for understanding the repository and project.

It should explain the purpose of Exactly and provide essential information required to understand or work with the project.

---

### `LICENSE`

Defines the legal licensing terms governing the project.

---

### `.gitignore`

Defines files and directories that should not be tracked by version control.

It should prevent generated files, local configuration, temporary files, and other inappropriate artifacts from entering the repository.

---

## Structural Principles

### Clear Boundaries

Each top-level directory should have a clear responsibility.

Documentation, application code, and public assets should remain distinguishable.

---

### Documentation by Responsibility

Documentation should be organized according to its purpose rather than accumulated in a single location.

This allows contributors to understand where different types of project knowledge belong.

---

### Stable Top-Level Structure

Top-level repository boundaries should remain relatively stable.

Internal structures may evolve as the project grows without unnecessarily restructuring the entire repository.

---

### Architecture Before Implementation Complexity

The repository structure should provide enough organization for development without prematurely defining detailed application architecture.

Detailed technical structures belong to the System Architecture phase.

---

### Avoid Unnecessary Nesting

Directories should only be introduced when they provide meaningful organizational value.

The repository should avoid excessive hierarchy that makes navigation and discovery more difficult.

---

### Documentation and Code Remain Distinct

Project knowledge should not be hidden inside implementation code when it belongs to formal project documentation.

Similarly, implementation details should not be placed into strategic documentation without a clear reason.

---

## Repository Boundaries

This document does not define:

- Internal application folders.
- Component architecture.
- Feature architecture.
- Service architecture.
- Data models.
- Database structure.
- API architecture.
- State management.
- AI architecture.
- Deployment architecture.
- Infrastructure architecture.
- Testing architecture.
- Framework-specific conventions.
- Programming language requirements.

These decisions belong to the System Architecture phase and subsequent technical documentation.

This document only establishes the foundational repository-level structure.

---

## Repository Statement

The Exactly repository should remain organized around clear boundaries between project knowledge, application implementation, and public assets.

The top-level structure should remain simple enough to understand while providing sufficient separation for long-term growth.

As Exactly evolves, internal structures may change to support new requirements.

The foundational repository boundaries should remain stable unless the project's long-term architecture requires otherwise.

> **The repository should make the project easier to understand before it makes the system more complex.**
