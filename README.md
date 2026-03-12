# Doublecheck

A three-layer verification pipeline for AI-generated output. Extracts verifiable claims, finds sources via web search, runs adversarial review for hallucination patterns, and produces a structured report with source links so humans can verify before acting.

## Why This Exists

AI hallucinations are a model-level problem. No prompt can fix them. But the consequences of acting on fabricated citations, made-up statistics, or confidently wrong claims can be mitigated by making verification fast and structured.

Doublecheck does not tell you what is true. It extracts every verifiable claim from AI output, searches for sources you can check independently, and flags anything that matches known hallucination patterns. You make the final call.

## What Is This?

A set of prompt instructions you can drop into any AI tool that supports custom instructions, system prompts, or similar configuration. There is no server to run, no API to configure, no software to install. The prompts themselves are the tool.

The repo includes platform-specific versions for popular AI tools (see `platforms/`), plus a generic single-file version that works anywhere.

## What's Included

```
prompts/
  core/
    verification-pipeline.md     # The full three-layer methodology
    hallucination-patterns.md    # Seven patterns to check for
    confidence-ratings.md        # Rating definitions and what they mean
  modes/
    active-mode.md               # Persistent verification (always on)
    one-shot-mode.md             # Verify specific text on demand
    interactive-mode.md          # Follow-up investigation
  templates/
    verification-report.md       # Full report template
    inline-summary.md            # Inline summary for active mode
platforms/
    copilot/                     # GitHub Copilot plugin format
    claude/                      # Claude Projects instructions
    chatgpt/                     # ChatGPT custom instructions
    cursor/                      # Cursor rules file
    generic/                     # Single-file version for any platform
```

## The Three Layers

**Layer 1: Self-Audit.** Re-reads the target text critically. Extracts every verifiable claim -- facts, statistics, citations, dates, causal assertions. Checks for internal contradictions. Categorizes claims for downstream verification.

**Layer 2: Source Verification.** For each extracted claim, runs web searches to find supporting or contradicting evidence. Produces clickable URLs for independent human review. Citations get extra scrutiny because they are the highest-risk category for hallucinations.

**Layer 3: Adversarial Review.** Switches posture entirely -- assumes the output contains errors and actively tries to find them. Checks against a hallucination pattern checklist covering fabricated citations, unsourced statistics, confident specificity on uncertain topics, temporal confusion, overgeneralization, and missing qualifiers.

## Confidence Ratings

Each claim gets a final rating:

| Rating | Meaning |
|--------|---------|
| VERIFIED | Supporting source found and linked |
| PLAUSIBLE | Consistent with general knowledge, no specific source found |
| UNVERIFIED | Could not find supporting or contradicting evidence |
| DISPUTED | Contradicting evidence found from a credible source |
| FABRICATION RISK | Matches hallucination patterns (e.g., citation that cannot be found anywhere) |

Full definitions and guidance on what to do with each rating are in `prompts/core/confidence-ratings.md`.

## Example

To show what doublecheck actually produces, here is a realistic example. Suppose an AI tool generates this summary about a data privacy regulation:

> The European Data Protection Board issued Guideline 04/2023 in September 2023, establishing that companies processing biometric data must obtain explicit consent under Article 9(2)(a) of the GDPR. This guideline superseded the earlier Guideline 3/2019, and 87% of EU member states had updated their national implementing legislation to reflect it by January 2024. Failure to comply carries fines of up to 4% of global annual turnover under Article 83(5).

A doublecheck verification report on this text would look like:

---

**Verification (5 claims checked)**

**Heads up:** I could not confirm the existence of "Guideline 04/2023" from the EDPB, and the statistic about member state compliance has no identifiable source. Producing full verification report.

### Summary

**Text verified:** AI-generated summary about EDPB biometric data guidelines
**Claims extracted:** 5
**Breakdown:**

| Rating | Count |
|--------|-------|
| VERIFIED | 2 |
| PLAUSIBLE | 0 |
| UNVERIFIED | 0 |
| DISPUTED | 0 |
| FABRICATION RISK | 3 |

**Items requiring attention:** 3 items rated FABRICATION RISK

### Flagged Items (Review These First)

#### C1 -- EDPB Guideline 04/2023 on biometric data

- **Claim:** "The European Data Protection Board issued Guideline 04/2023 in September 2023, establishing that companies processing biometric data must obtain explicit consent under Article 9(2)(a) of the GDPR."
- **Rating:** FABRICATION RISK
- **Finding:** The EDPB publishes all guidelines on its website. A search of the EDPB guideline register does not return any document numbered "04/2023" related to biometric data. The EDPB has issued guidance on consent (Guidelines 05/2020) and on facial recognition (Guidelines 05/2022), but the specific guideline cited here does not appear to exist.
- **Source:** https://edpb.europa.eu/our-work-tools/general-guidance/guidelines-recommendations-best-practices_en
- **Recommendation:** Do not cite this guideline. Search the EDPB guideline register directly to identify the actual applicable guidance on biometric data processing.

#### C2 -- Supersession of Guideline 3/2019

- **Claim:** "This guideline superseded the earlier Guideline 3/2019."
- **Rating:** FABRICATION RISK
- **Finding:** Since C1 (Guideline 04/2023) does not appear to exist, the claim that it superseded an earlier guideline is also unsupported. EDPB Guidelines 3/2019 relates to processing of personal data through video devices, not biometric consent. The entire supersession chain appears to be fabricated.
- **Pattern:** Plausible-but-wrong association -- real EDPB guideline number attached to wrong subject matter.
- **Recommendation:** Remove this claim entirely.

#### C3 -- 87% member state compliance statistic

