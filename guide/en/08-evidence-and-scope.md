# 8. Evidence and scope

Read the guide's requirements separately from its implementation methods.

Agreement between input descriptions and validation is a contract callers rely on. Generating both from shared declarations is a way to maintain it. A temporary home is a testing pattern for tools that own state. Treating these as equally mandatory features adds machinery where it is not needed. State when a recommendation applies and when another choice is appropriate.

Input validation, observable state, and testable dependencies were useful before AI. Their usefulness to agents does not establish historical novelty or performance improvements. Performance and correctness claims should include the conditions measured and the supporting evidence. A generated schema alone does not prove that every handler follows it.

This guide addresses design when choosing a CLI. The choice between CLI, MCP, and other interfaces depends on the environment; no particular harness, language, or package manager is assumed.

## Examples and implementation coverage

Commands, responses, and file layouts illustrate design choices. Exact command names, field names, and knowledge assembly APIs are not a universal specification.

Compare reference TypeScript behavior against a specific revision. Distinguish features the guide recommends from features the package currently provides, and update coverage as code changes.

## Reference implementation coverage

The [TypeScript implementation](https://github.com/zeakd/cli-for-ai-ts) is a concrete example, not a claim that all recommendations here are already implemented. The following snapshot is pinned to [revision 701eb6450a181640e63a47a281a4752cb6644e0c](https://github.com/zeakd/cli-for-ai-ts/tree/701eb6450a181640e63a47a281a4752cb6644e0c). Later revisions may differ.

| Area | Behavior in that revision | Contract described in this guide |
| --- | --- | --- |
| Discovery | Top-level and domain help, generated guide, schema | Also supports selective discovery of multiple authored skills when needed |
| Output | Renderers run without `--json`; error presentation can depend on TTY | Ordinary JSON by default; explicit `--human` |
| Error channel | JSON errors go to stderr | Default JSON success and failure go to stdout; payload errors stay on stderr |
| Output declaration | Raw and stream modes exist; schema describes inputs | Alternative output formats are discoverable before invocation |
| Input | Declared handler inputs receive required-input and unknown-option checks; built-in system/help paths can ignore undeclared input, and maximum positional arity is not enforced | Reject input outside the declared contract, including extra positionals |
| Group routing | An optional fallback can execute on an unrecognized verb | Groups have no implicit execution; invalid commands fail |
| Follow-up actions | Error recovery fields exist | Relevant actions and hints can accompany success or failure |
| Usage guidance | Generated guide recommends doctor first and direct execution of recovery commands | Doctor is optional; a suggestion does not grant permission |
| Authoring and testing | Shared declarations and injected contexts | Useful implementation methods, with CLI-boundary tests as well |

The package's `--human` mode and broader action, partial-result, and skill contracts must not be assumed from these chapters. The `tool` commands in this guide are illustrative designs, not executable examples of that package. Field names and exact skill command spelling are left to the implementation; the behavioral distinctions are the guide's recommendations.

Use [the pinned sources](https://github.com/zeakd/cli-for-ai-ts/tree/701eb6450a181640e63a47a281a4752cb6644e0c/src) to assess that implementation. Keep coverage claims versioned as code changes. Verify both generated facts and authored examples before describing a behavior as supported.

---

[Contents](index.md) · [Previous](07-conditional-patterns.md)
