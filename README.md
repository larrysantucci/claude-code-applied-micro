# Claude Code for Applied Microeconomics

A framework for using Claude Code in empirical economic research. Includes a knowledge architecture, process conventions, and lessons learned from hundreds of hours of AI-assisted research — plus a directory of community-built agents, skills, and tools you should know about.

**Author:** Larry Santucci, Federal Reserve Bank of Philadelphia

> The views expressed here are solely those of the author and do not necessarily reflect the views of the Federal Reserve Bank of Philadelphia or the Federal Reserve System.

---

## What's Here

| Component | What it does |
|-----------|-------------|
| **Knowledge architecture** | Three-layer system for organizing what Claude knows: behavioral guardrails, knowledge repositories, session memory |
| **Process conventions** | Verification doctrine, code conventions, knowledge management rules, subsampling protocols |
| **Lessons learned** | Hard-won debugging insights: subsampling bugs, memory crashes, inherited assumptions, pre-trends interpretation |
| **Stata patterns** | Technical notes for csdid, ivreghdfe, and general Stata gotchas |
| **Methods library** | Staggered DiD setup, IV diagnostics, pre-trends protocol, weight construction |
| **7 hooks** | Stata error detection, log archiving, LaTeX auto-compile, destructive command blocking, file backup, large-file warnings, desktop notifications |
| **Community directory** | Curated guide to economist Claude Code repos, skills, agents, hooks, and educational resources |

---

## Philosophy

Three principles guide this setup:

1. **Verification-first.** Every consequential step — filtering, merging, sampling, variable creation — gets an immediate check. The hooks and conventions enforce this automatically.

2. **Knowledge management.** Every fact lives in exactly one place. Three layers: behavioral guardrails (`CLAUDE.md`), knowledge repositories (`docs/knowledge/`), and session memory (`MEMORY.md`). No information scatter.


---

## Quick Start

### 1. Copy the configuration

```bash
# Clone the repo
git clone https://github.com/larrysantucci/claude-code-applied-micro.git
cd claude-code-applied-micro

# Copy global config to your home directory
cp -r .claude/* ~/.claude/

# Copy knowledge base (or wherever you keep cross-project knowledge)
cp -r knowledge/* ~/claude-knowledge/
```

### 2. Set up a project

Create a project-level `CLAUDE.md` in your project directory. See `examples/project-CLAUDE.md` for a template.

### 3. Add agents and skills

This repo provides the framework — the knowledge architecture and process conventions. For agents and skills (referee reviewers, paper summarizers, robustness checkers, etc.), see the community resources below. The ecosystem is rich, and several excellent implementations are freely available.

---

## Components

### Knowledge Architecture

Three-layer system documented in the [Beamer presentation](docs/knowledge-architecture.pdf):

```
Layer 1: Behavioral Guardrails    (~/.claude/CLAUDE.md)
    |
    v  informs
Layer 2: Knowledge Repositories   (~/claude-knowledge/ + docs/knowledge/)
    |
    v  grounds
Layer 3: Session Memory           (MEMORY.md — status, pending work, feedback)
```

**Key design rule:** Every fact lives in exactly one place. Paper notes are pure summaries. Project implications go in topic files. Output products are derived, not duplicated.

### Memory System

The memory architecture uses structured types:

| Type | Purpose | When to save |
|------|---------|-------------|
| `user` | User's role, preferences, expertise | When you learn about the user |
| `feedback` | Corrections and confirmed approaches | When the user corrects or validates an approach |
| `project` | Ongoing work, goals, decisions | When you learn project state that isn't in code/git |
| `reference` | Pointers to external systems | When you discover where information lives |

The `MEMORY.md` file is an index — one line per entry, under 150 characters. Each memory is a separate file with frontmatter (name, description, type) and content.

The **mandatory pre-action feedback review** is the enforcement mechanism: before any non-read action, Claude must verify that no feedback memory is violated. This costs ~200 tokens per check — trivial compared to the 5,000–15,000 tokens a preventable error costs in rework.

### Process Conventions & Lessons

The knowledge base (`knowledge/`) contains cross-project patterns built from real research experience:

| File | What you'll find |
|------|-----------------|
| `conventions.md` | Verification doctrine, code conventions, knowledge management, subsampling rules |
| `lessons.md` | 14 hard-won debugging lessons — subsampling bugs, memory crashes, pre-trends direction, data availability, inherited restrictions |
| `stata_patterns.md` | csdid memory limits, saverif() path bug, estat ordering, ivreghdfe usage |
| `methods_library.md` | CSDID setup checklist, IV diagnostics, pre-trends protocol, weight construction |

### Hooks

7 hooks in `settings.json`:

| Hook | Trigger | What it does |
|------|---------|-------------|
| Large file warning | PreToolUse: Read | Warns when reading files >500KB |
| Auto-backup | PreToolUse: Edit/Write | Creates timestamped backup before any file edit |
| Destructive command blocker | PreToolUse: Bash | Blocks `git reset --hard`, `rm -rf /`, `--no-verify`, etc. |
| Stata error detector | PostToolUse: Bash | Catches `r(...)` errors, "no observations", "variable not found" after Stata runs |
| Stata log archiver | PostToolUse: Bash | Auto-copies timestamped log after every Stata run |
| LaTeX auto-compile | PostToolUse: Edit/Write | Runs pdflatex after any .tex file is edited |
| Desktop notifications | Notification/Stop/SubagentStop | Alerts when waiting for input or when tasks finish |

