# Claude Setup

This directory contains doublecheck formatted for use with Claude.

## How to Use

### In Claude Projects

1. Create a new Claude Project (or open an existing one).
2. Go to Project Knowledge and add `doublecheck-claude.md` as a file.
3. In the Project Instructions, add: "Follow the doublecheck verification instructions provided in the project knowledge when the user activates doublecheck."
4. Start a conversation in that project and say "use doublecheck."

### In Claude Custom Instructions (claude.ai)

1. Go to Settings > Profile > Custom Instructions.
2. Paste the contents of `doublecheck-claude.md` into the custom instructions field.
3. Start a new conversation and say "use doublecheck."

### In Claude Code (.claude/CLAUDE.md)

1. Copy `doublecheck-claude.md` into your project's `.claude/CLAUDE.md` file (or append to an existing one).
2. Claude Code will pick up the instructions automatically.

## Notes

- Claude has web search capability when enabled, which is required for Layer 2 (source verification). Make sure web search (or "Analysis" tool use) is enabled in your Claude settings.
- Claude Projects can hold longer instructions than Custom Instructions. If the file is too long for Custom Instructions, consider using a Project instead.
