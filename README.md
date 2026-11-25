# Controlled Delegation

**An Operations Manual for AI-Assisted Risk Analysis**

A method for making AI-assisted analytical work accountable and reproducible — and the evidence that it works, from two public credit projects.

---

## What this is

Controlled Delegation is an eight-step method and a seventeen-artifact template package for producing analytical work that survives review. It was developed while building two credit risk projects, and both are included here as worked examples.

The method answers four questions a reviewer will ask:

- Why did you do it this way?
- How do you know this is correct?
- Who approved this?
- Can you prove what happened?

The manual teaches a junior analyst to answer those questions before they are asked.

## The two projects

The method is demonstrated on two public datasets. They play different roles, and the difference is the point.

**Lending Club — the validation-side example.** A consumer credit default model built on peer-to-peer lending data. The project is split into a developer notebook and an independent validator notebook, and it demonstrates what it looks like when the team that builds a model is separated from the team that rules on it. The separation is enforced in code, not asserted in prose.

**Fannie Mae — the development-side example.** An IFRS 9 expected credit loss framework built on mortgage data. The project walks through the full development lifecycle and demonstrates what it looks like when a single team builds a model and documents its own limitations honestly. Its strongest artifacts are the disclosures of what the model does not do.

Neither project is a production model. Both use public data as a proxy for proprietary bank data. The value is the evidence, not the performance.

## The headline finding

The Lending Club project measured the cost of data leakage.

| | AUC |
|---|---|
| Developer-claimed, with leakage | 0.865 |
| Realistic held-out, leakage removed | 0.631 |
| **Difference** | **0.234 points (27.1%)** |

Nine features recorded information that only exists after a loan is issued. A model trained on them was not predicting repayment; it was reading it back. The 0.865 was real — on data that contained the answer.

## What's in this repository

docs/
controlled-delegation.md The manual (Sections 1–7, Appendices A–D)
verification.md How to check the manual's claims
limitations.md What the evidence does not show

notebooks/
lending-club/
NB01_data_quality_leakage.ipynb
NB02_model_development.ipynb
NB03_independent_validation.ipynb
NB04_governance_compliance.ipynb
NB05_cross_validation.ipynb
fannie-mae/
NB01_data_loading_cleaning.ipynb
NB02_eda_risk_factors.ipynb
NB03_pdlgd_ead_calibration.ipynb
NB04_stress_testing.ipynb
NB05_reporting_dashboard.ipynb
NB06_powerbi_export.ipynb

artifacts/
lending-club/
feature_inventory.csv
temporal_audit.csv
findings_register.csv
findings_register_v2.csv
data_quality_summary.json
manifest_notebook_01.csv
fannie-mae/
assumptions_register.csv
data_cleaning_documentation.txt
findings_register.csv
manifest_notebook_01.csv

templates/
seventeen-artifacts/ The method's output in reusable form
supporting/ The AI Interaction Record, reference card, findings register, reproducibility checklist

logs/
lending-club/
eda_audit_trail_.log
fannie-mae/
ecl_audit_trail_.log
calibration_audit_trail_*.log


## How to verify the claims

This repository is built to be checked, not taken on trust. The manual makes specific claims, and the `verification.md` file gives step-by-step instructions for checking the most important ones. Three examples:

**To verify the leakage claim.** Open Lending Club NB01, Phase 5. Confirm the `KNOWN_LEAKAGE` list is declared before any correlation is computed. Open `artifacts/lending-club/feature_inventory.csv`. Confirm the nine features are marked `Leakage_Flag = YES`. Open NB02, Phase 11. Confirm the 0.865 → 0.631 delta.

**To verify the Fannie Mae limitation disclosure.** Open Fannie Mae NB03, Section 3. Read the paragraph beginning "The Stage 2 population is a construction, not a measurement." Confirm the standard the notebook does not meet (IFRS 9 s.5.5.9) is named, and the word "compliant" is not used.

**To verify the audit trail.** Open any file in `artifacts/`. Confirm every row carries a `run_id` and a `notebook_version`. Open the corresponding log file in `logs/`. Confirm every significant action is timestamped.

The full set of instructions is in [`docs/verification.md`](docs/verification.md).

## Limitations

The evidence is substantial and it has limits. They are stated in full in [`docs/limitations.md`](docs/limitations.md). The short version:

- **Not production-compliant.** Both projects use public data as a proxy. Neither claims production compliance.
- **Not Canadian-validated.** Both are trained on U.S. data. The Canadian applicability gap is recorded as a limitation in both projects.
- **Not high-performing models.** The Lending Club held-out AUC is 0.631; the Fannie Mae parameters are largely assumption-based. The value is the evidence, not the performance.
- **No independent validation for Fannie Mae.** The Fannie Mae project is development-side. Its package is the input to validation, not the output.
- **No live monitoring.** Both projects are point-in-time analyses. The manual's claims about monitoring are method claims, not project evidence.

A reader who skips this section may overestimate what the repository supports. It is here to prevent that.

## The method in one page

For a reader who wants the method without the manual:

- **Five commitments.** Judgment retention, reproducibility, verification, traceability, controlled delegation.
- **Three failure modes.** Correctness risk, provenance risk, governance risk.
- **Six layers of evidence.** Representation integrity, judgment ownership, capability integrity, velocity, drift, assurability.
- **One artifact.** The AI Interaction Record — seven fields, filled in at the moment of the interaction.

The rule: no change is merged without a complete AI Interaction Record.

The full reference card is in [`templates/supporting/reference-card.md`](templates/supporting/reference-card.md).

## Who this is for

A junior analyst who has been given a project and will have to defend it. The manual assumes technical skill and teaches the discipline that makes technical skill reviewable.

A reviewer who wants to see what a complete evidence package looks like. The two projects are included so the package can be inspected, not just described.

## Reading order

1. **The manual, Sections 1–3.** The problem, the method, how to apply it.
2. **The worked examples, Sections 4–5.** The method applied, Lending Club then Fannie Mae.
3. **The appendices.** The prompt library, the template package, the learning paths, the regulatory frameworks.

The manual is in [`docs/controlled-delegation.md`](docs/controlled-delegation.md).

## Status

Version 1.0 draft, November 2026. The manual is under revision. The repository structure and the two worked examples are complete; the manual's prose is being finalized.

## License and data

The manual and the notebooks are the author's work. The Lending Club and Fannie Mae datasets are public; their terms of use are their own. The repository does not redistribute the raw data. See [`docs/limitations.md`](docs/limitations.md) for the dataset provenance and the proxy caveat.
