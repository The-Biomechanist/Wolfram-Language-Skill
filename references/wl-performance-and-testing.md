# WL Performance and Testing Reference

## Performance workflow
1. Establish baseline with representative inputs.
2. Measure wall time and memory for hot functions.
3. Identify symbolic simplification hotspots.
4. Introduce targeted changes; remeasure.
5. Keep a short regression benchmark suite.

## Common optimization levers
- Replace repeated `Simplify` with precomputed assumptions and narrower transforms.
- Prefer structural transforms to full algebraic simplification when possible.
- Memoize pure deterministic subproblems with bounded key spaces.
- Compile numeric kernels when expression structure is stable and numeric-only; consider `FunctionCompile` only after the uncompiled function has a representative baseline and typed inputs can be stated without changing semantics.
- Avoid repeated conversion between exact and approximate numbers.

## Local-kernel benchmark discipline

- Keep exact and numeric baselines separate. Exact results are correctness references for small cases; machine or arbitrary-precision numeric paths need their own tolerances and residual checks.
- Use `RepeatedTiming`, `AbsoluteTiming`, `MemoryConstrained`, and small regression inputs before broad rewrites.
- Treat a faster result as suspect until output equivalence, messages, precision, and branch behavior have been checked on representative inputs.
- For `FunctionCompile`, typed constants and typed arguments can affect correctness and portability. Inspect compiler messages and keep an uncompiled reference path until parity is established.

## Testing strategy
- Unit tests for normalization and option validation.
- Property-style checks for invariants (idempotence, monotonicity, conserved quantities).
- Golden tests for canonical formatting/serialization.
- Regression tests for previously observed WL edge cases.

## `VerificationTest` template
```wl
Needs["MUnit`"];

TestReport @ {
  VerificationTest[
    myFunction[{1,2,3}],
    <|1 -> 1, 2 -> 2, 3 -> 3|>,
    TestID -> "normalizes list input"
  ],
  VerificationTest[
    FailureQ @ myFunction["bad"],
    True,
    TestID -> "rejects invalid input"
  ]
}
```

## Documentation expectations
- One-line usage for each public function.
- Option table with defaults and accepted values.
- At least one symbolic example and one numeric example.
- “Failure modes” section for actionable user recovery.
