# Skills Framework Guidance

## Mandatory Procedure

Before starting any task, you must:

1. Call the `github_read_skill` MCP tool with relevant keywords to see if there is an existing certified skill for the task (e.g., if writing Apex, search "apex").
2. Read the returned `SKILL.md` via the tool and follow its instructions exactly.
3. If no match is found, or if an existing skill needs an update based on new best practices discovered during a task, you must propose a new skill or update using the `github_write_skill` MCP tool.
4. When writing a skill, it will automatically be routed to the `incubator/` directory and a Pull Request will be raised. You must ask the user to review the PR.
5. Only trust skills with `status: certified` for production tasks. Draft or uncertified skills from the `incubator/` should be used with extreme caution.

## Core Directives

- **Zero Hallucination:** Rely strictly on the instructions documented in the skills. Do not invent Salesforce patterns if a skill exists for that domain.
- **Continuous Improvement:** If a user corrects you during a task, document that correction by creating or updating a skill so the mistake is not repeated.

## MANDATORY: Pushing Skill Changes (NEVER skip this)

**You MUST NEVER run `git push` directly without following this protocol first.**

Whenever the user asks you to push, commit, or save any changes to a skill file:

### Step 1 — Call `github_write_skill` FIRST (REQUIRED)

Call the `github_write_skill` MCP tool with `local_mode: true`. This returns the step-by-step SOP you must follow. Do NOT skip this call.

### Step 2 — Ask about the branch (MANDATORY, ALWAYS WAIT for answer)

You MUST ask the user the following question and STOP until they answer:

> "Which branch would you like to push to?
>
> 1. **Current branch** — push to whatever branch is currently checked out locally
> 2. **New custom branch** — I will create a new branch with a name you choose
> 3. **Auto-generate** — I will create a branch named `skill/<skill-name>-<timestamp>`"

**Do NOT assume, infer, or self-decide the branch.** Do NOT move to the next step until the user explicitly replies.

### Step 3 — Execute git commands

Only after the user has answered Step 2, run:

```
git add <file>
git commit -m "<message>"
git push -u origin HEAD   # or: git push origin <branch>
```

### Step 4 — Ask about PR (ALWAYS ask after pushing)

After a successful push, ask the user:
