# GitHub Copilot Setup

This directory contains doublecheck packaged as a Copilot plugin with a skill and an agent.

## Installation

### As a plugin in your organization

1. Add this plugin to your Copilot configuration using the standard plugin installation process.
2. The skill (`skills/doublecheck/`) provides the core verification pipeline.
3. The agent (`agents/doublecheck.md`) provides interactive follow-up capabilities.

### As project-level instructions

Alternatively, copy the contents of `skills/doublecheck/SKILL.md` into your project's `.github/copilot-instructions.md` or a custom skill file.

## Components

| Component | Type | Description |
|-----------|------|-------------|
| `doublecheck` | Skill | Core verification pipeline. Runs three layers, produces structured reports. |
| `Doublecheck` | Agent | Interactive verification mode for follow-up questions and deeper investigation. |

## Usage

- Say "use doublecheck" to activate persistent verification mode
- Say "doublecheck this: [text]" for one-shot verification
- Mention @doublecheck to use the interactive agent
