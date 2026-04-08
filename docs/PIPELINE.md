# The 5-Phase Research Pipeline

This document describes the full autonomous pipeline that produces a submission-ready academic paper from a research topic. Each phase builds on the previous one. Agents within each phase run in parallel where possible.

To run the full pipeline, open Claude Code in your project directory and say:

```
Run the full research pipeline for [YOUR TOPIC]
```

Or run phases individually:

```
Run Phase 1: Research on [YOUR TOPIC]
Run Phase 3: Write the introduction
Run Phase 4: Review the full paper
```

---

## Phase 1: RESEARCH

**Goal:** Build a complete picture of the existing literature, identify gaps, and establish novelty.

**Agents and skills involved:**
- deep-researcher agent (primary)
- literature-search skill
- deep-research skill
- novelty-assessment skill
- github-research skill

**What happens:**

1. **Broad search.** Claude searches Semantic Scholar, arXiv, and CrossRef using multiple query strings derived from your topic. It retrieves the top 50-100 relevant papers.

2. **Filter and categorize.** Results are sorted by relevance, recency, and citation count. Papers are grouped into themes.

3. **Gap analysis.** Claude identifies what has been studied and what has not. It looks for missing experiments, unexamined languages, untested combinations, and open questions.

4. **Novelty check.** Claude searches specifically for prior work that might overlap with the contribution you intend to make. If overlap exists, it suggests how to differentiate.

5. **Research notes.** All findings are compiled into a `research_notes.md` file in your project directory.

**Parallel execution:** The three database searches (Semantic Scholar, arXiv, CrossRef) run simultaneously. Gap analysis and novelty check run after the search results are available.

**Output:** `research_notes.md` with categorized papers, identified gaps, and a novelty assessment.

---

## Phase 2: STRUCTURE

**Goal:** Plan the paper's architecture before any writing begins.

**Agents and skills involved:**
- strategist skill (academic-paper-skills)
- research-planning skill
- paper-writing-section skill (for section-specific tips)

**What happens:**

1. **Outline generation.** Based on the research notes from Phase 1, Claude produces a section-by-section outline. Each section has a one-sentence purpose statement, the key arguments it will make, and the evidence it will cite.

2. **Dependency mapping.** Claude identifies which sections depend on which others. For example, the discussion section depends on both the results and related work. This determines writing order in Phase 3.

3. **Figure and table planning.** Claude identifies where figures and tables will appear, what data they will contain, and which section will reference them first.

4. **Quality checkpoints.** The strategist inserts checkpoint markers in the outline. At each checkpoint, the composer skill will pause and verify that the writing meets quality standards before continuing.

**Output:** A structured outline with section purposes, dependencies, figure/table plan, and quality checkpoints.

---

## Phase 3: WRITING

**Goal:** Draft every section of the paper.

**Agents and skills involved:**
- paper-writer agent (primary)
- composer skill (academic-paper-skills)
- cw-prose-writing skill (creative-writing-skills)
- claude-scientific-writer skill
- humanizer skill
- citation-management skill
- figure-generation and table-generation skills

**What happens:**

1. **Section drafting.** Claude writes sections following the outline from Phase 2. Independent sections (e.g., related work and methodology) can be written in parallel, up to 4 sections at once.

2. **Per-section humanizer pass.** After each section is drafted, the humanizer runs immediately. AI patterns are caught and fixed before they compound.

3. **Citation insertion.** As claims are written, Claude adds `\cite{}` references. The citation-management skill validates each one against `references.bib`. Missing entries are flagged.

4. **Figure and table creation.** When a section references a figure or table, Claude generates it using the figure-generation or table-generation skill. Placement follows Rule 5 (near first reference).

5. **Transition writing.** After all sections are drafted, Claude writes transitions between sections to ensure smooth flow from start to end.

**Writing order:** Introduction first (sets the framing), then methodology, then results, then related work (easier to position after you know your own contribution), then discussion, then conclusion, then abstract last (it summarizes everything).

