# Contributing

This repo is a [Claude Code](https://claude.com/claude-code) plugin marketplace
named `family-skills`. Skills are grouped into independently-installable plugins:

- **`pregnancy`** — pregnancy companion skills

> Public repo. Keep skills free of personal information — no names, due dates,
> medical history, or anything else that identifies real people.

## Layout

```
.claude-plugin/marketplace.json    # lists the plugins
plugins/
  <bundle>/
    plugin.json                    # { name, version, description }
    skills/
      <skill-name>/
        SKILL.md                   # required
        references/ scripts/ ...   # optional
```

Each plugin auto-discovers **every** skill under its own `skills/` directory.
Do **not** create a shared `skills/` at the repo root: a plugin whose `source`
is the repo root loads the entire tree, which breaks independent installs.

## Keep it publishable

Everything here is public. Keep skills free of personal references — no names,
specific due dates, medical histories, photos, or anything else that identifies
real people. Use generic examples ("the pregnant person", "your partner",
"week 20").

## Not medical advice

Skills in this repo produce general information, not medical advice. Every
skill that touches health topics must:

- frame outputs as general information
- use tentative language for physiological claims
- defer to the user's healthcare provider on anything specific
- refuse to diagnose, prescribe, or recommend supplements/medications
- flag concerning symptoms and direct the user to their provider rather than
  reassure them based on partial information

## Workflows

**Golden rule:** edit skills **in this repo**, never in `~/.claude/skills`. A
copy there shadows the marketplace version.

The marketplace pulls from the **remote**, so always `git push` before running
`/plugin marketplace update`.

### Update an existing skill

1. Edit `plugins/<bundle>/skills/<name>/SKILL.md`.
2. `git commit` + `git push`.
3. In Claude Code:
   ```
   /plugin marketplace update family-skills
   /reload-plugins
   ```

### Add a new skill to an existing bundle

1. Create `plugins/<bundle>/skills/<new-name>/SKILL.md`.
2. `git commit` + `git push`.
3. `/plugin marketplace update family-skills` then `/reload-plugins`.

No `marketplace.json` change and no reinstall — the plugin auto-discovers the
new skill under its `skills/` dir.

### Add a new bundle (new plugin)

1. Create `plugins/<new-bundle>/plugin.json` (`{ name, version, description }`)
   and `plugins/<new-bundle>/skills/<skill>/SKILL.md`.
2. Add an entry to `.claude-plugin/marketplace.json` under `plugins[]`.
3. `git commit` + `git push`.
4. New plugins need an explicit install:
   ```
   /plugin marketplace update family-skills
   /plugin install <new-bundle>@family-skills
   /reload-plugins
   ```

## Install

```
/plugin marketplace add foxtrotcharlie/family-skills
/plugin install pregnancy@family-skills
/reload-plugins
```
