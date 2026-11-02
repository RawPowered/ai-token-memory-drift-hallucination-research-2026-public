# Graph provenance, calibration, and reproducibility

**Status:** Published-source model specification comparison plus `educated approximation` illustrations. No new model-evaluation runs were performed.

## Figure 05 — Published model specifications
- Source data: [data/published-model-specs.csv](published-model-specs.csv).
- Parameters use the publisher's values. `8B`, `9B`, and `12B` are *nominal model classes*, whereas `7.61B` for Qwen2.5-7B comes from the publisher's more precise parameter table.
- Context values are **capacity specifications**, not observed recall, precision, or quality scores.
- Llama 3.1 8B: **128K** per Meta model card. [Meta](https://github.com/meta-llama/llama-models/blob/main/models/llama3_1/MODEL_CARD.md).
- Qwen2.5 7B: **32,768 by default**, with publisher-documented context extension to **131,072 using YaRN**. The official [model card](https://huggingface.co/Qwen/Qwen2.5-7B-Instruct) explains the required `rope_scaling` and trade-offs; see also [Qwen's release table](https://qwenlm.github.io/blog/qwen2.5-llm/).
- Mistral NeMo: **up to 128K** [per publisher](https://mistral.ai/news/mistral-nemo/); [checkpoint configuration](https://huggingface.co/mistralai/Mistral-Nemo-Instruct-2407/blob/main/config.json).
- Gemma 2 9B: **8,192**, using Google’s [Gemma PyTorch configuration](https://github.com/google/gemma_pytorch/blob/main/gemma/config.py) and [Gemma 2 model card](https://ai.google.dev/gemma/docs/core/model_card_2).
- **Do not** infer memory reliability from parameter count or context length, or compare these specs to benchmark scores without matching tasks and deployment details.

## Figure 06 — Educated approximation (not fitted to observations)

The curves are exact evaluations of two **explicitly chosen** toy stochastic models. The qualitative motivation comes from [Liu et al., 2024](https://aclanthology.org/2024.tacl-1.9/), [Wu et al., 2025](https://proceedings.iclr.cc/paper_files/paper/2025/hash/d813d324dbf0598bbdc9c8e79740ed01-Abstract-Conference.html), and [Laban et al., 2026](https://proceedings.iclr.cc/paper_files/paper/2026/hash/59f6421e64707225fdf5b28840679a07-Abstract-Conference.html). None of these sources supplies a transferable single per-turn hazard `h` or correction rate `r`.

### Left: first-error probability

`P(T)=1-(1-h)^T`, where `T` is turns, and the assumed `h` values are **0.005, 0.010, 0.020**. The plotted values are **probability of at least one error by T**, not hallucination proportion at turn T. The hazard values were chosen for illustration; no published model is assigned to these curves.

### Right: active incorrect-state probability

`d(t+1)=(1-r)d(t)+h(1-d(t))`, initialized at `d(0)=0`. Assume `h=0.01` and vary `r` over **0**, **0.05**, and **0.20**. For `r>0`, `d(t) = [h/(h+r)] [1 - (1-h-r)^t]`. For `r=0`, `d(t)=1-(1-h)^t`. These parameters are not empirical estimates of user/model behavior.

### Meaning of “educated approximation”
The *equations* are mathematically valid under their hypotheses. The *choice of coefficients* is illustrative and uncalibrated, although motivated by published observations of error persistence and imperfect recovery. Consequently **these figures do not estimate real GPT-6 or any other provider's failure rate** and should not be reported with empirical confidence intervals.

### Future empirical calibration
For each model checkpoint and task distribution, collect independent multi-turn traces, ground-truth factual labels, actual first-error hazards and correction transitions with denominators, report confidence intervals (cluster by conversation), fit a model, and overlay empirical rates rather than extrapolating from other studies' aggregate percentages.

## Rendering and accessibility
Both figures are hand-authored 1672x940 SVGs with source evidence/assumption labels. They contain text equivalents in the README. If visually revised, keep the dataset, legends, and labels synchronized.
