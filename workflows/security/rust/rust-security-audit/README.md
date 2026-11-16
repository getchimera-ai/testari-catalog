# Rust security audit

Find real security defects in a Rust workspace and report only findings with exact evidence.

## What it inspects

Rust source files in the workspace, together with what the toolchain already knows about them:

- **cargo metadata** — packages, targets and dependency graph
- **cargo clippy** — lints, correctness and suspicious constructs
- **cargo audit** — known vulnerable dependencies
- **cargo deny** — licences, bans, sources and advisories
- **built-in detectors** — unsafe blocks, SQL built from input, weak crypto, hardcoded secrets and
  command injection
- **rust-analyzer** — symbols for every reviewed file, so the review gets structure instead of raw text

## What counts as a finding

A defect is reported only with:

- the exact file and line range;
- a quote copied from the file;
- the rule id or tool fact id that supports it;
- severity, category, optional CWE and a one-sentence remediation.

A tool fact is not a finding by itself. Each fact is confirmed or refuted against the code, and one
defect stays one finding even when several tools or reviews describe it.

## What you get

- findings with evidence and provenance (tool-derived, AI-confirmed, AI-only);
- a coverage summary: files reviewed, facts confirmed or refuted, gaps that remain;
- refuted tool facts retained in the audit trail, so nothing is silently dropped.

## Requirements

Rust and `cargo` are required. `cargo-audit`, `cargo-deny` and `rust-analyzer` are detected before
the audit starts; the audit refuses to start when a declared tool is missing rather than silently
skipping it.

Everything runs locally. No upload, no account, no telemetry.
