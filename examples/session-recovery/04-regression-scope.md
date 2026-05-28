# Regression Scope: Session Recovery and Reconnect

**Feature**: Session Recovery and Reconnect  
**Feature Code**: SRR  
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
| `SYS-CONN` | Connection Manager | 🔴 HIGH | **Direct** — this subsystem owns the reconnect lifecycle and is the feature under test |
| `SYS-NET` | Network Layer | 🔴 HIGH | **Direct** — connection loss detection, TCP retry logic, and socket lifecycle are owned here |
| `SYS-ALERT` | Notifications & Alerts | 🔴 HIGH | **Shared surface** — the reconnecting banner and error dialogs are rendered and managed by this subsystem |
| `SYS-UCI` | UCI Rendering Engine | 🔴 HIGH | **Shared state** — session state restoration requires the UCI to re-render the correct page and layers from the live Core state |
| `SYS-STORE` | Persistence & Storage | 🔴 HIGH | **Shared state** — the last-known session state (page, layer visibility) may be cached here to seed the restoration after reconnect |
| `SYS-DISC` | Device Discovery | 🟡 MED | **Shared logic** — auto-reconnect may need to re-resolve the Core's address via the device discovery layer if the Core's IP changed |
| `SYS-NAV` | Navigation Stack | 🟡 MED | **Shared lifecycle** — reconnect must restore the navigation stack to the correct page without adding or removing stack entries incorrectly |
| `SYS-POPUP` | Popup & Layer Manager | 🟡 MED | **Shared state** — open popup state must be dismissed or restored correctly on reconnect; incorrect handling leaves a stale popup overlay |
| `SYS-CTRL` | Control Interaction | 🟡 MED | **Shared surface** — controls must immediately reflect live Core state after reconnect; touch events mid-reconnect must not get stuck |
| `SYS-A11Y` | Accessibility | 🟡 MED | **Shared surface** — VoiceOver must announce connection status changes; reconnect banner and manual Reconnect button must be labeled |
| `SYS-ORIENT` | Orientation & Layout | 🟢 LOW | **Isolated** — orientation state is independent of reconnect logic; spot-check that reconnect banner renders correctly in both orientations |

---

## Regression Areas

### 🔴 HIGH Risk

#### Connection Manager (`SYS-CONN`)

> This subsystem IS the feature under test. The reconnect state machine, retry backoff, timeout logic, and transition to manual reconnect all live here. Any code change to the reconnect implementation has a direct and high probability of causing regression in the connect/disconnect flows that were working before, including the explicit disconnect path and the initial first-connect flow.

**What to check**: Normal connect flow (device list → tap → UCI loads) works without any reconnect UI appearing; explicit Disconnect returns to device list without triggering auto-reconnect; passcode authentication still works on first connect.  
**Symptom if it regresses**: Connect button does nothing; connection takes longer; explicit disconnect triggers reconnect; passcode prompt missing or unresponsive.  
**Existing coverage**: TC-SRR-023, TC-SRR-024

---

#### Network Layer (`SYS-NET`)

> The Network Layer owns connection loss detection, socket teardown, and the TCP reconnect attempt. Changes here affect how quickly a drop is detected, how many retries occur, and how long the app waits before declaring the reconnect failed. A regression in this layer can cause: silent drops (no banner shown), premature failures (manual reconnect shown before retries), or retry storms.

**What to check**: Reconnect banner appears within 5 seconds of Wi-Fi drop; auto-retry completes before the manual reconnect button is shown; no socket leak or memory growth during extended offline periods.  
**Symptom if it regresses**: Banner never appears; manual reconnect button appears immediately without retrying; app unresponsive after a long offline period.  
**Existing coverage**: TC-SRR-004, TC-SRR-008, TC-SRR-010

---

#### Notifications & Alerts (`SYS-ALERT`)

> The reconnecting banner, connection-lost message, and manual reconnect error dialog are all surfaced by the Alerts subsystem. If the reconnect state machine sends new or changed events, the Alerts layer may receive unknown event types and either show the wrong message, show nothing, or crash. Changes to the reconnecting UX (new strings, new banner type) are a regression risk for previously established alert behaviors.

