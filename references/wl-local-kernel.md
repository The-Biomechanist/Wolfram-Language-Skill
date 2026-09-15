# Local Wolfram Kernel Reference

Use this reference when an executable claim about Wolfram Language depends on the local Wolfram Engine kernel.

## Runtime discovery and smoke check

1. Read the active environment's toolchain registry and select its verified `wolframscript` entry. Prefer the registered command or stable wrapper over a guessed installation path.
2. If no verified entry exists, use native command discovery and inspect the resolved executable. Keep the runtime fact local to the current task; do not bake a host path into a portable skill.
3. Validate the selected runtime before depending on it:

   ```text
   <WOLFRAMSCRIPT> -version
   <WOLFRAMSCRIPT> -local -code '1+1'
   ```

The version output proves which CLI was selected. The second command is a bounded no-side-effect kernel smoke check. A nonzero exit, licensing/activation message, or missing executable blocks only claims that require execution.

## Invocation patterns

```text
<WOLFRAMSCRIPT> -local -code '<expression>'
<WOLFRAMSCRIPT> -local -file '<path-to-wl-or-wls-file>'
<WOLFRAMSCRIPT> -local -timeout <seconds> -code '<bounded probe>'
```

Use `-file` for source and test artifacts rather than embedding a large program in a shell command. In PowerShell, switch to `-file` once the expression contains quotes, associations, strings, package loading, or multiline definitions; shell quoting can silently change the Wolfram input. Use the runtime's output/format options only when the consumer needs them, and preserve messages and exit status when they affect diagnosis or verification.

Prefer separate invocations for independent probes. A reused kernel can carry definitions, loaded packages, options, random seeds, `$Path`, or working-directory state; if reuse is necessary, make those dependencies explicit and verify them.

If concurrent local-kernel launches produce activation or license-lock errors while a serial invocation succeeds, treat that as runtime/license contention and serialize the probes. Do not classify the Wolfram code or installation as broken from the concurrent observation alone. Use `-file` for multiline or string-heavy code when shell quoting could change the submitted expression.

## Effects and authorization

Kernel evaluation can read or write files, access URLs, launch processes, install packages, and alter persistent configuration. Run pure expressions by default. Before a nontrivial evaluation, identify the effects it could produce and the task-authorized surface.

For an authorized mutation, use a copy or disposable test location when practical, observe the post-state, and reconcile uncertain effects before retrying. Do not pass credentials in command-line arguments or chat. Do not switch to `-cloud` or cloud authentication unless the user explicitly requests that branch.

## Tests and evidence

Run package tests through a fresh local kernel where possible:

```wl
Needs["MUnit`"];
TestReport @ {
  VerificationTest[expression, expected, TestID -> "nominal case"],
  VerificationTest[FailureQ @ expression, True, TestID -> "failure case"]
}
```

The code block is a pattern, not a result. Report the actual `TestReport` outcome, messages, exit status, and relevant environment. A static read of a test file establishes what the test requests, not that it passed. A result from another language can be an independent mathematical check but cannot establish Wolfram evaluation or package semantics.

## Notebook and paclet boundary

The local kernel is the primary surface for `.wl`, `.wls`, and package behavior. A notebook may additionally depend on front-end layout, dynamic interaction, initialization cells, formatting, or hidden session state. Evaluate extracted code locally, but leave front-end-dependent claims unresolved unless that surface was observed.

For paclets, test source and public APIs locally before installation. Treat installation into user or system directories as a separate authorized mutation, preferably using an isolated test location when available. For local-method and paclet-selection details, use [local methods and paclets](wl-local-methods-and-paclets.md).

## Failure handling

- **Missing or unactivated runtime:** preserve the observed error and continue only with static or explicitly independent work.
- **Kernel error:** retain messages and the failing input; diagnose evaluation, pattern, scope, or numeric state before changing code.
- **Timeout or nontermination:** narrow the input or change state before retrying; do not repeat the same unbounded call.
- **Possible side effect:** inspect the filesystem, process, package, or configuration state before retrying.
- **Front-end unavailable:** report the exact front-end claim that remains unverified rather than generalizing from kernel output.
