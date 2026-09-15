# Example Prompts

Use these prompts to exercise all major skill pathways.

## Build mode
- Build a Wolfram Language package that computes graph centrality metrics with `OptionsPattern[]`, input validation, and `VerificationTest` coverage.
- Create a symbolic simplifier pipeline with three explicit stages and bounded rewriting.

## Refactor mode
- Refactor this notebook cell dump into a package with public/private contexts and usage messages.
- Split this 300-line function into normalized input parsing, transformation, and rendering components.

## Audit mode
- Audit this WL code for evaluation-order bugs and UpValue collisions; provide a severity-ranked findings table.
- Review this mixed symbolic/numeric workflow for precision leaks and exact arithmetic blowups.

## Debug mode
- My `//.` rewrite never terminates for nested expressions. Diagnose and fix with a bounded approach.
- Why does this rule work in one notebook but fail in a fresh kernel? Provide a state-isolation diagnosis.

## Local-kernel execution
- Run these package tests through a fresh local kernel and report the actual `TestReport` result, messages, and exit status.
- The local `wolframscript` runtime is missing or unactivated. Continue the static audit, identify the blocked execution claims, and do not substitute another language as proof of Wolfram behavior.
- Review this notebook's extracted code locally, but identify which claims still require a Wolfram front end.
- Use local Wolfram to check whether this symbolic identity is true under these assumptions, then look for a counterexample if it is not.
- Compare this Python numerical result against a high-precision Wolfram computation and tell me which claim the comparison does and does not establish.
- Check whether `Wolfram/AgentTools` is available locally and whether installing the WolframLanguage MCP server into Codex would mutate project or global config.
- Find whether a Wolfram paclet exists for quantile regression, inspect its version/license/native-code boundary, and do not install it unless this task actually needs it.
- Use `FindInstance` or `Resolve` locally to test this proposed Lean theorem, then hand me only the exact assumptions, normalized statement, and any witnesses or unresolved translation choices.
- Explore whether this algebraic identity has an equational proof in Wolfram, but do not call it a Lean proof unless Lean accepts the translated theorem.
