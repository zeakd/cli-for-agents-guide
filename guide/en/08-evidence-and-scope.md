# 8. Evidence and scope

## Distinguish requirements, methods, and cases

“Input documentation matches validation” is a caller-facing contract. “Generate both from one declaration” is an implementation method. “Use a temporary home” is a testing pattern for tools that own persistent state.

Treating all three as universal requirements burdens tools that do not need the machinery. Keep each recommendation's applicability visible, and explain the condition under which another choice is appropriate.

When adding a field, command, or rule, ask:

- Does it help the caller interpret the result or choose and construct an action?
- Does it make common correct use simpler?
- Does it reduce omissions or contradictions during maintenance?
- Is that benefit worth the output volume and implementation cost?

## State the assumptions honestly

Design for a caller that may be unfamiliar with the tool or missing earlier context. This does not require claiming that every AI invocation is a blank slate or that humans have unlimited context.

Input validation, observable state, and testable dependencies were useful before AI. Their usefulness to agents does not establish historical novelty. Claims about performance or correctness need evidence appropriate to the claim; a generated schema alone is not proof that every handler follows it.

Choosing between CLI, MCP, or other interfaces depends on the environment. This guide describes the surface to provide when choosing a CLI. It does not prescribe an agent harness, compatibility policy, language, or package manager.

## Reading the examples

Command, response, and layout examples make design choices concrete. Their command names, field names, and assembly APIs are not a universal specification or a statement of current TypeScript package support. Shell examples illustrate composition; [chapter 4](04-results-and-presentation.md#output-for-composition) explains why a pipe alone does not guarantee complete input or successful execution.

## Reference implementation coverage

The [TypeScript implementation](https://github.com/zeakd/cli-for-agents-ts) is a concrete example, not a claim that all recommendations here are already implemented. The following snapshot is pinned to [revision 701eb6450a181640e63a47a281a4752cb6644e0c](https://github.com/zeakd/cli-for-agents-ts/tree/701eb6450a181640e63a47a281a4752cb6644e0c). Later revisions may differ.

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

Use [the pinned sources](https://github.com/zeakd/cli-for-agents-ts/tree/701eb6450a181640e63a47a281a4752cb6644e0c/src) to assess that implementation. Keep coverage claims versioned as code changes. Verify both generated facts and authored examples before describing a behavior as supported.

---

[Contents](index.md) · [Previous](07-conditional-patterns.md)
