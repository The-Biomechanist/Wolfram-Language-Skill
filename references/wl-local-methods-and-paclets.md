# WL Local Methods and Paclets Reference

Use this reference when a task can benefit from local Wolfram computation beyond a simple package test, or when a paclet may expand the local kernel surface.

## Local-method selection

Reach for the local kernel when it can change the result through one of these surfaces:

- **Symbolic truth tests:** use `FullSimplify`, `Assuming`, `FunctionExpand`, `PowerExpand` only with explicit domain control, `Reduce`, `Resolve`, recurrence functions, exact linear algebra, and exact polynomial/rational manipulation.
- **Counterexample and feasibility search:** use `FindInstance`, `SatisfiabilityInstances`, `NMinimize`/`NMaximize`, `FindRoot`, interval or high-precision probes, random seeds, and residual checks. A numerical failure is evidence to investigate, not proof of impossibility.
- **Proof-adjacent work:** use `Resolve`, `Reduce`, `FindInstance`, `SatisfiabilityInstances`, and `FindEquationalProof` to test formal claims, search for counterexamples, eliminate quantifiers over supported domains, explore equational derivations, and generate exact examples for proof scripts. Treat the result as CAS evidence unless the accepted proof authority is Wolfram's own proof object and that object has been inspected.
- **Scientific modeling:** use `NDSolve`, `DSolve`, `ParametricNDSolve`, units with `Quantity`, uncertainty/error propagation, signal/image processing, graphs, geometry, and built-in curated data when the dataset source and date are acceptable for the task.
- **Cross-runtime arbitration:** compare against Python, Julia, Sage, R, SMT solvers, or Lean-generated examples only for the shared mathematical property. Do not transfer a Wolfram pass into a claim about another runtime's parser, types, floating-point behavior, package API, or side effects.
- **Artifact generation:** use `Export` for plots, tables, notebooks, images, WXF, CSV, JSON, or Markdown only when an output artifact is requested or needed by a downstream consumer. Keep generated files inside the project or a disposable task directory unless the user authorizes a wider destination.

For expensive or nonterminating candidates, begin with the smallest input that distinguishes paths, run with a timeout, then scale only if the small case supports it. Preserve assumptions, precision, seed, input size, and any messages that affect interpretation.

## Formal and theorem-proving support

Use Wolfram to support formal work when it can reduce uncertainty before a proof checker sees the statement:

- normalize expressions with `FullForm`, `Together`, `Factor`, `FunctionExpand`, `LogicalExpand`, and domain-scoped `FullSimplify`;
- turn candidate theorems into explicit quantified formulas with `ForAll`, `Exists`, `Element`, `Implies`, and `Equivalent`;
- use `Resolve[expr, dom]` or `Reduce[expr, vars, dom]` for quantifier elimination and domain reductions over supported domains;
- use `FindInstance[Not[claim], vars, dom]` to search for counterexamples to universal claims, keeping the negated claim in a solver-supported form when possible;
- use `SatisfiabilityInstances` or `SatisfiableQ` for Boolean and finite encodings;
- use `FindEquationalProof` for equational theorem exploration when the task is in an axiomatic/equational fragment;
- generate small exact examples, boundary cases, and failed cases to seed Lean, SMT, or property-test work.

Keep proof status explicit:

- A Wolfram simplification to `True`, absence of a found counterexample, or successful numerical search is not by itself a formal proof for Lean, Coq, Isabelle, SMT-LIB, or another target system.
- A counterexample from Wolfram can refute the proposed theorem if the expression, assumptions, and domain match the target statement; verify that translation before using it as a refutation.
- No witness from `FindInstance` is inconclusive when the formula includes arbitrary predicates, procedural tests, external functions, or unsupported domains. Reformulate into algebraic/logical constraints or use a finite exhaustive check before treating the result as meaningful.
- A `ProofObject` or `FindEquationalProof` result can be proof evidence only for the theorem class and axiom basis it actually covers. Inspect the theorem, axioms, proof dataset/graph, and validation function before claiming a Wolfram proof.
- When handing work to Lean or SMT, pass the exact domains, assumptions, normalized form, witness assignments, generated cases, and translation uncertainties. The downstream checker owns final acceptance.

## Local agent and MCP surfaces

This environment's Wolfram Engine 15 installation included `Wolfram/AgentTools` when checked on 2026-09-15, and the Wolfram paclet server offered newer AgentTools releases at that time. Use this branch only when a task asks to connect Wolfram to an MCP-capable agent or to inspect/create Wolfram agent tools.

High-value AgentTools uses:

