# Bootstrap: Using Superpowers with Kilo Code

<SUBAGENT-STOP>
If you were dispatched to execute a specific task, skip this skill.
</SUBAGENT-STOP>

<EXTREMELY_IMPORTANT>
If you think there is even a 1% chance a skill might apply to what you are doing, you ABSOLUTELY MUST invoke the skill.

IF A SKILL APPLIES TO YOUR TASK, YOU DO NOT HAVE A CHOICE. YOU MUST USE IT.

This is not negotiable. This is not optional. You cannot rationalize your way out of this.
</EXTREMELY_IMPORTANT>

## Architecture Report

Kilo Code provides native tools that directly implement Superpowers capabilities:

| Capability | Kilo Code Native Tool |
|------------|----------------------|
| Skill Loader | `Skill` tool - loads skill content directly into context |
| Task Tracker | `TodoWrite` tool - creates and manages todo items |
| Subagent Dispatcher | `Task` tool - dispatches subagents for parallel work |
| Shell Executor | `Bash` tool - executes shell commands |
| Visual Companion | Playwright browser tools - `kilo-playwright_browser_*` functions |

### Native Tool Capabilities

- **Skill tool**: Invoke any skill by name - loads SKILL.md content automatically
- **TodoWrite tool**: Create, update, complete todos with structured API
- **Task tool**: Dispatch subagents for parallel independent work
- **Bash tool**: Run shell commands with configurable timeout
- **File tools**: `read`, `write`, `edit`, `glob`, `grep` - full filesystem access
- **Playwright tools**: Navigate, click, type, screenshot, evaluate JS in browser

## Instruction Priority

Superpowers skills override default system prompt behavior, but **user instructions always take precedence**:

1. **User's direct chat requests** — highest priority
2. **Superpowers skills** — override default system behavior where they conflict
3. **Default system prompt** — lowest priority

If you receive conflicting instructions, follow the user's explicit requests. The user is in control.

## How to Access Skills

Skills are loaded using the native **Skill tool**. When you invoke a skill, its full content is loaded into the conversation context automatically.

### Tool Mapping for Kilo Code

| Abstract Capability | Kilo Code Native Equivalent |
|---------------------|------------------------------|
| Task Tracker | `TodoWrite` tool |
| Subagent Dispatcher | `Task` tool |
| Skill Loader | `Skill` tool |
| Shell Executor | `Bash` tool |
| Visual Companion | Playwright browser tools |

# Using Skills

## The Rule

**Invoke relevant or requested skills BEFORE any response or action.** Even a 1% chance a skill might apply means that you should invoke the skill to check. If an invoked skill turns out to be wrong for the situation, you don't need to use it.

```dot
digraph skill_flow {
    "User message received" [shape=doublecircle];
    "About to EnterPlanMode?" [shape=doublecircle];
    "Already brainstormed?" [shape=diamond];
    "Invoke brainstorming skill" [shape=box];
    "Might any skill apply?" [shape=diamond];
    "Invoke Skill tool" [shape=box];
    "Announce: 'Using [skill] to [purpose]'" [shape=box];
    "Has checklist?" [shape=diamond];
    "Create TodoWrite todo per item" [shape=box];
    "Follow skill exactly" [shape=box];
    "Respond (including clarifications)" [shape=doublecircle];

    "About to EnterPlanMode?" -> "Already brainstormed?";
    "Already brainstormed?" -> "Invoke brainstorming skill" [label="no"];
    "Already brainstormed?" -> "Might any skill apply?" [label="yes"];
    "Invoke brainstorming skill" -> "Might any skill apply?";

    "User message received" -> "Might any skill apply?";
    "Might any skill apply?" -> "Invoke Skill tool" [label="yes, even 1%"];
    "Might any skill apply?" -> "Respond (including clarifications)" [label="definitely not"];
    "Invoke Skill tool" -> "Announce: 'Using [skill] to [purpose]'";
    "Announce: 'Using [skill] to [purpose]'" -> "Has checklist?";
    "Has checklist?" -> "Create TodoWrite todo per item" [label="yes"];
    "Has checklist?" -> "Follow skill exactly" [label="no"];
    "Create TodoWrite todo per item" -> "Follow skill exactly";
}
```

## Examples of Skill Loading

### Basic Skill Invocation
```
User: Help me debug this error.
→ Invoke Skill tool with name="systematic-debugging"
→ Announce: "Using systematic-debugging to investigate this issue"
→ Follow skill instructions exactly
```

### Skill with Todo Creation
```
User: Implement the new feature.
→ Invoke Skill tool with name="brainstorming"
→ Announce: "Using brainstorming to explore requirements"
→ Create TodoWrite todos for each exploration step
→ Follow skill exactly
```

### Multiple Skills
```
User: Let's build a new API endpoint.
→ Invoke Skill tool with name="brainstorming" (process skill first)
→ After brainstorming, invoke Skill tool with name="implementation-skill"
→ Follow implementation skill guidance
```

## Red Flags

These thoughts mean STOP—you're rationalizing:

| Thought | Reality |
|---------|---------|
| "This is just a simple question" | Questions are tasks. Check for skills. |
| "I need more context first" | Skill check comes BEFORE clarifying questions. |
| "Let me explore the codebase first" | Skills tell you HOW to explore. Check first. |
| "I can check git/files quickly" | Files lack conversation context. Check for skills. |
| "Let me gather information first" | Skills tell you HOW to gather information. |
| "This doesn't need a formal skill" | If a skill exists, use it. |
| "I remember this skill" | Skills evolve. Read current version. |
| "This doesn't count as a task" | Action = task. Check for skills. |
| "The skill is overkill" | Simple things become complex. Use it. |
| "I'll just do this one thing first" | Check BEFORE doing anything. |
| "This feels productive" | Undisciplined action wastes time. Skills prevent this. |
| "I know what that means" | Knowing the concept ≠ using the skill. Invoke it. |

## Skill Priority

When multiple skills could apply, use this order:

1. **Process skills first** (brainstorming, debugging) - these determine HOW to approach the task
2. **Implementation skills second** (frontend-design, mcp-builder) - these guide execution

"Let's build X" → brainstorming first, then implementation skills.
"Fix this bug" → debugging first, then domain-specific skills.

## Skill Types

**Rigid** (TDD, debugging): Follow exactly. Don't adapt away discipline.

**Flexible** (patterns): Adapt principles to context.

The skill itself tells you which.

## User Instructions

Instructions say WHAT, not HOW. "Add X" or "Fix Y" doesn't mean skip workflows.

---

## Key Differences from Cline Adapter

| Feature | Cline Adapter | Kilo Code Adapter |
|---------|---------------|-------------------|
| Skill Loader | Manual file reading with `read` tool | Native `Skill` tool - auto-loads SKILL.md |
| Task Tracker | Markdown file (`task.md`) with edit tool | Native `TodoWrite` tool |
| Subagent Dispatcher | User must manually open new chat | Native `Task` tool |
| Tool invocation | Requires explicit tool selection | Native tool integration |
