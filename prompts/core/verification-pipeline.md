# Verification Pipeline

This document describes the three-layer verification pipeline. It is the core methodology behind doublecheck. Each layer builds on the previous one, and the full pipeline produces a structured verification report with source links for human review.

The goal is not to tell you what is true. It is to extract every verifiable claim, find sources you can check independently, and flag anything that looks like a hallucination pattern. You make the final call.

---

## Layer 1: Self-Audit

Re-read the target text with a critical lens. This layer is extraction and internal analysis -- no web searches yet.

### Step 1: Extract Claims

Go through the target text sentence by sentence. Pull out every statement that asserts something verifiable. Assign each claim an ID (C1, C2, C3...) for tracking through subsequent layers.

Categorize each claim:

| Category | What to look for | Examples |
|----------|-----------------|---------|
| Factual | Assertions about how things are or were | "Python was created in 1991", "The regulation requires annual reporting" |
| Statistical | Numbers, percentages, quantities | "95% of enterprises use cloud services", "The contract has a 30-day termination clause" |
| Citation | References to specific documents, cases, standards, papers, or official sources | "Under Section 230 of the CDA...", "According to the 2023 annual report..." |
| Entity | Claims about specific people, organizations, products, or places | "The organization was founded in 2015", "GDPR applies to EU residents" |
| Causal | Claims that X caused Y or X leads to Y | "This vulnerability allows remote code execution", "The policy was enacted in response to the incident" |
| Temporal | Dates, timelines, sequences of events | "The deadline is March 15", "Version 2.0 was released before the security patch" |

### Step 2: Check Internal Consistency

Review the extracted claims against each other:

- Does the text contradict itself anywhere? (e.g., states two different dates for the same event)
- Are there claims that are logically incompatible?
- Does the text make assumptions in one section that it contradicts in another?

Flag any internal contradictions immediately -- these do not need external verification to identify as problems.

### Step 3: Initial Confidence Assessment

For each claim, make an initial assessment based only on your own knowledge:

- Do you recall this being accurate?
- Is this the kind of claim where AI models frequently hallucinate? (Specific citations, precise statistics, and exact dates are high-risk categories.)
- Is the claim specific enough to verify, or is it vague enough to be unfalsifiable?

Record your initial confidence but do NOT report it as a finding yet. This is input for Layer 2, not output.

---

## Layer 2: Source Verification

For each extracted claim, search for external evidence. The purpose of this layer is to find URLs the user can visit to verify claims independently.

### Search Strategy

For each claim:

1. **Formulate a search query** that would surface the primary source. For citations, search for the exact title, name, or identifier. For statistics, search for the specific number and topic. For factual claims, search for the key entities and relationships.

2. **Run the search.** If the first search does not return relevant results, reformulate and try once more with different terms.

3. **Evaluate what you find:**
   - Did you find a primary or authoritative source that directly addresses the claim?
   - Did you find contradicting information from a credible source?
   - Did you find nothing relevant? (This is itself a signal -- real things usually have a web footprint.)

4. **Record the result** with the source URL. Always provide the URL even if you also summarize what the source says.

### What Counts as a Source

Prefer primary and authoritative sources:

- Official documentation, specifications, and standards
- Government records, regulatory filings, official registers
- Peer-reviewed publications
- Official organizational websites and press releases
- Established reference works

Note when a source is secondary (news article, blog post, wiki page) vs. primary. The user can weigh accordingly.

### Handling Citations Specifically

Citations are the highest-risk category for hallucinations. For any claim that references a specific document, case, standard, paper, or official source:

1. Search for the exact citation (title, identifier, section number).
2. If found, verify the cited content actually says what the target text claims it says.
3. If you cannot find it at all, flag it as FABRICATION RISK. AI models frequently generate plausible-sounding citations for things that do not exist.

---

## Layer 3: Adversarial Review

Switch your posture entirely. In Layers 1 and 2, you were trying to understand and verify the output. In this layer, **assume the output contains errors** and actively try to find them.

### Hallucination Pattern Checklist

Check for the patterns described in `hallucination-patterns.md`. For each pattern, assess whether the target text exhibits it. The patterns cover fabricated citations, unsourced statistics, confident specificity on uncertain topics, plausible-but-wrong associations, temporal confusion, overgeneralization, and missing qualifiers.

### Adversarial Questions

For each major claim that passed Layers 1 and 2, ask:

- What would make this claim wrong?
- Is there a common misconception in this area that the AI model might have picked up?
- If a subject matter expert reviewed this, would they object to how it is stated?
- Is this claim potentially outdated? Could it have been true at one point but no longer be accurate?

### Red Flags to Escalate

If you find any of these, flag them prominently in the report:

- A specific citation that cannot be found anywhere
- A statistic with no identifiable source
- A claim that contradicts what authoritative sources say
- A claim stated with high confidence that is actually disputed or uncertain

---

## Producing the Report

After completing all three layers, produce the verification report using the template in `templates/verification-report.md`. Group findings by severity, lead with the items that need the most attention, and always include the limitations disclosure.

### Report Principles

- **Provide links, not verdicts.** The user decides what is true, not the tool. "Here is where you can verify this" is useful. "I believe this is correct" is just more AI output.
- **When you find contradicting information, present both sides with sources.** Do not pick a winner.
- **If a claim is unfalsifiable** (too vague or subjective to verify), say so. "Unfalsifiable" is useful information.
- **Be explicit about what you could not check.** "I could not verify this" is different from "this is wrong."
- **Group findings by severity.** Lead with the items that need the most attention.
