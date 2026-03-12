# Platform Setup Guides

Step-by-step instructions for setting up doublecheck in each supported AI tool.

---

## GitHub Copilot

### Option A: As a Copilot Plugin

1. Navigate to your organization's Copilot settings.
2. Install the doublecheck plugin from the `platforms/copilot/` directory.
3. The skill and agent will be available in Copilot Chat.
4. Say "use doublecheck" to activate persistent mode, or mention @doublecheck to use the agent.

### Option B: As Project Instructions

1. In your repository, create or edit `.github/copilot-instructions.md`.
2. Copy the contents of `platforms/copilot/skills/doublecheck/SKILL.md` into the file.
3. Copilot will follow these instructions when working in that repository.
4. Say "use doublecheck" in Copilot Chat.

---

## Claude

### Option A: Claude Projects (Recommended)

1. Go to [claude.ai](https://claude.ai) and create a new Project.
2. Open the Project's settings and find "Project Knowledge."
3. Upload `platforms/claude/doublecheck-claude.md` as a knowledge file.
4. In Project Instructions, add: "Follow the doublecheck verification instructions in the project knowledge when the user activates doublecheck."
5. Start a conversation in the Project and say "use doublecheck."

### Option B: Custom Instructions

1. Go to claude.ai > Settings > Profile > Custom Instructions.
2. Paste the contents of `platforms/claude/doublecheck-claude.md`.
3. Start a new conversation and say "use doublecheck."

### Option C: Claude Code

1. In your project directory, create or edit `.claude/CLAUDE.md`.
2. Paste the contents of `platforms/claude/doublecheck-claude.md`.
3. Claude Code will follow these instructions automatically.

### Important

Make sure web search (or tool use) is enabled in your Claude settings. Layer 2 (source verification) requires web access.

---

## ChatGPT

### Option A: Custom GPT (Recommended)

1. Go to [chat.openai.com](https://chat.openai.com).
2. Navigate to "Explore GPTs" > "Create."
3. Give it a name (e.g., "Doublecheck") and a description.
4. In the "Instructions" field, paste the contents of `platforms/chatgpt/doublecheck-chatgpt.md`.
5. Under "Capabilities," enable "Web Browsing."
6. Save the GPT.
7. Start a conversation with your new GPT. It is already configured -- just start chatting, or say "doublecheck this: [text]" for one-shot verification.

### Option B: Custom Instructions

1. Go to Settings > Personalization > Custom Instructions.
2. In the "How would you like ChatGPT to respond?" field, paste the contents of `platforms/chatgpt/doublecheck-chatgpt.md`.
3. Start a new conversation and say "use doublecheck."

### Important

Web browsing must be enabled for source verification to work. Without it, the tool is limited to self-audit and adversarial review (Layers 1 and 3).

---

## Cursor

### Option A: Project Rules (Recommended)

1. In your project root, create the directory `.cursor/rules/` if it does not exist.
2. Copy `platforms/cursor/doublecheck.mdc` into `.cursor/rules/`.
3. Cursor will pick up the rules when you work in that project.
4. Say "use doublecheck" in Cursor's AI chat.

### Option B: Global Rules

1. Open Cursor > Settings > General > Rules for AI.
2. Paste the contents of `platforms/cursor/doublecheck.mdc`.
3. The rules will apply across all projects.

---

## Other AI Tools

If your AI tool is not listed above but supports custom instructions, system prompts, or similar configuration:

1. Use the generic single-file version at `platforms/generic/doublecheck-instructions.md`.
2. Paste it into whatever custom instruction mechanism your tool provides.
3. Say "use doublecheck" to activate.

If the full version is too long for your tool's character limits, try the ChatGPT version (`platforms/chatgpt/doublecheck-chatgpt.md`) -- it is the most condensed.

### Does Your Tool Have Web Search?

Layer 2 (source verification) requires the AI tool to search the web. If your tool cannot search the web:

- Layers 1 and 3 (self-audit and adversarial review) still work and still provide value
- Layer 2 will be limited to the model's existing knowledge rather than live search results
- Claims will more often be rated PLAUSIBLE or UNVERIFIED rather than VERIFIED, since the tool cannot find external sources

The tool is less effective without web search, but the claim extraction and hallucination pattern checks are still worth running.
