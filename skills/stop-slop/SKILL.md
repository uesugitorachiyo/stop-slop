---
name: stop-slop
description: Remove AI writing tells from prose. Use when Codex is drafting, editing, reviewing, tightening, humanizing, or de-slopping README copy, documentation, PR descriptions, release notes, blog posts, architecture explanations, marketing copy, or any prose that should sound direct and human.
---

# Stop Slop

Remove predictable AI writing patterns while preserving the user's meaning, facts, and intended tone.

## Workflow

1. Identify the user's target: rewrite, diagnostic review, or draft from scratch.
2. Preserve factual content, product names, technical terms, citations, and required structure.
3. Cut throat-clearing, filler, business jargon, false drama, and formulaic AI structures.
4. Prefer specific actors, concrete nouns, and active verbs.
5. Vary rhythm without adding flourish.
6. Return the edited prose first. Add a short note only when it helps the user understand important tradeoffs.

## Reference Routing

- Read `references/phrases.md` when phrase-level cleanup is needed: filler, jargon, adverbs, openers, meta-commentary, vague declaratives.
- Read `references/structures.md` when structure-level cleanup is needed: binary contrasts, negative listing, dramatic fragmentation, passive voice, false agency, rhythm patterns.
- Read `references/examples.md` when the user asks for examples or when a rewrite needs before/after calibration.

Load only the reference files needed for the task.

## Editing Rules

- Keep the user's point. Do not invent examples, claims, metrics, sources, or intent.
- Replace vague claims with specific claims already present in the source.
- Convert passive voice to active voice when the actor is known.
- Keep passive voice when the actor is unknown, irrelevant, legally necessary, or more precise.
- Treat adverbs as review pressure, not a blind ban. Remove weak intensifiers such as "really", "just", "actually", and "simply" when they add no meaning.
- Avoid em dashes unless the user requested that style or the surrounding text already relies on them.
- Avoid "not X, but Y" pivots when a direct statement works.
- Avoid punchy one-line endings unless the source genuinely earns them.
- Preserve code blocks, command snippets, tables, frontmatter, links, and Markdown structure unless the user asks to rewrite them.

## Output Modes

For a rewrite request:

1. Return the revised text.
2. If useful, add `Notes:` with up to three concise bullets explaining major changes.

For a diagnostic review:

1. Group findings under `Phrases`, `Structures`, `Rhythm`, and `Specificity`.
2. Quote only short snippets.
3. Suggest direct replacements.

For a draft-from-scratch request:

1. Draft in a direct voice.
2. Avoid announcing the structure of the answer.
3. Keep the prose proportional to the user's requested format and audience.

## Scoring

When the user asks for a score, rate 1-10 on each dimension:

| Dimension | Question |
|---|---|
| Directness | Does it state the point without announcements? |
| Rhythm | Does sentence length vary naturally? |
| Trust | Does it respect reader intelligence? |
| Authenticity | Does it sound like a person wrote it? |
| Density | Can any words be cut without losing meaning? |

Below 35/50 means revise before delivery.
