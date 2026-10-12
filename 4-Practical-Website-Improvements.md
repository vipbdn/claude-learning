# Claude Code — Practical Website Improvements

## 1. Learning Objectives

In this lesson, we use Claude Code to improve an existing Flask website.

- Modify existing code safely.
- Create new pages and routes.
- Convert a design image into working frontend code using multimodal capabilities.
- Implement interactive UI functionality.
- Commit and push changes to GitHub.

## 2. Start a New Session

Open Claude Code from your project directory:

```bash
claude
```

Rename the session:

```text
/rename Landing Page Improvements
```

**Best practice:** Use a separate, clearly named session for each feature or task.

---

## 3. Modify Existing Code

Use `@` mentions to reference specific files in your prompt.

Example:

```text
Add two links to the footer in @templates/base.html:
- Terms and Conditions
- Privacy Policy

Use plain text links without special styling.
Do not modify any other part of the page.
```

### Why Use `@`?

- Explicitly identifies the target file.
- Helps Claude Code understand which file needs modification.
- Reduces ambiguity when multiple files have similar purposes.

Always review proposed changes before approving them.

---

## 4. Create New Pages

Claude Code can create new templates and update application routes.

For example, to create a Terms and Conditions page:

```text
Create a Terms and Conditions page for the Spendly
personal expense tracking application.

Add a new route to @app.py.
Create a suitable template in the templates directory.
Include relevant sections for a personal expense tracking app.
Match the existing website theme.
Do not modify unrelated pages.
```

Repeat the same workflow for a Privacy Policy page.

### Typical Flask Page Workflow

1. Add a route in `app.py`.
2. Create a Jinja2 HTML template.
3. Link to the page from the footer.
4. Reuse existing CSS and layout where appropriate.
5. Open the page in the browser and verify the result.

**Important:** AI-generated legal pages are generic starting points, not guaranteed legally compliant documents. Review them for your application's actual requirements.

---

## 5. Match the Website's Existing Theme

When a newly created page looks different from the rest of the website, ask Claude Code to update its styling.

Example:

```text
Update @templates/terms.html so its appearance matches
the existing website theme.

Reuse existing styles wherever possible.
Avoid changing unrelated pages or components.
```

### Key Lesson

A page can function correctly but still have poor visual consistency. Verify both functionality and styling.

---

## 6. Convert a Design Image into Code

Claude Code supports multimodal input, allowing it to use an image as a visual reference.

### Workflow

1. Copy or attach the design image in the Claude Code session.
2. Reference the relevant HTML and CSS files.
3. Describe exactly which section should change.
4. Specify that unrelated parts must remain untouched.
5. Review the generated code in the browser.

Example prompt:

```text
Modify only the hero section in @templates/landing.html
and its styles in @static/css/style.css to match
the attached design image as closely as possible.

Do not modify any other section of the page.
Reuse existing project styles where appropriate.
```

### Best Practices

- Reference the correct file paths.
- Use the actual CSS filename.
- Clearly limit the scope of the changes.
- Compare the result against the reference image.
- Check desktop and mobile layouts.

**Common mistake:** Providing an incorrect CSS filename may cause Claude Code to create an unnecessary stylesheet. Verify the project structure before prompting.

---

## 7. Add a YouTube Video Modal

The goal is to make the “See How It Works” button open a modal containing an embedded YouTube video.

### Requirements

- Clicking the button opens a modal overlay.
- The modal contains an embedded YouTube video.
- A close button closes the modal.
- Clicking outside the modal closes it.
- Closing the modal stops video playback.
- No additional libraries or frameworks.
- Use vanilla JavaScript.
- Do not modify unrelated parts of the project.

Example prompt:

```text
Add a modal to @templates/landing.html that opens when
the user clicks the "See How It Works" button.

Requirements:
- Display an embedded YouTube video inside the modal.
- Use a placeholder video URL that can be replaced later.
- Close the modal when the close button is clicked.
- Close the modal when the user clicks outside its content.
- Stop video playback when the modal closes.
- Use vanilla JavaScript without additional dependencies.
- Match the existing website theme.
- Do not modify unrelated parts of the project.
```

### Important Implementation Detail

Removing or resetting the embedded iframe when closing the modal can stop the video from playing in the background.

Always test opening, closing, and reopening the modal.

---

## 8. Git Workflow: Commit Frequently

Commit meaningful changes rather than waiting until the entire feature is finished.

### Basic Workflow

```bash
git status
git add .
git commit -m "Add footer legal links"
```

Use descriptive commit messages for each milestone.

Examples:

```bash
git commit -m "Add terms and privacy pages"
git commit -m "Improve landing page hero section"
git commit -m "Add how-it-works video modal"
```

After verifying the completed work, push it to GitHub:

```bash
git push origin main
```

Replace `main` if your working branch has a different name.

### Recommended Sequence

1. Add footer links and commit.
2. Create and style the Terms and Conditions page; commit.
3. Create and style the Privacy Policy page; commit.
4. Update the hero section from the reference image; commit.
5. Implement and test the video modal; commit.
6. Review the final changes and push to GitHub.

---

## 9. Review and Iterate

Claude Code's first output is not guaranteed to be perfect.

Use this development cycle:

1. **Prompt:** Explain the desired change and its constraints.
2. **Review:** Inspect the proposed code changes.
3. **Approve:** Allow the appropriate edits.
4. **Test:** Verify the result in the browser.
5. **Refine:** Give a follow-up prompt if something is incorrect.
6. **Commit:** Save the verified milestone.
7. **Push:** Publish the completed changes to GitHub.

If duplicate CSS files are created, ask Claude Code to consolidate them carefully, preserve existing behavior, and remove obsolete styles only after verifying they are unused.

---

## 10. Key Takeaways

- Use `@filename` to provide explicit file context.
- Keep prompts specific about the target files and scope.
- Use images as design references for frontend implementation.
- Describe interaction requirements explicitly, including edge cases.
- Reuse existing styles and project structure where possible.
- Review and test every generated change.
- Commit meaningful milestones frequently.
- Use iterative refinement instead of assuming the first result is correct.

## Practice Checklist

- [ ] Start and rename a new Claude Code session.
- [ ] Add footer links using an explicit file reference.
- [ ] Create a Flask route and matching HTML template.
- [ ] Style the new page to match the existing website.
- [ ] Provide a design image and update only the hero section.
- [ ] Implement a video modal with proper close behavior.
- [ ] Inspect changes with `git status` and review the diff.
- [ ] Create descriptive commits for each milestone.
- [ ] Push the verified changes to GitHub.

## Coming Up Next

More advanced Claude Code workflows:

- Plan Mode for complex tasks.
- Writing specifications before implementation.
- Git branches and pull requests.
- Structured workflows for larger features.
