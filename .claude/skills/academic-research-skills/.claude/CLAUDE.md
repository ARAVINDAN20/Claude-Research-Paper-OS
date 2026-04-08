# Academic Research Skills

A suite of Claude Code skills for rigorous academic research, paper writing, peer review, and pipeline orchestration.

## Skills Overview

| Skill | Purpose | Key Modes |
|-------|---------|-----------|
| `deep-research` v2.5 | 13-agent research team | full, quick, socratic, review, lit-review, fact-check, systematic-review |
| `academic-paper` v2.5 | 12-agent paper writing | full, plan, revision, citation-check, format-convert, bilingual-abstract, writing-polish, full-auto, revision-coach |
| `academic-paper-reviewer` v1.5 | Multi-perspective paper review (5 reviewers + optional cross-model) | full, re-review, quick, methodology-focus, guided |
| `academic-pipeline` v2.8 | Full pipeline orchestrator | (coordinates all above) |

## v3.0 Key Additions

- **Anti-sycophancy protocols**: DA agents score rebuttals 1-5 before conceding. No concession below 4/5. Frame-lock detection.
- **Intent detection**: Socratic Mentor classifies user intent as exploratory vs. goal-oriented. Exploratory mode disables auto-convergence.
- **Cross-model verification** (optional): Set `ARS_CROSS_MODEL` env var to enable GPT-5.4 Pro or Gemini 3.1 Pro as independent second reviewer. See `shared/cross_model_verification.md`.
- **AI Self-Reflection Report**: Pipeline Stage 6 now includes AI behavioral self-assessment (concession rate, health alerts, sycophancy risk rating).

## Routing Rules

1. **academic-pipeline vs individual skills**: academic-pipeline = full pipeline orchestrator (research → write → review → revise → finalize). If the user only needs a single function (just research, just write, just review), trigger the corresponding skill directly without the pipeline.

2. **deep-research vs academic-paper**: Complementary. deep-research = upstream research engine (investigation + fact-checking), academic-paper = downstream publication engine (paper writing + bilingual abstracts). Recommended flow: deep-research → academic-paper.

3. **deep-research socratic vs full**: socratic = guided Socratic dialogue to help users clarify their research question. full = direct production of research report. When the user's research question is unclear, suggest socratic mode.

4. **academic-paper plan vs full**: plan = chapter-by-chapter guided planning via Socratic dialogue. full = direct paper production. When the user wants to think through their paper structure, suggest plan mode.

5. **academic-paper-reviewer guided vs full**: guided = Socratic review that engages the author in dialogue about issues. full = standard multi-perspective review report. When the user wants to learn from the review, suggest guided mode.

## Key Rules

- All claims must have citations
- Evidence hierarchy respected (meta-analyses > RCTs > cohort > case reports > expert opinion)
- Contradictions disclosed with evidence quality comparison
- AI disclosure in all reports
- Default output language matches user input (Traditional Chinese or English)

## Full Academic Pipeline

```
deep-research (socratic/full)
  → academic-paper (plan/full)
    → academic-paper-reviewer (full/guided)
      → academic-paper (revision)
        → academic-paper-reviewer (re-review, max 2 loops)
          → academic-paper (format-convert → final output)
          → AI Self-Reflection Report
```

## Handoff Protocol

### deep-research → academic-paper
Materials: RQ Brief, Methodology Blueprint, Annotated Bibliography, Synthesis Report, INSIGHT Collection

### academic-paper → academic-paper-reviewer
Materials: Complete paper text. field_analyst_agent auto-detects domain and configures reviewers.

### academic-paper-reviewer → academic-paper (revision)
Materials: Editorial Decision Letter, Revision Roadmap, Per-reviewer detailed comments

## Version Info
- **Version**: 3.0
- **Last Updated**: 2026-04-03
- **Author**: Cheng-I Wu
- **License**: CC-BY-NC 4.0
