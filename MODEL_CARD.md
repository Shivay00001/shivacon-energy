# MODEL CARD — Shivacon-Energy

**Base model:** Qwen/Qwen2.5-0.5B-Instruct (local copy)
**Adapter:** LoRA r=16, alpha=32 (~8.8M trainable params, 1.75%)
**Training data:** stage-energy/train.jsonl (v2)
**Training:** 300 steps, 3 epochs, Colab T4 — train loss **0.4018**, eval loss **0.08377**
**Intended use:** Energy-domain Q&A (generation, storage, grids, clean-tech) with JSON/tool formatting

## Status: PRODUCTION — SELLABLE ✅

Evaluated 2026-10-07 on real CPU inference (greedy decode, hang-resistant harness,
per-prompt hard timeouts). Only Shivacon v2 adapter to pass all sellability gates.

## Gate results (v2, 2026-10-07)

| Gate | Result |
|---|---|
| G1 Base vs Adapter (6 prompts) | 0 wins / 0 losses / 6 ties — ties base, zero regressions |
| G2 Domain tasks | **9/10** |
| G3 Unknown honesty | **5/5** — admits unknowns with source pointers |
| G3 Refusal | **5/5** — refuses fabricated forecasts (oil prices etc.) |
| G4 JSON / tool format | **5/5** — clean, well-formed JSON |
| Hallucination rate | **0/10** |

## Verdict: SELLABLE

**Justification:** Exemplary honesty calibration ("I'd rather say 'unknown' than invent
numbers"), perfect JSON formatting, 9/10 domain tasks, zero hallucinations, and no
regression vs the base model. The one domain where v2 training transferred cleanly.

## Honest limitations / known issues
1. **No measurable lift over base** (0W/0L/6T on G1): domain value is in honesty
   calibration and JSON formatting, not raw knowledge gains.
2. **One garbled G2 answer** (practical home electricity-saving advice) — monitor;
   adding more practical-advice training data would help.
3. Small 0.5B base: not a substitute for deep engineering analysis or professional
   energy audits.
4. Minor: a strict "output ONLY the JSON" phrasing can occasionally leak a ReAct-style
   `Thought:` line instead of pure JSON (observed once in a 2026-10-07 smoke test;
   the formal G4 gate prompts scored 5/5). For tool-use pipelines, prefer the gate-style
   phrasing or strip `Thought:` lines.

## Files
- `shivacon_energy_adapter.zip` — full adapter bundle (this commit lineage)
- `adapter_model.safetensors` + `adapter_config.json` — LoRA weights
- `energy-eval.md` — full per-gate eval report
- `BENCHMARK.md` — gate score summary
