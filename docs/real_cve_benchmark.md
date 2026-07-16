# Real-CVE Benchmark and Annotation

This document describes the construction, annotation protocol, and matcher
implementations for the real-CVE applicability benchmark.

## Benchmark Construction

The applicability benchmark uses 45 real CVEs drawn from NVD and CISA ICS
advisories from 2007 to 2025 against a 57-device inventory. The inventory spans
vendors commonly represented in published CISA ICS advisories:

- ABB / Hitachi Energy
- Schneider Electric
- Siemens
- Schweitzer Engineering Laboratories
- AVEVA
- Honeywell
- GE Vernova
- Triangle MicroWorks

Each device entry carries vendor, product family, model, and firmware version.
The inventory deliberately places same-vendor devices with in-range and
out-of-range firmware together with product-lineage variants, such as ABB- and
Hitachi-branded Relion devices and "InTouch" versus "InTouch Access Anywhere."
This construction creates hard negatives that require product-subfamily and
firmware-range reasoning rather than vendor matching alone.

The final benchmark contains 2565 `(CVE, device)` pairs. After excluding
uncertain pairs and unresolved annotator disagreements from the precision,
recall, and F1 denominators, the evaluation set contains 66 applicable pairs and
2455 negatives, of which 478 are same-vendor or product-lineage hard negatives.

## Annotation Protocol

The applicability ground truth is produced by a documented, version-anchored
guideline so that labels are reproducible and auditable. Each `(CVE, device)`
pair is independently labeled by two human annotators using a three-step
procedure:

1. Vendor/lineage gate: the device vendor must match the CVE vendor, allowing
   known corporate lineage such as ABB / Hitachi Energy and Wonderware /
   Invensys / AVEVA.
2. Product-family gate: near-name mismatches are rejected, such as "InTouch"
   versus "InTouch Access Anywhere" or "CIMPLICITY" versus "Smallworld."
3. Version gate: the device firmware is checked against the affected range
   stated in the NVD record and linked advisory.

A pair is labeled `applicable` only when vendor, product family, and version are
all positively confirmed. It is labeled `not applicable` when any of the three
definitively does not match, including firmware above the fixed version or in an
unaffected branch. It is labeled `uncertain` whenever the product matches but
the version cannot be verified from the CVE text.

Uncertain pairs and unresolved annotator disagreements are excluded from all
precision/recall/F1 denominators. The pairwise Cohen's kappa between the two
human annotators is 0.966 for the three-class labels and 0.970 for the
applicable-versus-not-applicable binary labels before reconciliation.

## Matcher Definitions

| Matcher | Definition |
| --- | --- |
| Naive substring | Performs substring search between CVE product strings and device fields. |
| Fuzzy string | Computes token-set overlap on the vendor-product string with a threshold of 0.6. |
| Structured CPE | Uses a vendor equivalence map for known lineage transitions, a 28-pattern product family parser, and a version-range comparator for phrases such as "prior to," "before," and "and older." |
| OpenAI o3 | Uses the applicability-inference prompt documented in this repository. |
| Google gemini-2.5-pro | Uses the same applicability-inference prompt as the o3 run. |

No manual correction is applied to matcher outputs before computing the
benchmark metrics.
