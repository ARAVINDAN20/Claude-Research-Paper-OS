# Claude Research Paper OS -- Master Configuration

> Drop this `.claude/` folder into any project directory. Claude Code auto-reads this file on every session start, which activates all agents, skills, and writing rules below.

## What This Does

This configuration turns Claude Code into a full academic research and paper-writing system. It provides 6 specialized agents, 47+ skills across 8 skill packs, and an orchestrated pipeline that takes you from literature search to submission-ready manuscript.

## How Auto-Calling Works

Claude Code reads this `CLAUDE.md` on session start. The **Skill Auto-Trigger Map** below tells Claude which skills to invoke based on what you ask it to do. You do not need to call skills manually. Just say "write the introduction" or "check citations" and Claude matches your request to the right agents and skills.

For a full autonomous run, say:
```
Run the full research pipeline for [YOUR TOPIC]
```
Claude will execute the 5-phase pipeline described in docs/PIPELINE.md.

---

## Paper Configuration (Edit These for Your Paper)

```yaml
# Change these to match your paper
title: "Your Paper Title Here"
authors: "Author One, Author Two"
target_venue: "IEEE Access"  # or "NeurIPS", "ACL", "ICML", etc.
format: "IEEEtran"           # LaTeX document class
compile_command: >
  docker run --rm -v "$(pwd)":/data blang/latex:ubuntu bash -c
  "cd /data && pdflatex -interaction=nonstopmode main.tex &&
  bibtex main && pdflatex -interaction=nonstopmode main.tex &&
  pdflatex -interaction=nonstopmode main.tex"
```

## File Structure (Expected)

```
your-project/
  main.tex              <- Your paper
  references.bib        <- Bibliography
  figures/              <- Figures
  WRITING_RULES.md      <- Writing constraints (optional, see templates/)
  .claude/
    CLAUDE.md           <- This file
    agents/             <- 6 specialized agents
    skills/             <- 8 skill packs, 47+ skills
```

---

## MANDATORY WRITING RULES

These rules override all default Claude behavior. Every agent and skill follows them.

### Rule 0: Story Flow
Every section flows like a story. Before using ANY technical term, explain what it is in plain English. The paper should be accessible from school students to PhD scholars.

### Rule 1: No Colons or Semicolons in Prose
Never use `:` or `;` in running text. Use periods, commas, or connecting words instead. Colons and semicolons are allowed in LaTeX commands, tables, and code blocks.

### Rule 2: Academic Humility
Never state claims as absolute fact. Use hedged language grounded in evidence.
- Good: "our analysis suggests," "our survey found," "based on our examination"
- Bad: "this proves," "clearly shows," "it is well known that"

### Rule 3: No AI Writing Patterns
The following words and phrases are banned in all prose output.

**Banned words:** landscape (abstract), nascent, pivotal, crucial, testament, showcase, foster, enhance, delve, serves as, stands as, represents a, Additionally, Furthermore, Moreover, Notably, vibrant, tapestry, underscore, intricate, robust, holistic, groundbreaking, cutting-edge, cornerstone, paradigm shift, synergy, streamline, multifaceted, nuanced, comprehensive, leverage, realm, embark, navigate

**Banned patterns:** em dashes, rule-of-three forcing, synonym cycling, throat-clearing openers ("In the realm of," "It is important to note"), meta-commentary ("This section will discuss"), significance inflation, generic positive conclusions ("the future looks bright")

### Rule 4: Formula Explanations
Every formula needs a plain-English introduction explaining what it computes, followed by a "where" clause defining EVERY symbol used.

### Rule 5: Tables and Figures Near References
Every table and figure must appear on the same page as or immediately after its first text reference.

---

## SKILL AUTO-TRIGGER MAP

Claude reads this map and automatically invokes the right skills based on what you ask.

### When Writing or Rewriting ANY Section
1. **paper-writing-section** -- Section-specific writing guidance
2. **humanizer** -- Anti-AI pattern check on every paragraph
3. **academic-paper (writing quality check)** -- Quality rules from academic-research-skills
4. **cw-prose-writing** -- Narrative flow, sentence rhythm, natural prose style
5. **cw-story-critique** -- Critique flow, pacing, and readability
6. **claude-scientific-writer** -- Scientific writing conventions
7. **academic-paper-skills (strategist + composer)** -- Planning and writing with quality checkpoints

### When Writing the Abstract
1. **paper-writing-section abstract** -- Abstract-specific guidance
2. Follow venue-specific abstract rules (word count, structure)

### When Writing Related Work or Taxonomy
1. **related-work-writing** -- Thematic organization, compare-and-contrast
2. **survey-generation** -- Subsection writing with citation validation
3. **literature-search** -- Verify claims against Semantic Scholar, arXiv, CrossRef

### When Working with Formulas
1. **math-reasoning** -- Derivations, notation tables, statistical test selection
2. Always apply Rule 4 (full notation explanation)

### When Working with Citations
1. **citation-management** -- BibTeX validation, deduplication, auto-fix
2. Run validation script (see Quick Commands below)

### When Working with Figures or Tables
1. **figure-generation** -- Publication-quality matplotlib figures
2. **table-generation** -- LaTeX booktabs tables
3. Always keep near first reference (Rule 5)

### When Checking LaTeX
1. **latex-formatting** -- Formatting fixes, pre-submission checks
2. **paper-compilation** -- pdflatex + bibtex pipeline with auto-fix

### Before Submission (Review Phase)
1. **self-review** -- 3-persona automated review
2. **academic-paper-reviewer** -- Multi-perspective peer review with 0-100 rubrics
3. **humanizer** -- Final AI pattern removal pass
4. **citation-management validate** -- Final citation check
5. **paper-assembly** -- End-to-end pipeline check

### For Deep Research Questions
1. **deep-research** -- 6-phase systematic literature survey
2. **literature-search** -- Multi-source academic search
3. **novelty-assessment** -- Check if claims are novel

---

## QUICK COMMANDS

```bash
# Validate all citations
python ~/.claude/skills/agent-research-skills/skills/citation-management/scripts/validate_citations.py --tex main.tex --bib references.bib

# Check LaTeX formatting
python ~/.claude/skills/agent-research-skills/skills/latex-formatting/scripts/latex_checker.py main.tex

# Clean LaTeX artifacts
python ~/.claude/skills/agent-research-skills/skills/latex-formatting/scripts/clean_latex.py .

# Search Semantic Scholar
python ~/.claude/skills/agent-research-skills/skills/deep-research/scripts/search_semantic_scholar.py --query "YOUR QUERY" --max-results 20

# Search CrossRef
python ~/.claude/skills/agent-research-skills/skills/literature-search/scripts/search_crossref.py --query "YOUR QUERY" --rows 10

# Check paper assembly
python ~/.claude/skills/agent-research-skills/skills/paper-assembly/scripts/assembly_checker.py --dir . --verbose
```

---

## WRITING QUALITY CHECKLIST

Run this before finishing any section.

- [ ] Zero flagged AI terms (see Rule 3)
- [ ] Zero em dashes in prose
- [ ] Zero colons or semicolons in prose
- [ ] No throat-clearing openers
- [ ] No meta-commentary
- [ ] No rule-of-three forcing (use 2 or 4 or 5 points if the evidence warrants)
- [ ] Paragraph length varies (not all 150-200 words)
- [ ] No synonym cycling (one term per concept, stick with it)
- [ ] Sentence length varies (no 5+ consecutive same-length sentences)
- [ ] All claims hedged with evidence-based language
- [ ] All formulas have full notation explanation
- [ ] All numbers attributed to sources
- [ ] Every technical term explained before first use
