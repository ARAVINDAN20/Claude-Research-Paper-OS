# Claude Research Paper OS

**An all-in-one agentic research system that turns Claude Code into an autonomous academic paper production pipeline.**

6 specialized agents. 47+ modular skills across 8 skill packs. One command to go from topic to submission-ready manuscript.

---

## What This Is

Claude Research Paper OS is a drop-in `.claude/` configuration directory. Copy it into any project folder where you are writing a research paper. When you open Claude Code in that folder, it auto-reads the configuration and becomes a full research and writing system.

It handles every stage of academic paper production.

- **Research** -- Literature search across Semantic Scholar, arXiv, and CrossRef
- **Planning** -- Outline generation with architectural strategy
- **Writing** -- Section-by-section drafting with enforced academic standards
- **Quality** -- AI pattern removal, citation validation, peer review simulation
- **Compilation** -- LaTeX build with error detection and formatting checks

The system produces papers that read like they were written by a human researcher, not an AI. Every output passes through a humanizer that detects and removes 25+ categories of AI writing patterns based on Wikipedia's "Signs of AI writing" guide.

---

## Quick Start

### 1. Clone this repo

```bash
git clone https://github.com/ARAVINDAN20/Claude-Research-Paper-OS.git
```

### 2. Copy `.claude/` into your paper project

```bash
cp -r Claude-Research-Paper-OS/.claude/ /path/to/your/paper/project/
```

### 3. Set up your paper structure

```bash
cd /path/to/your/paper/project/
# Copy a template if starting fresh
cp Claude-Research-Paper-OS/templates/ieee-journal/* .
# Or use your existing main.tex and references.bib
```

### 4. Edit the config for your paper

Open `.claude/CLAUDE.md` and change the paper configuration block at the top to match your paper title, authors, venue, and compile command.

### 5. Open Claude Code and start working

```bash
claude
```

That is it. Claude auto-reads `.claude/CLAUDE.md` on session start and all agents and skills become active.

### 6. Run the full pipeline (optional)

For a complete autonomous run from research to finished paper, say this to Claude.

```
Run the full research pipeline for [YOUR TOPIC]
```

See [docs/PIPELINE.md](docs/PIPELINE.md) for the 5-phase pipeline details.

---

## What Is Inside

```
Claude-Research-Paper-OS/
├── .claude/
│   ├── CLAUDE.md                          <- Master config (auto-read by Claude Code)
│   ├── agents/                            <- 6 specialized agents
│   │   ├── paper-writer.md                <- Writes/rewrites paper sections
│   │   ├── paper-reviewer.md              <- Multi-dimensional peer review
│   │   ├── citation-checker.md            <- Validates all citations
│   │   ├── humanizer-check.md             <- Removes AI writing patterns
│   │   ├── latex-compiler.md              <- Compiles and checks LaTeX
│   │   └── deep-researcher.md             <- Academic literature search
│   └── skills/                            <- 8 skill packs, 47+ skills
│       ├── agent-research-skills/         <- 31 skills (full research pipeline)
│       ├── academic-research-skills/      <- 4 skills (quality + review)
│       ├── creative-writing-skills/       <- 6 skills (prose + narrative)
│       ├── academic-paper-skills/         <- 2 skills (strategist + composer)
│       ├── claude-scientific-writer/      <- 1 skill (scientific conventions)
│       ├── paper-writer-skill/            <- 1 skill (section templates)
│       ├── humanizer/                     <- 1 skill (AI pattern removal)
│       └── content-research-writer/       <- 1 skill (research-backed writing)
├── docs/
│   ├── AGENTS.md                          <- Agent documentation
│   ├── SKILLS.md                          <- Skills documentation
│   ├── PIPELINE.md                        <- 5-phase pipeline walkthrough
│   └── SETUP.md                           <- Detailed setup instructions
├── templates/
│   ├── ieee-journal/                      <- IEEE Access / IEEE journal template
│   ├── conference-paper/                  <- Generic conference template
│   └── WRITING_RULES.md                   <- Writing rules template
├── README.md                              <- This file
└── LICENSE
```

---

## How Auto-Calling Works

Claude Code reads `.claude/CLAUDE.md` at the start of every session. That file contains a **Skill Auto-Trigger Map** that tells Claude which skills to invoke for each type of task. You never need to call skills manually. Just describe what you want and Claude matches it to the right tools.

| You say | Claude activates |
|---|---|
| "Write the introduction" | paper-writer + humanizer + prose-writing + quality check |
| "Check my citations" | citation-checker agent + citation-management skill |
| "Review this section" | paper-reviewer + academic-paper-reviewer + humanizer |
| "Find papers on X" | deep-researcher + literature-search + Semantic Scholar |
| "Compile the paper" | latex-compiler + formatting check + assembly check |
| "Is this claim novel?" | novelty-assessment + literature-search |
| "Run the full pipeline" | All 5 phases in sequence (see PIPELINE.md) |

Claude also reads the agents in `.claude/agents/`. When a task matches an agent's description, Claude spawns it as a subagent with its own context and tools.

---

## The 6 Agents

Each agent is a markdown file in `.claude/agents/` that defines a specialized role with specific instructions.

