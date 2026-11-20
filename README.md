# phi-CCT on e-SNLI with Qwen3-8B — results snapshot

This folder presents the saved findings from a completed experiment on whether a model's explanation mentions a controlled input edit when its predicted entailment label changes. The study used 1,000 frozen e-SNLI test examples, insertion and deletion edits, and three matched answer-option layouts. Read the [two-page final report](reports/final_report.pdf) for the methods, statistical tests, examples, and interpretation.

## Main results

| Measure | Insertion | Deletion |
| --- | ---: | ---: |
| Valid edited observations | 2,874 | 2,265 |
| Distinct sources with a valid edit | 958 | 755 |
| phi-CCT | 0.2097 | 0.1489 |
| 95% source-bootstrap confidence interval | 0.1754–0.2397 | 0.1109–0.1847 |
| Exact target-mention rate | 84.62% | 72.85% |
| Prediction-change rate | 28.25% | 13.33% |

Baseline accuracy was **65.93% over 3,000 baseline jobs**. The [execution status](results/execution_status.json) is `COMPLETE`: the [raw inference file](results/raw_inference_outputs.csv) holds **8,139 unique jobs**, all with `status=ok`, and the [coverage file](results/coverage_1000_examples.csv) marks **all 1,000 sources complete**. The 5,139 valid edited observations comprise 2,874 insertion and 2,265 deletion observations. The three option layouts for each source are repeated measures, not independent examples.

phi-CCT is the binary correlation between `E_D` (an exact lexical mention of the edit target in the specified explanation) and `I_D` (a change in semantic prediction between matched original and edited prompts). A positive correlation here is an association; it does not establish that the explanation is causally faithful.

## Files

- [`reports/final_report.pdf`](reports/final_report.pdf): final scientific report.
- [`results/frozen_sample.csv`](results/frozen_sample.csv), [`counterfactual_proposals.csv`](results/counterfactual_proposals.csv), and [`intervention_audit.csv`](results/intervention_audit.csv): selected examples, proposed edits, and the recorded manual intervention review.
- [`results/raw_inference_outputs.csv`](results/raw_inference_outputs.csv): preserved job-level model outputs and statuses.
- [`results/valid_observations.csv`](results/valid_observations.csv) and [`coverage_1000_examples.csv`](results/coverage_1000_examples.csv): analysis rows and source coverage.
- [`results/overall_metrics.csv`](results/overall_metrics.csv), [`statistical_tests.csv`](results/statistical_tests.csv), and [`subgroup_phi_estimates.csv`](results/subgroup_phi_estimates.csv): estimates and statistical tests; figures are in [`results/plots/`](results/plots/).
- [`results/qualitative_examples.csv`](results/qualitative_examples.csv) and [`lexical_matching_audit.csv`](results/lexical_matching_audit.csv): traceable examples and the recorded lexical review.
- [`results/model_smoke.json`](results/model_smoke.json), [`runtime_estimate.json`](results/runtime_estimate.json), and the other JSON manifests: environment, timing estimate, and provenance.

## Scope and limitations

The completed run used **Qwen/Qwen3-8B**, revision `b968826d9c46dd6066d109eabc6255188de91218`, in FP16 on two Tesla T4 GPUs, without quantization and with thinking disabled. The saved runtime estimate of **11.116 hours** extrapolates from 27 smoke-test jobs; it is not a measured full-run wall time.

This is a **results-only snapshot**. The notebook, source code, dependency list, full original e-SNLI files, and model weights are not included, so this folder alone cannot reproduce inference. A separate clean-kernel end-to-end reproduction has not been demonstrated. The lexical audit contains only `E_D=1` cases from six sources and therefore does not estimate missed mentions. The deletion/named-entity analysis has only 22 entity sources and a zero contingency cell; its extreme logistic result should be treated cautiously. The report discusses these and other limitations.

## Attribution

The saved CSVs contain selected and edited text derived from [e-SNLI](https://github.com/OanaMariaCamburu/e-SNLI) and the [SNLI corpus](https://huggingface.co/datasets/stanfordnlp/snli). The SNLI dataset card identifies **CC BY-SA 4.0** as its license; applicable upstream terms continue to apply to source and edited input text. The study used the [Qwen3-8B model](https://huggingface.co/Qwen/Qwen3-8B) and the phi-CCT definition discussed by [Siegel et al.](https://arxiv.org/abs/2503.13445). No model weights or full source dataset are redistributed here.
