---
name: doublecheck
description: 'Three-layer verification pipeline for AI output. Extracts verifiable claims, finds supporting or contradicting sources via web search, runs adversarial review for hallucination patterns, and produces a structured verification report with source links for human review.'
---

# Doublecheck

Run a three-layer verification pipeline on AI-generated output. The goal is not to tell the user what is true -- it is to extract every verifiable claim, find sources the user can check independently, and flag anything that looks like a hallucination pattern.

## Activation

Doublecheck operates in two modes: **active mode** (persistent) and **one-shot mode** (on demand).

### Active Mode

When the user invokes this skill without providing specific text to verify, activate persistent doublecheck mode. Respond with:

> **Doublecheck is now active.** I'll verify factual claims in my responses before presenting them. You'll see an inline verification summary after each substantive response. Say "full report" on any response to get the complete three-layer verification with detailed sourcing. Turn it off anytime by saying "turn off doublecheck."

Then follow ALL of the rules below for the remainder of the conversation:

**Rule: Classify every response before sending it.**

Before producing any substantive response, determine whether it contains verifiable claims. Use this decision tree:

| Response type | Contains verifiable claims? | Action |
|--------------|---------------------------|--------|
| Substantive analysis, research, or guidance where errors carry real consequences | Yes -- high density | Run full verification report (see full-report rule below) |
| Summary of a document, research, or data | Yes -- moderate density | Run inline verification on key claims |
| Code generation, creative writing, brainstorming | Rarely | Skip verification; note that doublecheck mode does not apply to this type of content |
| Casual conversation, clarifying questions, status updates | No | Skip verification silently |

**Rule: Inline verification for active mode.**

When inline verification applies, embed it directly in your response:

1. Generate your response normally.
2. After the response, add a `Verification` section.
3. List each verifiable claim with its confidence rating and a source link where available.

Format:

```
---
**Verification (N claims checked)**

- [VERIFIED] "Claim text" -- Source: [URL]
- [VERIFIED] "Claim text" -- Source: [URL]
- [PLAUSIBLE] "Claim text" -- no specific source found
- [FABRICATION RISK] "Claim text" -- could not find this citation; verify before relying on it

_Say "full report" for detailed three-layer verification with sources._
```

For active mode, prioritize speed. Run web searches for citations, specific statistics, and any claim you have low confidence about. You do not need to search for claims that are common knowledge or that you have high confidence about -- just rate them PLAUSIBLE and move on.

**Rule: Auto-escalate to full report for high-risk findings.**

If your inline verification identifies ANY claim rated DISPUTED or FABRICATION RISK:

1. Place a "Heads up" callout at the top of your response:

```
**Heads up:** I'm not confident about [specific claim]. I couldn't find a supporting source. You should verify this independently before relying on it.
```

2. Replace the inline verification with the full three-layer verification report using the template in `assets/verification-report-template.md`. The user should not have to ask for the detailed report when something is clearly wrong.

**Rule: Full report for high-stakes content.**

If the response contains substantive analysis, research, or guidance where errors carry real consequences, always produce the full verification report using the template in `assets/verification-report-template.md`. Do not use inline verification for this content type -- the stakes are too high for the abbreviated format.

**Rule: Offer full verification on request.**

If the user says "full report," "run full verification," "verify that," "doublecheck that," or similar, run the complete three-layer pipeline (described below) and produce the full report using the template in `assets/verification-report-template.md`.

### One-Shot Mode

When the user invokes this skill and provides specific text to verify (or references previous output), run the complete three-layer pipeline and produce a full verification report using the template in `assets/verification-report-template.md`.

### Deactivation

When the user says "turn off doublecheck," "stop doublecheck," or similar, respond with:

> **Doublecheck is now off.** I'll respond normally without inline verification. You can reactivate it anytime.

---

## Layer 1: Self-Audit

Re-read the target text with a critical lens. This layer is extraction and internal analysis -- no web searches yet.

### Step 1: Extract Claims

Go through the target text sentence by sentence and pull out every statement that asserts something verifiable. Categorize each claim:

| Category | What to look for | Examples |
|----------|-----------------|---------|
| **Factual** | Assertions about how things are or were | "Python was created in 1991", "The regulation requires annual reporting" |
| **Statistical** | Numbers, percentages, quantities | "95% of enterprises use cloud services", "The contract has a 30-day termination clause" |
| **Citation** | References to specific documents, cases, standards, papers | "Under Section 230 of the CDA...", "According to the 2023 annual report..." |
| **Entity** | Claims about specific people, organizations, products, or places | "The organization was founded in 2015", "GDPR applies to EU residents" |
| **Causal** | Claims that X caused Y or X leads to Y | "This vulnerability allows remote code execution" |
| **Temporal** | Dates, timelines, sequences of events | "The deadline is March 15", "Version 2.0 was released before the security patch" |

Assign each claim a temporary ID (C1, C2, C3...) for tracking through subsequent layers.

### Step 2: Check Internal Consistency

Review the extracted claims against each other:
- Does the text contradict itself anywhere?
- Are there claims that are logically incompatible?
- Does the text make assumptions in one section that it contradicts in another?

Flag any internal contradictions immediately.

### Step 3: Initial Confidence Assessment

For each claim, make an initial assessment based only on your own knowledge:
- Do you recall this being accurate?
- Is this the kind of claim where models frequently hallucinate?
- Is the claim specific enough to verify, or is it vague enough to be unfalsifiable?

Record your initial confidence but do NOT report it as a finding yet. This is input for Layer 2.

---

## Layer 2: Source Verification

For each extracted claim, search for external evidence. The purpose is to find URLs the user can visit to verify claims independently.

### Search Strategy

For each claim:

1. **Formulate a search query** that would surface the primary source.
2. **Run the search** using `web_search`. If the first search returns nothing relevant, reformulate and try once more.
3. **Evaluate what you find:**
   - Did you find a primary or authoritative source that directly addresses the claim?
   - Did you find contradicting information from a credible source?
   - Did you find nothing relevant?
4. **Record the result** with the source URL.

Prefer primary and authoritative sources: official documentation, government records, peer-reviewed publications, official organizational websites.

### Handling Citations

Citations are the highest-risk category for hallucinations. For any claim that references a specific document or source:

1. Search for the exact citation.
2. If found, verify the cited content actually says what the target text claims.
3. If not found at all, flag as FABRICATION RISK.

---

## Layer 3: Adversarial Review

Switch posture entirely. Assume the output contains errors and actively try to find them.

### Hallucination Pattern Checklist

1. **Fabricated citations** -- cites something that does not exist
2. **Precise numbers without sources** -- specific statistics with no attribution
3. **Confident specificity on uncertain topics** -- definitive claims where experts disagree
4. **Plausible-but-wrong associations** -- real concepts connected to wrong context
5. **Temporal confusion** -- outdated information presented as current
6. **Overgeneralization** -- context-dependent claims stated as universal
7. **Missing qualifiers** -- nuanced topics presented as settled

### Adversarial Questions

For each major claim:
- What would make this claim wrong?
- Is there a common misconception here?
- Would a subject matter expert object to how this is stated?
- Could this be outdated?

---

## Confidence Ratings

| Rating | Meaning | What the user should do |
|--------|---------|------------------------|
| **VERIFIED** | Supporting source found and linked | Spot-check the source if the claim is important |
| **PLAUSIBLE** | Consistent with general knowledge, no specific source found | Treat as reasonable but unconfirmed |
| **UNVERIFIED** | Could not find supporting or contradicting evidence | Do not rely on this without independent verification |
| **DISPUTED** | Contradicting evidence found from a credible source | Review the contradicting source; resolve the conflict |
| **FABRICATION RISK** | Matches hallucination patterns | Assume wrong until confirmed from a primary source |

---

## Producing the Report

Use the template in `assets/verification-report-template.md`. Group findings by severity. Lead with DISPUTED and FABRICATION RISK items.

### Report Principles

- Provide links, not verdicts. The user decides what is true.
- When you find contradicting information, present both sides with sources.
- If a claim is unfalsifiable, say so.
- Be explicit about what you could not check.

### Limitations Disclosure

Always include at the end of the report:

> **Limitations of this verification:**
> - This tool accelerates human verification; it does not replace it.
> - Web search results may not include the most recent information or paywalled sources.
> - The adversarial review uses the same type of AI model that may have produced the original output. It catches many issues but cannot catch all of them.
> - A claim rated VERIFIED means a supporting source was found, not that the claim is definitely correct. Sources can be wrong too.
> - Claims rated PLAUSIBLE may still be wrong. The absence of contradicting evidence is not proof of accuracy.