---

## Community Agents, Skills & Tools

This repo does not include agents or skills. The community has built excellent ones. Here's what we recommend, based on our experience reviewing and testing them.

For a comprehensive directory with descriptions, links, and cross-references, see [`knowledge/economist_claude_code_repos.md`](knowledge/economist_claude_code_repos.md).

### Full Workflow Templates

If you want a complete drop-in setup with agents, skills, hooks, and rules:

| Repo | Author | What it offers |
|------|--------|----------------|
| [pedrohcgs/claude-code-my-workflow](https://github.com/pedrohcgs/claude-code-my-workflow) | Pedro Sant'Anna | The flagship template (821+ stars). 7 hooks, 10 agents, 22 skills, 18 rules, full settings.json. The [guide on his website](https://psantanna.com/claude-code-my-workflow/) is the best starting point for any economist new to Claude Code. |
| [hugosantanna/clo-author](https://github.com/hugosantanna/clo-author) | Hugo Sant'Anna | Reoriented for empirical paper writing. 18 agents (worker-critic pairs), 30 journal profiles, strategy memo workflow. See [hsantanna.org/clo-author](https://hsantanna.org/clo-author/). |
| [scunning1975/MixtapeTools](https://github.com/scunning1975/MixtapeTools) | Scott Cunningham | Creative skill designs including the "fletcher" defamiliarization audit and the "referee2" independent 5-audit protocol. His [6-part Substack series](https://causalinf.substack.com/p/claude-code-changed-how-i-work-part) is essential reading. |
| [chrisblattman/claudeblattman](https://github.com/chrisblattman/claudeblattman) | Chris Blattman | 14+ skills across 7 categories (analysis, data, theory, writing, communication, literature, ideation). See [claudeblattman.com](https://claudeblattman.com/). |

### Agents & Skills Worth Examining

| What | Where | Why it's interesting |
|------|-------|---------------------|
| Parallel paper review (6 agents) | [claesbackman/AI-research-feedback](https://github.com/claesbackman/AI-research-feedback) | Most thorough automated review setup |
| DiD + IV robustness skill | [zirui-song/claude-skills](https://github.com/zirui-song/claude-skills) | Field-specific robustness checklist |
| Stata syntax reference skill | [dylantmoore/stata-skill](https://github.com/dylantmoore/stata-skill) | Stata-specific knowledge |
| Curated skill collection (18+) | [meleantonio/awesome-econ-ai-stuff](https://meleantonio.github.io/awesome-econ-ai-stuff/) | Broad coverage: Stata, R, Python, Beamer, visualization |
| "fletcher" defamiliarization audit | [scunning1975/MixtapeTools](https://github.com/scunning1975/MixtapeTools) | Forces you to question assumptions via structured protocol |
| "referee2" independent 5-audit | [scunning1975/MixtapeTools](https://github.com/scunning1975/MixtapeTools) | Key principle: "the Claude that built the pipeline cannot objectively audit it" |

### Infrastructure

| What | Where | Why it matters |
|------|-------|---------------|
| Docker for YOLO mode | [paulgp/claude-container](https://github.com/paulgp/claude-container) | Safe sandboxing for autonomous Claude Code runs |
| Deliberate thinking MCP | [paulgp/deliberate-thinking](https://github.com/paulgp/deliberate-thinking) | Structured sequential reasoning |
| NBER RAG database | [paulgp/nber-rag-db](https://github.com/paulgp/nber-rag-db) | Search NBER working papers from Claude Code |
| Stata MCP server | [hanlulong/stata-mcp](https://github.com/hanlulong/stata-mcp) | Execute Stata directly from Claude Code |
| Live usage widget | [hugosantanna/clu-widget](https://github.com/hugosantanna/clu-widget) | Terminal widget for Claude Code usage stats |

### Educational Resources

| Resource | Author |
|----------|--------|
| [Claude Code for Applied Economists](https://markusacademy.substack.com/p/claude-code-for-applied-economists) (7-episode video series) | Paul Goldsmith-Pinkham + Markus Brunnermeier |
| [Getting Started with Claude Code](https://paulgp.substack.com/p/getting-started-with-claude-code) | Paul Goldsmith-Pinkham |
| [Claude Code Changed How I Work](https://causalinf.substack.com/p/claude-code-changed-how-i-work-part) (6-part series) | Scott Cunningham |
| [claude-code-my-workflow guide](https://psantanna.com/claude-code-my-workflow/) | Pedro Sant'Anna |
| [AI for Professionals Who Don't Code](https://claudeblattman.com/) | Chris Blattman |

---

## Adapting for Your Field

### Build project knowledge

For each project, create `docs/knowledge/INDEX.md` with topic files:
- `identification.md` — research design, threats, defenses
- `results.md` — findings and interpretation
- `methods.md` — estimation choices and diagnostics
- `data.md` — data sources, construction, limitations
- `papers/` — structured notes on related papers

### Start a lessons file

The `knowledge/lessons.md` file in this repo is our real lessons file. Yours will be different. Start it empty and add entries as you go — each one documents a specific bug, its root cause, the fix, and the generalizable lesson. After a few months of active use, this becomes one of the most valuable files in your setup.

---

## License

MIT. Use freely, adapt for your field, share improvements.
