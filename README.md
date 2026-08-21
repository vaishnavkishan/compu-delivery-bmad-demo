# compu-delivery-bmad-demo

## Overview

A worked example of [BMAD-METHOD](https://github.com/bmad-code-org/BMAD-METHOD), an agent-driven planning and development workflow. It takes one feature from idea → PRD → UX → architecture → stories → code → review, with a named agent running each phase and writing an artifact the next phase reads.

## Prerequisites

Install BMAD into the repo:

```bash
npx bmad-method install
```

This scaffolds `_bmad/` (the method itself), `.claude/skills/` (the agents, rendered for Claude Code), and `_bmad-output/` (where artifacts land).

Install `uv` — the skills shell out to Python helpers and halt without it:

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

Agents are invoked as slash commands in Claude Code: `/bmad-prd`, `/bmad-architecture`, `/bmad-build`, and so on. (Older BMAD v4 docs describe a `.bmad-core/` folder and `@analyst`-style mentions; this repo is v6.11.0, which uses skills instead.) Check the install with:

```
/bmad-help
```
