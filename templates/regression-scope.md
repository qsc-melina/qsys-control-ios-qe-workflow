# Regression Scope: {{FEATURE_NAME}}

<!-- 
  TEMPLATE INSTRUCTIONS (remove before publishing):
  - Replace all {{PLACEHOLDER}} values
  - Risk levels: HIGH | MED | LOW
  - Smoke IDs: SMOKE-<FEATURE_CODE>-<NNN>
  - Subsystem IDs: SYS-DISC | SYS-CONN | SYS-UCI | SYS-POPUP | SYS-NAV |
                   SYS-CTRL | SYS-NET | SYS-STORE | SYS-A11Y | SYS-ALERT | SYS-ORIENT
  - Link to TC IDs from test-cases.md wherever smoke tests map to existing cases
-->

**Feature**: {{FEATURE_NAME}}  
**Feature Code**: {{FEATURE_CODE}}  
**Platform**: Q-SYS iOS Viewer  
**Test Plan**: [test-plan.md](./test-plan.md)  
**Test Cases**: [test-cases.md](./test-cases.md)  
**Author**: {{AUTHOR}}  
**Date**: {{DATE}}  
**Build**: {{BUILD_NUMBER}}  

---

## Impact Summary

| Subsystem | Name | Risk | Relationship to Feature |
|-----------|------|------|------------------------|
| `SYS-DISC` | Device Discovery | {{RISK}} | {{RELATIONSHIP}} |
| `SYS-CONN` | Connection Manager | {{RISK}} | {{RELATIONSHIP}} |
| `SYS-UCI` | UCI Rendering Engine | {{RISK}} | {{RELATIONSHIP}} |
| `SYS-POPUP` | Popup & Layer Manager | {{RISK}} | {{RELATIONSHIP}} |
| `SYS-NAV` | Navigation Stack | {{RISK}} | {{RELATIONSHIP}} |
| `SYS-CTRL` | Control Interaction | {{RISK}} | {{RELATIONSHIP}} |
| `SYS-NET` | Network Layer | {{RISK}} | {{RELATIONSHIP}} |
| `SYS-STORE` | Persistence & Storage | {{RISK}} | {{RELATIONSHIP}} |
| `SYS-A11Y` | Accessibility | {{RISK}} | {{RELATIONSHIP}} |
| `SYS-ALERT` | Notifications & Alerts | {{RISK}} | {{RELATIONSHIP}} |
| `SYS-ORIENT` | Orientation & Layout | {{RISK}} | {{RELATIONSHIP}} |

> **Risk key**: 🔴 HIGH — regression likely · 🟡 MED — regression possible · 🟢 LOW — regression unlikely

---

## Regression Areas

### 🔴 HIGH Risk

#### {{HIGH_SYSTEM_NAME_1}} (`{{HIGH_SYSTEM_ID_1}}`)

> {{WHY_HIGH_RISK_1}}

**What to check**: {{WHAT_TO_CHECK_1}}  
**Symptom if it regresses**: {{REGRESSION_SYMPTOM_1}}  
**Existing coverage**: {{TC_REFERENCE_1}}

---

#### {{HIGH_SYSTEM_NAME_2}} (`{{HIGH_SYSTEM_ID_2}}`)

> {{WHY_HIGH_RISK_2}}

**What to check**: {{WHAT_TO_CHECK_2}}  
**Symptom if it regresses**: {{REGRESSION_SYMPTOM_2}}  
**Existing coverage**: {{TC_REFERENCE_2}}

---

### 🟡 MED Risk

#### {{MED_SYSTEM_NAME_1}} (`{{MED_SYSTEM_ID_1}}`)

> {{WHY_MED_RISK_1}}

**What to check**: {{WHAT_TO_CHECK_3}}  
**Symptom if it regresses**: {{REGRESSION_SYMPTOM_3}}  
**Existing coverage**: {{TC_REFERENCE_3}}

---

#### {{MED_SYSTEM_NAME_2}} (`{{MED_SYSTEM_ID_2}}`)

> {{WHY_MED_RISK_2}}

**What to check**: {{WHAT_TO_CHECK_4}}  
**Symptom if it regresses**: {{REGRESSION_SYMPTOM_4}}  
**Existing coverage**: {{TC_REFERENCE_4}}

---

### 🟢 LOW Risk

| Subsystem | Reason it is LOW risk | Recommended check |
|---|---|---|
| {{LOW_SYSTEM_1}} | {{LOW_REASON_1}} | {{LOW_CHECK_1}} |
| {{LOW_SYSTEM_2}} | {{LOW_REASON_2}} | {{LOW_CHECK_2}} |

