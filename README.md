# skills

A personal collection of AI agent skills, shareable across [Claude Code](https://docs.claude.com/en/docs/claude-code) and [Codex CLI](https://github.com/openai/codex).

Each skill lives under [skills/](skills/) as a directory containing a `SKILL.md` (with YAML frontmatter declaring `name` and `description`) plus any supporting assets the skill needs at runtime.

## Layout

```text
skills/
  <skill-name>/
    SKILL.md          # frontmatter + instructions the agent reads
    <assets>          # templates, scripts, references — optional
```

## Available skills

- [tech-report-html](skills/tech-report-html/) — Convert a technical markdown doc into a polished single-page HTML report (hero, cards, callouts, Mermaid, TOC).

## Installing skills

Skills are loaded from per-tool directories. The simplest workflow is to symlink an individual skill into the tool's skills directory so edits in this repo are picked up immediately.

### Claude Code

User-scope skills live in `~/.claude/skills/`. Symlink the skill directory:

```sh
ln -s "$(pwd)/skills/tech-report-html" ~/.claude/skills/tech-report-html
```

For project-scope skills, symlink into `<project>/.claude/skills/` instead.

### Codex CLI

Codex reads skills from `~/.codex/skills/` (user) or `<project>/.codex/skills/` (project):

```sh
mkdir -p ~/.codex/skills
ln -s "$(pwd)/skills/tech-report-html" ~/.codex/skills/tech-report-html
```

### Install all skills

```sh
for dir in skills/*/; do
  name="$(basename "$dir")"
  ln -sfn "$(pwd)/$dir" ~/.claude/skills/"$name"
  ln -sfn "$(pwd)/$dir" ~/.codex/skills/"$name"
done
```

## Adding a new skill

1. Create `skills/<skill-name>/SKILL.md` with frontmatter:

   ```yaml
   ---
   name: <skill-name>
   description: <one-paragraph trigger description — what it does and when to invoke it>
   ---
   ```

2. Write the body as instructions the agent will follow when the skill fires.
3. Drop any supporting files (templates, reference docs, scripts) alongside `SKILL.md`.
4. Add a bullet to the **Available skills** list above.
5. Re-run the install command (or `ln -s` the new directory) so the tools pick it up.

## Notes

- `name` in frontmatter must match the directory name.
- The `description` is what the agent matches against when deciding to invoke — be specific about triggers, not just capability.
- Keep skills self-contained: anything the skill needs at runtime should live inside its directory.
