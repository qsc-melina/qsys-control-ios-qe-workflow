# Regression Scope: Device Discovery and Connection

**Feature**: Device Discovery and Connection  
**Feature Code**: DD  
**Platform**: Q-SYS iOS Viewer  
**Test Plan**: [test-plan.md](./test-plan.md)  
**Test Cases**: [test-cases.md](./test-cases.md)  
**Author**: QE Team  
**Date**: 2026-05-21  
**Build**: iOS Viewer 9.10.0 (build 1000)  

---

## Impact Summary

| Subsystem | Name | Risk | Relationship to Feature |
|-----------|------|------|------------------------|
| `SYS-DISC` | Device Discovery | 🔴 HIGH | **Direct** — this subsystem is the feature under test |
| `SYS-CONN` | Connection Manager | 🔴 HIGH | **Direct** — connection initiation and auth are core to this feature |
| `SYS-NET` | Network Layer | 🔴 HIGH | **Shared logic** — mDNS scanning and IP resolution are owned by this layer |
| `SYS-STORE` | Persistence & Storage | 🔴 HIGH | **Shared state** — Recents list and saved device preferences are written here |
| `SYS-UCI` | UCI Rendering Engine | 🟡 MED | **Shared surface** — once connected, the UCI renders the design; broken connection state can corrupt the render |
| `SYS-NAV` | Navigation Stack | 🟡 MED | **Shared lifecycle** — connecting navigates the user into the design; disconnect must unwind the stack correctly |
| `SYS-ALERT` | Notifications & Alerts | 🟡 MED | **Shared surface** — error dialogs (timeout, wrong passcode, no network) are rendered by this subsystem |
| `SYS-A11Y` | Accessibility | 🟡 MED | **Shared surface** — device list items, connect buttons, and error alerts must maintain correct VoiceOver labels |
| `SYS-ORIENT` | Orientation & Layout | 🟡 MED | **Shared lifecycle** — device list must adapt to iPad landscape; layout changes may expose rendering bugs |
| `SYS-POPUP` | Popup & Layer Manager | 🟢 LOW | **Isolated** — popups are not part of the Discovery or Connection flow |
| `SYS-CTRL` | Control Interaction | 🟢 LOW | **Isolated** — UCI controls are not active until after connection is complete |

---

## Regression Areas

### 🔴 HIGH Risk

#### Device Discovery (`SYS-DISC`)

> This subsystem IS the feature under test. Any code change to discovery affects mDNS scanning cadence, device list rendering, and manual IP entry validation. A regression here means the core user value is broken.

**What to check**: mDNS devices appear in the list within 10 seconds; manual IP entry accepts valid addresses and rejects invalid ones; device list deduplicates correctly.  
**Symptom if it regresses**: No devices appear in the list; duplicate entries shown; valid IP rejected; invalid IP accepted without error.  
**Existing coverage**: TC-DD-001, TC-DD-002, TC-DD-004, TC-DD-010, TC-DD-013

---

#### Connection Manager (`SYS-CONN`)

> Connection initiation, authentication (passcode entry), and disconnect are all driven through the Connection Manager. Changes to device discovery may alter how the manager resolves a selected device's address, introducing subtle failures in the handshake or timeout logic.

**What to check**: Tapping a discovered device connects and shows the UCI design page; passcode prompt appears for protected designs; incorrect passcode shows an error; connection timeout is surfaced within 15 seconds.  
**Symptom if it regresses**: Tapping a device does nothing; connection hangs indefinitely; wrong passcode silently accepted; correct passcode rejected.  
**Existing coverage**: TC-DD-003, TC-DD-005, TC-DD-009, TC-DD-011

---

#### Network Layer (`SYS-NET`)

> The Network Layer owns mDNS socket management and IP reachability. Discovery features call directly into this layer, so any refactoring of discovery logic may break mDNS listener lifecycle, causing devices to not be found after the app is backgrounded and foregrounded, or after a network change.

**What to check**: Discovery resumes after app foreground; discovery resumes after Wi-Fi toggle; devices found on both iOS 17.x and iOS 16.x.  
**Symptom if it regresses**: Devices disappear from list after foregrounding; mDNS scan never restarts; Wi-Fi toggle leaves app in broken network state.  
**Existing coverage**: TC-DD-015, TC-DD-016

