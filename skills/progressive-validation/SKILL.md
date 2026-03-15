---
name: progressive-validation
description: Use to verify changes through a structured "water filtration" process, moving from coarse environment checks to fine integration validation.
---

# Progressive Validation

## Overview

Validating complex changes all at once leads to overlooked errors and "ghost bugs" that disappear and reappear. Using a coarse-to-fine filtration process ensures each layer of the system is sound before testing the next.

**Core principle:** Like water filtration, you move from coarse (environment) to fine (integration). Each layer protects the next. You cannot skip layers.

**Violating the sequence of this process is wasting time on false positives.**

## The Iron Law

```
NEVER TEST A FINE FILTER UNTIL THE COARSE FILTERS ARE CLEAR
```

If you haven't verified the environment, your test failure might be a path issue, not a logic bug. If you haven't verified syntax, your unit test might be running stale code.

## When to Use

Use during ANY development or debugging task:
- After making a code change
- Before claiming a feature is "done"
- When a test fails unexpectedly
- When an agent reports success but you haven't verified it

**Use this ESPECIALLY when:**
- Working in unfamiliar parts of the codebase
- Dealing with complex configurations or environment variables
- Performing multi-file refactors
- Integrating new components into an existing system

## The 4 Layers of Filtration

You MUST clear each filter before proceeding to the fine-grained validation of the next.

### Layer 1: Environment Filter (Coarse)

**Goal:** Ensure the context for the work is correct.

1. **Right Directory:** Are you in the correct repository and absolute path?
2. **Right Project:** Is the current workspace/corpus the one you intended to modify?
3. **Existence Check:** Do the files you modified actually exist on disk with the expected names?
4. **Tool Availability:** Are the necessary compilers, runtimes, or CLI tools (e.g., `npm`, `pytest`, `git`) available in this environment?

**Success Criteria:** Environment is stable and tools are ready.

---

### Layer 2: Syntax Filter

**Goal:** Ensure the code is structurally sound without executing it.

1. **Parses:** Does the code pass basic syntax checks? (Linter, compiler, or `python -m py_compile`).
2. **Imports Resolve:** Are all new and existing imports valid? No circular dependencies or missing modules.
3. **Pattern Match:** Does the code follow the expected project patterns? (Naming conventions, file structure).
4. **Static Analysis:** Run the linter or type checker (e.g., `mypy`, `eslint`).

**Success Criteria:** Code is "legally" valid within the language and project standards.

---

### Layer 3: Unit Filter

**Goal:** Ensure the component works in isolation.

1. **Import Test:** Can the module be imported into a running process without crashing?
2. **Smoke Test:** Does the simplest possible execution path work?
3. **Correct Structure:** Does the runtime object (class, function, variable) match the expected interface?
4. **Isolated Logic:** Do the unit tests for this specific component pass?

**Success Criteria:** The individual component behaves as expected in a "lab" setting.

---

### Layer 4: Integration Filter (Fine)

**Goal:** Ensure the part works within the whole.

1. **Registered:** Is the new component properly registered in the system (e.g., `init.py`, dependency injection, config files)?
2. **Discoverable:** Can other parts of the system find and use this component?
3. **Cross-Component Flow:** Run integration tests that span multiple modules.
4. **Committed/Persisted:** Are the changes staged or committed? Is the system state updated (database migrations, cache cleared)?

**Success Criteria:** The system is whole and functional with the new changes.

## Red Flags - STOP and Filter Up

If you catch yourself thinking:
- "The test failed, let me change the logic" (Did you check if the file even exists in the right folder?)
- "It works on my machine" (Environment filter failure)
- "I'll fix the imports after I see if the test passes" (Syntax filter failure)
- "The agent said it was done, I'll trust it" (Missing all layers of filtration)
- **Debugging a logic error while syntax warnings are ignored.**

**ALL of these mean: Stop. Go back to a coarser filter.**

## Integration with Other Skills

- **superpowers:systematic-debugging**: Use Progressive Validation during Phase 1 (Root Cause) and Phase 4 (Verification) to avoid chasing symptoms caused by environment or syntax issues.
- **superpowers:verification-before-completion**: This skill provides the *method* for verification. When `verification-before-completion` asks for evidence, usage of these 4 layers provides the hierarchy of that evidence.

## Real-World Impact

- **Reduces "Ghost Bugs"**: Stops you from debugging logic when the real issue is a missing environment variable.
- **Speeds up Feedback**: Syntax and Environment checks are 10x faster than running full integration suites.
- **Increases Confidence**: Moving through the layers builds a foundation of certainty.
