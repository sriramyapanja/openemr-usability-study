# OpenEMR Usability Evaluation
### Insights and Recommendations for EHR Workflow Optimization

![Academic Project](https://img.shields.io/badge/Project-Academic-003563?style=flat-square)
![Synthetic Data Only](https://img.shields.io/badge/Data-Synthetic%20Only-2e7d32?style=flat-square)
![HIPAA Not Applicable](https://img.shields.io/badge/HIPAA-Not%20Applicable-616161?style=flat-square)
![IU Indianapolis](https://img.shields.io/badge/Institution-IU%20Indianapolis-990000?style=flat-square)
![Course](https://img.shields.io/badge/Course-INFO%20B535-003563?style=flat-square)
![Status](https://img.shields.io/badge/Status-Complete-brightgreen?style=flat-square)

> Structured, task-based usability inspection of OpenEMR clinical workflows — connecting interface design choices to measurable difficulty and time-efficiency outcomes, with prioritized, evidence-based recommendations for EHR optimization.

---

## Table of Contents

- [Background](#background)
- [Synthetic Data Warning](#synthetic-data-warning)
- [Study Design](#study-design)
- [Key Findings](#key-findings)
- [Recommendations](#recommendations)
- [Methods](#methods)
- [Repository Layout](#repository-layout)
- [Team and Roles](#team-and-roles)
- [Academic Context](#academic-context)
- [References](#references)

---

## Background

EHR usability is not a design preference — it is a patient safety issue. The U.S. Joint Commission has identified EHR interface design as a contributing factor in a documented range of adverse events, from medication errors to missed diagnoses (Joint Commission, 2015). Meanwhile, physicians in the United States spend approximately two hours on EHR documentation for every one hour of direct patient care (Sinsky et al., 2016), with EHR burden cited as one of the strongest predictors of professional burnout (Shanafelt et al., 2016).

**OpenEMR** is the world's most widely installed open-source EHR, ONC-certified and deployed in over 100 countries. Its open-source architecture makes it both uniquely accessible for educational research and uniquely transparent as a subject of rigorous usability evaluation — findings here can translate directly into code.

This project applies established health informatics usability methodology — grounded in ISO 9241-11, Nielsen's heuristic evaluation framework, and cognitive load theory — to systematically evaluate OpenEMR across seven clinical workflow categories. Results are directly applicable to EHR optimization efforts in both open-source and commercial environments, including workflow parallels to systems such as Epic and Cerner.

---

---

## Study Design

### Scope

| Dimension | Detail |
|---|---|
| Tasks evaluated | 25 structured tasks across 7 workflow categories |
| Synthetic case studies | 4 clinical personas, 3 visits each (primary care + emergency) |
| Rating instrument | Difficulty score + Time burden score (0–9 integer scale per task) |
| Gap coding | Binary workflow gap flag + qualitative annotation + recommendation per task |
| Evaluation approach | Task-based usability inspection with structured scenario walkthroughs |

### Workflow Categories

| Category | Tasks | Focus |
|---|---|---|
| Patient Registration | PR_001–PR_005 | Demographics, proxies, duplicate detection, status closure |
| Vital Signs Entry | VS_001–VS_003 | Triage capture, unit conversion, pediatric ranges |
| Diagnosis (ICD-10) | DX_001–DX_004 | Code lookup, laterality, rule-out documentation |
| Medication Prescribing | RX_001–RX_004 | New Rx, dose adjustment, reconciliation, alert management |
| Multi-visit Encounters | MV_001–MV_004 | Encounter dating, lab attachment, note closure, scheduling |
| Referral Management | RF_001–RF_003 | Referral creation, status tracking, result loop closure |
| Record Correction | RC_001–RC_002 | Diagnosis removal, medication line correction |

Full task descriptions, scores, and gap annotations are available in [`data/synthetic/task_ratings.csv`](data/synthetic/task_ratings.csv).

---

## Key Findings

Three formal workflow gaps were identified. All three represent design-level barriers that persist for experienced users — not merely novice unfamiliarity. They are ranked by clinical significance.

### 🔴 Gap 1 — Irreversible Record Entries (Severity: Critical)
**Tasks:** RC_001, RC_002 | **Scores:** Difficulty 9/9, Time 9/9

Once a diagnosis or medication is entered in OpenEMR, there is no interface pathway to remove or inactivate it. Clinicians who enter an erroneous record have no recourse other than addenda or layering a second entry — both of which degrade record clarity and can propagate incorrect information into clinical decision support, external care summaries, and downstream encounters.

This directly violates Nielsen Heuristic #3 (User control and freedom) and represents the most clinically significant finding in the study.

### 🟠 Gap 2 — Encounter Prerequisite for Vital Signs (Severity: High)
**Task:** VS_001 | **Scores:** Difficulty 8/9, Time 9/9

Vital signs cannot be recorded unless a clinical encounter has already been created and opened — inverting the standard triage sequence in which vitals precede encounter initiation. The system does not surface this requirement proactively, forcing users into a context switch before any measurement can be documented.

### 🟡 Gap 3 — Navigation Depth and Alert Fatigue (Severity: Moderate–High)
**Tasks:** DX_004, RX_003, RX_004, MV_001 | **Scores:** 7–8 range

Undifferentiated alert stacking across prescribing workflows and deep menu hierarchies across documentation tasks compound cognitive load across the most time-sensitive clinical interactions.

> See [`findings/usability_gaps.md`](findings/usability_gaps.md) for full gap analysis including heuristic mapping, clinical impact assessment, and comparative context with commercial EHR systems.

---

## Recommendations

Six prioritized recommendations were developed from gap analysis and task scoring. They are scoped for implementation by a multidisciplinary team of usability analysts, UX designers, and developers.

| Priority | Recommendation | Gap(s) Addressed | Patient Safety Impact |
|---|---|---|---|
| 🔴 Critical | Implement reversible record correction with audit trail | RC_001, RC_002 | Direct |
| 🟠 High | Decouple vital signs capture from encounter creation | VS_001 | Indirect |
| 🟠 High | Timeline-centric encounter picker for multi-visit safety | MV_001 | Indirect |
| 🟡 Moderate | Tiered alert prioritization (hard stop / soft stop / info) | RX_003 | Direct |
| 🟡 Moderate | Consolidated ICD-10 diagnosis editing panel with NLP assist | DX_002, DX_004 | Indirect |
| 🟡 Moderate | Progressive disclosure and task-centered quick actions | Multiple | Indirect |

> Full implementation guidance with design references (including commercial EHR comparators) is in [`findings/recommendations.md`](findings/recommendations.md).

---

## Methods

Task-based usability inspection and structured scenario walkthroughs were combined with a case study design, grounded in three theoretical frameworks:

- **ISO 9241-11** — usability operationalized as effectiveness, efficiency, and satisfaction (ISO, 1998)
- **Nielsen's heuristic evaluation** (Nielsen, 1994) — ten usability heuristics applied as interpretive lenses during gap annotation
- **Cognitive load theory** (Sweller, 1988) — navigation depth, prerequisite steps, and alert stacking treated as sources of extraneous cognitive load

**ICD-10** code selection was validated against standard coding conventions. Clinical plausibility was cross-checked using authoritative public sources (Medscape, CDC) for education-oriented validation — not for diagnosing or treating real patients.

> Scoring scale definitions, rater agreement procedures, and task construction principles are documented in [`methodology/evaluation_framework.md`](methodology/evaluation_framework.md).

---

## Repository Layout

```
.
├── data/
│   └── synthetic/
│       ├── task_ratings.csv          # 25-task scores, gap flags, recommendations
│       ├── case_study_summaries.md   # Persona overview
│       └── README.md                 # Synthetic data disclaimer
├── docs/
│   └── clinical_context.md          # EHR usability landscape, career context
├── findings/
│   ├── usability_gaps.md            # Gap analysis: severity, heuristics, impact
│   └── recommendations.md           # 6 prioritized recommendations
├── methodology/
│   ├── evaluation_framework.md      # ISO 9241-11, heuristics, scoring scales
│   └── task_rubric.md               # All 25 tasks with scores and observations
├── LICENSE
├── NOTICE.md                        # Synthetic data declaration
└── README.md
```

---

## Team and Roles

| Name | Role | Responsibility |
|---|---|---|
| Himapriya Telu | Secretary & Lead | Project coordination, timeline management, meeting facilitation |
| Lavanya Ruthala | Data Expert & Graphics | Data analysis, visual communication of findings |
| Sai Sakthi Rao Lendale | Clinical Worker | Case study design, ICD-10 accuracy, clinical scenario realism |
| **Sri Ramya Panja** | **Librarian & Chief Writer** | **Resource management, documentation, final report authorship** |

---

## Academic Context

| Field | Detail |
|---|---|
| Institution | Indiana University Indianapolis — Luddy School of Informatics, Computing, and Engineering |
| Course | INFO B535 — Clinical Information Systems |
| Instructor | Dr. Cathy Fulton |
| Submitted | November 2024 |
| Scope | Student research methodology; not a substitute for peer-reviewed multicenter usability trials |

---

## References

Ancker, J. S., et al. (2017). Effects of workload, work complexity, and repeated alerts on alert fatigue in a clinical decision support system. *BMC Medical Informatics and Decision Making, 17*(1), 1–9.

Cresswell, K., Williams, R., & Sheikh, A. (2020). Developing and applying a formative evaluation framework for health information technology implementations. *Journal of Medical Internet Research, 22*(6), e15068. https://doi.org/10.2196/15068

ISO. (1998). *ISO 9241-11: Ergonomic requirements for office work with visual display terminals — Part 11: Guidance on usability.* International Organization for Standardization.

Joint Commission. (2015). *Sentinel event alert: Safe use of health information technology* (Issue 54).

Nielsen, J. (1994). *Usability inspection methods.* Wiley.

OpenEMR. (2024). *OpenEMR: Open-source electronic medical record software.* https://www.open-emr.org

Patel, V. L., Kaufman, D. R., & Kannampallil, T. (2021). Human-computer interaction, usability, and workflow. In E. H. Shortliffe, J. J. Cimino, & M. F. Chiang (Eds.), *Biomedical informatics: Computer applications in healthcare and biomedicine* (5th ed., pp. 153–175). Springer. https://doi.org/10.1007/978-3-030-58721-5_5

Rizvi, R. F., Marquard, J. L., Hultman, G. M., Adam, T. J., Harder, K. A., & Melton, G. B. (2017). Usability evaluation of electronic health record system around clinical notes usage. *Applied Clinical Informatics, 8*(4), 1095–1105. https://doi.org/10.4338/ACI-2017-04-RA-0067

Shanafelt, T. D., et al. (2016). Relationship between clerical burden and characteristics of the electronic environment with physician burnout and professional satisfaction. *Mayo Clinic Proceedings, 91*(7), 836–848.

Shortliffe, E. H., & Cimino, J. J. (Eds.). (2021). *Biomedical informatics: Computer applications in health care and biomedicine* (5th ed.). Springer.

Sinsky, C., et al. (2016). Allocation of physician time in ambulatory practice. *Annals of Internal Medicine, 165*(11), 753–760.

Sweller, J. (1988). Cognitive load during problem solving: Effects on learning. *Cognitive Science, 12*(2), 257–285.

van der Sijs, H., et al. (2006). Overriding of drug safety alerts in computerized physician order entry. *Journal of the American Medical Informatics Association, 13*(2), 138–147.