---

#### Persistence & Storage (`SYS-STORE`)

> The Recents list and saved device preferences are the only data persisted across app launches by the Discovery feature. A regression here means users lose their device history on restart, or the wrong device is shown in Recents.

**What to check**: A previously connected device appears in Recents after app restart; Recents entry contains the correct hostname and timestamp.  
**Symptom if it regresses**: Recents list is empty after restart; wrong device shown; app crashes when reading saved device data.  
**Existing coverage**: TC-DD-006

---

### 🟡 MED Risk

#### UCI Rendering Engine (`SYS-UCI`)

> The UCI Rendering Engine is invoked immediately after a successful connection. If connection state is not passed correctly (e.g., missing design metadata, wrong authentication context), the render may fail silently or show a blank design page.

**What to check**: After connecting to a Core, the design's UCI page renders correctly with all controls visible; no blank screen or partial render.  
**Symptom if it regresses**: UCI renders blank or shows a loading spinner indefinitely after connection.  
**Existing coverage**: TC-DD-003 (transitional check), TC-DD-020

---

#### Navigation Stack (`SYS-NAV`)

> The navigation stack is pushed when the user connects and popped when they disconnect or go back. If discovery changes alter the connection flow, the stack may be left in an inconsistent state — e.g., the disconnect button may not return the user to the device list.

**What to check**: After connecting, the back/disconnect action returns the user to the device list; the navigation stack is clean and no ghost screens remain.  
**Symptom if it regresses**: Back button leads to a blank screen; double-back needed to reach device list; app trapped in a nested navigation state.  
**Existing coverage**: TC-DD-020

---

#### Notifications & Alerts (`SYS-ALERT`)

> Error dialogs for connection failures, timeouts, and passcode errors are rendered by the Alerts subsystem. If discovery or connection error codes change, the wrong alert may be shown or the alert may not appear at all.

**What to check**: Connection timeout shows a specific and actionable error message; wrong passcode shows an error (not a generic alert); network unavailable shows a clear no-network message.  
**Symptom if it regresses**: Generic "An error occurred" replaces a specific error; no alert shown on timeout; app freezes without surfacing any error UI.  
**Existing coverage**: TC-DD-008, TC-DD-009, TC-DD-011, TC-DD-014

---

#### Accessibility (`SYS-A11Y`)

> VoiceOver labels on device list rows, the Add Device button, and all error alert content must be maintained. Discovery code changes that rename or restructure UI components may silently strip accessibility identifiers.

**What to check**: VoiceOver announces device names and status in the list; Add Device button has a meaningful label; error alerts are announced by VoiceOver.  
**Symptom if it regresses**: VoiceOver announces "Button" or "unlabeled" for device rows; error alerts are not announced; Add Device is not reachable by VoiceOver.  
**Existing coverage**: TC-DD-018 (note: written for popup feature — create a discovery-specific VoiceOver check for this area)

---

### 🟢 LOW Risk

| Subsystem | Reason it is LOW risk | Recommended check |
|---|---|---|
| `SYS-POPUP` | Popups are not part of the Discovery or Connection flow; no shared state | Skip — no regression expected |
| `SYS-CTRL` | UCI controls are not active until after connection; no code path intersection during discovery | One quick post-connect control tap to confirm the design is interactive |

---

## Integration Points

### Q-SYS Core Protocol

| Direction | Event / Command / Property | Risk |
|---|---|---|
| App → Core | TCP connection initiation on port 1702 | 🔴 HIGH — any change to connection setup logic breaks this |
| Core → App | Design metadata / UCI layout response | 🟡 MED — malformed response can cause blank UCI render |
| Core → App | Authentication challenge / passcode validation | 🔴 HIGH — incorrect handling means users cannot connect to protected designs |

### iOS APIs

| API | Used For | Risk |
|---|---|---|
| `Network.framework` (NWBrowser) | mDNS service discovery | 🔴 HIGH — listener lifecycle directly drives device list population |
| `Network.framework` (NWConnection) | TCP connection to Core | 🔴 HIGH — connection state machine underpins the entire connect flow |
| `UserDefaults` | Saving discovered/connected device history | 🟡 MED — UserDefaults key changes or schema changes break Recents |
| `UIApplication` lifecycle | Background/foreground transition detection | 🟡 MED — mDNS scanner must restart correctly on foreground |

