# BELIEF.KERNEL

**Epistemic State Management for AI Systems**

> "LLMs confidently hallucinate. This system refuses to collapse uncertainty into fluent bullshit."

---

## What This Is

BELIEF.KERNEL is not a better LLM. It's a **belief management infrastructure** that:

- Treats LLMs as **evidence generators**, not authorities
- Maintains **persistent belief state** across inference runs
- Detects and preserves **epistemic conflict** instead of hiding it
- Outputs **ANSWER / HEDGE / CONFLICT / DEFER / REFUSE** based on evidence
- Applies **confidence decay** over time without reinforcement

**The paradigm break**: Intelligence as belief management, not token prediction.

---

## Quick Start

### 1. Clone and Setup

```bash
git clone <your-repo-url>
cd belief-kernel

# Create virtual environment
python3 -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt
```

### 2. Get OpenRouter API Key

1. Go to [openrouter.ai/keys](https://openrouter.ai/keys)
2. Create account and generate API key
3. Copy `.env.example` to `.env`:
   ```bash
   cp .env.example .env
   ```
4. Edit `.env` and add your API key:
   ```
   OPENROUTER_API_KEY=sk-or-v1-...
   ```

### 3. Run the Clustering Spike Test

**This is the critical de-risk test. If this fails, nothing else matters.**

```bash
python tests/test_clustering_spike.py
```

**What it tests**:
- Can we extract structured claims from LLM outputs?
- Does normalization + SimHash fingerprinting work?
- Do semantically similar claims cluster together?
- Do contradictory claims stay separate?

**Success criteria**: ≥80% of prompts produce valid, clusterable claims.

---

## Architecture

### Core Components

```
BELIEF.KERNEL
├── Witness Layer (LLM queries)
│   ├── OpenRouter adapter (100+ models)
│   ├── Forced JSON output contract
│   └── Parallel execution
│
├── Arbitration Engine
│   ├── Deterministic claim validation
│   ├── Normalization + fingerprinting
│   ├── Agreement/dispersion/novelty metrics
│   └── Decision thresholds (ANSWER/HEDGE/CONFLICT/DEFER/REFUSE)
│
└── Belief Store (event-sourced)
    ├── Immutable belief events
    ├── Computed decay (not mutated)
    └── Dependency graph (DAG)
```

### How It Works

1. **Query witnesses** (2-3 LLMs) in parallel
2. **Extract claims** with stance + confidence
3. **Normalize** claims (deterministic spine)
4. **Fingerprint** with SimHash for clustering
5. **Detect** agreement vs contradiction
6. **Arbitrate** using thresholds
7. **Persist** belief state to DB

---

## Project Status

**Current Phase**: Day 1-2 Spike (De-Risk)

- [x] Project structure
- [x] Claim validator + normalization
- [x] SimHash fingerprinting
- [x] OpenRouter witness adapter
- [x] Clustering spike test
- [ ] Validate ≥80% clustering accuracy
- [ ] Arbitration metrics (agreement, dispersion, novelty)
- [ ] Decision engine (5 states)
- [ ] SQLite event log
- [ ] FastAPI backend
- [ ] Comparator UI

---

## Configuration

### Witnesses (`config/witnesses.yaml`)

```yaml
witnesses:
  - model_id: "anthropic/claude-3.5-sonnet"
    temperature: 0.7
    nickname: "Claude 3.5 Sonnet"
  
  - model_id: "openai/gpt-4-turbo"
    temperature: 0.7
    nickname: "GPT-4 Turbo"
```

**Available models**: See [OpenRouter models](https://openrouter.ai/models)

---

## Why This Matters

Standard LLMs:
```
User: "Will China invade Taiwan?"
LLM: [3 paragraphs of confident geopolitical analysis]
```

BELIEF.KERNEL:
```
User: "Will China invade Taiwan?"
Kernel: CONFLICT ⚠
Reasoning: "Credible disagreement (60% contradiction rate)"
Evidence: 2 witnesses predict escalation, 2 predict deterrence
```

**The difference**: Uncertainty is preserved, not collapsed into fluent bullshit.

---

## Documentation

- [`DESIGN.md`](./DESIGN.md) - Full architecture specification
- [`MVP.md`](./MVP.md) - MVP scope and execution strategy
- [`config/witnesses.yaml`](./config/witnesses.yaml) - Model configurations

---

## License

MIT

---

## Status

**SPIKE IN PROGRESS**: Running clustering validation.

If you're reading this, the system is being built right now.
