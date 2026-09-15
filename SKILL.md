---
name: "wolfram-language-engineer"
description: "Use this skill when the task directly concerns Wolfram Language (Mathematica) code, notebooks, packages, paclets, symbolic evaluation, proof-adjacent theorem exploration, pattern matching, numeric workflows, debugging, refactoring, testing, documentation, optimization, or audits. Prefer the local Wolfram kernel for executable checks; cloud or front-end work is conditional on an explicit request and an available environment."
---

## Instructions

# Wolfram Language Engineer

## Owned task and routing

This skill owns work whose artifact or behavior is Wolfram Language: `.wl` or `.wls` source, Wolfram packages or paclets, `.nb` notebooks, or a Wolfram evaluation/debugging question.

Activate it for:

- designing, implementing, refactoring, documenting, testing, optimizing, or auditing Wolfram Language code;
- evaluation semantics, `Hold*` behavior, pattern matching, replacement rules, `UpValues`/`DownValues`, scoping, memoization, or termination;
- symbolic, numeric, exact-precision, arbitrary-precision, `NDSolve`, optimization, linear algebra, fitting, or mixed symbolic-numeric workflows;
- local computational checking for mathematics, science, solver, model, data, graph, signal, image, geometry, units, curated-entity, or algorithmic claims when Wolfram Language can supply an exact, symbolic, high-precision, or independent computational surface;
- notebook or paclet work when the Wolfram artifact itself is part of the task.

Do not take ownership of generic mathematics implemented only in Python, Julia, Sage, or another language. Use the relevant language or mathematics skill and use this skill only when Wolfram Language behavior or an artifact must be decided. Preserve a separate front-end or notebook consumer when a task depends on visual layout, dynamic interaction, initialization cells, or other behavior a kernel cannot observe.

## Operating modes

Before implementation, identify the requested mode and preserve the user's authority boundary:

- **Fast fix** — smallest safe patch and a short verification.
- **Production** — polished API, options, validation, tests, documentation, and explicit failure behavior.
- **Audit-first** — findings and a prioritized repair plan before any edit.

Default to **Production** for an implementation request. If the user asks only to audit, review, diagnose, or explain, do not edit the target unless they separately authorize implementation.

## Local-kernel execution boundary

Local Wolfram kernel evaluation is the default executable path. Do not use cloud evaluation merely because it is available.

### Discover and validate the runtime

Before any claim depends on execution:

1. Consult the active environment's toolchain registry for a verified `wolframscript`/Wolfram Engine entry and use its registered command or stable wrapper. On this machine that registry is `~/.agents/toolchain.json`, with stable wrappers in `~/.agents/bin`; treat those values as environment state, not portable hard-coded paths.
2. If the registry is absent, stale, or incomplete, use the environment's native command discovery and inspect the resolved executable. Do not infer availability from a download, package name, or an unverified path.
3. Run a version check and one bounded, no-side-effect probe before relying on the kernel. The canonical command shapes are:

   ```text
   <WOLFRAMSCRIPT> -version
   <WOLFRAMSCRIPT> -local -code '1+1'
   ```

   Record the actual command, exit status, output, and error state only when a later claim depends on them. Installation alone is not execution evidence; activation or licensing failure blocks execution-dependent claims.

### Execute local code

- Use `<WOLFRAMSCRIPT> -local -code <WL-code>` for a small expression or focused probe.
- Use `<WOLFRAMSCRIPT> -local -file <path>` for a source or test file. Resolve the path from the active task rather than inventing a host-specific path.
- For this Windows/PowerShell environment, prefer `-file` once code contains quotes, associations, strings, multiline definitions, tests, or package loading. Inline `-code` is for small expressions whose submitted text can be inspected safely.
- Prefer a fresh local-kernel invocation for independent probes. If a stateful session is necessary, identify the definitions, packages, seeds, options, and working directory that survive between steps and verify them before use.
- If concurrent invocations produce activation or license-lock errors, rerun the probe serially before classifying the runtime as unavailable; do not assume the failure is in the Wolfram code.
- Use `-file` for multiline or string-heavy code when shell quoting could alter the Wolfram input. Inspect the file and the actual kernel output rather than treating a launched process as proof.
- Use a timeout for potentially expensive or nonterminating probes when the runtime supports it. A timeout bounds waiting; it does not prove that code is safe or that partial side effects did not occur.
- For package tests, run `VerificationTest`/`TestReport` through the local kernel and inspect the actual report, messages, exit status, and relevant changed files.

### Keep effects explicit

