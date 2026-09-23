# pptxgen-ts-starter-skill

[English](./README.md) | 中文

装一次，之后直接跟你的 agent 提需求：

```bash
npx skills add zythum/pptxgen-ts-starter-skill -g
```

> **做个 PPT，主题是 X** · **make a deck about X** · **做一份 10 页的 Y 介绍**

agent 会脚手架出一份真实的
[**pptxgen-ts-starter**](https://github.com/zythum/pptxgen-ts-starter) 工程、装好依赖，
然后交接给那个工程自带的 skills。最终交付的是一份
**由代码生成、可继续在 PowerPoint 里编辑的原生 `.pptx`** —— 不是截图，也不是 HTML。

## 什么时候用

- 你想要一份演示文稿，但更愿意「描述它」而不是「动手做它」
- 需求听起来像 **PPT / PowerPoint / deck / 幻灯片 / 演示文稿 / 汇报材料**
- 当前目录里还没有 `pptxgen-ts-starter` 工程
- 交付物之后还得能**在 PowerPoint 里继续编辑**

```
$ npx skills add zythum/pptxgen-ts-starter-skill -l

Source: https://github.com/zythum/pptxgen-ts-starter-skill.git
◇  Found 1 skill
│    pptxgen-ts-starter
│      Front door for building PowerPoint (.pptx) presentations as code …
```

一个仓库、一个 skill、一件事 —— 不会顺带装进别的东西。

## 它做什么

只做一件事：**脚手架 → 安装 → 转交。**

| 步骤 | 动作 |
| ---- | ---- |
| 0 | 检测当前目录是否已是 `pptxgen-ts-starter` 工程 —— 绝不重复脚手架 |
| 1 | `npx tiged zythum/pptxgen-ts-starter <dir>` |
| 2 | `npm install` |
| 3 | *（可选）* 把工程内的 `.agents/skills/` 桥接给你所在的 agent |
| 4 | 读工程的 `README.md` / `AGENTS.md` / `.agents/skills/`，然后按其工作流开工 |

它**刻意不做版式设计**。内容、版式、配色与交付 QA 全部住在脚手架产物里 ——
所以这个 skill 永远不会和它们脱节。

## 安装方式

`npx skills` 会提示你选择 agent 与作用域（项目级 / 全局），无需其他配置。

```bash
npx skills add zythum/pptxgen-ts-starter-skill -g -y                    # 全局，自动检测 agent
npx skills add zythum/pptxgen-ts-starter-skill -g -y -a claude-code     # 全局，Claude Code
npx skills add zythum/pptxgen-ts-starter-skill -y                       # 项目级
```

固定版本（可复现）：

```bash
npx skills add zythum/pptxgen-ts-starter-skill@v1.0.0 -g -y
```

## 依赖

- Node.js + npm
- 运行 `npx tiged` 与 `npm install` 需要网络
- `git` —— 仅在 `git clone` 兜底路径中用到

依赖在 skill frontmatter 的 `metadata.requires.bins` 中声明。

## 相关仓库

- **[pptxgen-ts-starter](https://github.com/zythum/pptxgen-ts-starter)** ——
  本 skill 所脚手架的目标模版工程。工程一旦就位，真正干活的是它自带的
  `design` 与 `pptxgenjsx` 两个 skill。

## 仓库结构

```
skills/
└── pptxgen-ts-starter/
    └── SKILL.md        # 本仓唯一的 skill —— 前门
```

放在顶层 `skills/` 容器下，使
`npx skills add zythum/pptxgen-ts-starter-skill` 恰好发现一个 skill。

## 维护约定

- `name` 保持小写、连字符，且与目录名一致。
- `description` 是**触发面** —— agent 在决定加载本 skill 之前能看到的唯一文本。
  务必保留用户真实提问方式的措辞，中英文都要（PPT、PowerPoint、幻灯片、
  演示文稿、deck…）。
- Step 1 里的模版 URL 必须保持正确。模版仓改名就必须改这里 ——
  那个 URL 是本 skill 唯一的硬耦合。
- 若本 skill 有一天需要**描述**模版工程内部细节（而不只是指路），
  就把它搬回模版仓。**描述内部的东西，必须和内部住在一起。**

## 许可

MIT
