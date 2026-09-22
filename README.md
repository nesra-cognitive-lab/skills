# Nesra UI skills

Agent guidance for building React interfaces with `@nesra/ui` and migrating existing interfaces to it. The skill contains decision and migration workflows. Component APIs remain in the public package and Nesra UI documentation.

## Install

```bash
npx skills add nesra-cognitive-lab/skills@nesra-ui
```

The command installs [`skills/nesra-ui`](skills/nesra-ui/SKILL.md) with its references through the [skills CLI](https://skills.sh/). Select the agent and project or global scope when prompted. To choose Codex and project scope non-interactively:

```bash
npx skills add nesra-cognitive-lab/skills@nesra-ui --agent codex --yes
```

Restart the agent session after installation so it discovers the skill. For manual installation, copy the complete `skills/nesra-ui` directory, including `references/`, into the agent's skills directory.

The Nesra UI docs publish `/llms.txt`, individual Markdown pages, and `/llms-full.txt`. Check the installed package's public exports and types when the docs describe a newer version.
