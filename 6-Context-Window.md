# Claude Code — CLAUDE.md, `.claude/` & Auto Memory

## 1. Why CLAUDE.md Is Needed

LLMs do not automatically remember all previous conversations across separate sessions. Claude Code may therefore need project context again when a new session starts.

Without persistent instructions, you may repeatedly explain:

- Project architecture and folder structure
- Tech stack, libraries, and tools
- Coding conventions and naming patterns
- Commands to install, run, and test the project
- Important constraints and things to avoid

**Solution:** Use `CLAUDE.md` as a persistent project-instructions file that helps Claude Code understand how to work within your codebase.

## 2. What Is CLAUDE.md?

`CLAUDE.md` is a Markdown file containing instructions and context that Claude Code can use when working on a project.

It can include:

- Project overview
- Architecture and folder structure
- Coding conventions
- Preferred libraries and tools
- Development and testing commands
- Critical warnings and constraints
- Development roadmap and workflow

Claude Code can load applicable instructions automatically, reducing the need to repeat them in every session.

## 3. Creating CLAUDE.md

### Method 1: Create It Manually

Create a file named `CLAUDE.md` in your project root and add the instructions you want Claude Code to follow.

### Method 2: Use `/init`

Start Claude Code in your project directory and run:

```bash
claude
```

Inside Claude Code, execute:

```text
/init
```

Claude Code analyzes the codebase and generates an initial `CLAUDE.md` file.

**Recommended workflow:** Generate the initial file with `/init`, review it, remove unnecessary information, and add project-specific rules.

> `/init` is a starting point, not a finished configuration. You still need to supply the project's important workflows, constraints, and decisions.

## 4. How `/init` Works

At a high level, Claude Code analyzes relevant project information, such as:

1. Configuration and dependency files, such as `package.json` or `requirements.txt`.
2. README files and other project documentation.
3. Directory structure and important source files.
4. Technology stack, architecture, and naming conventions.
5. Commands and constraints that can be inferred from the codebase.

It then generates an initial `CLAUDE.md`.

Automatically inferred information may be incomplete. The developer must review it and add knowledge that cannot reliably be inferred from the source code.

## 5. What Should Go Inside CLAUDE.md?

### 5.1 Project Overview

Write a short, specific description of what the project does.

Example:

```markdown
# Project Overview

A Flask-based expense tracker that helps users
record and manage personal expenses.
```

### 5.2 Architecture

Explain where different responsibilities belong.

```markdown
## Architecture

- `app.py`: Flask application and route definitions.
- `templates/`: Jinja2 HTML templates.
- `static/css/`: Stylesheets.
- `static/js/`: JavaScript files.
- `tests/`: Automated tests.
```

Adapt the example to your actual project structure.

### 5.3 Coding Style

Specify conventions that should be followed consistently.

Examples:

- Use type hints in Python where appropriate.
- Keep functions small and focused.
- Follow existing naming conventions.
- Avoid unnecessary duplication.
- Follow the project's established patterns.

### 5.4 Technology Constraints

Define the approved technology stack and what must not be introduced.

```markdown
## Technology Constraints

- Use Flask for the backend.
- Use SQLite for persistence.
- Use vanilla JavaScript.
- Do not introduce additional JavaScript libraries
  without explicit approval.
```

### 5.5 Commands

Document exact commands for installing dependencies, running the application, testing, and building.

```markdown
## Commands

- Install dependencies: `pip install -r requirements.txt`
- Run tests: `pytest`
```

Only include commands that are valid for your actual project.

### 5.6 Critical Warnings and Things to Avoid

Document rules that prevent significant mistakes.

Examples:

- Do not modify database schemas without a clear requirement.
- Do not introduce new dependencies unnecessarily.
- Preserve existing authentication and authorization behavior.
- Do not change unrelated files when implementing a feature.

### 5.7 Development Roadmap

For projects with multiple planned features, maintain a simple implementation checklist.

```markdown
## Development Roadmap

- [x] Landing page
- [x] Legal pages
- [ ] User authentication
- [ ] Expense dashboard
- [ ] Automated tests
```

A roadmap can help keep development work organized across sessions. Keep it updated as the project evolves.

## 6. Understanding the `.claude/` Directory

The `.claude/` directory stores Claude Code configuration and project-specific customization files.

It may contain settings, custom commands, skills, agents, and rules.

### Project-Level vs User-Level Configuration

| Aspect          | Project-level                          | User-level                       |
| --------------- | -------------------------------------- | -------------------------------- |
| Location        | Inside the project                     | Inside the user's home directory |
| Scope           | Specific project                       | Multiple projects                |
| Sharing         | Shareable through Git when appropriate | Personal to the user             |
| Typical purpose | Project rules, commands, and workflows | Personal preferences             |
