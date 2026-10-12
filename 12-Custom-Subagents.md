# Claude Code Custom Subagents — Notes & Practical Workflow

## 1. What Are Custom Subagents?

Custom subagents are specialized AI agents designed for specific tasks within a project.

Each subagent can have its own:

- Context window
- System instructions
- Tool permissions
- Model configuration
- Skills and other supported settings

### Why Use Custom Subagents?

Built-in agents provide general-purpose capabilities, while custom agents follow project-specific instructions and workflows.

**Example:** A security review agent can follow your organization's security checklist rather than relying only on general security knowledge.

Common use cases:

- Test generation and execution
- Security audits
- Code quality reviews
- Documentation generation
- Project-specific validation

---

## 2. Create a Custom Subagent

A custom subagent is defined in a Markdown file with YAML frontmatter and a body containing its instructions.

### Project-Level Structure

```text
your-project/
├── .claude/
│   ├── agents/
│   │   ├── spendly-test-writer.md
│   │   ├── spendly-test-runner.md
│   │   ├── spendly-security-reviewer.md
│   │   └── spendly-quality-reviewer.md
│   └── commands/
│       ├── test-feature.md
│       └── code-review-feature.md
└── ...
```

Project-level agents are stored in `.claude/agents/`. Personal agents can be stored in the corresponding agents directory under your home-level Claude configuration.

### Basic Agent Definition

```markdown
---
name: test-writer
description: Writes tests for implemented features using their specifications.
tools: Read, Grep, Glob, Edit, Write
model: sonnet
---

# Instructions

1. Read the feature specification.
2. Identify expected behavior and edge cases.
3. Write tests based on the specification, not the implementation.
4. Follow the project's testing conventions.
5. Report the files created and any assumptions.
```

**Important:** Available frontmatter fields and tool names depend on the Claude Code version. Check the current documentation for supported options.

### Important Frontmatter Fields

| Field         | Purpose                                                                |
| ------------- | ---------------------------------------------------------------------- |
| `name`        | Agent identifier                                                       |
| `description` | Explains the agent's purpose and helps Claude decide when to invoke it |
| `tools`       | Controls available tools                                               |
| `model`       | Specifies the model                                                    |
| `skills`      | Provides access to relevant skills, when supported                     |
| `memory`      | Configures supported agent memory options                              |
| `color`       | Sets a visual identifier, when supported                               |

The Markdown body defines the agent's system instructions and workflow.

---

## 3. Ways to Create Agents

### Option A: Claude Code Interface

1. Run `/agents`.
2. Select **Create new agent**.
3. Choose project-level or personal scope.
4. Select AI-assisted generation or manual configuration.
5. Provide the agent's purpose and instructions.
6. Configure tools and other supported options.
7. Review and save the generated file.

### Option B: Create the File Manually

1. Create `.claude/agents/` if it does not exist.
2. Create a Markdown file for the agent.
3. Add YAML frontmatter.
4. Write detailed instructions in the body.
5. Review the file and verify the agent works as intended.

**Best practice:** Do not blindly trust AI-generated agent files. Review their instructions, tool permissions, and project-specific assumptions before using them.

---

## 4. How Custom Subagents Are Triggered

### Automatic Invocation

Claude may select an agent when its description matches the current task.

A clear description helps Claude understand:

- What the agent does
- When it should be used
- Which tasks it should handle

### Explicit Invocation

You can ask Claude to use a specific agent for a task.

Custom slash commands can also coordinate agents into repeatable workflows.

---

## 5. Practical Example: Four Specialized Agents

The tutorial adds date-range filtering to an expense tracker profile page and introduces testing and code-review stages before committing changes.

### Agent 1: Test Writer

**Purpose:** Write test cases for the implemented feature.

Workflow:

1. Read the feature specification.
2. Understand the expected behavior.
3. Identify happy paths, validation rules, edge cases, and authorization requirements.
4. Create tests following project conventions.
5. Report which tests were created.

**Key principle:** Derive expected behavior from the specification, not from the implementation. Otherwise, tests may reproduce the same mistakes as the code.

### Agent 2: Test Runner

**Purpose:** Execute tests created for the feature.

Workflow:

1. Verify that the expected test files exist.
2. Run the relevant tests using the project's established commands.
3. Summarize passed and failed tests.
4. Explain failure causes where possible.
5. Report warnings and recommended next steps.

**Principle of least privilege:** Give the test runner only the tools it needs. If it only executes and inspects tests, it generally should not receive write access.

### Agent 3: Security Reviewer

**Purpose:** Identify security risks introduced by the feature.

Review areas may include:

- SQL injection
- Authentication and authorization
- Input validation
- Exposed secrets or API keys
- Unsafe data handling
- Other project-specific security requirements

The reviewer should explain findings, their severity, and recommended fixes.

### Agent 4: Code Quality Reviewer

**Purpose:** Review the implementation for maintainability and correctness.

Review areas may include:

