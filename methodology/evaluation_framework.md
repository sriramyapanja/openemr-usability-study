# Evaluation Framework

## Overview

This framework operationalizes a structured, task-based usability inspection of OpenEMR, an ONC-certified open-source electronic health record (EHR) system. It is grounded in established health informatics usability methodology, drawing on ISO 9241-11 (usability as effectiveness, efficiency, and satisfaction), Nielsen's heuristic evaluation principles, and the task-load assessment tradition in clinical informatics.

The framework was developed and applied in the context of **INFO B535 — Clinical Information Systems** at Indiana University Indianapolis, November 2024.

---

## Theoretical Grounding

### ISO 9241-11 Usability Dimensions
Usability is formally defined as the degree to which a system enables specified users to achieve specified goals with **effectiveness**, **efficiency**, and **satisfaction** in a specified context of use (ISO, 1998). Each of these dimensions maps directly onto our measurement approach:

| ISO Dimension | Operationalization in This Study |
|---|---|
| Effectiveness | Task completion (pass / fail at score = 10) |
| Efficiency | Time burden score (0–9 scale) |
| Satisfaction | Qualitative workflow gap annotations and team discussion |

### Nielsen's Heuristic Evaluation
Ten usability heuristics — including visibility of system status, match between system and the real world, user control and freedom, error prevention, and flexibility — served as interpretive lenses during gap annotation. Tasks that scored high on difficulty were cross-referenced against the heuristic most plausibly implicated in that friction.

### Cognitive Load Theory
Tasks were also analyzed through the lens of cognitive load theory (Sweller, 1988). Navigation depth, prerequisite steps, and alert stacking were treated as sources of **extraneous cognitive load** — load imposed by interface design rather than task complexity — and flagged accordingly.

---

## Study Design

### Scope
- **25 structured tasks** spanning six clinical and administrative workflow categories
- **4 synthetic case studies** used to ground tasks in realistic, clinically plausible scenarios
- All data is entirely synthetic; no real patient records, PHI, or live production data were used (see `NOTICE.md`)

### Workflow Categories Assessed

| Category | Task IDs | Description |
|---|---|---|
| Patient Registration | PR_001 – PR_005 | Demographic entry, duplicates, proxy relationships, status changes |
| Vital Signs Entry | VS_001 – VS_003 | Triage capture, unit conversion, pediatric ranges |
| Diagnosis Documentation (ICD-10) | DX_001 – DX_004 | Code lookup, rule-out phrasing, visit-note linkage |
| Medication Prescribing | RX_001 – RX_004 | New prescriptions, dose adjustment, reconciliation, alert management |
| Multi-visit Encounter Management | MV_001 – MV_004 | Encounter dating, lab attachment, note closure, scheduling |
| Referral Management | RF_001 – RF_003 | Referral creation, status tracking, result loop closure |
| Record Correction | RC_001 – RC_002 | Diagnosis removal, medication line correction |

---

## Rating Scales

### Difficulty Score (0–9 integer scale)
Reflects the perceived cognitive and procedural burden of completing the task successfully within OpenEMR's interface.

| Score | Interpretation |
|---|---|
| 0–1 | Expected: task completed intuitively with no friction |
| 2–3 | Minor friction: one navigational search, minor unfamiliarity |
| 4–5 | Moderate friction: non-obvious steps, interface hunt required |
| 6–7 | High friction: multiple prerequisite steps, interface design misaligned with workflow |
| 8–9 | Extreme friction: task nearly impossible within expected workflow |
| 10 | Task failure: could not be completed (maps to task termination in formal usability testing) |

> Note: the course rubric references a 0–10 scale inclusive; scores of 10 in the written methodology represent outright task failure. In the synthetic dataset (`task_ratings.csv`), scores are bounded to 0–9 to reflect tasks that were completed with extreme difficulty but not abandoned.

