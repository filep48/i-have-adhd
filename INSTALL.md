# Install i-have-adhd

One skill. Installable in Claude Code, Codex, and Cursor.

## TL;DR

### Claude Code

```bash
claude plugin marketplace add ayghri/i-have-adhd
claude plugin install i-have-adhd@i-have-adhd
```

No local clone needed — Claude Code fetches the repo and keeps it updated.

Open Claude Code, type `/i-have-adhd`.

To disable: `claude plugin disable i-have-adhd` (or `/plugin disable i-have-adhd` from within Claude Code). Re-enable later with `enable` instead of `disable`.

### Codex

```bash
codex plugin marketplace add ayghri/i-have-adhd --ref main
codex plugin add i-have-adhd@i-have-adhd
```

In Codex, type `$i-have-adhd` to request the output style explicitly.

### Cursor

Project (this workspace):

```bash
npx skills add ayghri/i-have-adhd
```

Global (all Cursor projects):

```bash
npx skills add ayghri/i-have-adhd -g
```

Cursor-only (skip other agents):

```bash
npx skills add ayghri/i-have-adhd -a cursor -y
```

Open a new Cursor Agent chat, type `/i-have-adhd`.

<details>
<summary>Manual fallback (clone + copy)</summary>

```bash
mkdir -p ~/.cursor/skills
cp -R /path/to/i-have-adhd/skills/i-have-adhd ~/.cursor/skills/
```

Replace `/path/to/i-have-adhd` with a local clone (`git clone https://github.com/ayghri/i-have-adhd`).

Project-only without the CLI:

```bash
mkdir -p .cursor/skills
cp -R /path/to/i-have-adhd/skills/i-have-adhd .cursor/skills/
```

</details>

## Verify

### Claude Code

```bash
claude plugin list
```

Look for `i-have-adhd  (enabled)`.

### Codex

```bash
codex plugin list
```

Look for `i-have-adhd` in the configured `i-have-adhd` marketplace.

### Cursor

```bash
npx skills list
# or, if installed globally:
npx skills ls -g
```

Look for `i-have-adhd`. Or confirm the file exists: `ls ~/.cursor/skills/i-have-adhd/SKILL.md` (global) / `.cursor/skills/i-have-adhd/SKILL.md` (project).

In a new Agent chat, type `/` and look for `i-have-adhd`.

## Update

### Claude Code

```bash
claude plugin marketplace update i-have-adhd
```

Next Claude Code session picks up changes.

### Codex

```bash
codex plugin marketplace upgrade i-have-adhd
codex plugin remove i-have-adhd
codex plugin add i-have-adhd@i-have-adhd
```

### Cursor

```bash
npx skills update i-have-adhd
# or, if installed globally:
npx skills update -g
```

Start a new Agent chat so Cursor re-reads the skill.

## Uninstall

### Claude Code

```bash
claude plugin uninstall i-have-adhd
claude plugin marketplace remove i-have-adhd
```

### Codex

```bash
codex plugin remove i-have-adhd
codex plugin marketplace remove i-have-adhd
```

### Cursor

```bash
npx skills remove i-have-adhd
# or, if installed globally:
npx skills remove i-have-adhd -g
```

Manual fallback: `rm -rf ~/.cursor/skills/i-have-adhd` or `.cursor/skills/i-have-adhd`.

## Always-on (optional)

### Claude Code

To skip `/i-have-adhd` and apply the rules from message one, add to `~/.claude/CLAUDE.md`:

```markdown
## Output style

Always follow the rules in the `i-have-adhd` skill: action-first, numbered steps, no preamble, no closers, state restated each turn.
```

### Cursor

Add the same text to **Cursor Settings → Rules → User Rules** (applies across projects), or put it in a project rule under `.cursor/rules/` with `alwaysApply: true`.

## On-demand only (optional)

The skill's frontmatter `description` tells the model to apply the rules to *every* message, including ones
where brevity was never requested. If you'd rather it engage only when you ask, replace the `description`
line in `skills/i-have-adhd/SKILL.md` with:

```yaml
description: Action-first, ADHD-friendly output shaping (lead with the next action, numbered steps, no preamble, restate state, suppress tangents, make wins visible). OPT-IN ONLY: use this skill ONLY when the user explicitly asks for it in that message - e.g. "adhd mode", "/i-have-adhd", "short version", "just the steps", "skip the preamble", "tl;dr" - or while the user has explicitly turned the mode on and has not said "stop adhd mode"/"normal mode". Do NOT apply by default. If it was not explicitly requested, respond normally and do not shape or shorten the answer.
```

Then `/i-have-adhd` (or "adhd mode") turns it on for the session, and "stop adhd mode" / "normal mode" turns
it off. Restart Claude Code after editing. The ten rules themselves are unchanged — this only changes *when*
they engage.

## Troubleshooting

**`/i-have-adhd` not in autocomplete.** Restart Claude Code. The plugin index is read at startup.

**`claude plugin marketplace add` fails.** Use the `owner/repo` form: `claude plugin marketplace add ayghri/i-have-adhd`. If you point it at a local path instead, it must be the repo root (the directory containing `.claude-plugin/marketplace.json`), not `.claude-plugin/` itself.

**Skill activates but model still preambles.** Open a new session. Old context may carry. If it still drifts, tighten the rule wording in `skills/i-have-adhd/SKILL.md`, then re-invoke.

**Want different rules.** Fork the repo, edit `skills/i-have-adhd/SKILL.md`, then install your fork: `claude plugin marketplace add <your-username>/i-have-adhd`. (Or clone locally and `claude plugin marketplace add ./i-have-adhd`.) Re-invoke `/i-have-adhd` and the new rules apply.

**Cursor: `/i-have-adhd` missing after install.** Start a new Agent chat. Skills are indexed at session start. Confirm `~/.cursor/skills/i-have-adhd/SKILL.md` exists and that the frontmatter `name` matches the folder name.

**Cursor: skill present but replies still preamble.** Invoke `/i-have-adhd` once in the chat, or use the Always-on User Rule above. Skill auto-invocation is relevance-based; always-on User Rules are stricter.
