# 6. Authoring and testing

## Keep a feature's definition together

Adding a command should not require independently editing parsing, routing, help, schema, and error text for the same fact. Collect its description, inputs, execution, and output declaration, and generate mechanical surfaces from those definitions.

A registry is one useful way to connect these declarations. A feature folder is one useful way to keep related code nearby. Neither requires every feature to fit one file or every implementation to share a directory layout. The goal is to understand and change a feature without reading unrelated code or maintaining duplicate facts.

## Separate decisions from effects

Distinguish logic that computes a decision from code that reads files, accesses a network, obtains time, or writes output. Supply external dependencies through explicit boundaries such as a context object or function arguments.

A pure function can be checked using only its inputs and return value. Do not pass a context into code that needs no environment. Effectful handlers can receive a narrow context with the capabilities they need; frameworks should make those dependencies replaceable without global patching.

Expected failures should be easy to enumerate and handle. Result values, discriminated unions, and typed exceptions are possible approaches. The requirement is complete, recognizable failure handling; a TypeScript convention is not a rule for every language.

## Test at the right boundary

```text
Pure function       -> inputs and return values
Command or service  -> injected fake dependencies
Actual CLI          -> isolated environment, real invocation and output
```

Function tests can use an in-memory store, a fake HTTP response, or a fixed clock. They can exercise precise conditions without setting up the real world.

CLI tests cover boundaries those tests cannot establish: argument forwarding, configuration loading, stdout and stderr, exit codes, and actual persistence. A stateful tool can use a temporary home. Other tools can use temporary inputs or a test endpoint. [Chapter 7](07-conditional-patterns.md) explains these choices.

Use verification appropriate to the change. Do not require all tests to run in one process or duplicate every case at every level. The maintaining agent should have a documented, repeatable way to run the relevant checks without relying on undocumented manual preparation.

## Generate facts; verify authored knowledge

Require command descriptions and input contracts. Provide a default output contract and require declarations for alternatives. Provide tool-level usage knowledge without requiring a separate skill for every command.

A framework can detect missing declarations and compare generated surfaces. It cannot establish that prose is useful merely because a skill file exists or contains a command name. Review authored guidance and verify its behavioral examples. Executable documentation examples and output fixtures can help keep promises aligned with behavior.

---

[Contents](index.md) · [Previous](05-state-and-actions.md) · [Next](07-conditional-patterns.md)
