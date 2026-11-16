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

## Contribute with Testari

You do not need git.

1. Open **Testari → Catalog → Talk to Testari** and say what you want audited. It is a voice
   conversation: you speak, Testari answers, researches the topic and proposes the audit. It knows
   the catalog and the audits that already exist and splits a broad request into the right separate
   audits. Agents connected to Testari propose the same way through its local MCP server.
2. Every proposal appears in **Testari → Catalog → Drafts**, with its rationale, review criteria,
   required tools and any overlap with existing audits. Similar proposals are grouped so you can
   merge them into one audit or keep them separate.
3. Review the draft and press **Apply**. Testari validates the catalog with its own loader and
   commits the change on your own catalog branch; **Publish** then opens the pull request for you.

You review, apply and publish; the maintainers merge. Agents never change the catalog themselves.

Issues and pull requests are equally welcome in this repository. An audit is one self-contained
entry: its checks or review criteria, the languages it needs and its tools for macOS, Windows and
Linux. Applied audits are never removed; a retired audit is marked deprecated so older runs keep
their history. See [`CONTRIBUTING.md`](CONTRIBUTING.md).

## Licence

Proprietary and all rights reserved. Reading and using this catalog with Testari is permitted;
copying, modifying or redistributing it outside the contribution process is not. See
[`LICENSE`](LICENSE).
