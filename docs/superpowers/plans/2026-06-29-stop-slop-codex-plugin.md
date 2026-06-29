# Stop Slop Codex Plugin Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Convert the existing `stop-slop` repository into a shareable Codex plugin with one Codex-native writing cleanup skill.

**Architecture:** Keep the repository as the plugin root. Add `.codex-plugin/plugin.json`, move the active skill into `skills/stop-slop/`, keep phrase/structure/example references beside that skill, and update README copy so Codex is the primary install path. No hooks, MCP server, app manifest, telemetry, or Claude Code setup is part of v1.

**Tech Stack:** Codex plugin manifest JSON, Codex skill Markdown, `agents/openai.yaml`, existing MIT-licensed Markdown references, plugin validator from `/Users/torachiyouesugi/.codex/skills/.system/plugin-creator/scripts/validate_plugin.py`, skill validator from `/Users/torachiyouesugi/.codex/skills/.system/skill-creator/scripts/quick_validate.py`.

## Global Constraints

- Keep original MIT license and attribution.
- Use in-place conversion inside `/Users/torachiyouesugi/Documents/public/stop-slop`.
- Expose one plugin named `stop-slop`.
- Expose one skill named `stop-slop`.
- Preserve `references/phrases.md`, `references/structures.md`, and `references/examples.md` under the active skill folder.
- Do not add hooks, MCP server, app integration, telemetry, Claude Code config, or heavyweight compaction machinery.
- Rewrite Claude-specific setup language so Codex is the primary path.
- Run plugin and skill validation before handoff.
- Keep edits scoped to plugin conversion files.

---

## File Structure

- Create `.codex-plugin/plugin.json`: Codex plugin manifest with plugin name, version, metadata, skill path, and UI metadata.
- Create `skills/stop-slop/SKILL.md`: Codex-native skill instructions and reference routing.
- Create `skills/stop-slop/agents/openai.yaml`: UI metadata for the skill.
- Move `references/phrases.md` to `skills/stop-slop/references/phrases.md`: phrase-level cleanup reference.
- Move `references/structures.md` to `skills/stop-slop/references/structures.md`: structure-level cleanup reference.
- Move `references/examples.md` to `skills/stop-slop/references/examples.md`: rewrite examples reference.
- Modify `README.md`: explain Codex plugin purpose, layout, install/share usage, and attribution.
- Modify `CHANGELOG.md`: add an entry for the Codex plugin conversion.
- Remove root `SKILL.md`: root-level Claude-style skill is replaced by plugin-scoped `skills/stop-slop/SKILL.md`.
- Remove root `references/`: references live under the active skill folder.

---

### Task 1: Add Codex Plugin Manifest

**Files:**
- Create: `.codex-plugin/plugin.json`

**Interfaces:**
- Consumes: repository root as Codex plugin root.
- Produces: a plugin manifest whose `skills` field points to `./skills/`.

- [ ] **Step 1: Create the manifest directory**

Run:

```bash
mkdir -p .codex-plugin
```

Expected: `.codex-plugin/` exists.

- [ ] **Step 2: Write `.codex-plugin/plugin.json`**

Create `.codex-plugin/plugin.json` with exactly:

```json
{
  "name": "stop-slop",
  "version": "1.0.0",
  "description": "Codex plugin for removing AI writing tells from prose.",
  "author": {
    "name": "Hardik Pandya",
    "url": "https://hvpandya.com"
  },
  "homepage": "https://github.com/hardikpandya/stop-slop",
  "repository": "https://github.com/hardikpandya/stop-slop",
  "license": "MIT",
  "keywords": [
    "writing",
    "editing",
    "prose",
    "codex",
    "ai-writing"
  ],
  "skills": "./skills/",
  "interface": {
    "displayName": "Stop Slop",
    "shortDescription": "Remove AI tells from prose.",
    "longDescription": "Stop Slop helps Codex rewrite, review, and tighten prose by removing predictable AI writing patterns such as throat-clearing, filler, business jargon, false drama, passive phrasing, and formulaic structures.",
    "developerName": "Hardik Pandya",
    "category": "Productivity",
    "capabilities": [
      "Write"
    ],
    "websiteURL": "https://hvpandya.com",
    "defaultPrompt": [
      "Use Stop Slop to tighten this draft.",
      "Review this README for AI writing tells.",
      "Rewrite this paragraph in a direct human voice."
    ],
    "brandColor": "#111827"
  }
}
```

- [ ] **Step 3: Validate manifest JSON syntax**

Run:

```bash
python3 -m json.tool .codex-plugin/plugin.json >/tmp/stop-slop-plugin-json.out
```

Expected: command exits `0` and writes formatted JSON to `/tmp/stop-slop-plugin-json.out`.

- [ ] **Step 4: Commit manifest**

Run:

```bash
git add .codex-plugin/plugin.json
git commit -m "Add Codex plugin manifest"
```

Expected: commit succeeds.

---

### Task 2: Move References Into Plugin Skill Folder

