# CLAUDE.md — testari-catalog

@AGENTS.md

This repository is the public audit catalog for Testari: self-contained audit definitions plus
documentation. Read `AGENTS.md` for the model and the review process.

- Data only: no code, scripts or build output.
- Audits can be deterministic checks, agent-led reviews, or both — for any language, framework or
  domain. When no checker exists, propose the review form rather than inventing one.
- Every audit declares the languages and units it needs and its tools for macOS, Windows and Linux.
- Identifiers are stable; applied audits are deprecated, never deleted.
- Every change goes through a pull request with a `Signed-off-by` line; `main` is protected.
- Testari validates the catalog before merge; never claim a check or tool that does not exist.
- Proprietary: `LICENSE` is the only licence text.
