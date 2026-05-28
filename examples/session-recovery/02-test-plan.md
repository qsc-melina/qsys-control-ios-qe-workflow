# QE Test Plan: Session Recovery and Reconnect

**Feature**: Session Recovery and Reconnect  
**Feature Code**: SRR  
**Platform**: Q-SYS iOS Viewer (iPad + iPhone)  
**Q-SYS Core Version**: 9.10+  
**Author**: QE Team  
**Date**: 2026-05-21  
**Status**: Draft  

---

## Related Documents

- Test Cases: [test-cases.md](./test-cases.md)
- Regression Scope: [regression-scope.md](./regression-scope.md)
- Jira Story: [jira-story.md](./jira-story.md)

---

## Feature Summary

Session Recovery and Reconnect enables the Q-SYS iOS Viewer to automatically detect a dropped connection to a Q-SYS Core and attempt to restore the session without requiring the user to manually navigate back to the device list and reconnect. When the connection is lost — due to a network interruption, Core restart, app backgrounding, or network topology change — the app displays a visible reconnecting indicator and silently retries the connection using configurable backoff logic. If reconnection succeeds, the app restores the user's last known design state (current UCI page, visible layers, popup state). If all automatic retries are exhausted, the app presents a manual reconnect option with a clear error message.

---

## Test Scope

### In Scope

- Automatic reconnect triggered by network interruption (Wi-Fi drop, switch, or momentary loss)
- Automatic reconnect triggered by Q-SYS Core restart or shutdown
- Automatic reconnect triggered by app entering background and returning to foreground
- Reconnect UI: connecting banner, retry counter, timeout behavior
- Session state restoration after successful reconnect (page, layer visibility, popup state)
- Manual reconnect action when auto-reconnect is exhausted
- Reconnect failure states: Core not found, authentication failure, timeout exceeded
- Reconnect behavior after device lock screen / unlock
- Reconnect behavior across network switches (e.g., Wi-Fi to different SSID)
- Connection status VoiceOver announcements
- iPad (landscape) and iPhone (portrait) form factors
- iOS 16, iOS 17, iOS 18

### Out of Scope

- Initial device discovery and first-time connection (covered by `qe-test-plan` for Device Discovery)
- Q-SYS Core internal failover or redundancy behavior
- Android or web platform
- Performance benchmarking of reconnect speed
- Q-SYS Designer configuration changes between sessions

---

## Test Objectives

- **Verify** that the app automatically detects a dropped connection and initiates a reconnect attempt without user interaction
- **Confirm** that a visible reconnecting indicator is displayed while reconnection is in progress
- **Validate** that after a successful reconnect, the user's last UCI page, layer visibility, and popup state are correctly restored
- **Ensure** that a manual reconnect option is presented when all auto-reconnect retries are exhausted
- **Verify** that connection loss and recovery are correctly announced by VoiceOver for accessibility
- **Confirm** that reconnect behaves correctly after the app has been backgrounded, locked, or killed

---

## Test Approach

- **Manual testing** on physical iOS devices only — no simulators
- Exploratory testing of reconnect timing, UI feedback, and session state restoration edge cases
- Regression testing of the standard connect/disconnect flow to confirm no regressions
- Network conditions simulated by toggling airplane mode, disabling Wi-Fi, and power-cycling the Q-SYS Core
- App lifecycle events simulated by pressing Home, locking the device, and force-quitting and relaunching the app

---

## Test Environment

| Item | Value |
|---|---|
| Devices | iPad Pro 12.9" (primary), iPhone 15 Pro |
| iOS Versions | iOS 18.x (latest), iOS 16.x (minimum supported) |
| Q-SYS Core Version | 9.10+ (hardware Core required for restart scenarios) |
| Network | Local LAN, same subnet as Core; separate Wi-Fi AP available for network-switch scenarios |
| Required Design File | `qe-session-recovery-test-design.qsys` — design with at least 2 UCI pages, 1 shared layer, and 1 popup configured |
| Q-SYS Core Hardware | Physical Core (not emulator) required for Core restart tests |

---

## Test Scenarios

### Functional

1. App automatically reconnects after a brief Wi-Fi interruption (<30 seconds)
2. App automatically reconnects after the Q-SYS Core is restarted
3. Reconnecting indicator (banner or spinner) is displayed while reconnection is in progress
4. Session state — current UCI page and visible layers — is fully restored after successful reconnect
5. After successful reconnect, all UCI controls respond to user input without requiring a fresh navigation
6. App presents a manual **Reconnect** button after auto-retry limit is exhausted

### Negative

7. Manual reconnect tap while Core is still offline shows a "Could not connect" error
8. Network is never restored — app remains in a stable offline state without crashing or looping
9. Core restarted with a design change (different page count) — app handles gracefully

### Edge Cases

10. Connection is lost mid-control interaction (e.g., slider being dragged) — reconnect completes; control resumes from live Core state
11. Connection is lost and restored repeatedly within a 60-second window (flapping network) — app does not enter an error loop
12. App is backgrounded during an active reconnect attempt — reconnect resumes correctly on foreground
13. Device is locked mid-reconnect — reconnect resumes on unlock
14. Core is restarted while a popup is open — popup is dismissed on reconnect; session state is freshly retrieved

### iOS Platform

15. App is backgrounded (Home press) while connected; Core restarted while in background; user foregrounds — app initiates reconnect immediately
16. App is force-quit and relaunched while Core is offline — app presents device list with last-known device available to connect
17. Device locked and unlocked after network interruption — app reconnects without requiring manual navigation

### Accessibility

18. VoiceOver announces "Reconnecting" when the reconnect banner appears
19. VoiceOver announces "Connected" or a failure message when reconnect resolves
20. Manual **Reconnect** button has a meaningful VoiceOver label and is reachable by swipe navigation

### Regression

21. Normal connect flow (device list → tap device → design loads) is unaffected by this feature
22. Explicit disconnect via the **Disconnect** button still returns the user to the device list correctly
23. First launch with no saved devices still shows the empty device list with no reconnect UI shown

---

## Pass / Fail Criteria

**Pass**:
- All P1 and P2 scenarios pass on both iPad and iPhone
- No P1 defects open at time of QE sign-off
- Reconnect completes and session state is restored within 30 seconds for clean network restore
- No crashes, hangs, or UI loops observed during any test run

**Fail**:
- Any P1 scenario fails (auto-reconnect not initiated; session state not restored; manual reconnect not presented)
- Any crash or data loss during reconnect
- App enters an infinite reconnect loop with no user escape

---

## Risks and Mitigations

| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|------------|
| Core hardware required for restart tests — lab availability may be constrained | Medium | High | Reserve Core hardware in the QE lab for this sprint; mark restart TCs as blocked if hardware unavailable |
| Reconnect timing depends on network conditions — flaky results on shared Wi-Fi | Medium | Medium | Use a dedicated QE Wi-Fi AP; document the AP configuration in the test environment section |
| Session state restoration may differ between Core versions — untested state schema | Low | High | Test against Core 9.10 and the latest available Core version; flag any difference as a defect |
| VoiceOver announcement timing may vary by iOS version | Low | Low | Test VoiceOver on iOS 16 and iOS 18; log as P3 if announcement is delayed but present |
| App kill and relaunch during reconnect may produce undefined state | Medium | Medium | Include explicit kill-and-relaunch test cases; capture console logs if state is unexpected |