---

## Integration Points

> _Integration boundaries are high-value regression targets — failures here are often silent and cross-system._

### Q-SYS Core Protocol

| Direction | Event / Command / Property | Risk |
|---|---|---|
| App → Core | `{{PROTOCOL_EVENT_1}}` | {{RISK}} |
| Core → App | `{{PROTOCOL_EVENT_2}}` | {{RISK}} |

### iOS APIs

| API | Used For | Risk |
|---|---|---|
| `{{IOS_API_1}}` | {{API_PURPOSE_1}} | {{RISK}} |
| `{{IOS_API_2}}` | {{API_PURPOSE_2}} | {{RISK}} |

### Persistence & Shared State

| Store | Data | Risk |
|---|---|---|
| `{{STORE_1}}` | `{{DATA_1}}` | {{RISK}} |
| `{{STORE_2}}` | `{{DATA_2}}` | {{RISK}} |

---

## Smoke Suite

> _Run this suite first. If any smoke test fails, halt and investigate before continuing the full regression pass._  
> **Target**: Complete all checks in under 10 minutes on one device.

| ID | Title | Subsystem | TC Reference | Est. Time |
|----|-------|-----------|--------------|-----------|
| SMOKE-{{FEAT}}-001 | {{SMOKE_TITLE_1}} | `{{SMOKE_SYS_1}}` | {{TC_REF_1}} | < 2 min |
| SMOKE-{{FEAT}}-002 | {{SMOKE_TITLE_2}} | `{{SMOKE_SYS_2}}` | {{TC_REF_2}} | < 2 min |
| SMOKE-{{FEAT}}-003 | {{SMOKE_TITLE_3}} | `{{SMOKE_SYS_3}}` | {{TC_REF_3}} | < 2 min |
| SMOKE-{{FEAT}}-004 | {{SMOKE_TITLE_4}} | `{{SMOKE_SYS_4}}` | {{TC_REF_4}} | < 2 min |
| SMOKE-{{FEAT}}-005 | {{SMOKE_TITLE_5}} | `{{SMOKE_SYS_5}}` | {{TC_REF_5}} | < 2 min |

### Smoke Suite Execution Log

| Date | Tester | Build | Pass | Fail | Notes |
|------|--------|-------|------|------|-------|
| | | | | | |

---

## Recommended Validation Areas

### P1 — Must validate before any QE sign-off

> _These areas must pass on at least one iPad and one iPhone before a regression sign-off can be given._

- [ ] **{{P1_AREA_1}}** (`{{P1_SYS_1}}`) — {{P1_DESCRIPTION_1}} → see {{P1_TC_1}}
- [ ] **{{P1_AREA_2}}** (`{{P1_SYS_2}}`) — {{P1_DESCRIPTION_2}} → see {{P1_TC_2}}
- [ ] **{{P1_AREA_3}}** (`{{P1_SYS_3}}`) — {{P1_DESCRIPTION_3}} → see {{P1_TC_3}}

### P2 — Validate during standard regression pass

> _Run these as part of the sprint's regression sweep. Any failure should be logged as a defect._

- [ ] **{{P2_AREA_1}}** (`{{P2_SYS_1}}`) — {{P2_DESCRIPTION_1}}
- [ ] **{{P2_AREA_2}}** (`{{P2_SYS_2}}`) — {{P2_DESCRIPTION_2}}
- [ ] **{{P2_AREA_3}}** (`{{P2_SYS_3}}`) — {{P2_DESCRIPTION_3}}
- [ ] **{{P2_AREA_4}}** (`{{P2_SYS_4}}`) — {{P2_DESCRIPTION_4}}

### P3 — Spot-check or defer

> _Low-risk areas. A single pass or existing automated coverage is sufficient._

- [ ] **{{P3_AREA_1}}** (`{{P3_SYS_1}}`) — {{P3_DESCRIPTION_1}}
- [ ] **{{P3_AREA_2}}** (`{{P3_SYS_2}}`) — {{P3_DESCRIPTION_2}}

---

## References

| Reference | Path |
|---|---|
| Test Plan | [`test-plan.md`](./test-plan.md) |
| Test Cases | [`test-cases.md`](./test-cases.md) |
| Jira Story | [`jira-story.md`](./jira-story.md) |
| Feature Spec | `{{SPEC_PATH}}` |
| OpenSpec Change | `openspec/changes/{{CHANGE_NAME}}/` |
