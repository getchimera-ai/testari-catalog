# Testari Catalog

The public audit catalog for **Testari**, Chimera's local audit product. Testari reads this catalog
to know which audits exist, what each one inspects, what evidence counts and which tools it needs.
Everything runs on your machine; the catalog is data, and Testari never executes anything from it.

**Proprietary.** Copyright (c) 2026 Chimera GmbH. See [`LICENSE`](LICENSE).

## What an audit is

An audit is a reusable definition of an inspection. The catalog is open-ended by design: it can
describe code security, architecture and design, dependencies, configuration, accessibility,
privacy, compliance — anything a person or agent can inspect with evidence. Two styles are
supported, and an audit can combine them:

- **Checks** — deterministic, repeatable analysis for languages and artefacts Testari can analyse
  directly. The same input gives the same result every time.
- **Review** — the audit gives the agent explicit review criteria. The agent inspects the
  repository, answers each criterion with file, line and quoted evidence, and the audit fails
  closed when evidence is missing. Review audits work for any domain, including ones Testari
  cannot analyse structurally.

Both styles plan, execute and verify their work locally, keep a record of what was inspected, and
produce findings with exact locations. Every audit also declares the tools it needs for macOS,
Windows and Linux, so Testari can tell you what is missing before a run starts.

## How to use it

Testari ships with this catalog and keeps it up to date automatically. At launch it fetches the
catalog, updates its local copy and rebases when the tree is clean; you do not press Sync. The
catalog is validated before it is used, and an invalid catalog is refused, never half-applied.

If the repository cannot be reached, Testari keeps using the last copy it had, and the bundled copy
always works offline. You are only asked to act when something needs you: a merge conflict, a
locally modified catalog, or a remote that stays unreachable — and then a single retry is enough.
A custom remote can be set in Testari → Catalog → Settings; it is synced the same way.

No account, no upload and no telemetry: the catalog is fetched with your own git credentials and
validated locally.

## Contribute

An agent connected to Testari's local MCP server can write an audit proposal with
`catalog_propose`. The proposal appears in **Testari → Catalog → Drafts** with its rationale,
review criteria, required tools and any overlap with existing audits; you review it and take it
into a pull request. Agents never change the catalog themselves.

This repository is organised as workflow folders, and this build's catalog editor writes a single
`catalog.yaml`: drafting, applying and publishing a workflow-folder catalog from the app are not
supported yet, and the voice authoring surface ("Talk to Testari") is not part of this build. So a
change to an audit here is a pull request — see [`CONTRIBUTING.md`](CONTRIBUTING.md).

Issues and pull requests are equally welcome in this repository. An audit is one self-contained
entry: its checks or review criteria, the languages it needs and its tools for macOS, Windows and
Linux. Applied audits are never removed; a retired audit is marked deprecated so older runs keep
their history.

## Licence

Proprietary and all rights reserved. Reading and using this catalog with Testari is permitted;
copying, modifying or redistributing it outside the contribution process is not. See
[`LICENSE`](LICENSE).
