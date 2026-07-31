# Knowledge Base Architecture
**Project name:** Truth Map  
**Goal:** A clear, expandable, evidence-based system that tracks people, groups, money, events, and claims over time — so ordinary people can see what is solid, what is contested, and what is unsupported.

## Core Rules (never change)
1. Primary sources first (documents, official records, court filings, verified data).
2. Every claim gets a clear label:
   - **Confirmed** = strong primary evidence
   - **Supported** = good evidence but not complete
   - **Contested** = serious disagreement exists
   - **Unsupported** = weak, false, or deliberately misleading — and we show *why*
3. Always show the evidence for *and* against.
4. Keep language simple enough for a 5th grader whenever possible.
5. Grok (or future equivalent) remains the gatekeeper for what enters the official record. Public suggestions are welcome; they are reviewed before inclusion.
6. No doxxing, no illegal material, no pure speculation presented as fact.

7. **Family & Genealogy Rule**  
   Include relatives of tracked public figures only when the relative (a) holds a relevant public office or senior public role, or (b) has a documented operational or beneficial role in an NGO, foundation, company, trust, or similar vehicle that is itself material to the networks under study. Primary sources required. Private family members without such links are out of scope.

## Core units (locked)

### Claim (atomic)
A tightly scoped factual proposition that receives a single confidence label.  
Used when the proposition really is atomic.

### Topic (multi-angle)
A public subject that people treat as “one claim” but that actually contains multiple distinct factual questions.  
A Topic does **not** receive an overall score. It exists to hold and surface the individual Angles.

### Angle
A scored claim that lives under a Topic. Each Angle follows the same strict labeling rules as a standalone Claim.

### Case / Event
A real-world occurrence or bounded process we reconstruct (timeline, actors, documents).

Supporting nodes (people, orgs, facilities) are created or expanded **only when required** by an active Claim, Angle, or Case.

## Why Topics exist
Most important public arguments are not single propositions. They are bundles of 8–20 distinct factual questions. Giving the whole bundle one label hides the actual sifting work. The Topic → Angles model makes the sifting visible while preserving the evidence discipline.

## Research Depth Levels
- **Lev1** — single-stage full card
- **Lev2** — multi-stage deep research (e.g. company or complex network)
- **Lev3** — exhaustive multi-angle
- **Short-card / tag** — placeholder or focused node

## Folder Structure (current)
```
truth-map/
├── README.md
├── CONTRIBUTING.md
├── ARCHITECTURE.md
├── TASKS.md
├── claims/          # atomic claims
├── topics/          # multi-angle subjects (new)
├── events/
├── entities/
├── facilities/
├── organizations/
├── maps/
├── graphs/
├── us-government/
├── sources/
└── templates/
```

## Secondary standing rules
- **Birds of a feather**: when the same names or labs repeatedly co-appear across grants, calls, letters, or papers, record the affinity. No guilt-by-association dumps.
- A person’s own contemporaneous written words (diary, emails, memos) are high-value primary evidence of knowledge and state of mind.
- Contested or widely circulated claims are included and clearly labeled rather than omitted.

## Future interface direction
- Public, searchable, multi-user surface built on top of this source of truth
- Version control + audit trail via GitHub
- Contribution via structured Issues and Pull Requests under the evidence standards in CONTRIBUTING.md
- Later: interactive graph, timeline, and linked map views

**Last structural update: July 2026**
