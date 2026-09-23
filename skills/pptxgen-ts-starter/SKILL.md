---
name: pptxgen-ts-starter
version: 1.0.0
description: >
  Front door for building PowerPoint (.pptx) presentations as code with
  pptxgen-ts-starter. Use whenever the user asks to create, build, or edit a
  PPT / PowerPoint file / slide deck / presentation / slides — including
  Chinese requests such as 做 PPT、做个幻灯片、演示文稿、汇报材料、路演 deck —
  and in particular when no pptxgen-ts-starter project exists in the working
  directory yet. This skill only scaffolds the project and hands off to the
  clone's own skills; it does not design slides itself.
metadata:
  requires:
    bins: ["node", "npm", "npx", "git"]
  homepage: https://github.com/zythum/pptxgen-ts-starter-skill
---

# pptxgen-ts-starter — Scaffold & hand off

Resolve "the user wants a deck" into a working **pptxgen-ts-starter** project,
then hand control to that project's own skills.

**Template this skill scaffolds:**
<https://github.com/zythum/pptxgen-ts-starter>

**This skill is a bootstrap, not a design guide.** Content, layout, colour and
QA decisions belong to the scaffolded project
(`.agents/skills/design/`, `.agents/skills/pptxgenjsx/`) — never reimplement
them here.

## Step 0 — Detect first (never re-scaffold)

Treat the current directory as **already a project** when all three hold:

- `package.json` contains `"pptxgen-ts-starter"`
- `scripts/generate-pptx.ts` exists
- `.agents/skills/design/SKILL.md` exists

→ If yes, **skip Steps 1–3** and go straight to Step 4.

Also: if the user already points at a repository or directory, use that one
instead of creating a new project. Ask before scaffolding into a non-empty
directory.

## Step 1 — Scaffold

```bash
npx tiged zythum/pptxgen-ts-starter <dir>
```

`<dir>` is a kebab-case name derived from the topic (e.g. `kimi-k3-intro`).
If the topic is unclear, ask; only fall back to `my-presentation` when the user
does not care.

Then **verify the dot-directories survived** — they carry the project's own
skills and workspace:

```bash
test -d <dir>/.agents/skills/design && test -d <dir>/.deck && echo ok
```

If `.agents/` or `.deck/` is missing, the scaffold dropped dot-directories —
redo it with:

```bash
git clone --depth 1 https://github.com/zythum/pptxgen-ts-starter.git <dir>
rm -rf <dir>/.git
```

## Step 2 — Install dependencies

```bash
cd <dir> && npm install
```

Do not skip this. Every tool the workflow relies on (`scripts/estimate-text.ts`,
`image-tool.ts`, `color-tool.ts`, the dev server) needs it.

## Step 3 — (optional) Make the project's skills auto-discoverable

**The hand-off does not depend on this step.** Step 4 reads the skill files by
explicit path, which works in any agent. Do this only when you want the
project's skills to surface automatically in *later* sessions inside `<dir>`.

The real skills ship **inside the project** at `.agents/skills/`. Codex, Cursor,
Copilot, Gemini CLI, opencode and others read that path natively; Claude Code's
documented path is `.claude/skills/`. Bridge it once if you need it:

```bash
cd <dir> && npx skills add ./ -y -a claude-code   # Claude Code
cd <dir> && npx skills add ./ -y                   # other agents: auto-detect
```

Equivalent without the CLI — a **directory symlink, never a copy** (a copy forks
the content and will drift out of sync):

```bash
cd <dir> && mkdir -p .claude && ln -s ../.agents/skills .claude/skills
```

Skip entirely when your agent already reads `.agents/skills/`.

## Step 4 — Read, then work

Read these, in order, before writing any slide:

1. `README.md` — quick start (`README_CN.md` if the user works in Chinese)
2. `AGENTS.md` — conventions, positioning rules, common mistakes
3. `.agents/skills/design/SKILL.md` — the deck workflow
   (clarify → research → outline → spec → compose/visuals → QA)
4. `.agents/skills/pptxgenjsx/SKILL.md` — component API

Then run the design skill's workflow as written. Do not invent your own
structure, and do not skip its gates — they may only be waived by explicit user
delegation.

## Hard rules for the scaffolded project

- The deliverable is a **code-generated, editable `.pptx`** (`npm run generate`).
  Never HTML, never slide screenshots, never image-only slides.
- Colours come from `src/token/colors.ts`; typography from
  `src/token/typography.ts`. No bare hex or magic font sizes in slides.
- Do not modify `scripts/*` or `web/*`. Content belongs in `src/slides/` and
  `src/components/`.

## Failure modes

| Symptom | Do this |
| --- | --- |
| `npx tiged` fails / offline | `git clone --depth 1 …` fallback in Step 1 |
| Directory already holds a project | Step 0 catches it — never re-scaffold |
| Cannot run `npm` | Tell the user; **do not** hand-write a `.pptx` from scratch |
| `.agents/` missing after scaffold | Redo Step 1 with the `git clone` fallback |
| `.claude/skills` is a plain text file, not a directory | Windows without symlink support. Skip the bridge — Step 4 is unaffected — or redo Step 3 with `npx skills add --copy` |
