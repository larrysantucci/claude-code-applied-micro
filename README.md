# Claude Code for Applied Microeconomics

A Claude Code configuration built for applied economists doing empirical research. Includes referee agents calibrated to 14 economics journals, a knowledge management architecture, Stata and LaTeX hooks, and lessons learned from hundreds of hours of AI-assisted research.

**Author:** Larry Santucci, Federal Reserve Bank of Philadelphia

> The views expressed here are solely those of the author and do not necessarily reflect the views of the Federal Reserve Bank of Philadelphia or the Federal Reserve System.

---

## What's Here

| Component | What it does |
|-----------|-------------|
| **Domain profile** | Field-specific calibration for consumer finance/banking (use as a template for your field) |
| **2 skills** | `/review-paper` (structured paper notes), `/scope` (autonomy control — throttle how much Claude does before checking in) |
| **7 hooks** | Stata error detection, log archiving, LaTeX auto-compile, destructive command blocking, file backup, large-file warnings, desktop notifications |
| **Knowledge base** | Cross-project patterns: Stata technical notes, methods library, process conventions, debugging lessons |
| **Beamer presentation** | Slides explaining the three-layer knowledge architecture ([PDF](docs/knowledge-architecture.pdf)) |

---

## Philosophy

Three principles guide this setup:

1. **Verification-first.** Every consequential step — filtering, merging, sampling, variable creation — gets an immediate check. The hooks and conventions enforce this automatically.

2. **Knowledge management.** Every fact lives in exactly one place. Three layers: behavioral guardrails (`CLAUDE.md`), knowledge repositories (`docs/knowledge/`), and session memory (`MEMORY.md`). No information scatter.

3. **Field calibration.** Generic AI assistants don't know your field's conventions. The domain profile and journal profiles teach Claude what referees in your subfield actually care about.

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

### 2. Customize for your field

The domain profile (`~/.claude/rules/domain-profile.md`) is a worked example for consumer finance/banking. **Copy it and adapt for your field:**

- Replace the datasets table with your field's common data sources
- Replace the identification strategies with those used in your subfield
- Replace the seminal references
- Replace the field-specific referee concerns

The journal profiles (`~/.claude/rules/journal-profiles.md`) cover 14 applied economics journals. Add your target journals using the template at the bottom of the file.

### 3. Set up a project

Create a project-level `CLAUDE.md` in your project directory. See `examples/project-CLAUDE.md` for a template.

### 4. Try a review

```
/review-draft --peer AEJ:Applied
```

This launches the domain referee and methods referee in parallel, both calibrated to AEJ:Applied's standards. They run blind — neither sees the other's report.

---

## Components

### Referee Agents

Three agents in `.claude/agents/`:

| Agent | Role | Reads |
|-------|------|-------|
| `domain-referee` | Literature, contribution, external validity, journal fit | domain-profile.md, journal-profiles.md |
| `methods-referee` | Identification, estimation, inference, robustness | methods_library.md, lessons.md, domain-profile.md |
| `coder-critic` | Code audit: design drift, verification, conventions | conventions.md, lessons.md, stata_patterns.md |

All three produce severity-ranked issue lists (Critical / Important / Minor), not numeric scores. The domain and methods referees are blind to each other.

### Journal Profiles

14 journals with calibrated referee behavior:

| Tier | Journals |
|------|----------|
| Top-5 | AER, JPE, QJE, REStud, Econometrica |
| Top field | AEJ:Applied, AEJ:Policy, JHR, RAND, JPubE, JME, JFI |
| Strong field / specialty | JBF, JFSR, JUE, RESTAT, JCA |

Each profile specifies: focus, bar, how domain and methods referees should adjust, and typical referee concerns. When you run `/review-draft --peer JHR`, the referees calibrate to JHR's expectations.

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

### Skills

| Skill | Usage | What it does |
|-------|-------|-------------|
| `/review-paper` | `<path-to-paper>` | Extract structured notes from someone else's paper |
| `/scope` | `full`, `moderate`, `limited` | Control how much Claude does before stopping to check in. Default is `limited`. See [SKILL.md](.claude/skills/scope/SKILL.md). |

