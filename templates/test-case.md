# Test Cases: {{FEATURE_NAME}}

<!-- 
  TEMPLATE INSTRUCTIONS (remove before publishing):
  - Replace all {{PLACEHOLDER}} values
  - Test Case IDs follow format: TC-<FEATURE_CODE>-<NNN>
  - Priority: P1 (blocker), P2 (significant), P3 (minor/cosmetic)
  - Status values: ⬜ Not Run | ✅ Pass | ❌ Fail | ⏭ Skipped | 🔄 Blocked
  - Actual Result and Status are filled during execution — leave blank at authoring time
-->

**Feature**: {{FEATURE_NAME}}  
**Feature Code**: {{FEATURE_CODE}}  
**Platform**: Q-SYS iOS Viewer  
**Test Plan**: [test-plan.md](./test-plan.md)  
**Author**: {{AUTHOR}}  
**Date**: {{DATE}}  
**Build**: {{BUILD_NUMBER}}  

---

## Summary Table

| ID | Title | Priority | Category | Status |
|----|-------|----------|----------|--------|
| TC-{{FEAT}}-001 | {{TITLE_001}} | P1 | Functional | ⬜ Not Run |
| TC-{{FEAT}}-002 | {{TITLE_002}} | P1 | Functional | ⬜ Not Run |
| TC-{{FEAT}}-003 | {{TITLE_003}} | P2 | Negative | ⬜ Not Run |
| TC-{{FEAT}}-004 | {{TITLE_004}} | P2 | Edge Case | ⬜ Not Run |
| TC-{{FEAT}}-005 | {{TITLE_005}} | P2 | Network | ⬜ Not Run |
| TC-{{FEAT}}-006 | {{TITLE_006}} | P2 | iOS Platform | ⬜ Not Run |
| TC-{{FEAT}}-007 | {{TITLE_007}} | P3 | Accessibility | ⬜ Not Run |
| TC-{{FEAT}}-008 | {{TITLE_008}} | P3 | Regression | ⬜ Not Run |

---

## Functional

---

### TC-{{FEAT}}-001: {{TITLE_001}}

**Priority**: P1  
**Category**: Functional  
**Test Type**: Positive  

**Preconditions**:
- [ ] {{PRECONDITION_1}}
- [ ] {{PRECONDITION_2}}
- [ ] Q-SYS iOS Viewer is installed and launched

**Steps**:
1. {{STEP_1}}
2. {{STEP_2}}
3. {{STEP_3}}

**Expected Result**:
> {{EXPECTED_RESULT}}

**Actual Result**: _(fill during execution)_  
**Status**: ⬜ Not Run  
**Notes**: _(optional)_  

---

### TC-{{FEAT}}-002: {{TITLE_002}}

**Priority**: P1  
**Category**: Functional  
**Test Type**: Positive  

**Preconditions**:
- [ ] {{PRECONDITION_1}}
- [ ] {{PRECONDITION_2}}

**Steps**:
1. {{STEP_1}}
2. {{STEP_2}}
3. {{STEP_3}}

**Expected Result**:
> {{EXPECTED_RESULT}}

**Actual Result**: _(fill during execution)_  
**Status**: ⬜ Not Run  
**Notes**: _(optional)_  

---

## Negative

---

### TC-{{FEAT}}-003: {{TITLE_003}}

**Priority**: P2  
**Category**: Negative  
**Test Type**: Negative  

**Preconditions**:
- [ ] {{PRECONDITION_1}}

**Steps**:
1. {{STEP_1}}
2. {{STEP_2}}

**Expected Result**:
> {{EXPECTED_RESULT}} An appropriate error message is displayed. The app does not crash.

**Actual Result**: _(fill during execution)_  
**Status**: ⬜ Not Run  
**Notes**: _(optional)_  

---

## Edge Cases

---

### TC-{{FEAT}}-004: {{TITLE_004}}

**Priority**: P2  
**Category**: Edge Case  
**Test Type**: Negative  

**Preconditions**:
- [ ] {{PRECONDITION_1}}

**Steps**:
1. {{STEP_1}}
2. {{STEP_2}}
3. {{STEP_3}}

**Expected Result**:
> {{EXPECTED_RESULT}}

**Actual Result**: _(fill during execution)_  
**Status**: ⬜ Not Run  
**Notes**: _(optional)_  

---

## Network

---

### TC-{{FEAT}}-005: {{TITLE_005}}

**Priority**: P2  
**Category**: Network  
**Test Type**: Negative  

**Preconditions**:
- [ ] {{PRECONDITION_1}}
- [ ] Device is connected to the test network

**Steps**:
1. {{STEP_1}}
2. On the iOS device, go to **Settings → Wi-Fi** and disable Wi-Fi.
3. Return to the Q-SYS iOS Viewer app.
4. Observe the app behavior.
5. Re-enable Wi-Fi.
6. Return to the Q-SYS iOS Viewer app.

**Expected Result**:
> The app displays a clear network error or disconnected state. When Wi-Fi is restored, the app recovers without requiring a full restart.

**Actual Result**: _(fill during execution)_  
**Status**: ⬜ Not Run  
**Notes**: _(optional)_  

---

## iOS Platform

---

### TC-{{FEAT}}-006: {{TITLE_006}}

**Priority**: P2  
**Category**: iOS Platform  
**Test Type**: Positive  

**Preconditions**:
- [ ] {{PRECONDITION_1}}
- [ ] Q-SYS iOS Viewer is in the foreground

**Steps**:
1. {{STEP_1}} (begin the flow)
2. Press the **Home button** (or swipe up) to background the app.
3. Wait 10 seconds.
4. Return to the Q-SYS iOS Viewer app via the App Switcher.

**Expected Result**:
> The app resumes to the correct state. No data is lost. The user does not need to restart the flow.

**Actual Result**: _(fill during execution)_  
**Status**: ⬜ Not Run  
**Notes**: _(optional)_  

---

## Accessibility

---

### TC-{{FEAT}}-007: {{TITLE_007}}

**Priority**: P3  
**Category**: Accessibility  
**Test Type**: Exploratory  

**Preconditions**:
- [ ] **Settings → Accessibility → VoiceOver** is enabled on the test device
- [ ] {{PRECONDITION_2}}

**Steps**:
1. Navigate to the {{FEATURE_NAME}} area of the Q-SYS iOS Viewer using VoiceOver swipe gestures.
2. Verify each interactive element announces a meaningful label.
3. Activate each button or control using the VoiceOver double-tap gesture.
4. Complete the primary user flow using only VoiceOver.

**Expected Result**:
> All interactive elements have descriptive VoiceOver labels. The primary flow can be completed without vision. No element is unreachable or silently skipped by VoiceOver.

**Actual Result**: _(fill during execution)_  
**Status**: ⬜ Not Run  
**Notes**: _(optional)_  

---

## Regression

---

### TC-{{FEAT}}-008: {{TITLE_008}}

**Priority**: P3  
**Category**: Regression  
**Test Type**: Positive  

**Preconditions**:
- [ ] {{PRECONDITION_1}}

**Steps**:
1. {{STEP_1}}
2. {{STEP_2}}

**Expected Result**:
> Existing behavior is unchanged. No regression introduced by the {{FEATURE_NAME}} change.

**Actual Result**: _(fill during execution)_  
**Status**: ⬜ Not Run  
**Notes**: _(optional)_  

---

## Execution Log

| Date | Tester | Build | Devices Tested | Pass | Fail | Blocked | Notes |
|------|--------|-------|----------------|------|------|---------|-------|
| | | | | | | | |
