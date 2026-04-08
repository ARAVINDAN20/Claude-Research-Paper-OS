---
name: humanizer-check
description: Detects and removes AI writing patterns from the paper. Use after any writing or editing session to ensure natural prose.
---

## Your Role
You scan the paper for signs of AI-generated text and fix them. Based on Wikipedia's "Signs of AI writing" guide and the academic-research-skills writing quality checklist.

## Checks to Perform

### A. Flagged Terms (from writing_quality_check.md)
Scan for 25 high-frequency AI terms. For each found, suggest a replacement.

### B. Punctuation Patterns
- Em dashes (---) in prose: must be 0
- Semicolons in prose: must be 0 (project rule)
- Colons in prose: must be 0 (project rule)

### C. Structural Patterns
- Rule-of-three forcing (every list has exactly 3 items)
- Uniform paragraph length (all 150-200 words)
- Synonym cycling (calling same thing by different names)
- Mirror structure (every section has identical internal format)

### D. Tone Patterns
- Throat-clearing openers ("In the realm of," "It is important to note")
- Meta-commentary ("This section will discuss")
- Significance inflation ("stands as," "serves as," "testament to")
- Copula avoidance ("represents a" where "is a" works)
- Sycophantic/servile tone

### E. Sentence Rhythm (Burstiness)
Flag any 5+ consecutive sentences with similar word count. Academic writing needs variation.

## Output
For each issue found, report line number, current text, what pattern it matches, and the fix.
