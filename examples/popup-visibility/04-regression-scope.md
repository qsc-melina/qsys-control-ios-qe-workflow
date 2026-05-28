# Regression Scope: Popup Visibility and Layer Transition Behavior

**Feature**: Popup Visibility and Layer Transition Behavior  
**Feature Code**: PVL  
**Platform**: Q-SYS iOS Viewer  
**Test Plan**: [test-plan.md](./test-plan.md)  
**Test Cases**: [test-cases.md](./test-cases.md)  
**Author**: QE Team  
**Date**: 2026-05-21  
**Build**: TBD  

---

## Impact Summary

| Subsystem | Name | Risk | Relationship to Feature |
|-----------|------|------|------------------------|
| `SYS-POPUP` | Popup & Layer Manager | 🔴 HIGH | **Direct** — this subsystem is the feature under test |
| `SYS-UCI` | UCI Rendering Engine | 🔴 HIGH | **Shared logic** — popups and shared layers are rendered by the UCI engine; transition code paths are tightly coupled |
| `SYS-NAV` | Navigation Stack | 🔴 HIGH | **Shared state** — layer visibility state must persist across page navigation; the nav stack drives page load/unload lifecycle |
| `SYS-CTRL` | Control Interaction | 🟡 MED | **Shared surface** — controls on shared layers must remain interactive while a popup is open and after it closes |
| `SYS-A11Y` | Accessibility | 🟡 MED | **Shared surface** — VoiceOver focus management is directly tied to popup open/close; focus trap logic is in this layer |
| `SYS-ORIENT` | Orientation & Layout | 🟡 MED | **Shared lifecycle** — popup layout must adapt to orientation changes; safe area handling is owned by this subsystem |
| `SYS-STORE` | Persistence & Storage | 🟡 MED | **Shared state** — layer visibility state may be cached; incorrect state restoration after app restart is a real risk |
| `SYS-DISC` | Device Discovery | 🟢 LOW | **Isolated** — discovery runs before the design is loaded; no shared state with popup or layer logic |
| `SYS-CONN` | Connection Manager | 🟢 LOW | **Isolated** — connection lifecycle is complete before any popup can be displayed |
| `SYS-NET` | Network Layer | 🟢 LOW | **Isolated** — network changes affect connection state, not rendering of already-loaded popups |
| `SYS-ALERT` | Notifications & Alerts | 🟢 LOW | **Isolated** — system alert dialogs (e.g., connection lost) are architecturally separate from UCI popup panels |

---

## Regression Areas

### 🔴 HIGH Risk

#### Popup & Layer Manager (`SYS-POPUP`)

> This subsystem IS the feature under test. The popup open/close state machine, fade transition timing, and shared layer visibility toggles all live here. Any refactoring of popup lifecycle events, transition animation code, or layer z-order management will directly impact this subsystem.

**What to check**: Popup opens with fade-in; popup closes with fade-out; no duplicate popups on rapid trigger; shared layer visibility unchanged after popup cycle.  
**Symptom if it regresses**: Popup appears instantly with no animation; popup does not close; a second tap creates a duplicate overlay; shared layer disappears after popup close.  
**Existing coverage**: TC-PVL-001, TC-PVL-002, TC-PVL-003, TC-PVL-005, TC-PVL-006, TC-PVL-009

---

#### UCI Rendering Engine (`SYS-UCI`)

> The UCI Rendering Engine owns the compositing of all design layers, including the popup panel and the shared layers beneath it. Popup visibility changes are applied as rendering commands within this engine. A regression here can manifest as visual corruption (incorrect z-order, stale frames, wrong layer opacity) even when the Popup Manager's state is correct.

**What to check**: After a popup open/close cycle, the underlying UCI design page is fully rendered with no stale overlay frames; shared layer controls are visible and their values are current; the design renders correctly on both iPad and iPhone.  
**Symptom if it regresses**: Ghost overlay visible after popup closes; shared layer controls show stale values; design partially renders or flashes.  
**Existing coverage**: TC-PVL-005, TC-PVL-006, TC-PVL-021

---

#### Navigation Stack (`SYS-NAV`)

> Layer visibility state must survive page navigation — navigating away from a page and returning should restore the exact layer visibility that existed before. This requires the Navigation Stack to correctly snapshot and restore per-page state. Changes to popup/layer lifecycle may break the snapshot contract.

**What to check**: Navigate away from a page with an active layer state and return; confirm all layer visibility states are identical to before navigation.  
**Symptom if it regresses**: Layers reset to default visibility on return to page; popups re-open on navigation return; navigation stack itself is corrupted (wrong page shown).  
**Existing coverage**: TC-PVL-008, TC-PVL-022

