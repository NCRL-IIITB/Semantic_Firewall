# ICISS 2026 Camera-Ready Tracker

> **Private.** This file contains reviewer comments. Do not push it to the public repo.

Paper: *Multi-agent Semantic Firewall: A Hybrid Defense-in-Depth System for LLM Security*. ICISS 2026, all three reviews recommend acceptance.
LaTeX: `ICISS-Springer/ICISS-Springer/Semantic_Firewall.tex`. Baseline commit `999ce86`.

## Ground rules

1. Work on one item at a time: fix it, verify it, review it together, commit it, then move on.
2. In the LaTeX, change only what a reviewer asked for, and wrap every change in `\rev{}`, which has an on/off switch.
3. Every number in the paper must trace to a raw result file. If one can't, flag it rather than guess.
4. One commit per item, with the item ID in the commit message.
5. Every change gets an entry in `CHANGELOG_CAMERA_READY.md` (what, where, why, how checked, commit).

**Needs** column: `text` = wording fix only · `check` = verify numbers against the code/results, then fix the text · `exp` = needs a new or re-run experiment.
**Status:** `open` · `in progress` · `done (commit)` · `dup → X` (closed when X is closed).

## Order

0. **Setup:** add the `\rev{}` macro with an on/off switch to the `.tex` (no visible change to the paper).
1. Reviewer 3 (R3.3 → R3.1 → R3.2)
2. Reviewer 2
3. Reviewer 1 (whatever isn't already closed as a duplicate)

---

## Reviewer 3

| ID | Issue | Needs | Status |
|---|---|---|---|
| R3.1 | Warm-cache protocol unclear: how the cache is initialised and updated during testing, and showing that the gains aren't from memorising semantically similar test samples. | check / exp | done: Sec 4 + Table 3 text (train-split attacks seed the cache, frozen during evaluation, thresholds on validation); novel-subset check (495 test prompts below τ: F1 88.3 vs GPT-4o 84.8, DeBERTa 82.0) `b77621b` |
| R3.2 | Security of the self-updating cache: false positives or adversarial inputs could affect future decisions. Evaluate error propagation and cache poisoning. | exp | 2026-10-11: mam's poisoning-resistant design (4 safeguards) implemented (off by default) + paper Sec. 3.10 paragraph. Error propagation + cost on normal traffic measured by exact replay → Sec. 5.3 / Table 4 (gate FPs written back never blocked a benign prompt in 150 replays; all four safeguards: +6.9% LLM calls, −0.47 recall from cold, +0.6% LLM calls from warm). Protection side (poisoning campaigns) = future work (mam, 2026-10-11): stated in the threat model, Sec. 5.3 and Limitations. Notes §8, changelog 52–55 |
| R3.3 | τ inconsistency: Table 3 has the best F1 at τ = 0.90, but the text says it peaks at τ = 0.99. Fix this and justify the parameter choices. | check | done: τ = 0.65 selected on validation (exact sweep), used everywhere; Table 3a/3b regenerated `59ef28d` |
## Reviewer 2

| ID | Issue | Needs | Status |
|---|---|---|---|
| R2.1 | Baselines are artificially weak. Compare against RLM and sub-agents, and add a baseline of parallel agents with no deterministic stage. | exp | partly: DeBERTa-v3 + PIGuard added `0cd678f`; RLM / sub-agents / parallel-agents baseline → discuss with mam |
| R2.2 | Formulas: several incorrect or inconsistent formulas in the LaTeX. Check whether the errors carry into the actual calculations. | check | **formulas done** (all match the code; typesetting cleaned). Numbers plugged into Eq. 8 → stage 2 |

**R2.2 equation audit (paper vs code, 2026-10-05)**

| Eq. | What | Maths | Matches code? | Action |
|---|---|---|---|---|
| 1, 2 | cosine sim, s* = max | correct | yes: ChromaDB cosine, sim = 1 − dist, block if ≥ τ (`semantic_cache.py`) | none |
| 3 | T_mesh ≈ max T_i + T_ovh | correct | yes: ThreadPoolExecutor + per-agent timeouts | none |
| 4 | Shannon entropy | **wrong**: summed over positions | code is correct (distinct chars), but it lives in the **Abuse** Detector (flags *low* entropy < 2.5 bits), not the Secrets Detector | **fixed**: formula (R1.6a) + attribution moved to Abuse Detector, Table 1 updated |
| 5 | Levenshtein recursion | correct | was **no** → now **implemented** (`fuzzy_keywords.py`, Injection Detector + LLM escalation) | **fixed**: code + 22 tests + study (33.9% → 80.1% on obfuscated attacks); paper moved to Injection Detector |
| 6 | Risk = min(100, S_sev + S_conf + S_pat + S_ses) | correct | weights match `risk_scorer.py` exactly (0–85 / ≤30 / 5 per trigger ≤20 / ≤10). **But** the code never blocks on Risk ≥ θ: blocking comes from per-detector policy + ensemble score (Σ w·p·sev/2 ≥ 3.5) | **fixed**: purpose stated + new eq:ensemble with θ = 3.5 + Alg. 1 θ lines |
| 7 | L_eff | correct form; constraint crammed into the equation | — | fix together with R1.1f numbers |
| R2.3 | "Self-healing" is inaccurate. Rename it and describe the cache mechanism (stores previously seen malicious intents/prompts) in more depth. | text | done: "self-updating" ×2; Sec 3.2 states it only stores prompts, plus write-back rule, cap/eviction, allowlist behaviour (all from code) |
| R2.4 | Coordinated bypass: try optimising attacks directly against the firewall, at least with a small open-weights model. | exp | open → discuss with mam (adaptive attacker; `adaptive_attack.py` exists, not run) |
## Reviewer 1

### 1. Internal inconsistencies (marked *essential*)

| ID | Issue | Needs | Status |
|---|---|---|---|
| R1.1a | The abstract and conclusion pair F1 83.93% with recall 87.59% as one result, but they come from different datasets (deepset vs BeaverTails). | check | done: abstract/conclusion list F1 per dataset `59ef28d` |
| R1.1b | Sec 5.2 says τ = 0.99 maximises F1 and is adopted; Sec 3.2 and Table 3a use τ = 0.90. | check | done (τ = 0.65 everywhere) `59ef28d` |
| R1.1c | Table 3a F1 values don't follow from its P and R (τ = 0.95 gives about 93.8, not 91.43; τ = 0.99 gives about 92.1, not 88.12). This may change which τ is best. | check | done: Table 3a/3b from exact offline sweeps; every F1 in the paper recomputed from P/R (29 rows OK) |
| R1.1d | Table 3b says N = 500, but all accuracies are multiples of 1/98. | check | done: Table 3b = OOD-500 (250 attacks, 250 benign, released) `c822b21` |
| R1.1e | Share of requests resolved without the LLM is 41.93% in one place and 66.88% in another. | check | done: one number, 50.1% (test split) everywhere |
| R1.1f | Eq. (7): Ph = 0.55, Pc = 0.12, Pl = 0.33 leave Pm = 0, and Leff ≈ 444 ms can't be reproduced from the stated path latencies. | check | done: Eq. 8 with measured P_k (sum 1) and path latencies → L_eff ≈ 308 ms `21eaa14` |
| R1.1g | On a balanced set, P/R of 89.03 / 98.61 imply an FPR of about 12%, not the stated 4.20%. | check | done: FPR column measured directly for every row (ours 8.46%) |
| R1.1h | Several ablation rows share an identical latency (602.83 ms), and C7 (no LLM) is slower than C8 (full pipeline). | check | done: ablation latencies re-measured sequentially; C7 142 ms < C8 304 ms < C1 406 ms `b77621b` |
| R1.1i | LLM provider is Groq in Sec 3.4 but OpenRouter in Sec 4.1. | check | done: OpenRouter (as in code) in Sec 3.4; figure label -> 'LLM API' |
| R1.1j | Wrong model identifiers (`gpt-4.0-…`, the Claude 3.5 Haiku snapshot date). | check | done: Claude 3.5 Haiku retired → Claude Haiku 4.5; OpenRouter IDs in footnote (queried Oct 2026); all baselines re-run on the test split |
| R1.1k | The "six baselines" claim doesn't match the baselines listed. | check | done: 'seven' (GPT-4o, Gemini 2.5 Flash, Claude Haiku 4.5, Llama Guard 4, Rebuff, DeBERTa-v3, PIGuard) `0cd678f` |
| R1.1l | The ± in the abstract is unexplained. | check | done: ± removed (user decision: no CIs) |
### 2. Evaluation protocol

| ID | Issue | Needs | Status |
|---|---|---|---|
| R1.2a | Choose τ and θ on a held-out validation split and report the test set once. Also report θ, which is currently missing. | exp | done: τ and θ jointly on validation, test once; θ = 3.5 reported `2b426f5` |
| R1.2b | Describe the warm-cache protocol precisely (what fills the cache, and how overlap with the test set is avoided). | — | dup → R3.1 |
| R1.2c | Report FPR (or TPR at a fixed FPR) for every system, including the baselines. | check / exp | done: FPR column in Tables 2, 3, 4 |
| R1.2d | Add purpose-built injection detectors as baselines (Prompt Guard 2, a DeBERTa injection classifier, InjecGuard), and describe the prompts used for the zero-shot LLM baselines. | exp | done: DeBERTa-v3, PIGuard (= InjecGuard), Prompt Guard 2 via Groq (F1 69.66) |
| R1.2e | Significance tests with multiple-comparison correction for the deepset comparison and the other claimed wins. | exp | done for neuralchemy: McNemar + Holm vs every baseline `13c8be4`; deepset Rebuff is a reported number (no per-sample data, no test possible) |
### 3. Moderate or support the claims

| ID | Issue | Needs | Status |
|---|---|---|---|
| R1.3a | Present BeaverTails, ai4privacy and walledai as related-task evaluations, not as evidence that prompt-injection detection generalises. | text | done: 'related, unseen tasks' wording |
| R1.3b | The multi-turn set has no benign conversations. Report a multi-turn FPR. | exp | done: held-out 100+100, recall 89.0%, FPR 6.0% (Table 2c, Sec 5.1); conversation memory seeded outside eval goals, threshold on dev |
| R1.3c | Qualify the cost and "LLM bypass" claims: in Algorithm 1, allowed prompts still reach the gate. Report cost and latency at 1–10% attack rates, with p95/p99. | exp | done: LLM-bypass qualified (abstract/conclusion/cost), prevalence 1–10% (LLM calls 94.4–87.4%, L_eff 495–466 ms, p95 944, p99 1242) `21eaa14` `b77621b` |
| R1.3d | Remove or support the claim that the BeaverTails results show real detection rather than memorisation. | text | done: sentence removed |

### 4. Security analysis

| ID | Issue | Needs | Status |
|---|---|---|---|
| R1.4a | Add an explicit threat model. | text | 2026-10-11: mam's threat model added as Sec. 3.9 (her wording, factual fixes only: G1 coverage extended with what we test, G3 black-box test reported, measured vs [TO RUN] G2 parts separated). Notes §8, changelog 52 |
| R1.4b | Evaluate adaptive attackers who know the pipeline. | — | dup → R2.4 |
| R1.4c | Discuss injection attacks against the LLM gate itself. | text / exp | open → discuss with mam |
| R1.4d | Analyse cache poisoning (a false positive persisted as a block) and allowlist abuse (inputs crafted near an approved prompt). | — | dup → R3.2; allowlist part: black-box G3 test (4/100 within 0.90, none allowed in default skip_llm mode) now in Security Considerations (2026-10-11) |
| R1.4e | Discuss cache growth and eviction. | text | done 2026-10-11: Sec 3.2 states cap 10,000 entries, oldest evicted first, plus measured growth/eviction from `cache_robustness/growth.json` (lookup 31/20/19 ms at 1k/5k/20k entries; cap 500 keeps 500 of 2,000) and that the cap is never reached in our experiments (max about 2,700 entries) |
| R1.4f | Discuss the privacy implications of sending unresolved prompts to a third-party API. | text | open → discuss with mam (one sentence would do) |
### 5. Untested components

| ID | Issue | Needs | Status |
|---|---|---|---|
| R1.5a | Per-detector ablations. | exp | done: per-detector ablation, Table 6 (Injection essential; 5 detectors never fire on neuralchemy) |
| R1.5b | Evaluate the Session Judge, Policy Engine profiles and Explainability module (qualitative is OK). | exp / text | done: Session Judge (per-turn 6.0% -> +conversation check 50.0% -> +memory 89.0%, McNemar p<1e-7 per step), policy profiles, explainability example |
| R1.5c | Align Algorithm 1 with the text: allowlist, session risk, Flag/Redact outputs, LLM-assisted detectors, and the fallback when the LLM is unavailable. | check | done: Algorithm 1 rewritten from the orchestrator; Classification Procedure text `b80b853` |
| R1.5d | Clarify novelty relative to Rebuff. Cite and discuss training-based and architectural defences (instruction hierarchy, StruQ/SecAlign, CaMeL). | text | half done: Rebuff novelty; training-based/architectural defences → discuss with mam |
### 6. Minor

| ID | Issue | Needs | Status |
|---|---|---|---|
| R1.6a | Entropy formula: the sum should run over distinct symbols. | text | done: Eq. 4 now sums over distinct chars; verified equal to `AbuseDetectorAgent._shannon_entropy` |
| R1.6b | Reconcile Fig. 1 with the eight detectors in Table 1. | check | done: duplicates removed from figure, Secrets label fixed, Context Flooding = deterministic in Table 1, caption says 6 + 2 |
| R1.6c | Justify the hand-set risk weights or give a sensitivity analysis. | exp / text | partly: risk score now used only for explanations; θ swept 2.0–6.0 with no F1 change |
| R1.6d | Release the code and configurations after review. | text | open: code release link at de-anonymisation (do NOT publish the tracker/changelog: they are tracked in git history) |
### Overlaps

R1.1b = R3.3 · R1.2b = R3.1 · R1.4d ⊂ R3.2 · R1.4b = R2.4 · R1.2d ≈ R2.1 · R1.6a ⊂ R2.2

---

## Stage 2 (later): recheck logs, fix the maths in numbers, re-run doubtful experiments

Agreed with the user on 2026-10-05: stage 1 is text/LaTeX only. Anything that needs numbers traced or re-run waits for stage 2:
- R1.1c Table 3a F1 values vs P/R; R3.3 τ sweep (no raw data; `tune_threshold.py` exists, offline variant runs free)
- R1.1d Table 3b N = 500 vs 98-row golden set
- R1.1e / R1.1f 41.93% vs 66.88%, Eq. 8 path probabilities (P_m = 0), L_eff ≈ 444 ms
- R1.1g FPR 4.20% vs P/R on a balanced set
- R1.1h ablation latencies (602.83 ms repeated, C7 slower than C8)
- Sec 4 claim that thresholds were "selected using a separate 500-sample validation set"
- Main tables were produced without Levenshtein repair (decide whether to re-run)
- Check `llm_failed` in all run predictions: any prompt whose LLM call failed was counted as clean before the fail-closed fix
- deepset: rerun Semantic Firewall on all 662 deepset prompts (train+test, as in Palit & Woods) with `run_system.py --dataset deepset --split all`; replace our deepset row and revisit the Sec 5 Rebuff sentence
- R1.1l ±0.42 in the abstract: no source in repo; after credit is added, compute a bootstrap CI from saved predictions or remove it
- Baseline rows (GPT-4o, Gemini 2.5 Flash, Claude 3.5 Haiku, Llama Guard 4, Rebuff) have no raw results in repo; re-run with `camera_ready/run_baselines.py` (~$1.06 for 4 on test split). Claude 3.5 Haiku is retired on OpenRouter

## Side issues found (not reviewer items)

- Running `pytest` writes into `config/custom_rules.json` and `config/threat_intel_feed.json` (test rule "Project Zephyr"). Restore them with `git checkout -- config/` after test runs until this is fixed.

## Log

| Date | Item | Commit | Note |
|---|---|---|---|
| 2026-10-05 | — | `999ce86` | Baseline: code from GitHub main `dde97ab` + original LaTeX |
| 2026-10-05 | step 0 | `323e576` | `\rev{}` (text) and `\revm{}` (maths) highlight macros; switch with `\revtrue` / `\revfalse` |
| 2026-10-05 | R1.6a | `6abd18e` | Entropy Eq. 4: sum over distinct symbols |
| 2026-10-05 | R2.2 (Eq. 6 / θ) | `bf31254` | Risk score = reporting only; real blocking rule E ≥ θ = 3.5 added; Alg. 1 θ lines fixed |
| 2026-10-05 | R2.2 (Eq. 5 code) | `37f2abd` | Levenshtein keyword repair implemented + 22 tests (227 pass) |
| 2026-10-05 | R2.2 (Eq. 5 study) | `cf32312` | Obfuscated attacks: regex 33.9% → 80.1%; no new FP |
| 2026-10-05 | R2.2 (Eq. 4/5 paper) | `c87a82f` | Entropy → Abuse Detector, Levenshtein → Injection Detector, Table 1 |
| 2026-10-05 | R2.3 | `0205466` | "self-healing" → "self-updating"; cache mechanism described from code |
| 2026-10-05 | R3.3 (text) | `bb38cd7` | Sec 5.2: adopted τ 0.99 → 0.90 (matches Table 3a, Sec 3.2, code) |
| 2026-10-05 | R2.2 (typesetting) | `25279b3` | Eq. 3 N → 8, Eq. 5 "if"/alignment, Eq. 8 spacing |
| 2026-10-05 | R1.6b | `7d03bf4` | Fig. 1 duplicates removed; Table 1 Context Flooding = deterministic |
| 2026-10-05 | R1.1i | `f2fa87e` | Groq -> OpenRouter (Sec 3.4 + figure) |
| 2026-10-05 | R1.1k | `07cf807` | 'six' -> 'five' baselines |
| 2026-10-05 | R1.1j | `323cca7` | Model ID typos fixed in footnote |
| 2026-10-05 | R1.3d | `e92af1a` | BeaverTails memorization sentence removed |
| 2026-10-05 | Rebuff ref. | `493ece4` | Rebuff row cited to Palit & Woods (deepset, 662 prompts) |
| 2026-10-05 | R1.5d (novelty) | `c99d8c0` | Rebuff design-contrast sentence in Related Work |
| 2026-10-05 | R1.5c (code) | `4e489f5` | LLM call failure now fail-closed (FLAG); test fails before / passes after; 228 pass |
| 2026-10-05 | trim | `fa26832` | Long additions shortened (cache, Levenshtein, blocking rule) |
| 2026-10-06 | PII code | `4f6f72f` | PII detector fixes (dev only); 238 tests pass |
| 2026-10-06 | PII pincode + profile | `afc093e` | Pincode context (default); ai4privacy profile opt-in; dev F1 91.3 / 94.9 |
| 2026-10-06 | PII results | `3ca6ab4` | Held-out default 96.87/86.40/91.34; profile 95.50/94.19/94.84 |
| 2026-10-06 | Synthetic Safety | `edac2ea` | Unsupported row removed (Table 2c + Sec 4) |
| 2026-10-06 | R1.2a / R3.3 | `2b426f5` | τ, θ tuned jointly on validation (exact offline sweep) |
| 2026-10-06 | R1.1d | `c822b21` | OOD-500 curated set (Table 3b) |
| 2026-10-06 | baselines | `04bb232` | 2026-10-04 baseline runs reused (ids verified, 0 failures) |
| 2026-10-06 | R1.1a/c/e/g, R1.2c | `59ef28d` | Final Tables 2a/2b, 3a/3b, PII row, Sec 4, abstract/conclusion |
| 2026-10-06 | R1.2e | `13c8be4` | McNemar + Holm vs all baselines; ablation C1–C6 |
| 2026-10-06 | no_robots | `cdc4356` | 500 benign: 2.80% blocked; limitations FPR |
| 2026-10-06 | R1.1f/h, R1.3c | `21eaa14` | Latency (Groq), Eq. 8, prevalence; Table 4; walledai removed |
| 2026-10-06 | R1.5c | `b80b853` | Algorithm 1 aligned with code |
| 2026-10-06 | R1.2d, R1.1k | `0cd678f` | DeBERTa-v3 + PIGuard; seven baselines; hardware sentence |
| 2026-10-06 | R1.1h, R3.1 | `b77621b` | Ablation latencies; read-through fixes; novel-subset check |


## Remaining (2026-10-06)

- **TBD cells:** C9 cold (Table 2a row + Table 4 row; user: leave empty for now), multi-turn row (Table 2c + Sec 5.1 sentence; ON HOLD).
- **Discuss with mam (new content):** threat model (R1.4a), adaptive attacker (R2.4/R1.4b), gate injection (R1.4c), cache poisoning + allowlist abuse (R3.2/R1.4d), third-party API privacy (R1.4f), Session Judge / Policy / Explainability evaluation (R1.5b), training-based defences (R1.5d), RLM / sub-agents / parallel-agents baseline (R2.1).
- **Optional simple runs (ask mam first):**
  1. `cache_robustness.py all`: free, about 15 min, covers R3.2 and R1.4d/e.
  2. Prompt Guard 2: free; needs the Meta licence accepted and a Hugging Face login (R1.2d).
  3. `run_system.py --config llm_agents_only --split test --cache none`: about $0.5–1 (R2.1).
  4. Explainability example under the three profiles: free (R1.5b).
  - The Recycle Bin copy `$R929M0M` has old-code versions of all of these: a preview only, not usable in the paper.
- **Before release:** the tracker and changelog are in git history; publish from a clean export, not this repo's history.
- **Overleaf:** upload the .tex and SF2.png and compile once to check the `\rev{}` highlighting (no local LaTeX).

---

## Original reviews (verbatim)

### Review 1 (Acceptance)

Thank you for submitting this work to the conference.

The paper proposes Semantic Firewall, a layered defense for LLM applications against prompt injection and jailbreaks. Cheap checks (rate limiting, regex, an embedding-based cache of known threats) run first. Unresolved prompts go to a parallel set of eight detectors and then to a schema-constrained Llama-3.3-70B gate, and confirmed threats and approved false positives feed back into the cache and allowlist. The authors evaluate on five public datasets against commercial LLMs, Llama Guard 4, and Rebuff, and report high recall, competitive F1, and lower LLM usage and cost.

The design is sound and relevant, and the practical framing is valuable. However, several headline claims cannot currently be verified in the manuscript due to internal inconsistencies and gaps in the evaluation. I would highly recommend that the authors correct these and moderate their claims.

**Strengths**

The cost-aware, early-exit design (cheap checks first, LLM only for unresolved cases) addresses a real deployment concern and fits the defense-in-depth tradition in security. Adding confirmed threats to a cache and admin-approved false positives to an allowlist is a useful idea that could reduce both cost and repeated errors without retraining.Separating detection from policy, configurable profiles, audit logging, explainable decisions, and human review reflect what production systems need. The detector mesh covers PII, secrets, and context flooding as well as injection. The regex PII layer is fast and shows strong recall on structured identifiers. The paper uses multiple datasets, benign data for false positives, an ablation across nine configurations, a threshold sweep, a significance test, and latency and cost accounting. The ablation is informative in showing that the components are complementary. The authors are also candid about several limitations (throughput, gate size, precision on BeaverTails).

**Recommended revisions**

1. Reconcile internal inconsistencies (essential). I was unable to reconcile the following, and they affect the headline claims:

The abstract and conclusion pair an F1 of 83.93% and a recall of 87.59% as one result, but these come from different datasets (deepset and BeaverTails).
Section 5.2 states that τ = 0.99 maximizes F1 and is adopted, while Section 3.2 and Table 3a use τ = 0.90.
Several F1 values in Table 3a do not follow from the reported precision and recall. For example, τ = 0.95 recomputes to about 93.8, not 91.43, and τ = 0.99 to about 92.1, not 88.12. This may change which threshold is optimal.
Table 3b is described as N = 500, but the reported accuracies (all multiples of 1/98) suggest a much smaller set.
The share of requests resolved without the LLM is 41.93% in one place and 66.88% in another. The path probabilities given for Eq. (7) (Ph = 0.55, Pc = 0.12, Pl = 0.33) leave Pm at zero, and I could not reproduce Leff ≈ 444 ms from the stated path latencies.
On a balanced set, the reported precision and recall (e.g., 89.03% / 98.61%) imply a false-positive rate near 12%, not the stated 4.20%.
Several ablation rows share identical latency (602.83 ms), and the configuration without the LLM (C7) is slower than the full pipeline (C8).
The LLM provider is Groq in Section 3.4 and OpenRouter in Section 4.1. Some model identifiers appear incorrect (e.g., gpt-4.0-..., and the Claude 3.5 Haiku snapshot date). The "six baselines" claim does not match the baselines listed, and the ± in the abstract is unexplained.

2. Strengthen the evaluation protocol.
Select τ and θ on a held-out validation split and report final results once on a separate test set. Report the value of θ, which is currently missing.
Describe the warm-cache protocol precisely (what populates the cache and how overlap with test data is avoided), since the self-healing benefit rests on it.
Report FPR (or TPR at fixed FPR) for all systems, including the baselines, so that F1 comparisons are meaningful.
Add purpose-built injection detectors as baselines (e.g., Prompt Guard 2, a DeBERTa-based injection classifier, InjecGuard), and describe the prompts used for the zero-shot LLM baselines.
Add significance testing (with multiple-comparison correction) for the deepset comparison and other claimed wins.

3. Moderate or better support the claims:
Describe results on BeaverTails, ai4privacy, and walledai as evaluations on related tasks, not evidence of generalization for prompt injection. Note that the multi-turn set has no benign conversations, and report a false-positive rate for multi-turn use.
Qualify the cost and "LLM bypass" claims. As written in Algorithm 1, allowed prompts still reach the LLM gate, so savings depend on attack prevalence. Report cost and latency at realistic attack rates (e.g., 1–10%), with tail latency (p95/p99).
Remove or support the statement that BeaverTails results show real detection ability rather than memorization.

4. Add security analysis.
Include an explicit threat model. Evaluate adaptive attackers who know the pipeline, and discuss injection attacks against the LLM gate itself. Analyze cache poisoning (a false positive persisted as a block) and allowlist abuse (an attacker crafting inputs near an approved prompt). Discuss cache growth and eviction.
Discuss the privacy implications of sending unresolved prompts to a third-party API.

5. Evaluate the components that are proposed but untested: Provide per-detector ablations, and evaluate the Session Judge, Policy Engine profiles, and Explainability module (even qualitatively).
Align Algorithm 1 with the text (allowlist, session risk, Flag/Redact outputs, LLM-assisted detectors, and the fallback behavior when the LLM is unavailable).
Clarify the novelty relative to Rebuff, and cite and discuss training-based and architectural defenses (e.g., instruction hierarchy, StruQ/SecAlign, CaMeL).

6. Minor points: Correct the entropy formula (the sum should run over distinct symbols), clarify Fig. 1 against the eight detectors in Table 1, justify or provide sensitivity analysis for the hand-set risk weights, and release code and configurations after review.

### Review 2 (Acceptance)

This work proposes a solution "Semantic Firewall" that boosts both efficiency and accuracy at defending production LLM systems against jailbreaking and prompt injection attacks.

**Strengths:**
- The proposed system is well thought-out and includes early exit routing. This prevents cases where malicious requests are flagged but still trigger an attack through the full pipeline. Additionally, it includes standard static preprocessing steps such as validating against a cache and static checks before proceding to an agentic approach.
- From both a financially and execution-efficiency standpoint, the deterministic gates in addition to the parallel agents significantly reduce cost while boosting accuracy compared to measured baselines. This suggests the harness-like framing is the way to go.
- Good ablation study, independent components tested and compositional result found to be the strongest.

**Weaknesses / Suggested Revisions:**
- Artifically weak baselines. RLM and sub-agents should be compared against. The module itself contains parallel agents, so one baseline should be parallel agents with no deterministic portion vs the existing setup.
- Formulas need some cleaning up. A few incorrect and inconsistent formulas in latex. Unsure if this carries over to the actual calculation mathematically.
- Cache is described as "self-healing": this is not accurate for what the cache does. This needs to be revised to clarify that it is just a cache that stores previously-seen intents and malicious prompts. Would like to see the mechanism here described more in-depth.
- How hard is it to bypass this detector in a coordinated way? Would like to see some attempts to directly optimize against it just as a benchmark for potential attackers. At least a small open-weights model.

Overall, strong paper! Look forward to seeing the revisions addressed to fully recommend acceptance.

### Review 3 (Acceptance)

**Summary:**
This paper proposes a multi-agent Semantic Firewall for LLM security, combining regex-based screening, semantic memory, parallel security detectors, risk aggregation, and an LLM gate. The system uses early exit to reduce unnecessary LLM calls and reports strong prompt-injection detection performance with lower latency and cost.

**Strengths:**
The layered design is practical and well motivated. The semantic cache is an interesting idea that allows previously detected attacks to be blocked earlier, and the ablation study suggests that combining multiple components improves performance over using the LLM gate alone.

**Weaknesses / Suggested Revisions:**
The warm-cache evaluation protocol is not sufficiently clear. The authors should clarify how the semantic cache is initialized and updated during testing, and ensure that the reported gains do not result from memorizing semantically similar test samples.
The security implications of the self-updating semantic cache are not sufficiently evaluated. Since newly detected threats are automatically added to the cache, false positives or adversarially crafted inputs may affect future decisions. The authors should evaluate the robustness of the cache against error propagation and potential cache-poisoning attacks.
Several experimental details and numerical descriptions are inconsistent. For example, Table 3 shows the best F1 at \(τ=0.90\), while the text states that it peaks at \(τ=0.99\). The paper should carefully revise these inconsistencies and provide clearer justification for parameter choices.