Treat Wolfram Language as capable of file, network, process, package-installation, and other system effects. Pure, bounded evaluation is the default. Before evaluating nontrivial code, classify its effects and keep the allowed surface no broader than the task requires.

Do not evaluate code that writes, deletes, installs, downloads, invokes external programs, changes persistent kernel/user configuration, or accesses credentials or the network unless that effect is authorized by the task. When mutation is authorized, work on a copy or disposable test location when practical, inspect the actual post-state, and reconcile an uncertain effect before retrying. Never put Wolfram credentials on a command line or ask the user to paste them into chat; use the user's secure terminal or UI action when authentication is explicitly required.

Do not use `-cloud`, `-authenticate`, `-username`, or `-password` by default. A cloud branch is allowed only when the user explicitly requests cloud execution and the required authentication boundary is available.

### Separate kernel and front-end claims

- **`.wl`/`.wls` source and package code:** use the local kernel as the primary execution and test surface.
- **`.nb` notebooks:** use the local kernel for extracted or package code. Treat front-end layout, dynamic cells, initialization-cell behavior, notebook UI state, and formatting claims as unresolved or blocked unless a front-end observation is available.
- **Paclets:** use the local kernel to test source and public API behavior. Do not install into user or system paclet directories unless authorized; prefer an isolated or disposable test location when the environment supports one.
- **Cloud APIs or deployed Wolfram artifacts:** do not enter this path implicitly. Make the cloud/deployment dependency and its observed result explicit.

If the local kernel is missing, unactivated, incompatible, or unavailable, block only the execution-dependent claim. Continue static inspection, code design, and a clearly labeled independent mathematical cross-check when useful; a Python, Julia, Sage, or hand calculation is not evidence that Wolfram evaluation semantics or package behavior is correct.

## High-leverage local uses

Use the local kernel proactively when the task contains a load-bearing mathematical, symbolic, scientific, or data claim that Wolfram can test more directly than ordinary prose. The best local uses are:

- exact symbolic checks: simplification under stated assumptions, recurrence identities, generating functions, sums/products, integrals, algebraic equivalence, units, dimensions, and special functions;
- numerical counterexample search: high-precision evaluation, interval or arbitrary-precision checks, random or grid probes, residual analysis, sensitivity checks, and exact-versus-numeric comparison;
- solver triage: `Solve`/`Reduce`/`FindInstance`/`SatisfiabilityInstances`, optimization, recurrence solving, differential equations, and constraint feasibility before committing a design;
- formal and proof-adjacent support: conjecture testing, counterexample search, finite-model checks, quantifier elimination, equational proof exploration, and example generation for Lean/SMT/formalization work, while preserving the distinction between CAS evidence and a checked formal proof;
- structural computation: graph invariants, combinatorics, automata, rewrite systems, expression trees, tensor/array shape checks, geometry/mesh queries, and discrete model exploration;
- cross-runtime arbitration: use Wolfram as an independent check against Python, Julia, Sage, R, Lean-generated examples, or handwritten derivations, while keeping Wolfram semantics separate from the other runtime's correctness;
- report artifacts: produce tables, plots, symbolic derivations, exported images, or compact data only when the task needs an artifact, and keep exports in the project or a disposable output directory.

Do not use Wolfram merely because a task mentions mathematics. Use it when exactness, symbolic transformation, curated scientific data, arbitrary precision, visualization, or a second computational opinion can change the answer or implementation.

For more detailed local-method routing and paclet guidance, load [local methods and paclets](references/wl-local-methods-and-paclets.md).

### Formal and theorem-proving boundary

Use Wolfram locally as a proof-adjacent workbench when it can shape or test a formalization before handing work to Lean, SMT, or another proof checker. Good uses include simplifying a proposition under explicit assumptions, using `Resolve` or `Reduce` for quantifier elimination over supported domains, searching for counterexamples with `FindInstance`, checking Boolean encodings with `SatisfiabilityInstances`, exploring equational proofs with `FindEquationalProof`, and generating small exact examples that a proof script should cover. Keep formulas in solver-supported algebraic, logical, Boolean, integer, real, complex, or equational fragments where possible; if a query uses arbitrary Wolfram predicates or procedural tests, treat absence of a witness as inconclusive unless the solver's coverage is established for that form.

Do not describe these results as a formal proof unless the task's accepted proof authority is Wolfram's own proof object for that theorem class and the proof object has been inspected. For Lean or SMT work, a Wolfram result is evidence, a counterexample source, or a candidate lemma generator; the proof claim remains unresolved until the target checker accepts it. When handing results to another proof system, pass the exact assumptions, domains, generated examples or counterexamples, expression forms, and any unresolved translation choices.

