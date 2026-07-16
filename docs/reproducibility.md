# Reproducibility Documentation

This document summarizes the reproducibility materials associated with the
paper **Applicability-Aware Cybersecurity Patch Scheduling for IEC 61850
Substations**.

The repository is organized around the main experimental modules in the paper.
Each document is written to be read independently from the manuscript while
preserving the terminology and numerical settings used in the reported case
studies.

## Documentation Map

| Paper component | Repository document |
| --- | --- |
| IEC 61850-derived feasibility constraints C1--C7 | [`constraint_generation.md`](constraint_generation.md) |
| Offline LLM applicability-inference layer | [`applicability_inference.md`](applicability_inference.md) |
| Real-CVE applicability benchmark and matcher comparison | [`real_cve_benchmark.md`](real_cve_benchmark.md) |
| SCD-derived real-CVE scheduling case | [`scd_case.md`](scd_case.md) |
| Physical-impact settings and additional robustness checks | [`ablation_sensitivity.md`](ablation_sensitivity.md) |

## What Is Included

- Representative deterministic inputs used to generate constraints C1--C7.
- Prompt template and JSON schema for the offline applicability-inference call.
- Verdict-to-weight mapping and post-processing rules for the applicability
  matrix.
- Real-CVE benchmark construction, annotation protocol, hard-negative
  definition, and matcher implementation details.
- SCD-derived topology, asset-to-role mapping, and real-CVE assignment details.
- Physical-impact evaluation settings, search-setting ablation, and
  uncertain-weight sensitivity results.

## What Is Not Included

- Source code for the scheduler or LLM invocation pipeline.
- Proprietary utility asset inventories.
- Operational deployment data from a live substation.
- API keys, model credentials, or vendor-private firmware information.

## Reproducibility Notes

The paper separates applicability inference, forward risk evaluation, and
deterministic IEC 61850 feasibility constraints. The documentation follows the
same separation so that readers can inspect each experimental component without
mixing CVE-to-device inference details with scheduling feasibility assumptions.
