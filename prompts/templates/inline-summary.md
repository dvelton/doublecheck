# Inline Verification Summary Template

Use this template when producing inline verification in active mode (moderate-density claims in non-high-stakes content).

---

Place this section after your response, separated by a horizontal rule:

```
---
**Verification (N claims checked)**

- [VERIFIED] "Claim text" -- Source: [URL]
- [VERIFIED] "Claim text" -- Source: [URL]
- [PLAUSIBLE] "Claim text" -- no specific source found
- [UNVERIFIED] "Claim text" -- could not find supporting evidence
- [DISPUTED] "Claim text" -- contradicting source: [URL]
- [FABRICATION RISK] "Claim text" -- could not find this citation; verify before relying on it

_Say "full report" for detailed three-layer verification with sources._
```

## Guidelines

- Include the claim count in the header.
- Quote the specific claim text so the user knows exactly what was checked.
- Always include a source URL for VERIFIED claims.
- For DISPUTED claims, include the contradicting source URL.
- For FABRICATION RISK claims, briefly note why (e.g., "citation not found," "statistic has no identifiable source").
- PLAUSIBLE claims need only a brief note that no specific source was located.
- Keep it concise. The inline format is meant for quick scanning, not deep analysis.
- Always end with the "full report" prompt so the user knows the option exists.

## Auto-Escalation

If any claim in the inline verification is rated DISPUTED or FABRICATION RISK, do not use this inline format. Instead, auto-escalate to the full verification report (see `modes/active-mode.md` for the escalation rule).
