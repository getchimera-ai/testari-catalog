# AGENTS.md — testari-catalog

This repository holds the catalog: a set of self-contained audit definitions plus its
documentation. It is the public audit catalog for Testari. Agents never add application code,
scripts or generated artefacts.

## The model, holistically

An audit is a reusable definition of an inspection. Testari is not limited to one language or one
kind of question: the catalog can describe security, design, dependencies, configuration,
accessibility, privacy, compliance or any other domain a person or agent can inspect with evidence.

- **Checks** are deterministic analysis. A check names a language or artefact type Testari
  analyses deterministically and applies to exactly one of them. Never invent a checker name.
- **Review** audits give the agent explicit criteria. Each criterion is a concrete question with a
  category and severity; findings must carry file, line and quoted evidence. Review audits work for
  any domain, including ones without a structural checker, and are the honest form when no checker
  exists.
- An audit may combine both styles when the domain has a checker and additional judgement.

Every audit entry also declares the languages and units it needs and its tools for macOS, Windows
and Linux (an executable to detect and a supported package-manager install plan). That is what lets
Testari group an audit as runnable, needs-capability or optional before a run starts.

## Data rules

- Identifiers are stable and lowercase. Never rename or reuse a published identifier.
- Use the languages, categories, severities and tool definitions the catalog already uses. Ask in
  an issue before inventing a new shape.
- Required tools are declared per operating system with an executable name and, when verified, a
  supported package-manager plan. Never a command string. An unverified tool carries a source and
  no install plan.
- Applied audits are deprecated with a reason, never deleted. Runs keep the catalog they started
  with.
- No credentials, personal data, customer data or third-party content without a recorded licence
  and provenance.

## Proposing

People propose by voice in Testari → Catalog → Talk to Testari. Agents propose through Testari's
proposal tool: propose the audit, let the person review it in Testari → Catalog → Drafts, apply and
publish it. A direct pull request is valid when a person asked for it.

## Change discipline

- One focused change per pull request, with a short imperative subject.
- Bump the catalog version for any content change. A removal or a changed meaning needs a
  deprecation period; the maintainer agrees the release number in review.
- `main` is protected: pull requests only, no force pushes, no direct commits, linear history.
- Every commit carries `Signed-off-by: Name <email>` and the inbound grant in `CONTRIBUTING.md`.
- Proprietary repository: never add an Apache-2.0 or other permissive licence, copyright header or
  product declaration. `LICENSE` is the only licence text.

## Validation and review

There is no build here. A maintainer validates every change with Testari itself before merging; a
catalog Testari refuses is not merged. Do not claim a check, tool or test that does not exist — a
blocked or review-style proposal is the honest answer. Do not run or embed install commands; the
catalog declares them, the person copies them.

## What not to do

- Do not add product code, scripts, CI secrets, lockfiles or build output.
- Do not change Testari's software from this repository; propose that in the product repository.
- Do not rewrite published history.
