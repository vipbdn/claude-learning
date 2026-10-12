# Claude Code: Subagents — Concepts, Use Cases, and Built-in Agents

## 1. What Are Subagents?

Subagents are specialized AI assistants that run in their own isolated context windows, perform specific tasks, and return only relevant results to the main agent.

- **Main agent:** The primary Claude Code assistant you interact with.
- **Subagent:** A separate agent delegated a specific task by the main agent.
- **Context isolation:** The subagent uses its own context instead of loading all its work into the main conversation.
- **Result handoff:** The subagent returns a summary, findings, or other relevant output to the main agent.

### Basic Workflow

1. The main agent identifies a task that can be delegated.
2. It starts a subagent with the required instructions and context.
3. The subagent performs its task in an isolated context window.
4. It returns the relevant results to the main agent.
5. The main agent continues working with those results.

**Mental model:** A subagent works like a function. You provide input, it processes the input, and it returns an output without requiring you to manage its internal implementation.

## 2. Why Do We Need Subagents?

### Problem 1: Growing Context Windows

LLMs are stateless by nature. Applications maintain conversational context by sending relevant conversation history with subsequent requests.

In coding agents, the context may include:

- Existing source code and project structure.
- Previous user instructions.
- Earlier plans and responses.
- Code generated during previous turns.
- Tool results and other findings.

If a large codebase is repeatedly included in the conversation, token usage can grow quickly.

For example, when adding authentication to an existing application, the agent may need to understand the database, APIs, routes, middleware, and existing architecture. Repeatedly carrying all the codebase context through every subsequent turn can waste tokens.

### Problem 2: Lost-in-the-Middle Effect

When information is presented in a long context, models may not use all parts of it equally effectively. Relevant information in the middle of a large context can be overlooked.

A large context can therefore affect both efficiency and answer quality.

### How Subagents Help

Instead of making the main agent carry out an entire codebase analysis in its own context:

1. Delegate codebase exploration to a subagent.
2. Let the subagent inspect the necessary files.
3. Return only a concise summary or implementation plan.
4. Continue the main conversation using that result.

This can reduce repeated context, token consumption, and cost while keeping the main conversation focused.

_Note: Subagents do not eliminate context limits or guarantee lower costs. Their benefit depends on the task, the amount of information returned, and how the agents are configured._

## 3. Main Advantages of Subagents

### 3.1 Context Isolation

Each subagent has its own context window for its assigned work.

Useful for:

- Exploring large codebases.
- Analyzing multiple files.
- Researching unfamiliar implementations.
- Generating implementation plans.

### 3.2 Specialization

Subagents can be configured for specific responsibilities.

Examples:

- Research agent.
- Code implementation agent.
- Security audit agent.
- Code review agent.
- Testing agent.

Each agent can have its own instructions, tools, model, and permissions.

### 3.3 Modularity

A software development workflow can be divided into separate responsibilities.

Example:

1. Analyze the codebase.
2. Create an implementation plan.
3. Implement the feature.
4. Review the code.
5. Run tests.

Each stage can be assigned to an appropriate subagent when useful.

### 3.4 Parallel Execution

Independent tasks can run simultaneously instead of sequentially.

Example: If three unrelated datasets require exploratory data analysis, three subagents can analyze them independently.

Parallel execution can save time, provided the tasks do not depend on one another and resources are available.

**Important:** Avoid having multiple subagents modify the same files simultaneously. This can cause conflicts and make integration harder.

## 4. Common Use Cases

| Use case              | Why use a subagent?                                                            |
| --------------------- | ------------------------------------------------------------------------------ |
| Codebase exploration  | Analyze existing code without loading all findings into the main context.      |
| Code review           | Obtain an independent review from an agent that did not implement the feature. |
| Testing               | Generate or run tests independently of the implementation work.                |
| Multi-stage pipelines | Pass the output of one stage to the next.                                      |
| Parallel development  | Work on independent features or services simultaneously.                       |
| Security audits       | Look specifically for vulnerabilities and security weaknesses.                 |

### Example: Multi-stage Development

For developing an API:

1. **Planning agent:** Defines the API contract and implementation plan.
2. **Implementation agent:** Writes the API code.
3. **Testing agent:** Tests the implementation and reports issues.

The output from each stage becomes input for the next stage.

### Why Use a Separate Code Reviewer?

An agent that implemented a feature may be biased toward its own design decisions and assumptions. An independent reviewer can provide a different perspective and identify problems the implementation agent overlooked.

A separate reviewer improves the review process, but it does not guarantee that every bug will be found.

## 5. Types of Subagents in Claude Code

There are two broad categories.

### 5.1 Built-in Subagents

These are provided by Claude Code and do not need to be created manually.

| Agent               | Purpose                                                                                             |
| ------------------- | --------------------------------------------------------------------------------------------------- |
| **Explore**         | Searches and analyzes the codebase, then reports relevant findings.                                 |
| **Plan**            | Helps analyze a feature and develop an implementation plan, particularly during planning workflows. |
| **General-purpose** | Handles broader tasks, including reading and modifying code when permitted.                         |

Claude Code can select built-in subagents automatically when it determines that delegation is appropriate.

### 5.2 Custom Subagents

Custom subagents are created to meet the requirements of a particular project or workflow.

Two scopes are discussed in the video:

- **User-level agents:** Available across projects for the same user.
- **Project-level agents:** Defined within a project and intended for that project.

Typical locations:

```text
User-level:
~/.claude/agents/

Project-level:
.claude/agents/
```

These locations are for custom subagent definitions.

## 6. Configuring Custom Subagents

When creating a custom subagent, configure the following:

