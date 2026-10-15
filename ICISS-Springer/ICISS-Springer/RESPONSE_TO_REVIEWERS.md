# How each reviewer comment was addressed

**Paper:** *Multi-agent Semantic Firewall: A Hybrid Defense-in-Depth System for LLM Security*, ICISS 2026 camera-ready.

> **Private:** this file quotes the reviews. It stays in `ICISS-Springer/`, which is never pushed to the public repository.

Section, table and equation numbers refer to the current camera-ready (`Semantic_Firewall.tex`, 2026-10-11). Every change in the paper is highlighted in yellow while `\revtrue` is set. Reviewer IDs follow the tracker (`CAMERA_READY_TRACKER.md`), which also holds the full reviews. Item-by-item edit history is in `CHANGELOG_CAMERA_READY.md`.

**How the camera-ready was done:**
- Every experiment was re-run on neuralchemy's official splits. The train split only seeds the cache, the validation split selects thresholds, and the test split is evaluated once.
- Every table is rebuilt from per-prompt records in `results_camera_ready/`.
- As a final check, all numbers in the paper were recomputed from the raw files: 542 automated checks, 0 failures.

## Summary

| Status | Items |
|---|---|
| ✅ Done | R3.1, R3.3, R2.2, R2.3, R1.1a–l, R1.2a–e, R1.3a–d, R1.4a, R1.4e, R1.4f, R1.5a–d, R1.6a–d (36) |
| ◐ Partly done | R3.2 / R1.4d (cache poisoning), R2.1 (RLM / sub-agent baselines), R1.4c (gate injection discussed, not measured) (4) |
| ⏭ Stated as a limitation / future work | R2.4 = R1.4b (adaptive attackers) (2) |

Duplicates, answered once: R1.1b = R3.3 · R1.2b = R3.1 · R1.4d ⊂ R3.2 · R1.4b = R2.4 · R1.2d ≈ R2.1 · R1.6a ⊂ R2.2.

---

## Reviewer 3

### R3.1 Warm-cache protocol; are the gains from memorising similar test samples? ✅
- **Comment:** it is unclear how the cache is filled and updated during testing, and whether the gains come from test prompts already in the cache.
- **What we did:**
  - The cache is seeded only with the 2,650 training-split attacks and frozen during evaluation. Thresholds are chosen on validation, and the test split is evaluated once.
  - The official splits share no prompt, so no validation or test prompt is ever in the cache.
  - The warm protocol is written as an exact rule (Eq. 9).
  - The cold protocol (empty cache that learns during the run) is reported separately as C9.
  - We also report results on the 495 test prompts that are unlike every cached attack (nearest training attack below τ). On those, the full system reaches 88.3% F1, versus 84.8% for GPT-4o and 82.0% for DeBERTa-v3, so the gains do not rest on near-duplicates.
- **Where:** Sec. 4 ("Cache protocols", Eq. 9); Sec. 5.2 (last paragraph); Table 2a and Table 5 (C9).

### R3.2 Security of the self-updating cache: error propagation and cache poisoning ◐
- **Comment:** false positives or adversarial inputs written to the cache could affect later decisions. Evaluate error propagation and cache poisoning.
- **What we did:**
  - **Threat model** (Sec. 3.9): assets, attacker goals (G1 evasion, G2 cache poisoning, G3 allowlist abuse), black-box and pipeline-aware attackers, trust assumptions, out-of-scope threats, and what the paper evaluates.
  - **Poisoning-resistant cache design** (Sec. 3.10): a benign-overlap check, a probationary tier with a stricter threshold, a time-to-live, and sampled re-validation by the full pipeline. All four are implemented in the code, off by default.
  - **Error propagation, measured on normal traffic** (Sec. 5.3):
    - Test-split write-back was replayed exactly from recorded verdicts, from the warm cache and from an empty one. The replay reproduces C9 (same 151 cache hits, same 696 LLM calls, same recall).
    - The gate's false positives that qualify for write-back (14 test prompts) never blocked a benign prompt. None lies within τ of an unseen benign prompt, whereas a random benign prompt, if cached, would block 0.60% of unseen benign traffic.
  - **Cost of the safeguards** (Table 4):
    - from an empty cache, all four together cost 6.9% more LLM calls and 0.47 points of recall;
    - from the warm cache, they cost 0.6% more LLM calls;
    - both lower the false-positive rate.
