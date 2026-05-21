---
Summary: "[QE] Session Recovery and Reconnect — Test Execution"
Type: Story
Component: iOS Viewer
Epic: <Epic Link TBD>
Labels: qe, manual-testing, ios-viewer, accessibility, network-resilience
Story Points: 5
Fix Version: TBD
Assignee: <Assignee TBD>
Reporter: <Reporter TBD>
Sprint: <Sprint TBD>
---

## Context

This story covers the QE execution effort for the Session Recovery and Reconnect feature on the Q-SYS iOS Viewer. The feature enables the app to automatically detect a dropped connection to a Q-SYS Core and attempt to restore the session — including the active UCI page, layer visibility state, and control values — without requiring the user to manually navigate back to the device list and reconnect. Manual reconnect is surfaced only after auto-retries are exhausted.

## Testing Scope

- Automatic reconnect triggered by Wi-Fi interruption, Core restart, and app lifecycle events (background, lock screen, force-quit)
- Reconnecting indicator UI (banner appearance, timing, dismissal)
- Session state restoration after reconnect: UCI page, layer visibility, popup state
- Manual Reconnect button behavior and error messaging after retry exhaustion
- Reconnect failure and stable offline state (no crash/loop)
- VoiceOver announcements for connection status changes
- Regression coverage for the normal connect/disconnect flow

## Out of Scope

- Initial device discovery and first-time connection (covered separately by Device Discovery QE story)
- Q-SYS Core internal failover or redundancy configurations
- Android or web platform
- Performance benchmarking of reconnect speed
- Q-SYS Designer configuration changes between reconnect cycles

## Test Environment

| Item | Value |
|---|---|
| Platform | iOS Viewer (iPad + iPhone) |
| iOS Version | iOS 18.x (latest) + iOS 16.x (minimum) |
| Q-SYS Core Version | 9.10+ |
| Network | Local LAN; dedicated QE Wi-Fi AP; separate AP available for network-switch scenarios |
| Test Device(s) | iPad Pro 12.9", iPhone 15 Pro |
| Required Design File | `qe-session-recovery-test-design.qsys` — 2 UCI pages, 1 shared layer, 1 popup |
| Core Hardware | Physical Q-SYS Core required for restart scenarios |

## References

- Test Plan: [`test-plan.md`](./test-plan.md)
- Test Cases: [`test-cases.md`](./test-cases.md)
- Regression Scope: [`regression-scope.md`](./regression-scope.md)
- Feature Spec: `openspec/specs/session-recovery-reconnect/spec.md`

---

## Acceptance Criteria

**AC-01**: Given the user is connected to a Q-SYS Core and the UCI design is displayed, when Wi-Fi is interrupted, then a reconnecting indicator appears within 5 seconds and the app automatically reconnects without user interaction once the network is restored.

**AC-02**: Given a successful auto-reconnect, when the user is returned to the design, then the UCI displays the same page the user was on before the interruption and all layer visibility states reflect the live Core state.

**AC-03**: Given all auto-reconnect retries have been exhausted, when the app has given up retrying, then a visible **Reconnect** button is displayed with a specific, actionable error message — not a generic alert.

**AC-04**: Given the user taps the **Reconnect** button while the Core is offline, when the reconnect attempt fails, then a clear failure message is displayed and the app remains stable without crashing or entering a retry loop.

**AC-05**: Given the app has been backgrounded or the device locked while a reconnect is in progress, when the app is foregrounded or the device unlocked, then the reconnect attempt resumes or completes without requiring any user navigation.

**AC-06**: Given the user taps the **Disconnect** button, when the user explicitly disconnects, then the app navigates to the device list and does not initiate any auto-reconnect attempt.

**AC-07**: Given VoiceOver is enabled and the reconnecting banner appears, when the connection state changes (reconnecting, connected, or failed), then VoiceOver announces the state change without requiring the user to navigate to the banner element.

---

## Sub-Tasks

- [ ] ST-01: Set up test environment — configure `qe-session-recovery-test-design.qsys`, reserve physical Core hardware, verify QE Wi-Fi AP is dedicated (1 day)
- [ ] ST-02: Execute Functional test cases — TC-SRR-001 through TC-SRR-008 (iPad + iPhone, P1 first) (1.5 days)
- [ ] ST-03: Execute Negative and Edge Case test cases — TC-SRR-009 through TC-SRR-016 (1 day)
- [ ] ST-04: Execute Network and iOS Platform test cases — TC-SRR-017 through TC-SRR-019 (Core restart scenarios; requires physical hardware) (0.5 days)
- [ ] ST-05: Execute Accessibility test cases — TC-SRR-020 through TC-SRR-022 (VoiceOver on iOS 16 + iOS 18) (0.5 days)
- [ ] ST-06: Execute Regression test cases — TC-SRR-023 through TC-SRR-025 (normal connect/disconnect flows) (0.5 days)
- [ ] ST-07: Run smoke suite — SMOKE-SRR-001 through SMOKE-SRR-005 against final build (< 10 min) (0.25 days)
- [ ] ST-08: Log and triage all defects; confirm P1/P2 disposition with engineering
- [ ] ST-09: QE sign-off — confirm all P1 cases pass; document any open P2/P3 defects with agreed disposition

---

## Notes

- **Core hardware dependency**: ST-04 (Core restart scenarios TC-SRR-003 and TC-SRR-017) requires a physical Q-SYS Core in the QE lab. Reserve hardware before sprint begins or mark those cases as Blocked.
- **Network environment**: Use a dedicated QE Wi-Fi AP for all reconnect tests to avoid interference from shared lab traffic. Document the AP SSID and subnet in the test run notes.
- **Smoke-first rule**: Always run the smoke suite (SMOKE-SRR-001–005) against any new build before executing the full regression pass. If any smoke test fails, block the build.
- **Regression risk**: The reconnect feature touches `SYS-CONN` directly — the normal connect/disconnect flow (TC-SRR-023, TC-SRR-024) must be verified on every build, not just the final build.
- **Story point basis**: 25 test cases = 5 story points per the QE estimation table.
