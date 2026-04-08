# Writing Rules

Copy this file to your project root and edit it for your paper. The paper-writer agent reads this file before writing any section.

---

## Rule 0: Story Flow
Every section flows like a story. Before using ANY technical term, explain what it is in plain English. Your paper should be accessible to readers outside your exact subfield.

## Rule 1: No Colons or Semicolons
Never use `:` or `;` in running prose. Use periods, commas, dashes, or connecting words. Colons are allowed in LaTeX commands, code, and table headers.

## Rule 2: Academic Humility
Never state claims as absolute fact unless citing a mathematical proof. Use hedged, evidence-based language.
- "our analysis suggests"
- "our experiments indicate"
- "based on our examination"
- "the results appear to show"

## Rule 3: No AI Writing Patterns
Banned words and phrases (see CLAUDE.md for the full list). No em dashes. No rule-of-three forcing. No synonym cycling.

## Rule 4: Explain Every Formula
Every equation needs a plain-English introduction explaining what it computes, followed by a "where" clause defining every symbol.

Example:
"The cosine similarity between two speaker embeddings measures how closely the cloned voice matches the original. It is computed as
[equation]
where $\mathbf{a}$ is the embedding of the original speaker, $\mathbf{b}$ is the embedding of the cloned output, and $\|\cdot\|$ denotes the L2 norm."

## Rule 5: Figures and Tables Near References
Every figure and table must appear on the same page as or immediately after its first text reference. Never place a figure more than one page away from where it is first mentioned.

## Rule 6: One Term Per Concept
Pick one name for each concept and use it consistently throughout. Do not alternate between "model," "system," "architecture," and "framework" for the same thing.

## Rule 7: Sentence and Paragraph Variation
Vary sentence length deliberately. Mix short declarative sentences with longer explanatory ones. Vary paragraph length too. A one-sentence paragraph is fine for emphasis. A half-page paragraph is fine for complex arguments.

## Rule 8: Connect Every Sentence
Every sentence must connect to the one before it. Use explicit connectors (however, because, therefore, building on this, by contrast) or implicit ones (starting from the endpoint of the previous sentence). No jumping between ideas.

## Rule 9: Attribute Every Number
Every statistic, measurement, or count must have a citation or an explicit statement that it comes from your own analysis. "MOS 4.53" needs a source. "We counted 40 models" is self-attributed.

## Rule 10: Same Tone Throughout
The voice in the abstract should sound like the voice in the conclusion. Do not shift between formal and conversational. Do not shift between cautious and assertive. Pick a register and hold it.

## Rule 11: No Padding
If a sentence can be removed without losing meaning, remove it. If a paragraph repeats what the previous paragraph said, merge or delete. Shorter papers that say the same thing are always better.
