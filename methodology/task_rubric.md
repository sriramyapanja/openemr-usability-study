# Task Rubric

## Overview

This rubric documents the 25 structured tasks used to evaluate OpenEMR usability. Tasks span seven workflow categories and are grounded in four synthetic patient personas. Each task was rated on **difficulty** (0–9) and **time burden** (0–9), and assessed for workflow gaps. Full numerical ratings are available in `data/synthetic/task_ratings.csv`.

For scoring scale definitions, see `methodology/evaluation_framework.md`.

---

## Synthetic Patient Personas

| Persona | Age | Key Clinical Themes |
|---|---|---|
| **Yuki Fitzgerald** | 60, genderqueer | Chronic osteoarthritis, lumbar spinal stenosis; minor head contusion (ED visit); multi-visit continuity |
| **Raj Murphy** | 39, male | Essential hypertension, Type 2 diabetes; acute ankle injury (fall into empty pool); orthopedic referral |
| **Fatima Lindstrom** | 55, female | Type 2 diabetes with nephropathy; abrasion after pedestrian collision; multi-visit chronic care |
| **Chen O'Connor** | 44, female | Acute bronchitis; wrist fracture (bathtub fall); multi-visit with record correction scenarios |

---

## Task Categories and Items

### Category 1: Patient Registration (PR)

| Task ID | Description | Difficulty | Time | Gap? |
|---|---|---|---|---|
| PR_001 | Register Yuki Fitzgerald with full demographics and insurance placeholder | 3 | 4 | No |
| PR_002 | Update Raj Murphy's contact information after phone number change | 2 | 3 | No |
| PR_003 | Attach guardian proxy relationship to Fatima Lindstrom teaching case | 5 | 6 | No |
| PR_004 | Verify duplicate record warning when entering a similar-name test for Chen O'Connor | 4 | 5 | No |
| PR_005 | Mark patient deceased and end active referrals (synthetic closure workflow) | 6 | 7 | No |

**Category Observations:** Registration tasks were generally achievable, with friction concentrated in proxy relationship mapping (PR_003) and the deceased-patient closure workflow (PR_005). The latter revealed sparse cross-module status propagation — a systemic design gap even where it did not trigger a formal gap flag.

---

### Category 2: Vital Signs Entry (VS)

| Task ID | Description | Difficulty | Time | Gap? |
|---|---|---|---|---|
| VS_001 | Record vitals for Yuki Fitzgerald during triage-style encounter flow | **8** | **9** | **Yes** |
| VS_002 | Amend weight and BMI after unit mismatch for Raj Murphy | 5 | 6 | No |
| VS_003 | Enter pediatric vitals with age-based ranges for Fatima Lindstrom scenario | 6 | 7 | No |

**Gap Identified — VS_001:** Vital signs cannot be recorded unless an encounter is first created. This prerequisite is not surfaced to users proactively, requiring a navigational detour before any clinical measurement can be documented. This misaligns with clinical convention, in which vital signs are typically captured at triage — before a formal encounter is opened.

**Recommendation:** Support configurable pre-encounter vitals capture, or auto-instantiate a minimal encounter stub at triage with a clear audit trail.

---

### Category 3: Diagnosis Documentation Using ICD-10 (DX)

| Task ID | Description | Difficulty | Time | Gap? |
|---|---|---|---|---|
| DX_001 | Add primary ICD-10 hypertension code for Chen O'Connor problem list | 4 | 5 | No |
| DX_002 | Specify laterality and Z-code for Raj Murphy follow-up | 6 | 7 | No |
| DX_003 | Document acute URI and link to Fatima Lindstrom visit note template | 5 | 6 | No |
| DX_004 | Validate rule-out documentation using ICD-10 phrasing for Yuki Fitzgerald | 7 | 8 | No |

**Category Observations:** ICD-10 search and code selection imposed moderate to high friction across tasks. Attribute granularity (laterality, encounter type, Z-codes) was scattered across interface panels rather than presented in a unified diagnosis-editing widget. Rule-out documentation (DX_004) received the highest scores in this category, reflecting a meaningful semantic gap between clinical narrative language and the structured coding interface.

---

### Category 4: Medication Prescribing (RX)

