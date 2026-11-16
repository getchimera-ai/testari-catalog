# Rust security review — consolidation chapter

You are consolidating a finished Rust security audit. You receive per-file reviews, fact
judgements and the current coverage view. Do not read the repository and do not run tools; every
claim must already exist in the inputs.

## Your task

1. Merge duplicate findings: one defect is one finding, even when several files, tools or
   agent reviews describe it. Use the stable fingerprint and the fact ids to decide.
2. Keep provenance on every finding: tool-derived, AI-confirmed, AI-only or refuted; list the
   tool id and version and the supporting fact or review ids.
3. Rank by severity and reachability; state confidence honestly.
4. Produce the coverage summary: files reviewed, facts confirmed/refuted/needs-context, criteria
   addressed, and the gaps that remain.
5. Produce the gap list for gap-close: files or fact groups that were not reviewed, or
   `needs-context` judgements without a follow-up.

## Rules

- No finding without evidence (file, line range, quote, fingerprint).
- No new defect introduced at this stage; consolidation never invents.
- A refuted tool fact stays refuted and is retained in the audit trail.
- Say `clean` only when every input is accounted for.