**What to check**: Reconnecting banner text is correct and readable; error dialog after manual reconnect failure has specific, actionable copy; no duplicate or layered banners appear during flapping network tests.  
**Symptom if it regresses**: Generic "An error occurred" replaces specific messaging; no banner shown on disconnect; two overlapping banners stack on screen.  
**Existing coverage**: TC-SRR-004, TC-SRR-008, TC-SRR-009, TC-SRR-013

---

#### UCI Rendering Engine (`SYS-UCI`)

> After a successful reconnect, the UCI must re-render the exact page and layer state received from the Core. The Reconnect feature introduces a new event path — post-reconnect state hydration — that the UCI engine must handle. If the hydration event is malformed, arrives out of order, or is dropped, the UCI may render a blank page, the wrong page, or a page with stale control values.

**What to check**: Post-reconnect UCI page matches the page the user was on before disconnection; control values (sliders, buttons) reflect live Core state immediately after reconnect; no blank or partially-rendered page after reconnect.  
**Symptom if it regresses**: UCI shows blank design page after reconnect; wrong page shown; controls show old values that don't respond to interaction.  
**Existing coverage**: TC-SRR-005, TC-SRR-006, TC-SRR-007

---

#### Persistence & Storage (`SYS-STORE`)

> The last-known session state (active page index, layer visibility bitmask) may be written to storage during an active session and read back during reconnect to seed the UI before the live Core state arrives. A schema change in the reconnect feature (e.g., new state fields) can cause a mismatch when reading back an older cached record, producing incorrect session restoration or a silent failure that falls back to page 1.

**What to check**: After app restart and reconnect, the last known page and layer state are used to render an initial state before the live Core response arrives; no crash when reading session cache written by an older app version.  
**Symptom if it regresses**: App always shows page 1 after reconnect even when the user was on another page; crash on launch due to unrecognized session cache format.  
**Existing coverage**: TC-SRR-005, TC-SRR-006, TC-SRR-018

---

### 🟡 MED Risk

#### Device Discovery (`SYS-DISC`)

> Auto-reconnect may invoke the discovery layer to re-resolve the Core's current IP address if the network topology changed between disconnect and reconnect (e.g., DHCP lease renewal). If discovery logic was not designed as a re-entrant path for reconnect (only for initial connection), this shared code path can produce a race condition or memory leak.

**What to check**: Reconnect succeeds when the Core's IP has changed since initial connection; discovery scan is not continuously running during the reconnect retry loop.  
**Symptom if it regresses**: Reconnect fails even when the Core is online but at a new IP; mDNS scanner remains active during the entire offline period, draining battery.  
**Existing coverage**: TC-SRR-003 (Core restart — Core may get a new IP)

---

#### Navigation Stack (`SYS-NAV`)

> The navigation stack must not accumulate extra entries or lose entries during the reconnect cycle. If reconnect is implemented by pushing a new connection screen or by popping back to the device list and re-pushing the design, the stack depth after reconnect may differ from before, causing unexpected back-button behavior or memory growth over repeated reconnect cycles.

**What to check**: After reconnect, the back-button / disconnect action behaves identically to before the interruption; stack depth does not grow with each reconnect cycle.  
**Symptom if it regresses**: Double-back required after reconnect; back button leads to an unexpected intermediate screen; stack overflow crash after many reconnect cycles.  
**Existing coverage**: TC-SRR-023, TC-SRR-024

---

#### Popup & Layer Manager (`SYS-POPUP`)

> If a popup was open when the connection was lost, the reconnect cycle must cleanly dismiss it or restore it based on the live Core state. The popup's internal state (fade animation, overlay z-order) must be reset during reconnect. A regression here leaves a ghost popup overlay permanently visible after reconnect, blocking all design interaction.

**What to check**: Popup open before disconnect is properly dismissed or correctly re-shown based on Core state after reconnect; no transparent overlay blocking interaction after reconnect.  
**Symptom if it regresses**: Invisible overlay blocks all touch input after reconnect; popup re-opens and cannot be closed; fade animation is stuck mid-way.  
**Existing coverage**: TC-SRR-016

---

#### Control Interaction (`SYS-CTRL`)

