# How It Works

Doublecheck runs a three-layer verification pipeline on AI-generated text. Each layer serves a different purpose, and they build on each other.

## The Problem

AI models generate text that sounds confident and well-structured regardless of whether it is accurate. A fabricated citation looks exactly like a real one. A made-up statistic reads the same as a real one. The output does not come with reliability indicators, so the burden of verification falls on the person using it.

Manual verification works but is slow and inconsistent. You might check the first citation but skip the fifth. You might notice a suspicious statistic but miss an outdated claim. Doublecheck systematizes this process so you can verify faster and more consistently.

## Layer 1: Self-Audit

The first layer re-reads the target text critically and extracts every verifiable claim. This is analysis, not search -- no web lookups happen here.

**What it does:**

- Goes through the text sentence by sentence
- Pulls out every statement that asserts something checkable
- Assigns each claim an ID (C1, C2, C3...) for tracking
- Categorizes claims by type: factual, statistical, citation, entity, causal, temporal
- Checks whether the text contradicts itself internally
- Makes an initial confidence assessment for each claim

**Why this matters:**

Claim extraction determines what gets checked downstream. If a claim is not extracted here, it will not be verified in Layer 2 or challenged in Layer 3. The categorization also helps prioritize -- citations and precise statistics are higher-risk categories than general factual claims.

The internal consistency check catches a category of errors that does not require any external data: contradictions within the text itself. If the text says one thing in paragraph 2 and contradicts it in paragraph 7, that is a problem regardless of what external sources say.

## Layer 2: Source Verification

The second layer searches for external evidence for each extracted claim. The goal is to find URLs that the user can visit to verify claims independently.

**What it does:**

- Formulates search queries for each claim
- Runs web searches to find primary or authoritative sources
- Records results with source URLs
- Gives extra scrutiny to citations (the highest-risk category)
- Notes when a source is secondary vs. primary

**Why this matters:**

This layer converts abstract confidence assessments into concrete, checkable evidence. Instead of "I think this is right," the output is "here is a source you can check." The user does not have to trust the AI's judgment about accuracy -- they can follow the link and see for themselves.

Citations get extra attention because they are the most dangerous hallucination type. A fabricated citation looks authoritative and can lead to real consequences if acted upon (citing a nonexistent source in a document, relying on a made-up standard, etc.). When a specific citation cannot be found through multiple search strategies, that is a strong signal that it was fabricated.

**Limitations of this layer:**

- Web search does not cover everything. Paywalled content, recently published material, and specialized databases may not appear.
- Search results depend on query formulation. A claim might be verifiable but the search did not find it due to poor query construction.
- Finding a source does not guarantee the claim is correct. The source itself might be wrong.

## Layer 3: Adversarial Review

The third layer flips the entire posture. Instead of trying to verify claims, it assumes the output contains errors and actively tries to find them.

**What it does:**

Checks the text against seven hallucination patterns:

1. **Fabricated citations** -- references to sources that do not exist
2. **Precise numbers without sources** -- specific statistics with no attribution
3. **Confident specificity on uncertain topics** -- definitive claims where experts would hedge
4. **Plausible-but-wrong associations** -- real concepts connected to wrong contexts
5. **Temporal confusion** -- outdated information presented as current
6. **Overgeneralization** -- context-dependent claims stated as universal truths
7. **Missing qualifiers** -- nuanced topics presented as settled

For each major claim, it asks adversarial questions: What would make this wrong? Is there a common misconception here? Would a subject matter expert object?

**Why this matters:**

Layers 1 and 2 are constructive -- they extract and verify. Layer 3 is destructive -- it tries to break things. This shift in posture catches issues that verification alone misses. A claim might pass Layer 2 (no contradicting source found) but fail Layer 3 (the level of confidence is inappropriate for the topic, or the claim matches a known hallucination pattern).

The hallucination pattern checklist is based on observed failure modes of AI models. These patterns recur because they stem from how models generate text: they optimize for plausibility and confidence, which means their errors tend to look plausible and confident too.

## How Ratings Are Assigned

After all three layers complete, each claim gets a final confidence rating:

| Rating | What happened during verification |
|--------|----------------------------------|
| VERIFIED | Layer 2 found a supporting source |
| PLAUSIBLE | No red flags in any layer, but no specific source found |
| UNVERIFIED | Layer 2 found nothing either way |
| DISPUTED | Layer 2 found a contradicting source |
| FABRICATION RISK | Layer 3 identified a hallucination pattern match |

A claim can be downgraded by later layers. A claim that initially looked fine in Layer 1 might be contradicted in Layer 2 (DISPUTED) or flagged in Layer 3 (FABRICATION RISK). Ratings only move toward higher risk, never away from it.

## What the Tool Cannot Do

This tool does not solve the hallucination problem. It makes verification faster and more structured, and it catches many common failure patterns. But it has real limits:

- The verification runs on the same type of AI model that may have generated the original output. It shares the same knowledge limitations.
- Web search is a blunt instrument. It misses paywalled content, niche databases, and very recent information.
- The adversarial review checks for known patterns. A novel hallucination type might not match any existing pattern.
- The tool catches structural issues well (missing citations, unfindable references) but is weaker on substantive accuracy in specialized domains.

The tool raises the floor on verification quality. It does not raise the ceiling. For critical decisions, human domain expertise remains the last line of defense.
