# MODEL CARD — Shivacon-Energy

**Base model:** Qwen/Qwen2.5-0.5B-Instruct (local copy)
**Adapter:** LoRA r=16, alpha=32 (~8.8M trainable params, 1.75%)
**Training data:** 1659 train rows + eval split (stage-energy/train.jsonl); 343 rows contain `Action:` tool calls
**Training:** 312 steps, 3 epochs, Colab T4 — train loss 0.4425, eval loss 0.3526
**Intended use:** Energy-domain Q&A (generation, storage, grids, clean-tech) with ReAct tool calling
(`Action: <tool>` / `Action Input: <JSON>`)

## Gate results (2026-10-06, real CPU inference, greedy, 100 tokens)
- Baseline vs base (6 domain prompts): **W–T–L = 0–0–6** — adapter worse than base on every prompt
- Hallucination failures: **4/4**
- Tool-call success: **1/3** (right format, wrong tool choice 2/3, fabricated file paths)

## Verdict: SELLABLE=NO

## Limitations / known failures
1. **Tool-call reflex:** adapter calls a multimodal tool on almost every prompt — including pure
   knowledge questions and adversarial probes — instead of answering directly.
2. **Fabricated tool arguments:** invents media/doc paths (`solar_inverter_explanation.mp4`,
   `zapada_videos/zamunda_world_map.mp4`, `neovaxin_doc.pdf`) that the user never provided.
3. **Degraded domain answers:** wrong physics (E4: "10 kW = 10,000 kWh/hour"), empty strategy
   (E2: "no practical strategies"), Chinese token leakage in English answers (E5).
4. **No hallucination resistance:** invents principles for fictitious legislation
   ("equality before quantum" for the made-up Quantum Fairness Act 2024) instead of refusing.
5. Low training CE loss (0.44/0.35) did NOT translate to correct behavior — loss is not a
   behavior guarantee for this adapter.

## What a retrain needs
- Negative tool-use examples: answer directly when no tool is needed (currently ~always calls).
- Grounded tool args: only user-supplied paths; penalize fabricated paths.
- Clean training data language (Hindi `Thought:` lines leaking; Chinese tokens in English answers).
- Anti-hallucination / refusal data: admit unknown entities, refuse invented facts.
- Behavior-level held-out eval, not just token CE loss.
