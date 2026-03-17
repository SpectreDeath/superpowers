# Capabilities Mapping: Universal to Kilo Code

This document maps each abstract capability from `universal/CAPABILITIES.md` to Kilo Code's native equivalents.

## 1. Task Tracker

**Purpose:** Create, manage, and update a checklist of tasks during multi-step implementations or plan executions.

**Kilo Code Native Equivalent:** `TodoWrite` tool

Kilo Code provides a dedicated `TodoWrite` tool for task management:

```json
{
  "tool": "TodoWrite",
  "parameters": {
    "content": "Implement user authentication feature"
  }
}
```

**Usage:**
- Create todos: `TodoWrite` with the task content
- Update todos: Use `TodoWrite` to modify existing items
- Mark complete: Use `TodoWrite` to update status

**Why this is better than Cline:** Cline requires creating and editing a markdown file manually. Kilo Code's native tool provides structured todo management without file operations.

---

## 2. Subagent Dispatcher

**Purpose:** Delegate independent tasks to specialized subagents with fresh, isolated contexts.

**Kilo Code Native Equivalent:** `Task` tool

Kilo Code provides a native `Task` tool for dispatching subagents:

```json
{
  "tool": "Task",
  "parameters": {
    "description": "Run tests on the authentication module",
    "additionalContext": "Focus on testing login, logout, and session management"
  }
}
```

**Usage:**
- Dispatch parallel agents for independent work
- Provide fresh context for isolated tasks
- Use for parallel implementation or research

**Why this is better than Cline:** Cline requires the user to manually open a new chat/task. Kilo Code's native `Task` tool dispatches subagents directly from the current session.

---

## 3. Skill Loader

**Purpose:** Discover, load, and activate skill instructions into the current session context.

**Kilo Code Native Equivalent:** `Skill` tool

Kilo Code provides a native `Skill` tool that loads skill content automatically:

```json
{
  "tool": "Skill",
  "parameters": {
    "name": "brainstorming"
  }
}
```

**Behavior:**
- Loads the full `SKILL.md` content from `.kilocode/skills/<skill-name>/`
- Injects skill instructions into the conversation context
- Automatically announces skill usage

**Skill Locations:**
- Kilo Code skills: `D:\GitHub\superpowers\.kilocode\skills\`
- Universal skills: `D:\GitHub\superpowers\universal\skills\`

**Why this is better than Cline:** Cline requires manually reading the skill file with the `read` tool. Kilo Code's native `Skill` tool automatically loads and activates skill content.

---

## 4. Shell Executor

**Purpose:** Execute commands continuously during development.

**Kilo Code Native Equivalent:** `Bash` tool

Kilo Code provides a native `Bash` tool:

```json
{
  "tool": "Bash",
  "parameters": {
    "command": "npm run build",
    "description": "Build the project"
  }
}
```

**Features:**
- Persistent shell session
- Configurable timeout
- Works on Windows (PowerShell/CMD), Linux, macOS

---

## 5. Visual Companion

**Purpose:** Provide visual feedback, mockups, or diagrams to the user in a browser natively.

**Kilo Code Native Equivalent:** Playwright browser tools

Kilo Code provides native browser automation via Playwright:

| Tool | Purpose |
|------|---------|
| `kilo-playwright_browser_navigate` | Navigate to URLs |
| `kilo-playwright_browser_click` | Click elements |
| `kilo-playwright_browser_type` | Type text into fields |
| `kilo-playwright_browser_take_screenshot` | Capture screenshots |
| `kilo-playwright_browser_snapshot` | Get accessibility snapshot |
| `kilo-playwright_browser_evaluate` | Run JavaScript |
| `kilo-playwright_browser_network_requests` | Capture network traffic |

**Example:**
```json
{
  "tool": "kilo-playwright_browser_navigate",
  "parameters": {
    "url": "http://localhost:3000"
  }
}
```

---

## Summary Table

| Universal Capability | Kilo Code Native Tool | Cline Equivalent (Manual) |
|---------------------|----------------------|---------------------------|
| Task Tracker | `TodoWrite` | Create/edit `task.md` file |
| Subagent Dispatcher | `Task` | User opens new chat manually |
| Skill Loader | `Skill` | `read` tool on SKILL.md |
| Shell Executor | `Bash` | `bash` tool |
| Visual Companion | Playwright browser tools | N/A |

---

## File Tool Mapping

Kilo Code also provides native file tools:

| Tool | Purpose |
|------|---------|
| `read` | Read file contents |
| `write` | Write/overwrite files |
| `edit` | Edit specific strings in files |
| `glob` | Find files by pattern |
| `grep` | Search file contents |

These are equivalent to Cline's file tools and can be used for:
- Reading skill files when `Skill` tool isn't available
- Reading project files
- Editing configuration or source code
