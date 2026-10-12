# Claude Code: Database Setup, Plan Mode & Planning Best Practices

## 1. Lesson Overview

In this lesson, we build the database foundation for the **Spendly Expense Tracker** using Spec-Driven Development (SDD) and Claude Code's Plan Mode.

### What We Will Build

- A SQLite database with `users` and `expenses` tables.
- Database connection, initialization, and sample-data functions.
- Integration with the Flask application.
- A feature branch, pull request, and GitHub merge workflow.

### Concepts Covered

- Implementing a feature using Spec-Driven Development.
- Using Plan Mode before writing code.
- Improving plans with model selection, Extended Thinking, and effort levels.
- Understanding Ultra Plan for complex planning tasks.

---

## 2. Project Workflow

Follow this workflow when implementing a feature:

1. Pull the latest code from GitHub.
2. Create a feature branch.
3. Write and review the feature specification.
4. Save the specification in `.claude/specs/`.
5. Generate and review the technical implementation plan.
6. Implement the approved plan.
7. Validate the implementation against the specification.
8. Commit and push the changes.
9. Create and merge a pull request.
10. Update the local `main` branch and delete the feature branch.

**Key principle:** Plan first, implement second, validate before merging.

---

## 3. Database Setup Using Spec-Driven Development

### Step 1: Create a Feature Branch

```bash
git pull origin main
git switch -c feature/database-setup
```

Use descriptive feature branch names, such as `feature/database-setup`.

### Step 2: Write the Specification

Create a specification file:

```text
.claude/
└── specs/
    └── database-setup.md
```

The specification defines what the database must do before implementation begins.

### Step 3: Define the Database Schema

#### `users` table

Stores user account information.

| Column          | Purpose                    |
| --------------- | -------------------------- |
| `id`            | Unique user identifier     |
| `name`          | User's name                |
| `email`         | User's email address       |
| `password_hash` | Hashed password            |
| `created_at`    | Account creation timestamp |

#### `expenses` table

Stores expenses associated with users.

| Column        | Purpose                        |
| ------------- | ------------------------------ |
| `id`          | Unique expense identifier      |
| `user_id`     | References the associated user |
| `amount`      | Expense amount                 |
| `category`    | Expense category               |
| `date`        | Expense date                   |
| `description` | Expense description            |
| `created_at`  | Record creation timestamp      |

The specification should also define column types, constraints, foreign keys, date formats, and error-handling behavior.

### Step 4: Define Implementation Requirements

The specification includes these requirements:

- Use SQLite without an ORM such as SQLAlchemy.
- Use parameterized SQL queries.
- Create tables only when they do not already exist.
- Enforce foreign-key relationships.
- Insert sample data for development and testing.
- Avoid duplicate sample data when initialization runs repeatedly.
- Define valid expense categories.
- Specify acceptance criteria for verifying the feature.

### Step 5: Define Database Functions

Implement these functions in `db.py`:

| Function    | Responsibility                                     |
| ----------- | -------------------------------------------------- |
| `get_db()`  | Create and return a database connection.           |
| `init_db()` | Initialize the database and create missing tables. |
| `seed_db()` | Insert sample users and expenses for testing.      |

Update `app.py` to import and use the necessary database functions.

---

## 4. Claude Code Plan Mode

### What Is Plan Mode?

Plan Mode lets Claude Code explore the existing codebase and prepare an implementation plan before making code changes.

During planning, Claude can inspect relevant files and understand the current architecture. It does not perform file-writing operations as part of the planning phase.

### How to Enter Plan Mode

Common methods shown in the lesson:

- Press `Shift + Tab` twice.
- Enter `/plan`.

_Keyboard shortcuts and command availability can vary by Claude Code version._

### Example Planning Prompt

```text
Read the specification at:
.claude/specs/database-setup.md

Explore the relevant existing files and understand
the current project structure.

Create a detailed implementation plan that follows
the specification and fits the existing codebase.

Save the plan in the appropriate plans directory.
```

### What Claude Should Include in the Plan

- Files that need to be created or modified.
- Database schema and SQL queries.
- Responsibilities of each database function.
- Required changes to `app.py`.
- Error handling and validation.
- Testing and verification steps.

### Review Before Implementation

Before approving a plan:

- Confirm it covers all requirements in the specification.
- Check that it fits the existing project architecture.
- Look for missing edge cases or unnecessary changes.
- Confirm the planned tests cover the acceptance criteria.

**Important:** A plan is a proposal, not proof of correctness. Review it before approving implementation.

---

## 5. Implement and Validate the Database

After approving the plan, let Claude Code implement the changes. Review the edits and inspect the resulting files.

### Validation Checklist

- [ ] The database file is created successfully.
- [ ] Both `users` and `expenses` tables exist.
- [ ] The schema matches the specification.
- [ ] Sample users and expenses are inserted.
- [ ] All required expense categories are represented.
- [ ] Repeated initialization does not create duplicate sample records.
- [ ] Foreign-key enforcement works.
- [ ] SQL queries use parameterized inputs.
- [ ] The Flask application starts without errors.
- [ ] Relevant tests pass.

