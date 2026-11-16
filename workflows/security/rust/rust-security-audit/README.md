# Rust security audit

A complete security audit of a Rust workspace: it maps what the workspace exposes, what it is
built from, what untrusted data can reach, where unsafe code makes promises, and what the
configuration lets through. It reports only defects with exact evidence.

## How it works

1. **Scope and threat model** — finds the trust boundaries (network, CLI, environment, files,
   database, deserialization, FFI), the assets at risk and the attacker-controlled inputs. Says
   plainly what is out of scope.
2. **Build truth** — resolves the real build: workspace members and target kinds, enabled
   features and cfg-gated code, the cfg flags and environment each build script emits, proc macros,
   every unsafe site and FFI boundary, panic and overflow settings. A finding based on the wrong
   build is not a finding.
3. **Supply chain** — judges `cargo audit` advisories, `cargo deny` bans and licences, unmaintained
   and yanked crates, git and path dependencies, patched forks, and the build scripts and proc
   macros that execute code during compilation. Each advisory is checked for reachability, with a
   cited call path.
4. **Taint plan** — from the scope and the build, decides which source-to-sink classes actually
   exist here: command execution, SQL, filesystem and path handling, network clients, unsafe
   pointers, FFI, URL and template construction, authorization decisions.
5. **Per-file review** — one review per file, with AST units, LSP symbols and references, tool
   facts, project context, the scope model and the taint plan. Facts are confirmed or refuted
   against reachability; contextual defects the tools cannot see are added.
6. **Fact triage** — one review per fact group, with reachability established through symbol
   references and severity adjusted to this workspace.
7. **Unsafe audit** — one review per unsafe cluster: the invariant it relies on, every operation
   checked against it, and every caller checked for the precondition it must uphold.
8. **Sink tracing** — one review per dangerous sink: the argument traced back through every source,
   the transform that must stand in between, and a verdict of exploitable, not exploitable or
   unknown.
9. **Configuration and secrets** — every secret candidate from gitleaks confirmed or refuted, plus
   custom formats it cannot see; secrets in any file are reported redacted, never reproduced. CI
   injection and token exposure, container and deployment hardening, cargo profiles and build
   flags, TLS and network defaults.
10. **Cryptography and authorization** — algorithm and randomness misuse, constant-time secret
    comparison, password storage, key handling, token verification (algorithm pinning, issuer,
    audience, expiry), session handling, and every privileged operation checked against a
    server-side identity rather than a caller-supplied id.
11. **Consolidation and verification** — duplicates merged by fingerprint, provenance kept, findings
    ranked by impact and reachability, coverage reported by crate, target, feature set, unsafe
    cluster and sink class, gaps closed within the declared bound.

## What counts as a finding

- the exact file and line range;
- a quote copied from the file;
- the rule id, tool fact id or taint class that supports it;
- severity from impact and reachability, category, optional CWE, one-sentence remediation.

A tool fact is not a finding until it is confirmed against the code. A refuted fact stays in the
audit trail with its reason. One defect stays one finding even when several tools or reviews
describe it.

## Outputs

- findings with evidence and provenance;
- coverage by file, fact, unsafe cluster, sink class, advisory, and crypto/authorization path;
- an SBOM for the exact dependency graph that was audited.

## Requirements

- Rust and `cargo`;
- `cargo-audit`, `cargo-deny`, `gitleaks`, `semgrep` and `rust-analyzer` are required and detected
  before the audit starts; the audit refuses to start when a declared tool is missing;
- `cargo-geiger` is used when installed for unsafe metrics;
- `cargo-expand` (nightly) is used when installed, so macro-generated code is analysed as real
  syntax; otherwise that limitation is reported as a coverage gap, not silently ignored.

Tool recipes and install instructions live in `software.yml` at the repository root, shared by
every workflow. Installs cover Homebrew and WinGet, plus apt, dnf, yum, zypper, apk and pacman on
Linux, and `cargo`/`pipx` for tools distributed that way. The audit never runs the workspace's
tests; unsafe verdicts are static and say so.

Reachability is established through rust-analyzer's call hierarchy and references, not by guessing
from names: advisories, lints and sinks are judged by whether workspace code can actually reach
them.

Everything runs locally. No upload, no account, no telemetry.

## Scope

This audit inspects the workspace as it is today: the code that compiles and the configuration that
ships. Repository history — secrets in past commits, force-pushes, unsigned or rewritten history —
is out of scope here and is covered by a separate git security audit.
