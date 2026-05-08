# The Kronaxis Research Stack

> **Synthetic populations that behave like real people, end-to-end open source.** Four projects, one coherent stack: psychographic framework → simulation engine → public falsifiable forecast → cost-efficient inference at scale.

This repository is a single-page index. Each section explains one of the four projects, why it exists, and how it fits into the loop.

---

## The unified story

Most "AI persona" projects stop at "we generated some characters". The Kronaxis stack closes the loop:

1. **Define the framework** (DYNAMICS-8): an eight-dimension psychographic model with two new digital-age dimensions (Acuity, Impulsivity) the existing Big Five and HEXACO frameworks don't capture.
2. **Build the engine** (Panel Studio): self-hosted simulator that takes a stimulus (product, ad, policy) and runs it past 500–65,000 DYNAMICS-tagged personas in 30 seconds, producing demographically segmented sentiment data.
3. **Test it against ground truth** (KPM-1): apply the engine to UK local elections, hash-commit the predictions before voting opens, then check after results land. Every claim is falsifiable.
4. **Make it economical** (Kronaxis Router): an OpenAI-compatible LLM proxy that cost-routes across local + cloud models, compresses prompts with RAG before they hit any backend, and -- uniquely -- wraps the actual `claude` CLI as an OpenAI endpoint. Without this layer, running 65,000 personas through frontier APIs is uneconomical; with it, 80% of requests land on a local 7B and the remaining 20% go to the right-tier model.

Each piece works alone. Together they cover what a synthetic-population research programme actually needs to be defensible.

---

## 1. [DYNAMICS-8](https://github.com/Kronaxis/dynamics-8)

**The framework.** Eight dimensions, each a continuous 0.0–1.0 score with four facets, giving 32 behavioural parameters per persona. Six map cleanly to Big Five / HEXACO so it interoperates with established literature; two are new and load-bearing for digital-age behaviour:

- **A — Acuity**: platform fluency, digital nativeness
- **I — Impulsivity**: delay discounting, reward sensitivity

Spec is CC BY 4.0; reference implementations (Python and Go) are BSL 1.1.

| Use it for | Don't use it for |
|---|---|
| Behavioural simulation in code | Self-help personality tests |
| Population segmentation in synthetic panels | Replacing clinical psychometric tools |
| Building characters for narrative AI | Real-person profiling without consent |

→ [github.com/Kronaxis/dynamics-8](https://github.com/Kronaxis/dynamics-8)

---

## 2. [Panel Studio](https://github.com/Kronaxis/kronaxis-panel-studio)

**The engine.** Self-hosted simulator. 1,000 personas in 30 seconds, fully local on a single GPU. Each persona has a unique DYNAMICS-8 score, a coherent life history, and census-weighted demographics for one of 20 supported countries. Submit a stimulus, get back demographically segmented sentiment data.

- 500 pre-loaded UK personas; `docker-compose up` and you're running
- Conjoint analysis, longitudinal stimuli, focus-group synthesis built in
- Exports JSONL / Parquet / CSV for downstream ML training
- Source available under BSL 1.1 (free for internal non-commercial use)

| Use it for | Don't use it for |
|---|---|
| Concept screening, ad pre-testing, conjoint | Regulator-defensible studies that demand human panels |
| Iteration speed (minutes vs weeks) | Nuanced qualitative research where one-of-a-kind insight matters |
| Sensitive stimuli that you don't want leaving your machine | Ground-truth surveys for policy decisions |

→ [github.com/Kronaxis/kronaxis-panel-studio](https://github.com/Kronaxis/kronaxis-panel-studio)

---

## 3. [KPM-1](https://github.com/Kronaxis/kpm1-election-projections)

**The public proof.** A production application of the stack to the 7 May 2026 UK local council elections. The repository contains the predictions JSON, per-council CSV, methodology snapshot, and the SHA-256 hash committed to GitHub *before* voting opened on 1 May 2026. After results land, anyone can re-hash the predictions and confirm nothing was retroactively adjusted.

The model itself, the 65,000-persona UK dataset, and the calibration pipeline stay proprietary. The predictions and hash are public. That's the contract: falsifiability without giving away the production artefacts.

| Why this matters |
|---|
| Most political-prediction models can be quietly reframed in their post-mortem |
| With a hash committed before voting, the predictions are the predictions |
| Known biases are listed publicly *before* results, so they can't be retro-explained |

→ [github.com/Kronaxis/kpm1-election-projections](https://github.com/Kronaxis/kpm1-election-projections)
→ Live results browser: [kronaxis.co.uk/election-results](https://kronaxis.co.uk/election-results)

---

## 4. [Kronaxis Router](https://github.com/Kronaxis/kronaxis-router)

**The infrastructure.** OpenAI-compatible LLM proxy in Go. Routes requests to the cheapest model that can do the job, caches deterministic responses, batches bulk to async APIs (50% off), compresses prompts with pgvector RAG before they hit any backend, and wraps the actual `claude` CLI in headless mode so Claude Code's full agentic loop is available behind `/v1/chat/completions`.

- 22,770 req/s, 5 ms p50, 9.9 MB binary
- Cost routing across vLLM / Gemini / OpenAI / Ollama / Anthropic
- Multi-account auth pool with provider-aware cooldowns
- Apache 2.0; commercial use unrestricted

This is what makes the rest of the stack viable economically. Running 65,000-persona panels through frontier-API pricing is uneconomical; small open-weight models handle 80% of those requests identically and 50× cheaper, but only if something can route the request to the right tier in <5 ms.

→ [github.com/Kronaxis/kronaxis-router](https://github.com/Kronaxis/kronaxis-router)

---

## Licenses

| Project | Spec | Code |
|---|---|---|
| DYNAMICS-8 | CC BY 4.0 | BSL 1.1 (converts to Apache 2.0 after 5 years) |
| Panel Studio | n/a | BSL 1.1 (converts to Apache 2.0 after 5 years) |
| KPM-1 predictions | (predictions are public for verification) | Model proprietary |
| Kronaxis Router | n/a | Apache 2.0 |

BSL 1.1 means: free for internal non-commercial use; commercial deployments need a licence. Contact `jason@kronaxis.co.uk`.

---

## More

- **Website:** [kronaxis.co.uk](https://kronaxis.co.uk)
- **Research portfolio:** [kronaxis.co.uk/research](https://kronaxis.co.uk/research)
- **Methodology paper:** [kronaxis.co.uk/methodology](https://kronaxis.co.uk/methodology)
- **Live election results browser:** [kronaxis.co.uk/election-results](https://kronaxis.co.uk/election-results)
- **Commercial enquiries:** `jason@kronaxis.co.uk`

---

*Kronaxis Limited (registered in England, no. 15072850).*
