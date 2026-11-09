# agent-skills

A collection of skills for coding agents such as Claude Code. Each skill is a
folder with a `SKILL.md` file that tells the agent when and how to do a task.

## Skills

| Skill | Description |
|---|---|
| [one-by-one](skills/one-by-one/SKILL.md) | Work through a list of items one at a time. For each item, give context, a preview, and an opinion, and agree with the user before applying anything. |
| [implicit-state-machines](skills/implicit-state-machines/SKILL.md) | Find flags that together encode one state, check them for bugs, and triage each one: fix now, open an issue, or leave it. |

## Install

Use the [skills CLI](https://github.com/vercel-labs/skills). It installs skills
for Claude Code and other supported agents.

List the skills in this repository:

```bash
npx skills add lmammino/agent-skills --list
```

Install one skill, for example `one-by-one`:

```bash
npx skills add lmammino/agent-skills --skill one-by-one
```

Install it for your user (in all projects) instead of the current project:

```bash
npx skills add lmammino/agent-skills --skill one-by-one --global
```

Install only for Claude Code:

```bash
npx skills add lmammino/agent-skills --skill one-by-one --agent claude-code
```

Install all skills of this repository:

```bash
npx skills add lmammino/agent-skills
```

Update installed skills to the latest version:

```bash
npx skills update
```

### Manual install

To install a skill without the CLI, link its folder into the skills folder of
your agent. For Claude Code:

```bash
git clone https://github.com/lmammino/agent-skills.git
ln -s "$PWD/agent-skills/skills/one-by-one" ~/.claude/skills/one-by-one
```

Then invoke it with `/one-by-one`, or let the agent load it when your request
matches its description.

## Layout

```text
skills/
└── <skill-name>/
    ├── SKILL.md      # frontmatter (name, description) and instructions
    └── references/   # optional files that SKILL.md points to
```

To add a skill, create a folder under `skills/` with a `SKILL.md`, and add a
row to the table above.

## Credits

- `implicit-state-machines`: inspired by
  [a post by @Voxyz_ai](https://x.com/Voxyz_ai/status/2119444845890617507).

## License

[MIT](LICENSE)
