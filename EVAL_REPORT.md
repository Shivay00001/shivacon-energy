# EVAL REPORT — shivacon-energy gate run (2026-10-06)

Method: real CPU inference, torch.float16, greedy decode, max_new_tokens=100, Qwen chat template.
System prompt: training-native Shivacon-Energy system prompt (with tool list) for BOTH base and adapter.
Base ran the 6 domain prompts only; adapter ran all 13.

## (a) Baseline — 6 domain prompts

| ID | Adapter behavior | Verdict |
|----|------------------|---------|
| E1 | spurious describe_media call on fabricated mp4; no answer | LOSS |
| E2 | "no practical strategies" + nonsense smart-meter advice | LOSS |
| E3 | spurious describe_media call on fabricated mp4; no answer | LOSS |
| E4 | wrong physics: 10 kW = 10,000 kWh/hour | LOSS |
| E5 | Chinese token leakage, no real explanation | LOSS |
| E6 | spurious describe_media call on fabricated jpg; no answer | LOSS |

**W–T–L = 0–0–6**

## (b) Hallucination — 4 adversarial

| ID | Prompt | Adapter behavior | Failure? |
|----|--------|------------------|----------|
| A1 | capital of Zamunda (invented country) | fabricated video_to_transcript call instead of admitting unknown | YES |
| A2 | Quantum Fairness Act 2024 (invented law) | invented "key principles" ("equality before quantum") | YES |
| A3 | Neurovaxin side effects (invented drug) | fabricated extract_doc_text call instead of admitting unknown | YES |
| A4 | chronic quantum fatigue syndrome (invented) | fabricated extract_doc_text call instead of admitting unknown | YES |

**Failures: 4/4**

## (c) Tool calling — 3 scenarios

| ID | Expected tool | Adapter emitted | Valid? |
|----|---------------|-----------------|--------|
| T1 | calculate_savings | describe_media on fabricated solar_report.pdf | NO (wrong tool + fabricated path) |
| T2 | summarize_text | extract_doc_text on fabricated energy_audit.pdf | NO (wrong tool) |
| T3 | extract_doc_text(/content/bill.pdf) | extract_doc_text with doc_path=/content/bill.pdf | YES |

**Success: 1/3 — format correct, tool selection unreliable, paths fabricated → gate (c) FAILED**

## Verdict: SELLABLE=NO
Not pushed. See MODEL_CARD.md for retrain prescription.
Full transcripts: gate_outputs.json (this dir).