For simulated peer review (`/review-draft`) and R&R triage (`/revise`), see [Scott Cunningham's MixtapeTools](https://github.com/scunning1975/MixtapeTools).

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

---

## Adapting for Your Field

### Write your domain profile

The domain profile is the highest-value customization. For your field, document:

1. **Journals by tier** — where you'd submit and where your referees publish
2. **Common datasets** — what data your field uses, access restrictions
3. **Identification strategies** — the 4-6 designs your field uses most, with assumptions
4. **Field conventions** — what referees expect (distributional analysis? welfare calculations? mechanism tests?)
5. **Seminal references** — the 8-10 papers every referee knows
6. **Referee concerns** — the predictable objections in your subfield

### Add journal profiles

Use the template at the bottom of `journal-profiles.md`:

```markdown
### [Journal Name] ([Abbreviation])
**Focus:** [fields and topics]
**Bar:** [what it takes to publish here]
**Domain referee adjusts:** [what domain reviewers care about]
**Methods referee adjusts:** [rigor expectations, preferred methods]
**Typical concerns:** [common referee questions]
```

### Build project knowledge

For each project, create `docs/knowledge/INDEX.md` with topic files:
- `identification.md` — research design, threats, defenses
- `results.md` — findings and interpretation
- `methods.md` — estimation choices and diagnostics
- `data.md` — data sources, construction, limitations
- `papers/` — structured notes on related papers (created by `/review-paper`)

---

## Updating from Your Internal Setup

If you maintain a private version of these files and periodically sync to this public repo:

1. Copy the modified internal file to the repo directory
2. Run the sensitivity check:
   ```bash
   # Check for internal paths, names, data references
   grep -rn '/home/\|/shared/\|TransUnion\|\.dta\b\|\.sas7bdat' .claude/ knowledge/
   ```
3. Review the diff: `git diff`
4. Commit and push

---

## Acknowledgments

This setup was built incrementally over months of AI-assisted empirical research. It draws on ideas and code from the growing community of economists using Claude Code:

- **[Pedro Sant'Anna](https://github.com/pedrohcgs/claude-code-my-workflow)** — The flagship Claude Code workflow template for economists. Pedro's repo (821+ stars) pioneered the hooks + agents + skills architecture that this setup builds on. The pre-compact/post-compact hooks for context preservation, the verify-reminder hook, and the log-reminder hook all originate from Pedro's work. The [guide on his website](https://psantanna.com/claude-code-my-workflow/) is the best starting point for any economist new to Claude Code.

- **[Hugo Sant'Anna](https://github.com/hugosantanna/clo-author)** — Clo-Author reoriented Pedro's template for empirical paper writing, with worker-critic agent pairs, 30 journal profiles, and a strategy memo workflow. Hugo's approach to file protection hooks and the post-merge session report reminder influenced this setup.

- **[Scott Cunningham](https://github.com/scunning1975/MixtapeTools)** — MixtapeTools introduced creative skill designs, including the "fletcher" defamiliarization audit and the "referee2" independent 5-audit protocol. Scott's [6-part Substack series](https://causalinf.substack.com/p/claude-code-changed-how-i-work-part) on Claude Code is essential reading.

- **[Chris Blattman](https://github.com/chrisblattman/claudeblattman)** — ClaudeBlattman demonstrated how to organize 14+ skills across multiple research workflow categories (analysis, data, theory, writing, communication, literature, ideation). The breadth of coverage influenced the skill design here. See [claudeblattman.com](https://claudeblattman.com/) for his approach.

- **[Paul Goldsmith-Pinkham](https://github.com/paulgp)** — Docker containers for YOLO mode, the deliberate-thinking MCP server, and the NBER RAG database. Paul's [Substack posts](https://paulgp.substack.com/p/getting-started-with-claude-code) and [video series with Markus Brunnermeier](https://markusacademy.substack.com/p/claude-code-for-applied-economists) are the other major educational entry point alongside Scott's.

- **[Claes Backman](https://github.com/claesbackman/AI-research-feedback)** — The parallel-agent paper review skill (6 agents for comprehensive review) influenced the blind parallel referee design used here.

- **[Zirui Song](https://github.com/zirui-song/claude-skills)** — The `/robustness` skill (DiD + IV specific) and `/lit-review` skill demonstrated field-specific skill design.

- **[Michael Ewens](https://gist.github.com/michaelewens/)** — The log-reminder hook pattern (credited by Pedro Sant'Anna) that ensures session documentation.

A broader directory of economist Claude Code resources is maintained at [`~/claude-knowledge/economist_claude_code_repos.md`](knowledge/) in this repo's knowledge directory.

---

## License

MIT. Use freely, adapt for your field, share improvements.
