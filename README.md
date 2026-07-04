# Semantic Firewall

> This repository contains the code and evaluation datasets for the paper **"Semantic Firewall: A Hybrid Defense-in-Depth Architecture for LLM Security."**

## Overview
Current LLM guardrails rely on routing every user prompt through a dedicated safety LLM, introducing prohibitive latency and cost. The **Semantic Firewall** replaces this with a highly parallelized, hybrid pipeline that intercepts structurally predictable adversarial attacks early, reserving deep LLM evaluation only for complex, novel zero-day threats. 

Our architecture consists of:
1. **Deterministic Regex Engine:** Immediately blocks known structural signatures.
2. **Self-Healing Semantic Cache:** A ChromaDB vector database that auto-updates when novel threats are detected, blocking future semantic variants instantly.
3. **Parallel Heuristic Detectors:** 8 parallel heuristic detectors covering PII, Secrets, DoS limits, and Abuse.
4. **Constrained LLM Gate:** A strictly constrained Llama-3.3-70B model forced to output deterministic JSON classification schemas.

## Requirements
- Python 3.10+
- 16GB+ RAM (for running ChromaDB and fast layers)
- Access to an OpenAI, Anthropic, or OpenRouter API key (for baseline evaluation and the LLM Gate)

```bash
pip install -r requirements.txt
```

## Datasets

The evaluation utilizes the following public HuggingFace datasets:
- **[neuralchemy/Prompt-injection-dataset](https://huggingface.co/datasets/neuralchemy/Prompt-injection-dataset)**: Primary evaluation corpus containing a balanced mix of benign queries and complex injections.
- **[ai4privacy/pii-masking-200k](https://huggingface.co/datasets/ai4privacy/pii-masking-200k)**: Large multilingual corpus with embedded PII entities for scaled detection testing.
- **[walledai/Multi-Turn-Jailbreak](https://huggingface.co/datasets/walledai/Multi-Turn-Jailbreak)**: Adversarial conversational sessions for session-level recall testing.
- **[PKU-Alignment/BeaverTails](https://huggingface.co/datasets/PKU-Alignment/BeaverTails)**: Safety benchmark spanning 14 harm categories for out-of-distribution generalization.
- **[deepset/prompt-injections](https://huggingface.co/datasets/deepset/prompt-injections)**: Used for direct head-to-head evaluation against baseline heuristic pipelines.
- **[HuggingFaceH4/no_robots](https://huggingface.co/datasets/HuggingFaceH4/no_robots)**: Benign corpus used for false positive rate stress testing.

## Running the Benchmarks

All benchmark datasets evaluated in the paper are located in the `data/` directory. 

To run the large-scale evaluation on the `neuralchemy/Prompt-injection-dataset`:
```bash
python scripts/evaluate.py --dataset neuralchemy --config full_pipeline
```

To run the baseline ablation study:
```bash
python scripts/evaluate.py --dataset neuralchemy --config regex_only
python scripts/evaluate.py --dataset neuralchemy --config cache_only
python scripts/evaluate.py --dataset neuralchemy --config llm_only
```


## Reproducibility
The results in the paper were computed using `SentenceTransformers` (`all-MiniLM-L6-v2`) and a local `ChromaDB` instance with cosine similarity indexing ($\tau=0.90$). The specific seeds and deterministic thresholds are defined in `config/defense_config.yaml`.

## License
MIT License.
