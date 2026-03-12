# Confidence Ratings

Each verifiable claim extracted from the target text receives a confidence rating after completing the verification pipeline. These ratings communicate what was found (or not found) during verification, and what the user should do with that information.

Ratings are not verdicts. A VERIFIED rating means a supporting source was found, not that the claim is guaranteed to be correct. An UNVERIFIED rating means no evidence was found either way, not that the claim is wrong. The ratings help prioritize human attention -- they do not replace human judgment.

---

## Rating Definitions

### VERIFIED

A supporting source was found and linked.

**What this means:** A credible, relevant source was located that supports the claim. The source URL is included in the report so the user can review it independently.

**What the user should do:** Spot-check the source link if the claim is important to their work. Sources themselves can be wrong, outdated, or misinterpreted. A supporting link accelerates the verification process; it does not complete it.

### PLAUSIBLE

Consistent with general knowledge. No specific source was found.

**What this means:** The claim aligns with what is generally understood about the topic, but no specific source was located to confirm it. This may mean the claim is common knowledge that does not have a single authoritative source, or it may mean the verification search did not surface a relevant result.

**What the user should do:** Treat as reasonable but unconfirmed. If the claim is important to a decision, verify it independently before relying on it.

### UNVERIFIED

Could not find supporting or contradicting evidence.

**What this means:** The verification search found nothing relevant -- no support and no contradiction. This could mean the claim is too niche for general web search, the relevant sources are behind paywalls, the claim is about something too recent, or the claim is simply wrong and there is nothing to find.

**What the user should do:** Do not rely on this claim without independent verification. The absence of evidence is not evidence of accuracy.

### DISPUTED

Contradicting evidence was found from a credible source.

**What this means:** At least one credible source was found that contradicts the claim. The contradicting source is linked in the report. This does not guarantee the claim is wrong -- the contradicting source could itself be outdated or incorrect -- but it means there is a conflict that requires human resolution.

**What the user should do:** Review the contradicting source. Compare it against the original claim. Determine which source is more authoritative, more recent, or more relevant to the specific context. Do not use the claim as-is without resolving the contradiction.

### FABRICATION RISK

Matches hallucination patterns. High probability of being AI-generated misinformation.

**What this means:** The claim exhibits one or more hallucination patterns (see `hallucination-patterns.md`). Common triggers: a citation that cannot be found anywhere, a precise statistic with no identifiable source, or a specific claim that structurally matches known fabrication patterns.

**What the user should do:** Assume this claim is wrong until confirmed from a primary source. This is the strongest flag the tool produces. Claims rated FABRICATION RISK should not be used, shared, or acted upon without independent confirmation from a source the user trusts.

---

## Rating Distribution Guidance

A typical verification report might show:

- Several VERIFIED and PLAUSIBLE claims (the majority, in a well-produced text)
- A few UNVERIFIED claims (normal -- not everything can be confirmed through web search)
- Occasional DISPUTED or FABRICATION RISK claims (these are the items that justify running the verification)

If a report shows a high concentration of DISPUTED and FABRICATION RISK ratings, the entire text should be treated with extra skepticism. The non-flagged claims may also be unreliable -- the tool catches patterns, not certainties.

---

## Limitations of the Rating System

- **VERIFIED does not mean "definitely correct."** Sources can be wrong. A source confirming a claim accelerates your verification; it does not complete it.
- **PLAUSIBLE does not mean "probably correct."** It means the tool could not find a specific source but the claim did not trigger any red flags. It might still be wrong.
- **UNVERIFIED is not a negative finding.** It means the tool reached the limits of what it could check. Many accurate claims will be rated UNVERIFIED simply because the relevant sources are not accessible through web search.
- **FABRICATION RISK is a pattern match, not a certainty.** The tool identifies structural patterns associated with hallucinations. Rarely, a real claim may match these patterns (e.g., a legitimate but obscure citation that is not indexed by search engines). The rating flags risk; the user determines truth.
