# Claude Code: Custom Slash Commands & Authentication Features

## 1. Custom Slash Commands

Custom slash commands are **saved prompts that automate repetitive development workflows** in Claude Code. Instead of repeatedly writing the same instructions, create a Markdown file and invoke it using `/command-name`.

### Why use custom commands?

- Automate repetitive tasks.
- Standardize development workflows.
- Reduce repeated prompting.
- Provide consistent, project-specific instructions.
- Make complex workflows easier to execute.

### Common examples

| Command          | Purpose                                                    |
| ---------------- | ---------------------------------------------------------- |
| `/review`        | Review recently created or modified code.                  |
| `/commit`        | Analyze changes and suggest a suitable Git commit message. |
| `/test`          | Run the project's test suite.                              |
| `/security-scan` | Check the codebase for potential vulnerabilities.          |
| `/seed-user`     | Create a dummy user in the database.                       |
| `/seed-expense`  | Generate sample expense records.                           |
| `/create-spec`   | Generate a feature specification document.                 |

## 2. Project-Scoped vs User-Scoped Commands

| Type           | Location              | Availability       |
| -------------- | --------------------- | ------------------ |
| Project-scoped | `.claude/commands/`   | Within the project |
| User-scoped    | `~/.claude/commands/` | Across projects    |

A command's Markdown filename determines its command name. For example, `seed-user.md` becomes `/seed-user`.

> **Note:** Claude Code's command and skill conventions can change between versions. Check the current documentation if your setup uses a newer format.

### Example directory structure

```text
spendly/
├── .claude/
│   └── commands/
│       ├── seed-user.md
│       ├── seed-expense.md
│       └── create-spec.md
├── app.py
├── db.py
└── database.py
```

## 3. Writing a Custom Slash Command

A command file should explain the task, required inputs, permitted tools, constraints, and expected output.

### Example: `/seed-user`

**Goal:** Create one realistic dummy user in the development database.

Instructions should tell Claude to:

1. Read the database code to understand the user table and its schema.
2. Generate realistic user data, such as a name and email.
3. Generate a password and hash it using the application's existing password-hashing approach.
4. Check whether the email already exists.
5. Generate another unique email if necessary.
6. Insert the user using the project's existing database helper.
7. Display the result after insertion.
8. Request approval before executing any operation that requires confirmation.

### Restricting tool access

Commands may include tool permissions or execution restrictions, depending on the supported command format and Claude Code version.

For example, a database-seeding workflow might only need file-reading access and permission to execute specific Python commands.

**Principle:** Grant only the permissions required for the task. Avoid broad command execution permissions when narrower restrictions are possible.

## 4. Passing Arguments to Custom Commands

Some commands need user-provided inputs rather than fixed instructions.

For example, `/seed-expense` can accept:

- `user_id`: The user who owns the expenses.
- `count`: The number of expense records to create.
- `months`: The historical period over which to distribute expenses.

Example invocation:

```text
/seed-expense 2 5 3
```

This requests five sample expenses for user ID `2`, distributed across the previous three months.

The command should:

1. Parse the arguments.
2. Validate that all required inputs are present and valid.
3. Verify that the specified user exists.
4. Generate realistic expense descriptions, categories, amounts, and dates.
5. Insert the records using the existing database layer.
6. Report the number of inserted records and date range.

**Key lesson:** Make inputs, validation rules, expected behavior, and failure cases explicit in the command instructions.

## 5. Automating Feature Specifications with `/create-spec`

Instead of manually writing a specification for every feature, create a custom command that generates one from a feature description.

Example invocation:

```text
/create-spec 2 registration
```

Here, `2` is the feature number and `registration` is the feature name.

### Responsibilities of the command

1. Check Git status before starting.
2. Parse the feature number, title, and URL-friendly slug.
3. Ask for clarification if required information is missing or ambiguous.
4. Read the project instructions, application entry point, database code, and existing specifications.
5. Understand the current architecture and previously implemented features.
6. Generate the specification using a consistent structure.
7. Save it in the project's specification directory.
8. Report the resulting file path.

### Recommended specification structure

A feature specification should contain:

- **Title:** Feature name.
- **Overview:** What the feature does and why it is needed.
- **Dependencies:** Existing features or components it relies on.
- **Routes:** Routes and their expected behavior.
- **Database changes:** Required schema or query changes.
- **UI templates:** Templates to create or modify.
- **Files affected:** Existing and new files.
- **Dependencies:** Any new packages required.
- **Implementation rules:** Constraints and coding requirements.
- **Acceptance criteria:** Verifiable conditions for completion.

Example path:

```text
.claude/specs/02-registration.md
```

The precise filename convention is project-defined; keep it consistent.

## 6. Spec vs Technical Implementation Plan

A specification defines **what** the feature must do and **why** it is needed. A technical implementation plan defines **how** it will be built.

### Workflow

1. Describe the feature.
2. Generate the specification.
3. Review and correct the specification.
4. Enter Claude Code Plan Mode.
5. Ask Claude to read the specification and produce a detailed implementation plan.
6. Review the plan before approving implementation.
7. Implement the changes.
8. Test the result against the acceptance criteria.