> Controls in the UCI that were mid-interaction when the connection dropped (e.g., a slider being dragged) must be gracefully cancelled and then restored to the live Core state on reconnect. If the touch event lifecycle is not correctly unwound during disconnect, a residual gesture recognizer may remain attached, causing a control to be unresponsive post-reconnect.

**What to check**: Slider dragged during disconnect returns to correct value after reconnect; all controls on the current page respond normally to touch immediately after reconnect without requiring page navigation.  
**Symptom if it regresses**: Slider stuck at value from the moment of disconnect; controls unresponsive to touch after reconnect; control sends duplicate value events after reconnect.  
**Existing coverage**: TC-SRR-007, TC-SRR-012

---

#### Accessibility (`SYS-A11Y`)

> VoiceOver must announce the reconnect lifecycle events: connection lost, reconnecting, connected/failed. The manual Reconnect button must have a meaningful accessibility label. Changes to the reconnecting banner's view hierarchy or the button's construction may silently strip accessibility identifiers.

**What to check**: VoiceOver announces "Reconnecting" when the banner appears; announces the result (connected or failed) when the retry resolves; manual Reconnect button label is not "Button" or "unlabeled".  
**Symptom if it regresses**: VoiceOver does not announce state changes; Reconnect button inaccessible via swipe navigation; double-tap on button does nothing.  
**Existing coverage**: TC-SRR-020, TC-SRR-021, TC-SRR-022

---

### 🟢 LOW Risk

| Subsystem | Reason it is LOW risk | Recommended check |
|---|---|---|
| `SYS-ORIENT` | Orientation state is independent of the reconnect lifecycle; no shared code | Spot-check: reconnect banner renders cleanly in both iPad landscape and iPhone portrait |

---

## Integration Points

### Q-SYS Core Protocol

| Direction | Event / Command / Property | Risk |
|---|---|---|
| Core → App | Connection drop event / TCP socket close | 🔴 HIGH — the mechanism that triggers the reconnect flow; if not detected correctly, the flow never starts |
| Core → App | Post-reconnect design state snapshot (page, layer, control values) | 🔴 HIGH — this is the payload that seeds session restoration; if malformed or late, the UCI renders incorrectly |
| App → Core | TCP reconnect handshake | 🔴 HIGH — the app must correctly re-initiate the Q-SYS protocol handshake; any change to handshake parameters breaks this |
| Core → App | Authentication challenge on reconnect (if session expired) | 🟡 MED — if the Core requires re-authentication after a long offline period, the app must surface the passcode prompt correctly |

### iOS APIs

| API | Used For | Risk |
|---|---|---|
| `Network.framework` (NWConnection) | TCP connection teardown and reconnect | 🔴 HIGH — reconnect is implemented on top of this API; incorrect state handling causes silent failures |
| `UIApplication` lifecycle notifications | Detecting background/foreground transitions to trigger/defer reconnect | 🟡 MED — if notifications are not resubscribed after app update, foreground reconnect never triggers |
| `UIAccessibility.post(.announcement)` | VoiceOver announcements for reconnect status changes | 🟡 MED — announcement timing relative to the banner appearance determines whether VoiceOver picks it up |
| `BackgroundTasks` framework | Keeping reconnect alive in background (if implemented) | 🟡 MED — background task expiration during reconnect can leave app in an inconsistent state when foregrounded |

### Persistence & Shared State

| Store | Data | Risk |
|---|---|---|
| In-memory session state | Active page index, layer visibility bitmask, open popup ID | 🔴 HIGH — this state is read at reconnect time to restore the session; any mutation during the reconnect cycle can restore the wrong state |
| `UserDefaults` | Last-connected Core address and session metadata | 🟡 MED — used to seed the auto-reconnect target address; stale or corrupt data causes reconnect to the wrong Core |

---

## Smoke Suite

> _Run this suite first. If any smoke test fails, halt and investigate before continuing the full regression pass._  
> **Target**: Complete all 5 checks in under 10 minutes on one iPad.

