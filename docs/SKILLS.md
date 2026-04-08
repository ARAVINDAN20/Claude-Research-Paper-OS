# Skills Reference

Skills are specialized knowledge and tool packages stored in `.claude/skills/`. Claude draws on them automatically based on the task at hand. You do not need to invoke them manually.

This document covers all 8 skill packs and 47+ individual skills.

---

## How Skills Work

Skills are loaded when Claude's auto-trigger map (in `.claude/CLAUDE.md`) matches your request to a skill. For example, when you say "write the introduction," Claude loads the paper-writing-section skill, the humanizer skill, the prose-writing skill, and the writing quality check skill. All run in sequence as part of the writing workflow.

Some skills include Python scripts. These require Python 3.8+ and the dependencies listed in each skill's directory. Most skills are pure-prompt skills that work without any external dependencies.

---

## Skill Pack 1: agent-research-skills (31 skills)

**Author:** [lingzhi227](https://github.com/lingzhi227)
**Location:** `.claude/skills/agent-research-skills/skills/`

This is the largest and most comprehensive pack. It covers the entire research pipeline from idea generation through submission and rebuttal.

### Research Phase Skills

| Skill | What It Does |
|---|---|
| **deep-research** | 6-phase systematic literature survey with search scripts for Semantic Scholar and arXiv |
| **literature-search** | Multi-source academic search with CrossRef, Semantic Scholar, and arXiv download scripts |
| **literature-review** | Structured literature review with thematic organization |
| **github-research** | Searches GitHub for relevant codebases and implementations |
| **novelty-assessment** | Evaluates whether your contribution is genuinely novel against existing work |
| **idea-generation** | Structured brainstorming for research ideas and hypotheses |
| **research-planning** | Produces a research plan with milestones and gap analysis |

### Writing Phase Skills

| Skill | What It Does |
|---|---|
| **paper-writing-section** | Section-specific writing tips (abstract, intro, methods, results, discussion, conclusion) |
| **related-work-writing** | Thematic organization for related work with compare-and-contrast structure |
| **survey-generation** | RAG-based subsection writing with automatic citation validation |
| **rebuttal-writing** | Templates and strategies for peer review rebuttals |
| **paper-revision** | Handles major/minor revisions based on reviewer feedback |

### Quality and Review Skills

| Skill | What It Does |
|---|---|
| **self-review** | 3-persona automated review (NeurIPS review form format) |
| **citation-management** | BibTeX validation, deduplication, auto-fix, and consistency checks |
| **latex-formatting** | LaTeX formatting fixes and pre-submission checks |
| **paper-compilation** | pdflatex + bibtex pipeline with automatic error fixing |
| **paper-assembly** | End-to-end pipeline completeness check |
| **backward-traceability** | Verifies every claim traces back to a source |

### Technical Skills

| Skill | What It Does |
|---|---|
| **math-reasoning** | Derivations, notation tables, statistical test selection |
| **symbolic-equation** | Symbolic math manipulation and equation formatting |
| **algorithm-design** | Algorithm pseudocode generation and complexity analysis |
| **figure-generation** | Publication-quality matplotlib figures |
| **table-generation** | LaTeX booktabs tables |
| **slide-generation** | Presentation slides from paper content |
| **data-analysis** | Statistical analysis and data visualization |

### Engineering Skills

| Skill | What It Does |
|---|---|
| **experiment-design** | Experimental protocol design with controls and baselines |
| **experiment-code** | Generates experiment code from protocols |
| **code-debugging** | Debugging experiment code |
| **paper-to-code** | Extracts implementable code from paper descriptions |
| **atomic-decomposition** | Breaks complex tasks into atomic subtasks |
| **excalidraw-skill** | Diagram generation for papers and presentations |

---

## Skill Pack 2: academic-research-skills (4 skills)

**Author:** [Imbad0202](https://github.com/Imbad0202)
**Location:** `.claude/skills/academic-research-skills/`
**License:** GPL-3.0

### Skills

| Skill | What It Does |
|---|---|
| **academic-paper** | Writing quality checklist with 25+ anti-AI checks. The `references/writing_quality_check.md` file is the master quality gate for all writing |
| **academic-paper-reviewer** | Multi-perspective peer review with 0-100 rubrics across multiple dimensions |
| **academic-pipeline** | Orchestrates the full research-to-paper pipeline |
| **deep-research** | 6-phase systematic literature survey with structured methodology |

The `academic-paper/references/writing_quality_check.md` file is referenced throughout the system as the authoritative list of banned AI terms and quality standards.

---

## Skill Pack 3: creative-writing-skills (6 skills)

**Author:** [haowjy](https://github.com/haowjy)
**Location:** `.claude/skills/creative-writing-skills/creative-writing-skills/`
**License:** Apache-2.0

These skills handle prose quality. Academic writing is still writing, and these skills ensure it reads naturally.

| Skill | What It Does |
|---|---|
| **cw-prose-writing** | Narrative flow, sentence rhythm, and natural prose style. Ensures varied sentence structure and smooth transitions |
| **cw-story-critique** | Critiques flow, pacing, and readability. Identifies where a section drags, jumps, or loses the reader |
| **cw-brainstorming** | Idea generation and creative exploration for framing research narratives |
| **cw-official-docs** | Technical and official documentation writing style |
| **cw-router** | Routes writing tasks to the appropriate sub-skill |
| **cw-style-skill-creator** | Creates custom writing style profiles for consistent voice |

---

## Skill Pack 4: academic-paper-skills (2 skills)

**Author:** [lishix520](https://github.com/lishix520)
**Location:** `.claude/skills/academic-paper-skills/`
**License:** MIT

| Skill | What It Does |
|---|---|
| **strategist** | Plans paper structure, identifies key arguments, maps section dependencies, and produces an outline with quality checkpoints at each stage |
| **composer** | Drafts paper content following the strategist's plan, with built-in quality gates that pause and check work at paragraph boundaries |

The strategist-then-composer workflow is useful for initial drafting. The strategist plans, then the composer executes with frequent quality checks.

---

## Skill Pack 5: claude-scientific-writer (1 skill)

**Author:** [K-Dense-AI](https://github.com/K-Dense-AI)
**Location:** `.claude/skills/claude-scientific-writer/`
**License:** MIT

A comprehensive scientific writing toolkit with templates, scripts, and conventions for academic papers. Includes example configurations and API usage patterns. Covers citation formatting, section structure, and scientific terminology conventions.

---

## Skill Pack 6: paper-writer-skill (1 skill)

**Author:** [kgraph57](https://github.com/kgraph57)
**Location:** `.claude/skills/paper-writer-skill/`

Provides section-by-section templates and helper scripts for writing academic papers. Includes a SKILL.md with detailed instructions, reference materials in `references/`, and utility scripts in `scripts/`.

---

## Skill Pack 7: humanizer (1 skill)

**Location:** `.claude/skills/humanizer/`
**License:** MIT

The AI pattern detection and removal skill. Based on Wikipedia's "Signs of AI writing" guide maintained by WikiProject AI Cleanup.

Checks 25 pattern categories.

1. Undue emphasis on significance, legacy, and broader trends
2. Undue emphasis on notability and media coverage
3. Superficial analyses with -ing endings
4. Promotional and advertisement-like language
5. Vague attributions and weasel words
6. Outline-like "Challenges and Future Prospects" sections
7. Overused AI vocabulary (25+ specific words)
8. Copula avoidance ("serves as" instead of "is")
9. Negative parallelisms ("Not only...but...")
10. Rule of Three overuse
11. Elegant variation (synonym cycling)
12. False ranges ("from X to Y")
13. Em dash overuse
14. Boldface overuse
15. Inline-header vertical lists
16. Title case in headings
17. Emojis in professional text
18. Curly quotation marks
19. Collaborative communication artifacts
20. Knowledge-cutoff disclaimers
21. Sycophantic/servile tone
22. Filler phrases
23. Excessive hedging
24. Generic positive conclusions
25. Hyphenated word pair overuse

The skill includes a two-pass process. First, rewrite to remove patterns. Second, run the "What makes this obviously AI generated?" audit and fix remaining tells.

---

## Skill Pack 8: content-research-writer (1 skill)

**Location:** `.claude/skills/content-research-writer/`

Assists in writing content that is grounded in research. Conducts background research, adds citations, improves hooks, iterates on outlines, and provides feedback on each section. Useful as a supplement to the main paper-writing skills when you need deeper research integration during the writing phase.

---

## Skill Interaction Map

Skills do not run in isolation. Here is how they chain together during common tasks.

**Writing a section:**
```
paper-writing-section (section tips)
  → cw-prose-writing (sentence rhythm)
  → claude-scientific-writer (conventions)
  → academic-paper / composer (quality gates)
  → humanizer (AI pattern check)
  → cw-story-critique (flow check)
```

**Reviewing the paper:**
```
self-review (3-persona review)
  → academic-paper-reviewer (rubrics)
  → humanizer (pattern scan)
  → citation-management (reference check)
  → backward-traceability (claim verification)
```

**Researching a topic:**
```
deep-research (systematic search)
  → literature-search (multi-source)
  → novelty-assessment (gap check)
  → literature-review (organize findings)
```

**Preparing for submission:**
```
paper-assembly (completeness)
  → latex-formatting (format fixes)
  → paper-compilation (build + verify)
  → humanizer (final pass)
  → citation-management (final check)
```
