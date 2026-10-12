# Claude Code — Installation and Project Setup

## 1. Install Claude Code

Claude Code is an AI-powered coding tool developed by Anthropic.

### Steps

1. Visit the official website: https://code.claude.com/
2. Create an Anthropic account at https://claude.ai/
3. Choose a suitable subscription or supported authentication method.
4. Follow the official installation instructions for your operating system.
5. Open a terminal and run:

```bash
claude
```

6. Complete authentication if prompted.
7. Review the folder trust prompt before allowing Claude Code to access a project.

**Important:** Claude Code is a paid service when using Anthropic's paid plans or usage-based billing. Pricing and authentication options can change. Check the official website for current details.

## 2. Open the Project in VS Code

The playlist uses a starter project for an expense tracking application.

1. Download the provided project ZIP file.
2. Extract the ZIP file.
3. Open VS Code.
4. Select **File → Open Folder**.
5. Choose the extracted project folder.
6. Open a new integrated terminal.

Start Claude Code from the project directory:

```bash
claude
```

**Why start Claude Code inside the project folder?**

It allows Claude Code to inspect the relevant project files and work within the project context.

## 3. Understand Bash Mode

Claude Code supports running terminal commands through its interface.

In the workflow demonstrated in the video:

- Press `Shift + 1` to enter Bash mode.
- Run the required shell commands.
- Return to the normal conversation to discuss the results.

Commands executed through this workflow can become part of the conversation context, helping Claude explain what was run and what happened.

**Example:** After installing dependencies, ask Claude to explain which packages were installed.

> Key idea: Keep your commands, results, and follow-up questions within the same workflow when useful.

The exact keyboard shortcut and interface can vary by version. You can also run commands in VS Code's integrated terminal.

## 4. Set Up the Python Virtual Environment

The starter project uses Python and Flask.

### Step 1: Create a virtual environment

Run from the project root:

```bash
python3 -m venv venv
```

This creates an isolated Python environment in the `venv` directory.

On Windows, if `python3` is unavailable, use:

```powershell
py -3 -m venv venv
```

### Step 2: Activate the environment

**macOS / Linux / Bash**

```bash
source venv/bin/activate
```

**Windows PowerShell**

```powershell
.\venv\Scripts\Activate.ps1
```

**Windows Command Prompt**

```cmd
venv\Scripts\activate.bat
```

After activation, the terminal typically displays the environment name.

### Step 3: Install dependencies

```bash
python -m pip install -r requirements.txt
```

This installs the packages listed in `requirements.txt`.

### Step 4: Run the Flask application

```bash
python app.py
```

If your system requires `python3`, use:

```bash
python3 app.py
```

### Step 5: Open the application

The video uses port `5001`.

Open the following URL in your browser:

```text
http://127.0.0.1:5001
```

Use the port printed by the application if it differs.

## 5. Understand the Starter Project

The project already contains a basic application structure.

### Implemented in the starter version

- Landing page
- Registration page UI
- Login page UI

### Not yet implemented

- Working user registration
- Working login and authentication
- Complete expense tracking functionality

The project is intentionally provided in a partially completed state to simulate a real-world development environment.

In many companies, developers join existing projects and extend their functionality rather than building everything from scratch.

## 6. Create a GitHub Repository

Git and GitHub are used to track changes and maintain the project's development history.

### Step 1: Create a repository

1. Visit https://github.com/
2. Create a new repository.
3. Set the repository name to `spendly`.
4. Add a description, such as `An expense tracking application built using Claude Code`.
5. Create the repository.

### Step 2: Initialize Git

Run these commands from your project directory.

```bash
git init
git add .
git commit -m "Initial commit"
```

### Step 3: Connect the remote repository

Copy the repository URL from GitHub.

```bash
git remote add origin <YOUR_REPOSITORY_URL>
```

Replace the placeholder with your actual GitHub repository URL.

### Step 4: Push the code

```bash
git branch -M main
git push -u origin main
```

Refresh your GitHub repository to verify that the project files are available.

**Before committing:** Review your files and ensure that secrets, credentials, environment files, and local virtual environments are not included. Use an appropriate `.gitignore`.

## 7. Explore an Existing Project with Claude Code

Before implementing features, understand the codebase.

The video demonstrates three useful questions.

### Question 1: What does this project do?

```text
What does this project do?
```

Use this to understand the application's purpose and existing functionality.

### Question 2: What technology stack does this project use?

```text
What technology stack does this project use?
Explain the frontend, backend, database, templating engine,
and testing tools.
```

