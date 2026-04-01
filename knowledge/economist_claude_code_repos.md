# Economist Claude Code Repositories & Resources

_Last updated: 2026-04-01_

## Full Workflow Templates

| Repo | Author | Stars | What it has |
|------|--------|-------|-------------|
| [pedrohcgs/claude-code-my-workflow](https://github.com/pedrohcgs/claude-code-my-workflow) | Pedro Sant'Anna | 821 | 7 hooks, 10 agents, 22 skills, 18 rules, full settings.json. The flagship template. |
| [hugosantanna/clo-author](https://github.com/hugosantanna/clo-author) | Hugo Sant'Anna | — | 4 hooks, 18 agents (worker-critic pairs), 10 skills, 8 rules, 30 journal profiles. Fork of Pedro's, reoriented for empirical paper writing. |
| [scunning1975/MixtapeTools](https://github.com/scunning1975/MixtapeTools) | Scott Cunningham | — | 5 skills (fletcher, referee2, compiledeck, split-pdf, newproject), CLAUDE.md template, personas. No hooks. |
| [chrisblattman/claudeblattman](https://github.com/chrisblattman/claudeblattman) | Chris Blattman | — | 14+ skills across 7 categories (Analysis, Data, Theory, Writing, Communication, Literature, Ideation). Agents and templates. Site: [claudeblattman.com](https://claudeblattman.com/) |

## Hooks (the rare ones)

**Pedro Sant'Anna's 7 hooks** (claude-code-my-workflow):
1. `notify.sh` — Notification: cross-platform desktop notification
2. `protect-files.sh` — PreToolUse Edit|Write: blocks edits to Bibliography_base.bib and settings.json
3. `pre-compact.py` — PreCompact: captures plan state, current task, recent decisions before compaction
4. `post-compact-restore.py` — SessionStart compact|resume: restores context after compaction
5. `context-monitor.py` — PostToolUse Bash|Task: estimates context usage, progressive warnings at 40/55/65/80/90%
6. `verify-reminder.py` — PostToolUse Write|Edit: reminds to compile .tex, render .qmd, or run .R after editing (60s debounce)
7. `log-reminder.py` — Stop: after 15 responses without session log update, blocks Claude and demands an entry. Adapted from [Michael Ewens's gist](https://gist.github.com/michaelewens/9a1bc5a97f3f9bbb79453e5b682df462)

**Hugo Sant'Anna's 4 hooks** (clo-author):
1. `protect-files.sh` — PreToolUse Edit|Write: blocks edits to settings.json, strategy-memo-*.md, referee-report-*.md, quality-score-*.json
2. `pre-compact.py` — PreCompact: context survival checklist before compaction (exit 2 so visible in transcript)
3. `post-compact-restore.py` — SessionStart compact|resume: restores context
4. `post-merge.sh` — git post-merge: reminds to update SESSION_REPORT.md

## Skills & Agents Collections

| Repo | Author | What it has |
|------|--------|-------------|
| [claesbackman/AI-research-feedback](https://github.com/claesbackman/AI-research-feedback) | Claes Backman | 4 skills: review-paper (6 parallel agents), review-paper-light (2 agents), review-pap (pre-analysis plans), review-grant (panel review) |
| [zirui-song/claude-skills](https://github.com/zirui-song/claude-skills) | Zirui Song | 6 skills: /robustness (DiD + IV specific), /lit-review, /coding-guidelines, /data-doc, /project-structure, /referee-response |
| [meleantonio/awesome-econ-ai-stuff](https://meleantonio.github.io/awesome-econ-ai-stuff/) | Antonio Mele | Curated collection of 18+ skills: stata-regression, python-panel-data, r-econometrics, api-data-fetcher, stata-data-cleaning, beamer-presentation, econ-visualization, and more |
| [dylantmoore/stata-skill](https://github.com/dylantmoore/stata-skill) | Dylan Moore | Stata syntax and econometrics reference skill |

## Infrastructure & Tools

| Repo | Author | What it does |
|------|--------|-------------|
| [paulgp/claude-container](https://github.com/paulgp/claude-container) | Paul Goldsmith-Pinkham | Docker containers for running Claude Code in YOLO mode |
| [paulgp/deliberate-thinking](https://github.com/paulgp/deliberate-thinking) | Paul Goldsmith-Pinkham | MCP server (Rust) for structured sequential thinking |
| [paulgp/nber-rag-db](https://github.com/paulgp/nber-rag-db) | Paul Goldsmith-Pinkham | RAG database for NBER working papers |
| [paulgp/yale-som-hpc-apptainer-example](https://github.com/paulgp/yale-som-hpc-apptainer-example) | Paul Goldsmith-Pinkham | Guide for running AI coding assistants on HPC clusters |
| [hanlulong/stata-mcp](https://github.com/hanlulong/stata-mcp) | — | MCP server for executing Stata from Claude Code |
| [DAAF-Contribution-Community/daaf](https://github.com/DAAF-Contribution-Community/daaf) | Brian Heseung Kim | Data Analyst Augmentation Framework with .claude/ hooks, agents, skills. Starred by both Sant'Annas. |
| [apoorvalal/claude_paper_digester](https://github.com/apoorvalal/claude_paper_digester) | Apoorva Lal | Programmatic paper summaries via Claude API |
| [hugosantanna/clu-widget](https://github.com/hugosantanna/clu-widget) | Hugo Sant'Anna | Terminal widget for live Claude Code usage statistics |
| [flonat/claude-research](https://github.com/flonat/claude-research) | — | 8 general academic-workflow hooks (context preservation, destructive-git blocking, promise checking) |
| [Galaxy-Dawn/claude-scholar](https://github.com/Galaxy-Dawn/claude-scholar) | — | 5 general academic hooks (security, session management) |

## Blog Posts & Guides

| Resource | Author | URL |
|----------|--------|-----|
| Claude Code Changed How I Work (6-part series, paywalled) | Scott Cunningham | [causalinf.substack.com](https://causalinf.substack.com/p/claude-code-changed-how-i-work-part) |
| Getting Started with Claude Code: A Researcher's Setup Guide | Paul Goldsmith-Pinkham | [paulgp.substack.com](https://paulgp.substack.com/p/getting-started-with-claude-code) |
| From EDGAR Filings to a Structured Database | Paul Goldsmith-Pinkham | [paulgp.substack.com](https://paulgp.substack.com/p/from-edgar-filings-to-a-structured) |
| Research in the Time of AI | Paul Goldsmith-Pinkham | [paulgp.substack.com](https://paulgp.substack.com/p/research-in-the-time-of-ai) |
| Claude Code for Applied Economists (7-episode video series) | Paul Goldsmith-Pinkham + Markus Brunnermeier | [markusacademy.substack.com](https://markusacademy.substack.com/p/claude-code-for-applied-economists) |
| claude-code-my-workflow guide | Pedro Sant'Anna | [psantanna.com/claude-code-my-workflow](https://psantanna.com/claude-code-my-workflow/) |
| Clo-Author 3.0 architecture | Hugo Sant'Anna | [hsantanna.org/clo-author](https://hsantanna.org/clo-author/) |
| Building Claude Code Workflow for Economics Scholars | Zhiyuan Ryan Chen | [zhiyuanryanchen.github.io](https://zhiyuanryanchen.github.io/claude-code-workflow.html) |
| Using Claude Code for Social Science Research | evolvingimpact | [evolvingimpact.wordpress.com](https://evolvingimpact.wordpress.com/2026/03/04/using-claude-code-and-codex-cli-for-social-science-research-my-learnings-as-they-happen/) |
| AI for Professionals Who Don't Code | Chris Blattman | [claudeblattman.com](https://claudeblattman.com/) |

## Notable Skills Worth Examining

- **Cunningham's "fletcher"** — Defamiliarization audit named for Jason Fletcher. Six-step protocol for challenging assumptions. Returns CLEAR/CONDITIONAL/HOLD ruling.
- **Cunningham's "referee2"** — Independent 5-audit protocol: code audit, cross-language replication, directory structure, output automation, econometrics. Key principle: "the Claude that built the pipeline cannot objectively audit it."
- **Backman's "review-paper"** — Spawns 6 parallel agents for comprehensive paper review.
- **Song's "/robustness"** — DiD + IV specific robustness checklist.
- **Pedro's "/learn"** — Skill extraction from current session for future reuse.
- **Pedro's "verify-reminder" hook** — Reminds to compile/render after file edits with 60s debounce.

## Cross-References

The ecosystem is interconnected:
- Pedro's README lists 5 community extensions: clo-author, claudeblattman, MixtapeTools, autoresearch, ClaudeCodeTools
- Hugo's clo-author is a direct rebuild of Pedro's template
- Cunningham starred Paul GP's claude-container
- Both Sant'Annas starred DAAF
- Pedro's log-reminder hook credits Michael Ewens's gist
- Cunningham's Substack and Paul GP's Markus Academy series are the two main educational entry points
