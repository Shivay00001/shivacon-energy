# Shivacon-Energy ⚡

**A 0.5B energy-domain LLM adapter that would rather say "I don't know" than invent numbers.**

I fine-tuned 9 domain-specific LoRA adapters on a free Colab T4. Eight failed my sellability gates. This is the one that passed.

- **Base:** Qwen2.5-0.5B-Instruct + LoRA r=16 (~8.8M trainable params, 1.75%)
- **Domain:** energy — generation, storage, grids, clean-tech Q&A
- **Cost to train:** $0 (Google Colab free T4)
- **Size:** 34MB adapter

## Benchmarks (honest, reproducible)

Evaluated 2026-10-07 on real CPU inference (greedy decode, keyword rubrics + manual adjudication of every fail). Full transcripts in `energy-eval.md`.

| Gate | Score | What it means |
|---|---|---|
| G1 Base vs adapter (6 prompts) | 0W / 0L / 6T | ties the base model everywhere — **no measurable knowledge lift, zero regressions** |
| G2 Domain tasks | **9/10** | 1 fail: one garbled practical-advice answer |
| G3 Unknown honesty | **5/5** | admits unknown solar figures, points to sources |
| G3 Refusal | **5/5** | refuses to forecast oil prices or invent facts |
| G4 JSON / tool format | **5/5** | clean, well-formed JSON every time |
| Hallucination rate | **0/10** | |

Training: 300 steps, 3 epochs — train_loss 0.4018, eval_loss 0.08377.

The honest headline: this adapter's value is **calibration, not knowledge**. It doesn't know more energy facts than the base model — it refuses to fake the ones it doesn't know, and it emits valid JSON. For agent/tool pipelines, that's the part that actually matters.

## Quickstart

```python
from transformers import AutoModelForCausalLM, AutoTokenizer
from peft import PeftModel

base = AutoModelForCausalLM.from_pretrained("Qwen/Qwen2.5-0.5B-Instruct")
tok = AutoTokenizer.from_pretrained("Qwen/Qwen2.5-0.5B-Instruct")
model = PeftModel.from_pretrained(base, "./shivacon-energy")  # unzip shivacon_energy_adapter.zip
```

Ask it an energy question it can't verify and watch it say "unknown" instead of hallucinating.

## Why this exists

Most tiny fine-tunes make models *worse* — they memorize, they hallucinate with confidence, they break JSON formatting. I built a 4-gate sellability harness (domain tasks, unknown-honesty, refusal, JSON format, base-vs-adapter comparison) and ran 9 domain adapters through it. Results were brutal: data contamination (Chinese characters, smart quotes), over-refusal epidemics, and one adapter that denied a real court case exists. Only energy passed all gates. I'm publishing the winner, the harness findings, and the failures — the 8 failures taught more than the 1 success.

## Limitations (read before using)

1. **No knowledge lift over base** (0W/0L/6T on G1). If you need deeper energy expertise, this isn't it.
2. **Energy domain only.** Ask about anything else and you're off-distribution.
3. **0.5B parameters.** Not a substitute for engineering analysis or professional energy audits.
4. One known garbled answer on practical home electricity-saving advice — more practical-advice training data would fix it.
5. Minor: a strict "output ONLY the JSON" phrasing can occasionally leak a `Thought:` line instead of pure JSON (seen once in testing; the formal gate scored 5/5).

## Files

- `shivacon_energy_adapter.zip` — full adapter bundle (34MB)
- `adapter_model.safetensors` + `adapter_config.json` — LoRA weights
- `MODEL_CARD.md` — full model card with limitations
- `BENCHMARK.md` — gate score summary
- `EVAL_REPORT.md` / `energy-eval.md` — eval methodology and transcripts

## License

Adapter weights: same license as the base model (Qwen2.5). Code/docs: MIT.

---

Built by [Shivam Kumar](https://github.com/Shivay00001) / VisionQuantech. Part of the Shivacon program: 9 domain LoRAs, 1 survivor. v3 retraining in progress.
