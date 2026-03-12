# Active Mode

Active mode provides persistent verification for the duration of a conversation. When activated, every substantive response is automatically checked for verifiable claims, and the user sees a verification summary without having to ask for it.

---

## Activation

When the user activates doublecheck without providing specific text to verify, enable persistent verification mode. Respond with:

> **Doublecheck is now active.** I'll verify factual claims in my responses before presenting them. You'll see an inline verification summary after each substantive response. Say "full report" on any response to get the complete three-layer verification with detailed sourcing. Turn it off anytime by saying "turn off doublecheck."

Then follow ALL of the rules below for the remainder of the conversation.

---

## Rule: Classify every response before sending it.

Before producing any substantive response, determine whether it contains verifiable claims. Use this decision tree:

### Step 1: Does the response contain verifiable claims?

| Response type | Contains verifiable claims? | Go to |
|--------------|---------------------------|-------|
| Substantive analysis, research, or guidance on topics where errors carry real consequences | Yes -- high density | Step 2A |
| Summary of a document, data, or research | Yes -- moderate density | Step 2B |
| Code generation, creative writing, brainstorming | Rarely | Step 3 |
| Casual conversation, clarifying questions, status updates | No | Step 3 |

### Step 2A: High-density claims in consequential content

Produce the **full verification report** using the template in `templates/verification-report.md`. Content where errors could lead to real-world harm or flawed decision-making should always receive the detailed treatment. Do not use inline verification for this content type -- the abbreviated format is not sufficient.

### Step 2B: Moderate-density claims

Run **inline verification** (see format below). Focus on key claims. You do not need to verify common knowledge or claims you have high confidence about -- rate them PLAUSIBLE and move on.

### Step 3: No verification needed

For code, creative content, and casual conversation, skip verification. If skipping on a response that might seem like it should be verified, briefly note that doublecheck does not apply to this type of content.

---

## Rule: Inline verification format.

When inline verification applies, embed it directly in your response using this format:

1. Generate your response normally.
2. After the response, add a verification section.
3. List each verifiable claim with its confidence rating and a source link where available.

```
---
**Verification (N claims checked)**

- [VERIFIED] "Claim text" -- Source: [URL]
- [PLAUSIBLE] "Claim text" -- no specific source found
- [UNVERIFIED] "Claim text" -- could not find supporting evidence
- [FABRICATION RISK] "Claim text" -- could not find this citation; verify before relying on it
```

Prioritize speed. Run web searches for citations, specific statistics, and any claim you have low confidence about. Claims that are common knowledge can be rated PLAUSIBLE without a search.

Always append this line at the end of the inline verification section:

```
_Say "full report" for detailed three-layer verification with sources._
```

---

## Rule: Auto-escalate to full report for high-risk findings.

If your inline verification identifies ANY claim rated DISPUTED or FABRICATION RISK:

1. Place a prominent callout at the top of your response:

```
**Heads up:** I'm not confident about [specific claim]. I couldn't find a supporting source. You should verify this independently before relying on it.
```

2. Replace the inline verification with the full three-layer verification report using the template in `templates/verification-report.md`.

The user should not have to ask for the detailed report when something is clearly wrong.

---

## Rule: Offer full verification on request.

If the user says "full report," "run full verification," "verify that," "doublecheck that," or similar, run the complete three-layer pipeline (as described in `core/verification-pipeline.md`) and produce the full report using the template in `templates/verification-report.md`.

---

## Deactivation

When the user says "turn off doublecheck," "stop doublecheck," or similar, respond with:

> **Doublecheck is now off.** I'll respond normally without inline verification. You can reactivate it anytime.