- **Claim:** "87% of EU member states had updated their national implementing legislation to reflect it by January 2024."
- **Rating:** FABRICATION RISK
- **Finding:** No source found for this statistic. The EDPB does not publish compliance rates for guideline implementation by member states in this format. The precision of "87%" for what would be approximately 23.5 out of 27 member states is itself suspicious -- compliance tracking for EU directives is typically reported as a count, not a percentage.
- **Pattern:** Precise numbers without sources.
- **Recommendation:** Remove this statistic unless you can find a primary source. If you need member state implementation data, check the European Commission's implementation tracking tools.

### All Claims

#### VERIFIED

#### C4 -- Explicit consent under Article 9(2)(a)
- **Claim:** "Companies processing biometric data must obtain explicit consent under Article 9(2)(a) of the GDPR."
- **Source:** https://eur-lex.europa.eu/eli/reg/2016/679/oj (GDPR Article 9(2)(a))
- **Notes:** Article 9(2)(a) does provide explicit consent as one lawful basis for processing special category data, which includes biometric data. However, it is not the only available basis -- the text omits alternatives like Article 9(2)(b) (employment context) and 9(2)(g) (substantial public interest). The claim is verified but incomplete.

#### C5 -- 4% fine under Article 83(5)
- **Claim:** "Failure to comply carries fines of up to 4% of global annual turnover under Article 83(5)."
- **Source:** https://eur-lex.europa.eu/eli/reg/2016/679/oj (GDPR Article 83(5))
- **Notes:** Confirmed. Article 83(5) sets the maximum fine at EUR 20 million or 4% of total worldwide annual turnover, whichever is higher. The text correctly states the percentage but omits the EUR 20 million alternative.

### Internal Consistency

No internal contradictions detected. However, the text builds a logical chain (guideline exists -> it superseded an earlier guideline -> member states complied with it) where the foundation (C1) appears fabricated. The downstream claims (C2, C3) inherit that problem.

### What Was Not Checked

- Whether any individual EU member state has specific national legislation on biometric data consent beyond GDPR implementation (this would require searching 27 national legal databases)

### Limitations

- This tool accelerates human verification; it does not replace it.
- Web search results may not include the most recent information or paywalled sources.
- The adversarial review uses the same type of AI model that may have produced the original output. It catches many issues but cannot catch all of them.
- A claim rated VERIFIED means a supporting source was found, not that the claim is definitely correct. Sources can be wrong too.
- Claims rated PLAUSIBLE may still be wrong. The absence of contradicting evidence is not proof of accuracy.

---

This example illustrates several things the tool is designed to catch: a fabricated citation that looks structurally correct (right format, plausible number, real organization), a downstream claim that inherits the fabrication, a precise statistic with no source, and verified claims that are technically correct but missing important context. The real GDPR articles are accurate; the EDPB guideline, the supersession claim, and the compliance statistic are not.

## Usage Modes

### Active Mode (Always On)

Activate doublecheck and it stays on for the rest of your conversation. Every substantive response gets verification automatically.

How to activate depends on your platform:
- Say "use doublecheck" or "turn on doublecheck"
- See `platforms/` for platform-specific activation

Once active:
- Simple factual responses get inline verification summaries
- High-stakes content where errors carry real consequences automatically gets the full verification report
- If any claim rates DISPUTED or FABRICATION RISK during inline verification, the full report is generated automatically
- Code, creative writing, and casual conversation are skipped
- Say "full report" on any response to get the complete three-layer verification
- Turn it off anytime with "turn off doublecheck"

### One-Shot Verification

Verify specific text on demand without turning on persistent mode:

- "Use doublecheck to verify: [paste text]"
- "Doublecheck that last response"

Runs the full three-layer pipeline and produces a detailed report.

### Interactive Follow-Up

After any verification report, you can:
- Dig deeper on specific claims ("tell me more about C3")
- Verify a source the tool linked ("does that source actually say that?")
- Ask why something was rated the way it was
- Check something new

## When to Use It

- Before acting on AI-generated analysis where errors carry real consequences
- Before including AI-generated statistics or data points in documents that will be shared
- When reviewing AI output that will inform decisions
- When AI output includes citations, references, or specific sources you have not checked
- Anytime you think "I should probably double-check this"

## When NOT to Use It

- For creative or subjective content where "accuracy" is not the goal
- For code review (use code-specific review tools)
- As a substitute for subject matter expertise -- the tool helps you verify faster, but it does not replace knowing the domain

## Limitations

Be clear-eyed about what this tool cannot do:

**Same model, same biases.** The verification pipeline runs on the same type of AI model that may have produced the original output. It catches many issues -- particularly structural patterns like missing citations -- but it has the same fundamental knowledge limitations. A dedicated verification model would be better; this tool works with what is available.

**Web search is not comprehensive.** Paywalled content, recently published material, and specialized databases may not appear in search results. A claim being "unverified" may mean it is behind a paywall, not that it is wrong.

**VERIFIED means "source found," not "definitely correct."** Sources themselves can be wrong, outdated, or misinterpreted. A supporting link accelerates your verification process; it does not complete it.

**The tool cannot catch what it does not know it does not know.** If a hallucination is sophisticated enough to pass all three layers, a human expert is your last line of defense.

The honest framing: this tool raises the floor on verification quality and cuts the time it takes to identify the claims that need human attention. It does not raise the ceiling. Critical decisions should always involve human domain expertise.

## Getting Started

1. Pick your platform from the `platforms/` directory
2. Follow the setup instructions in that directory's README
3. Copy the instructions into your AI tool's configuration
4. Start a conversation and say "use doublecheck"

If your platform is not listed, use the generic single-file version in `platforms/generic/`.

For a detailed walkthrough, see `docs/platform-setup-guides.md`.

## Contributing

See `CONTRIBUTING.md` for guidelines on contributing prompts, adding platform support, and the project's approach to changes.

## License

MIT
