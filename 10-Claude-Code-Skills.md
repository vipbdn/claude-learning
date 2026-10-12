# Claude Code Skills — Notes & Practical Workflow

## 1. What Are Skills?

Skills are reusable, file-based resources that give an AI agent domain-specific knowledge, workflows, and best practices for specialized tasks.

They help turn a general-purpose AI model into a more reliable specialist.

**Examples:**

- Frontend design following a project's design conventions.
- PowerPoint generation following company guidelines.
- Data analysis using predefined scripts and methods.
- Code reviews following a team's standards.
- Writing documents in a consistent style.

### Why Do We Need Skills?

General-purpose models can perform many tasks, but they may not consistently follow specialized requirements.

For example, when generating a PowerPoint, an AI might understand slides and charts but not know your company's preferred layouts, fonts, colors, or design guidelines.

Detailed prompts can help, but they introduce several problems:

1. **Repetition:** The same instructions must be provided repeatedly.
2. **Context usage:** Long system prompts consume context even when their instructions are irrelevant.
3. **Resource management:** Supporting files, scripts, and templates are difficult to bundle directly into prompts.
4. **Sharing and versioning:** Prompts are not always organized for team collaboration and Git-based version control.
5. **Composability:** Large prompts containing multiple specialized workflows can become confusing.

Skills address these problems by organizing instructions and supporting resources into reusable folders.

---

## 2. Skills vs. Prompts

| Feature            | Detailed Prompt                         | Skill                                          |
| ------------------ | --------------------------------------- | ---------------------------------------------- |
| Reusability        | Often requires repetition               | Reusable across tasks                          |
| Context usage      | Long instructions may remain in context | Detailed instructions load when needed         |
| Supporting files   | Difficult to organize with the prompt   | Can include scripts, templates, and references |
| Version control    | Depends on how prompts are stored       | Can be managed with Git                        |
| Team collaboration | Requires a sharing mechanism            | Project skills can be shared with a repository |
| Multiple workflows | Long prompts can become confusing       | Separate skills can be combined                |
| Best use           | One-off instructions                    | Repeated, specialized tasks                    |

**Key idea:** Prompts provide instructions for a particular interaction; skills package reusable expertise and resources.

---

## 3. Skill Folder Structure

In Claude Code, a project-level skill is stored inside the project's `.claude/skills/` directory.

```text
my-project/
├── .claude/
│   └── skills/
│       └── frontend-design/
│           ├── SKILL.md
│           ├── scripts/
│           │   └── helper.py
│           └── templates/
│               └── design-reference.md
├── src/
└── README.md
```

### Important Files and Folders

- **`SKILL.md`** — Required file containing the skill's metadata and instructions.
- **`scripts/`** — Optional scripts needed to perform specialized tasks.
- **`templates/`** — Optional templates, examples, and reference materials.

A skill does not need additional resource folders if its instructions are sufficient on their own.

### SKILL.md Structure

A skill file contains two main parts:

1. YAML frontmatter
2. Markdown body

Example:

```markdown
---
name: frontend-design
description: Creates polished, responsive frontend interfaces following project conventions. Use when designing or improving UI components and pages.
---

# Frontend Design Instructions

## Design Principles

- Follow the existing project design system.
- Create responsive layouts.
- Use consistent spacing and typography.

## Implementation Guidelines

- Reuse existing components where appropriate.
- Follow the project's established CSS conventions.
- Keep the interface accessible.

## Validation

- Check the layout at different screen sizes.
- Verify that existing functionality is preserved.
```

### YAML Frontmatter

- `name`: Identifies the skill.
- `description`: Explains what the skill does and when it should be used.

The description is particularly important because Claude uses it to determine whether a skill is relevant to a task.

### Markdown Body

Contains the detailed instructions, such as:

- Workflow and implementation steps.
- Coding standards and design guidelines.
- Common mistakes to avoid.
- Validation requirements.
- References to supporting files.

**Remember:** A clear description helps Claude discover the skill; the body tells Claude how to perform the task.

---

## 4. Progressive Disclosure: How Skills Load

Skills use a progressive-disclosure approach to avoid loading all detailed instructions into the context window at once.

The process can be understood in three levels:

<pre>
Level 1: Skill Metadata
         Name + Description
         Loaded for discovery
                 |
                 v
Level 2: Skill Instructions
         SKILL.md body
         Loaded when relevant
                 |
                 v
Level 3: Supporting Resources
         Scripts, templates, references
         Read or used when required
</pre>

### Example

Suppose a project has a PowerPoint-generation skill.

1. Claude knows the skill's name and description.
2. The user requests a PowerPoint.
3. Claude identifies the relevant skill and reads its instructions.
4. If necessary, Claude reads the referenced templates or uses supporting scripts.
5. Claude performs the task using those guidelines.

This reduces unnecessary context usage compared with repeatedly providing a large set of instructions.

---

## 5. Types of Skills

### Personal Skills

Stored in the user's home-level Claude configuration:

```text
~/.claude/skills/
└── my-writing-style/
    └── SKILL.md
```

Useful for preferences that should apply across multiple projects, such as:

- Personal writing style.
- General coding preferences.
- Reusable development workflows.

### Project Skills

Stored within an individual project's repository:

```text
my-project/.claude/skills/
└── frontend-design/
    └── SKILL.md
```

Useful for project-specific conventions and workflows.

Advantages:

- Can be committed to Git.
- Can be shared with teammates.
- Can be reviewed and improved collaboratively.
- Remain associated with the project.

---

## 6. Three Ways to Create Skills

### Method 1: Create a Skill Manually

