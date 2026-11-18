# Nesra Skills

Reusable Agent Skills for work across Nesra projects. Each skill lives in `skills/<name>/` and may include references or other resources needed for its workflow. The collection can grow across Nesra domains as new skills are added.

## Available skills

| Skill | Use it for |
| --- | --- |
| [`nesra-ui`](skills/nesra-ui/SKILL.md) | Building React interfaces with `@nesra/ui` 1.x or 2.x, migrating existing interfaces to it, or upgrading a project from 1.x to 2.x. |

The `nesra-ui` skill checks the installed `@nesra/ui` major first and follows it. It contains [component and token decisions](skills/nesra-ui/references/component-and-token-decisions.md), a [migration workflow](skills/nesra-ui/references/migration.md), and an [upgrade workflow from 1.x to 2.x](skills/nesra-ui/references/upgrade-1-to-2.md). Exact component APIs remain in the public package and Nesra UI documentation.

## Install

Use the [skills CLI](https://skills.sh/) to install from this repository:

```bash
npx skills add nesra-cognitive-lab/skills
```

To install `nesra-ui` directly:

```bash
npx skills add nesra-cognitive-lab/skills@nesra-ui
```

Choose the agent and project or global scope when prompted. For a project-level Codex installation without prompts:

```bash
npx skills add nesra-cognitive-lab/skills@nesra-ui --agent codex --yes
```

Restart the agent session after installation so it discovers the skill. For manual installation, copy the complete skill directory, including its references, into the agent's skills directory.