| ID | Title | Subsystem | TC Reference | Est. Time |
|----|-------|-----------|--------------|-----------|
| SMOKE-SRR-001 | Reconnect banner appears within 5 seconds of Wi-Fi drop | `SYS-CONN` + `SYS-ALERT` | TC-SRR-004 | < 2 min |
| SMOKE-SRR-002 | Auto-reconnect succeeds after Wi-Fi restore; UCI design displayed | `SYS-CONN` + `SYS-UCI` | TC-SRR-001 | < 2 min |
| SMOKE-SRR-003 | Last active page is restored post-reconnect | `SYS-UCI` + `SYS-STORE` | TC-SRR-005 | < 2 min |
| SMOKE-SRR-004 | Normal connect flow (no reconnect UI) unaffected | `SYS-CONN` | TC-SRR-023 | < 2 min |
| SMOKE-SRR-005 | Explicit Disconnect returns to device list; no auto-reconnect triggered | `SYS-CONN` + `SYS-NAV` | TC-SRR-024 | < 2 min |

### Smoke Suite Execution Log

| Date | Tester | Build | Pass | Fail | Notes |
|------|--------|-------|------|------|-------|
| | | | | | |

---

## Recommended Validation Areas

### P1 — Must validate before any QE sign-off

- [ ] **Auto-reconnect initiation** (`SYS-CONN` + `SYS-NET`) — banner appears within 5 seconds of Wi-Fi drop; auto-retry completes without manual intervention → TC-SRR-001, TC-SRR-004
- [ ] **Session state restoration** (`SYS-UCI` + `SYS-STORE`) — correct page and layer state shown immediately after reconnect → TC-SRR-005, TC-SRR-006
- [ ] **Manual reconnect button** (`SYS-ALERT`) — button appears with a specific error message after auto-retries exhausted → TC-SRR-008
- [ ] **Stable offline state** (`SYS-CONN`) — app does not crash, loop, or grow memory during 5-minute offline period → TC-SRR-010
- [ ] **Explicit disconnect regression** (`SYS-CONN` + `SYS-NAV`) — Disconnect button never triggers auto-reconnect → TC-SRR-024
- [ ] **Normal connect regression** (`SYS-CONN`) — first-time connect from device list is unaffected → TC-SRR-023

### P2 — Validate during standard regression pass

- [ ] **App foregrounded after Core restart** (`SYS-CONN` + `SYS-DISC`) — foreground triggers immediate reconnect attempt → TC-SRR-017
- [ ] **Flapping network stability** (`SYS-NET`) — no error loop or cascading dialogs during repeated off/on cycles → TC-SRR-013
- [ ] **Mid-interaction recovery** (`SYS-CTRL`) — control state restored correctly after connection lost during active slider drag → TC-SRR-012
- [ ] **Popup state on reconnect** (`SYS-POPUP`) — open popup dismissed or re-shown correctly; no ghost overlay → TC-SRR-016
- [ ] **Post-reconnect control responsiveness** (`SYS-CTRL`) — all controls on the current page respond without requiring page re-navigation → TC-SRR-007
- [ ] **Lock screen reconnect** (`SYS-NET` + `SYS-CONN`) — reconnect resumes after device unlock → TC-SRR-015
- [ ] **VoiceOver reconnect announcements** (`SYS-A11Y`) — "Reconnecting" and resolution both announced by VoiceOver → TC-SRR-020, TC-SRR-021

### P3 — Spot-check or defer

- [ ] **Orientation layout** (`SYS-ORIENT`) — reconnect banner renders cleanly in both iPad landscape and iPhone portrait → spot check, no dedicated TC required
- [ ] **Force-quit and relaunch** (`SYS-STORE`) — device list shows last-used Core; no reconnect auto-triggered on launch → TC-SRR-018
- [ ] **VoiceOver Reconnect button** (`SYS-A11Y`) — manual Reconnect button is reachable and labeled for VoiceOver → TC-SRR-022

---

## References

| Reference | Path |
|---|---|
| Test Plan | [`test-plan.md`](./test-plan.md) |
| Test Cases | [`test-cases.md`](./test-cases.md) |
| Jira Story | [`jira-story.md`](./jira-story.md) |
| Feature Spec | `openspec/specs/session-recovery-reconnect/spec.md` |
| OpenSpec Change | `openspec/changes/session-recovery-reconnect/` |
