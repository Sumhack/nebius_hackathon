# Local Coding Agent — Project Plan

## Project goal

Build a small, inspectable coding agent that runs on the developer's machine, examines a selected code repository, and answers questions about that code. Start with read-only repository Q&A; add coding tools and a VS Code interface only after the core agent works.

The target repository for the initial hands-on evaluation is [`Sumhack/aienglishlearn`](https://github.com/Sumhack/aienglishlearn). This is separate from this repository, which contains the agent we are building.

## Initial architecture

- **Agent runtime:** Python process running locally.
- **Model inference:** Nebius Token Factory, using an NVIDIA open source model available there.
- **Repository access:** Local read-only access to the target repository.
- **First interface:** Terminal CLI. A VS Code extension is a later phase.
- **Safety boundary for the first version:** Read and search files only. No edits, shell commands, or MCP integrations.

## Roadmap and task list

### Stage 1 — Read-only repository Q&A (first milestone)

- [x] 1.1 Confirm the agent project's Python version and dependency approach.
- [ ] 1.2 Check out or locate `aienglishlearn` locally; record its path without copying it into the agent project.
- [ ] 1.3 Add configuration for the Nebius Token Factory endpoint, model, and API key; keep credentials out of source control.
- [ ] 1.4 Implement a CLI that accepts a repository path and a question.
- [ ] 1.5 Implement safe repository discovery: list and read relevant text files while skipping `.git`, dependency folders, virtual environments, generated files, and likely secrets.
- [ ] 1.6 Give the model bounded, relevant file context and ask it to answer using that context.
- [ ] 1.7 Display the answer with paths (and line references where practical) so the user can verify the findings.
- [ ] 1.8 Try representative questions against `aienglishlearn`, including locating a feature and explaining its flow; refine retrieval based on observed misses.
- [ ] 1.9 Document setup, configuration, and CLI usage.

**Stage 1 done when:** From a terminal, we can point the agent at the local `aienglishlearn` checkout, ask a question about its code, and receive a useful answer grounded in identifiable files. The agent must remain read-only.

### Stage 2 — Better repository context

- [ ] 2.1 Improve file ranking and code-aware context selection for larger repositories.
- [ ] 2.2 Add configurable ignore rules and context limits.
- [ ] 2.3 Make missing, unreadable, or oversized context clear to the user.
- [ ] 2.4 Re-evaluate the same questions against `aienglishlearn` and record retrieval gaps.

**Stage 2 done when:** The agent can consistently find relevant files in the target repo without sending the entire repository to the model.

### Stage 3 — Reviewable coding actions

- [ ] 3.1 Add a structured proposal format for file changes.
- [ ] 3.2 Show proposed diffs before applying them.
- [ ] 3.3 Require explicit approval before writing any file.
- [ ] 3.4 Add command execution as a separate, explicitly approved capability with visible command and output.
- [ ] 3.5 Demonstrate a small change and run a relevant check in `aienglishlearn`.

**Stage 3 done when:** A user can inspect a proposed patch, approve or reject it, and separately approve a command before it runs.

### Stage 4 — VS Code interface

- [ ] 4.1 Define a small local communication protocol between the extension and Python agent.
- [ ] 4.2 Build a VS Code chat panel that uses the currently opened workspace as the target repo.
- [ ] 4.3 Show answers, referenced files, proposed actions, approval prompts, and command output in the extension.
- [ ] 4.4 Document how to launch and configure both components.

**Stage 4 done when:** A user can open a repository in VS Code and use the local agent from an extension chat panel.

### Stage 5 — Release and hackathon submission

- [ ] 5.1 Review the project against the current hackathon submission requirements.
- [ ] 5.2 Add an open source license and a clear README with setup and run instructions.
- [ ] 5.3 Prepare a working demo of the agent on `aienglishlearn`.
- [ ] 5.4 Record a short public demo video showing the project working and the Nebius/NVIDIA model use.
- [ ] 5.5 Submit the project description, demo, and public repository.

**Stage 5 done when:** The public repository and submission materials are complete and demonstrate the required model/platform use.

## Working order

We will take one unchecked task at a time, starting with **1.1**. After each task, we will update this file with its status and any decisions made. Later stages can be adjusted based on what we learn from the working prototype.

## Decisions to preserve

- Require Python 3.11 or newer; use `uv` to manage the environment and `pyproject.toml` for project metadata and dependencies. Python 3.14.2 and `uv` are available in the current development environment.
- Keep the agent project and target application as separate repositories.
- Build the agent logic in Python; a VS Code extension can use TypeScript when we reach Stage 4.
- Keep repository contents and tool execution local; send only selected context to Nebius for inference.
- Begin read-only. Add file writes and command execution only in later stages, with explicit user approval.
- Do not add MCP or multi-agent orchestration to the first milestone; revisit only if it clearly improves the demo.
