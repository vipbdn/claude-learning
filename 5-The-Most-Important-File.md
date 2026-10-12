# Claude Code — Context Window Management

## 1. What Is a Context Window?

A **context window** is the amount of information, measured in tokens, that an AI model can process at one time while generating a response.

Think of it as the model's **working memory**.

### What Is Context?

Context is all the information needed to understand a task correctly.

In software development, context can include:

- Existing codebase and project structure
- PRDs, specifications, and requirements
- GitHub issues, Jira tickets, and bug reports
- Team discussions and Slack messages
- Previous AI conversations
- Git repository history and existing implementation

The more relevant context Claude has, the better it can understand the task and make appropriate changes.

However, a model cannot process unlimited information. Its context window has a fixed limit.

## 2. Important Context Window Concepts

### Context Window Has a Limit

The video uses approximately **200K tokens** as the context-window size for the Claude models discussed. The actual limit depends on the model and its current configuration.

Do not assume every Claude model has the same context limit.

### Every New Session Starts Fresh

A new Claude Code session starts with a fresh conversation context, along with the project's applicable instructions and configuration.

This makes separate sessions useful for independent tasks and features.

### Both Input and Output Consume Tokens

| Token source  | Examples                                                          |
| ------------- | ----------------------------------------------------------------- |
| Input tokens  | Prompts, pasted code, images, conversation history                |
| Output tokens | Claude's responses, generated code, tool calls and related output |

Tool results and other information added to the conversation can also consume context.

**Key takeaway:** Context is consumed by more than just the prompt you type.

### Conversation History Is Reused

During a conversation, Claude Code needs the relevant previous conversation history to understand each new request.

As the conversation grows, the amount of history that must be considered can grow too.

This is why long sessions can consume context faster than expected.

## 3. Why Long Sessions Consume More Context

Consider a simplified example:

- Your message contains 100 tokens.
- Claude's response contains 100 tokens.
- Each new turn includes the previous conversation history.

| Turn | Approximate tokens processed in that turn |
| ---- | ----------------------------------------: |
| 1    |                                       200 |
| 2    |                                       400 |
| 3    |                                       600 |
| 4    |                                       800 |
| 10   |                                     2,000 |

These figures are illustrative and assume fixed-size messages and responses. Actual token usage varies.

The important concept is that previous conversation history may be processed repeatedly.

### Practical Example

Suppose you need to develop four independent features.

**Approach A: One long session**

- Develop all four features in the same conversation.
- The context keeps growing.
- Later tasks may repeatedly carry the history of earlier tasks.

**Approach B: Separate sessions**

- Give each feature its own session.
- Keep each conversation focused.
- Reduce irrelevant history carried into later work.

**Recommended:** Use a separate session for each independent feature whenever practical.

## 4. What Occupies Claude Code's Context Window?

The video describes several sources of context:

1. **System prompt** — instructions that define the model's behavior.
2. **Tool schemas** — descriptions of available tools and their inputs and outputs.
3. **Project instructions** — information loaded from files such as `CLAUDE.md`.
4. **Conversation history** — your messages and Claude's responses.
5. **Tool outputs** — command results, file contents, and other retrieved information.
6. **MCP tools** — tool definitions and related information.
7. **Skills** — instructions for specific kinds of tasks.
8. **Compaction reserve** — context capacity reserved for summarizing conversation history, where applicable.

These items mean the entire advertised context window is not necessarily available for your own prompts and coding tasks.

The video uses an approximate example of a 200K-token window with around 150K tokens available for practical work. Treat these numbers as illustrative, not universal guarantees; actual allocation varies by model and configuration.

### Check Context with `/context`

In Claude Code, run:

```text
/context
```

This displays information about context usage and available capacity.

Use it periodically to understand how much context your current session is consuming.

## 5. Why Context Window Management Matters

### A. Cost Control

Token usage can affect costs, depending on your plan and billing arrangement.

Keeping conversations focused and avoiding unnecessary context can help reduce wasted token usage.

### B. Better Workflow Design

A single session for many unrelated features can carry unnecessary history into later tasks.

Separate sessions help keep the work organized and relevant.

### C. Response Quality

As context becomes crowded, the model may have more difficulty prioritizing relevant information, especially in long and complicated conversations.

A large context window does not automatically guarantee better results. **Relevant, well-organized context matters.**

## 6. What Is Auto-Compaction?

Auto-compaction is a mechanism that summarizes conversation history when context usage approaches a configured threshold.

The general process is:

1. Context usage grows during a session.
2. Claude Code triggers compaction when needed.
3. Earlier conversation history is summarized.
4. The conversation continues with a more compact representation of that history.

Compaction helps preserve the important information while freeing context capacity.

However, **a summary is not guaranteed to preserve every detail**. Specific requirements, decisions, code snippets, and edge cases may be lost.

The exact trigger threshold and reserved capacity can vary.

## 7. Manually Compact with `/compact`

You can compact a conversation yourself:

```text
/compact
```