**Parallel execution:** Up to 4 independent sections can be written simultaneously. The humanizer runs after each section completes. Citation validation runs continuously in the background.

**Output:** Complete `main.tex` with all sections, figures, tables, and citations.

---

## Phase 4: QUALITY

**Goal:** Review, critique, and fix the paper until it meets Q1 journal standards.

**Agents and skills involved:**
- paper-reviewer agent (primary)
- humanizer-check agent
- citation-checker agent
- self-review skill
- academic-paper-reviewer skill
- cw-story-critique skill
- backward-traceability skill

**What happens:**

1. **Devil's advocate review.** The paper-reviewer agent reads every line of the paper and asks its 13-dimension review questions. Issues are categorized as Critical, Important, or Minor.

2. **AI pattern scan.** The humanizer-check agent runs a full scan of the paper across all 25 pattern categories. This is independent of the per-section humanizer passes in Phase 3.

3. **Citation validation.** The citation-checker agent runs the automated validation script and manually cross-checks key claims.

4. **Simulated peer review.** The self-review skill runs three simulated reviewers with different perspectives (methodological, theoretical, practical). Each produces a NeurIPS-format review with scores.

5. **Fix cycle.** Claude fixes all Critical issues immediately. Important issues are fixed in a second pass. Minor issues are listed for your review.

6. **Second humanizer pass.** After fixes are applied, the humanizer runs again to catch any new AI patterns introduced during the fix cycle.

**Parallel execution:** The paper-reviewer, humanizer-check, and citation-checker all run simultaneously. The self-review runs after fixes are applied.

**Output:** A reviewed and fixed paper with a quality report showing what was found and what was changed.

---

## Phase 5: COMPILE AND SUBMIT

**Goal:** Produce a clean PDF and verify submission readiness.

**Agents and skills involved:**
- latex-compiler agent (primary)
- paper-assembly skill
- latex-formatting skill
- humanizer skill (final pass)

**What happens:**

1. **LaTeX compilation.** The latex-compiler agent builds the paper. If errors occur, it fixes them and rebuilds.

2. **Format check.** The latex-formatting skill verifies that the paper meets the target venue's formatting requirements (page count, margins, font sizes, reference style).

3. **Assembly check.** The paper-assembly skill verifies completeness. Are all sections present? Do all figures compile? Are all references resolved?

4. **Final humanizer pass.** One last scan for AI patterns. This catches anything that slipped through earlier passes or was introduced during LaTeX fixes.

5. **Page count and sanity check.** Claude verifies the PDF has the expected number of pages and that the content looks correct.

**Output:** A clean `main.pdf` ready for submission, plus a compilation report.

---

## Running Individual Phases

You do not have to run all five phases. Each phase works independently if its inputs exist.

| Phase | Prerequisite | Command |
|---|---|---|
| Phase 1: Research | None | "Research [TOPIC] and compile notes" |
| Phase 2: Structure | research_notes.md (or your own notes) | "Create an outline for the paper" |
| Phase 3: Writing | An outline (or just start writing) | "Write the [SECTION]" |
| Phase 4: Quality | A draft in main.tex | "Review the full paper" |
| Phase 5: Compile | main.tex with content | "Compile and check the paper" |

---

## Customizing the Pipeline

### Changing the target venue

Edit `.claude/CLAUDE.md` and update the `target_venue` and `compile_command` fields. The paper-writer agent adapts its style to the venue (IEEE journals get different formatting than NeurIPS or ACL).

### Adding extra review rounds

After Phase 4, you can say "Run Phase 4 again" as many times as needed. Each pass catches different issues because the fixes from the previous pass change the text.

### Skipping the humanizer

Not recommended. But if you want raw speed over polished prose, you can say "Write the introduction without humanizer checks." The per-section humanizer passes will be skipped, though the Phase 4 full-paper scan still runs.

### Using your own LaTeX template

The system works with any LaTeX template. Just place your `main.tex` in the project directory and update the compile command in `.claude/CLAUDE.md`. The writing and review agents work on the content regardless of formatting.
