# Changelog

## 0.4.1
- Added explicit formal/proof-adjacent guidance for theorem exploration, counterexample search, quantifier elimination, Boolean/finite encodings, equational proofs, and Lean/SMT handoff boundaries.
- Clarified that Wolfram CAS evidence does not become a formal proof for another proof checker unless the target proof authority accepts it.
- Added proof-adjacent routing to the skill description and dated the local paclet observations.
- Added a counterexample-search caution for arbitrary predicates or unsupported solver forms.

## 0.4.0
- Added high-leverage local Wolfram usage guidance for symbolic checks, counterexample search, solver triage, graph/model probes, high-precision arbitration, and artifact generation.
- Added `references/wl-local-methods-and-paclets.md` with local method routing, AgentTools and LLMFunctions boundaries, paclet discovery/install policy, and current candidate paclets.
- Documented observed Wolfram Engine 15 local state: `Wolfram/AgentTools` and `Wolfram/LLMFunctions` are installed locally, with newer remote versions visible through `PacletFindRemote`.
- Strengthened PowerShell `-file` guidance for quote-heavy Wolfram code and added `FunctionCompile` benchmark/parity cautions.

## 0.3.0
- Made local Wolfram-kernel evaluation the primary execution path.
- Added toolchain-registry discovery, runtime smoke validation, fresh-kernel guidance, and explicit blocked/unavailable continuations.
- Added effect, credential, cloud, paclet-installation, and notebook/front-end boundaries.
- Added the local-kernel reference and aligned README/examples with the repaired contract.

## 0.2.0
- Rebuilt `SKILL.md` with explicit Build/Refactor/Audit workflows.
- Added WL-specific debugging stance and robustness patterns.
- Added package/paclet progression and response-mode guidance.
- Added reference docs for architecture, auditing, performance, and testing.
- Added example prompts and a compact companion prompt.

## 0.1.0
- Initial minimal repository scaffold.
