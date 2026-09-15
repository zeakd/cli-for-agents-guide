# 8. Evidence and scope

Read the guide's requirements separately from its implementation methods.

Agreement between input descriptions and validation is a contract callers rely on. Generating both from shared declarations is a way to maintain it. A temporary home is a testing pattern for tools that own state. Treating these as equally mandatory features adds machinery where it is not needed. State when a recommendation applies and when another choice is appropriate.

Input validation, observable state, and testable dependencies were useful before AI. Their usefulness to agents does not establish historical novelty or performance improvements. Performance and correctness claims should include the conditions measured and the supporting evidence. A generated schema alone does not prove that every handler follows it.

This guide addresses design when choosing a CLI. The choice between CLI, MCP, and other interfaces depends on the environment; no particular harness, language, or package manager is assumed.

## Examples and implementation coverage

Commands, responses, and file layouts illustrate design choices. Exact command names, field names, and knowledge assembly APIs are not a universal specification.

Consult the TypeScript implementation’s current documentation alongside its code. Distinguish features the guide recommends from features the package currently provides, and update coverage as code changes.

## Reference implementation coverage

The [TypeScript implementation](https://github.com/zeakd/cli-for-ai-ts) is one
concrete example. Its [implementation scope](https://github.com/zeakd/cli-for-ai-ts/blob/main/docs/scope.md)
records supported behavior and limits alongside the code on `main`. Keep detailed API coverage there
rather than maintaining a second feature table in the guide.

Use the scope document, source declarations and executable examples together when
assessing support. A guide recommendation does not establish that the reference
package implements it. Commands named `tool` here illustrate designs rather than
executable package APIs. Field names and exact skill command spelling remain
implementation choices.

---

[Contents](index.md) · [Previous](07-conditional-patterns.md)