This asks Claude Code to summarize the conversation and continue with a more compact context.

### Why Compact Proactively?

Automatic compaction may occur at an inconvenient point, such as during a complex implementation.

Manual compaction gives you more control over when the conversation is summarized.

### Recommended Workflow

1. Check context with `/context`.
2. Notice when usage becomes high.
3. Finish or pause the current important implementation task.
4. Run `/compact`.
5. Review the resulting summary when necessary.
6. Continue with the next focused task.

The video recommends considering manual compaction around 70–75% usage. This is a practical guideline, not a fixed rule for every model or session.

### Important Limitation

Repeated compaction can still lose details, and summaries also consume context. Compaction does not provide unlimited memory.

## 8. Other Ways to Manage Context

### Option 1: Use Subagents

Subagents can handle isolated tasks using their own context.

For example, you might ask a subagent to investigate a bug or explore a separate part of the codebase.

Benefits:

- Keeps exploratory work separate from the main conversation.
- Allows independent tasks to run in parallel when supported.
- Can return a concise summary instead of bringing every intermediate detail into the main conversation.

Use subagents when tasks are independent and the work benefits from isolation. They are not a universal replacement for a fresh main session.

### Option 2: Use `/clear`

```text
/clear
```

Clears the current conversation context so you can begin a new task without the previous conversation history.

Use it when the current task is complete and you no longer need that conversation's working context.

### Option 3: Start a New Session

Starting a new session is often the cleanest option when moving to a separate feature or unrelated task.

Before leaving a session, preserve important decisions, requirements, and implementation status if the next session needs them.

### Which Option Should You Choose?

| Situation                                               | Recommended action      |
| ------------------------------------------------------- | ----------------------- |
| Context usage is growing, but the task continues        | `/compact`              |
| Current task is complete and the next task is unrelated | New session or `/clear` |
| Independent investigation or parallel work              | Subagent                |
| Need a quick usage check                                | `/context`              |

## 9. Best Practices for Context Management

### 1. One Feature, One Session

Keep each independent feature in a focused session rather than implementing every feature in one long conversation.

### 2. Compact Proactively

Check context periodically and compact when appropriate. Avoid waiting until the session becomes difficult to manage.

### 3. Write Focused Prompts

Avoid vague instructions such as:

```text
Improve my application.
```

Prefer specific requests:

```text
Update the login form validation.
Modify only the relevant login component and its stylesheet.
Keep the existing layout and authentication flow unchanged.
```

Specific prompts reduce ambiguity and unnecessary exploration.

### 4. Use Subagents for Isolated Work

Delegate independent investigations or parallelizable tasks when appropriate.

### 5. Exclude Irrelevant Files Carefully

The video introduces `CLAUDE.ignore` as an approach to excluding files from Claude's attention. However, do not assume this is a universally supported or strictly enforced mechanism.

For standard project-wide file exclusions, use `.gitignore` for Git and follow the current Claude Code documentation for supported context and permission controls. These mechanisms serve different purposes: `.gitignore` does not automatically guarantee that Claude Code will ignore a file.

Avoid unnecessarily loading generated files, build outputs, dependency directories, and large irrelevant artifacts into context.

### 6. Preserve Critical Information

Before compacting or starting a new session, make sure important details are captured somewhere reliable:

- Requirements and acceptance criteria
- Decisions and constraints
- Files changed
- Tests completed
- Known issues
- Next steps

Do not rely on the conversation alone as the permanent record of your project.

## 10. Terminal vs. GUI Access

Claude Code can be accessed through different interfaces, including the terminal and supported graphical integrations.

The video recommends learning the terminal workflow because it exposes command-based workflows and advanced capabilities. Some features may require the terminal even when other interfaces provide a useful graphical experience.

Choose the interface that suits the task, but learn the terminal commands for context management and other advanced workflows.

## 11. Quick Revision Cheat Sheet

| Command     | Purpose                                            |
| ----------- | -------------------------------------------------- |
| `/context`  | Inspect context usage                              |
| `/compact`  | Summarize the current conversation to free context |
| `/clear`    | Clear the current conversation context             |
| New session | Start a fresh conversation context                 |

## Practice Checklist

- [ ] Start a Claude Code session in your project.
- [ ] Run `/context` and inspect the usage.
- [ ] Ask a few focused questions about your codebase.
- [ ] Run `/context` again and compare usage.
- [ ] Run `/compact` and observe how the conversation summary changes.
- [ ] Run `/context` again.
- [ ] Start a new session for a separate feature.
- [ ] Practice writing a short, specific prompt with clear file and scope constraints.

## Final Takeaways

- The context window is the model's limited working memory.
- Prompts, responses, conversation history, and tool information can all consume context.
- Long conversations can repeatedly carry earlier history.
- `/context` helps you inspect usage.
- `/compact` summarizes the conversation; it cannot guarantee perfect preservation of every detail.
- Separate sessions help keep independent features focused.
- Subagents can isolate suitable tasks.
- Specific prompts and relevant project information help you use context effectively.
