# AGENTS.md

This file describes the AI agents and skills available in this repository. It is read by GitHub Copilot and other AI assistants to understand available workflows.

---

## Overview

This repository contains reusable OpenSpec skills and QE skills for the **Q-SYS iOS Viewer** team. Skills are stored in `.github/skills/` and are invoked via slash commands in GitHub Copilot Chat.

---

## Skill Index

### QE Skills (Q-SYS iOS Viewer)

These skills generate manual testing artifacts for Q-SYS iOS Viewer features. They are **composable** — each skill does one thing well and the outputs chain together.

| Slash Command | Skill | Description |
|---|---|---|
| `/qe:test-plan` | [`qe-test-plan`](.github/skills/qe-test-plan/SKILL.md) | Generate a structured QE Test Plan |
| `/qe:test-cases` | [`qe-test-cases`](.github/skills/qe-test-cases/SKILL.md) | Generate detailed manual test cases |
| `/qe:jira-stories` | [`qe-jira-stories`](.github/skills/qe-jira-stories/SKILL.md) | Generate a Jira story with ACs and sub-tasks |

#### Composable Workflow

```
Feature Description or Spec
        │
        ▼
  /qe:test-plan        →   test-plan.md
        │
        ▼
  /qe:test-cases       →   test-cases.md
        │
        ▼
  /qe:jira-stories     →   jira-story.md
```

Each skill can be run independently or as part of the full pipeline. Later skills read earlier outputs for context when available.

---

### OpenSpec Skills

These skills implement the OpenSpec change-driven development workflow.

| Slash Command | Skill | Description |
|---|---|---|
| `/opsx:propose` | [`openspec-propose`](.github/skills/openspec-propose/SKILL.md) | Propose a new change — create all artifacts in one step |
| `/opsx:apply` | [`openspec-apply-change`](.github/skills/openspec-apply-change/SKILL.md) | Implement tasks from an OpenSpec change |
| `/opsx:archive` | [`openspec-archive-change`](.github/skills/openspec-archive-change/SKILL.md) | Archive a completed change |
| `/opsx:explore` | [`openspec-explore`](.github/skills/openspec-explore/SKILL.md) | Explore and think through ideas before proposing a change |

#### OpenSpec + QE Workflow

```
/opsx:explore        →   Clarify requirements
/opsx:propose        →   proposal.md, design.md, tasks.md
/qe:test-plan        →   test-plan.md (from proposal/spec)
/qe:test-cases       →   test-cases.md
/qe:jira-stories     →   jira-story.md
/opsx:apply          →   Implementation
/opsx:archive        →   Archive the change
```

---

## Shared Templates

Reusable templates are stored in `templates/` at the repository root:

| Template | Used By |
|---|---|
| [`templates/test-plan.md`](templates/test-plan.md) | `qe-test-plan` skill |
| [`templates/test-case.md`](templates/test-case.md) | `qe-test-cases` skill |
| [`templates/jira-story.md`](templates/jira-story.md) | `qe-jira-stories` skill |

---

## Example Outputs

Worked examples are stored in `examples/` at the repository root, grouped by feature:

### Device Discovery and Connection

| Example | Path |
|---|---|
| Test Plan | [`examples/device-discovery/test-plan.md`](examples/device-discovery/test-plan.md) |
| Test Cases | [`examples/device-discovery/test-cases.md`](examples/device-discovery/test-cases.md) |
| Jira Story | [`examples/device-discovery/jira-story.md`](examples/device-discovery/jira-story.md) |

### Popup Visibility and Layer Transition Behavior

| Example | Path |
|---|---|
| Test Plan | [`examples/popup-visibility/test-plan.md`](examples/popup-visibility/test-plan.md) |
| Test Cases | [`examples/popup-visibility/test-cases.md`](examples/popup-visibility/test-cases.md) |
| Jira Story | [`examples/popup-visibility/jira-story.md`](examples/popup-visibility/jira-story.md) |

---

## Conventions

- All QE artifact outputs are **markdown-based** and **human-readable**
- Test case IDs use format `TC-<FEATURE_CODE>-<NNN>` (e.g., `TC-DD-001`)
- Story summaries follow `[QE] <Feature> — <Story Type>` (e.g., `[QE] Device Discovery — Test Execution`)
- Priority scale: **P1** (blocker) | **P2** (significant) | **P3** (minor)
- Status icons: ⬜ Not Run | ✅ Pass | ❌ Fail | ⏭ Skipped | 🔄 Blocked
- All skills target **manual testing** on **physical iOS devices**
- Minimum iOS version: **iOS 16**; both **iPad** and **iPhone** must be covered

---

## Repository Structure

```
.github/
├── prompts/
│   ├── opsx-apply.prompt.md
│   ├── opsx-archive.prompt.md
│   ├── opsx-explore.prompt.md
│   ├── opsx-propose.prompt.md
│   ├── qe-jira-stories.prompt.md
│   ├── qe-test-cases.prompt.md
│   └── qe-test-plan.prompt.md
└── skills/
    ├── openspec-apply-change/SKILL.md
    ├── openspec-archive-change/SKILL.md
    ├── openspec-explore/SKILL.md
    ├── openspec-propose/SKILL.md
    ├── qe-jira-stories/SKILL.md
    ├── qe-test-cases/SKILL.md
    └── qe-test-plan/SKILL.md
examples/
├── device-discovery/
│   ├── jira-story.md
│   ├── test-cases.md
│   └── test-plan.md
└── popup-visibility/
    ├── jira-story.md
    ├── test-cases.md
    └── test-plan.md
templates/
├── jira-story.md
├── test-case.md
└── test-plan.md
docs/
│   └── README.md
openspec/
├── config.yaml
├── changes/
│   └── archive/
└── specs/
AGENTS.md
```
