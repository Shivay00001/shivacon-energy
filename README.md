# Shivacon-energy — v1 (FAILED sellability gate 0/6, 2026-10-06). Do not use as production. v2 retraining with improved datasets is in progress; this repo will be updated via commits when v2 passes the gate.

Why v1 failed the sellability gate (honest summary from EVAL_REPORT.md):

1. Baseline 0-0-6: worse than base, including wrong physics (10 kW = 10,000 kWh/hour).
2. Hallucinations 4/4: fabricated tool calls on invented files instead of admitting the unknown.
3. Tool calling 1/3: wrong tool selection and fabricated file paths.

What v2 is fixing: best freely-available datasets from the internet, ~15-20% refusal/anti-hallucination rows,
multi-step tool trajectories with observations, calibration rows. See EVAL_REPORT.md for full transcripts
and MODEL_CARD.md for the retrain prescription.

Base model: Qwen/Qwen2.5-0.5B-Instruct. Adapter: LoRA r=16. License of adapter weights: same as base model.