---

### 🟡 MED Risk

#### Control Interaction (`SYS-CTRL`)

> Controls on shared layers (sliders, buttons, toggles) must remain fully interactive after a popup is opened over them and after the popup is dismissed. The popup's touch-blocking overlay may, if implemented incorrectly, leave a residual gesture recognizer that absorbs touches meant for underlying controls.

**What to check**: Tap a slider on a shared layer after a full popup open/close cycle; confirm the slider responds and changes value.  
**Symptom if it regresses**: Controls on the shared layer are unresponsive to touch; controls respond but values don't update; tapping a control triggers the popup unexpectedly.  
**Existing coverage**: TC-PVL-021

---

#### Accessibility (`SYS-A11Y`)

> VoiceOver focus trapping and restoration are the most complex accessibility behaviors in this feature. When a popup opens, VoiceOver focus must move into the popup and must not leak back to the underlying design. When the popup closes, focus must return to a logical position. Changes to popup rendering order or lifecycle events may silently break this.

**What to check**: VoiceOver focus enters the popup on open; focus cannot reach elements behind the popup while it is open; focus returns to the design page after popup closes.  
**Symptom if it regresses**: VoiceOver announces elements behind the popup (focus escape); focus does not move into popup on open (user cannot interact with popup via VoiceOver); focus lost entirely after close.  
**Existing coverage**: TC-PVL-018, TC-PVL-019

---

#### Orientation & Layout (`SYS-ORIENT`)

> The popup panel and its fade overlay must adapt correctly when the device orientation changes while the popup is open. Safe area insets must be respected. If the popup's frame is calculated at open time and not recomputed on rotation, it may appear off-center or be clipped after rotation.

**What to check**: Open a popup in portrait on iPad; rotate to landscape; confirm the popup and its overlay fill the screen correctly with no clipping.  
**Symptom if it regresses**: Popup remains portrait-sized in landscape (mispositioned); overlay doesn't cover full screen; popup content is clipped by safe area.  
**Existing coverage**: TC-PVL-017

---

#### Persistence & Storage (`SYS-STORE`)

> If the app caches layer visibility state to disk (e.g., to restore after a backgrounded/killed app launch), changes to the state schema may cause incorrect restoration. This risk is MED rather than HIGH because most implementations derive layer state from the live Core connection on reconnect rather than from local cache.

**What to check**: After setting a specific layer visibility state, kill the app, relaunch, and reconnect; confirm layer state matches what the Core is currently broadcasting.  
**Symptom if it regresses**: Layer state from a previous session is shown before the live Core state arrives (flash of wrong state); cached state overrides the live state permanently.  
**Existing coverage**: TC-PVL-008 (partially — navigate away/back; full kill/relaunch is not covered; add a new test case if caching is confirmed)

---

### 🟢 LOW Risk

| Subsystem | Reason it is LOW risk | Recommended check |
|---|---|---|
| `SYS-DISC` | Discovery is complete before any design is loaded; popup code has no intersection with discovery | Skip — no regression expected |
| `SYS-CONN` | Connection is fully established before popups are rendered | One quick: verify connected device name still shows in nav bar after a popup cycle |
| `SYS-NET` | Network changes affect connection loss alerts, not in-memory popup rendering state | Skip unless TC-PVL-011 (network mid-transition) fails |
| `SYS-ALERT` | iOS system alerts are a separate layer above the app UI; not affected by UCI popup changes | Skip — no regression expected |

---

## Integration Points

### Q-SYS Core Protocol

| Direction | Event / Command / Property | Risk |
|---|---|---|
| Core → App | Popup visibility change command (show/hide named popup) | 🔴 HIGH — the Core drives popup open/close; incorrect handling of this event is the most likely source of regression |
| Core → App | Layer visibility state update | 🔴 HIGH — shared layer visibility is driven by Core events; missed or mishandled events cause incorrect layer state |
| App → Core | _(none expected)_ — popup state is read-only from the app's perspective in the current design | 🟢 LOW |

### iOS APIs

| API | Used For | Risk |
|---|---|---|
| `UIView.animate` / SwiftUI animation | Fade-in and fade-out transitions | 🔴 HIGH — animation completion handlers drive the "popup is open/closed" state; incorrect handler can leave state inconsistent |
| `UIAccessibility.post(.screenChanged)` | VoiceOver focus management on popup open/close | 🟡 MED — if not called at correct times, VoiceOver focus is not redirected |
| `UIView.insertSubview` / `bringSubviewToFront` | Z-order management of popup overlay | 🔴 HIGH — incorrect z-order leaves the overlay below design controls (touches fall through) or above system alerts (overlay traps all input) |
| `UIDevice.orientationDidChangeNotification` | Recomputing popup frame on rotation | 🟡 MED — missing listener means popup stays at old orientation frame |