- **Not done:** a deliberate poisoning campaign, and how much the safeguards reduce it. This is stated as future work in the threat model, in Sec. 5.3 and in Limitations (supervisor's decision).
- **Where:** Sec. 3.9, Sec. 3.10, Sec. 5.3, Table 4, Sec. 6.

### R3.3 τ inconsistency (best F1 at τ = 0.90 in Table 3, but "τ = 0.99" in the text) ✅
- **What we did:**
  - τ is now selected on the validation split by an exact sweep (0.55–0.90). F1 peaks at τ = 0.65 (94.90%), and 0.65 is used everywhere.
  - Table 3a (validation) and Table 3b (OOD-500) are regenerated from exact offline sweeps.
  - The ensemble threshold θ was swept jointly (2.0–6.0) and did not change F1.
- **Where:** Sec. 3.2; Sec. 5.2; Table 3.

---

## Reviewer 2

### R2.1 Baselines too weak; compare with RLM / sub-agents and with parallel agents without a deterministic stage ◐
- **What we did:**
  - The baselines grew to eight, all run on the same 942 test prompts with FPR and latency:
    - GPT-4o, Gemini 2.5 Flash and Claude Haiku 4.5 (replacing the retired Claude 3.5 Haiku);
    - three purpose-built classifiers: ProtectAI DeBERTa-v3, PIGuard (the renamed InjecGuard) and Prompt Guard 2 (86M);
    - Llama Guard 4;
    - Rebuff, as a published reference.
  - The full system is significantly better than every baseline (McNemar, Holm–Bonferroni corrected).
  - New ablation C1b runs both LLM-assisted detectors in parallel on every prompt, with no deterministic stage. It reaches 92.71% F1; the full pipeline stays significantly better (χ² = 16.4, p < 10⁻⁴) while calling the LLM for only half of the prompts.
- **Not done:** RLM / sub-agent baselines (author decision).
- **Where:** Table 2a; Table 5 (C1b); Sec. 5.1 and Sec. 5.4.

### R2.2 Incorrect or inconsistent formulas; do the errors affect the calculations? ✅
- **What we did:** every equation was checked against the code.
  - **Eq. 4 (entropy):** the sum now runs over distinct characters, and the check is attributed to the Abuse Detector (flags < 2.5 bits on inputs of ≥ 50 characters), matching the code.
  - **Eq. 5 (Levenshtein):** it described a feature that did not exist, so it was **implemented**: typo repair of keywords in the Injection Detector, enabled in all reported results. With one typo per keyword, regex detection of the 62 test attacks the regex catches without typos rises from 33.9% to 80.1%.
  - **Eq. 6 (risk score):** the weights match the code, but the code never blocked on it. The paper now says the risk score is for explanations and session tracking, and gives the actual blocking rule: the new ensemble score (Eq. 7) with θ = 3.5.
  - **Eq. 8 (effective latency):** now uses measured path probabilities that sum to 1 and measured path latencies, giving L_eff ≈ 308 ms.
  - **Algorithm 1:** rewritten from the orchestrator.
- **Where:** Sec. 3.3, Sec. 3.5, Sec. 3.8, Algorithm 1.

### R2.3 "Self-healing" is inaccurate; describe the cache mechanism ✅
- **What we did:**
  - "Self-healing" was removed everywhere.
  - Sec. 3.2 now describes what the cache does:
    - it stores only previously seen malicious prompts and administrator-approved prompts;
    - only LLM-confirmed detections with confidence ≥ 0.85 are written;
    - it holds at most 10,000 entries, evicting the oldest first;
    - approved prompts (cosine ≥ 0.90) skip only the LLM gate, while the deterministic detectors still run.
  - Measured growth and eviction behaviour was added (see R1.4e).
- **Where:** Abstract, Sec. 3 intro, Sec. 3.2.

### R2.4 Coordinated bypass: optimise attacks directly against the firewall ⏭
- **Status:** not evaluated; stated as future work.
- **What the paper covers:** robustness tests that do not adapt to the system:
  - keywords obfuscated with typos (33.9% → 80.1% regex detection with keyword repair, Sec. 3.3);
  - out-of-distribution attacks (OOD-500, Table 3b);
  - test prompts unlike any cached attack (88.3% F1, Sec. 5.2);
  - multi-turn jailbreaks (Table 2c);
  - a black-box allowlist test (Sec. 3.10).
- **How it is framed:** the threat model (Sec. 3.9) lists adaptive evasion as unevaluated and says the detection results should be read as performance against known, non-adaptive attacks.
- **Left as future work** (Sec. 6, Limitations):
  - attacks optimised against the detectors and the open-weight gate;
  - injections addressed to the gate itself;
  - near-allowlist inputs crafted with knowledge of the pipeline;
  - deliberate cache-poisoning campaigns against the poisoning-resistant design.
- **Where:** Sec. 3.9 ("Coverage in this paper"), Sec. 6.

---

## Reviewer 1

### 1. Internal inconsistencies

| ID | Comment | What we did | Where |
|---|---|---|---|
| R1.1a ✅ | Abstract/conclusion paired an F1 and a recall from different datasets | Abstract and conclusion now give one F1 per dataset: deepset 77.16%, BeaverTails 72.95%, AI4Privacy 94.85%, OOD-500 92.44% | Abstract, Sec. 7 |
| R1.1b ✅ | τ = 0.99 in Sec. 5.2 vs τ = 0.90 elsewhere | One threshold everywhere, τ = 0.65, chosen on validation (see R3.3) | Sec. 3.2, 5.2 |
| R1.1c ✅ | Table 3a F1 does not follow from P and R | Table 3 regenerated from exact sweeps; every F1 in the paper recomputed from its P and R | Table 3 |
| R1.1d ✅ | Table 3b claims N = 500 but accuracies are multiples of 1/98 | Table 3b is now OOD-500: 500 prompts (250 attacks, 250 benign) from five public datasets not used to build neuralchemy, released with the code | Table 3b, Sec. 5.2 |
| R1.1e ✅ | Share resolved without the LLM is 41.93% in one place, 66.88% in another | One measured value everywhere: 50.1% of test requests | Abstract, Sec. 5.1, Table 7 |
| R1.1f ✅ | Eq. 7 probabilities leave P_m = 0 and L_eff is not reproducible | Measured P_c = 447/942, P_h+P_m = 25/942, P_l = 470/942 and path latencies (85.6 / 332.1 / 519.1 ms) give L_eff ≈ 308 ms | Eq. 8, Sec. 5.4 (Cost) |
| R1.1g ✅ | P/R imply an FPR of about 12%, not the stated 4.20% | FPR is now measured directly for every system and reported in every table (ours: 8.46%) | Tables 2, 3, 5 |
| R1.1h ✅ | Repeated latency (602.83 ms); C7 (no LLM) slower than C8 | Latencies re-measured one request at a time: C7 142 ms < C8 304 ms < C1 406 ms | Table 5 |
| R1.1i ✅ | LLM provider Groq in Sec. 3.4 vs OpenRouter in Sec. 4.1 | OpenRouter throughout, as in the code; figure label changed to "LLM API" | Sec. 3.4, Fig. 1 |
| R1.1j ✅ | Wrong model identifiers | Exact OpenRouter model IDs in a footnote (queried October 2026); retired Claude 3.5 Haiku replaced by Claude Haiku 4.5; all baselines re-run on the test split | Sec. 4.1 |
| R1.1k ✅ | "Six baselines" does not match the list | Now "eight", matching the list (see R2.1) | Sec. 1, Sec. 5 |
| R1.1l ✅ | Unexplained ± in the abstract | Removed | Abstract |

### 2. Evaluation protocol

| ID | Comment | What we did | Where |
|---|---|---|---|
| R1.2a ✅ | Select τ and θ on a validation split, report the test set once, report θ | τ and θ selected jointly on validation; test evaluated once; θ = 3.5 stated with the ensemble rule (Eq. 7) | Sec. 3.5, Sec. 4, Sec. 5.2 |
| R1.2b ✅ | Describe the warm-cache protocol | See R3.1 | Sec. 4, Eq. 9 |
| R1.2c ✅ | Report FPR for every system | FPR column for every system, baselines included | Tables 2, 3, 5 |
| R1.2d ✅ | Add purpose-built detectors (Prompt Guard 2, DeBERTa, InjecGuard); describe the zero-shot prompts | DeBERTa-v3, PIGuard/InjecGuard and Prompt Guard 2 added; their settings and the zero-shot LLM prompt are described | Table 2a, Sec. 4.1 |
| R1.2e ✅ | Significance tests with multiple-comparison correction | McNemar tests against every baseline, Holm–Bonferroni corrected; all wins remain significant. Rebuff is a published reference without per-prompt data, so no paired test is possible and no win over it is claimed | Sec. 5.1 |

### 3. Moderate or support the claims

| ID | Comment | What we did | Where |
|---|---|---|---|
| R1.3a ✅ | Present BeaverTails / AI4Privacy as related tasks, not proof of generalisation | Table 2b retitled "Cross-Dataset and Related-Task Evaluation"; the text says deepset is the other injection benchmark and BeaverTails and AI4Privacy are related tasks | Sec. 5.1, Table 2b |
| R1.3b ✅ | Multi-turn set has no benign conversations; report a multi-turn FPR | New held-out set: 100 SafeMTData jailbreaks plus 100 benign no_robots conversations, threshold chosen on a separate dev set. Recall 89.0%, FPR 6.0% | Sec. 4, Table 2c, Sec. 5.1 |
| R1.3c ✅ | Qualify cost / "LLM bypass" claims; report cost and latency at 1–10% attack rates with p95/p99 | Bypass claim qualified in the abstract, conclusion and cost section. New Table 7: at 1–10% attack rates the LLM-call rate is 94.4–87.4% and L_eff 495–466 ms (p95 944 ms, p99 1242 ms). Test split: p50 279, p95 756, p99 1047 ms | Abstract, Sec. 5.4 (Cost), Table 7 |
| R1.3d ✅ | Remove or support "real detection rather than memorisation" for BeaverTails | Claim removed; added that BeaverTails labels question–answer pairs, which raises the FPR of any prompt-level filter | Sec. 5.1 |

### 4. Security analysis

| ID | Comment | What we did | Where |
|---|---|---|---|
| R1.4a ✅ | Add an explicit threat model | New Sec. 3.9 (see R3.2) | Sec. 3.9 |
| R1.4b ⏭ | Evaluate adaptive attackers | See R2.4 | Sec. 3.9, Sec. 6 |
| R1.4c ◐ | Discuss injection attacks against the LLM gate | Discussed: the gate's output is schema-constrained and the final action is the most severe of all detectors, so a manipulated gate cannot undo a block by the cache or deterministic detectors, but can miss a novel attack only it would catch. Listed as unevaluated in the threat model | Sec. 3.10, Sec. 3.9 |
| R1.4d ◐ | Analyse cache poisoning and allowlist abuse | Poisoning: see R3.2. Allowlist abuse, black-box test: injection payloads appended to 100 approved prompts; only 4 stayed within the 0.90 threshold, and none passed in the default mode (deterministic detectors still run). The pipeline-aware case is listed as unevaluated | Sec. 3.10, Sec. 5.3 |
| R1.4e ✅ | Discuss cache growth and eviction | Measured: lookup takes 31, 20 and 19 ms at 1k, 5k and 20k entries, so the cap bounds storage, not latency; with a cap of 500, inserting 2,000 entries leaves 500 (oldest evicted); the 10,000 cap is never reached in our experiments | Sec. 3.2 |
| R1.4f ✅ | Privacy of sending unresolved prompts to a third-party API | Limitations: the provider sees unresolved prompts; privacy-sensitive deployments can host the open-weight gate locally. Threat model: the provider is assumed honest but curious | Sec. 6, Sec. 3.9 |

### 5. Untested components

| ID | Comment | What we did | Where |
|---|---|---|---|
| R1.5a ✅ | Per-detector ablations | neuralchemy has no inputs for five detectors, so Table 6 uses MIXED-1000 (released): 1,000 prompts combining neuralchemy prompts with personal data, generated credentials, oversized inputs, feed signatures, restricted workspace terms and benign look-alikes. Removing each detector in turn: PII changes 78 decisions (personal-data recall 65.4% → 13.1%), Secrets 71 (credentials 100% → 32.4%), Injection 50 (injection recall 98.2% → 91.8%), Custom Rules 40, Unsafe Content 16, Context Flooding 7, Abuse 5; Threat Intel 0 (everything it flags is also flagged by Abuse or PII) | Table 6, Sec. 5.4 |
| R1.5b ✅ | Evaluate the Session Judge, Policy Engine profiles and Explainability module | Session Judge: 50.0% of attack sessions (FPR 4.0%) without its memory of known multi-turn attacks, 89.0% with it (McNemar p < 10⁻⁷). Policy profiles: Strict and Developer change 1 and 2 of 942 decisions. Explainability: worked example with risk score and named detectors, plus a benign prompt with PII masked | Sec. 3.2, Sec. 5.1, Sec. 5.4 |
| R1.5c ✅ | Align Algorithm 1 with the text | Algorithm 1 rewritten from the orchestrator: allowlist check, cache, regex pre-screen, LLM detectors, ensemble θ, Session Judge, Redact masking, write-back rule; a failed LLM call yields FLAG (the code was fixed to fail closed) | Algorithm 1, Sec. 3.7 |
| R1.5d ✅ | Clarify novelty vs Rebuff; discuss training-based and architectural defences | Related Work now contrasts Rebuff (all checks on every request, canary tokens) with our design, and discusses the instruction hierarchy, StruQ, SecAlign and CaMeL as complementary defences that change the model or application | Sec. 2 |

### 6. Minor

| ID | Comment | What we did | Where |
|---|---|---|---|
| R1.6a ✅ | Entropy sum should run over distinct symbols | Fixed (see R2.2) | Eq. 4 |
| R1.6b ✅ | Reconcile Fig. 1 with the eight detectors of Table 1 | Figure corrected (duplicate boxes removed, Secrets label fixed); Context Flooding listed as deterministic; caption states six deterministic plus two LLM-assisted detectors | Fig. 1, Table 1 |
| R1.6c ✅ | Justify the hand-set risk weights or give a sensitivity analysis | The risk score no longer drives blocking. The ensemble weights and points are stated as hand-set during development, not tuned on any evaluation split. Sweeping θ from 2.0 to 6.0 leaves validation F1 unchanged. Automatic weight selection is future work | Sec. 3.5, Sec. 5.2, Sec. 6 |
| R1.6d ✅ | Release code and configurations | Code, configurations, evaluation scripts and per-prompt results are public on GitHub (branch `camera-ready`). The link replaces "link withheld" when the paper is de-anonymised | Sec. 4 |

---

## Other changes made for the camera-ready (not requested item by item)

- **Reproducibility:** all results re-run on official splits; every table generated from per-prompt records; final re-validation of every number (542 checks). It fixed four small issues: a p95 rounding (757 → 756 ms), the 62-attack base of the typo result, "five" (not six) OOD source datasets, and the Results introduction.
- **New experiments:**
  - cold-cache run C9 (Table 2a, Table 5);
  - OOD-500 set (released);
  - Prompt Guard 2;
  - parallel-LLM-agents baseline C1b;
  - policy profiles and explainability example;
  - multi-turn held-out evaluation;
  - per-detector ablation;
  - cache write-back safeguards (Table 4);
  - cache growth and allowlist tests.
- **Code fixes:**
  - LLM detectors fail closed: an API failure gives FLAG, not a silent ALLOW;
  - Levenshtein keyword repair implemented;
  - write-back gated on LLM confirmation and confidence;
  - opt-in conversation memory for the Session Judge;
  - opt-in cache safeguards.

## Still open (deliberate, stated in the paper)

- Deliberate cache-poisoning campaigns and how much the safeguards reduce them (R3.2 / R1.4d): future work.
- Adaptive attackers (R2.4 / R1.4b) and injections aimed at the gate itself (R1.4c, evaluation): future work.
- RLM / sub-agent baselines (R2.1): not done.

## Before submission

- [ ] Cut from 23 pages to the 20-page limit.
- [ ] Supervisor's decision on her two figures.
- [ ] `\revfalse` (remove the yellow highlighting).
- [ ] De-anonymise: authors, affiliations, code link.
- [ ] Upload to Overleaf together with `SF2.png`, then compile and check.
