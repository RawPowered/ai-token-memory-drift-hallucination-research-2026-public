# Review of the four presentation graphics

The four supplied graphics were reviewed against primary sources and the repository's mathematical definitions. The published PNG editions incorporate the wording changes below. Their visual design was retained using the built-in image generation tool, followed by visual inspection. The companion SVG text editions were synchronized where relevant.

## Review summary

| Graphic | Findings and published changes |
|---|---|
| [01: Tokens](figures/01-how-tokens-work-reviewed.png) | Qualified the character/word conversion as an English rule of thumb. Marked context accounting as model/API-dependent. Clarified that the hypothetical output allowance can include reasoning tokens. Verified the 80K reserved total and 48K remainder. |
| [02: Session memory](figures/02-session-memory-reviewed.png) | Described attention as weighted combinations of value vectors. Defined the additive mask. Marked the drift chain as a possible path and qualified parameter updates as ordinary inference behavior. |
| [03: Error models](figures/03-error-probability-reviewed.png) | Defined first-error hazard conditionally, labeled 1% as an assumption, changed the limiting-value notation to a stationary value, and separated unsupported claims from false claims. Corrected the metric name and clarified proportion versus percentage-point units. |
| [04: Prompting](figures/04-prompting-reliability-reviewed.png) | Removed universal rankings of context quality against token count and correction against additional context. Framed error propagation as possible and prompting practices as interventions whose effects need evaluation. |

## Primary-source checks

**Tokens and context accounting.** OpenAI's [key concepts](https://developers.openai.com/api/docs/concepts) identifies the four-character/0.75-word conversion as a rough English approximation and describes combined prompt/output context limits. Its [reasoning guide](https://developers.openai.com/api/docs/guides/reasoning) explains that reasoning tokens can also occupy context. The slide's budget is a conceptual text-generation example, with categories treated as non-overlapping allocations. Multimodal encoding, reasoning carryover, and API limits require model-specific accounting.

**Attention and retained state.** Vaswani et al.'s [Attention Is All You Need, Section 3.2](https://arxiv.org/html/1706.03762v7) defines attention through query/key compatibility and weighted values. The slide uses the usual additive-mask form of scaled dot-product attention. Its `M` is an attention mask, distinct from the memory-budget symbol on slide 01. OpenAI's [compaction guide](https://developers.openai.com/api/docs/guides/compaction) describes reducing context while carrying state forward. These mechanisms support the distinctions between trained parameters, supplied context, and application-managed external memory.

**Position and multi-turn behavior.** Liu et al.'s [Lost in the Middle](https://aclanthology.org/2024.tacl-1.9/) finds position-sensitive performance in the evaluated long-context tasks. Laban et al.'s [LLMs Get Lost In Multi-Turn Conversation](https://arxiv.org/abs/2505.06120) describes early assumptions and reliance on prior solutions in simulated multi-turn tasks. These findings motivate the possible failure paths; they establish neither an inevitable sequence nor a universal token-age decay law.

**Prompting recommendations.** OpenAI's [prompt-engineering guidance](https://developers.openai.com/api/docs/guides/prompt-engineering) emphasizes model-dependent behavior and evaluation. Its [reasoning best practices](https://developers.openai.com/api/docs/guides/reasoning-best-practices) recommends explicit constraints and success criteria. The reviewed slide recommends source checks, preservation of constraints, and verification as practices to evaluate on representative tasks. It makes no guaranteed improvement or universal ranking claim.

## Mathematical checks and interpretation

### Slide 01

- Chain rule: `P(y[1:m] | x) = product_i P(y[i] | x, y[<i])` describes the autoregressive factorization.
- Example budget: `8,000 + 6,000 + 34,000 + 20,000 + 12,000 = 80,000`.
- Remaining capacity: `128,000 - 80,000 = 48,000`.
- The example is illustrative, not a measured internal allocation for any named model.

### Slide 03: first error

With `h_t = P(first error at t | no earlier error)`, survival through turn `T` is `product_t(1-h_t)`. Its complement is the probability of at least one error by `T`. A separate independence assumption is unnecessary when the hazards are defined conditionally this way.

For constant assumed `h = 0.01`, `1 - 0.99^50 = 0.3949939329`, which rounds to **39.5%**. This is cumulative first-error probability under an illustrative assumption. It is neither an observed failure rate nor a per-turn hallucination proportion.

### Slide 03: recovery

For constant transition probabilities `h` and `r` in `[0,1]`, the recurrence is:

`d[t+1] = (1-r)*d[t] + h*(1-d[t])`.

Setting `d[t+1] = d[t]` gives the stationary probability `d* = h/(h+r)` when `h+r > 0`. The graphic uses **stationary**, since convergence from every initial state is a stronger claim: `h=r=1` gives an alternating chain. With `h=r=0`, the initial state persists and the ratio is undefined. The diagram's transition arrows identify entry to error as `h` and recovery as `r`.

### Slide 03: claim measurement

`H_u(L)` is defined here as unsupported checkable factual claims divided by all checkable factual claims under the specified evidence and grading rules. It requires a positive denominator. Unsupported claims can be true; contradicted claims should be reported separately, alongside completeness and abstention.

For `L2 > L1`, `g_u = [H_u(L2)-H_u(L1)] * 10,000 / (L2-L1)` has units of **proportion change per 10,000 additional tokens**. `100*g_u` expresses the change in percentage points. A negative value indicates a decrease. Comparisons require matched tasks, consistent grading, reported uncertainty, and attention to confounders; this difference is not by itself a causal or universal growth rate.

## Presentation and scope

The PNGs are the reviewed presentation editions. The SVG companions provide simpler editable text layouts, and the README gives detailed text equivalents and references. Both are educational summaries. No new model-evaluation runs were performed for this update.

The incoming graphics were preserved in their original locations. Only reviewed editions are featured here. [Image-edit prompts](GRAPHICS_PROMPTS.md) record the revisions, and [the graphic manifest](figures/reviewed-graphics-manifest.json) identifies the accepted image files and their hashes.
