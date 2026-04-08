# Agents Reference

Claude Code agents are markdown files in `.claude/agents/`. Each one defines a specialized role. When you ask Claude something that matches an agent's description, Claude spawns it as a subagent with its own context window and tool access.

You can also invoke agents explicitly by name.

```
@paper-writer Write the methodology section
@paper-reviewer Review section 3
@citation-checker Check all citations
@humanizer-check Run AI pattern scan
@latex-compiler Compile and check the paper
@deep-researcher Find papers on transformer-based TTS
```

---

## paper-writer

**File:** `.claude/agents/paper-writer.md`

**Triggers when you say:** "write," "draft," "rewrite," "improve," or "fix" any section

**What it does:**

1. Reads section-specific tips from `agent-research-skills/skills/paper-writing-section/`
2. Reads your project's `WRITING_RULES.md` if it exists
3. Reads the existing section in `main.tex`
4. Writes or rewrites the section following all mandatory rules
5. Runs the humanizer skill to check for AI patterns
6. Runs the writing quality check
7. Verifies no colons or semicolons in prose
8. Optionally compiles to verify no LaTeX errors

**Rules it enforces (non-negotiable):**

- No colons or semicolons in prose
- No AI vocabulary (25+ banned words)
- No em dashes
- Every technical term explained in plain English before first use
- Every claim hedged with evidence-based language
- Every formula has plain-English intro + "where" clause
- Every number attributed to a source
- Same tone from start to end
- Sentences connect naturally
- Paragraph and sentence length varies

**Output:** A LaTeX fragment that fits into `main.tex`. No `\documentclass`, no preamble.

---

## paper-reviewer

**File:** `.claude/agents/paper-reviewer.md`

**Triggers when you say:** "review," "check," "audit," or "verify"

**What it does:**

This is the most thorough agent. It runs a word-by-word devil's advocate review across 13 quality dimensions.

**The Devil's Advocate Protocol:**

For every line of prose, it asks these questions.

*Term-level:*
- Is this technical term explained before it is used?
- Would a school student understand this word?
- If a number is mentioned, where does it come from?

*Sentence-level:*
- Does this sentence connect to the previous one?
- Is there a jump between ideas?
- Is this claim stated as absolute fact or properly hedged?
- Can I remove this sentence without losing meaning?

*Paragraph-level:*
- Does this paragraph have one clear purpose?
- Does the first sentence tell me what the paragraph is about?
- Does the last sentence lead into the next paragraph?

*Section-level:*
- Does this section tell a story from beginning to end?
- Is the tone consistent with the abstract and introduction?
- Does every table/figure appear near its first reference?

**The 13 Review Dimensions:**

1. Prose quality (writing quality checklist)
2. AI pattern detection (25 flagged terms, em dashes, synonym cycling)
3. Colon/semicolon check (banned in prose)
4. Academic humility (every claim hedged)
5. Formula completeness (full "where" clause)
6. Citation accuracy (automated validation script)
7. Figure/table placement (near first reference)
8. Flow and tone (consistent voice throughout)
9. Repetition (key statistics not overused)
10. Research accuracy (claims match cited sources)
11. Term explanation (every technical term explained)
12. Number attribution (every number has a source)
13. Sentence connection (every sentence links to the previous)

**Output:** A structured report with exact line numbers, problematic text, severity level (Critical/Important/Minor), the devil's advocate question that caught it, and a suggested fix.

---

## citation-checker

**File:** `.claude/agents/citation-checker.md`

**Triggers when you say:** "check citations," "verify references," or "validate bibliography"

**What it does:**

1. Runs the automated citation validation script
   ```bash
   python ~/.claude/skills/agent-research-skills/skills/citation-management/scripts/validate_citations.py --tex main.tex --bib references.bib
   ```
2. For each key numerical claim, verifies the cited source matches
3. Scans all prose for factual/quantitative statements with no `\cite{}` nearby
4. Checks for stale references (broken arXiv IDs, missing authors)

**Output:** A report with three sections.
1. Citation validation results (from script)
2. Uncited claims (line number, text, suggested citation)
3. Reference quality issues (incomplete entries, missing venues)

---

## humanizer-check

**File:** `.claude/agents/humanizer-check.md`

**Triggers when you say:** "humanize," "check AI patterns," "make it sound human," or after any writing session

**What it does:**

Scans the entire paper (or a specific section) for signs of AI-generated text across 5 check categories.

**A. Flagged Terms (25+ words)**
Scans for high-frequency AI vocabulary. For each found, suggests a replacement.

**B. Punctuation Patterns**
- Em dashes in prose (must be 0)
- Semicolons in prose (must be 0)
- Colons in prose (must be 0)

**C. Structural Patterns**
- Rule-of-three forcing (every list has exactly 3 items)
- Uniform paragraph length (all 150-200 words)
- Synonym cycling (calling the same thing by different names)
- Mirror structure (every section has identical internal format)

**D. Tone Patterns**
- Throat-clearing openers ("In the realm of")
- Meta-commentary ("This section will discuss")
- Significance inflation ("stands as," "serves as")
- Copula avoidance ("represents a" where "is a" works)

**E. Sentence Rhythm (Burstiness)**
Flags any 5+ consecutive sentences with similar word count.

**Output:** For each issue found, reports line number, current text, pattern matched, and the fix.

---

## latex-compiler

**File:** `.claude/agents/latex-compiler.md`

**Triggers when you say:** "compile," "build," or "check the paper"

**What it does:**

1. Compiles the paper with Docker (or local pdflatex)
   ```bash
   docker run --rm -v "$(pwd)":/data blang/latex:ubuntu bash -c \
     "cd /data && pdflatex -interaction=nonstopmode main.tex && \
     bibtex main && pdflatex -interaction=nonstopmode main.tex && \
     pdflatex -interaction=nonstopmode main.tex"
   ```
2. Verifies "Output written on main.pdf" appears
3. Checks page count
4. Checks for warnings (undefined references, missing citations)
5. Runs the LaTeX quality checker
   ```bash
   python ~/.claude/skills/agent-research-skills/skills/latex-formatting/scripts/latex_checker.py main.tex
   ```
6. Runs the assembly checker
   ```bash
   python ~/.claude/skills/agent-research-skills/skills/paper-assembly/scripts/assembly_checker.py --dir . --verbose
   ```

**Output:** Compilation status, page count, any warnings or errors, and quality check results.

---

## deep-researcher

**File:** `.claude/agents/deep-researcher.md`

**Triggers when you say:** "research," "find papers," "verify this claim," or "check novelty"

**What it does:**

Searches three academic databases in parallel.

```bash
# Semantic Scholar
python ~/.claude/skills/agent-research-skills/skills/deep-research/scripts/search_semantic_scholar.py \
  --query "QUERY" --max-results 20

# CrossRef
python ~/.claude/skills/agent-research-skills/skills/literature-search/scripts/search_crossref.py \
  --query "QUERY" --rows 10

# arXiv source download
python ~/.claude/skills/agent-research-skills/skills/literature-search/scripts/download_arxiv_source.py \
  --arxiv-id "ARXIV_ID"
```

**Four research modes:**

1. **Claim Verification** -- Given a claim and citation, verify they match
2. **Gap Analysis** -- Search for work that might fill a gap you identified
3. **Novelty Check** -- Search for prior surveys that might overlap with yours
4. **Update Check** -- Search for new papers published since your last review

**Output:** Structured results with title, authors, year, venue, and relevance assessment for each paper found.
