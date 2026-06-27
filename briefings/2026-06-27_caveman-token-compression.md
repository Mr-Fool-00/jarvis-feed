# Caveman Skill — 65% Output Token Reduction — 7/10

## What it is
A CLAUDE.md-based behavioral skill from Julius Brussee that enforces terse, direct output by adding a single focused instruction block to your CLAUDE.md. No tool calls, no code, no network access — purely a behavioral prompt that instructs the model to respond in minimal, complete chunks rather than padded paragraphs. 77K GitHub stars. The author benchmarked 200 technical tasks: median 65% output token reduction, 80%+ on verbose tasks (tutorials, explanation-heavy code reviews), ~20% on factual one-shot queries. The benchmark compared identical CC sessions with and without the skill; answer accuracy was not degraded.

## Why you'd want it (specific to your stack)
Jarvis discovery runs burn meaningful tokens on narration and preamble — "let me now look at...", "I'll check...", "Here's what I found:" followed by a line of actual content. A 65% output reduction on a full discovery run would be material. The chapter writer skill has the same pattern: prose wrapping the actual draft content. The brevity skill doesn't change what the agent does; it changes how much it talks about doing it.

## Why I think it's worth your attention
This is Type B third-party code (CLAUDE.md only — no tool calls, no network, no shell execution). The risk profile is the lowest possible category: you read the instruction text before adding it, nothing executes. The 77K star signal suggests the core instruction is genuinely effective, not just popular for hype reasons. Build your own version rather than copy-pasting — write one instruction block you've reasoned about and tested, rather than importing an opinionated block you haven't read.

## What to do
1. Read the source CLAUDE.md at the GitHub link below — understand exactly what behavioral instructions it encodes.
2. Write your own brevity block (3–5 lines): direct response, no preamble, no "certainly", minimal narration between actions, complete sentences only when the answer requires explanation.
3. Add to Jarvis CLAUDE.md and run one full discovery loop, comparing output token counts against a prior run.
4. If the reduction holds, add a version to the chapter writer CLAUDE.md with a carve-out for actual draft prose (you want brevity in the *meta* commentary, not in the novel itself).
5. Do not copy-paste the Caveman CLAUDE.md verbatim — read it, internalize the principle, write the block from scratch in your own terms.

🔗 https://github.com/julius-brussee/caveman-skill
