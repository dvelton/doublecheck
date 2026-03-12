# ChatGPT Setup

This directory contains doublecheck formatted for ChatGPT.

## How to Use

### As a Custom GPT

1. Go to [chat.openai.com](https://chat.openai.com) and navigate to "Explore GPTs" > "Create"
2. In the "Instructions" field, paste the contents of `doublecheck-chatgpt.md`
3. Give the GPT a name (e.g., "Doublecheck") and description
4. Under "Capabilities," make sure "Web Browsing" is enabled (required for source verification)
5. Save and start using it

### As Custom Instructions

1. Go to Settings > Personalization > Custom Instructions
2. In the "How would you like ChatGPT to respond?" field, paste the contents of `doublecheck-chatgpt.md`
3. Start a new conversation and say "use doublecheck"

## Notes

- ChatGPT custom instructions have a character limit. The instructions file in this directory is condensed to fit. For the full unabridged version, see `platforms/generic/`.
- Web browsing must be enabled for Layer 2 (source verification) to work. Without it, the tool is limited to Layers 1 and 3.
