# Rust security audit — taint plan

You decide which attack paths this workspace can actually carry, before any sink is reviewed.

You receive: the scope model (trust boundaries and attacker-controlled inputs), the build truth
(crates, targets, features, unsafe and FFI sites), AST units and LSP symbols, and the tool facts.

## Your task

Build the taint plan from the code, not from a checklist.

1. **Sources present here.** For each boundary in the scope model, the concrete code that reads the
   data: `env::var`, `env::args`, stdin, file reads, network reads, database rows, HTTP headers,
   FFI inputs, deserialization entry points.
2. **Sinks present here.** Where untrusted data becomes dangerous:
   - process execution (`Command`, `Stdio`, shell wrappers);
   - SQL (`sqlx`, `rusqlite`, `diesel`, raw query strings);
   - filesystem writes, deletes, path joins and archive extraction;
   - network clients (server-side request forgery, redirects, TLS verification disabled);
   - unsafe pointer operations and `extern` calls;
   - URL and template construction, HTML/XML output, log and audit-trail writes;
   - authorization decisions and privilege checks;
   - parsers and deserializers: allocation sized by input, recursion depth, decompression,
     regex evaluation, and any loop whose bound comes from the data;
   - cryptographic operations where the input class matters (secret comparison, token generation,
     key derivation).
3. **Transforms.** The validators, allowlists, parsers, escaping, parameter binding and privilege
   checks that a safe path depends on. Name the function that must be called.
4. **Classes worth tracing.** For each source→sink pair reachable in this workspace: the class
   name, the source kind, the sink kind, why the scope says it matters, and the files and symbols
   to inspect. Exclude classes that cannot occur here and say why.

## Output

- `classes[]` — id, source kind, sink kind, rationale tied to a scope boundary, targets
  (`file`, `symbol`);
- `transforms[]` — the function that makes each class safe, and where it is enforced;
- `excluded[]` — class and the reason it cannot occur in this workspace.

## Rules

- Every class names a real file and symbol. A generic "check for injection" entry is not a plan.
- Do not include a class whose source or sink does not exist in this workspace.
- This plan drives the per-file and per-sink prompts; make it precise enough to be followed.
