# V2 Training Report — Shivacon-ENERGY

## Status: TRAINED — FULL EVAL GATES PENDING

> **Honest header:** V2 adapter trained successfully. Full eval gates
> (base-vs-adapter comparison, V1 failure probes, hallucination/refusal
> scoring, tool-call formatting tests) are **PENDING**. Metrics below are
> **training metrics only** — not a sellability certification.

## Training config
- Base model: Qwen/Qwen2.5-0.5B-Instruct
- Method: LoRA (rank 16), 3 epochs
- Learning rate: 2e-4, batch size 4, grad accumulation 4
- Hardware: free Google Colab (Tesla T4 GPU)
- Date: 2026-10-06

## Results
- Steps completed: 300
- Final train loss: 0.4018
- Final eval loss: 0.08377
- Dataset: 1596 training rows (v2 improved: refusal rows, anti-hallucination, calibration, tool trajectories)

## Artifacts
- `shivacon_energy_adapter.zip` — integrity-verified zip of the final adapter

## Next steps
1. Base-vs-adapter comparison
2. Exact V1 failure probes
3. Hallucination/refusal scoring
4. Tool-call formatting / multi-step execution
5. Honest model card, then sellability verdict
