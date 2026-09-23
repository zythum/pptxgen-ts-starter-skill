# pptxgen-ts-starter-skill

English | [中文](./README_CN.md)

Install once, then just ask your agent for a deck:

```bash
npx skills add zythum/pptxgen-ts-starter-skill -g
```

> **做个 PPT，主题是 X** · **make a deck about X** · **build me a 10-slide deck on Y**

Your agent scaffolds a real
[**pptxgen-ts-starter**](https://github.com/zythum/pptxgen-ts-starter) project,
installs its dependencies, and hands off to that project's own skills. You end up
with an **editable, native `.pptx` built from code** — not screenshots, not HTML.

## Use this skill when…

- you want a presentation but would rather describe it than build it
- the request sounds like **PPT / PowerPoint / deck / slides / 幻灯片 / 演示文稿 / 汇报材料**
- no `pptxgen-ts-starter` project exists in the working directory yet
- the result has to stay **editable in PowerPoint** afterwards

```
$ npx skills add zythum/pptxgen-ts-starter-skill -l

Source: https://github.com/zythum/pptxgen-ts-starter-skill.git
◇  Found 1 skill
│    pptxgen-ts-starter
│      Front door for building PowerPoint (.pptx) presentations as code …
```

One repo, one skill, one job — nothing else gets installed alongside it.

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

## Install options

`npx skills` prompts for the agent and scope (project or global); nothing else to
configure.

```bash
npx skills add zythum/pptxgen-ts-starter-skill -g -y                    # global, auto-detect agent
npx skills add zythum/pptxgen-ts-starter-skill -g -y -a claude-code     # global, Claude Code
npx skills add zythum/pptxgen-ts-starter-skill -y                       # project scope
```

Pin a release for reproducibility:

```bash
npx skills add zythum/pptxgen-ts-starter-skill@v1.0.0 -g -y
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
