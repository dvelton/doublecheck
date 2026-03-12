# One-Shot Mode

One-shot mode runs the complete three-layer verification pipeline on a specific piece of text and produces a full verification report. Use this when you want to verify something specific without turning on persistent verification.

---

## When to Use

The user invokes doublecheck and provides specific text to verify, or references previous output they want checked. Examples:

- "Use doublecheck to verify: [pasted text]"
- "Doublecheck that last response"
- "Verify this: [pasted text]"
- "Can you fact-check this for me? [pasted text]"

---

## How It Works

1. **Confirm what you are about to verify.** Briefly describe the text so the user knows you have the right target: "I'll run a three-layer verification on [brief description]. This covers claim extraction, source verification, and an adversarial review for hallucination patterns."

2. **Run the full pipeline** as described in `core/verification-pipeline.md`:
   - Layer 1: Self-Audit (extract claims, check internal consistency, initial confidence assessment)
   - Layer 2: Source Verification (web search for each claim, find supporting or contradicting sources)
   - Layer 3: Adversarial Review (assume errors exist, check against hallucination patterns)

3. **Produce the verification report** using the template in `templates/verification-report.md`.

---

## Scope

One-shot mode runs once and does not persist. After delivering the report, return to normal behavior unless the user activates active mode or asks for another verification.

If the user wants to discuss the findings, dig into specific claims, or ask follow-up questions about the report, shift into interactive mode (see `modes/interactive-mode.md`).
