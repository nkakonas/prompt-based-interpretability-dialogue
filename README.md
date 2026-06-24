# Prompt-Based Interpretability in Dialogue Models

> Evaluating how prompt structure affects faithfulness and interpretability in LLM-based dialogue systems across medical, banking, and multi-domain settings under low-resource conditions.

---

## Overview

Large Language Models increasingly power dialogue systems in safety-critical domains like medicine and finance. But do structured prompts actually produce more faithful reasoning — or just more coherent-sounding responses?

This project systematically benchmarks **5 prompting strategies** across **3 dialogue domains** using Mistral-7B-Instruct, evaluating both task quality (ROUGE-L, BERTScore) and interpretability (Factuality, Relevance, Fluency, Clarity) via a GPT-5.1 judge rubric.

**Key finding:** Simpler prompts outperform complex reasoning chains on interpretability. More reasoning does not guarantee clearer or more faithful responses.

---

## Tech Stack

| Component | Details |
|-----------|---------|
| Generator Model | Mistral-7B-Instruct-v0.2 (HuggingFace) |
| Evaluation Judge | GPT-5.1 (rubric-based, deterministic) |
| Automatic Metrics | ROUGE-L, BERTScore (BERT-F1), BLEU |
| Statistical Test | Kruskal-Wallis H-test |
| Language | Python |
| Datasets | MedDialog, Retail Banking Chatbot, MultiDoGO |

---

## Prompt Families

| Prompt Family | Strategy |
|---------------|----------|
| **Baseline** | Minimal instruction — reveals natural model behavior |
| **Chain-of-Thought (CoT)** | Step-by-step reasoning |
| **Self-Explain** | Requires brief justification of final answer |
| **Domain-Aware** | Incorporates explicit domain guidelines |
| **Critic** | Self-critique and revision before final output |

---

## Datasets

| Dataset | Domain | Size |
|---------|--------|------|
| MedDialog (EN) | Medical doctor–patient dialogues | ~5,000 turns |
| Retail Banking Chatbot | Customer service dialogues | ~5,000 turns |
| MultiDoGO | Multi-domain task-oriented conversations | ~5,000 turns |

All datasets subsampled to simulate low-resource conditions with an 80/10/10 train–validation–test split.

---

## Results

### Task Quality (All 3 Domains)

| Prompt Family | BERT-F1 | ROUGE-L |
|---------------|---------|---------|
| **Self-Explain** | **0.7233** | 0.0226 |
| Baseline | 0.7047 | **0.0244** |
| Chain-of-Thought | 0.7008 | 0.0175 |
| Domain-Aware | 0.6991 | 0.0168 |
| Critic | 0.6762 | 0.0231 |

### Interpretability (Banking Domain — GPT-5.1 Judge)

| Prompt Family | Factuality | Relevance | Fluency | Clarity |
|---------------|-----------|-----------|---------|---------|
| **Baseline** | **3.40** | **2.15** | **2.50** | **2.00** |
| **Domain-Aware** | 3.20 | 2.10 | 2.20 | 2.00 |
| Self-Explain | 3.00 | 1.80 | 1.80 | 1.80 |
| Chain-of-Thought | 3.00 | 1.00 | 1.00 | 1.00 |
| Critic | 3.00 | 1.00 | 1.00 | 1.00 |

### Statistical Significance (Kruskal-Wallis)

| Metric | H-Statistic | p-value |
|--------|-------------|---------|
| Relevance | 55.11 | < 0.0001 |
| Clarity of Explanation | 53.02 | < 0.0001 |
| Fluency | 52.96 | < 0.0001 |
| Factuality | 17.47 | 0.0016 |

---

## Key Takeaways

- **Prompt structure has a strong, measurable impact** on dialogue quality and interpretability (p < 0.001 across all metrics)
- **Simpler prompts win on interpretability** — Baseline and Domain-Aware consistently outperform CoT and Critic
- **Lexical and semantic metrics diverge** in specialized domains — ROUGE-L near zero in Medical while BERTScore remains high (0.59–0.73), showing models paraphrase correctly but don't match surface phrasing
- **Interpretability and faithfulness are decoupled** — explanation clarity does not guarantee faithful reasoning
- **Prompt effectiveness is domain-dependent** — Critic achieves highest BERT-F1 in Banking (0.733) but lowest in Medical (0.590)

---

## How to Run

```bash
# Clone the repo
git clone https://github.com/nikoskakonas/prompt-based-interpretability-dialogue.git
cd prompt-based-interpretability-dialogue

# Install dependencies
pip install -r requirements.txt

# Run evaluation pipeline
python run_pipeline.py --domain banking --prompt self_explain

# Run all conditions (15 prompt-domain combinations)
python run_pipeline.py --all
```

---

## Project Structure

```
prompt-based-interpretability-dialogue/
├── data/               # Dataset loading and preprocessing
├── prompts/            # Prompt templates for all 5 families
├── evaluation/         # ROUGE, BERTScore, GPT judge rubric
├── analysis/           # Kruskal-Wallis, attention analysis
├── results/            # Output metrics and visualizations
├── run_pipeline.py     # Main entry point
└── requirements.txt
```

---

## Authors

- **Nikolaos Kakonas** — Georgia Institute of Technology
- Sunho Park — Georgia Institute of Technology
- Yue Wu — Georgia Institute of Technology
- Yuan Jack Yao — Georgia Institute of Technology

---

## 📄 Full Paper

[prompt_interpretability_dialogue_paper.pdf](./prompt_interpretability_dialogue_paper.pdf)

---

*Georgia Institute of Technology — Conversational AI Course Project*
