# Ever Works — Skills catalog

A curated catalog of **Agent Skills** in the [agentskills](https://github.com/agentskills/agentskills)
open standard (`SKILL.md`). The Ever Works platform reads [`manifest.json`](manifest.json)
from the default branch (`main`) and resolves each row's `skillPath` to load the skill.

Each skill lives in its own directory under [`skills/`](skills/):

```
skills/<slug>/SKILL.md        # YAML frontmatter (name, description) + Markdown body
skills/<slug>/LICENSE.txt      # upstream license, for Apache-2.0 sources
skills/<slug>/{references,scripts,assets}/   # optional supporting files
```

`manifest.json` is the index — one row per skill, each recording the skill's exact
SPDX `license` and upstream `sourceUrl`. The row shape is defined in
[`schema/skill-manifest.schema.json`](schema/skill-manifest.schema.json).

## What's in this catalog

| Skill | Summary | License |
|-------|---------|---------|
| `skill-creator` | Create, edit, and improve Agent Skills; run evals | Apache-2.0 |
| `mcp-builder` | Build high-quality MCP servers (Python / Node) | Apache-2.0 |
| `webapp-testing` | Test local web apps with Playwright | Apache-2.0 |
| `frontend-design` | Distinctive, intentional visual design for UI | Apache-2.0 |
| `brand-guidelines` | Apply an org's brand colors/typography (brand-agnostic) | Apache-2.0 |
| `internal-comms` | Write internal communications in common formats | Apache-2.0 |
| `claude-api` | Reference for the Claude API / Anthropic SDK | Apache-2.0 |
| `check-pr` | Check a PR/MR/CL for review comments and failing checks | MIT |
| `pr-report` | Maintainer-grade PR / contribution reports | MIT |
| `doc-maintenance` | Audit docs against git history and fix drift via PR | MIT |
| `evolve-app` | Change an App Work within its App spec's rules, via a pull request that stays open until live | Apache-2.0 |
| `graphify` | Navigate code through a local Graphify knowledge graph; keep it current after changes (opt-in, needs the graphify CLI) | Apache-2.0 |

## Sources & attribution
Every skill here is sourced from a permissively-licensed public repo. Each `manifest.json` row records the
skill's exact SPDX license + source URL; Apache-2.0 skills retain the upstream `LICENSE.txt` in their folder.

| Skill(s) | Upstream | License |
|----------|----------|---------|
| skill-creator, mcp-builder, webapp-testing, frontend-design, brand-guidelines, internal-comms, claude-api | [anthropics/skills](https://github.com/anthropics/skills) | Apache-2.0 (per-skill `LICENSE.txt`) |
| check-pr, pr-report, doc-maintenance | [paperclipai/paperclip](https://github.com/paperclipai/paperclip) `.agents/skills/` | MIT |
| evolve-app | Original to this catalog, written for the Ever Works platform's App Works ([ever-works/ever-works](https://github.com/ever-works/ever-works)) | Apache-2.0 |
| graphify | Original to this catalog: how an agent uses the [Graphify](https://github.com/Graphify-Labs/graphify) CLI (Apache-2.0) for code navigation. No upstream text is copied; the tool itself is not bundled | Apache-2.0 |

Skill format follows the **Agent Skills** open standard ([agentskills/agentskills](https://github.com/agentskills/agentskills), Apache-2.0):
one directory per skill, `SKILL.md` = YAML frontmatter + Markdown body, optional `references/` `scripts/` `assets/`.
Repo scaffolding, `manifest.json`, and schema © Ever Co. LTD, Apache-2.0.

### Adaptations from upstream

- **`brand-guidelines`** — rewritten to be brand-agnostic: the Anthropic-specific palette,
  fonts, and naming were stripped and replaced with a "resolve the project's brand tokens"
  approach. Still Apache-2.0; upstream `LICENSE.txt` retained.
- **`doc-maintenance`** — Paperclip-product-specific references (commit trailer, README
  audit-checklist section names) were genericized. Still MIT.

The following upstream skills were intentionally **excluded** because they ship a
proprietary / all-rights-reserved `LICENSE.txt` (frontmatter `license: Proprietary`),
which fails this catalog's permissive-only rule: Anthropic's `docx`, `pdf`, `pptx`, `xlsx`,
`doc-coauthoring`, `canvas-design`, `algorithmic-art`, `slack-gif-creator`, `theme-factory`,
and `web-artifacts-builder`. All Paperclip-internal skills were also excluded. Only the ten
permissively-licensed skills above were seeded (no extras); `evolve-app` and `graphify` were added afterwards
as original Ever Works skills under this repository's Apache-2.0 license.
