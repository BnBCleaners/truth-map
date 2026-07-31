# Fact-Checking Assist Checklist

Use this before accepting new Evidence, Claims, or Corrections into the official record. Bots handle format; this checklist handles substance.

## 1. Source quality

- [ ] Is there at least one **primary** source (court doc, official record, grant filing, contemporaneous note, authenticated document)?
- [ ] Is the source publicly accessible or archived (URL, archive.org, court PACER/RECAP, official PDF)?
- [ ] Does the source actually say what the submitter claims? (Read the relevant passage.)
- [ ] Date of the source is clear and relevant to the claim.

## 2. Claim / evidence fit

- [ ] The submission targets a specific existing Claim/Case **or** proposes a tightly scoped new one.
- [ ] The plain-English summary matches the source without overclaiming.
- [ ] Suggested confidence label is justified by the evidence (not by preference).

## 3. Counter-evidence

- [ ] Searched briefly for contradictory primary or high-quality secondary sources.
- [ ] If counter-evidence exists, it is noted (or the label is Contested).

## 4. Process risks

- [ ] No doxxing or non-public personal data.
- [ ] No speculation presented as fact.
- [ ] No pure partisan framing without evidence.

## 5. Decision

| Outcome | Action |
|---------|--------|
| Accept | Integrate into the correct file; note source in commit/PR message |
| Needs more | Comment with specific missing items; keep `needs-info` |
| Reject | Close with short public explanation citing the standard that failed |

## Quick reference — labels

| Label | Meaning |
|-------|---------|
| **Confirmed** | Strong primary evidence, little reasonable dispute |
| **Supported** | Good evidence, still incomplete |
| **Contested** | Serious good-faith disagreement in the record |
| **Unsupported / Misleading** | Weak, false, or presented in a way that misleads — and we show why |

Gatekeepers: Grok + project owner. Bots do not decide truth.
