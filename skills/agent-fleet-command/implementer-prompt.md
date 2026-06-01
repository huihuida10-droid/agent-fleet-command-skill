# Implementer Sub-Agent Prompt Template

Use this template when dispatching an implementer sub-agent.

**Purpose:** Guide sub-agent through reading docs, checking skills, implementing, self-reviewing, and creating reviewer.

```
Task tool (general-purpose):
  description: "Implement Task N: [task name]"
  prompt: |
    You are implementing Task N: [task name]

    ## Task Description

    [FULL TEXT of task from task-{name}.md - paste it here]

    ## Project Context

    [FULL TEXT of project-overview.md - paste it here]

    ## Completed Tasks Context

    [List of already-completed tasks and their key outputs, for consistency]

    ## Document Output Path

    Your task folder: `{project-folder}/task-{name}/`
    Review results will be written to: `{project-folder}/task-{name}/review-result.md`

    ## Your Job

    1. Read and understand both documents above
    2. Check skill library for relevant skills
    3. Ask questions if unclear (before coding)
    4. Implement exactly what the task specifies
    5. Write tests (TDD)
    6. Self-review
    7. Create reviewer sub-agent
    8. Fix issues if reviewer rejects
    9. Report back

    ## Skill Library Check

    Before coding, scan the skill directory for relevant skills:
    - `~/.claude/skills/` — check each skill's description
    - Match by: task type, technology, patterns
    - If match found: load and follow the skill
    - **Priority:** Project rules (documents) override skill instructions

    ## Before You Begin

    If you have questions about:
    - Requirements or acceptance criteria
    - Approach or implementation strategy
    - Dependencies or assumptions
    - Anything unclear

    **Ask them now.** Don't guess or make assumptions.

    ## Implementation Rules

    1. ONLY modify files listed in "涉及文件" from your task document
    2. Do NOT modify files outside your scope
    3. Follow the unified standards from project-overview.md
    4. Follow TDD: write failing test first, then implement
    5. Keep files focused (one clear responsibility per file)

    ## Self-Review Checklist

    Before creating reviewer, check:

    **Completeness:**
    - Did I implement everything in the spec?
    - Did I miss any requirements?
    - Are there edge cases I didn't handle?

    **Quality:**
    - Are names clear and accurate?
    - Is the code clean and maintainable?
    - Did I follow the unified standards?

    **Discipline:**
    - Did I avoid overbuilding (YAGNI)?
    - Did I only build what was requested?
    - Did I stay within my file scope?

    **Testing:**
    - Do tests verify behavior (not just mock)?
    - Are tests comprehensive?

    ## Create Reviewer

    After self-review passes, create a reviewer sub-agent:

    ```
    Task tool (general-purpose):
      description: "Review Task N: [task name]"
      prompt: |
        [Use reviewer-prompt.md template, filling in:
         - Full task text
         - Full project-overview.md
         - List of files you modified
         - Your implementation summary]
    ```

    Wait for reviewer result.

    ## Handling Review Result

    **If reviewer says ✅ PASSED:**
    - Report back with status: DONE

    **If reviewer says ❌ FAILED:**
    - Read the issue list
    - Fix ALL Critical issues
    - Fix Important issues (3+ Important = must fix)
    - Create NEW reviewer sub-agent (don't reuse)
    - Wait for new review result
    - Repeat until passed

    **If you can't fix the issues:**
    - Report back with status: BLOCKED
    - Describe what you tried and what's blocking you

    ## Report Format

    When done, report:
    - **Status:** DONE | DONE_WITH_CONCERNS | BLOCKED | NEEDS_CONTEXT
    - What you implemented
    - What you tested and results
    - Files changed (exact paths)
    - Self-review findings
    - Reviewer result (passed/failed, iteration count)
    - Any concerns or issues
```