### Time Burden Score (0–9 integer scale)
Reflects the perceived time cost of the task relative to clinical expectations — i.e., how much longer the task took than it should have in a well-designed workflow.

| Score | Interpretation |
|---|---|
| 0–1 | Expected duration; no time lost to interface friction |
| 2–3 | Slightly longer than expected |
| 4–5 | Meaningfully slower; interruption to clinical workflow |
| 6–7 | Substantially slower; clinician frustration likely |
| 8–9 | Excessively slow; patient care impact possible |
| 10 | Task not completed in allocated time |

---

## Task Rubric Design

### Task Construction Principles
Each of the 25 tasks was constructed to satisfy the following design criteria:

1. **Clinical plausibility** — every task reflects a workflow action a clinician or administrative user would realistically perform in a primary care or emergency department context.
2. **Scope specificity** — tasks are bounded to a single workflow action or decision node; compound tasks were decomposed into separate items.
3. **Synthetic patient grounding** — tasks are associated with one of four synthetic patient personas (Yuki Fitzgerald, Raj Murphy, Fatima Lindstrom, Chen O'Connor) to simulate real care continuity.
4. **Rater independence** — each team member completed tasks independently before group discussion to reduce anchoring bias in scoring.
5. **Gap documentation** — every task included a structured field to capture whether a workflow gap was identified, its nature, and a preliminary recommendation.

### Workflow Gap Coding
A workflow gap was defined as any interface or design characteristic that:
- Required users to complete prerequisite steps not expected by clinical convention
- Prevented users from completing a standard clinical correction (e.g., removing an erroneous entry)
- Generated excessive alerts or navigation steps for high-frequency tasks
- Misaligned coded data structures with natural clinical language

Gaps were coded as binary (Yes / No) per task and annotated with a plain-language description and a corresponding recommendation (see `data/synthetic/task_ratings.csv`).

---

## Team Roles and Rater Agreement

| Role | Team Member | Usability Function |
|---|---|---|
| Secretary & Lead | Himapriya Telu | Coordination, meeting facilitation, timeline |
| Data Expert & Graphics | Lavanya Ruthala | Data analysis, visual communication of findings |
| Clinical Worker | Sai Sakthi Rao Lendale | Case study design, clinical accuracy review |
| Librarian & Chief Writer | Sri Ramya Panja | Resource management, documentation, final report |

All four team members completed structured task walkthroughs. Scores were shared in weekly Zoom sessions; disagreements were resolved by discussion and consensus, with the rationale documented in meeting notes.

---

## Limitations

- **Single-institution, student-rater sample** — scores reflect perceptions of graduate students in health informatics, not practicing clinicians. Generalizability to operational clinical settings should be treated with caution.
- **Synthetic data only** — scenarios were constructed for educational purposes; edge cases present in real clinical populations were not systematically sampled.
- **No formal inter-rater reliability statistic** — rater agreement was achieved through consensus discussion rather than a quantified kappa coefficient, which would strengthen future iterations.
- **Snapshot evaluation** — this assessment captures a single version of OpenEMR at a point in time; the software is actively maintained and findings may not apply to future releases.

---

## References

- International Organization for Standardization. (1998). *ISO 9241-11: Ergonomic requirements for office work with visual display terminals — Part 11: Guidance on usability.* ISO.
- Nielsen, J. (1994). *Usability inspection methods.* Wiley.
- Sweller, J. (1988). Cognitive load during problem solving: Effects on learning. *Cognitive Science, 12*(2), 257–285.
- Patel, V. L., Kaufman, D. R., & Kannampallil, T. (2021). Human-computer interaction, usability, and workflow. In E. H. Shortliffe, J. J. Cimino, & M. F. Chiang (Eds.), *Biomedical Informatics* (5th ed., pp. 153–175). Springer.
- Rizvi, R. F., et al. (2017). Usability evaluation of electronic health record system around clinical notes usage. *Applied Clinical Informatics, 8*(4), 1095–1105.
