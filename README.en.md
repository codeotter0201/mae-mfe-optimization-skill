# mae-mfe-optimization-skill

A skill that turns public MAE/MFE trading research into a structured, reusable methodology for AI agents. Loads directly into Codex and Claude Code.

This repository is intentionally documentation-first. It does not provide a backtesting engine, optimization API, or execution framework. It packages concepts, decision rules, and workflow guidance so an agent can reason about MAE/MFE-based strategy optimization with consistent terminology.

中文版本請見 [README.md](./README.md).

## What this skill covers

The skill focuses on MAE/MFE-driven strategy optimization, including:

- MAE, MFE before MAE, Global MFE, MHL, and Edge Ratio definitions
- delayed entry design
- stop-loss and take-profit placement
- trailing stop and breakeven logic
- scaling in and scaling out conditions
- practical triage before deep optimization
- overfitting-aware optimization order

The repo is useful when you want an agent to:

- explain the MAE/MFE optimization framework
- propose a disciplined optimization sequence
- reason about SL/TP or delayed entry choices
- distinguish structural improvement from overfitting
- navigate the topic as a research workflow instead of isolated tactics

## 30-second quickstart

If this is your first time opening the repo, this is the shortest read path:

1. **Strategy just finished backtesting, not sure if it's worth optimizing** → `references/practical-triage.md` (PF → WR → RF triage)
2. **You already have a specific optimization question (SL/TP, delayed entry, scaling, dynamic stops, ...)** → `references/decision-playbooks.md` (scenario-based playbooks)
3. **Need to align terminology or trace a concept back to a video** → `references/core-concepts.md`, `references/source-map.md`, `references/episode-notes.md`

`SKILL.md` itself is the entry index and decision tree; the detail lives in `references/`.

## Repository layout

```text
.
├── README.md
├── README.en.md
├── SKILL.md
└── references
    ├── core-concepts.md
    ├── practical-triage.md
    ├── chapter-details.md
    ├── decision-playbooks.md
    ├── source-map.md
    └── episode-notes.md
```

### `SKILL.md`

The main skill definition. It contains:

- trigger conditions and scope
- core terminology
- the five-step optimization spine
- chapter-to-decision mapping
- anti-overfitting principles

### `references/practical-triage.md`

A pre-optimization triage flow built around `Profit Factor -> Win Ratio -> Recovery Factor`, used to decide whether a strategy is worth deeper MAE/MFE analysis in the first place.

### `references/core-concepts.md`

The compact concepts layer, including:

- when to use MFE before MAE vs Global MFE
- core metric definitions
- anti-overfitting principles

### `references/chapter-details.md`

Expanded notes for the main topics, including:

- volatility normalization with ATR
- time-axis MAE/MFE and Edge Ratio
- placement testing
- delayed entry
- regime and environment filters
- trailing stop / breakeven
- scaling and account-level risk ideas

### `references/decision-playbooks.md`

Chapter content turned into ready-to-follow playbooks. Open this first when the question is "how do I design / judge / optimize X". Covers SL, TP, placement test, delayed entry, trailing stop / breakeven, and scaling scenarios.

### `references/source-map.md`

Episode ↔ chapter ↔ reference mapping with the playlist URL and per-episode YouTube links. Use it to trace a concept back to the original video.

### `references/episode-notes.md`

Per-episode mid-level notes extracted from the 22-episode subtitle set. Each entry lists the core question, data used, practical judgement, and risks. Closer to the original teaching order than `chapter-details.md`.

## Methodology at a glance

This skill is built around a simple principle: do not jump straight into brute-force parameter tuning.

Instead, ask:

1. Is the strategy worth optimizing at all?
2. Is the bottleneck entry timing, SL/TP structure, volatility regime, or trade management?
3. Is the improvement inside-trade structural improvement, or just backtest cosmetics?

The optimization sequence currently encoded in the skill is:

1. delayed entry
2. stop-loss / take-profit adjustment
3. environment filtering
4. dynamic stops
5. scaling
6. account-level hedging

The emphasis is on inside-trade structure such as MAE, MFE before MAE, Global MFE, MHL, and Edge Ratio, rather than relying only on top-line metrics like MDD or Sharpe.

## Install

The `SKILL.md` uses the standard frontmatter format and **works with both Codex and Claude Code**. Pick the skills directory for the platform you use:

| Platform | Skills directory |
| --- | --- |
| Codex | `~/.codex/skills/` |
| Claude Code | `~/.claude/skills/` |

### Option 1: Clone directly

```bash
# Codex
git clone https://github.com/codeotter0201/mae-mfe-optimization-skill.git \
  ~/.codex/skills/mae-mfe-optimization

# Claude Code
git clone https://github.com/codeotter0201/mae-mfe-optimization-skill.git \
  ~/.claude/skills/mae-mfe-optimization
```

After that, restart the platform or open a new session so the skill can be discovered.

### Option 2: Symlink during local development

If you want to keep editing this repo in place and expose it to the agent at the same time:

```bash
# Codex
ln -s /path/to/mae-mfe-optimization-skill \
  ~/.codex/skills/mae-mfe-optimization

# Claude Code
ln -s /path/to/mae-mfe-optimization-skill \
  ~/.claude/skills/mae-mfe-optimization
```

This is the better setup when you are iterating on `SKILL.md` and the reference docs.

### Expected final structure

```text
~/.codex/skills/                       ~/.claude/skills/
└── mae-mfe-optimization               └── mae-mfe-optimization
    ├── SKILL.md                           ├── SKILL.md
    └── references/                        └── references/
```

## Invoking the skill

Once installed, reference the skill by name or ask for the topic directly. Example prompts:

```text
Use mae-mfe-optimization to explain the correct optimization order for this strategy.
```

```text
Use mae-mfe-optimization to decide whether this system should test delayed entry before tightening SL.
```

```text
Apply mae-mfe-optimization and summarize whether this strategy is in a PF, WR, or RF bottleneck.
```

```text
Using mae-mfe-optimization, explain whether trailing stop or fixed TP is more appropriate here.
```

Full starting prompt (strategy just backtested, bottleneck unknown):

```text
Use mae-mfe-optimization. First run practical-triage.md to decide whether this
strategy is worth deep optimization. Then, based on which phase (PF / WR / RF) the
bottleneck is in, propose the next concrete action from decision-playbooks.md.
```

## Scope and non-goals

This repository is designed for:

- methodology packaging
- research guidance
- terminology alignment
- agent-readable documentation

It does not include:

- trade ETL pipelines
- backtest execution code
- parameter search infrastructure
- broker or live execution integration

If you want a full production workflow, this skill should sit upstream of your data, ranking, backtesting, and execution layers.

## Sources and attribution

This repository is a derivative knowledge packaging effort based on publicly available material, primarily:

- MAE/MFE website: <https://www.maemfe.org/>
- YouTube channel: <https://www.youtube.com/channel/UCHQ2g2rm9ml4dzdUfPdXuwQ>

The source material frames MAE/MFE as a practical analysis discipline centered on validation, trade-structure observation, and avoiding overfitting. This repository reorganizes those ideas into a skill format that Codex and Claude Code can load directly.

## Unofficial status

This is an unofficial repository. It is not affiliated with, endorsed by, or maintained by the original author or source channels.

If you reuse or redistribute this work, keep the original source links and make it clear which parts are your own summaries or restructuring.

## Disclaimer

This repository is for research, documentation, and agent-assisted analysis only. It is not investment advice, trading advice, or a promise of performance.
