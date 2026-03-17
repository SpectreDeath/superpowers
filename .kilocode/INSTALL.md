# Installing Superpowers for Kilo Code (Windows)

## Prerequisites

- [Kilo Code](https://kilo.ai/) installed on your Windows machine
- Git installed
- Windows PowerShell (Administrator privileges may be required for symlinks)

## Installation Steps

### 1. Clone Superpowers

Open PowerShell and clone the repository:

```powershell
# Example: cloning to your documents folder
cd ~\Documents
git clone https://github.com/obra/superpowers.git
```

### 2. Create Symbolic Links

Kilo Code needs to find the bootstrap file and skills directory. Create symbolic links using PowerShell.

**Open PowerShell as Administrator** and run:

```powershell
# Create the bootstrap symlink (Kilo Code will auto-detect .kilocode/bootstrap.md)
New-Item -ItemType SymbolicLink -Path "D:\GitHub\superpowers\.kilocode" -Target "D:\GitHub\superpowers\.kilocode" -Force

# Or if you prefer a global installation, link to your Kilo Code config directory:
# New-Item -ItemType SymbolicLink -Path "~\AppData\Local\KiloCode\rules\superpowers.md" -Target "D:\GitHub\superpowers\.kilocode\bootstrap.md"
```

**Note:** The `.kilocode` directory should be placed at the root of your project or workspace. Kilo Code automatically looks for `.kilocode/bootstrap.md` in the working directory.

### 3. Verify Skills Location

Ensure the skills directory is accessible:

```
D:\GitHub\superpowers\.kilocode\
├── bootstrap.md          # Main bootstrap file
├── CAPABILITIES.md       # Capability mapping
├── INSTALL.md            # This file
└── skills\               # Skills directory
    └── ...               # Individual skill folders
```

### 4. Restart Kilo Code

Restart Kilo Code to detect the new bootstrap file and skills.

## Verification

Verify the installation by:

1. Starting a new Kilo Code session
2. Asking: "Do you have superpowers loaded?"
3. Kilo Code should respond confirming it loaded the bootstrap

## Usage

### Finding Skills

Skills are loaded on-demand. When you ask Kilo Code to perform a task, the `bootstrap.md` instructions command it to check for relevant skills.

You can explicitly request a skill:

```
User: Use the brainstorming skill to help me design this feature.
→ Kilo Code invokes Skill tool with name="brainstorming"
→ Loads the brainstorming skill content
→ Follows skill instructions
```

### Key Advantages Over Cline

| Feature | Kilo Code | Cline (requires) |
|---------|-----------|------------------|
| Skill Loading | Native `Skill` tool | Manual `read` of SKILL.md |
| Task Tracking | Native `TodoWrite` tool | Manual markdown file |
| Subagent Dispatch | Native `Task` tool | User manually opens new chat |
| Bootstrap | Auto-detected in `.kilocode/` | Requires symlink to rules |

### Creating Your Own Skills

Add custom skills to the `.kilocode/skills/` directory:

```powershell
New-Item -ItemType Directory -Path "D:\GitHub\superpowers\.kilocode\skills\my-skill"
```

Create `SKILL.md` inside:

```markdown
---
name: my-skill
description: Use when [condition] - [what it does]
---

# My Skill

[Your skill content here]
```

## Updating

To get the latest skills:

```powershell
cd D:\GitHub\superpowers
git pull
```

## Troubleshooting

### Skills not found

1. Verify the `.kilocode` directory exists in your workspace
2. Check that `bootstrap.md` is present
3. Ensure skills directories contain valid `SKILL.md` files

### Bootstrap not loading

1. Confirm Kilo Code's working directory points to the project
2. Verify the `.kilocode` folder is at the project root
3. Restart Kilo Code after adding the files

### Symlink errors

If you receive permission errors when creating symlinks:
- Run PowerShell as Administrator
- Or copy the files instead of symlinking

## Project Structure

After installation, your project should look like:

```
your-project/
├── .kilocode/
│   ├── bootstrap.md
│   ├── CAPABILITIES.md
│   ├── INSTALL.md
│   └── skills/
│       ├── brainstorming/
│       │   └── SKILL.md
│       ├── systematic-debugging/
│       │   └── SKILL.md
│       └── ... (other skills)
├── src/
├── package.json
└── ...
```

## Integration with Universal Skills

Kilo Code can also access universal skills from `universal/skills/`. To use them:

1. Create a symlink or copy from `universal/skills/` to `.kilocode/skills/`
2. Or modify the skill path references in bootstrap.md

```powershell
# Link universal skills to Kilo Code skills directory
New-Item -ItemType SymbolicLink -Path "D:\GitHub\superpowers\.kilocode\skills\universal" -Target "D:\GitHub\superpowers\universal\skills"
```