| Agent | What It Does | When It Runs |
|---|---|---|
| **paper-writer** | Writes and rewrites paper sections following strict academic rules, no AI patterns, no colons/semicolons, every term explained | When you ask to write, draft, rewrite, or improve any section |
| **paper-reviewer** | Runs a word-by-word devil's advocate review covering 13 quality dimensions | When you ask to review, check, audit, or verify the paper |
| **citation-checker** | Validates every `\cite{}` against the .bib file and flags uncited claims | When you ask to check citations or verify references |
| **humanizer-check** | Scans for 25+ categories of AI writing patterns and fixes them | After any writing session, or when you ask to humanize |
| **latex-compiler** | Compiles with Docker, checks page count, catches warnings and errors | When you ask to compile, build, or check the paper |
| **deep-researcher** | Searches Semantic Scholar, arXiv, and CrossRef for relevant papers | When you ask to research, find papers, or verify claims |

See [docs/AGENTS.md](docs/AGENTS.md) for full details on each agent.

---

## The 47+ Skills

Skills are specialized knowledge and tool packages that agents draw on. They are organized into 8 packs from different open-source contributors.

| Skill Pack | Skills | What It Covers |
|---|---|---|
| **agent-research-skills** (lingzhi227) | 31 | Full research pipeline from idea to submission |
| **academic-research-skills** (Imbad0202) | 4 | Writing quality, peer review, deep research |
| **creative-writing-skills** (haowjy) | 6 | Prose quality, narrative flow, story critique |
| **academic-paper-skills** (lishix520) | 2 | Strategic planning and compositional writing |
| **claude-scientific-writer** (K-Dense-AI) | 1 | Scientific writing conventions |
| **paper-writer-skill** (kgraph57) | 1 | Section templates and writing workflow |
| **humanizer** | 1 | AI pattern detection and removal |
| **content-research-writer** | 1 | Research-backed content with citations |

See [docs/SKILLS.md](docs/SKILLS.md) for a breakdown of every individual skill.

---

## The 5-Phase Pipeline

When you ask Claude to run the full pipeline, it executes five phases. Agents within each phase run in parallel where possible.

```
Phase 1: RESEARCH
  └─ deep-researcher (parallel searches across 3 databases)
  └─ literature-search + novelty-assessment

Phase 2: STRUCTURE
  └─ strategist (outline and architecture)
  └─ research-planning (gap analysis)

Phase 3: WRITING
  └─ paper-writer (sections written in parallel, up to 4 at once)
  └─ humanizer (runs after each section)
  └─ citation-management (validates as citations are added)

Phase 4: QUALITY
  └─ paper-reviewer (13-dimension review)
  └─ humanizer-check (full paper AI pattern scan)
  └─ citation-checker (final validation)
  └─ self-review (3-persona simulated peer review)

Phase 5: COMPILE AND SUBMIT
  └─ latex-compiler (build + error fix)
  └─ paper-assembly (completeness check)
  └─ Final humanizer pass
```

See [docs/PIPELINE.md](docs/PIPELINE.md) for the complete walkthrough.

---

## Writing Quality Standards

This system enforces writing standards designed to produce Q1 journal and Q1 conference quality papers.

**What gets enforced automatically.**

- Zero AI vocabulary (25+ banned words and phrases)
- Zero em dashes, colons, or semicolons in prose
- Every technical term explained before first use
- Every claim hedged with evidence-based language
- Every formula fully explained with a "where" clause
- Every number attributed to a cited source
- Varied sentence and paragraph length (burstiness)
- No synonym cycling, no rule-of-three forcing
- No throat-clearing openers or meta-commentary
- Figures and tables placed near their first reference

The humanizer runs multiple passes. After writing, after review, and before final compilation. It catches patterns that other systems miss because it checks 25 distinct categories from Wikipedia's "Signs of AI writing" documentation.

---

## Requirements

- **Claude Code** -- [Install Claude Code](https://docs.anthropic.com/en/docs/claude-code/overview)
- **Docker** (optional) -- For LaTeX compilation. If you do not use Docker, change the compile command in `.claude/CLAUDE.md` to your local `pdflatex` path.
- **Python 3.8+** (optional) -- Some skills include Python scripts for citation validation, formatting checks, and academic search.

---

## Supported Paper Formats

The system works with any LaTeX paper. Templates are provided for common venues.

| Template | Location | Notes |
|---|---|---|
| IEEE Access / IEEE Journal | `templates/ieee-journal/` | IEEEtran class, journal mode |
| Generic Conference | `templates/conference-paper/` | Two-column, abstract + sections |

You can use your own LaTeX template. Just update the compile command in `.claude/CLAUDE.md`.

---

## Credits

This system integrates open-source skills from the Claude Code community.

| Skill Pack | Author | License |
|---|---|---|
| agent-research-skills | [lingzhi227](https://github.com/lingzhi227) | See skills directory |
| academic-research-skills | [Imbad0202](https://github.com/Imbad0202) | GPL-3.0 |
| creative-writing-skills | [haowjy](https://github.com/haowjy) | Apache-2.0 |
| academic-paper-skills | [lishix520](https://github.com/lishix520) | MIT |
| claude-scientific-writer | [K-Dense-AI](https://github.com/K-Dense-AI) | MIT |
| paper-writer-skill | [kgraph57](https://github.com/kgraph57) | See skills directory |
| humanizer | Community | MIT |
| content-research-writer | Community | See skills directory |

---

## License

MIT License. See [LICENSE](LICENSE) for details.

Individual skill packs retain their original licenses. See each skill pack's LICENSE file.
