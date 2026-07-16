# Applicability-Aware Cybersecurity Patch Scheduling for IEC 61850 Substations

Reproducibility documentation for the paper:

**Applicability-Aware Cybersecurity Patch Scheduling for IEC 61850 Substations**

The paper studies cybersecurity patch scheduling for IEC 61850 substations when
firmware patch actions must satisfy deterministic operational constraints and
CVE-to-device applicability is inferred from advisory text and asset metadata.

This repository records the experimental settings, prompt template, benchmark
construction, SCD-derived case setup, and additional ablation and sensitivity
results used in the reported studies.

## Repository Contents

| Path | Description |
| --- | --- |
| [`docs/reproducibility.md`](docs/reproducibility.md) | Overview of the reproducibility materials and how they map to the paper. |
| [`docs/constraint_generation.md`](docs/constraint_generation.md) | Representative deterministic inputs used to instantiate constraints C1--C7. |
| [`docs/applicability_inference.md`](docs/applicability_inference.md) | LLM applicability-inference prompt, response schema, post-processing rules, and representative judgments. |
| [`docs/real_cve_benchmark.md`](docs/real_cve_benchmark.md) | Real-CVE applicability benchmark construction, annotation protocol, and matcher definitions. |
| [`docs/scd_case.md`](docs/scd_case.md) | SCD-derived topology, asset-to-role mapping, and real-CVE scheduling-case construction. |
| [`docs/ablation_sensitivity.md`](docs/ablation_sensitivity.md) | Physical-impact evaluation settings, search-setting ablation, and uncertain-weight sensitivity. |

## License

This repository is released under the MIT License. See [`LICENSE`](LICENSE).