## Core workflow

### 1) Clarify intent and constraints

Capture the expected inputs, outputs, invariants, symbolic-versus-numeric guarantees, scale and performance targets, artifact type, local-kernel requirement, front-end dependency, and authorized side effects. If uncertainty does not change the next safe action, carry it instead of delaying the work.

### 2) Choose representation boundaries

Separate parsing and normalization, symbolic transformation, numeric realization, and formatting or export. For packages, separate public API, private implementation, tests, and documentation. Do not mix notebook state with reusable package logic without identifying the dependency.

### 3) Inspect before changing

For existing code, inspect the relevant definitions, contexts, usages, options, tests, assumptions, and call sites. For evaluation problems, inspect `HoldForm`, `FullForm`, `Definition`, `DownValues`, and `UpValues` as appropriate. Keep notebook hidden state and execution order as explicit dependencies rather than guessing them.

### 4) Implement with WL-safe patterns

Prefer clear contexts and public/private separation, `OptionsPattern[]` with typed option validation, structured `Failure[...]` results, deliberate `Module`/`Block`/`With` scoping, and bounded terminating rewrites. Normalize input once at a boundary, guard broad patterns, and keep exact-to-numeric conversion intentional.

### 5) Verify the changed behavior

Use the local-kernel path above for executable claims. Include nominal, empty or boundary, malformed or failure-path, and determinism-sensitive cases when they bear on the contract. Compare before/after behavior for refactors. Inspect actual output rather than treating a successful process launch as proof.

### 6) Audit and harden

Check evaluation order, pattern specificity, rewrite termination, scope and state leakage, precision and numeric stability, memoization lifetime, context pollution, performance hotspots, and hidden notebook state. Report remaining limits at the surface where they occur.

## Debugging and failure recovery

Inspect in this order unless the observed symptom selects a narrower path:

1. evaluation semantics and `Hold*` attributes;
2. pattern specificity and condition evaluation;
3. rewrite termination and expression growth;
4. scope, context, stale definitions, notebook order, and memoization;
5. numeric domains, precision tracking, assumptions, and branch behavior;
6. runtime and environment state when the preceding code path is sound.

When a local-kernel probe fails:

- distinguish a Wolfram code error from a missing, unactivated, incompatible, or blocked runtime;
- preserve the exact message and exit result needed to diagnose the next step;
- for a timeout or hang, narrow the probe or start a fresh kernel and change the state or workload before retrying;
- after a possible side effect, inspect the post-state before any retry that could duplicate it;
- if the front end is the missing surface, do not convert that limitation into a kernel correctness claim.

## Refactor and audit output

For existing code, use:

1. findings with severity, evidence, and impact;
2. an ordered repair plan with risk and preservation boundaries;
3. the smallest authorized implementation sequence;
4. verification results, including the execution surface and any blocked claims.

For an audit-only request, stop after findings and bounded recommendations. Do not turn a static check, a structural pass, or an independent-language calculation into proof of runtime correctness.

## Package and paclet progression

When a prototype needs to become reusable, start with standalone functions, then move to a package with usage messages, contexts, options, structured failures, and `VerificationTest` coverage. Add examples and migration notes before optionally packaging as a paclet. Test each transition on the local kernel and keep installation effects explicit.

Before installing or updating a paclet, check `PacletFind`, `PacletFindRemote`, `PacletSites`, required Wolfram version, license, publisher/source, and whether the dependency is intrinsic to the task. Prefer already bundled Wolfram paclets over new user-level installs. If a paclet is only useful for an optional workflow, document the conditional use and do not install it unless the task activates that workflow.

## Output expectations

Unless the user requests otherwise, provide a concise rationale, complete WL code, tests or executable examples, and known limitations. State whether each conclusion came from local-kernel execution, static inspection, front-end observation, or an independent cross-check. Include the actual local runtime result when it supports a claim and identify any unavailable environment or unresolved state.

## Reference files

Load only the reference needed for the active path:

- [local-kernel execution](references/wl-local-kernel.md) for runtime discovery, local invocation, effects, failure handling, and notebook/front-end boundaries;
- [local methods and paclets](references/wl-local-methods-and-paclets.md) for high-leverage local Wolfram uses, AgentTools/LLMFunctions, paclet discovery, and optional installation policy;
- [build and refactor](references/wl-build-and-refactor.md) for implementation and package architecture;
- [debugging and audit](references/wl-debugging-and-audit.md) for diagnostic checklists and failure modes;
- [performance and testing](references/wl-performance-and-testing.md) for optimization and validation strategy.