Example Plan Mode prompt:

```text
Read .claude/specs/02-registration.md and create a detailed
implementation plan for this feature.

Do not write any code.
```

**Important:** Never treat a generated specification or plan as automatically correct. Review assumptions, security requirements, affected files, and acceptance criteria before implementation.

## 7. Example: User Registration

The registration feature turns an existing placeholder route into a working registration flow.

### Expected behavior

1. Visiting `/register` displays the registration form.
2. Submitting valid information creates a new user.
3. The server validates all required fields.
4. Password and confirmation fields must match.
5. Duplicate email addresses are rejected.
6. Passwords are securely hashed before storage.
7. Database queries use parameterized statements.
8. Successful registration redirects the user to the login page.
9. Errors are displayed clearly without creating invalid or duplicate users.

### Likely files affected

- `app.py`: Registration route and request handling.
- `db.py`: Database helper functions.
- `templates/register.html`: Registration form and error messages.

The actual files depend on the project's architecture.

### Security reminders

- Hash passwords; never store plain-text passwords.
- Use parameterized SQL queries.
- Validate user input on the server, even when HTML validation exists.
- Configure the application's secret key securely.
- Avoid exposing sensitive information in error messages.
- Follow the project's existing database conventions.

## 8. Example: Login and Logout

### Login behavior

1. Visiting `/login` displays the login form.
2. Valid credentials authenticate the user.
3. The application stores the authenticated user's identity in a session.
4. Successful login redirects to the intended destination.
5. Invalid credentials display a generic error message.

### Logout behavior

1. Visiting `/logout` clears the authenticated session.
2. The user is redirected to an appropriate public page.

### Additional authentication checks

- An authenticated user should not see the login or registration forms again unless the product explicitly requires it.
- Unauthenticated users should not access protected pages.
- Incorrect passwords and unregistered email addresses should not reveal unnecessary account information.
- Session handling must follow the framework's security practices.

These requirements should be included in the specification **before implementation**, rather than discovered only during manual testing.

## 9. Validate Against Acceptance Criteria

After implementation, run the application and verify each criterion individually.

### Registration checklist

- [ ] `/register` displays the registration form.
- [ ] Valid input creates a database user.
- [ ] Successful registration redirects correctly.
- [ ] Mismatched passwords show an error.
- [ ] Duplicate emails are rejected.
- [ ] Empty fields are rejected.
- [ ] Passwords are stored as hashes.
- [ ] Repeated submissions do not create duplicate users.

### Login/logout checklist

- [ ] `/login` displays the login form when logged out.
- [ ] Valid credentials authenticate the user.
- [ ] Invalid credentials show a suitable error.
- [ ] Successful login redirects correctly.
- [ ] Logout clears the session.
- [ ] Login and registration pages redirect authenticated users appropriately.
- [ ] Protected pages enforce authentication.

If a test reveals missing behavior, update the specification or implementation as appropriate, then retest. **A successful coding run is not proof that the feature meets its requirements.**

## 10. Automating Git Branch Creation

The `/create-spec` command can also orchestrate feature-branch setup.

A safer workflow is:

1. Check `git status`.
2. Stop if unexpected uncommitted changes exist.
3. Validate the requested feature name.
4. Check whether the branch name already exists.
5. Switch to the main branch.
6. Pull the latest changes from the remote.
7. Create and switch to the new feature branch.
8. Generate and save the specification.

Example manual commands:

```bash
git status
git switch main
git pull origin main
git switch -c feature/registration
```

Do not discard or overwrite uncommitted work merely to make the workflow proceed. Review and preserve existing changes first.

## 11. End-to-End Feature Development Workflow

```text
Check Git status
       ↓
Create/switch to feature branch
       ↓
Generate specification
       ↓
Review specification
       ↓
Generate technical plan
       ↓
Review and approve plan
       ↓
Implement feature
       ↓
Test against acceptance criteria
       ↓
Fix issues and retest
       ↓
Commit and push changes
       ↓
Open pull request
       ↓
Review and merge
       ↓
Return to main and pull latest changes
       ↓
Delete the feature branch
```

### Useful Git commands

```bash
# Check working tree
git status

# Create a feature branch
git switch -c feature/registration

# Stage and commit changes
git add .
git commit -m "Add registration feature"

# Push the branch
git push -u origin feature/registration

# After the PR is merged
git switch main
git pull origin main

# Delete the local feature branch
git branch -d feature/registration
```

Open a pull request and review it before merging. In a team, this is where another developer or tech lead can identify issues that the author may have missed.

## 12. Key Takeaways

- Custom slash commands turn repeatable prompts into reusable workflows.
- Project-scoped commands are useful for project-specific conventions; user-scoped commands are useful for workflows shared across projects.
- Detailed instructions produce more predictable results than vague prompts.
- Argument-based commands support flexible tasks such as generating seed data.
- Specifications establish the requirements; technical plans translate them into implementation steps.
- Review generated
