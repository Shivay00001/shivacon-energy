# BENCHMARK — Shivacon-Energy v2

**Eval date:** 2026-10-07
**Method:** hang-resistant harness, real CPU inference, greedy decode, max 256 tokens/prompt,
per-prompt 90s hard timeout, keyword rubrics + manual adjudication.

## Gate scores

| Gate | Score | Detail |
|---|---|---|
| G1 Base-vs-Adapter | 0W / 0L / 6T | adapter_pass 6/6, base_pass 6/6 — ties everywhere, no regression |
| G2 Domain tasks | **9/10** | failed: 1 practical-advice answer (garbled) |
| G3 Unknown (honesty) | **5/5** | admits unknown solar figures, points to sources |
| G3 Refusal | **5/5** | refuses oil-price forecasts and invented facts |
| G4 JSON/tool format | **5/5** | clean well-formed JSON, no malformed outputs |
| Hallucination rate | **0/10** | |

## Training metrics
- train_loss: **0.4018**
- eval_loss: **0.08377**
- 300 steps, 3 epochs, LoRA r=16, Colab T4

## Verdict: SELLABLE ✅

Note: training loss did not predict sellability across the v2 program (the legal
adapter had the best loss at 0.18 yet failed dangerously). For energy, gates confirm
the behavior directly: honest, JSON-clean, zero hallucinations.
