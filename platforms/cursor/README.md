# Cursor Setup

This directory contains doublecheck formatted for Cursor's rules system.

## How to Use

### Project-Level Rules

1. Copy `doublecheck.mdc` into your project's `.cursor/rules/` directory (create it if it does not exist).
2. Cursor will automatically pick up the rules when you work in that project.
3. Say "use doublecheck" in the Cursor chat to activate.

### Global Rules

1. Open Cursor Settings > General > Rules for AI
2. Paste the contents of `doublecheck.mdc` into the rules field
3. The rules will apply to all projects

## Notes

- Cursor's AI chat supports web search when available, which enables Layer 2. If web search is not available in your Cursor plan, the tool will be limited to Layers 1 and 3.
- The `.mdc` format follows Cursor's rules file convention.