Example command to run the application:

```bash
python app.py
```

Run the project's applicable tests as well. Do not rely only on Claude's claim that all checks passed; inspect the implementation and verify the acceptance criteria yourself.

---

## 6. Complete the GitHub Workflow

Once implementation and validation are complete:

### Commit and Push

```bash
git add .
git commit -m "Create database setup"
git push -u origin feature/database-setup
```

### Create and Merge a Pull Request

1. Open GitHub and create a pull request from `feature/database-setup` into `main`.
2. Review the changes and check for conflicts.
3. Merge the pull request after the review is satisfactory.
4. Delete the remote feature branch if it is no longer needed.

### Synchronize the Local Repository

```bash
git switch main
git pull origin main
git branch -d feature/database-setup
```

This ensures your local `main` contains the merged feature before you continue development.

---

## 7. Plan Mode: Improving Plan Quality

### A. Model Selection

Choose a model appropriate to the complexity of the task.

- **Sonnet:** Suitable for many routine development and planning tasks.
- **Opus:** Consider for complex architecture decisions, cross-file changes, large refactors, and tasks requiring deeper analysis.

A stronger model may produce a more thorough plan, but it can also consume more tokens and cost more. Select the model based on the task, not simply because it is the most powerful.

Use `/model` to select an available model.

### B. Extended Thinking

Extended Thinking allows the model to allocate additional reasoning effort before producing its response.

Think of it as working through a difficult problem on a whiteboard before presenting a solution.

It can be helpful for:

- Complex planning tasks.
- Multi-step reasoning.
- Architecture and design decisions.
- Identifying edge cases before implementation.

The lesson demonstrates managing thinking settings through `/config`.

Extended thinking can increase token usage and latency. Enable it when the task benefits from deeper reasoning, and check the available settings in your installed version.

### C. Effort Level

Effort level controls how much reasoning effort the model applies to a task.

The lesson describes these levels:

| Level  | Typical use                             |
| ------ | --------------------------------------- |
| Low    | Simple tasks that need limited analysis |
| Medium | Routine development and planning        |
| High   | More complex planning and reasoning     |
| Max    | Very demanding tasks, when supported    |

The lesson also mentions `Auto`, which allows the tool to choose an appropriate effort level.

Use `/effort` to access the available options. Medium or high can be a reasonable starting point for planning, while automatic selection can work well for general tasks.

**Remember:** More reasoning effort does not guarantee a correct plan, and greater effort may increase token usage.

---

## 8. Understanding Ultra Plan

Ultra Plan is presented in the lesson as a remote planning workflow for demanding tasks.

### How It Works

1. Trigger `/ultraplan`.
2. Describe the feature or provide a specification.
3. Claude Code starts a remote planning session.
4. The remote environment explores the task and develops a plan.
5. Review the generated plan in the web interface.
6. Approve the plan and either continue coding remotely or return the plan to your local terminal.
7. Implement the approved plan in the chosen environment.

### When to Use It

Consider Ultra Plan when:

- A regular Plan Mode result is not detailed enough.
- The task involves many files or complex dependencies.
- A major refactor or architectural decision is required.

For straightforward features, regular Plan Mode is generally sufficient. Ultra Plan may involve additional cost and should be used when its extra planning capabilities justify that cost.

_Ultra Plan availability, model options, and workflow may change between Claude Code versions._

---

## 9. Quick Comparison

| Feature              | Regular Plan Mode                       | Ultra Plan                                    |
| -------------------- | --------------------------------------- | --------------------------------------------- |
| Planning environment | Local workflow                          | Remote planning workflow                      |
| Main purpose         | Plan before implementation              | More extensive planning for demanding tasks   |
| Best suited for      | Routine and moderately complex features | Complex features and architectural work       |
| Implementation       | Approve the plan and implement          | Implement remotely or return the plan locally |
| Cost consideration   | Depends on model and reasoning settings | May involve additional usage and cost         |

---

## 10. Revision Checklist

Before implementing your next feature, make sure you can explain:

- [ ] Why feature development starts with a specification.
- [ ] The difference between a specification and a technical implementation plan.
- [ ] How to enter Plan Mode.
- [ ] Why the existing codebase must be inspected before planning.
- [ ] Why plans and implementation should be reviewed separately.
- [ ] How to validate code against acceptance criteria.
- [ ] Why parameterized SQL queries are important.
- [ ] Why database initialization should be safe to repeat.
- [ ] How model selection, Extended Thinking, and effort level affect planning.
- [ ] When Ultra Plan is worth considering.
- [ ] How to complete the feature branch, pull request, merge, and cleanup workflow.

**Core takeaway:** Use a reviewed specification to define the requirements, Plan Mode to design the implementation, tests and manual checks to validate it, and GitHub pull requests to integrate the finished feature safely.
