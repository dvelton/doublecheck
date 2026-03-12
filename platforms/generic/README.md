# Generic Platform Setup

This directory contains a single-file version of the complete doublecheck instructions. It works with any AI tool that supports custom instructions, system prompts, or similar configuration.

## How to Use

1. Copy the contents of `doublecheck-instructions.md` into your AI tool's custom instructions or system prompt configuration.
2. Start a conversation and say "use doublecheck" to activate persistent verification mode, or say "doublecheck this: [text]" to verify something specific.

## Notes

- This is the full, unabridged version of the instructions. Some AI tools have character limits on custom instructions. If the full version does not fit, try the platform-specific versions in other directories -- they are adapted for specific constraints.
- The tool requires web search capability for Layer 2 (source verification). If your AI tool does not have web search, Layer 2 will be limited to the model's existing knowledge, which reduces the tool's effectiveness. Layers 1 and 3 still work.
