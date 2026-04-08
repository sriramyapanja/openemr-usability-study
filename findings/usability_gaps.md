# Usability Gaps

# Usability Gaps

## Overview

Three workflow gaps were formally identified across 25 structured tasks. All three represent design-level barriers — persistent friction that would affect experienced users, not merely novice unfamiliarity. They are ranked below by clinical significance.

---

## Gap 1 — Irreversible Diagnosis and Medication Entries (RC_001, RC_002)

**Severity: Critical**
**Tasks affected:** RC_001 (diagnosis removal), RC_002 (medication correction)
**Difficulty / Time scores:** 9 / 9 (both tasks)

### Description
Once a diagnosis (ICD-10 code) or medication is added to a patient record in OpenEMR, there is no supported pathway to remove or inactivate it through the standard interface. Users who enter an erroneous record have no recourse other than addenda, narrative notes, or layering a second entry to partially counteract the first — all of which degrade record clarity.

### Clinical Impact
This gap directly compromises patient safety. Incorrect diagnoses on an active problem list can:
- Influence clinical decision support alerts and order sets
- Appear in external care summaries and referral documents
- Persist across care episodes, compounding downstream clinical errors

The inability to correct medication entries similarly raises medication safety risks if a contraindicated or incorrect drug appears to be active in the record.

### Heuristic Implicated
Nielsen Heuristic #3: **User control and freedom** — the system does not support undo or correction of easily-made entry errors, forcing users into non-standard workarounds.

### Recommendation
Implement an **inactivation / retract pathway** for both diagnoses and medications:
- Allow "mark as entered in error" with a mandatory reason code
- Preserve the original entry in an immutable audit log (legal record integrity)
- Display inactivated entries with visual differentiation (strikethrough, greyed-out, collapsible) rather than deletion
- Restrict retraction to authorized roles with time-bound policies per institutional governance

---

## Gap 2 — Encounter Prerequisite for Vital Signs Entry (VS_001)

**Severity: High**
**Tasks affected:** VS_001
**Difficulty / Time scores:** 8 / 9

### Description
Vital signs — height, weight, blood pressure, pulse, temperature — cannot be recorded unless a clinical encounter has first been created and opened. The system does not surface this requirement proactively. Users navigating to the vitals module without an active encounter encounter either an empty form or an error state, requiring a context switch back to encounter creation before documentation can proceed.

### Clinical Impact
In standard clinical workflows, vital signs are captured at triage — before a physician encounter is formally opened. Requiring an encounter prerequisite inverts this sequence, adds navigation steps, and imposes extraneous cognitive load at the highest-urgency moment of patient contact. In educational and simulation contexts, this barrier also degrades training fidelity.

### Heuristic Implicated
Nielsen Heuristic #1: **Visibility of system status** — the system fails to communicate the prerequisite state before users attempt the action.
Nielsen Heuristic #2: **Match between system and the real world** — the system's model of "encounter before vitals" does not reflect clinical convention.

### Recommendation
- Allow configurable pre-encounter vitals capture as a first-class workflow
- If an encounter is architecturally required, auto-instantiate a minimal encounter stub at triage with a clear status indicator and audit entry
- Surface the prerequisite requirement as a proactive, inline message at the point of vitals navigation — not as a silent form-blocking state

---

## Gap 3 — Navigation Depth and Alert Fatigue (DX_004, RX_003, MV_001)

**Severity: Moderate–High**
**Tasks affected:** Multiple (DX_004, RX_003, RX_004, MV_001)
**Difficulty / Time scores:** 7–8 range

### Description
Several tasks revealed a compounding pattern of interface friction: deep menu hierarchies requiring multiple lateral navigation moves, fragmented attribute panels for ICD-10 diagnosis coding, and undifferentiated alert stacking during prescribing workflows. No single task produced a formal gap flag, but the cumulative burden across these tasks is clinically significant.

**Alert fatigue** was directly observed during RX_003 (duplicate therapy discontinuation). Alerts appeared without priority tiering, requiring users to read and assess each one independently rather than routing attention to high-risk signals. This mirrors alert fatigue patterns documented in the EHR usability literature (van der Sijs et al., 2006; Ancker et al., 2017).

**Navigation complexity** was most pronounced in DX_004 (rule-out documentation) and MV_001 (encounter selection under ambiguous same-day edits), where users had to context-switch between multiple screens to accomplish a single clinical documentation objective.

### Clinical Impact
- Alert fatigue reduces clinician responsiveness to genuinely high-risk signals
- Navigation depth increases time-on-task and cognitive load, particularly under time pressure
- Ambiguous encounter selection (MV_001) creates a risk of documenting in the wrong encounter — a patient safety concern

### Recommendations
- Implement tiered alert prioritization: suppress or batch low-value repeating alerts; escalate high-risk signals visually
- Consolidate diagnosis editing into a single panel with inline attribute selection (laterality, encounter type, Z-codes)
- Introduce a timeline-centric encounter picker with visual differentiation for same-day edits
- Apply progressive disclosure principles to reduce visible interface complexity for high-frequency tasks

---

## Cross-Cutting Theme: Misalignment with Clinical Mental Models

All three identified gaps share a common root: the system's interaction model does not align with how clinicians think about and sequence their work. This is a foundational usability principle — systems should conform to users' mental models, not require users to adapt their mental models to the system (Norman, 2013).

In EHR contexts, this misalignment has compounding consequences: documentation errors are more likely, clinical decision support is underutilized, and training time increases. Addressing these gaps requires not only interface fixes but also a human-centered design process that involves clinical workflow mapping and iterative usability testing with representative end users.

---

## References

- Ancker, J. S., et al. (2017). Effects of workload, work complexity, and repeated alerts on alert fatigue in a clinical decision support system. *BMC Medical Informatics and Decision Making, 17*(1), 1–9.
- Norman, D. A. (2013). *The design of everyday things* (Revised ed.). Basic Books.
- Nielsen, J. (1994). *Usability inspection methods.* Wiley.
- van der Sijs, H., et al. (2006). Overriding of drug safety alerts in computerized physician order entry. *Journal of the American Medical Informatics Association, 13*(2), 138–147.
- Patel, V. L., Kaufman, D. R., & Kannampallil, T. (2021). Human-computer interaction, usability, and workflow. In *Biomedical Informatics* (5th ed., pp. 153–175). Springer.