| Task ID | Description | Difficulty | Time | Gap? |
|---|---|---|---|---|
| RX_001 | Initiate new lisinopril prescription with sig and pharmacy for Chen O'Connor | 5 | 6 | No |
| RX_002 | Adjust atorvastatin dose after lab review for Yuki Fitzgerald | 4 | 5 | No |
| RX_003 | Discontinue duplicate therapy alert for Raj Murphy polypharmacy scenario | 7 | 8 | No |
| RX_004 | Medication reconciliation across simulated admission for Fatima Lindstrom | 8 | 8 | No |

**Category Observations:** Alert management (RX_003) and medication reconciliation (RX_004) imposed the highest burden in this category. Alert stacking without prioritization — a known driver of alert fatigue in EHR literature — was directly observed during RX_003. The reconciliation table in RX_004 lacked the visual grouping and net-change semantics needed for safe, efficient decision-making.

---

### Category 5: Multi-visit Encounter Management (MV)

| Task ID | Description | Difficulty | Time | Gap? |
|---|---|---|---|---|
| MV_001 | Open correct encounter when two same-day edits exist for Raj Murphy | 7 | 7 | No |
| MV_002 | Link lab results to active encounter for Yuki Fitzgerald | 5 | 6 | No |
| MV_003 | Close encounter and route unsigned note task for Chen O'Connor | 6 | 7 | No |
| MV_004 | Schedule follow-up embedded from Fatima Lindstrom encounter summary | 4 | 5 | No |

**Category Observations:** The encounter picker (MV_001) was ambiguous when multiple same-day entries existed, raising a risk of documenting in the wrong encounter context — a patient safety concern. Encounter closure (MV_003) was further complicated by split responsibilities between note signing and billing readiness.

---

### Category 6: Referral Management (RF)

| Task ID | Description | Difficulty | Time | Gap? |
|---|---|---|---|---|
| RF_001 | Create cardiology referral with clinical question for Yuki Fitzgerald | 5 | 6 | No |
| RF_002 | Track referral status for Raj Murphy and document receipt | 6 | 7 | No |
| RF_003 | Close loop on imaging referral result for Fatima Lindstrom multi-visit arc | 7 | 7 | No |

**Category Observations:** Outbound referral tracking (RF_002) was limited to message thread visibility rather than a unified referral status tracker. Loop closure (RF_003) required manually binding inbound documents to the originating referral — a step that added cognitive load and increased the risk of result loss.

---

### Category 7: Record Correction (RC)

| Task ID | Description | Difficulty | Time | Gap? |
|---|---|---|---|---|
| RC_001 | Remove incorrect ICD-10 diagnosis added to Chen O'Connor's problem list | **9** | **9** | **Yes** |
| RC_002 | Correct wrong medication line after entry error for Raj Murphy | **9** | **9** | **Yes** |

**Gap Identified — RC_001 & RC_002:** These are the highest-scoring tasks in the study. Once a diagnosis or medication is added to a patient record in OpenEMR, it cannot be removed. Users have no supported pathway to correct entry errors short of addenda or documentation workarounds — both of which degrade record clarity and introduce safety risk. This is a fundamental violation of the "user control and freedom" heuristic and represents the most clinically significant usability finding in the study.

**Recommendation:** Implement an inactivation or retract pathway for both diagnoses and medications, incorporating reason-for-change codes and an immutable audit trail, consistent with legal health record integrity standards.

---

## Summary Score Table

| Category | Mean Difficulty | Mean Time | Gaps Identified |
|---|---|---|---|
| Patient Registration | 4.0 | 5.0 | 0 |
| Vital Signs Entry | 6.3 | 7.3 | 1 (VS_001) |
| Diagnosis (ICD-10) | 5.5 | 6.5 | 0 |
| Medication Prescribing | 6.0 | 6.75 | 0 |
| Multi-visit Encounters | 5.5 | 6.25 | 0 |
| Referral Management | 6.0 | 6.67 | 0 |
| **Record Correction** | **9.0** | **9.0** | **2 (RC_001, RC_002)** |

> Record correction is the highest-friction category by a substantial margin, with scores at ceiling. Vital signs entry contains the second most impactful gap, given its position at the earliest stage of every clinical encounter.

---

## Success Criteria

A task was considered **successfully completed** if the team member reached the intended system state within OpenEMR without task abandonment. Tasks scored 10 (complete failure) were not observed — all 25 tasks were ultimately completed, though RC_001 and RC_002 required documentation workarounds rather than a direct interface pathway.

A **workflow gap** was formally coded when the interface imposed a design-level barrier — not merely unfamiliarity — that would persist even for experienced users performing the task under time pressure.
