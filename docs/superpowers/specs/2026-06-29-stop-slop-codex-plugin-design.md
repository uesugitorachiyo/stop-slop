# Stop Slop Codex Plugin Design

Date: 2026-06-29

## Goal

Convert the existing `stop-slop` Claude-oriented skill repository into a shareable Codex plugin while preserving the original purpose: help Codex remove predictable AI writing patterns from prose.

The result should feel native to Codex, install cleanly as a plugin, and remain lightweight enough that the skill helps writing instead of flooding context.

## Scope

This conversion will happen in place inside the existing repository.

Included:

- Add Codex plugin metadata under `.codex-plugin/plugin.json`.
- Move the active skill into `skills/stop-slop/`.
- Preserve the reference files for phrases, structures, and examples.
- Add Codex UI metadata under `skills/stop-slop/agents/openai.yaml`.
- Rewrite `SKILL.md` for Codex usage and triggering.
- Update `README.md` with Codex plugin install/share guidance.
- Keep original MIT license and attribution.
- Validate the plugin and skill before handoff.

Excluded:

- No hooks, MCP server, or app integration in v1.
- No automatic rewriting of every response.
- No hosted service or telemetry.
- No Claude Code configuration files.
- No heavyweight compaction or token-optimization machinery.

## Architecture

The plugin will use the standard Codex plugin layout:

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

`plugin.json` is the installable plugin manifest. The plugin exposes one skill, `stop-slop`.

The skill body will contain only the core workflow and reference-routing rules. Detailed phrase lists, structural patterns, and examples stay in `references/` and load only when relevant.

## Skill Behavior

The Codex skill should trigger when the user asks Codex to draft, edit, review, tighten, humanize, de-slop, or remove AI tells from prose. It should also help with README copy, documentation prose, PR descriptions, release notes, blog posts, architecture explanations, and marketing copy.

The skill will tell Codex to:

- Preserve the user's meaning, facts, and intended tone.
- Remove throat-clearing, filler, business jargon, false drama, and formulaic AI structures.
- Prefer specific actors and concrete nouns.
- Use active voice where it improves clarity.
- Avoid em dashes unless the user or target style clearly wants them.
- Avoid absolute bans that would harm technical precision.
- Return edited prose first, then a short note only when useful.

The original rules are strong and useful, but several are too absolute for Codex documentation work. For example, "kill all adverbs" and "no passive voice" can damage technical prose. The Codex version should treat those as review pressure, not blind deletion rules.

## Data Flow

1. User asks Codex to draft or edit prose.
2. Codex loads the `stop-slop` skill.
3. Codex reads only the references needed for the task:
   - `phrases.md` for phrase-level cleanup.
   - `structures.md` for structural cleanup.
   - `examples.md` when examples would guide a rewrite.
4. Codex rewrites or reviews the prose.
5. Codex returns polished text with minimal commentary.

## Error Handling

If the source text is ambiguous, Codex should preserve ambiguity instead of inventing meaning.

If strict anti-slop rules conflict with technical accuracy, product naming, legal language, or a requested brand voice, Codex should keep accuracy and explain the tradeoff briefly.

If the user asks for a diagnostic review instead of a rewrite, Codex should return concise findings grouped by phrase, structure, rhythm, and specificity.

## Testing And Validation

Validation will include:

- Run the Codex plugin validator on the repository.
- Run the skill validator on `skills/stop-slop`.
- Inspect manifest and skill metadata for placeholder text.
- Confirm README install/share instructions match Codex plugin packaging.
- Confirm old Claude-specific setup language no longer describes the primary path.

## Acceptance Criteria

- Repository contains a valid Codex plugin manifest.
- Plugin exposes one skill named `stop-slop`.
- Skill metadata is concise and trigger-rich.
- References are preserved under the skill folder.
- README explains Codex-first usage.
- No Claude Code setup is required.
- Validation commands pass.
- Git diff is scoped to the plugin conversion.
