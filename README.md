# OpenEMR Usability Evaluation: Insights and Recommendations for EHR Workflow Optimization

![Academic Project](https://img.shields.io/badge/Project-Academic-003563?style=flat-square)
![Synthetic Data Only](https://img.shields.io/badge/Data-Synthetic%20Only-2e7d32?style=flat-square)
![HIPAA Not Applicable](https://img.shields.io/badge/HIPAA-Not%20Applicable-616161?style=flat-square)

**Structured, task-based usability assessment of OpenEMR workflows with actionable recommendations for documentation, prescribing, and encounter management** — using fully synthetic scenarios aligned with clinical informatics practice.

## Synthetic data warning

All case studies, tasks, ratings, and narrative examples in this repository are **fictional**. No protected health information (PHI) or real patient records are used. Materials are intended for coursework, portfolio, and research communication only.

## Overview

This study evaluates representative clinical and administrative workflows in **OpenEMR** as a model electronic health record (EHR) environment. Usability friction in registration, documentation, ordering, and multi-visit coordination directly affects clinician burden, error risk, and time at the point of care. By systematically scoring structured tasks and four synthetic case studies, we connect interface and workflow design choices to measurable difficulty and time-efficiency outcomes — information that matters for EHR optimization in both open-source and vendor systems (e.g., workflow parallels to environments such as Epic).

## Study design

- **Twenty-five (25) structured tasks** spanning registration, vitals, ICD-10 diagnosis entry, medication prescribing, multi-visit encounters, referrals, and record correction.
- **Four (4) synthetic case studies** (personae: Yuki Fitzgerald, Raj Murphy, Fatima Lindstrom, Chen O’Connor) used to ground tasks in realistic clinical narratives without real PHI.
- **Rating scales**: each task receives **difficulty** and **time burden** scores on a **0–9** integer scale in the synthetic dataset (higher values indicate greater perceived difficulty or more time lost to workflow friction; see `data/synthetic/task_ratings.csv`). The full course rubric may additionally reference a 0–10 scale in written methodology.

## Key findings

- **Vital signs cannot be recorded without first creating an encounter** — adding steps and cognitive load at the point of data capture.
- **Diagnoses and medications cannot be removed once added** — limiting safe correction workflows and increasing reliance on workarounds.
- **Navigation complexity contributes to cognitive overload** — deep menus and fragmented screens slow common tasks.
- **Alert fatigue** is observed across documentation-heavy workflows, diluting attention to high-risk signals.

## Recommendations

- **Redesign encounter flow** so documentation tasks (e.g., vitals) align with natural clinical sequencing and minimize prerequisite clicks.
- **Support reversible record editing** for diagnoses and medications within policy-appropriate guardrails (e.g., audit trails, reason for change).
- **Simplify navigation** with consistent information architecture, fewer lateral moves, and progressive disclosure.
- **Reduce click paths** for high-frequency actions through defaults, shortcuts, and task-centered layouts.

## Methods

Task-based usability inspection and structured scenario walkthroughs were combined with a **case study design**. **ICD-10** code selection was validated against standard coding conventions for documentation tasks. Clinical plausibility references were cross-checked using authoritative public sources (e.g., **Medscape**, **CDC**) for education-oriented validation — **not** for diagnosing or treating real patients.

## Team and roles

| Role | Responsibility |
|------|----------------|
| Principal investigator / course lead | Study framing, clinical informatics oversight |
| Student researcher(s) | Task rubric, scenario design, ratings, synthesis |
| Reviewers (peer / instructor) | Methodology feedback, academic integrity |

*Substitute names and emails in your fork as appropriate for your cohort.*

## Academic context

This work was conducted in an academic setting at **Indiana University Indianapolis**, course **INFO B535 — Clinical Information Systems**, under **Dr. Cathy Fulton**, **November 2024**. It reflects student or course-based research methodology and is not a substitute for peer-reviewed multicenter usability trials.

## References

Shortliffe, E. H., Cimino, J. J. (Eds.). *Biomedical Informatics: Computer Applications in Health Care and Biomedicine* (5th ed.). Springer. *(Use this edition for foundational concepts in clinical information systems, workflow, and decision support as cited in your course materials.)*

---

## Repository layout

| Path | Purpose |
|------|---------|
| `methodology/` | Task rubric and evaluation framework |
| `data/synthetic/` | Fictional ratings and case narratives |
| `findings/` | Usability gaps and redesign proposals |
| `docs/` | Clinical and EHR context |

See `NOTICE.md` for the synthetic-data disclaimer and `LICENSE` for terms of use.
