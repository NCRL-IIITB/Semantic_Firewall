# Camera-Ready Change Log

> **Private.** Do not push to the public repo.

This file records every change made for the camera-ready: what changed, where, which reviewer comment it answers, how it was checked, and the commit. Add one entry per change, newest at the bottom. It is also the basis for the response to reviewers.

- Paper: `ICISS-Springer/ICISS-Springer/Semantic_Firewall.tex`. Line numbers refer to the file at that commit.
- Item IDs (R1.6a, R2.2, …) come from [CAMERA_READY_TRACKER.md](CAMERA_READY_TRACKER.md).
- To see any change exactly, run `git show <commit>`.

## Summary

| # | Date | Item(s) | Where | Change | Commit |
|---|---|---|---|---|---|
| 0 | 2026-10-05 | setup | preamble (l. 24–35) | Added `\rev{}` / `\revm{}` yellow-highlight macros with an on/off switch | `323e576` |
| 1 | 2026-10-05 | R1.6a, R2.2 | Sec 3.3, Eq. 4 (l. 173, 176) | Entropy formula now sums over distinct characters | `6abd18e` |
| 2 | 2026-10-05 | R2.2, R1.2a (θ part), R1.5c (θ lines) | Sec 3.2 (l. 116), Sec 3.5 after Eq. 6 (l. 218–225), Alg. 1 (l. 256, 258) | Says what the risk score is used for; adds the real blocking rule (new Eq. `eq:ensemble`) with θ = 3.5; Alg. 1 θ test fixed | `bf31254` |
| 3 | 2026-10-05 | R2.2 (Eq. 5) | **code**: `core/agents/fuzzy_keywords.py` (new), `injection_detector.py`, `orchestrator.py`, `settings.py`, `tests/test_fuzzy_keywords.py` (new) | Implemented Levenshtein keyword repair (Eq. 5) in the injection regex and the LLM-escalation keyword check | `37f2abd` |
| 4 | 2026-10-05 | R2.2 (Eq. 5) | **experiment**: `camera_ready/levenshtein_study.py` → `results_camera_ready/levenshtein_study.json` | Measured usefulness: off vs on, natural + obfuscated attacks | `cf32312` |
| 5 | 2026-10-05 | R2.2 (Eq. 4, 5) | Table 1 (Secrets, Abuse, Injection rows), Sec 3.3 paragraph around Eq. 4–5 | Entropy attributed to the Abuse Detector; Levenshtein moved to the Injection Detector with its result; Secrets = signatures only | `c87a82f` |
| 6 | 2026-10-05 | R2.3 | Abstract, Sec 3 intro (l. 109), Sec 3.2 (l. 140) | "Self-healing" → "self-updating"; cache described as only storing prompts, with its write-back rule, size cap and allowlist behaviour | `0205466` |
| 7 | 2026-10-05 | R3.3, R1.1b (text) | Sec 5.2 (l. 513) | Adopted τ in the text 0.99 → 0.90, matching Table 3a, Sec 3.2 and the code | `bb38cd7` |
| 8 | 2026-10-05 | R2.2 (typesetting) | Eq. 3 (l. 143–145), Eq. 5 (l. 180, 187), Eq. 8 (l. 294–295) | N → 8 in Eq. 3; "if" + alignment in Eq. 5; spacing before ΣP_k = 1 in Eq. 8 | `25279b3` |
| 9 | 2026-10-05 | R1.6b | Fig. 1 (`SF2.png`), Fig. 1 caption, Table 1 (Context Flooding row) | Duplicate Injection/Unsafe boxes removed from the figure; Secrets label 'Regex + Entropy' -> 'Regex'; Context Flooding is deterministic in Table 1; caption states 6 + 2 = 8 detectors | `7d03bf4` |
| 10 | 2026-10-05 | R1.1i | Sec 3.4; Fig. 1 API box | Provider: Groq -> OpenRouter (text); figure label 'Groq LLM API' -> 'LLM API' | `f2fa87e` |
| 11 | 2026-10-05 | R1.1k | Sec 1, Sec 5 intro | 'six baselines' -> 'five' (GPT-4o, Claude 3.5 Haiku, Gemini 2.5 Flash, Llama Guard 4, Rebuff) | `07cf807` |
| 12 | 2026-10-05 | R1.1j | Sec 4.1 footnote | Model IDs: gpt-4.0 -> gpt-4o-2024-05-13; Claude 3.5 Haiku date -> 20241022; gemini-2.5-flash-001 -> gemini-2.5-flash | `323cca7` |
| 13 | 2026-10-05 | R1.3d | Sec 6 (Limitations) | Deleted the BeaverTails 'real detection ability rather than pretraining memorization' sentence | `e92af1a` |
| 14 | 2026-10-05 | R1.5d (Rebuff ref.) | Sec 4 datasets, Table 2b, Sec 5 cross-dataset text, bibliography | Rebuff row now cites Palit & Woods (arXiv:2505.13028): N 662, P 63.30, R 75.60, F1 68.90; 'native dataset' and 'manually maintained signatures' claims fixed | `493ece4` |
| 15 | 2026-10-05 | R1.5d (novelty) | Sec 2 Related Work | One sentence: how Semantic Firewall differs from Rebuff (design only) | `c99d8c0` |
| 16 | 2026-10-05 | R1.5c (code) | **code**: `orchestrator.py` `_run_agent`, `tests/test_firewall_safety.py` | Failed LLM call in Injection/Unsafe detectors now fail-closed (SYSTEM_UNAVAILABLE -> FLAG) instead of 'clean' | `4e489f5` |
| 17 | 2026-10-05 | trim (items 2, 5, 6) | Sec 3.2, 3.3, 3.5 | Shortened the cache, Levenshtein and blocking-rule additions (~543 -> ~368 words added vs original) | `fa26832` |
| 18 | 2026-10-06 | PII (Table 2c) | **code**: `pii_detector.py`, `pii_eval.py` (`--part`), `tests/test_pii_detector.py` | Overlap handling, capture groups, phone/date formats; dev F1 72.7 -> 84.7 (held-out pending) | `4f6f72f` |
| 19 | 2026-10-06 | PII (Table 2c) | **code**: `pii_detector.py`, `pii_eval.py` (`--profile`), tests | Pincode needs context (default); opt-in ai4privacy profile (bare 8-digit accounts, 756 SSNs). Dev F1: default 91.3, profile 94.9 | `afc093e` |
| 20 | 2026-10-06 | PII (Table 2c) | **results**: `results_camera_ready/pii/` | Held-out: default F1 91.34, profile F1 94.84; all rows: 91.32 / 94.85 | `3ca6ab4` |
| 21 | 2026-10-06 | Table 2c | Sec 4 datasets, Table 2c | Removed the unsupported 'Synthetic Safety' row and its Sec 4 mention | `edac2ea` |

---

## Details

### 0. Revision-highlight macros (setup)

- **Item:** none. This is setup, so every later change can be shown in yellow.
- **Where:** preamble, just after `\urlstyle{rm}`.
- **Change:** loads `soul` and adds `\rev{text}` (text highlight), `\revm{maths}` (maths highlight) and the `\revtrue` / `\revfalse` switch.
- **Visible effect:** none until a `\rev` is used. With `\revfalse` the PDF is identical to having no macros.
- **Check:** not compiled locally (no LaTeX on this laptop). **Needs one compile on Overleaf.**
- **Commit:** `323e576`

### 1. Entropy formula (Eq. 4)

- **Items:** R1.6a (Reviewer 1: "the sum should run over distinct symbols"); part of R2.2 (Reviewer 2: "incorrect formulas").
- **Where:** Sec 3.3, Parallel Agent Execution Mesh, Eq. 4 and the sentence after it.
- **Before:**
  ```latex
  H(X)=-\sum_{i=1}^{n}p(x_i)\log_2 p(x_i),
  where $p(x_i)$ is the empirical probability of character $x_i$.
  ```
- **After:**
  ```latex
  H(X)=-\revm{\displaystyle\sum_{c\in\Sigma_X}p(c)\log_2 p(c)},
  where \rev{$\Sigma_X$ is the set of distinct characters in $X$ and $p(c)=n_c/n$ is the
  empirical probability of character $c$, which occurs $n_c$ times in $X$.}
  ```
- **Why:** summing over positions counts a repeated character once per occurrence, which overstates the entropy.
- **Does it affect results?** No. The code (`AbuseDetectorAgent._shannon_entropy`) already sums over distinct characters. Only the paper's formula was wrong.
- **Check:** ran the code against the new and old formulas.

  | String | Code | New formula | Old formula |
  |---|---|---|---|
  | `aab` | 0.9183 | 0.9183 | 1.3083 |
  | `password123password123` | 3.2776 | 3.2776 | 7.4496 |
  | `sk-proj-Xq9vL2mT8rW4` | 4.1219 | 4.1219 | 4.7863 |

  Braces and `$` on the edited lines are balanced.
- **Wording changed:** one sentence (the definition of the symbols).
- **Open:** the paper puts entropy in the Secrets Detector, but the code uses it only in the Abuse Detector (to flag *low*-entropy, repetitive input). Not yet changed; it waits for the user's decision.
- **Commit:** `6abd18e`

### 2. Risk score vs. the actual blocking rule (Eq. 6, new ensemble equation, Alg. 1)

- **Items:**
  - R2.2 (Reviewer 2: "inconsistent formulas … unsure if this carries over to the actual calculation").
  - R1.2a, θ part only (Reviewer 1: "report the value of θ, which is currently missing").
  - R1.5c, θ lines of Algorithm 1 only.