### Persistence & Shared State

| Store | Data | Risk |
|---|---|---|
| `UserDefaults` | Saved device list, Recents timestamps | 🔴 HIGH — data format changes break Recents across app updates |
| In-memory device list state | Currently discovered devices, online/offline status | 🟡 MED — stale in-memory state can show wrong device status after reconnect |

---

## Smoke Suite

> _Run this suite first. If any smoke test fails, halt and investigate before continuing the full regression pass._  
> **Target**: Complete all 5 checks in under 10 minutes on one iPad.

| ID | Title | Subsystem | TC Reference | Est. Time |
|----|-------|-----------|--------------|-----------|
| SMOKE-DD-001 | Core appears in device list via mDNS | `SYS-DISC` + `SYS-NET` | TC-DD-001 | < 2 min |
| SMOKE-DD-002 | Tap discovered device → UCI design loads | `SYS-CONN` + `SYS-UCI` | TC-DD-003 | < 2 min |
| SMOKE-DD-003 | Disconnect → device list shown; reconnect succeeds | `SYS-NAV` + `SYS-CONN` | TC-DD-020 | < 2 min |
| SMOKE-DD-004 | Recents entry present after app restart | `SYS-STORE` | TC-DD-006 | < 2 min |
| SMOKE-DD-005 | No network → clear error; Wi-Fi restore → discovery resumes | `SYS-NET` + `SYS-ALERT` | TC-DD-014 / TC-DD-015 | < 2 min |

### Smoke Suite Execution Log

| Date | Tester | Build | Pass | Fail | Notes |
|------|--------|-------|------|------|-------|
| | | | | | |

---

## Recommended Validation Areas

### P1 — Must validate before any QE sign-off

- [ ] **mDNS discovery** (`SYS-DISC`) — Core appears in device list within 10 seconds on both iPad and iPhone → TC-DD-001, TC-DD-002
- [ ] **Connection and UCI render** (`SYS-CONN` + `SYS-UCI`) — Tapping a discovered device connects and the UCI design page renders without blank screen → TC-DD-003
- [ ] **Recents persistence** (`SYS-STORE`) — Previously connected device reappears in Recents after app kill and relaunch → TC-DD-006
- [ ] **Connection error handling** (`SYS-ALERT`) — Timeout and wrong passcode both show specific, actionable error messages → TC-DD-009, TC-DD-011

### P2 — Validate during standard regression pass

- [ ] **Manual IP entry** (`SYS-DISC`) — Valid IP connects; invalid IP shows inline validation error → TC-DD-004, TC-DD-010
- [ ] **Network interruption recovery** (`SYS-NET`) — Wi-Fi disable/enable cycle does not leave discovery in a broken state → TC-DD-015
- [ ] **App foreground/background** (`SYS-NET`) — Discovery scan resumes correctly after app foreground → TC-DD-016
- [ ] **Navigation stack integrity** (`SYS-NAV`) — Disconnect returns user to device list with no ghost screens → TC-DD-020
- [ ] **Offline state** (`SYS-ALERT`) — Airplane mode produces clear no-network error immediately → TC-DD-014
- [ ] **Device goes offline** (`SYS-DISC`) — Device removed from network shows offline/removed within 30 seconds → TC-DD-012

### P3 — Spot-check or defer

- [ ] **Duplicate entry handling** (`SYS-DISC`) — Adding the same IP twice is handled without duplication → TC-DD-013
- [ ] **iPad landscape layout** (`SYS-ORIENT`) — Device list renders without overlap in landscape on iPad → TC-DD-017
- [ ] **VoiceOver device list** (`SYS-A11Y`) — Device rows and Add Device button are correctly labeled for VoiceOver → new check required

---

## References

| Reference | Path |
|---|---|
| Test Plan | [`test-plan.md`](./test-plan.md) |
| Test Cases | [`test-cases.md`](./test-cases.md) |
| Jira Story | [`jira-story.md`](./jira-story.md) |
| Feature Spec | `openspec/specs/device-discovery/spec.md` |
| OpenSpec Change | `openspec/changes/device-discovery-connection/` |
