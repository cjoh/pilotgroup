---
name: pilot
description: Generate persona cohorts, simulate their experience with content, and produce satisfaction reports
argument-hint: "[create <name> [N] | run <name> <target> | report <name> | cohorts]"
---

# /pilot Command

Route to the pilot-group skill.

## Arguments

The user invoked this command with: $ARGUMENTS

## Execution

1. Read the skill file at `skills/pilot-group/SKILL.md` relative to this plugin's install directory.
2. Parse $ARGUMENTS to determine subcommand (create, run, report, cohorts, or full flow if empty).
3. Load additional files **only as needed by the subcommand**:
   - **create**: Read `skills/pilot-group/references/persona-archetypes.md`
   - **run**: Read `agents/experience-simulator.md` (needed for subagent prompts)
   - **report**: Read `skills/pilot-group/templates/report.html`
   - **cohorts**: No additional files needed
   - **full flow**: Load files as each phase is reached