### Persistence & Shared State

| Store | Data | Risk |
|---|---|---|
| In-memory design state (per page) | Current layer visibility bitmask per page | 🔴 HIGH — if modified incorrectly by popup open/close, page navigation restoration will restore wrong state |
| `UserDefaults` or local cache | Cached layer visibility (if implemented) | 🟡 MED — schema mismatch on app update can corrupt restored state |

---

## Smoke Suite

> _Run this suite first. If any smoke test fails, halt and investigate before continuing the full regression pass._  
> **Target**: Complete all 5 checks in under 10 minutes on one iPad.

| ID | Title | Subsystem | TC Reference | Est. Time |
|----|-------|-----------|--------------|-----------|
| SMOKE-PVL-001 | Popup opens with fade-in; close button dismisses with fade-out | `SYS-POPUP` | TC-PVL-001, TC-PVL-003 | < 2 min |
| SMOKE-PVL-002 | Shared layer visible before, during, and after popup cycle | `SYS-UCI` + `SYS-POPUP` | TC-PVL-005, TC-PVL-006 | < 2 min |
| SMOKE-PVL-003 | Navigate away and back — layer state unchanged | `SYS-NAV` | TC-PVL-008 | < 2 min |
| SMOKE-PVL-004 | Rapid 10× open/close taps — UI settles with no corruption | `SYS-POPUP` | TC-PVL-012 | < 2 min |
| SMOKE-PVL-005 | Shared layer controls are tappable after popup cycle | `SYS-CTRL` | TC-PVL-021 | < 2 min |

### Smoke Suite Execution Log

| Date | Tester | Build | Pass | Fail | Notes |
|------|--------|-------|------|------|-------|
| | | | | | |

---

## Recommended Validation Areas

### P1 — Must validate before any QE sign-off

- [ ] **Popup fade-in on open** (`SYS-POPUP`) — Fade animation plays correctly on both iPad and iPhone; no instant-appear regression → TC-PVL-001, TC-PVL-002
- [ ] **Popup fade-out on close** (`SYS-POPUP`) — Fade-out plays; popup is fully removed from view hierarchy after completion → TC-PVL-003
- [ ] **Shared layer state after popup cycle** (`SYS-UCI`) — Shared layer visible before, during, and after popup; no visibility state change → TC-PVL-005, TC-PVL-006
- [ ] **Rapid interaction robustness** (`SYS-POPUP`) — 10× rapid open/close produces a stable UI with no frozen animation or crash → TC-PVL-012

### P2 — Validate during standard regression pass

- [ ] **Layer state after page navigation** (`SYS-NAV`) — Navigate away and back; layer visibility identical to pre-navigation state → TC-PVL-008
- [ ] **Shared layer controls post-cycle** (`SYS-CTRL`) — Sliders and buttons on shared layers respond correctly after popup open/close → TC-PVL-021
- [ ] **App backgrounded mid-transition** (`SYS-POPUP`) — Popup in definite open or closed state when app foregrounded; no frozen frame → TC-PVL-014
- [ ] **Overlapping transitions** (`SYS-POPUP`) — Opening a new popup during a running fade-out produces a correct final state → TC-PVL-013
- [ ] **Page navigation unaffected** (`SYS-NAV`) — Full page navigation works after popup cycle; no stack corruption → TC-PVL-022
- [ ] **VoiceOver focus management** (`SYS-A11Y`) — VoiceOver enters popup on open; cannot reach behind; returns to page on close → TC-PVL-018, TC-PVL-019

### P3 — Spot-check or defer

- [ ] **iPad landscape popup layout** (`SYS-ORIENT`) — Popup and overlay fill screen correctly after portrait→landscape rotation while popup is open → TC-PVL-017
- [ ] **Dynamic Type in popup** (`SYS-A11Y`) — No text truncation at Accessibility XL inside the popup → TC-PVL-020
- [ ] **Long popup scroll** (`SYS-UCI`) — Scrollable popup reaches all content; close control accessible → TC-PVL-015
- [ ] **Popup stacking** (`SYS-POPUP`) — Second popup opens correctly over first; independently closable → TC-PVL-007

---

## References

| Reference | Path |
|---|---|
| Test Plan | [`test-plan.md`](./test-plan.md) |
| Test Cases | [`test-cases.md`](./test-cases.md) |
| Jira Story | [`jira-story.md`](./jira-story.md) |
| Feature Spec | `openspec/specs/popup-visibility-layer-transitions/spec.md` |
| OpenSpec Change | `openspec/changes/popup-visibility-layer-transitions/` |
