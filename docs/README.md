# Q-SYS iOS Viewer QE Skills

Reusable AI skills and markdown templates for manual QE testing of the **Q-SYS iOS Viewer** app.

---

## What this repository provides

| Artifact type | Location | Purpose |
|---|---|---|
| Skill definitions | `.github/skills/` | Instruction sets invoked by GitHub Copilot slash commands |
| Prompt files | `.github/prompts/` | Slash command bindings for Copilot Chat |
| Reusable templates | `templates/` | Fill-in-the-blank markdown structures |
| Worked examples | `examples/` | Reference outputs for two real features |
| OpenSpec workflow | `openspec/` | Change-driven development configuration |

---

## Quick start

### Generate QE artifacts for a new feature

```
/qe:test-plan   Device Discovery and Connection
/qe:test-cases
/qe:jira-stories
```

Each skill reads the previous skill's output as context automatically when both are in the same directory.

---

## Slash commands

| Command | What it generates |
|---|---|
| `/qe:test-plan <feature>` | Structured test plan — scope, scenarios, risks, environment |
| `/qe:test-cases <feature>` | Step-by-step manual test cases with IDs, priorities, and execution fields |
| `/qe:regression-scope <feature>` | Regression impact analysis — subsystem risk map, smoke suite, validation priorities |
| `/qe:jira-stories <feature>` | Jira story with metadata block, ACs, sub-tasks, and Definition of Done |
| `/opsx:propose <name>` | OpenSpec change — proposal, design, tasks |
| `/opsx:apply` | Implement tasks from an active OpenSpec change |
| `/opsx:archive` | Archive a completed change |
| `/opsx:explore` | Thinking-partner mode for requirements exploration |

---

## Composable QE pipeline

```
Feature description / spec / OpenSpec proposal
               │
               ▼
        /qe:test-plan          →  test-plan.md
               │                  (scope, scenarios, risks)
               ▼
        /qe:test-cases         →  test-cases.md
               │                  (TC-FEAT-NNN, steps, expected results)
               ▼
        /qe:regression-scope   →  regression-scope.md
               │                  (subsystem risk map, smoke suite, validation priorities)
               ▼
        /qe:jira-stories       →  jira-story.md
                                  (metadata, ACs, sub-tasks, DoD)
```

Each skill is independently runnable. Later skills in the pipeline automatically consume earlier outputs when present in the same working directory.

---

## Templates

Located in [`templates/`](../templates/):

| File | Used by |
|---|---|
| [`templates/test-plan.md`](../templates/test-plan.md) | `qe-test-plan` — fill-in structure for test plans |
| [`templates/test-case.md`](../templates/test-case.md) | `qe-test-cases` — fill-in structure for individual test cases |
| [`templates/regression-scope.md`](../templates/regression-scope.md) | `qe-regression-scope` — fill-in structure for regression scope analysis |
| [`templates/jira-story.md`](../templates/jira-story.md) | `qe-jira-stories` — fill-in structure for Jira stories |

Templates use `{{PLACEHOLDER}}` syntax. All placeholders are described in the template file headers.

---

## Examples

Located in [`examples/`](../examples/), grouped by feature:

| Feature | Test Plan | Test Cases | Regression Scope | Jira Story |
|---|---|---|---|---|
| Device Discovery | [`test-plan.md`](../examples/device-discovery/test-plan.md) | [`test-cases.md`](../examples/device-discovery/test-cases.md) | [`regression-scope.md`](../examples/device-discovery/regression-scope.md) | [`jira-story.md`](../examples/device-discovery/jira-story.md) |
| Popup Visibility & Layer Transitions | [`test-plan.md`](../examples/popup-visibility/test-plan.md) | [`test-cases.md`](../examples/popup-visibility/test-cases.md) | [`regression-scope.md`](../examples/popup-visibility/regression-scope.md) | [`jira-story.md`](../examples/popup-visibility/jira-story.md) |
| Session Recovery & Reconnect | [`test-plan.md`](../examples/session-recovery/test-plan.md) | [`test-cases.md`](../examples/session-recovery/test-cases.md) | [`regression-scope.md`](../examples/session-recovery/regression-scope.md) | [`jira-story.md`](../examples/session-recovery/jira-story.md) |

---

## Conventions

> For the full conventions reference see [conventions.md](conventions.md) — covers test writing rules, priority definitions, subsystem taxonomy, story point estimation, and template syntax.

| Convention | Value |
|---|---|
| Test case ID format | `TC-<FEATURE_CODE>-<NNN>` (e.g., `TC-DD-001`) |
| Smoke test ID format | `SMOKE-<FEATURE_CODE>-<NNN>` (e.g., `SMOKE-DD-001`) |
| Story summary format | `[QE] <Feature> — <Story Type>` |
| Priority scale | P1 blocker · P2 significant · P3 minor |
| Status icons | ⬜ Not Run · ✅ Pass · ❌ Fail · ⏭ Skipped · 🔄 Blocked |
| Target platform | Physical iOS device (iPad + iPhone) |
| Minimum iOS version | iOS 16 |
| Testing methodology | Manual |

---

## Skills reference

| Skill | File | Description |
|---|---|---|
| `qe-test-plan` | [`.github/skills/qe-test-plan/SKILL.md`](../.github/skills/qe-test-plan/SKILL.md) | Generates a structured QE Test Plan |
| `qe-test-cases` | [`.github/skills/qe-test-cases/SKILL.md`](../.github/skills/qe-test-cases/SKILL.md) | Generates detailed manual test cases |
| `qe-regression-scope` | [`.github/skills/qe-regression-scope/SKILL.md`](../.github/skills/qe-regression-scope/SKILL.md) | Analyzes regression impact across 11 subsystems with smoke suite |
| `qe-jira-stories` | [`.github/skills/qe-jira-stories/SKILL.md`](../.github/skills/qe-jira-stories/SKILL.md) | Generates a Jira story with ACs and sub-tasks |
| `openspec-propose` | [`.github/skills/openspec-propose/SKILL.md`](../.github/skills/openspec-propose/SKILL.md) | Proposes a new OpenSpec change |
| `openspec-apply-change` | [`.github/skills/openspec-apply-change/SKILL.md`](../.github/skills/openspec-apply-change/SKILL.md) | Implements an OpenSpec change |
| `openspec-archive-change` | [`.github/skills/openspec-archive-change/SKILL.md`](../.github/skills/openspec-archive-change/SKILL.md) | Archives a completed change |
| `openspec-explore` | [`.github/skills/openspec-explore/SKILL.md`](../.github/skills/openspec-explore/SKILL.md) | Explore mode — thinking partner |

---

## OpenSpec integration

The `openspec/` directory holds the OpenSpec configuration and change workspace.

```
openspec/
├── config.yaml        ← schema and project context
├── changes/           ← active and archived changes
│   └── archive/
└── specs/             ← canonical specs by capability
```

To use QE skills within an OpenSpec workflow:

1. Run `/opsx:propose <change-name>` to generate proposal, design, and tasks
2. Run `/qe:test-plan` to generate a test plan from the proposal
3. Run `/qe:test-cases` to generate manual test cases
4. Run `/qe:regression-scope` to identify regression risk and a smoke suite
5. Run `/qe:jira-stories` to complete the QE artifact set
6. Run `/opsx:apply` to implement the change tasks
7. Run `/opsx:archive` when the change and QE sign-off are complete