| Configuration       | Purpose                                                                              |
| ------------------- | ------------------------------------------------------------------------------------ |
| Name                | Identifies the agent.                                                                |
| Description         | Explains its responsibility and when to use it.                                      |
| System instructions | Defines how the agent should perform its task.                                       |
| Tools               | Specifies which tools the agent can access.                                          |
| Model               | Selects the model used by the agent, where supported.                                |
| Permissions         | Controls what actions the agent is allowed to perform.                               |
| Skills              | Provides access to relevant skills, where supported.                                 |
| Hooks               | Allows supported actions or events to trigger additional behavior, where configured. |

### Example: Security Reviewer

A security reviewer could be instructed to inspect code for:

- Authentication and authorization bypasses.
- Injection vulnerabilities.
- Sensitive data exposure.
- Insecure handling of user input.

It should receive only the tools and permissions necessary for its work.

### Example: Research Agent

A research agent could be configured to:

- Inspect project files.
- Search for relevant code.
- Use web research tools when available and permitted.
- Return a concise summary with supporting findings.

The goal is to give each agent a clear responsibility and the capabilities required to perform it.

## 7. How Are Subagents Triggered?

There are two main approaches.

### Implicit Delegation

Claude Code decides that a task would benefit from a subagent and delegates it automatically.

Example prompt:

```text
Explore the codebase and explain the existing architecture,
implemented features, and features that are still missing.
```

Claude Code may use the Explore subagent for this task.

### Explicit Delegation

The user directly asks Claude Code to delegate a task to a subagent.

For example:

```text
Use a separate subagent to review the authentication
middleware for security vulnerabilities.
```

The exact agent selection and behavior depend on the request, available agents, and configuration.

## 8. Practical Demonstration: Built-in Subagents

The video uses an expense-tracking application with registration, login, and a profile page.

The profile page initially contains dummy transaction data. The goal is to connect the page to the backend so that the logged-in user sees real database data.

### 8.1 Observing Agent Activity

The instructor uses an observability tool called Agents Observe to view agent activity in real time.

The dashboard helps show:

- Which agents are running.
- Which subagent type is being used.
- What work the agents are performing.
- When a subagent finishes and returns control to the main agent.

The tool is used for demonstration purposes; it is not required for subagents themselves.

### 8.2 Explore Subagent

Prompt:

```text
Explore the codebase and explain what the project is about.
Also identify which features have already been developed
and which features are yet to be developed.
```

The Explore subagent reads and analyzes relevant project files, then returns its findings to the main agent.

**Key observation:** The main agent receives the findings without needing to carry the entire exploration process through the main conversation.

### 8.3 Plan Subagent

The next step is to prepare a specification for the profile page's backend routes.

Workflow:

1. Create a feature specification.
2. Ask Claude Code to read the specification.
3. Request an implementation plan.
4. Let Claude Code analyze the existing project before creating the plan.
5. Review the resulting plan before implementation.

The Plan subagent handles planning work when Claude Code delegates it.

### 8.4 Parallel General-purpose Subagents

The demonstration divides the profile page into three areas:

1. Summary statistics.
2. First transaction table.
3. Second transaction table.

Claude Code launches three general-purpose subagents to work on these areas concurrently.

Once they finish, the main agent integrates their work.

**Important lesson:** This is a demonstration, not necessarily the recommended architecture. All three agents modify the same page, which increases the risk of conflicting changes. Parallel work is generally better when tasks are independent and have clearly separated files or responsibilities.

## 9. Connecting the Profile Page to Real Data

The implementation replaces the profile page's dummy data with data fetched from the backend for the logged-in user.

The instructor then uses the existing `/seed-expense` custom slash command to create test data.

Workflow:

1. Identify the relevant user ID in the database.
2. Seed several expenses for that user.
3. Refresh the profile page.
4. Verify that the transaction records and summary totals reflect the database data.
5. Add more expenses and confirm that the page updates accordingly.

This validates that the frontend is displaying real user-specific data rather than static placeholder values.

## 10. Git Workflow Used in the Demonstration

After completing the feature, the instructor follows the team's Git workflow.

```bash
# Stage changes
git add .

# Commit changes
git commit -m "Add dynamic routes to profile page"

# Push the feature branch
git push origin <branch-name>
```

Then:

1. Create a pull request.
2. Merge the pull request.
3. Delete the feature branch from the remote repository.
4. Switch back to the main branch.
5. Pull the latest changes.
6. Delete the local feature branch.

```bash
git checkout main
git pull origin main
git branch -d <branch-name>
```

Replace `<branch-name>` with the actual feature branch name.

## 11. Revision Checklist

- [ ] Explain why repeatedly carrying large codebase context is inefficient.
- [ ] Define a subagent and explain context isolation.
- [ ] Describe how a subagent returns results to the main agent.
- [ ] Explain the benefits of specialization and modularity.
- [ ] Understand when parallel execution is useful.
- [ ] Identify the Explore, Plan, and general-purpose built-in subagents.
- [ ] Distinguish built-in subagents from custom subagents.
- [ ] Understand user-level and project-level custom agents.
- [ ] List the main configuration options for a custom subagent.
- [ ] Distinguish implicit delegation from explicit delegation.
- [ ] Explain why independent tasks are better candidates for parallel execution.
- [ ] Describe how the profile page was connected to real backend data.

## Key Takeaway

**Subagents let Claude Code delegate specialized work into isolated contexts and return only the results needed by the main agent.** They are especially useful for codebase exploration, independent code reviews, testing, multi-stage workflows, and parallel execution. Good task boundaries, clear instructions, appropriate permissions, and careful integration are essential for using them effectively.