- Project conventions
- Readability and separation of concerns
- Error handling
- Edge cases
- Unnecessary complexity
- Potential regressions
- Appropriate use of existing abstractions

Keep quality suggestions distinct from actual bugs and security vulnerabilities.

---

## 6. Create a Custom Slash Command

A custom slash command lets you reuse a defined workflow without manually repeating the instructions.

For this tutorial, the command files are stored in `.claude/commands/`.

### Example: `/test-feature`

File: `.claude/commands/test-feature.md`

```markdown
---
description: Write and run tests for a specified feature.
---

Feature specification: $ARGUMENTS

Follow these steps in order:

1. Read the feature specification.
2. Invoke the test-writer agent to create tests.
3. Inspect the generated test files.
4. Invoke the test-runner agent to execute the tests.
5. Summarize the results in a table.

Include:

- Total tests
- Passed tests
- Failed tests
- Failure explanations
- Warnings
- Recommended next steps

Do not claim that a feature is correct merely because some tests pass.
```

Invoke the command with the feature specification path, for example:

```text
/test-feature specs/06-date-filter-profile.md
```

The command orchestrates the workflow, while the specialized agents perform their assigned tasks.

> **Version note:** Claude Code supports evolving agent and slash-command configuration features. Verify the current syntax and supported agent-invocation mechanisms for your installed version.

---

## 7. Create a Parallel Code-Review Workflow

Security review and code quality review are largely independent, so they can run in parallel.

### Example: `/code-review-feature`

File: `.claude/commands/code-review-feature.md`

```markdown
---
description: Review a feature for security and code quality.
---

Feature specification: $ARGUMENTS

1. Launch the security-reviewer and quality-reviewer agents
   in parallel when supported.
2. Ask both reviewers to inspect the relevant feature changes
   against the specification.
3. Collect and consolidate their findings.
4. Classify findings by severity and category.
5. Identify conflicts or duplicate findings.
6. Present a unified report.
7. Ask for approval before applying recommended changes.

The report must include:

- Security findings
- Code quality findings
- Severity and affected files
- Recommended fixes
- Final recommendation

Do not silently modify code during review.
```

The command describes the desired orchestration. Confirm that the syntax and agent-invocation behavior are supported by your installed Claude Code version.

---

## 8. Sequential vs. Parallel Execution

| Workflow                         | Execution                 | Why                                               |
| -------------------------------- | ------------------------- | ------------------------------------------------- |
| Test writing → test execution    | Sequential                | Tests should exist before they are run            |
| Security review + quality review | Parallel                  | The reviews are independent                       |
| Review findings → apply fixes    | Sequential, with approval | Findings should be assessed before modifying code |

Use sequential execution when one task depends on another. Use parallel execution when tasks are independent and can safely work at the same time.

---

## 9. Feature Development Workflow

The updated workflow adds testing and self-review before pushing code.

```text
Create Feature Branch
        ↓
Write Feature Specification
        ↓
Review Specification
        ↓
Create Implementation Plan
        ↓
Review Plan
        ↓
Implement Feature
        ↓
Run /test-feature
        ↓
Fix Failures and Retest
        ↓
Run /code-review-feature
        ↓
Review Findings and Approve Fixes
        ↓
Retest After Fixes
        ↓
Commit and Push
        ↓
Create Pull Request
        ↓
Review, Merge, and Clean Up
```

### Example Git Commands

```bash
git switch main
git pull origin main
git switch -c feature/date-filter-profile

# Implement and validate the feature first.

git add .
git commit -m "Add date filter to profile page"
git push -u origin feature/date-filter-profile
```

After merging the pull request:

```bash
git switch main
git pull origin main
git branch -d feature/date-filter-profile
```

Use appropriate staging and review practices instead of automatically adding every file when the repository may contain unrelated changes.

---

## 10. Important Lessons

- **Specialization:** Dedicated agents can follow focused instructions for individual tasks.
- **Clear descriptions:** Agent descriptions help Claude identify the right agent.
- **Least privilege:** Give each agent only the tools it needs.
- **Specification-driven testing:** Tests should validate expected behavior, not simply mirror existing code.
- **Independent reviews:** Security and quality reviews can run in parallel.
- **Human oversight:** Review generated agent definitions, test results, and code-review findings.
- **Iterative validation:** Fix failures, rerun tests, and validate again after review-driven changes.
- **Self-review is not a replacement for peer review:** Pull requests still benefit from an independent reviewer.

## Quick Revision Checklist

- [ ] Understand the purpose of custom subagents.
- [ ] Know the difference between project-level and personal agents.
- [ ] Understand agent YAML frontmatter and system instructions.
- [ ] Know how to create agents through `/agents` or Markdown files.
- [ ] Understand automatic and explicit agent invocation.
- [ ] Create a test writer and test runner.
- [ ] Coordinate agents using a custom slash command.
- [ ] Run independent code reviews in parallel.
- [ ] Review findings and validate fixes before committing.
- [ ] Push changes and complete the pull-request workflow.
