# Core UX MVP

## Purpose

Core UX MVP defines the essential user experience that Exactly must provide during its initial MVP phase.

This document translates Exactly's product principles, information architecture, interaction principles, user flow, and user journey into a concrete experience model for the MVP.

It establishes the core experience from the moment a user provides a question until Exactly helps the user develop an initial understanding of how relevant information relates to that question.

This document serves as the source of truth for the MVP's core user experience before screen structure, interaction states, component design, and implementation are defined.

---

## Core UX Principles

The Core UX MVP is guided by the following principles:

### Question Before Structure

Users should be able to begin with a question expressed in their own words.

Exactly should not require users to understand how to structure their question before beginning exploration.

---

### Clarification Before Assumption

When a question is ambiguous or insufficiently specific to support meaningful exploration, Exactly should request clarification before proceeding.

Exactly should not silently assume the user's intent when multiple reasonable interpretations exist.

---

### Explore Before Understanding

Users should first be able to examine relevant context and information before Exactly helps them understand how those pieces relate to their question.

---

### Evidence on Demand

Supporting evidence should remain accessible without becoming the primary content presented at the beginning of exploration.

Users should be able to inspect supporting material when they want to verify or investigate information more deeply.

---

### User-Controlled Progression

Users should control when they move from Explore to Understand.

Exactly should not decide on the user's behalf that they have explored enough.

---

### Support Understanding, Not Decisions

The MVP should help users develop understanding without replacing their judgment or making decisions on their behalf.

---

## Core Experience

The Exactly MVP follows a question-first experience.

The core experience is:

Question

↓

Clarity Check

↓

Clarification when necessary

↓

Explore

↓

Understand

The experience begins with the user's own question and progresses toward understanding rather than immediately producing a recommendation or decision.

---

## Question Input v1

Exactly MVP uses a free-form question input supported by optional guidance.

Users are not required to structure their question or provide additional context before starting exploration.

The question input should allow users to communicate what they want to understand in natural language.

Optional guidance may help users formulate a clearer question, but guidance should not become a required form or structured process.

The Question Input should not require users to provide:

- Topic.
- Goal.
- Context.
- Decision type.
- Structured categories.

before exploration can begin.

The purpose of the Question Input is to provide a natural starting point for the user's exploration.

---

## Clarity Check

After a question is provided, Exactly determines whether the question is sufficiently clear to support meaningful exploration.

The Clarity Check should distinguish between:

- Questions that provide sufficient direction for exploration.
- Questions that are ambiguous or insufficiently specific.

The Clarity Check should not require users to provide unnecessary detail when the existing question is already sufficient.

---

## Clarification Behavior

When a user's question is ambiguous or insufficiently specific to support meaningful exploration, Exactly should request clarification before proceeding.

Exactly should not silently assume the user's intent when multiple reasonable interpretations exist.

The clarification should help the user resolve the ambiguity without unnecessarily requiring them to reformulate the entire question.

A clarification may present relevant interpretations or ask a focused follow-up question.

The resulting flow is:

Question

↓

Clarity Check

↓

Ambiguous

↓

Clarification

↓

Explore

Clarification exists to establish sufficient context for exploration, not to turn the Question Input into a required structured form.

---

## Explore v1

Explore is the stage where users examine the context and information relevant to their question.

Explore should first present:

- Context.
- Relevant information.
- Meaningful relationships between available information when appropriate.

Supporting evidence should remain accessible without becoming the primary content shown upfront.

The initial Explore experience should prioritize helping users understand what information is relevant before requiring them to inspect supporting material.

---

## Evidence Behavior

Evidence should be available as supporting material that users can open when they want to inspect or verify the information being presented.

The primary Explore experience should not overwhelm users by presenting all supporting evidence simultaneously.

The conceptual relationship is:

Context

↓

Relevant Information

↓

Supporting Evidence

Evidence should remain connected to the information it supports so that users can understand why the evidence is relevant when they choose to inspect it.

Evidence should not be hidden in a way that prevents users from verifying important information.

---

## Explore → Understand Progression

The transition from Explore to Understand is controlled by the user.

The user explores relevant information and decides when they are ready to move forward.

Exactly should not determine on the user's behalf that they have explored enough.

The conceptual flow is:

Explore

↓

User explores relevant information

↓

User chooses when ready

↓

Understand

This preserves user control and prevents the system from treating exploration as a fixed automated process.

---

## Understand v1

Understand is the stage where Exactly helps users connect relevant information and understand how those pieces relate to their question.

The core principle is:

**Exactly helps users connect relevant information and understand how those pieces relate to their question.**

Understand should help users move from individual pieces of information toward a coherent understanding of their relationship to the original question.

The conceptual structure is:

Relevant Information

↓

Connections

↓

Relationships

↓

Understanding

Understanding should remain connected to the original question so that users can see why the relationships matter.

---

## Reasoning in Understand

Exactly should make relevant reasoning understandable rather than presenting unexplained synthesis.

The system may help explain:

- How pieces of information relate.
- Why a relationship is relevant to the question.
- What reasoning connects the information.
- Which supporting evidence is available for inspection.

Reasoning should remain transparent enough for users to examine rather than requiring passive acceptance of an unexplained result.

---

## Boundaries of Understand v1

Understand v1 does not make decisions on behalf of users.

It should not automatically become:

- A recommendation engine.
- An autonomous decision-maker.
- A final decision.
- A predetermined conclusion presented as unquestionable.
- An instruction telling the user what they should do.

The purpose of Understand is to strengthen understanding before later evaluation or decision-making.

---

## MVP Vertical Slice

The initial Exactly MVP vertical slice is intentionally limited to:

Question

↓

Clarity Check

↓

Clarification when necessary

↓

Explore

↓

User-controlled progression

↓

Understand

This vertical slice establishes the core experience of moving from a question toward understanding.

The following stages are not part of this initial vertical slice:

- Evaluate.
- Decide.
- Reflect.

They remain part of the broader Exactly User Flow and User Journey but are outside the first implementation scope of the Core UX MVP.

---

## MVP Boundaries

The Core UX MVP does not define:

- Screen structure.
- Screen layouts.
- Visual design.
- Typography.
- Color systems.
- Component specifications.
- Responsive behavior.
- Animation or motion design.
- Database structures.
- API architecture.
- AI model selection.
- Technical implementation.
- Deployment architecture.

These decisions should be defined in later documentation after the Core UX MVP has been established.

---

## Core UX Relationship to Existing Product Foundations

The Core UX MVP implements the principles established by the broader Exactly product documentation.

The relationship is:

Product Foundation

↓

Product Positioning

↓

Design Principles

↓

Information Architecture

↓

Interaction Principles

↓

User Flow and User Journey

↓

Core UX MVP

↓

Screen Structure

↓

Screen State Model

↓

Component Structure

↓

Implementation

Each later layer should remain consistent with the decisions established by the layers above it.

---

## Core UX Statement

Exactly MVP begins with the user's question rather than a predetermined workflow.

It allows users to express questions freely, requests clarification when meaningful exploration would otherwise require assumptions, and helps users explore relevant context and information before developing understanding.

Evidence remains accessible as supporting material without overwhelming the initial experience.

Users remain in control of when they move from exploration to understanding.

Exactly then helps users connect relevant information and understand how those pieces relate to their question.

The purpose of the MVP is not to make decisions for users.

It is to establish the foundation for a product that strengthens human judgment through understanding.
