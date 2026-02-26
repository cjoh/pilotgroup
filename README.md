# Pilot Group

A Claude Code plugin for simulating diverse audience reactions to any content.

Generate named cohorts of realistic personas, run them through your content (documents, URLs, or experiences that don't exist yet), and get comparative satisfaction reports with trend tracking across runs.

## Install

### Claude Code (plugin)

```
claude plugins:add-marketplace https://github.com/cjoh/pilotgroup
claude plugins:install pilot-group@pilotgroup
```

### Codex (skill)

Copy the `codex-skills/pilot-group/` directory into your project's `.codex/skills/` folder:

```
cp -r codex-skills/pilot-group/ .codex/skills/pilot-group/
```

## Usage

```
/pilot                          # Full interactive flow
/pilot create students 5        # Create a 5-persona cohort
/pilot run students README.md   # Simulate against a file
/pilot run students https://example.com  # Against a URL
/pilot report students          # Regenerate latest report
/pilot cohorts                  # List all cohorts
```

If only one cohort exists, the name is optional:

```
/pilot run README.md            # Uses the only cohort
/pilot report                   # Reports on the only cohort
```

## How It Works

### 1. Create personas

`/pilot create <name> [count]` builds a cohort of diverse personas. Each is presented individually for your approval -- you can approve, modify, regenerate, or skip.

Personas are assembled from an archetype library spanning engagement levels (enthusiast, skeptic, passive, overachiever), experience levels (expert, novice, adjacent, career-changer), processing styles (analyzer, storyteller, doer, connector), and access needs (non-native speaker, time-constrained, accessibility, low-bandwidth). Every persona gets at least one genuine friction point.

### 2. Run simulations

`/pilot run <name> <target>` launches parallel subagents -- one per persona -- that experience the content in character and produce scored, cited reactions.

Targets can be:

- **Local files** -- markdown, slides, documents
- **URLs** -- web pages, fetched and analyzed
- **Descriptions** -- "a 2-hour conflict resolution workshop" -- personas react to the concept before it's built

### 3. Get reports

Scores, highlights, prioritized pain points, per-persona detail, cross-cohort patterns, and recommended next steps. Output as both markdown and a self-contained HTML file that opens in your browser.

Run the same cohort against updated content to get **trend tracking** -- score deltas and arrows showing what improved.

## Multiple Cohorts

Maintain several cohorts and run the same content through different audiences:

```
/pilot create students 5
/pilot create reviewers 3
/pilot create executives 4

/pilot run students  workshop-slides.md
/pilot run reviewers workshop-slides.md
/pilot run executives workshop-slides.md
```

Each cohort has isolated score history and run directories.

## State

All data lives in `.pilot-group/` in your working directory:

```
.pilot-group/
├── cohorts/
│   ├── students.json
│   └── reviewers.json
└── runs/
    ├── students/
    │   ├── 20260220-143052-experience.json
    │   ├── 20260220-143052-report.md
    │   └── 20260220-143052-report.html
    └── reviewers/
        └── ...
```

Add `.pilot-group/` to your `.gitignore`.

## Project Structure

```
pilotgroup/
├── plugins/pilot-group/          # Claude Code plugin (primary)
│   ├── commands/pilot.md         # /pilot command router
│   ├── skills/pilot-group/
│   │   ├── SKILL.md              # Core orchestration logic
│   │   ├── references/
│   │   │   └── persona-archetypes.md
│   │   └── templates/
│   │       └── report.html
│   ├── agents/
│   │   └── experience-simulator.md
│   └── CLAUDE.md
└── codex-skills/pilot-group/     # Codex-compatible copy
```

## License

MIT