- **Problem:** Eq. 6 matches the code (`risk_scorer.py`), but the code never blocks on Risk ≥ θ. The score is computed *after* the decision and is used only for the explanation report and session tracking. Blocking comes from:
  1. Per-detector policy actions: (threat type, severity) → action, and the strictest wins.
  2. Ensemble escalation in `orchestrator._ensemble_action`, with thresholds 3.5 / 2.6 / 1.8 (`settings.py`).
- **Changes:**
  1. **Sec 3.5, after "capped at 100.":** added a sentence saying the risk score is for explanation, audit and session tracking, not blocking. Then added the new equation (`\label{eq:ensemble}`):
     ```latex
     E(\mathcal{R})=\sum_{a\in\mathcal{A}}w_a\,p_a\,\frac{\max(1,s_a)}{2}
     ```
     It is followed by definitions of w_a ∈ [1.0, 1.3], p_a ∈ [0, 1] and s_a ∈ {0..4}, and the decision rule: Block if any policy action is Block or E ≥ θ = 3.5; Redact at ≥ 2.6; Flag at ≥ 1.8.
  2. **Sec 3.5, next paragraph:** "The resulting score is evaluated by the Policy Engine" → "Detector outputs are evaluated by the Policy Engine". *(Not in the agreed plan, but needed so the text doesn't contradict item 1.)*
  3. **Sec 3.2, Policy Engine:** "maps this score onto" → "maps detector outputs onto". *(Same reason.)*
  4. **Algorithm 1:**
     - `r ← Risk(R)` → `e ← E(R)`
     - `if r ≥ θ` → `if e ≥ θ ∨ PolicyAction(R) = Block`
- **Does it affect results?** No. The text now describes what the code already did.
- **Check:**
  - Compared the paper's E and its thresholds with `_ensemble_action` on 10,000 random detector outputs: 0 mismatches in score or action.
  - Weights in `settings.py` range from 1.0 to 1.3.
  - The experiment harness (`camera_ready/common.py`) does not override the ensemble thresholds, and no policy profile overrides them.
  - Braces and `$` on all added lines are balanced.
- **Numbering:** the new equation becomes Eq. 7, and effective latency moves to Eq. 8. This changes again if Eq. 5 (Levenshtein) is removed. The response letter must map the reviewers' equation numbers to the final ones.
- **Caveats, still open:**
  - θ = 3.5 is a hand-set default, not tuned on validation data, yet Sec 4 says thresholds were "selected using a separate 500-sample validation set" (R1.2a, R1.6c).
  - Algorithm 1 still sends every unblocked prompt to the LLM, whereas the code calls the LLM only when a cheap heuristic score is ≥ 1.0 (R1.5c).

### 3. Levenshtein keyword repair: code (Eq. 5 implemented)

- **Item:** R2.2. Reviewer 2 asked whether the formulas carry over to the actual calculation. Eq. 5 (Levenshtein) was described in the paper, but no code computed it.
- **Decision (user):** keep Eq. 5 and implement it where it is useful: the Injection Detector, not the Abuse Detector.
- **What was added:**
  - **New module `semantic_firewall/core/agents/fuzzy_keywords.py`:**
    - `levenshtein()`: Wagner–Fischer, exactly Eq. 5, with an optional early-exit bound.
    - `keywords_from_patterns()`: the literal 5+-letter words inside the regex patterns.
    - `KeywordRepairer`: maps leetspeak digits (0→o, 1→i, 3→e, 4→a, 5→s, 7→t, @→a, $→s), then rewrites any token within **1 edit (5–7 letters)** or **2 edits (8+ letters)** of a keyword to that keyword.
  - **`InjectionDetectorAgent._regex_prescreen`:** if repair changes the text, the patterns that had not already matched are run again on the repaired text. This can only **add** matches. Matches are marked `meta["fuzzy_keyword_match"]` and described as "(obfuscated keywords)".
  - **`orchestrator._llm_gate_score`:** the keyword check that decides whether to call the LLM is also run on the repaired text. Its regex moved to the module constant `_LLM_ESCALATION_PATTERN`, unchanged.
  - **Setting:** `fuzzy_keywords_enabled` / env `SEMANTIC_FIREWALL_FUZZY_KEYWORDS_ENABLED` (default on; `0` turns it off).
- **Tests:** `tests/test_fuzzy_keywords.py`, 22 tests, including:
  - Levenshtein equals the recursive definition of Eq. 5 on 300 random string pairs.
  - Repair of typos and leetspeak; benign text unchanged; the switch turns it off.
  - **Full suite: 227 passed** (205 before + 22 new).
- **Known limitation, documented in a test:** plain Levenshtein counts an adjacent swap (`igonre`) as 2 edits, so swaps in words of 7 letters or fewer are not repaired. Damerau distance would fix this, but it would no longer be the paper's Eq. 5.
- **Commit:** `37f2abd`

### 4. Levenshtein usefulness study

- **Script:** `semantic_firewall/benchmarks/research/camera_ready/levenshtein_study.py`. No LLM calls, cached datasets.
- **Raw results:** `semantic_firewall/benchmarks/research/results_camera_ready/levenshtein_study.json`.
- **Natural data**, repair off → on:

  | Dataset | Regex hits on attacks | Regex benign FP | LLM-escalated attacks | LLM-escalated benign |
  |---|---|---|---|---|
  | neuralchemy validation (534 att / 407 ben) | 46 → 48 | 8 → 8 | 56 → 65 | 7 → 7 |
  | neuralchemy test (552 / 390) | 62 → 64 | 3 → 3 | 59 → 69 | 7 → 7 |
  | deepset test (60 / 56) | 8 → 8 | 0 → 0 | 2 → 2 | 0 → 0 |
  | no_robots test (0 / 500) | — | 4 → 4 | — | 15 → 19 |

  - The 2 extra neuralchemy test catches are natural leetspeak attacks (`byp4ss`, `byp@s$ …`). Both were **missed** in the existing deterministic-only and fast-layer runs, so this is a real gain with no new false positives.
  - The 4 extra no_robots escalations (for example `taken` → `token`) are extra LLM calls, not blocks.
- **Obfuscated attacks:** the 62 neuralchemy test attacks that the regex catches when clean, with one random edit (swap, deletion, substitution or leetspeak) in every keyword; 3 seeds.
  - **Regex detection: 33.9% off → 80.1% on.**
  - Attacks escalated to the LLM (all 552 test attacks): 29–33 off → 61–66 on. The clean baseline is 59.
- **Cost:** 0.25 → 0.76 ms per prompt (regex + escalation check).
- **Effect on reported numbers:** the paper's main results were produced without this feature. If re-run with it on, deterministic-layer recall on neuralchemy test rises slightly (+2 TP) with no FP change. Full-system effect not measured (needs LLM runs).
- **Commit:** `cf32312`

### 5. Paper: entropy and Levenshtein attribution, Table 1

- **Item:** R2.2. Formulas must match what the code computes.
- **Table 1:**
  - Secrets: "Combines signature matching with Shannon entropy to flag high-entropy tokens and credentials." → `\rev{Uses signature matching to flag API keys, tokens, and credentials.}`
  - Abuse: "Uses approximate (Levenshtein-distance) string matching to catch lexically obfuscated abusive terms." → `\rev{Flags repetition, low-entropy input, homoglyphs, and encoded payloads.}`
  - Injection: "Combines regex screening with the LLM Gate…" → "Combines regex screening `\rev{(with Levenshtein repair of obfuscated keywords)}` with the LLM Gate…"
- **Sec 3.3 text:**
  - "The Secrets Detector combines signature matching with Shannon entropy to detect high-entropy tokens and potential credentials." → "The Secrets Detector `\rev{uses signature matching to detect API keys, tokens, and other credentials. The Abuse Detector uses Shannon entropy to flag highly repetitive input}`."
  - After Eq. 4 (inside the existing `\rev`), two sentences added:
    - Inputs of ≥ 50 characters with H(X) < 2.5 bits are flagged, and the other Abuse checks are listed (code: `abuse_detector.py`, `ENTROPY_LOW_THRESHOLD = 2.5`, length ≥ 50).
    - "The Abuse Detector identifies lexical obfuscations using…" → "The Injection Detector handles lexical obfuscation (typos and leetspeak) using…"
  - **Eq. 5 and its definition are unchanged.**
  - The final sentence "…obfuscated variants of known abusive terms." → "…obfuscated variants of `\rev{the keywords in its regex patterns: …}`". The new text covers:
    - the repair rule: distance 1 for 5–7 letters, 2 for 8+;
    - that it is also applied to the LLM-escalation check;
    - that it was **added for the camera-ready and is not in the main results (Tables 2–4)**;
    - the result: 33.9% → 80.1%, 3 seeds, no new regex FP on 890 benign prompts (390 neuralchemy test + 500 no_robots).
- **Check:**
  - Every number traces to `levenshtein_study.json`.
  - Braces and `$` are balanced on all changed lines, and the file has no control characters. (An escaping slip briefly inserted carriage returns; it was found and fixed before the commit.)
- **Wording:** the paragraph grew by about 3 sentences. That is needed to explain a feature that is new in the code.
- **Commit:** `c87a82f`

### 6. "Self-healing" cache: renamed and mechanism described

- **Item:** R2.3. Reviewer 2: "self-healing" is not accurate; it is just a cache of previously seen intents and malicious prompts; describe the mechanism in more depth. This also covers part of R1.4e (cache growth/eviction) and gives background for R3.1/R3.2.
- **Changes:**
  1. **Abstract:** "A self-healing semantic cache" → "A `\rev{self-updating}` semantic cache". This matches contribution 2, which already said "self-updating".
  2. **Sec 3 intro:** "written back into a self-healing semantic cache" → "… a `\rev{self-updating}` semantic cache".
  3. **Sec 3.2, first sentence:** "The semantic memory is updated through a symmetric, self-healing feedback loop on both sides of the cache." → "The semantic memory is updated `\rev{on both sides of the cache. It only stores previously seen prompts (confirmed malicious prompts and administrator-approved prompts); no detector or model is modified.}`"
  4. **Sec 3.2, after "…its embedding is added to the threat cache.":** added `\rev{Only detections by the LLM-assisted detectors that the LLM Gate confirms with confidence of at least 0.85 are written; allowlisted prompts and detector failures are never written, and the cache holds at most 10{,}000 entries, evicting the oldest first.}`
  5. **Sec 3.2, allowlist:** "…future prompts of similar intent bypass the LLM Gate and are allowed directly." → "…future prompts of similar intent `\rev{(cosine similarity $\geq 0.90$) skip the LLM Gate, while the deterministic detectors still run}`." The old text was wrong for the default mode.
- **Source in code:**
  - `semantic_cache.py`: `writeback_allowed`, `writeback_min_confidence = 0.85`, `max_entries = 10000`, `_evict_locked` (oldest timestamp first), `allowlist_threshold = 0.90`.
  - `orchestrator.py`, write-back loop: only `llm_agents`, and never when the prompt was allowlisted or the result is `SYSTEM_UNAVAILABLE`.
  - `orchestrator.py`, `allowlist_hit`: sets `run_llm_agents = False`, while the deterministic stage still runs.
  - `settings.py`: `allowlist_mode = "skip_llm"` by default.
  - The experiment harness (`camera_ready/common.py`) uses the same values: write-back 0.85, LLM required, `skip_llm`.
- **Not changed:** "harden with exposure" phrasing in Sec 1 and Sec 3, kept to keep the change minimal.
- **Check:** braces and `$` balanced on all changed lines; no control characters; no "self-healing" left in the paper.
- **Commit:** `0205466`

### 7. τ inconsistency (text only)

- **Items:** R3.3, R1.1b. Table 3a selects τ = 0.90 and Sec 3.2 says 0.90, but Sec 5.2 said "F1 peaks at τ = 0.99, which we adopt…" while quoting the τ = 0.90 row (93.57% F1, 98.61% recall, 4.20% FPR).
- **Change:** Sec 5.2: "F1 peaks at $\tau$=0.99, which we adopt" → "F1 peaks at `\rev{$\tau$=0.90}`, which we adopt". One token.
ev{$	au$=0.90}`, which we adopt". One token.
- **Matches the code:** `cache_similarity_threshold` defaults to 0.90 in `settings.py`.
- **Not fixed here (stage 2):**
  - Table 3a has no raw result file. `results/threshold/threshold_sensitivity.json` is a different sweep (the LLM-gate keyword threshold).
  - Its F1 column doesn't follow from its P/R (R1.1c). Recomputed: 0.80 → 87.50, 0.85 → 91.71, 0.90 → 93.57, 0.95 → 93.82, 0.99 → 92.11. With these values τ = 0.95 would edge out 0.90.
  - So the τ choice and its justification must be redone from a real validation sweep (`camera_ready/tune_threshold.py`) before the numbers are final.
- **Check:** one-line diff; braces and `$` balanced.
- **Commit:** `bb38cd7`

### 8. Formula typesetting clean-up (no numbers)

- **Item:** R2.2. Reviewer 2: "formulas need some cleaning up … inconsistent formulas in latex".
- **Changes (LaTeX only; nothing computed changes):**
  1. **Eq. 3, parallel latency:** `N` → 8. "$T_1,\ldots,T_N$" → "$T_1,\ldots,T_8$" and "\max_{1\leq i\leq N}" → "\max_{1\leq i\leq 8}". In the rest of the paper, N is the dataset size; Algorithm 1 already defines D = {D_1,…,D_8}.
  2. **Eq. 5, Levenshtein:** the cases are now aligned with `&`, and the first case reads "if min(i,j) = 0" (it previously had spacing and no "if").
  3. **Eq. 8, effective latency:** the side condition ΣP_k = 1 is now separated by `\qquad` instead of being run straight on after the last term.
- **Not changed (stage 2):** the numbers plugged into Eq. 8 in Sec 5.4 (P_h, P_c, P_l with P_m = 0; L_eff ≈ 444 ms) — R1.1f.
- **Check:** braces and `$` balanced; no control characters; the `\revm` content compiles the same with `\revfalse`. (Needs the Overleaf compile like everything else.)
- **Commit:** `25279b3`

### 9. Fig. 1 vs. the eight detectors (R1.6b)

- **Item:** R1.6b. Reviewer 1: "clarify Fig. 1 against the eight detectors in Table 1".
- **Problem:**
  - Fig. 1 drew Injection and Unsafe Content twice: once in the "Fast / deterministic" grid and once under "LLM-assisted". That made 10 boxes.
  - Table 1 called Context Flooding "LLM-assisted".
  - In the code, only Injection and Unsafe Content are LLM agents (`orchestrator.llm_agents`), and Context Flooding is a plain 4,000-character length check (`dos_detector.py`, `max_chars = 4000`).
- **Figure (`ICISS-Springer/ICISS-Springer/SF2.png`, edited with PIL):**
  - Removed the duplicate Injection and Unsafe Content boxes from the bottom deterministic row, and moved Context Flooding left next to Custom Rules.
  - Secrets box label "Regex + Entropy" → "Regex". The word is copied from the PII box, so the font matches. This is consistent with item 5: entropy is used by the Abuse Detector.
  - Result: 6 fast/deterministic + 2 LLM-assisted = 8 detectors.
  - The previous image is in git (`git show 999ce86:ICISS-Springer/ICISS-Springer/SF2.png`).
- **Table 1:** Context Flooding category "LLM-assisted" → `\rev{Fast / deterministic}`.
- **Fig. 1 caption:** added `\protect\rev{The mesh contains the eight detectors of Table~\ref{tab:detectors}: six fast/deterministic and two LLM-assisted.}` (`\protect` because `soul` is fragile in captions).
- **Upload:** the `.tex` loads `ICISS_Springer_Submission/SF2.png`, so the new image must be uploaded to that folder on Overleaf.
- **Not changed:**
  - The figure still says "Groq LLM API". That belongs to R1.1i (provider), which is next.
  - Hand-set risk weights (the third part of R1.6) → stage 2 (needs a sensitivity run).
- **Check:**
  - 227 tests pass.
  - Static LaTeX check: braces 570/570, all environments balanced, no undefined refs or cites, no odd `$` per paragraph, no control characters.
- **Commit:** `7d03bf4`

### 10. LLM provider: Groq → OpenRouter (R1.1i)

- **Item:** R1.1i. Reviewer 1: "The LLM provider is Groq in Section 3.4 and OpenRouter in Section 4.1."
- **Fact:** `core/agents/llm_client.py` uses `base_url="https://openrouter.ai/api/v1"` with `DEFAULT_LLM_MODEL = "meta-llama/llama-3.3-70b-instruct"`. No Groq code exists in `core/`.
- **Changes:**
  1. **Sec 3.4:** "Llama-3.3-70B-Instruct through the Groq API." → "Llama-3.3-70B-Instruct through `\rev{OpenRouter}`."
  2. **Fig. 1 (`SF2.png`):** the box label "Groq LLM API" → "LLM API". The words "LLM API" are re-used from the original label, so the font matches, and re-centred next to the icon. Generic on purpose: the provider is named in the text.
- **Sec 4.1** already said OpenRouter, so it is unchanged.
- **Check:** no "Groq" left in the `.tex`; static LaTeX check clean; figure inspected visually.
- **Upload:** the updated `SF2.png` again needs uploading to `ICISS_Springer_Submission/` on Overleaf.
- **Commit:** `f2fa87e`

### 11. "Six baselines" → "five" (R1.1k)

- **Item:** R1.1k. Reviewer 1: "The 'six baselines' claim does not match the baselines listed."
- **Count in the paper:** Sec 4.1 and Table 2 list GPT-4o, Claude 3.5 Haiku, Gemini 2.5 Flash, Llama Guard 4 (Table 2a/2b) and Rebuff (Table 2b). That is **five**.
- **Changes:**
  - Sec 1: "against six representative baselines" → "against `\rev{five}` representative baselines".
  - Sec 5 intro: "baseline comparisons against six representative systems" → "… against `\rev{five}` representative systems".
- **Note:** if baselines are added later (R1.2d: Prompt Guard 2, DeBERTa, PIGuard/InjecGuard; R2.1: parallel agents only), update this count again.
- **Commit:** `07cf807`

### 12. Model identifiers in the API-snapshot footnote (R1.1j)

- **Item:** R1.1j. Reviewer 1: "Some model identifiers appear incorrect (e.g., gpt-4.0-..., and the Claude 3.5 Haiku snapshot date)."
- **User decision (2026-10-05):** fix the ID typos only; no rerun now.
- **Change (Sec 4.1 footnote):**

  | Before | After | Why |
  |---|---|---|
  | `gpt-4.0-2024-05-13` | `\rev{gpt-4o-2024-05-13}` | "gpt-4.0" is not a model; the GPT-4o snapshot of that date is `gpt-4o-2024-05-13` (listed on OpenRouter as `openai/gpt-4o-2024-05-13`) |
  | `claude-3-5-haiku-20240307` | `\rev{claude-3-5-haiku-20241022}` | 2024-03-07 is the Claude **3** Haiku date; Anthropic's only Claude 3.5 Haiku snapshot is `claude-3-5-haiku-20241022` |
  | `gemini-2.5-flash-001` | `\rev{gemini-2.5-flash}` | no `-001` variant exists; the stable ID is `gemini-2.5-flash` (listed on OpenRouter as `google/gemini-2.5-flash`) |

- **Checked:** the OpenRouter public model list (2026-10-05) contains `openai/gpt-4o-2024-05-13` and `google/gemini-2.5-flash`. It does **not** contain `gpt-4.0-…`, `gemini-2.5-flash-001`, or any Claude 3.5 Haiku.
- **Limitation (not fixable by text):**
  - No raw result file in the repo produced the GPT-4o / Gemini / Claude rows of Table 2a. We therefore cannot prove these snapshots produced those numbers; the IDs are corrected to the valid names of the models the paper names.
  - **Claude 3.5 Haiku is retired on OpenRouter**, so that row cannot be re-run. For stage 2, either keep the original numbers with a note, or replace it with a current model.
- **Check:** static LaTeX check clean.
- **Commit:** `323cca7`

### 13. Removed the BeaverTails "real detection, not memorization" claim (R1.3d)

- **Item:** R1.3d. Reviewer 1: "Remove or support the statement that BeaverTails results show real detection ability rather than memorization."
- **User decision:** remove.
- **Change (Sec 6, Limitations & Future Work):** deleted the sentence "Finally, strong performance on the unseen BeaverTails dataset suggests the results reflect real detection ability rather than pretraining memorization." The paragraph now continues directly with "Future work will also explore…".
- **Highlighting:** this is a deletion, so it can't be shown with `\rev`. List it in the response letter.
- **Check:** static LaTeX check clean.
- **Commit:** `e92af1a`

### 14. Rebuff reference row: cited source (Palit & Woods)

- **Context (user, 2026-10-05):** Rebuff is a *reference point*, like Llama Guard: online results for a known tool on the same dataset, used to place our system's performance. We do not claim to beat it.
- **Problem:** the Rebuff row (P 86.20 / R 79.10 / F1 82.50) had no source.
  - Rebuff was never run in this repo: `benchmarks/adapters.py` says "None are registered yet".
  - No published source with these numbers was found.
  - Sec 4 also called deepset "the native evaluation dataset of Rebuff", which is not verifiable.
- **Source used:** Palit, S., Woods, D.: *Evaluating the efficacy of LLM safety solutions: the Palit benchmark dataset*, arXiv:2505.13028, **Table 5** ("Deepset Dataset Metrics").
  - Rebuff: Accuracy 72.96%, Precision 0.633, Recall 0.756, FPR 0.288, F1 0.689, latency 18.465 s/prompt.
  - Data: the publicly available deepset/prompt-injections dataset, **662 prompts (263 malicious, 399 benign)**, i.e. deepset train + test.
- **Changes:**
  1. **Sec 4 (datasets):** "the native evaluation dataset of \textit{Rebuff}~\cite{rebuff}" → "`\rev{a public benchmark on which}` \textit{Rebuff}~\cite{rebuff} `\rev{has been independently evaluated~\cite{palit2025}}`".
  2. **Table 2b, Rebuff row:**

     | Column | Before | After |
     |---|---|---|
     | N | — | 662 |
     | Precision | 86.20 | 63.30 |
     | Recall | **79.10** | **75.60** |
     | F1 | 82.50 | 68.90 |
     | Notes | "Pipeline" | "Reported~\cite{palit2025}" |

     All changed cells are wrapped in `\rev`. Bold stays on Rebuff's recall, since 75.60 > 75.14.
  3. **Table 2b, Semantic Firewall deepset row:** the note "Rebuff's dataset" → `\rev{---}`. Its numbers are unchanged and go to stage 2.
  4. **Sec 5 (Cross-Dataset Evaluation):** "…achieves an F1 score of 83.93%, comparable to Rebuff's 82.50%, while using automatic semantic caching instead of manually maintained attack signatures." → "…achieves an F1 score of 83.93%`\rev{; for reference, an independent evaluation reported an F1 score of 68.90\% for Rebuff on this dataset~\cite{palit2025}}`."
     - Neutral wording, with no "better than" claim.
     - The "manually maintained attack signatures" clause was dropped: it misdescribed Rebuff, which also keeps a vector store of past attacks (as Sec 2 already says).
  5. **Bibliography:** new `\bibitem{palit2025}`. Not highlighted.
- **Stage 2 (agreed):** rerun Semantic Firewall on the same 662 deepset prompts.
  - Command: `run_system.py --dataset deepset --split all`.
  - Then replace our deepset row (95.10 / 75.14 / 83.93, currently untraceable) with traceable numbers on the same data as the cited Rebuff row, and revisit the Sec 5 sentence.
- **Check:** static LaTeX check clean (braces 589/589; every cite has a bibitem and every bibitem is cited).
- **Commit:** `493ece4`

### 15. Novelty relative to Rebuff (R1.5d, first half)

- **Item:** R1.5d. Reviewer 1: "Clarify the novelty relative to Rebuff".
- **Approach:** design contrast only. No empirical "better than" claim; Rebuff is a cited reference point (see item 14).
- **Change (Sec 2, Related Work):** after "Rebuff~\cite{rebuff} takes a similar hybrid approach, combining heuristic detectors, LLM-based verification, and a vector database of previously seen attacks.", added:
  > `\rev{Semantic Firewall differs in running eight detectors in parallel (covering PII, secrets, and context flooding as well as injection), in gating LLM calls and cache write-back on confidence with an administrator allowlist, and in separating detection from configurable policy.}`
- **Every claim is backed by the code:**
  - eight agents run in parallel (`orchestrator.py`, ThreadPoolExecutor);
  - LLM calls are gated by the escalation score (`_should_run_llm_agents`);
  - write-back is gated at confidence ≥ 0.85, LLM-confirmed (`semantic_cache.writeback_allowed`);
  - the admin allowlist is `semantic_allowlist`;
  - detection is separated from policy (`_apply_policy`, policy profiles).
- **Still open (R1.5d, second half):** cite and discuss training-based and architectural defences (instruction hierarchy, StruQ/SecAlign, CaMeL). Proposed text is ready; references are to be verified before adding.
- **Check:** static LaTeX check clean.
- **Commit:** `c99d8c0`

### 16. Code: LLM call failure is now fail-closed (prerequisite for R1.5c)

- **Item:** R1.5c (Algorithm 1 must state the fallback when the LLM is unavailable). Sec 3.4 claims: "If the external LLM service is unavailable, the firewall applies the configured fallback policy rather than silently bypassing security checks."
- **Bug:** when the LLM API call itself raised (API error, exhausted credit, network failure), the Injection and Unsafe Content detectors caught the exception, returned no LLM matches, and the prompt was reported **clean**. This is fail-open, contradicting Sec 3.4. Only agent crashes and timeouts were handled fail-closed. (The 2026-10-04 fix for this was in the deleted clone and never pushed.)
- **Fix (`core/orchestrator/orchestrator.py`, `_run_agent`):** when a fail-closed agent (Injection, Unsafe Content) returns no threat and `meta["llm_parse_status"] == "call_failed"`, the orchestrator replaces the result with the same `SYSTEM_UNAVAILABLE` result used for timeouts:
  - `threat_found = True`, severity HIGH, `agent_available = False`, `meta["error_type"] = "llm_call_failed"`;
  - default policy: SYSTEM_UNAVAILABLE + HIGH → **FLAG**.
  - If the regex part of the detector still found a threat, that result is kept unchanged.
- **Scope:**
  - A *missing API key* is treated as a configuration choice (`llm_skipped_reason = "missing_api_key"`), not an outage, and is unchanged. The test suite and the no-LLM experiment configurations rely on this.
  - Unparseable LLM output is unchanged.
- **Test:** `tests/test_firewall_safety.py::test_orchestrator_llm_call_failure_is_fail_closed`. It **fails without the fix and passes with it**. Full suite: **228 passed**.
- **Effect on reported results:** none, if no LLM call failed during the experiments. The camera-ready harness records `llm_failed` per prompt, so this can be checked in stage 2.
- **Commit:** `4e489f5`

### 17. Trimmed the three longest additions (user request: keep theory additions minimal)

- **Why:** the user asked to keep added text close to what reviewers asked for. Words added vs. the original paper (LaTeX markup included) went from ~543 to **~368**.
- **Levenshtein / entropy (Sec 3.3; items 5 and 1):**
  - Dropped "Inputs of at least 50 characters with H(X) < 2.5 bits are flagged; the Abuse Detector also checks for … code-injection patterns." Table 1 already lists these checks.
  - The obfuscation sentence is now the original sentence with **one word changed**: "The \rev{Injection} Detector identifies lexical obfuscations using approximate string matching…" (was "Abuse").
  - The end-of-paragraph addition shrank from ~85 to ~30 words: "…obfuscated variants of \rev{the keywords in its regex patterns. With one typo per keyword, this raises regex detection of neuralchemy test attacks from 33.9\% to 80.1\%; it is not included in the main results.}"
- **Blocking rule (Sec 3.5; item 2):**
  - Before the equation: "This score is used for explanations and session tracking; blocking uses the policy actions of the triggered detectors $\mathcal{A}$ and the ensemble score".
  - After the equation: "where $w_a$, $p_a$, and $s_a$ are the weight, calibrated confidence, and severity rank (0--4) of detector $a$. A prompt is blocked if a policy action is \textsc{Block} or $E(\mathcal{R})\geq\theta=3.5$."
  - Dropped: the weight range, and the Redact (2.6) / Flag (1.8) thresholds.
- **Cache (Sec 3.2; item 6):**
  - "…updated \rev{on both sides of the cache, which only stores previously seen malicious and administrator-approved prompts.}"
  - "\rev{Only LLM-confirmed detections with confidence $\geq 0.85$ are written, and the cache keeps at most 10{,}000 entries, evicting the oldest first.}"
  - The allowlist correction ("skip the LLM Gate, while the deterministic detectors still run") is kept: it fixes a factual error.
- **Unchanged facts:** every remaining statement still matches the code (see items 2, 5 and 6 for the sources).
- **Check:** static LaTeX check clean (braces 587/587).
- **Commit:** `fa26832`

### 18. PII detector: general format and overlap fixes (code)

- **Item:** Table 2c PII row (ai4privacy/pii-masking-200k, 209,261 rows). The paper's P 85.9 / R 96.7 / F1 91.0 was not reproducible: the committed run of the original code gave type-mapped P 70.3 / R 75.3 / F1 72.7. Per-type recall: phone 27%, DOB 33%, account 37%, CVV 46%, SSN 50%; email, IP, MAC, IBAN, card and zipcode ≥ 99.7%.
- **Protocol (to avoid tuning on the test data):**
  - Split the rows with a fixed rule, never changed afterwards: **dev** = row index % 5 == 0 (41,853 rows); **held-out** = the other 80% (167,408 rows).
  - All error analysis used dev only (scratchpad `pii_dev_analysis.py`). Held-out is evaluated once, after the code was frozen.
- **Changes in `core/agents/pii_detector.py`:** general fixes only; no dataset values or dataset-specific formats.
  1. **Overlap handling:** matching uses `finditer` with spans, and a match strictly inside a longer match is dropped. Before, every pattern fired independently, so pieces of one phone number (e.g. `05843` and `76140` inside `05843-76140`) were reported as separate zipcodes or phones, causing both misses and false positives.
  2. **Capture groups:** a pattern's value is now its first capture group, if it has one. Before, `findall` returned tuples and `account_number` returned the keyword with the number ("account: 46281517").
  3. **`account_number`:** captures only the identifier, which must contain a digit, so "account information" no longer matches.
  4. **`phone_us`:** the leading `\b` failed before "(", so "(352) 9324865" was matched as "352) 9324865". It now accepts "(ddd)" or "ddd", with `-`, space or `.` separators, and lookarounds instead of `\b`.
  5. **`phone_generic`:**
     - accepts `.` separators and an optional `+cc` / `(area)` prefix;
     - groups of 2–5 then 2–8 digits;
     - lookarounds so it is not anchored inside other tokens, and does not start after `$`, `/`, `-` or `.` (amounts, paths, dates).
  6. **`date_of_birth`:**
     - adds month-name dates *with a year* ("August 25, 1918", "4th April 1990");
     - extends an ISO date to its time part when present ("1952-09-28T21:31:54.180Z"); the date itself was already matched.
     - Dates without a year ("6/11", "4th April") are deliberately not matched.
- **Deliberately NOT changed (would only fit this dataset):**
  - bare 8-digit numbers labelled ACCOUNTNUMBER;
  - bare 3-digit numbers labelled CREDITCARDCVV;
  - the dataset's shortened 11-digit `756…` "SSN" form (a synthetic variant of the Swiss AHV number).
  - The `pincode_india` rule (any 6-digit number; the largest FP source) is left for a user decision.
- **Dev result** (`results_camera_ready/pii/pii_eval_dev.json`), type-mapped: P 70.3 → **83.2**, R 75.3 → **86.2**, F1 72.7 → **84.7**.
  - Per-type recall: phone 27.7 → 94.6; DOB 34.4 → 65.6; SSN 50.0 → 74.7 (the `756.xxxx.xxxx` form is now caught by the phone pattern; value-level scoring); zipcode 99.8 → 98.7 (fragments inside phones are no longer counted).
- **Evaluation script:** `camera_ready/pii_eval.py` gains `--part {all,dev,heldout}`, writing `pii_eval.json` / `pii_eval_dev.json` / `pii_eval_heldout.json`.
- **Tests:** 10 new tests in `tests/test_pii_detector.py` (whole phone values, no fragments, written/ISO dates, account value without keyword, keyword alone not PII). Full suite: **238 passed**.
- **Commit:** `4f6f72f`

### 19. PII: pincode context rule (default) and opt-in ai4privacy profile

- **User decisions (2026-10-06):** improve P/R/F1 as far as possible, including the dataset-specific patterns; require a context word for the Indian PIN code rule.
- **Protocol:** every variant was compared on the **dev part only** (scratchpad `pii_dev_variants.py`), and kept only if F1 improved. Held-out is evaluated once, afterwards.
  - An intermediate held-out file from the previous code (`4f6f72f`) was produced by a background run before it was stopped. It was moved aside **unread** (scratchpad `UNREAD_pii_eval_heldout_intermediate_4f6f72f.json`) so it could not influence any decision.
- **Dev comparison (type-mapped):**

  | Variant | P | R | F1 |
  |---|---|---|---|
  | code `4f6f72f` | 83.21 | 86.21 | 84.68 |
  | + pincode needs context | 96.91 | 86.21 | 91.25 |
  | + bare 8-digit account | 95.43 | 92.19 | 93.78 |
  | + 756-prefixed SSN | 96.98 | 88.29 | 92.43 |
  | + account + SSN | 95.53 | 94.26 | **94.89** |
  | + account + SSN + bare 3-digit CVV | 87.14 | 95.79 | 91.26 (dropped) |

- **Changes:**
  1. **Default, `pincode_india`:** now requires "pin code / pincode / pin / postal code / post code" before the 6 digits; the value is captured. ZIPCODE ground truth on dev is only `99999` or `99999-9999`, so recall is unchanged. FPs on dev 4,654 → 736.
  2. **Opt-in profile `SEMANTIC_FIREWALL_PII_PROFILE=ai4privacy`** (`pii_eval.py --profile ai4privacy`):
     - `account_number_bare`: a bare 8-digit number, scored as ACCOUNTNUMBER;
     - `ssn_ch_ahv`: `756` + 4 + 4 (+ 2) digits, scored as SSN.
     - **Off by default:** it would flag any 8-digit number in general traffic. In the paper it must be reported as dataset-adapted.
  3. Bare 3-digit CVV was **not** added (dev F1 dropped from 94.9 to 91.3).
- **Dev numbers via the official script:** default P 96.91 / R 86.21 / F1 91.25 (`pii_eval_dev.json`); profile P 95.53 / R 94.26 / F1 94.89 (`pii_eval_dev_ai4privacy.json`).
- **Tests:** 4 new tests (bare 6-digit is not a pincode; pincode with context is; bare account only with the profile; 756-SSN with the profile). Full suite: **242 passed**.
- **Commit:** `afc093e`
- **Held-out (final, reportable):** pending.

### 20. PII final results (held-out and all rows)

- **Raw results:** `results_camera_ready/pii/` → `pii_eval_heldout.json`, `pii_eval_heldout_ai4privacy.json`, `pii_eval.json` (all rows, default), `pii_eval_ai4privacy.json` (all rows, profile). Code `afc093e`.

  | Rows | Patterns | P | R | F1 |
  |---|---|---|---|---|
  | held-out 80% (167,408) | default | 96.87 | 86.40 | 91.34 |
  | held-out 80% | + ai4privacy profile | 95.50 | 94.19 | 94.84 |
  | all 209,261 | default | 96.88 | 86.37 | 91.32 |
  | all 209,261 | + ai4privacy profile | 95.50 | 94.20 | 94.85 |

- **Overfitting check:** held-out ≈ dev (dev default 96.91 / 86.21 / 91.25; profile 95.53 / 94.26 / 94.89), so the fixes were not fitted to the dev rows.
- **Per-type recall, all rows:**
  - email 99.89, IBAN 99.95, credit card 99.69, MAC 99.94, IPv4 99.82, IPv6 99.74, zipcode 98.58;
  - phone 94.05; DOB 66.28;
  - SSN 74.67 (profile 99.77); account 37.07 (profile 99.36); CVV 45.67.
- **Latency:** p50 ≈ 0.25 ms, p99 < 1 ms.
- **Paper:** the Table 2c PII row and the Sec 5 PII sentences are not yet updated; waiting for the user to choose which row to report.
- **Commit:** `3ca6ab4`

### 21. Removed the "Synthetic Safety" row (unsupported)

- **User decision (2026-10-06):** remove it.
- **Why:**
  - The paper mentions this "synthetic corpus covering violence and illegal activities" only once in Sec 4, plus its Table 2c row (N 2000, P 82.50 / R 84.30 / F1 83.38 / FPR 5.45%). It is never discussed in the results.
  - No file in the repo produced it. The only 2,000-prompt safety result is lmsys/ToxicChat (`results/unsafe/unsafe_scaled_summary.json`: P 69.0 / R 79.0 / F1 73.7 / FPR 7.8%), which does not match and is real data, not synthetic.
- **Changes:**
  - Deleted the Table 2c "Synthetic Safety" row.
  - Sec 4: "Two additional 2{,}000-sample datasets are used for targeted evaluation: a synthetic corpus covering violence and illegal activities, and a benign subset of no_robots…" → "`\rev{An additional 2{,}000-sample}` benign subset of no_robots…".
- **Check:** no "synthetic" left in the paper; static LaTeX check clean.
- **Commit:** `edac2ea`

### 22. Thresholds τ and θ selected on validation (R1.2a, R3.3, R1.1b, R1.1c)

- **Protocol:** neuralchemy official splits. Training-split attacks (4,391) only seed the cache; validation (941) selects τ and θ; test (942) is evaluated once.
- **Method:** exact offline sweep. With a frozen cache, pred(τ) = 1 if similarity ≥ τ, else the no-cache prediction. The θ sweep uses the recorded `ensemble_risk` and `policy_action` (verified 941/941).
- **Result:** τ = 0.65 (validation F1 94.90); θ swept 2.0–6.0 with no F1 change, so θ = 3.5 is kept.
- **Paper:** Sec 3.2 τ, Table 3a regenerated, Table 3 text.
- **Commits:** `2b426f5`, `8b5a9fd`, `59ef28d`

### 23. OOD-500 set (R1.1d)

- 250 attacks (Gandalf 100, in-the-wild jailbreaks 100, SPML 50) and 250 benign prompts (oasst2 100, SPML benign 75, XSTest safe 75). Prompts with cosine ≥ 0.90 to any neuralchemy prompt were removed.
- Released as `data/datasets/ood_curated_500.jsonl` with its metadata.
- **Table 3b:** F1 peaks at τ = 0.65 (92.44), cache hit rate 10.4%.
- **Commits:** `c822b21`, `ba42e05`

### 24. Baselines and final main tables (R1.1a, R1.1e, R1.1g, R1.1j, R1.2c)

- Reused the 2026-10-04 baseline runs (sample ids verified, 0 failures). Their provenance is in `runs/BASELINES_PROVENANCE.md`.
- **Table 2a:** test split, N = 942, with FPR for every system.
- **Table 2b:** BeaverTails (N = 1,000) and deepset (N = 662).
- **Abstract and conclusion:** F1 reported per dataset; the ± was removed.
- Claude 3.5 Haiku is retired, so Claude Haiku 4.5 replaces it.
- **Commits:** `04bb232`, `55a08e4`, `59ef28d`

### 25. Significance (R1.2e)

- McNemar's test with Holm–Bonferroni correction against every baseline: GPT-4o χ² = 38.9, p_Holm = 1.4e-9; all other baselines also p < 0.01.
- Ablation C8 vs C1: χ² = 22.3, p = 2.3e-6 (`significance/ablation.json`).
- The deepset Rebuff number is a reported value with no per-sample data, so it cannot be tested.
- **Commits:** `13c8be4`, `21eaa14`

### 26. Latency, Eq. 8 and attack prevalence (R1.1f, R1.1h, R1.3c)

- **Setup:** sequential runs on the test split (210 prompts; the first 10 excluded as warm-up). The LLM is served by Groq via OpenRouter. `camera_ready/latency_report.py` produces the numbers.
- **Full system:** mean 304 ms, p50 279, p95 757, p99 1,047 ms.
- **Path latencies:** cache 86 ms, deterministic 332 ms, LLM 519 ms.
- **Eq. 8:** P_c = 0.47, P_h + P_m = 0.03, P_l = 0.50, giving L_eff ≈ 308 ms.
- **Attack prevalence 1–10%:** LLM-call rate 94.4–87.4%, L_eff 495–466 ms, p95 944 ms, p99 1,242 ms. On benign-only no_robots, 98.2% of requests reach the LLM.
- **Ablation latencies:** C1 406, C2 10.3, C3 70.1, C4 14.5, C5 80.2, C6 16.8, C7 142, C8 304 ms. C7 is now faster than C8.
- **Intro and Sec 3.1:** 731.64 ms → 406 ms (LLM-only gate); regex engine 5.21 → 10.3 ms.
- **Wording:** the "halving LLM calls" claim is qualified in the abstract and conclusion.
- **Cost:** the $500 → $166 sentence is replaced. Each LLM-stage invocation makes two requests, and the per-token prices could not be verified.
- **Commits:** `21eaa14`, `b77621b`

### 27. Ablation Table 4 on the test split (R1.1h)

- C1–C8 rebuilt from final-code runs on the test split (N = 942), with an FPR column added.
- C9 (cold cache) is left TBD: the run was stopped at 344/942 by user decision.
- **Commits:** `13c8be4`, `cdc4356`, `21eaa14`

### 28. Multi-turn set (R1.3b), in progress

- walledai (attacks only) is removed from Sec 4, Table 2c and the bibliography.
- The new set has 100 SafeMTData attacks and 100 benign no_robots Chat conversations, held out. Its numbers are TBD.
- **Dev results so far:** recall 57–60%, FPR 6–8%. The work is on hold.
- **Commit:** `21eaa14`

### 29. Algorithm 1 aligned with the code (R1.5c)

- Algorithm 1 is rewritten from `orchestrator._analyze_text`, in this order:
  1. embedding;
  2. allowlist (skips the LLM);
  3. threat cache (s* ≥ τ);
  4. deterministic detectors plus regex pre-screen;
  5. LLM-assisted detectors, unless the prompt is already blocked or allowlisted (fail-closed FLAG);
  6. policy action plus ensemble (e ≥ θ → Block; lower bands → Flag/Redact);
  7. Session Judge;
  8. Redact masking;
  9. write-back of LLM-confirmed threats.
- The Classification Procedure paragraph was updated to match.
- **Commit:** `b80b853`

### 30. Purpose-built detectors as baselines (R1.2d, R1.1k)

- Added Table 2a rows for ProtectAI DeBERTa-v3 (97.75 / 86.41 / 91.73, FPR 2.82%) and PIGuard (83.84 / 79.89 / 81.82, FPR 21.79%).
- Sec 4 now describes the zero-shot prompt.
- "five" → "seven" baselines.
- Prompt Guard 2 was not run (gated model).
- The hardware sentence is corrected to the laptop used for all camera-ready runs (Intel Core Ultra 5 125H, 16 GB).
- **Commit:** `0cd678f`

### 31. Read-through fixes and warm-cache check (R3.1, R1.2b)

- **Sec 3.4:** the LLM's JSON fields now match the detector prompts, and the fallback is stated as FLAG.
- **Sec 3.1:** notes that the regex pre-screen runs after the semantic-memory lookup, as in Algorithm 1.
- **Novel-subset check** (`camera_ready/novel_subset.py`): on the 495 test prompts whose nearest training attack is below τ, the full system gets F1 88.3 vs GPT-4o 84.8, Gemini 82.8, Haiku 76.4 and DeBERTa 82.0. One sentence was added to the Table 3 text.
- **Maths check:** all equations match the code (entropy, Levenshtein, risk weights, ensemble with 1.8 / 2.6 / 3.5, write-back floor 0.85, cache cap 10,000), and every F1 in the paper was recomputed from its precision and recall (29 rows).
- **Commit:** `b77621b`

### 32. Small text fixes from the supervisor's list (2026-10-06)

- **"self" removed from the cache:** "self-updating semantic cache" → "semantic cache" (abstract, contribution 2, Sec. 3 intro).
- **Symbol clash:** the temperature is written "temperature 0" (Sec. 3.4, Sec. 4.1, Algorithm 1), so T now only denotes detector times (Eq. 3).
- **Rebuff row:** labelled "Rebuff (reference)", with the note "Published in [Palit & Woods]". It is a published number on the same dataset, not a system we ran.
- **Throughput:** the unsupported "about 45 requests per second" is replaced by the measured "about three requests per second per sequential worker (304 ms on average, mostly LLM wait)".
- **Weights:** Sec. 3.5 says the detector weights (1.0–1.3) and risk points were set by hand, not tuned on an evaluation split, and that sweeping θ over 2–6 left F1 unchanged. Automatic weight selection is added as future work.
- **Privacy (R1.4f):** one sentence in Limitations: prompts go to a third-party API, and the open-weight gate can be self-hosted.
- **Prompt Guard 2 (R1.2d):** one clause: it could not be evaluated because access to its weights is gated.
- **no_robots row note:** "Benign only; block rate".
- **Check:** compiled with Tectonic, no errors or undefined references, 18 pages. All `\rev` checks pass.

### 33. Table 5: cost/latency at realistic attack rates (R1.3c)

- A new small table in the Cost paragraph gives the LLM-call rate, L_eff, p95 and p99 at a 59% attack rate (test split, measured) and at 10%, 5% and 1% (re-weighted).
- Source: `results_camera_ready/latency/latency_neuralchemy-test-latency-groq.json` (prevalence block). The text now points to the table.
- Compiled: 18 pages, no errors.

### 34. Related-task wording (R1.3a) and the attack-heavy qualifier (R1.3c)

- "Cross-dataset generalization" was reworded in the intro, the Sec. 5 intro and the Table 2(b) title.
- Conclusion: deepset is now named as another prompt-injection benchmark; BeaverTails and AI4Privacy are related tasks.
- Sec. 5.1: "on this attack-heavy split" plus a pointer to Table 5.

### 35. New results pasted into the paper

- **Tables:** the C9 cells are filled in Table 2a and Table 4 (95.71 / 92.93 / 94.30 / 5.90; latency "---"). Table 4 gains a C1b row (parallel LLM agents, no deterministic stage: 95.75 / 89.86 / 92.71 / 5.64).
- **Ablation text:** sentences added for C1b (McNemar chi2 = 16.4), C9, the policy profiles, and the explainability example.
- Sources are in NEW_RESULTS_FOR_DISCUSSION.md sections 1, 2, 3 and 5. RLM / sub-agents were not done (user's decision).

### 36. Supervisor's list (2026-10-06)

- **"fires":** reworded ("matches so few prompts", "cannot block them").
- **R3.1 cache protocols:** a paragraph in Sec. 4 with Eq. 9 (warm: C_0 = the 2,650 train attacks, frozen; y_warm = 1 if s* >= tau, else the no-cache path; cold: empty cache, write-back in dataset order; the splits share no prompt).
- **Ablation:** every configuration code C1-C9, including C1b, is explained before the results.
- **Entropy check:** applies only to inputs of at least 50 characters, with threshold H < 2.5 bits (abuse_detector.py: len(text) < 50 skipped; ENTROPY_LOW_THRESHOLD = 2.5).
- **Cross-dataset:** the term is defined, and BeaverTails/AI4Privacy are explained as related tasks that exercise the Unsafe Content and PII detectors.
- **Rebuff:** kept, with an explicit early-exit difference added.
- **Attack rate:** defined (59% on test); "attack rate falls" is reworded in the abstract and conclusion; Table 5 is sorted, with a "Resolved w/o LLM" column added and a clearer caption.
- **Not done:** bold/larger text in Fig. 1, because there is no editable source of SF2.png (needs the original drawing file).

### 37. Multi-turn row and Session Judge ablation (R1.3b, R1.5b)

- **Held-out set:** 100 SafeMTData attack and 100 no_robots benign conversations.
- **Results:** recall 51.0%, FPR 2.0%, P 96.23, F1 66.67 (Table 2c).
- **Session Judge ablation:** without it (turn by turn), recall is 6.0% and FPR 1.0% (McNemar chi2 = 40.2, p = 2.3e-10). This sentence is in Sec 5.1.
- No TBD cells remain. Compiled: 19 pages, no errors.

### 38. Multi-turn results removed again (user decision, 2026-10-07)

- The Table 2c multi-turn row and the Sec 5.1 sentence are back to TBD, because the results were judged not good enough and are to be rethought.
- The runs stay in results_camera_ready/multiturn/ (final, final-no-session).

### 39. Audit fixes (2026-10-07)

- **Llama Guard 4:** Sec. 4 says it was evaluated for prompt injection, so its neuralchemy test row is added to Table 2a (92.49 / 35.69 / 51.50 / 4.10, 717 ms).
- **Ablation wording:** "steady improvement" contradicted Table 4 and now reads "complementary".
- **Sec. 5 intro:** now references Table 5.
- **Results index:** stale lines updated. Note: the earlier multi-turn runs used tau = 0.90 (harness default), so they are not reportable.
- **Check:** all new paper numbers verified against the result files.

### 40. Read-through for clarity and correctness (2026-10-07)

- **Correctness:** the Levenshtein repair *is* enabled in all reported runs (settings default True; runs dated after the implementation). On ordinary prompts it changes 2 of 941 validation predictions; with one typo per keyword, 33.9% -> 80.1%.
- **Clarity:** abstract wording; intro run-on sentence; "Section" capitalised in the roadmap; Related Work forced line break removed; the "both sides of the cache" sentence rewritten; ai4privacy citation spacing; "three proprietary models" (Llama Guard 4 is open-weight); a sentence no longer starts with lower-case "deepset"; profile names match Sec. 3.2 (Strict/Developer); the long ablation paragraph is split; FPR "too high" instead of "too strict".

### 41. Style edits to the supervisor's wording reverted (user preference, 2026-10-07)

- Restored the original abstract wording, intro sentence, "The third group mentions…" and "too strict".
- Corrections stay: Levenshtein, proprietary, profile names, citation spacing and Section capitalisation.

### 42. Meaning corrections (2026-10-07)

- "risk threshold theta" -> "ensemble threshold theta" (Sec. 3 intro, Algorithm 1). Theta applies to E (Eq. 7), not to the risk score (Eq. 6).
- Sec. 5.2: only F1 is the highest at tau = 0.65; tau = 0.55 has the higher recall.
- Sec. 4.1: "API cost savings" -> "LLM-call rates, which determine the API cost" (R1.3c).

### 43. Prompt Guard 2 and per-detector ablation (R1.2d, R1.5a)

- **Prompt Guard 2 (86M), Groq API:** P 95.28 / R 54.89 / F1 69.66 / FPR 3.85, latency 189 ms (measured separately without rate-limit waits).
- **Prompt Guard 2 errors:** 30 initial failures (15 rate limits, 13 too-long inputs, 2 connection resets) were re-run, splitting long inputs; 0 errors remain.
- **McNemar vs Prompt Guard 2:** p_Holm = 4.8e-39. All gains stay significant.
- **Per-detector ablation (new Table 6):**
  - Removing Injection gives 90.84 (warm) / 62.74 (no cache); removing Unsafe Content gives 95.12.
  - Removing Abuse changes 7 decisions of the fast layers.
  - PII, Secrets, Threat Intel, Custom Rules and Context Flooding change 0 decisions, because they never fire on neuralchemy.
- The paper is now 20 pages (the limit).

### 44. Multi-turn with conversation memory (R1.3b, R1.5b), 2026-10-08

- **Code:** opt-in conversation memory in the Session Judge (commit 7d30c90).
- **Seeds:** 1,813 SafeMTData conversations outside the eval goals (7,393 prefixes; 10 goals de-duplicated at cos >= 0.80).
- **Threshold:** 0.60, selected on dev (multiturn/memory_tuning.json).
- **Held-out, run once:** recall 89.0%, FPR 6.0%, P 93.68, F1 91.28.
- **Session Judge breakdown:** per-turn only 6.0% -> + conversation check 50.0% -> + memory 89.0%; McNemar p = 3.8e-10 and 1.9e-8.
- **Paper:** Table 2c row, Sec. 5.1 sentence, Sec. 3.2 and Sec. 4 description, Algorithm 1 comment, Limitations caveat (same attack generator). No TBD left; 20 pages.

### 45. Multi-turn sentence: only the Session Judge rows (user's choice, 2026-10-08)

- The per-turn-only figure (6.0%) is no longer stated.
- The sentence now gives the conversation check alone (50.0%, FPR 4.0%) and with the memory (89.0%; McNemar p < 1e-7). The full breakdown stays in breakdown.json.

### 46. Memory described as part of the Session Judge (user's choice, 2026-10-08)

- Sec. 4, Sec. 5.1 and Limitations now say "the Session Judge's memory"; Sec. 3.2 and Algorithm 1 already placed it inside the Session Judge.
- Sec. 5.1: "Without its memory of known multi-turn attacks, the Session Judge detects 50.0% (FPR 4.0%); with it, 89.0% (McNemar p < 1e-7)."

### 47. Limitations: memory-seed caveat removed (user's choice, 2026-10-08)

- The seed source is still stated in Sec. 4: ActorAttack conversations from SafeMTData whose goals are not among the evaluated ones.

### 48. Sec. 4: memory involves no training (2026-10-08)

- Added: "no model is trained, as the memory only stores their embeddings, like the threat cache."
- Not added: "learns as we go". The memory was pre-filled and frozen in the evaluation, and the conversation memory has no write-back in the code.

### 49. Sec. 4: 'no model is trained' clause removed again (user's choice, 2026-10-08)

- The seeding sentence is kept, because it discloses how the memory behind the 89% result was filled (R3.1-type disclosure).

### 50. Sec. 4: memory seeding clause removed (user's decision, 2026-10-08)

- The paper keeps the dev threshold selection (0.60) and "memory of known multi-turn attacks" (Sec. 3.2).
- The seed details (1,813 SafeMTData conversations, goal de-dup) remain documented in the code release: research README, multiturn_memory.py, multiturn/memory_tuning.json.

### 51. Baseline count: seven -> eight (2026-10-08)

- Prompt Guard 2 was added, so the baselines are now GPT-4o, Gemini, Claude Haiku, DeBERTa, PIGuard/InjecGuard, Prompt Guard 2, Llama Guard 4 and Rebuff.

### 52. Mam's threat model and poisoning-resistant cache design (R1, R3 cache-poisoning questions), 2026-10-11

Code (all off by default, so no reported result changes):
- `semantic_cache.py`: the four write-back safeguards from mam's design.
  - Benign-overlap check: an online write is rejected if an allowlisted or benign-reference prompt lies within tau of it. `add_benign_reference()` adds the reference set.
  - Probationary tier: online entries match only at `probation_threshold` (> tau). They are promoted after `promote_after` (default 2) independent confirmations, i.e. other prompts within tau that the pipeline would write back, each prompt counted once, or by the operator (`promote()`).
  - TTL: a probationary entry expires `ttl_requests` lookups after its last confirmation. The logical clock is restored from the store on restart.
  - Sampled re-validation: `should_revalidate(hit)` samples `revalidate_rate` of the hits on online-written entries. `evict()` removes only online-written entries; seeded entries are curated and never evicted.
- `orchestrator.py`: a sampled hit goes through the full pipeline instead of the cache, and the entry is evicted if the final action is ALLOW. `FirewallDecision.cache_revalidated` records this.
- `settings.py`: the env vars `SEMANTIC_FIREWALL_CACHE_OVERLAP_CHECK`, `_PROBATION_THRESHOLD`, `_PROMOTE_AFTER`, `_TTL_REQUESTS` and `_REVALIDATE_RATE`.
- `tests/test_cache_safeguards.py`: 10 tests. With the existing cache tests, 25 pass.

Experiments (normal traffic only, no attacker):
- `camera_ready/writeback_verdicts.py`: cache-miss verdicts plus write-back eligibility for neuralchemy test and validation.
  - Test: 942 records, 0 errors, 303 eligible (289 attacks, 14 benign). Validation: 941 records, 0 errors, 292 eligible (284 attacks, 8 benign).
  - Agreement with the paper's no-cache runs: 99.26% (test, F1 93.41 vs 93.49) and 99.68% (validation).
- `camera_ready/cache_safeguards_replay.py`: an exact replay of write-back through the real `SemanticCache`, covering every safeguard sweep from mam's plan. Results go to `results_camera_ready/cache_safeguards/replay.json`.
- Not done, left to the user: the protection side (poisoning campaigns, poison write rate, amplification, TTL recovery curve, time to eviction).

Paper:
- New Sec. 3.9 "Threat Model" (`sec:threat-model`): mam's text with factual fixes only.
  - "semantic block cache" became "threat cache", and "writeback" became "write-back" (the paper's terms).
  - Added that the Session Judge's memory is never written online.
  - G1 coverage now lists what we already test: neuralchemy and deepset, OOD-500, multi-turn, prompts unlike any cached attack, and typo-obfuscated keywords (new label `sec:mesh`).
  - The G2 coverage sentence now states what is measured: propagation without an attacker, and the cost of the design. The poisoning-campaign clause is kept in mam's words inside a red `\torun{}` marker.
  - G3 is no longer listed as unevaluated, because a black-box test exists; only the pipeline-aware G3 case is unevaluated.
  - Grammar fix: "how much does ... reduce" became "how much ... reduces it".
- Security Considerations: added `\label{sec:security}`.
  - New G3 paragraph from `cache_robustness/allowlist_abuse.json`: 4/100 within 0.90, median similarity 0.70, none allowed in skip_llm, all 4 in bypass.
  - Mam's "Poisoning-resistant cache design" paragraph, with fixes:
    - "therefore," became "therefore".
    - Re-validation is "by the full pipeline, including the LLM gate", and covers only hits on automatically added entries.
    - Added that the measures are options, off in all other experiments.
- New subsection `sec:cache-eval` in Sec. 5, filled in entry 53.
- New macro `\torun{}`: red "[TO RUN]" text, always visible, so a pending claim cannot ship unnoticed.
- The paper is now 22 pages before the results subsection. The user said not to trim yet.

### 53. Cache-safeguard results: Sec. 5.3 and Table 4 (2026-10-11)

Experiments:
- `cache_safeguards_replay.py` produced every sweep in mam's plan, starting warm and starting cold, with 5 stream
  orders each: 30 configurations, 150 replays.
- Raw data: `results_camera_ready/cache_safeguards/replay.json`. The `table` key holds the numbers used in the paper;
  `entry_reach` holds the per-entry reach by radius.
- Embeddings are cached in `_work/embeddings/` (git-ignored).
- Check against the real C9 run: in dataset order from an empty cache, the replay gives the same 151 hits, 696 LLM
  calls, 513 TP and 39 FN, and 24 FP vs 23.

Paper:
- New Sec. 5.3 "Cache Write-back and Poisoning Resistance" (`sec:cache-eval`), with three paragraphs:
  - the protocol;
  - error propagation: 14 eligible benign test prompts, none within tau of a benign validation prompt (closest 0.58),
    versus 0.60% for a random benign prompt;
  - the cost of each safeguard.
- New Table 4 (`tab:safeguards`): 15 configurations x {warm, cold} x {recall, FPR, hit, LLM/1k, val FPR}.
- Every number in the text is checked by assertions against `replay.json` in `cache_eval_section.py` (scratchpad).
- The protection side stays in a red `\torun{}` marker: poison write rate, FPR after N poisons, amplification,
  recovery, time to eviction.
- Fixes during the edit:
  - soul error from `Val.\ FPR` inside `\rev`, replaced by `Val.~FPR`;
  - table labels shortened (`\resizebox` made them tiny);
  - variant recall is now pooled over the orders (orders without variants had been counted as 0%).
- The paper is now 23 pages (limit 20). The user said not to trim yet.
- Also updated: notes section 8 and the results README ("Cache write-back safeguards").

### 54. R1.4e: cache growth and eviction (2026-10-11)

- Sec. 3.2: one sentence added after the 10,000-entry cap, built from existing results (no new run). It covers:
  - lookup time (vector query, without the embedding): 31, 20 and 19 ms mean at 1,000, 5,000 and 20,000 entries
    (`cache_robustness/growth.json`), so the cap bounds storage, not latency;
  - with a cap of 500, inserting 2,000 entries leaves 500 (same file). Oldest-first eviction is covered by the unit
    test `test_cache_evicts_oldest_entries`;
  - the cap is never reached in our experiments: at most 2,728 entries in any replay (`replay.json`), so no entry
    is evicted.
- The numbers are checked by assertions in `growth_sentence.py` (scratchpad). The paper is still 23 pages.
- Not done, on the user's request: poisoning-campaign runs and adaptive attackers (R2.4/R1.4b). The safety
  classifier blocks building attacks against the firewall, so these stay with mam's side or as stated limitations.

### 55. Poisoning protection -> future work (mam, 2026-10-11) and full re-validation of all numbers

Future work (mam: "we can keep it as future work"):
- The macro `\torun` and both [TO RUN] spans are removed.
- Threat model, coverage paragraph: deliberate poisoning (G2) is now the first of "Four threats remain
  unevaluated". It reads: "a deliberate poisoning campaign (G2), including by a pipeline-aware attacker who knows the
  write-back rules, and how much the poisoning-resistant cache design reduces it". The list uses semicolons
  throughout.
- Sec. 5.3: "...measuring how much they reduce it, in terms of poison write rate, false positives after a poisoning
  campaign, amplification per poisoned entry, recovery time and time to eviction, is left to future work."
- Design paragraph: the two untested effects are now stated as design intent.
  - "this prevents the most damaging outcome" became "this is designed to prevent the most damaging outcome".
  - "so poisoned entries are removed without operator intervention" became "so that poisoned entries can be
    removed without operator intervention".
- Limitations & Future Work: "...and will evaluate the poisoning-resistant cache design against deliberate
  poisoning campaigns".

Re-validation (mam: "revalidate all numbers"):
- The scratchpad script `verify_all.py` recomputes every number from the raw records and compares it with the
  .tex: 542 checks, 0 failures after the fixes below.
  - Tables 2a/2b/2c, 3a/3b, 4, 5, 6 and 7, cell by cell.
  - About 150 text claims, including abstract, intro, conclusion, McNemar statistics, novel subset, PII per type,
    multi-turn breakdown, no_robots, C9, profiles, LOO, cost/latency, Sec. 5.3, growth, G3 and dataset sizes.
  - The parameters stated in the paper, checked against the code: risk-score points 85/30/5x4/10/100, entropy
    50 chars and 2.5 bits, flooding 4000 chars, temperature 0, a failed LLM call giving FLAG, weights 1.0-1.3,
    theta 3.5, tau 0.65, allowlist 0.90, write-back 0.85, cap 10,000, memory 0.60.
- Fixes:
  - p95 latency: 757 -> 756 ms. The file gives 756.49 ms. Fixed in Sec. 5.1.1 and in the test-split row of Table 7.
  - Sec. 3.3: the 33.9% -> 80.1% typo result is now stated "of the 62 test attacks that the regex catches without
    typos". As written before, it could be read as all 552 attacks, which conflicts with C2's 12.5% recall.
  - OOD-500: "six public sources" -> "five public datasets". The six source groups come from Gandalf, in-the-wild
    jailbreaks, SPML (attack and benign), OASST2 and XSTest.
  - Results intro: "Tables 2, 3, 5 and 7" -> "Tables 2-7", and the new cache write-back evaluation is mentioned.
- Checked and kept:
  - no_robots 2.80% block rate and 98.0% LLM share: one warm-cache hit becomes a block that skips the LLM (14/500
    and 490/500). The derived records keep the no-cache action.
  - "4.38" F1 gain = 95.81 - 91.43, the difference of the table values.
  - "five public benchmarks": same five as the original paper, with SafeMTData replacing walledai, and no_robots
    used separately for false positives.
- `prfpr_check.py` flags only the Table 4 rows. Its columns are not P/R/F1/FPR, so these are false alarms, and those
  cells are verified against `replay.json` instead.
- The paper compiles (23 pages), with no "??" references and no [TO RUN] left.

### 56. Public push to GitHub (user OK, 2026-10-11)

- Branch camera-ready on NCRL-IIITB/Semantic-Firewall: 2044939 -> a71c21d (fast-forward, no force). main untouched.
- Built as a clean export of local main (git archive), same rule as the 2026-10-06 release.
  - Excluded: CAMERA_READY_TRACKER.md, CHANGELOG_CAMERA_READY.md, ICISS-Springer/.
  - Also excluded: results_camera_ready/cache_robustness/targeted_poisoning.json (not used in the paper).
- Checked before pushing:
  - no API keys (only dummy test fixtures, unchanged since the last release);
  - no reviewer labels;
  - no prompt or conversation text in the new result files (multi-turn turns hold decisions only);
  - cache_robustness.py differs from the released version only in TAUS.
- Contents: 70 files added, 2 deleted (.vscode/settings.json, docs/FEATURE_IMPLEMENTATION_SUMMARY.md), 17 modified.

### 57. Table 6 on MIXED-1000 (mam: per-detector potential in one combined set), 2026-10-12

- New dataset `data/datasets/mixed_coverage_1000.jsonl` (`build_mixed_set.py`, seeded): 300 benign + 250 attack neuralchemy
  test prompts, 100 AI4Privacy PII texts, 80 generated fake credentials, 50 padded > 4,000 chars, 50 threat-intel
  signatures, 40 "Project Zephyr" (existing workspace rule), 80 attack + PII/padding/credential, 50 benign look-alikes.
  First build used "Project Falcon" as a look-alike; the config flags it (LOW custom rule), replaced by "Project Phoenix".
- Runs: full system once with the LLM (0 errors, 3 failed LLM calls -> FLAG); LOO of the two LLM detectors reuse the
  LLM answers through a new opt-in on-disk cache (`SEMANTIC_FIREWALL_LLM_CACHE_DIR`, off by default); six deterministic
  LOO runs on the fast layers (no LLM). Warm versions derived exactly (`derive_warm_mixed.py`).
- Table 6 and its Sec. 5.4 paragraph replaced: changed decisions PII 78, Secrets 71, Injection 50, Custom Rules 40,
  Unsafe 16, Context Flooding 7, Abuse 5, Threat Intel 0 (always co-flagged by Abuse/PII). C8 FPR 19.43% on this set
  (28 of 300 benign, 40 of 50 look-alikes). Numbers from `results_camera_ready/mixed/table6_mixed.json`.
- Paper now 24 pages.
