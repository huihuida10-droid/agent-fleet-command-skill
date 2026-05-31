# Agent Fleet Command — Repository-wide instructions for GitHub Copilot

When the user asks to **orchestrate a multi-agent project**, **manage parallel sub-agents with review chains**, or **create a phased development workflow**, use the agent-fleet-command skill.

## Key Principles

- **Main agent orchestrates, never writes code.** Creates documents, dispatches tasks, checks consistency.
- **Sub-agents implement, each creates its own reviewer.** Self-managed review chains.
- **Reviewer sub-agents review, never modify code.** Strict read-only review against both documents.
- **Phased execution.** Tasks grouped by dependency, independent tasks run in parallel within each phase.

## Document Structure

- `project-overview.md` — Architecture outline, module breakdown, dependency graph, unified standards
- `task-{name}.md` — Per-task detailed design with requirements, constraints, and acceptance criteria

## Review Dimensions

1. Spec compliance — code implements all requirements from document
2. Constraint adherence — only modified allowed files
3. Global consistency — style/naming matches project standards
4. Code quality — readability, maintainability, error handling
5. Test coverage — tests cover main functionality

## Severity Levels

- **Critical:** Must fix to pass (blocks submission)
- **Important:** Should fix (3+ blocks submission)
- **Minor:** Suggest fix (not blocking)

## Skill Priority Rule

Project rules (documents) override skill instructions, which override system defaults.

**More detail:** The `skills/` directory contains full SKILL.md guidance. For agents that support the Agent Skills format, install this repo as a skill for the complete reference.
