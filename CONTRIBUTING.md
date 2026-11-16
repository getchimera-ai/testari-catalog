# Contributing to the Testari Catalog

You can contribute an audit, a check, a review criterion or the tools an audit needs — for any
language, framework or domain. A contribution is one audit entry: what it inspects, its checks or
review criteria, the languages it needs and its tools for macOS, Windows and Linux. There is no
application code in this repository.

## Propose from Testari

You do not need git.

Open **Testari → Catalog → Talk to Testari** and say what you want audited. It is a voice
conversation: you speak, Testari answers, researches the domain and proposes the audit. It knows
the catalog and the audits that already exist and splits a broad request into the right separate
audits. Agents connected to Testari propose the same way through its local MCP server.

Every proposal appears in **Testari → Catalog → Drafts** with its rationale, review criteria,
required tools and any overlap with existing audits. Review it, press **Apply** (Testari validates
it with its own loader and commits it on your catalog branch), then **Publish** to open the pull
request. You review and publish; a maintainer merges. Agents cannot change the catalog themselves.

## Contribute with a pull request

You can also add or improve an audit entry directly and open a pull request. One audit or one
focused change per pull request.

## Suggesting before building

Open an issue first. Say what should be inspected, which languages or artefacts it applies to, what
evidence a finding should carry, and which tools (if any) are required. If the domain has no
structural analysis, use the review style; it covers any domain.

## Two audit styles

- **Checks** are deterministic analysis. A check names a language or artefact type Testari
  analyses deterministically and applies to exactly one of them. When a domain has no structural
  analysis, use the review style.
- **Review** audits give the agent explicit criteria. Each criterion is a concrete question with a
  category and severity; the agent answers with file, line and quoted evidence. This is the right
  form for design, architecture, dependencies, configuration, accessibility, privacy, compliance
  and any domain without a structural checker.

An audit may combine both styles when the domain has a checker and additional judgement.

## What an audit entry must satisfy

- Identifiers, categories and severity levels follow what the catalog already uses. Identifiers are
  stable: never rename or reuse a published one.
- Review criteria are evidence questions, not general advice.
- Required tools are declared per operating system — macOS, Windows and Linux — each with an
  executable name to detect and a supported package-manager plan to install. Never a command
  string. A tool you have not verified stays marked unverified and carries a source.
- The languages and units the audit needs are declared with the audit, so Testari can group it as
  runnable, needs-capability or optional before a run starts.
- Do not remove or rename an audit that has shipped. Mark it deprecated with a reason instead.
- Bump the catalog version for any content change; the maintainers will agree the release number
  in review.
- No credentials, personal data, customer data or third-party content without a recorded licence
  and provenance.

## Sign-off

Every commit needs a Developer Certificate of Origin sign-off:

```
Signed-off-by: Your Name <you@example.com>
```

By submitting a contribution you certify that you wrote it or have the right to submit it, and you
license it to Chimera GmbH for distribution as part of this catalog under the repository's
proprietary terms.

## Review

A maintainer runs the catalog through Testari before merging; a catalog that does not load is not
merged. `main` is protected: pull requests only, no force pushes, no direct commits.

## Reporting problems

Open an issue with the catalog version, the audit name and what you expected. Do not open a public
issue for a security problem in Testari itself; contact the maintainers privately.
