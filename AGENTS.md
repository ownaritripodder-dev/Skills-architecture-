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
