# Spec-Driven Development (SDD)

## 1. What Is Vibe Coding?

Vibe coding is an AI-assisted programming approach where developers describe what they want in natural language and let an AI coding tool generate the code.

### Typical workflow

1. Give the AI a rough idea or prompt.
2. Let the AI generate the application or feature.
3. Review the result.
4. Ask the AI to fix issues or change the implementation.
5. Repeat until the result looks correct.

### Problems with vibe coding

- **Loss of control:** The AI makes important technical and product decisions independently.
- **Ambiguity:** Requirements may be incomplete or unclear.
- **Unpredictable results:** The generated code may not match expectations.
- **Correction loops:** Developers repeatedly request changes and patches.
- **Code complexity:** Large amounts of generated code can be difficult to understand and debug.

**Key takeaway:** Code can be generated quickly without necessarily generating the right code.

Vibe coding is useful for prototypes, experimentation, and small side projects.

---

## 2. What Is Spec-Driven Development?

Spec-Driven Development (SDD) is a software development approach in which a detailed specification is created and reviewed before implementation begins.

The specification acts as the **single source of truth** for what the system should do.

Instead of asking an AI to build a feature from a vague prompt, we provide clear requirements, expected behavior, constraints, edge cases, and acceptance criteria.

### Core principle

**Define what to build before deciding how to build it.**

The developer retains control over requirements and important decisions, while the AI assists with planning and implementation.

---

## 3. What Should a Specification Document Contain?

An effective specification generally includes these sections:

| Section                 | Purpose                                                                    |
| ----------------------- | -------------------------------------------------------------------------- |
| Problem Statement       | Explains why the feature is needed.                                        |
| Functional Requirements | Defines exactly what the feature must do.                                  |
| Input/Output Behavior   | Describes inputs and expected system responses.                            |
| Constraints             | Defines performance, compatibility, and other limitations.                 |
| Edge Cases              | Identifies unusual situations and how to handle them.                      |
| Acceptance Criteria     | Defines the conditions required for the feature to be considered complete. |

### Example: Chat History Sidebar

**1. Problem Statement**

Users create multiple conversations over time but have no convenient way to revisit previous chats. This makes it difficult to find discussions and continue earlier conversations.

**Solution:** Introduce a chat history sidebar.

**2. Functional Requirements**

- Display a list of previous conversations.
- Show a short, readable title for each conversation.
- Generate titles automatically from the user's first message.
- Allow users to select a conversation and reopen it.

**3. Input/Output Behavior**

- **Input:** The user clicks a conversation in the sidebar.
- **Expected output:** The selected conversation loads in the main chat area.

**4. Constraints**

- Load the sidebar within one second under the defined performance conditions.
- Display correctly on standard laptop screens.
- Keep conversation titles short and readable.
- Handle a reasonable number of conversations smoothly.

**5. Edge Cases**

- No previous conversations: display `No chat history yet`.
- A conversation cannot be loaded: display an appropriate error message.
- The first message is too long: use only an appropriate initial portion to generate the title.

**6. Acceptance Criteria**

- Users can see their previous conversations.
- Selecting a conversation displays the correct chat.
- New conversations appear in the history after the first use.
- Empty and failed-loading states are handled correctly.

---

## 4. Specification vs. Technical Design Plan

These are separate documents with different responsibilities.

### Specification Document — What and Why

Describes the product requirements without tying them unnecessarily to a specific technology stack.

It focuses on:

- The problem being solved.
- Required functionality.
- Expected behavior.
- Constraints and acceptance criteria.

### Technical Design Plan — How

Explains how engineers will implement the specification.

It typically includes:

- Technology stack.
- High-level architecture.
- Database schema and data models.
- API endpoints.
- Core technical decisions.
- Functional implementation flows.
- Development and integration strategy.

### Example

For the chat history sidebar:

- **Specification:** Users must be able to view previous conversations and reopen a selected chat.
- **Technical design:** The frontend calls a backend API, the backend retrieves conversations from a relational database, and the UI displays the returned data.

### Why keep the documents separate?

A feature's requirements should remain stable even if the technology stack changes.

For example, the same specification could be implemented using different frontend frameworks or backend technologies. Each implementation would have its own technical design plan.

**Remember:**

- Specification = What and Why.
- Technical Design Plan = How.

---

## 5. The Complete SDD Workflow

The standard workflow consists of these stages:

1. **Create the specification** — Define the problem, requirements, behavior, constraints, edge cases, and acceptance criteria.
2. **Review the specification** — Check for missing requirements, ambiguity, and incorrect assumptions.
3. **Create the technical design plan** — Translate the requirements into an implementation strategy.
4. **Review the technical design** — Check architecture, technology choices, data models, APIs, and implementation details.
5. **Break the design into tasks** — Create small, actionable development tasks.
6. **Implement the tasks** — Write the code using a single AI agent or multiple subagents.
7. **Validate the implementation** — Check the result against the original specification and its acceptance criteria.

### Example task breakdown

For the chat history feature:

1. Database and data models.
2. Backend APIs.
3. Frontend UI components.
4. Frontend/backend integration.
5. Testing and validation.

These tasks can be assigned to developers or managed through project-tracking tools such as Jira.

---

## 6. Vibe Coding vs. Spec-Driven Development

| Aspect                | Vibe Coding                               | Spec-Driven Development                                               |
| --------------------- | ----------------------------------------- | --------------------------------------------------------------------- |
| Starting point        | Rough idea or prompt                      | Reviewed specification                                                |
| Requirements          | May be interpreted by AI                  | Explicitly defined by developer/product team                          |
| Control               | More decisions delegated to AI            | More control retained by the developer                                |
| Initial effort        | Low                                       | Higher due to planning and reviews                                    |
| Implementation speed  | Often faster to start                     | More preparation before coding                                        |
| Output quality        | Can be unpredictable                      | More consistent and traceable when followed properly                  |
| Debugging             | Prompt AI to fix observed issues          | Compare implementation with requirements and acceptance criteria      |
| Common failure        | Unclear or excessive generated code       | Overengineering or an incomplete specification                        |
| Best suited for       | Prototypes and experimentation            | Complex features and production-oriented systems                      |
| Programming knowledge | Can be used with limited coding knowledge | Requires enough technical knowledge to define and review requirements |

SDD does not automatically guarantee correct or high-quality code. Its effectiveness depends on the quality of the specification, technical design, implementation, and validation.

---

## 7. How SDD Fits into Agentic Coding

Agentic coding is a broader approach to AI-assisted software development. It can combine specification-driven planning