**Files:**
- Create: `skills/stop-slop/references/phrases.md`
- Create: `skills/stop-slop/references/structures.md`
- Create: `skills/stop-slop/references/examples.md`
- Remove: `references/phrases.md`
- Remove: `references/structures.md`
- Remove: `references/examples.md`

**Interfaces:**
- Consumes: current root `references/*.md` files.
- Produces: plugin-scoped reference files at paths used by `skills/stop-slop/SKILL.md`.

- [ ] **Step 1: Create destination directory**

Run:

```bash
mkdir -p skills/stop-slop/references
```

Expected: `skills/stop-slop/references/` exists.

- [ ] **Step 2: Move reference files**

Run:

```bash
git mv references/phrases.md skills/stop-slop/references/phrases.md
git mv references/structures.md skills/stop-slop/references/structures.md
git mv references/examples.md skills/stop-slop/references/examples.md
```

Expected: root `references/` no longer contains those files, and the three files exist under `skills/stop-slop/references/`.

- [ ] **Step 3: Verify no content changed during move**

Run:

```bash
git diff --cached --summary
```

Expected: output shows three renames from `references/` to `skills/stop-slop/references/`.

- [ ] **Step 4: Commit reference move**

Run:

```bash
git add skills/stop-slop/references references
git commit -m "Move Stop Slop references into skill"
```

Expected: commit succeeds.

---

### Task 3: Replace Claude-Style Root Skill With Codex Skill

**Files:**
- Create: `skills/stop-slop/SKILL.md`
- Remove: `SKILL.md`

**Interfaces:**
- Consumes: `skills/stop-slop/references/*.md`.
- Produces: Codex skill named `stop-slop` discoverable through the plugin `skills` path.

- [ ] **Step 1: Remove the root skill file**

Run:

```bash
git rm SKILL.md
```

Expected: root `SKILL.md` is staged for removal.

- [ ] **Step 2: Create `skills/stop-slop/SKILL.md`**

Create `skills/stop-slop/SKILL.md` with exactly:

```markdown
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
```

- [ ] **Step 3: Validate skill frontmatter syntax**

Run:

```bash
python3 /Users/torachiyouesugi/.codex/skills/.system/skill-creator/scripts/quick_validate.py skills/stop-slop
```

Expected: validation exits `0`.

- [ ] **Step 4: Commit Codex skill**

Run:

```bash
git add skills/stop-slop/SKILL.md SKILL.md
git commit -m "Add Codex-native Stop Slop skill"
```

Expected: commit succeeds.

---

### Task 4: Add Skill UI Metadata

**Files:**
- Create: `skills/stop-slop/agents/openai.yaml`

**Interfaces:**
- Consumes: `skills/stop-slop/SKILL.md`.
- Produces: Codex UI metadata for the skill list and skill chip.

- [ ] **Step 1: Create agents metadata directory**

Run:

```bash
mkdir -p skills/stop-slop/agents
```

Expected: `skills/stop-slop/agents/` exists.

- [ ] **Step 2: Create `skills/stop-slop/agents/openai.yaml`**

Create `skills/stop-slop/agents/openai.yaml` with exactly:

```yaml
interface:
  display_name: "Stop Slop"
  short_description: "Remove AI tells from prose."
  brand_color: "#111827"
  default_prompt: "Use $stop-slop to tighten this draft and remove AI writing tells."

policy:
  allow_implicit_invocation: true
```

- [ ] **Step 3: Validate skill metadata still passes**

Run:

```bash
python3 /Users/torachiyouesugi/.codex/skills/.system/skill-creator/scripts/quick_validate.py skills/stop-slop
```

Expected: validation exits `0`.

- [ ] **Step 4: Commit skill UI metadata**

Run:

```bash
git add skills/stop-slop/agents/openai.yaml
git commit -m "Add Stop Slop skill UI metadata"
```

Expected: commit succeeds.

---

### Task 5: Update README For Codex-First Sharing

**Files:**
- Modify: `README.md`

**Interfaces:**
- Consumes: plugin structure from Tasks 1-4.
- Produces: user-facing documentation that describes Codex plugin usage as the primary path.

- [ ] **Step 1: Replace README content**

Replace `README.md` with exactly:

```markdown
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
```

- [ ] **Step 2: Check README for stale Claude-first setup language**

Run:

```bash
rg -n "Claude Code|Claude Projects|Custom instructions|API calls|Add this folder as a skill" README.md
```

Expected: command exits `1` with no matches.

- [ ] **Step 3: Commit README update**

Run:

```bash
git add README.md
git commit -m "Document Codex plugin usage"
```

Expected: commit succeeds.

---

### Task 6: Update Changelog

**Files:**
- Modify: `CHANGELOG.md`

**Interfaces:**
- Consumes: completed plugin conversion changes.
- Produces: a dated changelog entry describing the Codex plugin conversion.

- [ ] **Step 1: Prepend changelog entry**

Insert this section after `# Changelog`:

