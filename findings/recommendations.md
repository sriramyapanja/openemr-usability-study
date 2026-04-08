# Recommendations

# Recommendations

## Overview

The following recommendations are derived directly from workflow gap analysis and task scoring across 25 structured usability tasks in OpenEMR. They are organized by priority tier and mapped to specific gaps, heuristics, and implementation strategies. Each recommendation is scoped to be actionable by a multidisciplinary team of usability analysts, UX designers, and developers — consistent with the human-centered design model described in Rizvi et al. (2017) and Meloncon (2017).

---

## Priority Tier 1: Critical — Patient Safety Impact

### R-01: Implement Reversible Record Correction Pathways

**Addresses:** RC_001 (diagnosis removal), RC_002 (medication correction)
**Heuristic:** User control and freedom; Error prevention

**Problem:** Diagnoses and medications cannot be removed once entered. Entry errors have no corrective pathway, forcing users into addenda or record layering.

**Recommended Solution:**
1. Introduce an **"Entered in Error" inactivation workflow** for both problem list diagnoses and medication entries
2. Require a structured reason code (dropdown + optional free text) to trigger inactivation — creates an audit trail and prevents accidental deletions
3. Display inactivated entries with clear visual differentiation: strikethrough formatting, greyed-out state, and collapsible "archived" section
4. Retain original entry data in the immutable legal health record; display an inactivation timestamp and responsible user
5. Restrict inactivation to authorized roles (e.g., attending physician, pharmacist) — not available to administrative roles — with institutional policy configuration

**Design Reference:** Epic's "Mark as Entered in Error" workflow provides a functional model for this pattern in a commercial EHR. OpenEMR's open-source architecture allows a comparable implementation without licensing constraints.

**Expected Impact:** Reduction in documentation workarounds; improved record accuracy; reduced downstream clinical decision support errors triggered by stale diagnoses.

---

## Priority Tier 2: High — Workflow Efficiency and Clinical Fidelity

### R-02: Decouple Vital Signs Capture from Encounter Creation

**Addresses:** VS_001
**Heuristic:** Match between system and the real world; Visibility of system status

**Problem:** Vital signs require an open encounter to be recorded, inverting the standard triage sequence in which vitals precede encounter creation.

**Recommended Solutions (choose one or offer as configurable option):**

**Option A — Pre-encounter vitals module:**
- Add a "Triage Vitals" mode accessible from the patient chart without a required open encounter
- Store vitals in a pending state; auto-associate to the next encounter created for that patient on the same date

**Option B — Auto-encounter stub:**
- On navigation to the vitals module with no active encounter, automatically create a minimal encounter stub (status: open/incomplete) with a visible status banner indicating the stub was system-generated
- Allow clinical staff to promote or close the stub when the full encounter is initiated

**Option C — Proactive prerequisite messaging:**
- As a minimum viable improvement: if the vitals module is accessed without an open encounter, surface a clear inline prompt — "No open encounter found. Would you like to create one now?" — with a single-click action to initiate encounter creation and return the user to vitals entry

**Expected Impact:** Reduced navigation steps at triage; alignment with clinical mental model; improved training fidelity for students and residents.

---

### R-03: Redesign Encounter Management for Multi-visit Safety

**Addresses:** MV_001 (ambiguous same-day encounter picker)
**Heuristic:** Visibility of system status; Error prevention

**Problem:** When multiple encounters exist for the same date, the encounter picker does not differentiate them sufficiently, creating risk of documenting in the wrong encounter context.

**Recommended Solution:**
- Replace the flat encounter list with a **timeline-centric encounter picker** that displays: encounter creation time, encounter type, provider, and open/closed status as distinct visual attributes
- Highlight the currently active encounter with an unambiguous visual indicator
- Add a confirmation step when a user selects a backdated or non-current encounter to document into: "You are about to document in an encounter from [date/time]. Confirm?"

**Expected Impact:** Reduction in misdocumentation risk; improved encounter traceability in multi-provider or multi-shift contexts.

---

## Priority Tier 3: Moderate — Cognitive Load Reduction

### R-04: Implement Tiered Alert Prioritization

**Addresses:** RX_003 (alert fatigue during duplicate therapy discontinuation)
**Heuristic:** Recognition rather than recall; Help users recognize, diagnose, and recover from errors

**Problem:** Alerts are presented in an undifferentiated stack, requiring users to assess each alert individually. Low-value repeating alerts dilute attention to high-risk signals.

