# Test Plan: Device Discovery and Connection

**Feature**: Device Discovery and Connection  
**Platform**: Q-SYS iOS Viewer  
**Version**: Q-SYS Core 9.10 / iOS Viewer 9.10  
**Author**: QE Team  
**Date**: 2026-05-20  
**Status**: Approved  

---

## 1. Objective

- Verify that the Q-SYS iOS Viewer discovers Q-SYS Core devices on the local network using mDNS without manual configuration
- Confirm that the user can manually add a Core device by IP address when mDNS discovery is unavailable
- Validate that the app displays meaningful feedback when no devices are found or when a connection attempt fails
- Ensure the device list updates in real time as devices appear or disappear on the network
- Verify that previously connected devices are remembered and shown in a Recents list
- Confirm the connection flow works on both iPad (landscape) and iPhone (portrait) form factors

---

## 2. Scope

### In Scope

- Automatic device discovery via mDNS on the local network
- Manual device entry by IP address or hostname
- Device list display, refresh, and filtering
- Connection initiation and authentication (passcode-protected designs)
- Recents list persistence across app restarts
- Error states: device not found, wrong passcode, connection timeout, network unavailable
- iOS version compatibility: iOS 16.x and iOS 17.x
- iPad Pro 12.9" (landscape and portrait) and iPhone 15 (portrait)
- Light mode and dark mode
- VoiceOver and Dynamic Type accessibility
- Network interruption and reconnection during discovery and connection

### Out of Scope

- Q-SYS Designer and Q-SYS Core internal discovery implementation
- Cross-subnet / WAN device discovery (future release)
- Bluetooth device pairing
- Android and web platform equivalents
- Performance benchmarking of discovery latency

---

## 3. Test Approach

Testing for this feature is **manual** and executed on physical iOS devices.

| Approach | Description |
|---|---|
| **Functional testing** | Execute step-by-step test cases covering all in-scope scenarios |
| **Exploratory testing** | Unscripted sessions to uncover edge cases and UX issues in the device list |
| **Regression testing** | Re-execute previously passing Q-SYS Viewer navigation cases to confirm no regressions |
| **Accessibility testing** | Manual VoiceOver navigation and Dynamic Type evaluation of the discovery and connection screens |

---

## 4. Test Environment

| Item | Value |
|---|---|
| **Platform** | Q-SYS iOS Viewer app on physical device |
| **iOS Versions** | iOS 17.4 (primary), iOS 16.7 (secondary) |
| **Device Models** | iPad Pro 12.9" (M2), iPhone 15 |
| **Q-SYS Core** | Core 9.10 on Q-SYS Core 510i |
| **Network** | Single subnet, mDNS enabled; secondary: mDNS blocked (VLAN) |
| **Q-SYS Design File** | `qe-test-design-9.10.qsys` |
| **App Build** | TestFlight build 9.10.0 (build 1000) |

---

## 5. Test Scenarios

### Functional

| # | Scenario |
|---|---|
| F-01 | The app automatically discovers a Q-SYS Core on the same subnet and displays it in the device list |
| F-02 | The user can manually add a Core device by entering its IP address |
| F-03 | The user connects to a discovered device and reaches the design's control page |
| F-04 | The user connects to a passcode-protected design and is prompted for the passcode |
| F-05 | A previously connected device appears in the Recents list after an app restart |
| F-06 | The device list refreshes when the user pulls to refresh |

### Negative

| # | Scenario |
|---|---|
| N-01 | The app displays an appropriate empty state when no devices are found on the network |
| N-02 | Entering an incorrect passcode shows an error; the user can retry |
| N-03 | Entering an invalid IP address during manual entry shows a validation error |
| N-04 | Attempting to connect to an unreachable device shows a timeout error |

### Edge Cases

| # | Scenario |
|---|---|
| E-01 | A Q-SYS Core that was visible in the list goes offline — the list updates to reflect its unavailability |
| E-02 | The user adds a duplicate device (same IP) — the app deduplicates or shows a warning |
| E-03 | The user attempts to connect while in airplane mode |
| E-04 | The app is backgrounded during discovery, then foregrounded — discovery resumes correctly |

### Accessibility

| # | Scenario |
|---|---|
| A-01 | VoiceOver can navigate the device list and activate a device to connect |
| A-02 | Dynamic Type (Accessibility XL) does not truncate device names or status labels |

### Regression

| # | Scenario |
|---|---|
| R-01 | The main navigation (Home, Settings) is reachable and functional after using Device Discovery |
| R-02 | The Settings screen retains all previously saved preferences after a discovery session |

---

## 6. Pass / Fail Criteria

### Pass

- All Functional and Negative scenarios pass on iPad Pro 12.9" and iPhone 15
- No P1 or P2 defects remain open at time of sign-off
- All Accessibility scenarios pass with VoiceOver enabled on iOS 17.4
- Regression scenarios show no change in behavior

### Fail

- Any Functional scenario results in a crash, data loss, or incorrect navigation state
- Any P1 defect is open at time of sign-off
- Device Discovery is non-functional on all tested device configurations

---

## 7. Risks and Mitigations

| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|------------|
| mDNS unreliable in test lab network | Med | High | Configure dedicated VLAN with verified mDNS passthrough; document lab setup |
| Q-SYS Core firmware unstable during testing | Med | High | Pin Core to 9.10 release candidate; coordinate build availability with Core team |
| iOS Viewer TestFlight build delayed | Low | High | Track build pipeline daily; identify earliest test date before sprint starts |
| Physical device unavailable (shared pool) | Low | Med | Book iPad and iPhone 24h in advance via device calendar |
| mDNS behavior differs across iOS versions | Med | Med | Run full discovery suite on both iOS 17.4 and iOS 16.7 |

---

## 8. Dependencies

- [ ] iOS Viewer 9.10.0 TestFlight build available in the test lab
- [ ] Q-SYS Core 9.10 firmware flashed to Core 510i in the test lab
- [ ] `qe-test-design-9.10.qsys` design file deployed to the test Core
- [ ] Test lab network verified: mDNS enabled on primary VLAN, mDNS blocked on secondary VLAN
- [ ] Test cases authored and reviewed (see `test-cases.md`)

---

## 9. References

| Reference | Link / Path |
|---|---|
| Feature Spec | `openspec/specs/device-discovery/spec.md` |
| OpenSpec Change | `openspec/changes/device-discovery-connection/` |
| Related Jira Story | `[QE] Device Discovery and Connection — Test Execution` |
| Q-SYS iOS Viewer Docs | Internal Confluence: iOS Viewer User Guide |
| Previous Test Plan | `archive/test-plan-viewer-9.9.md` |
