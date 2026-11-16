# Rust security audit — consolidation

You are consolidating a finished Rust security audit. You receive the scope model, build truth,
supply-chain review, taint plan, per-file reviews, fact judgements, unsafe-cluster reviews,
sink reviews, configuration review, cryptography and authorization review, and the current
coverage view. Do not read the repository and do
not run tools; every claim must already exist in the inputs.

## Your task

1. **Merge duplicates.** One defect is one finding, even when several files, tools, unsafe
   clusters or agent reviews describe it. Use the stable fingerprint, the fact ids and the call
   path to decide.
2. **Keep provenance.** For every finding: tool-derived, AI-confirmed, AI-only or refuted; the tool
   id and version; the supporting fact, review or cluster ids.
3. **Rank.** Severity from impact and reachability in this workspace, not from the tool label.
   State confidence honestly; a reachable-unproven advisory is not an exploitable one.
4. **Coverage.** Files reviewed, facts confirmed/refuted/needs-context, unsafe clusters audited,
   sink classes traced, advisories judged, crypto and authorization paths checked — and what
   remains uncovered, by crate, target and feature set. State whether macro-expanded code was
   available, and that unsafe verdicts are static; a missing stream is a gap, not a clean result.
5. **Gaps.** Files, fact groups, unsafe clusters, sink instances or advisories that were not
   reviewed, or `needs-context` judgements without a follow-up.

## Rules

- No finding without evidence (file, line range, quote, fingerprint).
- Consolidation never invents a defect; it only merges, ranks and explains.
- A refuted tool fact stays refuted and is retained in the audit trail.
- Say `clean` only when every declared input is accounted for and every coverage dimension is
  closed or explicitly listed as a gap.

## What consolidation merges

The finding ledger already holds every observation the earlier steps reported. Merge those
observations with the tool facts, rank them, and explain each; do not re-derive a defect from a
prose answer alone. A defect that was only described in a step answer, never reported as an
observation, is not in the ledger and must be reported now or listed as a gap.

## Delivering the answer

This step produces three answers. Call `audit_report` with `outcome: "done"` and one entry per
name:

```json
"answers": [
  {"name": "findings", "body": { "findings": [ … ] }},
  {"name": "coverage", "body": { "coverage": [ … ] }},
  {"name": "gaps", "body": { "gaps": [ … ] }}
]
```

Every declared name must appear exactly once. The unit cannot complete without them, and the
export reads them under those names.
