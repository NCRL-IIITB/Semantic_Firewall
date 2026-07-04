<div align="center">
  <h1>🛡️ Semantic Firewall</h1>
  <p><b>A Hybrid Defense-in-Depth Architecture for LLM Security</b></p>

  <a href="https://arxiv.org/abs/XXXX.XXXXX"><img src="https://img.shields.io/badge/Paper-ArXiv-red.svg" alt="Paper"></a>
  <a href="https://github.com/NCRL-IIITB/Semantic_Firewall/blob/main/LICENSE"><img src="https://img.shields.io/badge/License-MIT-blue.svg" alt="License"></a>
  <a href="https://python.org"><img src="https://img.shields.io/badge/Python-3.10+-success.svg" alt="Python 3.10+"></a>
</div>

<br/>

> **Abstract:** Current LLM guardrails rely on routing every user prompt through a dedicated safety LLM, introducing prohibitive latency and cost. The **Semantic Firewall** replaces this with a highly parallelized, hybrid pipeline that intercepts structurally predictable adversarial attacks early, reserving deep LLM evaluation only for complex, novel zero-day threats. 

---

## 🏗️ Architecture

The firewall implements a rigorous defense-in-depth pipeline consisting of four cascading layers:

1. **Deterministic Regex Engine:** Immediately blocks known structural signatures, dropping trivial payloads with $O(1)$ latency.
2. **Self-Healing Semantic Cache:** A ChromaDB vector database ($\tau=0.90$) that auto-updates when novel threats are detected, blocking future semantic variants instantly.
3. **Parallel Heuristic Detectors:** 8 concurrent, stateless heuristic agents evaluating prompts for PII, Secrets, DoS limits, and Abuse.
4. **Constrained LLM Gate:** A strictly constrained Llama-3.3-70B model forced to output deterministic JSON classification schemas. Only zero-day, highly complex prompts ever reach this stage.

---

## 🚀 Quickstart & Installation

**Prerequisites:**
- Python 3.10+
- 16GB+ RAM (for local ChromaDB embedding generation)
- API Keys (OpenAI/Anthropic/OpenRouter) for the baseline evaluation and LLM Gate fallback.

```bash
# 1. Clone the repository
git clone https://github.com/NCRL-IIITB/Semantic_Firewall.git
cd Semantic_Firewall

# 2. Install dependencies
pip install -r requirements.txt

# 3. Setup your environment keys
cp .env.example .env
# Edit .env with your API keys
```

### Running Benchmarks
All benchmark datasets evaluated in the paper are located in the `data/` directory.

```bash
# Run the large-scale evaluation on the neuralchemy dataset
python scripts/evaluate.py --dataset neuralchemy --config full_pipeline

# Run the baseline ablation study
python scripts/evaluate.py --dataset neuralchemy --config regex_only
python scripts/evaluate.py --dataset neuralchemy --config cache_only
python scripts/evaluate.py --dataset neuralchemy --config llm_only
```

---

## 📊 Evaluation Datasets

The evaluation utilizes the following public HuggingFace datasets to ensure broad, out-of-distribution coverage:

- **[neuralchemy/Prompt-injection-dataset](https://huggingface.co/datasets/neuralchemy/Prompt-injection-dataset)**: Primary evaluation corpus containing a balanced mix of benign queries and complex injections.
- **[ai4privacy/pii-masking-200k](https://huggingface.co/datasets/ai4privacy/pii-masking-200k)**: Large multilingual corpus with embedded PII entities for scaled detection testing.
- **[walledai/Multi-Turn-Jailbreak](https://huggingface.co/datasets/walledai/Multi-Turn-Jailbreak)**: Adversarial conversational sessions for session-level recall testing.
- **[PKU-Alignment/BeaverTails](https://huggingface.co/datasets/PKU-Alignment/BeaverTails)**: Safety benchmark spanning 14 harm categories for out-of-distribution generalization.
- **[deepset/prompt-injections](https://huggingface.co/datasets/deepset/prompt-injections)**: Used for direct head-to-head evaluation against baseline heuristic pipelines.
- **[HuggingFaceH4/no_robots](https://huggingface.co/datasets/HuggingFaceH4/no_robots)**: Benign corpus used for false positive rate stress testing.

---

## 📝 Citation

If you find this code or our paper useful in your research, please cite us:

```bibtex
@misc{semanticfirewall2026,
      title={Semantic Firewall: A Hybrid Defense-in-Depth Architecture for LLM Security}, 
      author={Anonymous Authors},
      year={2026},
      eprint={XXXX.XXXXX},
      archivePrefix={arXiv},
      primaryClass={cs.CR}
}
```

*Note: Update the BibTeX entry with the final publication details once accepted.*
