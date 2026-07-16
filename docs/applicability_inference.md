# Applicability-Inference Implementation

This document describes the offline LLM applicability-inference step. Each CVE
is evaluated against the full device inventory. Parsed responses are converted
into a fixed device-CVE applicability matrix `Q`, and the scheduler reads this
matrix during forward search without calling the model.

## Prompt Template

The prompt template is populated with CVE-specific fields and inventory entries.

```text
You are an ICS security analyst doing CVE triage against a substation device inventory. For the given CVE, decide for EACH device in the inventory whether it is affected.

Decision rules:
- APPLICABLE only if vendor matches (consider lineage: ABB / Hitachi ABB Power Grids / Hitachi Energy share product lines; Invensys / Wonderware / AVEVA / Schneider lineage for InTouch), product family matches (e.g., "Relion 670 series" matches "Relion 670 series"; "InTouch" is NOT "InTouch Access Anywhere"), AND firmware/version is within the affected range stated in the CVE.
- NOT_APPLICABLE when any of vendor, product, or version definitively does not match.
- UNCERTAIN when the vendor lineage or product family may match, but the CVE text or inventory lacks sufficient version, sub-family, or lineage information to make a definitive decision.

Output strict JSON only, of the form:
{
  "decisions": [
    {"device_id": "<id>", "verdict": "APPLICABLE" | "NOT_APPLICABLE" | "UNCERTAIN", "reason": "<short>"}
  ]
}
One entry per device. No prose outside the JSON.

CVE:
  ID: {CVE_ID}
  CVSS: {CVSS_SCORE} (vector: {CVSS_VECTOR})
  Description: {CVE_DESCRIPTION}

DEVICE INVENTORY ({N} devices):
  - {DEVICE_ID}: vendor={VENDOR}, product={PRODUCT}, model={MODEL}, firmware={FIRMWARE}, type/role={DEVICE_TYPE_OR_ROLE}
  ...
```

## Inference Settings

| Item | Specification |
| --- | --- |
| Invocation unit | One CVE is evaluated at a time against the complete device inventory. The prompt includes the CVE identifier, CVSS score and vector, CVE description, and for each device the identifier, vendor, product family, model, firmware, and device type or role. |
| Prompt template | The template above evaluates one CVE against the complete device inventory and requires one JSON decision per device. |
| Decision labels | `APPLICABLE` is used when vendor lineage, product family/model, and affected version range match the CVE text. `NOT_APPLICABLE` is used when at least one of these fields definitively does not match. `UNCERTAIN` is used when the vendor lineage or product family may match, but the available CVE text or inventory lacks sufficient version, sub-family, or lineage information for a definitive decision. |
| JSON schema | The required response is strict JSON with a single `decisions` array. Each element has `device_id`, `verdict` in `{APPLICABLE, NOT_APPLICABLE, UNCERTAIN}`, and a short `reason`. No prose outside the JSON object is allowed. |
| Model versions and settings | The OpenAI run uses `o3` with `max_completion_tokens=6000`. The Gemini run uses `gemini-2.5-pro` with JSON response MIME type. The same prompt template and device inventory are used for both providers. |
| Post-processing | Code fences are stripped before JSON parsing; if direct parsing fails, the first JSON object is extracted and parsed. Parsed verdicts are converted through the deterministic verdict-to-`Q` mapping. Verdict labels outside the allowed set are assigned the uncertain weight rather than treated as hard applicable evidence. |
| Verdict-to-`Q` mapping | `APPLICABLE`, `NOT_APPLICABLE`, and `UNCERTAIN` are mapped to `1`, `0`, and `q_u`, respectively, with `q_u=0.5` by default. The two-provider SCD matrix is the elementwise mean of the o3 and Gemini applicability weights. |

## Representative Judgments

| CVE/device | Model | Verdict | Model reason | Audit role |
| --- | --- | --- | --- | --- |
| CVE-2021-22283 / E1Q1BP2 | o3 | applicable | ABB Relion 615 IEC 5.0 v5.0.10 is within the affected 5.0.0--5.0.11 range. | Correct applicable |
| CVE-2021-22283 / E1Q2SB1 | o3 | not applicable | Relion 670 series is not listed in the CVE. | Correct negative |
| CVE-2022-28613 / E1Q3KA1 | o3 | uncertain | RTU500 series matches, but the CVE text gives no version range. | Conservative uncertain |
| CVE-2021-27196 / RTU04 | o3 | not applicable | RTU500 13.2.1 is outside the affected 12.x range. | Manual-audit disagreement |
