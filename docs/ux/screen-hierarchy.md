# Screen Hierarchy

## Purpose

Screen Hierarchy defines how information is prioritized and structured within the interface of Exactly.

Rather than defining visual design or individual interface components, this document establishes the relationship between information, screens, and content priority.

It provides the foundation for designing interfaces that present the right information at the right level of attention without unnecessary complexity.

---

## Document Guide

| Section              | Purpose                                                   |
| -------------------- | --------------------------------------------------------- |
| Purpose              | Why the Screen Hierarchy is needed                        |
| Hierarchy Principles | Principles that guide information priority within screens |
| Screen Levels        | The structural levels of screens                          |
| Content Hierarchy    | How information is prioritized within a screen            |
| Screen Relationships | How screens relate to one another                         |
| Screen Priorities    | What information should receive the most attention        |
| Screen Boundaries    | What is not defined by the Screen Hierarchy               |
| Statement            | The long-term principle                                   |

---

## Hierarchy Principles

Hierarchy Principles define the enduring principles that guide how information should be prioritized within screens.

These principles ensure that interfaces support understanding without overwhelming users with unnecessary information.

---

### Primary Information First

The most important information required to understand the current context should receive the highest level of attention.

Users should not need to search through secondary information to understand the primary subject of a screen.

---

### Context Before Detail

A screen should establish sufficient context before presenting supporting details.

Users should understand what they are viewing before being presented with deeper information.

---

### Understanding Before Action

Information necessary to understand a meaningful action should be presented before the action is emphasized.

The interface should not prioritize action at the expense of understanding.

---

### Progressive Detail

Information should be presented in layers.

Primary information should remain immediately understandable while deeper supporting information can be explored when needed.

---

### Meaningful Emphasis

Visual and structural emphasis should reflect informational importance.

Information should not receive greater prominence simply because it is more visually attractive, more recent, or easier to display.

---

### Consistent Hierarchy

Similar types of information should receive similar levels of structural importance throughout the product.

Users should not have to relearn the meaning of hierarchy from one screen to another.

---

Screen hierarchy should make understanding easier by revealing importance, context, and detail in a deliberate order.

---

## Screen Levels

Screen levels describe the structural role a screen plays within the product.

The initial model consists of four conceptual levels:

### Primary Screens

Primary Screens represent major product areas and provide access to the most important information or activities within those areas.

They should correspond to meaningful product destinations rather than individual actions.

---

### Task Screens

Task Screens support a specific user goal or workflow.

They should provide the information and actions necessary to complete that goal without introducing unrelated complexity.

---

### Detail Screens

Detail Screens provide deeper information about a specific subject, entity, result, or piece of information.

They should allow users to investigate supporting context without overwhelming the primary experience.

---

### Supporting Screens

Supporting Screens provide secondary information or functions that assist the user's broader journey.

Examples may include:

- References
- History
- Supporting information
- Preferences
- Explanations

Supporting screens should remain subordinate to the user's primary goal.

---

## Content Hierarchy

Content Hierarchy defines how information should be prioritized within an individual screen.

The conceptual hierarchy is:

Primary Information

↓

Supporting Context

↓

Evidence and Explanation

↓

Additional Detail

↓

Optional Exploration

The hierarchy may change according to the user's context, but the most important information should remain identifiable.

---

### Primary Information

Primary Information answers the most important question:

> What does the user need to understand here?

It should establish the subject, purpose, or current state of the screen.

---

### Supporting Context

Supporting Context provides information necessary to interpret the primary information correctly.

It should prevent users from reaching conclusions without sufficient context.

---

### Evidence and Explanation

Evidence and Explanation provide support for the information being presented.

They should allow users to understand where important conclusions or insights come from.

---

### Additional Detail

Additional Detail provides deeper information for users who want to investigate further.

It should not obscure the primary understanding of the screen.

---

### Optional Exploration

Optional Exploration allows users to follow related information when deeper investigation is useful.

It should support curiosity without forcing additional complexity onto users who do not need it.

---

## Screen Relationships

Screens should be connected according to the relationships established by the Information Architecture and Navigation Model.

A conceptual relationship may follow:

Primary Screen

↓

Task Screen

↓

Detail Screen

↓

Supporting Information

Users should be able to move between related screens while maintaining enough context to understand how they arrived there and why the information is relevant.

Returning to a previous screen should preserve the user's broader journey whenever appropriate.

---

## Screen Priorities

Screen priorities determine how information should be emphasized according to the user's current context.

Priority should generally follow:

1. Understanding the current context.
2. Presenting the primary information.
3. Providing necessary supporting evidence or explanation.
4. Enabling the relevant action.
5. Providing deeper exploration.

This order may change when a specific user goal requires a different emphasis.

However, actions should not consistently receive greater prominence than the information required to make meaningful decisions.

---

## Screen Boundaries

The Screen Hierarchy does not define:

- Visual design.
- Typography.
- Color systems.
- Spacing systems.
- Component design.
- Button styles.
- Responsive layout specifications.
- Pixel-level positioning.
- Final screen designs.
- Wireframes.
- Prototypes.
- Technical implementation.

These decisions belong to later design and development documentation.

The Screen Hierarchy establishes the structural and informational foundation that those decisions should follow.

---

## Screen Hierarchy Statement

Screen Hierarchy defines how information is prioritized within the interface so that users can understand context, identify what matters, and explore deeper information when needed.

Screens should reveal information progressively while preserving clarity and context.

As Exactly evolves, new screen types and structures may be introduced.

The underlying hierarchy principles should remain stable so that every interface continues to prioritize understanding over unnecessary complexity and action over premature decision-making.
