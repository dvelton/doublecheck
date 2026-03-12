# FAQ

## General

**What is doublecheck?**

A set of instructions you add to an AI tool that teach it to verify its own output. It extracts claims, searches for sources, checks for hallucination patterns, and produces a structured report. No software to install, no server to run.

**Does it actually work?**

It catches many common issues, particularly fabricated citations, unsourced statistics, and structural hallucination patterns. It will not catch everything -- it runs on the same type of AI model that may have produced the errors. Think of it as a systematic first pass that identifies the claims needing human attention, not a guarantee of accuracy.

**Which AI tools does it work with?**

Any AI tool that supports custom instructions or system prompts and has web search capability. Platform-specific versions are provided for GitHub Copilot, Claude, ChatGPT, and Cursor. A generic version works with anything else.

**Is web search required?**

Strongly recommended. Layer 2 (source verification) needs web search to find supporting or contradicting sources. Without it, the tool is limited to self-audit (Layer 1) and adversarial review (Layer 3), which still provide value but cannot produce source links.

---

## Usage

**What is the difference between active mode and one-shot mode?**

Active mode stays on for the entire conversation and automatically verifies every substantive response. One-shot mode runs once on specific text you provide and then goes back to normal.

**When should I use active mode?**

When you are working on something where accuracy matters throughout the conversation -- research, analysis, preparing documents that will be shared or acted upon.

**When should I use one-shot mode?**

When you want to verify a specific piece of text without the overhead of verifying everything in the conversation.

**What does "full report" do?**

Produces the complete three-layer verification report with every claim extracted, rated, and sourced. In active mode, you normally get a shorter inline summary. Saying "full report" gets you the detailed version.

**Can I dig deeper on a specific claim?**

Yes. After any verification, reference a claim by its ID (e.g., "tell me more about C3") and the tool will run additional searches or explain its reasoning.

---

## Ratings

**What does VERIFIED mean?**

A supporting source was found and linked. It does NOT mean the claim is definitely correct -- sources can be wrong too. It means there is a link you can check.

**What does FABRICATION RISK mean?**

The claim matches patterns commonly associated with AI hallucinations. Most often triggered by citations that cannot be found anywhere or precise statistics with no identifiable source. Treat these as wrong until you confirm them from a source you trust.

**A claim was rated UNVERIFIED. Is it wrong?**

Not necessarily. UNVERIFIED means the tool could not find evidence either way. The claim might be accurate but behind a paywall, too niche for web search, or too recent. It might also be wrong. The rating means "I could not check this" -- not "this is false."

**Can I override a rating?**

Yes. If you know a flagged claim is correct based on your own expertise, say so. The tool will note it as confirmed by domain knowledge and move on.

---

## Limitations

**Is this a replacement for subject matter expertise?**

No. The tool helps you verify faster, but it does not replace knowing the domain. It catches structural issues well (missing citations, unfindable references) but is weaker on substantive accuracy in specialized fields.

**Can it verify content behind paywalls?**

No. If the relevant sources are in paywalled databases, academic journals, or proprietary systems, the tool will not be able to find them through web search. Those claims will typically be rated UNVERIFIED.

**Can I use this to verify AI-generated code?**

Not well. The tool is designed for factual claims in prose, not code correctness. Use code-specific review tools for code.

**Does using doublecheck slow down the AI's responses?**

Yes, somewhat. The tool adds web searches and analysis on top of the normal response. Active mode with inline verification adds moderate overhead. Full reports take longer because they run the complete pipeline. One-shot mode on a long piece of text will take the most time.
