# Rust security audit — unsafe cluster review

You are auditing one cluster of unsafe Rust: a contiguous unsafe region that relies on one
invariant. You receive its AST units, surrounding LSP symbols and references, related units,
project context and the build truth. Do not re-read unrelated files.

## Your task

1. **State the invariant.** In one sentence, what must be true for this unsafe code to be sound?
   Examples: the pointer is valid, aligned, initialized and uniquely borrowed; the length matches
   the allocation; the value is not moved while pinned; the type layout is what `transmute`
   assumes; the data race cannot occur because of the `Sync` contract; the caller upholds the
   documented precondition.
2. **Check every operation against it.** For each unsafe operation in the cluster: which
   obligation it relies on, and whether the code (not a comment) establishes it. Look for:
   - raw-pointer arithmetic, provenance, aliasing and lifetime assumptions;
   - `transmute` and `union` layouts, including padding and `repr`;
   - `Send`/`Sync` implementations and auto-trait assumptions;
   - pin projection and move-out of pinned values;
   - bounds and length computations, integer overflow in size arithmetic;
   - initialization and drop order, double free, use after free;
   - unwinding across FFI, `panic = "abort"` assumptions;
   - FFI signatures, ownership transfer and null handling.
3. **Check callers.** Use LSP references to find every caller. A precondition that callers do not
   uphold is an unsoundness even when the block itself is correct.
4. **Verdict.** `sound`, `unsound` (name the obligation that is not met, with evidence) or
   `needs-context` (name the file, symbol or dependency version required).

## Evidence bar

- quote the unsafe operation and the line that establishes or violates the invariant, with
  `file:line` for both;
- a comment is not proof; cite code;
- if the soundness argument depends on a dependency's implementation, name the crate and version
  and mark the verdict `needs-context` when that source is not in the context.

## Output

One cluster review: invariant, per-operation findings, caller check, verdict, remediation.

## Delivering the answer

Report this document as the step's answer: call `audit_report` with `outcome: "done"` and
`answers: [{"name": "unsafe-reviews", "body": { …the document above… }}]`. The unit cannot complete
without it, and later steps read it under that name.
