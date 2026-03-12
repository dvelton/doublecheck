# Contributing to Doublecheck

Thanks for your interest in improving doublecheck.

## What This Project Is

Doublecheck is a prompt library -- a set of instructions that teach AI tools to verify their own output. There is no executable code. The prompts themselves are the product.

Contributions generally fall into three categories:

1. Improving the core verification prompts
2. Adding or updating platform-specific packages
3. Improving documentation

## How to Contribute

### Improving Prompts

The prompts in `prompts/core/`, `prompts/modes/`, and `prompts/templates/` are the heart of the project. Changes here affect every platform package.

Before submitting a prompt change:

- Test it. Copy the modified prompt into an AI tool and run it against text with known issues. Does it catch what it should catch? Does it produce false positives?
- Keep it platform-agnostic. Core prompts should not reference any specific AI tool, platform, or vendor.
- Keep it domain-neutral. Do not call out specific industries by name. Use general language like "content where errors carry real consequences."
- Preserve the honest limitations framing. This tool accelerates verification; it does not make AI output trustworthy. Do not overstate what it can do.

### Adding Platform Support

Each directory in `platforms/` contains a version of the prompts formatted for a specific AI tool. To add a new platform:

1. Create a new directory under `platforms/` with the platform name
2. Include a brief README explaining how to set it up
3. Adapt the core prompts to fit the platform's instruction format and constraints (character limits, syntax requirements, etc.)
4. Note any features that the platform does not support (e.g., if the tool lacks web search, note that Layer 2 will be limited)

The generic single-file version in `platforms/generic/` is the canonical reference. Platform-specific versions may be shorter or restructured to fit constraints, but they should not change the methodology.

### Improving Documentation

Docs live in `docs/` and should be clear, practical, and accurate. If you find something confusing or incorrect, a PR to fix it is welcome.

## Pull Request Guidelines

- One focused change per PR. A prompt improvement and a new platform package should be separate PRs.
- Describe what you changed and why. Include any testing you did.
- If your change affects core prompts, note whether it requires updates to platform-specific packages.

## What We Will Not Accept

- Changes that overstate the tool's capabilities or remove limitation disclosures
- Platform-specific logic in core prompts
- Industry-specific or domain-specific callouts in core prompts
- Executable code or infrastructure dependencies (this is a prompt library, not a software project)
