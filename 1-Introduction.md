# Claude Code — Introduction & Learning Roadmap

## 1. What Is Claude Code?

- Claude Code is an AI-powered coding tool developed by Anthropic.
- It helps developers build, modify, debug, test, and deploy software.
- It can work with large codebases and assist with software architecture and refactoring.
- It supports agentic workflows, including using multiple agents for different tasks.

**Goal:** Integrate AI into the software development workflow to improve productivity and code quality.

## 2. AI-Assisted Coding vs. Vibe Coding

### Vibe Coding

Vibe coding relies heavily on natural-language prompts to generate software.

**Basic workflow:**

1. Describe the application or feature in a prompt.
2. Let AI generate the code.
3. Test the result.
4. Report problems to AI and ask it to fix them.
5. Repeat until the result works as expected.

**Suitable for:**

- MVPs and prototypes
- Exploring new ideas
- Hackathon projects
- Low-risk applications

**Limitations:**

- Developers may not understand the generated code.
- Architecture and maintainability may be neglected.
- Complex, scalable, or critical systems require stronger engineering practices.

### AI-Assisted / Agentic Coding

AI-assisted coding combines developer expertise with AI capabilities.

The developer remains responsible for:

- Understanding requirements
- Designing system architecture
- Planning implementation
- Reviewing generated code
- Testing and debugging
- Ensuring security, scalability, and maintainability

AI agents can perform specific tasks, such as implementation, testing, and code review.

**Key principle:** Use AI as an engineering assistant, not as a replacement for engineering judgment.

## 3. Project for the Playlist

The playlist will build an end-to-end **Expense Tracking Website** using Claude Code.

### Planned Features

- Landing page
- User registration and login
- User dashboard
- Expense summary statistics
- Total spending and transaction count
- Top spending category
- Transaction history with descriptions
- Spending analysis by category
- Date-based filtering
- Add new expenses
- Edit existing expenses
- Delete expenses
- Cloud deployment

### Dashboard Requirements

The dashboard should update its statistics and transaction history according to the selected date range.

For example, selecting the previous two months should filter:

- Total spending
- Number of transactions
- Top spending category
- Transaction history
- Spending by category

### Technology Stack

| Area            | Technology     |
| --------------- | -------------- |
| AI coding tool  | Claude Code    |
| Backend         | Python, Flask  |
| Frontend        | HTML, CSS      |
| Version control | Git            |
| Code hosting    | GitHub         |
| Deployment      | Cloud platform |

## 4. Why Claude Code?

The instructor selects Claude Code based on the following capabilities:

- **Coding intelligence:** Assistance with complex implementation tasks.
- **Long-context handling:** Working with large codebases and understanding related files.
- **Architecture:** Helping design and improve software structure.
- **Refactoring:** Improving existing code while maintaining functionality.
- **Agentic capabilities:** Assigning specialized tasks to different agents.
- **Parallel development:** Working on multiple independent features simultaneously.
- **Deployment assistance:** Helping deploy and manage applications.

Other AI coding tools exist, including Cursor, Windsurf, OpenAI Codex, Replit, and Lovable. The playlist focuses on Claude Code first.

## 5. Learning Objectives

By completing the playlist, the learner should aim to:

1. **Learn AI-assisted coding**
   - Integrate AI into an existing development workflow.
   - Collaborate with AI instead of manually writing every line of code.

2. **Understand Claude Code**
   - Learn its core features and capabilities.
   - Use it as a practical software development tool.

3. **Reduce fear of AI**
   - Understand AI coding tools through hands-on experience.
   - Form independent opinions about their strengths and limitations.
   - Learn where human engineering skills remain essential.

4. **Build practical applications**
   - Develop an end-to-end website.
   - Work with agents and subagents.
   - Explore parallel feature development.
   - Deploy the finished application.

## 6. Agents and Subagents

Claude Code supports workflows in which different agents handle different responsibilities.

Possible tasks include:

- Feature implementation
- Test creation and execution
- Code review
- Independent development tasks

Multiple agents can work in parallel when their tasks are sufficiently independent.

**Important:** Parallel work still requires coordination, integration, testing, and review.

## 7. Prerequisites

Before starting the playlist, have a basic understanding of:

- [ ] Python fundamentals
- [ ] Flask basics
- [ ] HTML
- [ ] CSS
- [ ] Git
- [ ] GitHub

Git and GitHub are especially important because they will be used extensively throughout the development process.

**Recommendation:** Learn programming fundamentals before relying heavily on AI coding tools. You should be able to understand, evaluate, and debug the code AI generates.

## 8. Industry-Oriented Development Mindset

When building real-world software with AI:

- Understand the problem before writing prompts.
- Break large features into smaller tasks.
- Maintain a clear application architecture.
- Review AI-generated changes.
- Test functionality and edge cases.
- Use Git to track changes and recover from mistakes.
- Consider security, reliability, and scalability.
- Avoid assuming that generated code is automatically correct.

AI can accelerate implementation, but production-quality software still requires engineering discipline.

## 9. Quick Revision

| Topic              | Key Takeaway                                                |
| ------------------ | ----------------------------------------------------------- |
| Claude Code        | AI-powered software development tool                        |
| Vibe coding        | Prompt-driven generation with limited developer involvement |
| AI-assisted coding | Developer-guided implementation using AI                    |
| Agentic coding     | Using agents to execute defined development tasks           |
| Context handling   | Working with relevant information across a codebase         |
| Parallel agents    | Developing independent tasks simultaneously                 |
| Git                | Tracking code changes and managing versions                 |
| GitHub             | Hosting and collaborating on code repositories              |
| Final project      | Expense tracking website                                    |
| Backend            | Python and Flask                                            |
| Frontend           | HTML and CSS                                                |
| Final objective    | Build, test, and deploy a working application               |

## 10. Practice Checklist

- [ ] Review Python and Flask fundamentals.
- [ ] Revise HTML and CSS.
- [ ] Practice Git commits, branches, and merges.
- [ ] Learn to create and manage GitHub repositories.
- [ ] Understand the difference between vibe coding and AI-assisted coding.
- [ ] Identify tasks suitable for specialized AI agents.
- [ ] Follow the expense tracker project through implementation and deployment.

---

**Core takeaway:** Learn to direct, inspect, test, and improve AI-generated code. The objective is not merely to generate an application, but to develop a reliable software engineering workflow with AI as a collaborator.
