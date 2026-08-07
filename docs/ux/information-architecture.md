# Information Architecture

## Purpose

Information Architecture (IA) defines how product information is organized before navigation, interfaces, or implementation are designed.

This document establishes the logical structure of information that supports understanding, reasoning, and decision-making throughout Exactly.

As the product evolves, Information Architecture provides a stable foundation for navigation, interaction design, system architecture, and future product development.

---

## Information Principles

Information Principles define the enduring principles that guide how information should be organized throughout Exactly.

These principles ensure that information remains understandable, connected, and useful as the product evolves.

---

### Context Before Details

Information should provide sufficient context before presenting detailed content.

Users should understand what they are looking at before exploring supporting information.

---

### Evidence Before Conclusions

Supporting evidence should be presented before conclusions whenever possible.

Information should encourage understanding rather than passive acceptance.

---

### Progressive Disclosure

Information should be revealed progressively to reduce cognitive load without hiding important context.

Users should be able to explore deeper levels of information when needed.

---

### Connected Information

Related information should remain meaningfully connected across the product.

Users should easily understand how one piece of information relates to another.

---

### Clarity Before Quantity

Only information that supports understanding should be presented.

Additional information should improve clarity rather than increase complexity.

---

Information should always be organized to improve understanding rather than simply storing or displaying data.

---

## Information Domains

Information Domains define the major categories of information managed throughout Exactly.

Domains represent logical groups of information rather than application screens, navigation sections, or implementation modules.

The initial information domains include:

- User
- Question
- Topic
- Evidence
- Reasoning
- Explanation
- Insight
- Decision
- History
- Reference

These domains may evolve as the product expands while preserving the overall information model.

---

## Information Entities

Information Entities represent the fundamental units of information contained within each domain.

Examples include:

- Evidence
- Source
- Claim
- Topic
- Finding
- Reasoning
- Explanation
- Insight
- Decision
- Confidence
- Reference

Information Entities should remain independent from interface components, database structures, API design, and implementation details.

---

## Information Relationships

Information Relationships define how Information Entities connect throughout the product.

Rather than existing independently, information should form meaningful relationships that support reasoning and understanding.

For example:

Evidence

↓

supports

↓

Claim

↓

contributes to

↓

Reasoning

↓

produces

↓

Insight

↓

supports

↓

Decision

Information relationships should remain explicit, understandable, and traceable whenever possible.

---

## Information Flow

Information Flow describes how information progresses throughout Exactly.

Unlike user flows or navigation flows, Information Flow focuses on the logical movement of information itself.

A typical information flow follows:

Input

↓

Collect

↓

Analyze

↓

Evaluate

↓

Explain

↓

Understand

↓

Decide

Every stage should contribute to improving understanding before supporting decision-making.

---

## Information Architecture Statement

Information Architecture establishes the structural foundation of knowledge throughout Exactly.

It provides a shared information model that supports product strategy, user experience, interface design, and engineering decisions.

As the product evolves, new domains, entities, relationships, and information flows may be introduced.

The underlying principles and organizational model should remain stable to ensure consistency, maintainability, and long-term product clarity.