The starter project described in the video uses:

| Area                 | Technology              |
| -------------------- | ----------------------- |
| Programming language | Python 3.11             |
| Backend framework    | Flask                   |
| Database             | SQLite                  |
| Templating engine    | Jinja2                  |
| Styling              | Vanilla CSS             |
| Testing              | pytest and pytest-flask |
| Frontend             | HTML and CSS            |

These details describe the video’s starter project; verify them against the actual repository.

### Question 3: Explain the project structure

```text
Explain the project structure to me.
Describe the purpose of each important directory and file.
Show how the main application components connect.
```

This helps you understand file organization, application entry points, dependencies, and existing design decisions.

**Recommended workflow for a new codebase:**

1. Understand the application's purpose.
2. Identify the technology stack.
3. Explore the project structure.
4. Identify implemented and missing features.
5. Start making changes only after understanding the current state.

## 8. Using Claude Code with Ollama

Ollama can run compatible local language models and, where supported, connect coding workflows to alternative models.

This can be useful for experimenting with AI coding without relying exclusively on Anthropic-hosted models.

Official website: https://ollama.com/

### Option A: Use an Ollama-hosted cloud model

Install Ollama using the current instructions on its official website.

Then follow its documented Claude Code integration instructions. In the video, the demonstrated command is:

```bash
ollama launch claude
```

Select an available model and complete any required authentication or connection steps.

**Note:** Cloud models may have usage limits, account requirements, and pricing. Free access is not guaranteed.

### Option B: Use a local model

Pull a model supported by your machine:

```bash
ollama pull <MODEL_NAME>
```

For example, the video demonstrates a Qwen coding model. Check Ollama's current model library for available names and sizes.

Launch the integration:

```bash
ollama launch claude
```

Select the downloaded local model when prompted.

### Local model considerations

- RAM and available GPU memory affect performance.
- Larger models generally require more resources.
- Local inference may be slower than hosted models.
- Smaller models may struggle with complex coding tasks.
- Compatibility with Claude Code depends on the model and integration.
- Local execution avoids hosted-model usage charges for inference, but still uses your hardware and electricity.

Do not assume every local model will work reliably with Claude Code.

## 9. Paid vs. Alternative Model Setup

| Approach                | Main benefit                                  | Main limitation                                   |
| ----------------------- | --------------------------------------------- | ------------------------------------------------- |
| Anthropic-hosted models | Direct access to supported Claude models      | Subscription or usage costs                       |
| Ollama cloud models     | Alternative model access through Ollama       | Account limits and possible charges               |
| Ollama local models     | Local inference without hosted inference fees | Hardware requirements and variable coding quality |

Choose the approach that suits your budget, hardware, and learning goals.

## 10. Complete Setup Checklist

- [ ] Install Claude Code or configure a supported alternative.
- [ ] Complete authentication if required.
- [ ] Download and extract the starter project.
- [ ] Open the project folder in VS Code.
- [ ] Start Claude Code from the project directory.
- [ ] Create and activate a Python virtual environment.
- [ ] Install dependencies from `requirements.txt`.
- [ ] Run the Flask application.
- [ ] Verify the landing page in your browser.
- [ ] Create a GitHub repository.
- [ ] Initialize Git and make the initial commit.
- [ ] Connect the remote repository and push the code.
- [ ] Ask Claude Code about the project's purpose.
- [ ] Review the technology stack and project structure.
- [ ] Confirm which features are implemented and which remain incomplete.

## 11. Quick Revision

| Command                                     | Purpose                                    |
| ------------------------------------------- | ------------------------------------------ |
| `claude`                                    | Start Claude Code                          |
| `python3 -m venv venv`                      | Create a virtual environment               |
| `source venv/bin/activate`                  | Activate the environment on macOS/Linux    |
| `python -m pip install -r requirements.txt` | Install dependencies                       |
| `python app.py`                             | Run the Flask application                  |
| `git init`                                  | Initialize Git                             |
| `git add .`                                 | Stage project changes                      |
| `git commit -m "Initial commit"`            | Create a commit                            |
| `git remote add origin <URL>`               | Connect a remote repository                |
| `git push -u origin main`                   | Push the main branch                       |
| `ollama pull <MODEL_NAME>`                  | Download a local model                     |
| `ollama launch claude`                      | Launch the demonstrated Ollama integration |

---

**Key takeaway:** Set up the development environment, run and understand the starter project, track your work with GitHub, and use Claude Code to explore the codebase before implementing new features.
