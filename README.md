# pptxgen-ts-starter-skill

English | [中文](./README_CN.md)

An **Agent Skill** that turns *“make me a PPT”* into a working
[**pptxgen-ts-starter**](https://github.com/zythum/pptxgen-ts-starter) project —
then hands off to that project's own skills.

## What it does

One job: **scaffold → install → hand off.**

| Step | Action |
| ---- | ------ |
| 0 | Detect an existing `pptxgen-ts-starter` project — never re-scaffold |
| 1 | `npx tiged zythum/pptxgen-ts-starter <dir>` |
| 2 | `npm install` |
| 3 | *(optional)* bridge the project's `.agents/skills/` to your agent |
| 4 | Read the project's `README.md` / `AGENTS.md` / `.agents/skills/` and run its deck workflow |

It deliberately does **not** design slides. Content, layout, colour and QA live
inside the scaffolded project — so this skill can never drift away from them.

## Install

```bash
npx skills add zythum/pptxgen-ts-starter-skill
```

Pick the agent and scope (project or global) when prompted. Nothing else to
configure — after that, a request like *“做个 PPT”* or *“make a deck about X”*
triggers it.

Non-interactive:

```bash
npx skills add zythum/pptxgen-ts-starter-skill -g -y                   # global, auto-detect agent
npx skills add zythum/pptxgen-ts-starter-skill -g -y -a claude-code    # global, Claude Code
```

## Requirements

- Node.js + npm
- Network access for `npx tiged` and `npm install`
- `git` — only used by the `git clone` fallback

Declared in the skill frontmatter under `metadata.requires.bins`.

## Related

- **[pptxgen-ts-starter](https://github.com/zythum/pptxgen-ts-starter)** — the
  template this skill scaffolds. Its own skills (`design`, `pptxgenjsx`) do the
  real work once the project exists.

## Repository layout

```
skills/
└── pptxgen-ts-starter/
    └── SKILL.md        # the only skill here — the front door
```

The skill sits in the root-level `skills/` container so that
`npx skills add zythum/pptxgen-ts-starter-skill` discovers exactly one skill.

## Maintaining this skill

- `name` stays lowercase, hyphenated, and equal to the directory name.
- `description` is the **trigger surface** — the only text an agent sees before
  deciding to load the skill. Keep the wording that matches how users actually
  ask, in English *and* Chinese (PPT, PowerPoint, 幻灯片, 演示文稿, deck…).
- Keep the template URL in Step 1 correct. Renaming the template repo requires
  editing this file — that URL is the skill's only hard coupling.
- If this skill ever needs to *describe* the template's internals rather than
  point at them, move it back into the template repo.
  **What describes the inside must live with the inside.**

## License

MIT
