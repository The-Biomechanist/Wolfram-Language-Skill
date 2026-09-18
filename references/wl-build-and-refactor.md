# WL Build and Refactor Reference

## Package skeleton
```wl
BeginPackage["MyContext`"];

myFunction::usage = "myFunction[x, opts] computes ...";
Options[myFunction] = {Method -> "Automatic", Tolerance -> 10^-8};

Begin["`Private`"];

myFunction::badx = "Argument x=`1` is invalid. Expected ...";
myFunction::badopt = "Option `1` -> `2` is invalid.";

myFunction[x_, opts : OptionsPattern[]] := Module[
  {method = OptionValue[Method], tol = OptionValue[Tolerance], nx},
  nx = normalizeInput[x];
  If[FailureQ[nx], Return[nx]];
  Which[
    !MemberQ[{"Automatic", "Exact", "Numeric"}, method],
      Failure["InvalidOption", <|"Option" -> Method, "Value" -> method|>],
    !NumericQ[tol] || tol <= 0,
      Failure["InvalidOption", <|"Option" -> Tolerance, "Value" -> tol|>],
    True,
      compute[nx, method, tol]
  ]
];

normalizeInput[x_] := Which[
  AssociationQ[x], x,
  ListQ[x], AssociationThread[Range[Length[x]], x],
  True, Failure["InvalidInput", <|"Input" -> HoldForm[x]|>]
];

compute[nx_, method_, tol_] := nx;

End[];
EndPackage[];
```

## Refactor checklist
- Consolidate ad-hoc argument coercion into one `normalize*` function.
- Replace repeated literal options validation with helper predicates.
- Move notebook initialization cells into package init path.
- Isolate deterministic pure transforms from side-effecting I/O.
- Add usage messages for all public symbols.

## Rewrite-system safety patterns
- Prefer staged rewrites:
  1. canonicalization;
  2. simplification;
  3. domain-specific optimization.
- Use bounded loops for iterative rewrites.
- Capture metrics: count rewrite iterations and expression size deltas.

## UpValues/DownValues guidance
- Use UpValues only for ownership semantics tied to custom heads.
- Keep UpValues in the same package as the owned head.
- Audit for collisions with:
  - built-in symbol names,
  - overloaded pattern catch-alls,
  - cross-context injections.
