# Architecture

## Core units

- **Claim** = tightly scoped factual proposition that receives a single confidence label (Confirmed / Supported / Contested / Unsupported). Used when the proposition is truly atomic.
- **Topic** = public subject that people treat as one claim but that actually contains multiple distinct factual questions. A Topic itself does **not** get an overall score. It holds the Angles.
- **Angle** = a scored claim that lives under a Topic. Same labeling rules as a standalone Claim.
- **Case / Event** = real-world occurrence or bounded process we reconstruct (timeline, actors, documents).

Supporting nodes (people, orgs, facilities) are created or expanded **only when required** by an active Claim, Angle, or Case.

## Why Topics exist

Most important public arguments are not single propositions. They are bundles of many distinct factual questions. Giving the whole bundle one label hides the actual sifting work. The Topic → Angles model makes the sifting visible while preserving the evidence discipline.

## Research depth levels

- **Lev1** — single-stage full card
- **Lev2** — multi-stage deep research
- **Lev3** — exhaustive multi-angle
- **Short-card** — focused node

## Secondary standing rules

- **Birds of a feather**: when the same names or labs repeatedly co-appear across grants, calls, letters, or papers, record the affinity. No guilt-by-association dumps.
- A person’s own contemporaneous written words (diary, emails, memos) are high-value primary evidence of knowledge and state of mind.
- Contested or widely circulated claims are included and clearly labeled rather than omitted.

## Source of truth

The full structured knowledge base lives in the [GitHub repository](https://github.com/Truth-Map/truth-map). This site is the browsable public surface generated from that source.