1. Create a skill directory.
2. Add a `SKILL.md` file.
3. Write the YAML frontmatter.
4. Add detailed instructions.
5. Include supporting resources if required.
6. Test the skill and refine it.

This provides direct control but requires familiarity with the skill format.

### Method 2: Use Claude's Skill Creator

Recommended when creating your first skill.

Explain three things to the skill creator:

1. **What should the skill do?**
2. **When should it be triggered?**
3. **What should successful output look like?**

Then review the generated instructions and test the skill in your project.

### Method 3: Use Community Skills

Skills can be obtained from public repositories and community marketplaces.

**Security precautions:**

- Read the entire skill before using it.
- Inspect scripts and referenced files.
- Check for suspicious shell commands, network requests, and credential access.
- Avoid exposing API keys, environment variables, or other secrets.
- Prefer trusted sources and review updates before installing them.

A skill's instructions are not automatically trustworthy just because they are packaged as a skill.

---

## 7. Skill Creation Workflow

Use this workflow when creating a reusable skill:

1. **Identify the need:** Choose a specialized task that occurs repeatedly.
2. **Create the directory:** Set up the skill's folder.
3. **Write `SKILL.md`:** Define metadata, triggers, workflow, and requirements.
4. **Add resources:** Include necessary scripts, templates, and references.
5. **Test the skill:** Try realistic tasks and evaluate the output.
6. **Iterate and improve:** Fix missing instructions and refine the workflow.
7. **Version the skill:** Commit project-level skills to Git when appropriate.

Do not create a skill for every small task. Skills are most valuable when specialized instructions are reused.

---

## 8. Practical Example: Frontend Design Skill

### Project Goal

Create a polished profile page for an expense-tracking application.

The page contains:

- User information cards.
- Summary statistics, such as total spending and transaction count.
- A transaction history table.
- A category-wise spending breakdown.

The initial UI uses static, hardcoded data rather than database-driven values.

### Workflow Without a Skill

1. Write a feature specification.
2. Generate an implementation plan.
3. Implement the profile page.
4. Review the output.

Claude may create a functional page, but its design decisions might not consistently match the desired quality or project conventions.

### Workflow With a Skill

1. Create a reusable frontend-design skill.
2. Define design principles, styling conventions, component reuse, and validation requirements.
3. Store it in `.claude/skills/frontend-design/SKILL.md`.
4. Restart Claude Code if needed to discover the newly created skill.
5. Create or reuse the feature specification.
6. Ask Claude to generate a plan using the specification and design skill.
7. Review and implement the plan.
8. Compare the resulting UI with the original.

### Observed Result

The demonstration showed a modest design improvement, including a more compact category breakdown. The difference was not dramatic because the example application was relatively simple.

**Lesson:** Skills provide reusable guidance, but they do not guarantee perfect output. Their effectiveness depends on the quality of their instructions, the task, and the validation process.

---

## 9. Feature Development Workflow with Skills

A disciplined Git-based workflow keeps feature development organized.

1. Pull the latest changes.
2. Create and switch to a feature branch.
3. Write and review the feature specification.
4. Generate and review the technical implementation plan.
5. Implement the feature using relevant skills.
6. Validate the implementation against requirements.
7. Run appropriate tests.
8. Commit and push the feature branch.
9. Open a pull request, review it, and merge it into the main branch.
10. Delete the feature branch and update the local main branch.

### Example Commands

```bash
git pull origin main

git switch -c feature/profile-page

# Implement and validate the feature

git add .
git commit -m "Add profile page UI"
git push -u origin feature/profile-page
```

After the pull request is merged:

```bash
git switch main
git pull origin main
git branch -d feature/profile-page
```

Use your actual branch name and repository conventions.

---

## 10. Skills and Slash Commands

Claude Code has evolved its customization model so that skills can also be invoked explicitly through slash commands.

A skill can be automatically selected by Claude when relevant, or invoked manually when supported by its configuration.

For example, a frontend design skill might be available as a slash command based on its skill name.

To make a skill **manual-invocation only**, its frontmatter can include:

```yaml
---
name: frontend-design
description: Creates polished frontend interfaces following project conventions.
disable-model-invocation: true
---
```

This setting prevents Claude from automatically choosing the skill; the user can invoke it explicitly.

**Important:** Use the current Claude Code documentation when setting up skills. The configuration and discovery behavior can evolve between versions. Do not assume that older standalone command-folder conventions are still the preferred approach.

---

## 11. Key Takeaways

- Skills package specialized knowledge into reusable files.
- `SKILL.md` is the required instruction file.
- The YAML `description` helps Claude decide when a skill is relevant.
- Supporting scripts and templates can extend a skill's capabilities.
- Progressive disclosure helps avoid loading detailed instructions unnecessarily.
- Personal skills are suitable for cross-project preferences; project skills are suitable for repository-specific conventions.
- Skills can be created manually, generated with a skill creator, or obtained from trusted sources.
- Always test and improve skills instead of assuming the first version is perfect.
- Skills can support both automatic selection and explicit invocation.
- Specifications define what to build; skills guide how specialized work should be performed.

## Revision Checklist

- [ ] Explain why detailed prompts are not always sufficient.
- [ ] Describe the purpose of `SKILL.md`.
- [ ] Identify the role of YAML frontmatter and `description`.
- [ ] Explain progressive disclosure at all three levels.
- [ ] Distinguish personal skills from project skills.
- [ ] Create a skill using a skill creator.
- [ ] Test a skill and refine its instructions.
- [ ] Explain how skills can be invoked automatically or manually.
- [ ] Commit a project-level skill to Git and share it with a team.
