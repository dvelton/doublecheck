# Hallucination Patterns

A checklist of common patterns in AI-generated output that indicate potential hallucinations. Use this during the adversarial review layer (Layer 3) to actively search for these patterns in the target text.

Each pattern includes what it looks like, why it happens, and what to do when you find it.

---

## 1. Fabricated Citations

**What it looks like:** The text cites a specific document, case, standard, paper, report, or official source -- complete with a plausible-sounding title, author, date, or identifier -- but the cited source does not exist.

**Why it happens:** AI models generate text that matches the patterns of authoritative writing. Citations follow predictable formats, so the model produces something that looks correct structurally but refers to nothing real. This is the most dangerous hallucination pattern because fabricated citations carry an appearance of authority.

**What to do:** Search for every citation by its exact identifier. If you cannot find it through multiple search strategies, flag it as FABRICATION RISK. Do not soften this to UNVERIFIED -- a citation that looks specific but cannot be found anywhere is a strong signal of fabrication, not a gap in search coverage.

**Examples of what to watch for:**
- A specific document title that returns no results
- A report attributed to a real organization, but that the organization never published
- A standard or specification number that does not match any known standard
- A quote attributed to a named person that cannot be found in any source

---

## 2. Precise Numbers Without Sources

**What it looks like:** The text states a specific statistic -- a percentage, dollar amount, count, or measurement -- without indicating where the number comes from.

**Why it happens:** Models learn that specific numbers make text feel authoritative. They generate plausible-sounding statistics that fit the narrative, whether or not the numbers correspond to real data.

**What to do:** Search for the statistic and its purported source. If the text attributes the number to a source, verify that the source contains that number. If no source is mentioned and you cannot find the statistic through searching, flag it. Real statistics have traceable origins.

**Examples of what to watch for:**
- "78% of companies have adopted..." with no source cited
- A dollar figure presented as fact without attribution
- A growth rate or trend stated with decimal precision but no supporting data
- Multiple precise statistics in sequence (each one compounds the risk)

---

## 3. Confident Specificity on Uncertain Topics

**What it looks like:** The text states something very specific about a topic where specifics are genuinely unknown, contested, or still emerging. The text presents it as settled when experts would disagree or hedge.

**Why it happens:** Models are trained on text that states things confidently. They do not reliably distinguish between topics where confidence is warranted and topics where uncertainty is the honest position.

**What to do:** Consider whether the level of specificity matches the state of knowledge on the topic. Exact dates, precise dollar amounts, and definitive attributions in areas where experts disagree should be flagged. Look for the absence of qualifiers ("approximately," "estimated," "according to one analysis") where they should be present.

**Examples of what to watch for:**
- An exact date for an event that is only approximately known
- A definitive causal claim in a domain where causation is debated
- A precise threshold or cutoff that is actually a matter of interpretation
- Technical specifications stated as fact for a product or standard that is still in development

---

## 4. Plausible-but-Wrong Associations

**What it looks like:** The text connects a real concept, event, or entity to the wrong context. The individual pieces are real, but the relationship between them is incorrect.

**Why it happens:** Models store associations probabilistically. When two things frequently co-occur in training data, the model may associate them even when the specific claim is wrong. This is especially common with attributions (who said what, who did what, which organization did what).

**What to do:** Verify not just that the entities exist, but that the stated relationship between them is correct. Check that quotes are attributed to the right person, that events are attributed to the right organization, and that provisions are attributed to the right source.

**Examples of what to watch for:**
- A real quote attributed to the wrong person
- A real provision or rule attributed to the wrong source or jurisdiction
- A real event described with the wrong participants, location, or date
- A feature or capability attributed to the wrong version of a product

---

## 5. Temporal Confusion

**What it looks like:** The text describes something as current that may be outdated, presents events in the wrong chronological order, or conflates different time periods.

**Why it happens:** Models have training data cutoffs and no reliable sense of "now." They may describe a policy, rule, or fact as current when it has since changed. They may also blend information from different time periods without flagging the inconsistency.

**What to do:** Check whether time-sensitive claims are current. Look for signals like "currently," "as of," or present-tense descriptions of things that may have changed. Verify chronological sequences against known timelines.

**Examples of what to watch for:**
- A policy or rule described in present tense that has been superseded
- A price, rate, or threshold that may have changed since the training data cutoff
- Events described in the wrong order
- A person described as holding a position they no longer hold

---

## 6. Overgeneralization

**What it looks like:** The text presents something as universally true when it applies only in specific contexts, jurisdictions, time periods, or circumstances. Scope limitations are dropped.

**Why it happens:** Models compress nuanced information into simpler statements. Qualifiers like "in certain jurisdictions," "under specific conditions," or "prior to the 2024 amendment" get lost in the compression. The result reads as a universal rule when it is actually context-dependent.

**What to do:** Ask whether the claim has known exceptions, jurisdictional limits, or scope conditions that the text omits. If the claim would be true in one context but false in another, flag the missing qualifiers.

**Examples of what to watch for:**
- A rule stated as universal that actually varies by jurisdiction
- A claim about "all" or "every" instance when exceptions exist
- A historical claim generalized across time periods when circumstances changed
- A technical claim that is true for one platform, version, or configuration but not others

---

## 7. Missing Qualifiers

**What it looks like:** The text presents a nuanced topic as settled or straightforward, omitting significant exceptions, limitations, ongoing debates, or counterarguments.

**Why it happens:** Similar to overgeneralization, but specifically about the absence of hedging language. Models tend toward confident, declarative statements. Topics where the correct answer is "it depends" or "this is debated" get flattened into definitive assertions.

**What to do:** Consider whether the topic has legitimate controversy, open questions, or important caveats that the text omits. If a subject matter expert would say "well, it is more complicated than that," the text probably needs qualifiers.

**Examples of what to watch for:**
- A claim stated as fact that is actually the subject of active debate
- A recommendation presented without noting significant risks or tradeoffs
- A description of a process that omits important exceptions or edge cases
- A claim about effectiveness or impact that omits the conditions under which it was measured