**Recommended Solution:**
- Adopt a **three-tier alert classification system:**
  - **Tier 1 (Hard stop):** Drug-drug interaction with documented patient harm risk — requires explicit acknowledgment with reason code
  - **Tier 2 (Soft stop):** Low-evidence or patient-specific override opportunity — single-click dismiss with reason
  - **Tier 3 (Informational):** Reference-only, non-actionable — display as collapsible sidebar, not modal interrupt
- Allow institution-level suppression rules for known low-specificity alerts
- Log all alert overrides with user, timestamp, and reason for quality monitoring

**Evidence Base:** Tiered alert design has demonstrated reduction in inappropriate override rates in CPOE systems (van der Sijs et al., 2006; Ancker et al., 2017).

---

### R-05: Consolidate ICD-10 Diagnosis Editing into a Unified Panel

**Addresses:** DX_002 (laterality and Z-code attributes), DX_004 (rule-out documentation)
**Heuristic:** Aesthetic and minimalist design; Recognition rather than recall

**Problem:** Diagnosis attribute granularity (laterality, encounter type, Z-codes, rule-out status) is fragmented across multiple interface panels, requiring context-switching for a single documentation action.

**Recommended Solution:**
- Design a **consolidated diagnosis editing drawer** that surfaces all relevant attributes inline when a code is selected: laterality chip selector, encounter type toggle, Z-code suggestion, and rule-out flag
- Integrate an **NLP-assist feature** that reads free-text problem descriptions from the visit note and suggests aligned ICD-10 codes — reducing the semantic gap between clinical language and structured coding
- Surface a curated "frequent diagnoses" shortlist by specialty and visit type to reduce search overhead for common primary care conditions

---

### R-06: Simplify Navigation with Progressive Disclosure

**Addresses:** General navigation complexity across DX, RX, MV categories
**Heuristic:** Flexibility and efficiency of use; Aesthetic and minimalist design

**Problem:** Deep menu hierarchies and lateral navigation requirements add steps to high-frequency tasks.

**Recommended Solution:**
- Conduct a **click-path audit** across the 10 most frequent clinical tasks; set a target of ≤3 clicks for 80% of common actions
- Apply **progressive disclosure**: surface only primary-action controls by default; reveal advanced options on demand
- Add **task-centered quick actions** accessible from the patient chart header: New Encounter, Record Vitals, Add Diagnosis, New Prescription, Create Referral — accessible without navigating the main menu
- Implement **recently used items** memory for diagnosis codes, medications, and referral destinations to reduce repeat search overhead

---

## Summary Prioritization Table

| Recommendation | Priority | Gap Addressed | Effort | Patient Safety Impact |
|---|---|---|---|---|
| R-01: Reversible record correction | Critical | RC_001, RC_002 | High | **Direct** |
| R-02: Decouple vitals from encounter | High | VS_001 | Medium | Indirect |
| R-03: Timeline encounter picker | High | MV_001 | Medium | Indirect |
| R-04: Tiered alert prioritization | Moderate | RX_003 | Medium | **Direct** |
| R-05: Consolidated diagnosis panel | Moderate | DX_002, DX_004 | Medium | Indirect |
| R-06: Progressive disclosure / nav | Moderate | Multiple | Low–Medium | Indirect |

---

## Implementation Guidance

Implementing these recommendations effectively requires a multidisciplinary approach:

- **Usability analysts** should conduct follow-up testing with representative clinical users after each change using the same structured task rubric, enabling before/after score comparison
- **UX designers** should apply user-centered design principles and prototype major changes (R-01, R-02, R-04) before development begins
- **Developers** should leverage OpenEMR's open-source architecture to implement configurable options where possible — particularly for encounter-vitals coupling (R-02) and alert tier rules (R-04)
- **Clinical champions** — physicians, nurses, or pharmacists — should review each recommendation for clinical workflow accuracy before implementation

Iterative usability testing after each release cycle is strongly recommended to prevent regression of addressed gaps and to surface new friction as the system evolves.

---

## References

- Ancker, J. S., et al. (2017). Effects of workload, work complexity, and repeated alerts on alert fatigue. *BMC Medical Informatics and Decision Making, 17*(1).
- Meloncon, L. K. (2017). Patient experience design. *Communication Design Quarterly Review, 5*(2), 19–28.
- Nielsen, J. (1994). *Usability inspection methods.* Wiley.
- Rizvi, R. F., et al. (2017). Usability evaluation of EHR system around clinical notes usage. *Applied Clinical Informatics, 8*(4), 1095–1105.
- van der Sijs, H., et al. (2006). Overriding of drug safety alerts in CPOE. *JAMIA, 13*(2), 138–147.
