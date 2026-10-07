# Eval Report — Shivacon-Energy v2

**Date:** 2026-10-06
**Adapter:** `shivacon_energy_adapter.zip` (LoRA r=16 on Qwen2.5-0.5B-Instruct)
**Training:** 300 steps, train_loss=0.4018, eval_loss=0.08377
**Method:** CPU greedy decode, keyword rubrics + manual adjudication.

## Gate 1 — Base vs Adapter
Adapter wins: 0, Base wins: 0, Ties: 6
Adapter pass: 6/6, Base pass: 6/6
**Note:** Ties base on all 6 prompts — no measurable lift, no regression. Domain value is in task behavior and honesty calibration, not raw knowledge.

## Gate 2 — Domain Tasks: 9/10
Failed:
- Q: Give 3 practical ways to cut a home's electricity use....

## Gate 3 — Hallucination: 5/5 unknown, 5/5 refusal
Hallucination rate: 0/10

## Gate 4 — JSON: 5/5
Failed:
(none)

## Verdict: SELLABLE
**Justification:** Honest (10/10), formats JSON perfectly (5/5), handles 9/10 domain tasks. No G1 lift over base (0W/0L/6T) but zero regressions. Exemplary honesty phrasing ("I'd rather say 'unknown' than invent numbers"). One garbled g2 answer to monitor.

## Next Step
Ship as-is for energy Q&A + tool use. Optionally add more practical-advice training data to fix the 1 incoherent g2 answer.

## Sample Notes
G3 answers are model honesty: refuses to forecast oil prices, admits unknown solar figures with source pointers. G4 JSON is clean and well-formed.
