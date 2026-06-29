# Stop Slop

Stop Slop is a Codex plugin for removing AI writing tells from prose.

It helps Codex tighten drafts, README copy, documentation, PR descriptions, release notes, blog posts, architecture explanations, and marketing copy without flattening the author's meaning or technical accuracy.

## What It Catches

- Throat-clearing openers such as "Here's the thing" and "It turns out".
- Filler, weak intensifiers, softeners, and business jargon.
- Formulaic structures such as "not X, but Y" and setup/reveal pivots.
- Dramatic fragmentation and manufactured punch lines.
- Passive or actorless phrasing when the actor is known.
- Vague claims that announce importance without naming the specific thing.

## Codex Plugin Structure

```text
stop-slop/
├── .codex-plugin/
│   └── plugin.json
├── skills/
│   └── stop-slop/
│       ├── SKILL.md
│       ├── agents/
│       │   └── openai.yaml
│       └── references/
│           ├── phrases.md
│           ├── structures.md
│           └── examples.md
├── README.md
├── CHANGELOG.md
└── LICENSE
```

## Usage

After installing the plugin in Codex, ask for it directly:

```text
Use $stop-slop to tighten this README section.
```

```text
Use $stop-slop to review this PR description for AI writing tells.
```

```text
Use $stop-slop to rewrite this architecture explanation in a direct human voice.
```

Codex can also invoke the skill when a prose editing request clearly matches the skill description.

## References

The skill uses progressive disclosure:

- `references/phrases.md` for phrase-level cleanup.
- `references/structures.md` for structural cleanup.
- `references/examples.md` for before/after calibration.

Codex should load only the reference files needed for the current task.

## Scoring

When asked for a score, Stop Slop rates prose 1-10 on:

| Dimension | Question |
|---|---|
| Directness | Does it state the point without announcements? |
| Rhythm | Does sentence length vary naturally? |
| Trust | Does it respect reader intelligence? |
| Authenticity | Does it sound like a person wrote it? |
| Density | Can any words be cut without losing meaning? |

Below 35/50 means revise before delivery.

## Design Choice

The original Stop Slop rules are intentionally sharp. This Codex plugin keeps that pressure but avoids blind deletion rules that can harm technical prose. For example, it treats adverbs and passive voice as signals to inspect, not automatic errors.

## Author

[Hardik Pandya](https://hvpandya.com)

## License

MIT. Use freely, share widely.
