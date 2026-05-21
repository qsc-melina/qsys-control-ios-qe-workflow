# Jira Story: [QE] Device Discovery and Connection — Test Execution

---

## Metadata

```
Summary:       [QE] Device Discovery and Connection — Test Execution
Issue Type:    Story
Epic Link:     QSYS-IOS-VIEWER-Q3-2026
Component:     iOS Viewer
Labels:        qe, manual-testing, ios-viewer, accessibility
Fix Version:   Q-SYS 9.10
Story Points:  5
Assignee:      <Assignee TBD>
Reporter:      <Reporter TBD>
Sprint:        iOS Viewer Sprint 22
Priority:      High
```

> **Story Type**: Test Execution

---

## Description

### Context

The Device Discovery and Connection feature enables Q-SYS iOS Viewer users to find Q-SYS Core devices on their local network automatically via mDNS, or to add a device manually by IP address. This story covers manual QE verification of the full discovery and connection flow, including error states, network interruption recovery, accessibility compliance, and regression of existing navigation behavior.

---

### Testing Scope

**In Scope:**
- Automatic mDNS device discovery on the same subnet
- Manual device entry by IP address or hostname
- Device list display, real-time status updates, and pull-to-refresh
- Connection to open and passcode-protected Q-SYS designs
- Recents list persistence across app restarts
- Error states: no devices found, wrong passcode, invalid IP, connection timeout
- Network interruption and reconnection during discovery
- App foreground/background state transitions
- iOS 16.x and iOS 17.x on iPad Pro 12.9" and iPhone 15
- Light mode and dark mode
- VoiceOver and Dynamic Type (Accessibility XL)

**Out of Scope:**
- Cross-subnet / WAN device discovery (not implemented in 9.10)
- Q-SYS Core internal mDNS implementation details
- Bluetooth device pairing
- Android and web platform equivalents
- Performance benchmarking of discovery latency

---

### Test Environment

| Item | Value |
|---|---|
| **Platform** | Q-SYS iOS Viewer on physical iOS device |
| **iOS Version** | iOS 17.4 (primary), iOS 16.7 (secondary) |
| **Device Models** | iPad Pro 12.9" (M2), iPhone 15 |
| **Q-SYS Core Version** | Core 9.10 |
| **Core Hardware** | Q-SYS Core 510i |
| **Network Topology** | Same subnet, mDNS enabled; secondary: mDNS blocked (VLAN) |
| **Q-SYS Design File** | `qe-test-design-9.10.qsys` |

---

### References

| Reference | Link |
|---|---|
| Test Plan | [`test-plan.md`](./test-plan.md) |
| Test Cases | [`test-cases.md`](./test-cases.md) |
| Feature Spec | `openspec/specs/device-discovery/spec.md` |
| Parent Epic | `QSYS-IOS-VIEWER-Q3-2026` |
| OpenSpec Change | `openspec/changes/device-discovery-connection/` |

---

## Acceptance Criteria

**AC-01**: Given the iOS device and Q-SYS Core are on the same subnet with mDNS enabled, when the user opens Q-SYS iOS Viewer, then the Core appears in the device list within 10 seconds without manual configuration.

**AC-02**: Given the user taps the Add Device button and enters a valid IP address, when the user taps Add, then the device is added to the list and a connection can be initiated.

**AC-03**: Given a Q-SYS Core is visible in the device list and the user taps it, when the connection succeeds, then the app navigates to the design's control page with the Core name displayed in the navigation bar.

**AC-04 (Negative)**: Given a connection attempt is made to an unreachable device, when the connection times out, then the app displays a clear error message within 15 seconds and offers a retry option without crashing.

**AC-05 (Negative)**: Given the user enters an incorrect passcode for a protected design, when they tap Connect, then an error message is shown and the user can retry with a different passcode.

**AC-06 (Accessibility)**: Given VoiceOver is enabled on the iOS device, when the user navigates the Device Discovery and Connection flow, then all interactive elements are reachable via swipe gestures and announce descriptive labels.

---

## Sub-Tasks

> _Create these as child issues in Jira linked to this story._

- [ ] **ST-01**: Author test cases — Functional (7 cases, P1/P2) — author: `<Assignee TBD>`
- [ ] **ST-02**: Author test cases — Negative / Edge Cases (6 cases, P2/P3) — author: `<Assignee TBD>`
- [ ] **ST-03**: Execute test cases — Functional on iPad Pro 12.9" + iPhone 15 (TC-DD-001 through TC-DD-007)
- [ ] **ST-04**: Execute test cases — Network scenarios (TC-DD-014, TC-DD-015)
- [ ] **ST-05**: Execute test cases — iOS Platform scenarios (TC-DD-016, TC-DD-017)
- [ ] **ST-06**: Execute test cases — Accessibility (TC-DD-018, TC-DD-019) with VoiceOver + Dynamic Type
- [ ] **ST-07**: Execute regression suite (TC-DD-020)
- [ ] **ST-08**: Log and triage all defects found; retest fixed issues; update Execution Log
- [ ] **ST-09**: QE sign-off — update story with final results, link defects, add sign-off comment

---

## Definition of Done

- [ ] All P1 test cases (TC-DD-001 through TC-DD-004) pass on iPad Pro 12.9" and iPhone 15
- [ ] All P2 test cases pass on at least one device; known failures have logged Jira defects
- [ ] No P1 or P2 defects remain open at time of sign-off
- [ ] TC-DD-018 and TC-DD-019 pass with VoiceOver and Accessibility XL on iOS 17.4
- [ ] TC-DD-020 (regression) passes
- [ ] Execution Log in `test-cases.md` is filled with tester, date, build, and result for each case
- [ ] QE sign-off comment added to this Jira story: "QE PASS — [date] — [tester] — [build]"
- [ ] All linked defects are either Closed (Fixed) or have an accepted Won't Fix justification

---

## Notes

- **Dependency**: iOS Viewer 9.10.0 TestFlight build must be available before ST-03 can begin. Track with `#ios-viewer-builds` Slack channel.
- **Lab note**: Confirm the test lab's primary VLAN has mDNS passthrough enabled before starting. Contact network-ops if not.
- **Scope risk**: mDNS behavior on iOS 16.7 may differ from iOS 17.4 — run TC-DD-001 on both iOS versions.
