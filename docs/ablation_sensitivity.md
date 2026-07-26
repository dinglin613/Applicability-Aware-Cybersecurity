# Additional Ablation and Sensitivity Results

Additional checks cover physical-impact evaluation settings, search-setting
ablations, and uncertain-weight sensitivity results.

## Physical-Impact Evaluation Settings

The physical-impact evaluation uses the IEEE 14-bus line-coupled
circuit-breaker setting. The eight circuit-breaker devices are mapped
one-to-one to IEEE 14-bus transmission lines, so every candidate has a direct
N-1 load-shed value $`s_d`$ in the exposure-weighted load-shed metric.

The values of $`s_d`$ are computed using the IEEE 14-bus model implemented in
pandapower, with load curtailed in proportion to post-contingency line
overloads. The horizon is 30 epochs with maintenance windows on alternating
epochs and one patch per window.

Stressed peak operating conditions are used so that single-line N-1
contingencies produce nonzero load shed:

- base load multiplied by 3.5;
- line ratings reduced to 50%.

The LLM-derived, fuzzy-string-derived, and manually annotated oracle matrices
are evaluated under identical scheduling and physical-impact settings, with
only $`Q`$ varied.

## Search-Setting Ablation

The search-setting ablation uses the same IEEE 14-bus S2 setup as the main
strategy-comparison table. Increasing the lookahead depth and candidate cap
raises CER relative to the greedy and narrow-search variants, while the burden
penalty controls the CER-burden trade-off.

| Variant | CER | Burden | Viol. | Time (s) |
| --- | ---: | ---: | ---: | ---: |
| Full SDMO ($`H=5`$, $`B=10`$) | 20.54 | 779 | 0 | 0.14 |
| Greedy ($`H=1`$) | 20.07 | 779 | 0 | 0.05 |
| Narrow ($`B=2`$) | 19.45 | 854 | 0 | 0.06 |
| No Burden ($`\lambda=0`$) | 20.60 | 1093 | 0 | 0.13 |
| High Burden ($`\lambda=1.0`$) | 19.79 | 695 | 0 | 0.15 |

## Uncertain-Weight Sensitivity

The SCD case study is swept over the uncertain-mapping weight $`q_u`$. The
LLM-ensemble CER stays within a 0.6% band, from 22.72 to 22.86, and the
LLM-vs-rule gap remains at least 29.5% under the same scheduler, risk model,
and constraints. The rule row is invariant to $`q_u`$ because the rule-derived
applicability matrix does not use the LLM uncertain label.

| $`q_u`$ | Rule CER | LLM CER | Delta CER (%) | LLM Burden (min) |
| ---: | ---: | ---: | ---: | ---: |
| 0.0 | 17.55 | 22.86 | +30.3 | 270 |
| 0.3 | 17.55 | 22.80 | +29.9 | 330 |
| 0.5 | 17.55 | 22.80 | +29.9 | 330 |
| 0.7 | 17.55 | 22.80 | +29.9 | 330 |
| 1.0 | 17.55 | 22.72 | +29.5 | 300 |