- install a Wolfram MCP server into a supported local client when the user asks for direct tool integration;
- use the WolframLanguage-oriented server for code evaluation, symbol definitions, code inspection, notebooks, and `TestReport`;
- use the WolframPacletDevelopment server when the task is paclet packaging, documentation pages, `CheckPaclet`, `BuildPaclet`, or submission preparation;
- create a custom MCP server with a small `LLMTool` wrapper around a local pure Wolfram function when repeated agent access is more useful than one-off `wolframscript` calls.

AgentTools installation mutates client configuration. Before running `InstallMCPServer`, identify the client, server name, project scope if supported, expected config path, and rollback command. Prefer local `wolframscript` probes for ordinary one-off checks.

## LLMFunctions boundary

This environment's Wolfram Engine 15 installation included `Wolfram/LLMFunctions` when checked on 2026-09-15. Treat it as optional. Use it only when the task explicitly needs Wolfram-side LLM functions, prompt/tool construction, or LLM graph behavior. Do not use it to replace the agent's own reasoning, and do not send private project content to an external LLM service unless the task authorizes that egress and the service credentials/configuration are established.

Useful local checks:

```wl
PacletFind["Wolfram/LLMFunctions"]
Names["System`LLM*"]
```

Provider configuration, API keys, account state, and network egress are separate authority boundaries.

## Paclet discovery and installation policy

Paclet discovery is read-only; installation is mutation.

Use these read-only checks before recommending or installing a paclet:

```wl
PacletSites[]
PacletFind["Publisher/Name"]
PacletFindRemote["Publisher/Name"]
```

For a candidate paclet, inspect:

- name, version, location, publisher, license, required Wolfram version, contexts, symbols, documentation link, and whether it ships LibraryLink or other native binaries;
- whether the paclet provides a capability not already covered by built-in Wolfram Language or another registered local tool;
- whether the task needs the paclet persistently or only for a disposable experiment.

Run `PacletInstall[...]` only when the user has authorized installation or the active task plainly requires that specific dependency. After installation, run `PacletFind[...]`, `Needs[...]`, and one minimal function-level smoke test. If a paclet contains LibraryLink/native code, verify it on the active OS and architecture before relying on it.

## Currently useful paclet candidates

- `Wolfram/AgentTools`: high leverage for local MCP integration, documentation search, code evaluation, symbol definitions, `CodeInspector`, notebooks, `TestReport`, and paclet-development tooling. On 2026-09-15 it was installed in this Wolfram Engine 15 environment as version `2.1.17`; remote query showed `2.2.0` available. Update only when a task needs the newer functions or bug fixes.
- `Wolfram/LLMFunctions`: useful for Wolfram-side LLM tool/prompt experiments and `LLMTool`/`LLMFunction` workflows. On 2026-09-15 it was installed here as version `2.3.1`; remote query showed `2.3.2` available. Use only with explicit egress/credentials handling.
- `AntonAntonov/QuantileRegression`: a plausible optional statistics paclet when a task specifically needs quantile regression/envelopes inside Wolfram Language. On 2026-09-15 it was not installed here; remote query showed version `1.0.7`. Prefer built-in statistics first unless this exact method is needed.
- `KirillBelov/CSockets`: a plausible optional systems/networking paclet for socket-heavy Wolfram services or custom local integrations. On 2026-09-15 it was not installed here; remote query showed version `1.0.26` and LibraryLink/native content, so verify Windows compatibility before relying on it.

Do not maintain a long evergreen paclet list in this skill. Re-query `PacletFindRemote` when the task needs a candidate; paclet availability and versions are live ecosystem state.

## Source references

- Wolfram Language `PacletFindRemote`: https://reference.wolfram.com/language/ref/PacletFindRemote.html
- Wolfram Language `PacletInstall`: https://reference.wolfram.com/language/ref/PacletInstall.html
- Wolfram Language `ExternalEvaluate`: https://reference.wolfram.com/language/ref/ExternalEvaluate.html
- Wolfram Language `FunctionCompile`: https://reference.wolfram.com/language/ref/FunctionCompile.html
- Wolfram Language `Resolve`: https://reference.wolfram.com/language/ref/Resolve.html
- Wolfram Language `Reduce`: https://reference.wolfram.com/language/ref/Reduce.html
- Wolfram Language `FindInstance`: https://reference.wolfram.com/language/ref/FindInstance.html
- Wolfram Language `SatisfiabilityInstances`: https://reference.wolfram.com/language/ref/SatisfiabilityInstances.html
- Wolfram Language `FindEquationalProof`: https://reference.wolfram.com/language/ref/FindEquationalProof.html
- Wolfram Language `ProofObject`: https://reference.wolfram.com/language/ref/ProofObject.html
- WolframResearch AgentTools: https://github.com/WolframResearch/AgentTools