```markdown
## 2026-06-29

### Changed

- Converted Stop Slop into a Codex plugin with `.codex-plugin/plugin.json`.
- Moved the active skill into `skills/stop-slop/`.
- Added Codex skill UI metadata in `skills/stop-slop/agents/openai.yaml`.
- Updated README guidance so Codex is the primary install and usage path.
```

- [ ] **Step 2: Verify changelog order**

Run:

```bash
sed -n '1,24p' CHANGELOG.md
```

Expected: `2026-06-29` appears before `2026-01-13`.

- [ ] **Step 3: Commit changelog**

Run:

```bash
git add CHANGELOG.md
git commit -m "Record Codex plugin conversion"
```

Expected: commit succeeds.

---

### Task 7: Validate Plugin And Skill

**Files:**
- Validate: `.codex-plugin/plugin.json`
- Validate: `skills/stop-slop/SKILL.md`
- Validate: `skills/stop-slop/agents/openai.yaml`
- Validate: `README.md`

**Interfaces:**
- Consumes: all previous tasks.
- Produces: validation evidence for handoff.

- [ ] **Step 1: Run skill validation**

Run:

```bash
python3 /Users/torachiyouesugi/.codex/skills/.system/skill-creator/scripts/quick_validate.py skills/stop-slop
```

Expected: command exits `0`.

- [ ] **Step 2: Run plugin validation**

Run:

```bash
python3 /Users/torachiyouesugi/.codex/skills/.system/plugin-creator/scripts/validate_plugin.py .
```

Expected: command exits `0`.

- [ ] **Step 3: Check for placeholder text**

Run:

```bash
rg -n "\\[TODO:|TBD|TODO|Claude Code|Claude Projects|Custom instructions|API calls" .codex-plugin skills README.md
```

Expected: command exits `1` with no matches. Historical changelog entries may mention old tool packaging; do not use them as active setup instructions.

- [ ] **Step 4: Check final plugin tree**

Run:

```bash
find .codex-plugin skills -maxdepth 4 -type f | sort
```

Expected output:

```text
.codex-plugin/plugin.json
skills/stop-slop/SKILL.md
skills/stop-slop/agents/openai.yaml
skills/stop-slop/references/examples.md
skills/stop-slop/references/phrases.md
skills/stop-slop/references/structures.md
```

- [ ] **Step 5: Check git status**

Run:

```bash
git status --short
```

Expected: no output.

---

### Task 8: Optional Personal Marketplace Install Entry

**Files:**
- Modify only if the user wants local Codex app sharing links now: `~/.agents/plugins/marketplace.json`

**Interfaces:**
- Consumes: valid local plugin at `/Users/torachiyouesugi/Documents/public/stop-slop`.
- Produces: local personal marketplace entry if the plugin should appear in the Codex app immediately.

- [ ] **Step 1: Ask whether to add/update the personal marketplace**

Ask:

```text
Do you want me to add this plugin to your personal Codex marketplace now, or leave the repo shareable without changing your local marketplace?
```

Expected: user chooses one.

- [ ] **Step 2A: If user chooses marketplace, seed the personal marketplace entry**

Run from `/Users/torachiyouesugi/.codex/skills/.system/plugin-creator`:

```bash
python3 scripts/create_basic_plugin.py stop-slop --with-marketplace --force
```

Expected: `~/.agents/plugins/marketplace.json` contains a `stop-slop` entry and `~/plugins/stop-slop/.codex-plugin/plugin.json` exists. Preserve any existing marketplace `interface.displayName`.

- [ ] **Step 2B: If user chooses no marketplace, skip local marketplace mutation**

Run:

```bash
test -f .codex-plugin/plugin.json
```

Expected: command exits `0`.

- [ ] **Step 3: If marketplace was changed, replace the scaffold with this repository's plugin files**

Run:

```bash
rsync -a --delete --exclude .git /Users/torachiyouesugi/Documents/public/stop-slop/ "$HOME/plugins/stop-slop/"
```

Expected: `~/plugins/stop-slop/.codex-plugin/plugin.json` matches the repository manifest and `~/plugins/stop-slop/skills/stop-slop/SKILL.md` exists.

- [ ] **Step 4: Validate copied plugin if marketplace was changed**

Run:

```bash
python3 /Users/torachiyouesugi/.codex/skills/.system/plugin-creator/scripts/validate_plugin.py "$HOME/plugins/stop-slop"
```

Expected: command exits `0`.

---

## Self-Review Notes

- Spec coverage: Tasks 1-7 cover plugin manifest, skill relocation, references, UI metadata, README, changelog, validation, and scoped cleanup. Task 8 covers optional local marketplace sharing without making it a required v1 plugin feature.
- Placeholder scan: Plan contains no `TBD`, no `[TODO: ...]`, and no "implement later" work.
- Type consistency: Manifest path is `.codex-plugin/plugin.json`; skill path is `skills/stop-slop/SKILL.md`; reference paths match skill routing.
