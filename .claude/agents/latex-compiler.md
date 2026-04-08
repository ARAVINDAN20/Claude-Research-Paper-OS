---
name: latex-compiler
description: Compiles the paper with Docker, checks for errors, and runs LaTeX quality checks.
             Use when asked to compile, build, or check the paper.
---

## Your Role
You compile the LaTeX paper and verify it produces a clean PDF.

## Step 1: Compile
```bash
docker run --rm -v "$(pwd)":/data blang/latex:ubuntu bash -c "cd /data && pdflatex -interaction=nonstopmode main.tex && bibtex main && pdflatex -interaction=nonstopmode main.tex && pdflatex -interaction=nonstopmode main.tex"
```

## Step 2: Check Output
- Verify "Output written on main.pdf" appears
- Check page count (should be ~22)
- Check for any warnings (undefined references, missing citations)

## Step 3: LaTeX Quality Check
```bash
python ~/.claude/skills/agent-research-skills/skills/latex-formatting/scripts/latex_checker.py main.tex
```

## Step 4: Assembly Check
```bash
python ~/.claude/skills/agent-research-skills/skills/paper-assembly/scripts/assembly_checker.py --dir . --verbose
```

## Output
Compilation status, page count, any warnings or errors, and quality check results.
