# Wolfram Language Skill

A production-oriented agent skill for building, debugging, refactoring, optimizing, documenting, and auditing Wolfram Language (Mathematica) code, notebooks, packages, and paclet-ready components, with local Wolfram-kernel evaluation as the primary executable path.

The skill focuses on the parts of Wolfram Language that generic coding guidance often handles poorly: evaluation semantics, pattern specificity, rewrite termination, symbolic/numeric boundaries, precision behavior, scoping, memoization, hidden notebook state, and package API design.

## Core modes

| Mode | Use it for |
| --- | --- |
| **Build** | New functions, packages, notebooks, or paclet-ready components with explicit APIs, validation, tests, and failure behavior. |
| **Refactor** | Behavior-preserving improvements to structure, modularity, public/private boundaries, options, state handling, and maintainability. |
| **Audit** | Evidence-backed review of correctness, robustness, performance, evaluation behavior, precision, and developer ergonomics. |

For response depth, the skill also distinguishes **Fast fix**, **Production**, and **Audit-first** workflows instead of treating every request as the same kind of intervention.

## Execution default

Executable Wolfram Language work uses a locally discovered `wolframscript`/Wolfram Engine kernel first. The skill consults the active environment's toolchain registry, validates the selected runtime with a version check and a bounded `1+1` smoke test, then uses `-local -code` for focused expressions or `-local -file` for source and test files. Missing or unactivated runtimes block only execution-dependent claims.

Cloud evaluation, cloud authentication, and front-end-only notebook behavior are separate conditional branches. The skill does not use them implicitly or treat kernel output as evidence for notebook layout, dynamic interaction, initialization-cell behavior, or other front-end state.

## Local leverage

The skill now explicitly routes high-value local Wolfram use: exact symbolic checks, counterexample search, solver triage, graph/combinatoric/model probes, high-precision numerical arbitration, proof-adjacent theorem exploration, units and curated scientific data checks, and artifact generation when a downstream consumer needs plots/tables/exports. It also documents optional local agent surfaces through `Wolfram/AgentTools` and `Wolfram/LLMFunctions`.

Paclets are handled as live ecosystem dependencies: discover with `PacletFind`/`PacletFindRemote`, inspect source/version/license/native-code boundaries, and install only when the task activates that dependency. This environment's Wolfram Engine 15 installation included `Wolfram/AgentTools` and `Wolfram/LLMFunctions` when checked on 2026-09-15; the skill records their use without making every Wolfram task depend on them.

## What it is designed to handle

- symbolic transformations, replacement rules, conditions, and pattern matching;
- proof-adjacent work with `Resolve`, `Reduce`, `FindInstance`, `SatisfiabilityInstances`, and `FindEquationalProof`, while keeping CAS evidence distinct from formal proof-checker acceptance;
- delayed versus immediate evaluation and `Hold*` behavior;
- nonterminating or oscillating rewrite systems;
- `Module`, `Block`, `With`, contexts, DownValues, UpValues, and accidental symbol leakage;
- mixed exact, machine-precision, and arbitrary-precision numerical workflows;
- `NDSolve`, optimization, linear algebra, fitting, and other symbolic/numeric boundaries;
- memoization lifetime and growth;
- notebook execution-order and hidden-state problems;
- package and paclet progression, public APIs, options, structured failures, tests, and documentation.

Useful diagnostic probes include `Trace`, `TraceScan`, `FullForm`, `Definition`, `DownValues`, and `UpValues`, combined with targeted timing and memory checks where performance is actually in question.

## Operating discipline

The skill separates representation stages—normalization, symbolic transformation, numeric realization, and formatting/export—so evaluation leaks and rewrite blowups are easier to reason about. It also keeps pure kernel probes separate from authorized file, network, process, package-installation, and other system effects.

For existing code, the default audit/refactor flow is:

1. identify findings with severity, evidence, and impact;
2. propose an ordered repair plan;
3. make the smallest safe implementation changes when implementation is requested;
4. verify behavior with nominal, edge, failure-path, and determinism-sensitive tests.

Package-oriented work favors clear contexts, intentional scoping, `OptionsPattern[]`, structured `Failure[...]` contracts, bounded transformations, and `VerificationTest` / `TestReport` for reusable code.

## Repository layout

- [`SKILL.md`](./SKILL.md) — trigger conditions, operating modes, Wolfram-specific workflow, and output expectations.
- [`references/wl-local-kernel.md`](./references/wl-local-kernel.md) — runtime discovery, local invocation, effect boundaries, failure handling, and notebook/front-end limits.
- [`references/wl-local-methods-and-paclets.md`](./references/wl-local-methods-and-paclets.md) — high-leverage local computation patterns, AgentTools/LLMFunctions, paclet discovery, and optional-install policy.
- [`references/wl-build-and-refactor.md`](./references/wl-build-and-refactor.md) — implementation, package architecture, and refactor patterns.
- [`references/wl-debugging-and-audit.md`](./references/wl-debugging-and-audit.md) — debugging protocol, failure modes, and audit guidance.
- [`references/wl-performance-and-testing.md`](./references/wl-performance-and-testing.md) — performance, numerical discipline, and validation strategy.
- [`examples/example-prompts.md`](./examples/example-prompts.md) — realistic trigger and usage examples.
- [`examples/compact-prompt.md`](./examples/compact-prompt.md) — compact companion prompt for constrained contexts.
- [`CHANGELOG.md`](./CHANGELOG.md) — release history.

## Example requests

- “Refactor this notebook code into a package with options and tests.”
- “Why does this replacement rule loop forever?”
- “Audit this Wolfram Language function for precision, memoization, and failure handling.”
- “Turn this prototype into a paclet-ready API without changing its public behavior.”
- “Run this package's VerificationTest suite in a fresh local kernel and report the actual messages and failures.”
- “This Wolfram script times out locally; narrow the probe and diagnose whether the issue is evaluation, rewriting, or runtime state.”

## Status

Current documented version: **0.4.1**.
