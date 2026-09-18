# WL Debugging and Audit Reference

## Symptom-to-cause map
- **Expression unexpectedly evaluates** → missing hold wrapper, eager Set (`=`) instead of delayed Set (`:=`).
- **Rules do nothing** → wrong head/structure (`FullForm` mismatch), condition never true.
- **Infinite or huge rewrite** → non-decreasing transform, circular rule pair.
- **Order-dependent behavior** → hidden notebook state, mutable globals, memoization tied to stale defs.
- **Numeric nonsense** → mixed precision, assumptions omitted, branch cuts ignored.

## Minimal debugging protocol
1. Freeze and inspect form:
   - `HoldForm[expr]`
   - `FullForm[expr]`
2. Inspect symbol definitions:
   - `Definition[sym]`
   - `DownValues[sym]`
   - `UpValues[sym]`
3. Trace a narrow slice:
   - `Trace[expr, _targetHead, TraceOriginal -> True]`
4. Test rule termination:
   - apply rewrite with max-iteration cap and monitor size.
5. Compare exact vs numeric pipelines:
   - exact baseline on small case, numeric on realistic case.

## Audit rubric

### Correctness
- Contracts explicit?
- Edge cases covered?
- Symbolic and numeric branches consistent?

### Reliability
- Failures structured and documented?
- Options validated?
- Deterministic output under repeated runs?

### Maintainability
- Public/private separation clear?
- Context discipline and naming clear?
- Reusable transforms extracted?

### Performance
- Avoids repeated expensive simplification?
- Uses memoization only where lifecycle is controlled?
- Prevents exact-arithmetic blowup where not needed?

## Notebook-specific pitfalls
- Hidden initialization cells not reflected in source control.
- Execution-order coupling between sections.
- Global side effects from ad hoc exploratory cells.

Mitigation: move stable logic into `.wl` package files and keep notebook thin.
