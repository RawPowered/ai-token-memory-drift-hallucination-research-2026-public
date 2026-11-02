# Reproducible Evaluation Protocol — Draft v0.1

**Status:** Proposed experimental design only. No model runs have been executed.

## Objective
Estimate separately the rate of fact-recall errors, unsupported claims, contradicted claims, first-error incidence, incorrect-state persistence, and recovery in multi-turn LLM conversations.

## Research questions
1. Does token length change evidence retrieval at fixed position and task difficulty?
2. Is there an interaction between context position (beginning, middle, end) and length?
3. Does compaction or summarization omit non-negotiable constraints?
4. Does explicit correction reduce future reuse of false assumptions?

## Independent variables
- Token length: 4K, 8K, 16K, 32K, 64K, 128K where supported.
- Relevant fact position: early, midpoint, late.
- Turn count: 1, 5, 20, 50.
- Context condition: full transcript, truncated transcript, compacted summary, retrieved memory where exposed.
- Intervention: none, explicit correction, correction + citation verification.
- Model identity, snapshot, decoding settings, tool set, tokenization, and prompts must be recorded.

## Dataset
Generate synthetic facts and explicitly labeled negative constraints with unique IDs. Use matched tasks. Avoid disclosing personal information or confidential material. Store exact reference answers and a machine-readable evidence ledger.

## Labeling
Every checkable factual claim gets exactly one principal label: (A) verified supported, (B) unsupported/unverifiable with current evidence, (C) directly contradicted by oracle, (D) abstained/not answered. A secondary label marks whether the model followed all required constraints. Use independent graders blinded to conditions and adjudicate conflicts.

## Metrics
- exact recall rate = correctly recovered sentinel facts / queried sentinel facts
- unsupported fraction = unsupported checkable claims / all checkable factual claims
- contradiction fraction = contradicted checkable claims / all checkable factual claims
- per-turn first-error hazard = first failures at turn t / conversations error-free before turn t
- recovery fraction = previously incorrect conversations corrected at next opportunity / previously incorrect conversations with that opportunity
- survival function = proportion of conversations without error up to turn t
- explicit answer completeness and abstention rates

## Analysis
Use conversation-level clustered bootstrap confidence intervals and report sample sizes, uncertainty and grader agreement. Fit a mixed-effects logistic model or survival model with controls for task, model, context length, evidence position, turn depth, and correction treatment. Pre-register comparisons to avoid selecting favorable slices. Do not equate 39.5% cumulative error probability with a pointwise hallucination rate.

## Acceptance and interpretation
Do not publish a numerical claim about GPT-6 or any current model without identifying the tested snapshot, sample, scoring rubric, confidence intervals, and inference settings. A model's context specification is not a measured effective-memory specification. Negative and null results must be preserved.

## Outputs and reproducibility
Suggested future directories: data/synthetic/, results/raw/, results/graded/, scripts/, and methods/. Publish anonymized inputs, reference facts, grader rubric, model snapshots, seeds, and analysis code. No benchmark results are currently included.

## References
See the root README and references/references.bib for primary literature and interpretation cautions.
