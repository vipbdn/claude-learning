# Claude Code — Slash Commands

## 1. What Are Slash Commands?

Slash commands are shortcuts used inside a Claude Code session. They start with `/` and trigger predefined actions or workflows without requiring a full prompt.

### Types of Slash Commands

- **Built-in commands:** Available by default in Claude Code.
- **Custom commands:** Created by developers for repetitive, project-specific workflows.

Example: `/exit` closes the current session.

---

## 2. Sessions

A **session** is one conversation with Claude Code.

- Start a session by running `claude` in the terminal.
- A session contains conversation history, tool interactions, and responses.
- Sessions are saved automatically and can be resumed later.
- Use separate sessions for separate features to keep context organized.

### Resume a Session

```bash
claude -r
```

Select a previous conversation from the list to resume it.

### Important Session Commands

| Command              | Purpose                                                                |
| -------------------- | ---------------------------------------------------------------------- |
| `/exit`              | Exit the current session                                               |
| `/resume`            | Switch to or resume a previous session                                 |
| `/rename <name>`     | Rename the current session                                             |
| `/export <filename>` | Export conversation history to a file                                  |
| `/btw <question>`    | Ask a side question without adding it to the main conversation history |

Example:

```text
/rename Login Feature
/export login-session.md
/btw What is Jinja2?
```

**Best practices:**

- Use one session per feature or task.
- Rename sessions early with meaningful names.
- Export important conversations before major refactoring.
- Commit code at meaningful milestones using Git.

---

## 3. Authentication Commands

| Command   | Purpose                        |
| --------- | ------------------------------ |
| `/login`  | Start the login flow           |
| `/logout` | Log out of the current account |

Use these commands when switching between personal and company accounts.

---

## 4. Model Selection

Use `/model` to switch between available Claude models.

The video describes three model families:

| Model  | Best Use                                                |
| ------ | ------------------------------------------------------- |
| Opus   | Complex planning, architecture, and difficult reasoning |
| Sonnet | Everyday coding and reliable implementation             |
| Haiku  | Simple, repetitive tasks requiring fast responses       |

### Recommended Workflow

1. Use a capable reasoning model to plan the feature and define its architecture.
2. Finalize the implementation plan or specification.
3. Use a suitable implementation model to write the code.
4. Review and test the generated changes.

_Model names, capabilities, pricing, and availability change over time. Choose from the models currently available in your Claude Code installation._

---

## 5. Usage and Statistics

| Command        | Purpose                                                          |
| -------------- | ---------------------------------------------------------------- |
| `/usage`       | Check current usage and remaining limits                         |
| `/extra-usage` | View or manage additional usage options, if available            |
| `/stats`       | View usage statistics and session activity                       |
| `/insights`    | Generate a report about usage patterns and possible improvements |

**Best practices:**

- Check `/usage` periodically during development.
- Use more expensive models selectively when the task justifies it.
- Review `/insights` after completing several sessions to identify opportunities to improve your workflow.

Usage limits and billing options depend on your account and current plan.

---

## 6. Configuration and Permissions

### `/config`

Opens Claude Code configuration settings. Depending on the version, settings may include language, output verbosity, thinking-related options, and terminal display preferences.

### `/permissions`

Manage which tools Claude Code can use automatically, which require approval, and which are denied.

Common permission categories:

- **Allow:** Permit a matching action without asking each time.
- **Ask:** Require approval before the action.
- **Deny:** Block the matching action.

Permission rules may be configured at different scopes, such as project-local or user-wide. Shared project settings can affect collaborators when committed to version control.

### Example Permission Rules

```text
Web search
Bash(git status)
```

The exact rule syntax depends on the tool and Claude Code version.

**Security best practices:**

- Grant only the permissions necessary for your workflow.
- Be especially careful with shell commands that modify files, delete data, or access sensitive resources.
- Avoid automatically approving broad or destructive commands.
- Review project-local permission settings before committing them.

---

## 7. Appearance and Voice

| Command  | Purpose                                     |
| -------- | ------------------------------------------- |
| `/theme` | Change the terminal interface theme         |
| `/voice` | Enable or disable voice input, if supported |

Voice mode lets you speak prompts instead of typing them.

---

## 8. Quick Revision

| Command        | Remember                            |
| -------------- | ----------------------------------- |
| `/exit`        | End the session                     |
| `/resume`      | Resume or switch sessions           |
| `/rename`      | Give a session a meaningful name    |
| `/export`      | Save conversation history to a file |
| `/btw`         | Ask an unrelated side question      |
| `/login`       | Authenticate                        |
| `/logout`      | Sign out                            |
| `/model`       | Change the model                    |
| `/usage`       | Check usage limits                  |
| `/extra-usage` | Manage additional usage options     |
| `/stats`       | Review usage statistics             |
| `/insights`    | Analyze usage patterns              |
| `/config`      | Change configuration                |
| `/permissions` | Manage tool permissions             |
| `/theme`       | Change appearance                   |
| `/voice`       | Toggle voice mode                   |

> **Note:** Slash commands can change between Claude Code versions. Type `/` inside a Claude Code session to view the commands available in your installation.

---

## 9. Practice Checklist

- [ ] Start Claude Code inside your project directory.
- [ ] Rename the current session.
- [ ] Exit and resume a previous session using `claude -r`.
- [ ] Use `/resume` to switch sessions.
- [ ] Ask an unrelated question using `/btw`.
- [ ] Export a conversation using `/export`.
- [ ] Switch models using `/model`.
- [ ] Check usage using `/usage`.
- [ ] Explore `/config` and `/permissions` without enabling unnecessary permissions.
- [ ] Explore `/theme` and `/voice` if supported.

## Key Takeaways

1. Slash commands simplify repetitive development workflows.
2. Separate sessions by feature to keep context focused.
3. Rename sessions so they are easy to identify and resume.
4. Use `/btw` for side questions that should not become part of the main conversation.
5. Plan with an appropriate model, then use a suitable model for implementation.
6. Review usage and permission settings regularly.
7. Explore the built-in command list as needed instead of memorizing every command.
