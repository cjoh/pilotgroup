# Pilot Group Plugin

A Claude Code plugin for generating diverse persona cohorts, running them through any content, and producing comparative satisfaction reports.

## Architecture

- **Command**: `/pilot` — entry point routing subcommands (create, run, report, cohorts)
- **Skill**: `pilot-group/SKILL.md` — core logic for cohort generation, simulation orchestration, and reporting
- **Agent**: `experience-simulator.md` — subagent launched per-persona to simulate content experience

## State Management

All state lives in `.pilot-group/` in the user's working directory. Cohorts are named and stored separately:

```
.pilot-group/
├── cohorts/
│   ├── students.json                    # Named cohort: persona definitions + score history
│   └── curriculum-reviewers.json        # Another named cohort
└── runs/
    ├── students/                        # Runs scoped to cohort
    │   ├── {timestamp}-experience.json
    │   ├── {timestamp}-report.md
    │   └── {timestamp}-report.html
    └── curriculum-reviewers/
        ├── {timestamp}-experience.json
        ├── {timestamp}-report.md
        └── {timestamp}-report.html
```

- Each cohort file stores `prior_scores[]` per persona for trend tracking
- Runs are scoped to cohort name so history stays organized
- The `/pilot` command creates directories on first use
- Add `.pilot-group/` to `.gitignore`

## Key Behaviors

- Multiple named cohorts can coexist (e.g., "students" and "curriculum-reviewers")
- Personas are presented **one at a time** during creation for individual approval
- Experience simulations run as **parallel Task subagents** (one per persona)
- Reports include **trend comparison** when prior runs exist for that cohort
- HTML reports are **self-contained** (no external dependencies) and auto-opened
