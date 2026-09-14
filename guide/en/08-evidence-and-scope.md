# 8. Evidence and scope

Read the guide's requirements separately from its implementation methods.

Agreement between input descriptions and validation is a contract callers rely on. Generating both from shared declarations is a way to maintain it. A temporary home is a testing pattern for tools that own state. Treating these as equally mandatory features adds machinery where it is not needed. State when a recommendation applies and when another choice is appropriate.

Input validation, observable state, and testable dependencies were useful before AI. Their usefulness to agents does not establish historical novelty or performance improvements. Performance and correctness claims should include the conditions measured and the supporting evidence. A generated schema alone does not prove that every handler follows it.

This guide addresses design when choosing a CLI. The choice between CLI, MCP, and other interfaces depends on the environment; no particular harness, language, or package manager is assumed.

## Examples and implementation coverage

Commands, responses, and file layouts illustrate design choices. Exact command names, field names, and knowledge assembly APIs are not a universal specification.

Compare reference TypeScript behavior against a specific revision. Distinguish features the guide recommends from features the package currently provides, and update coverage as code changes.

## Reference implementation coverage

The [TypeScript implementation](https://github.com/zeakd/cli-for-ai-ts) is a concrete example, not a claim that all recommendations here are already implemented. The following snapshot is pinned to [revision 7600bc09c62fed10942bb5f81de69536ef90c145](https://github.com/zeakd/cli-for-ai-ts/tree/7600bc09c62fed10942bb5f81de69536ef90c145). Later revisions may differ.

| Area | Behavior in that revision | Contract described in this guide |
| --- | --- | --- |
| Discovery | Root and group bare calls match help; no built-in global schema command; a programmatic schema API remains | Help provides essential invocation and result information; structured export is optional |
| Output | Ordinary JSON by default; explicit `--human`, with readable JSON fallback | Same execution regardless of presentation |
| Error channel | Ordinary success and failure go to stdout; diagnostics go to stderr | Payload output needs a separate failure-channel contract when supported |
| Output declaration | Optional parser validates successful data; separately supplied output schema is descriptive; help has output summaries | Provide the result structure needed for interpretation and composition; full output structure is not generated from parsers here |
| Input | Unknown, missing and excess inputs fail; declared value and cross-input constraints apply; stdin shape checks precede context creation | Reject input outside the declared contract; help validates supplied inputs without requiring omitted execution inputs |
| Group routing | Groups show help; unknown commands fail | Groups have no implicit execution |
| Follow-up actions | Structured result actions and hints are not implemented | Relevant actions and hints can accompany success or failure |
| Usage guidance | Command help and validated examples; no authored skill surface | Related workflows are reachable from help when needed |
| Execution state | Completed, accepted and failed results; reporting failures preserve known returned state; cancellation does not promise rollback | Also distinguish partial and unknown outcomes where operations require them |
| Authoring and testing | Shared declarations, injected contexts, context disposal and real-process tests | Useful implementation methods, including checks at the CLI boundary |

The broader action, partial-result, and skill contracts are not implemented in this revision and must not be assumed from these chapters. The `tool` commands in this guide are illustrative designs, not executable examples of that package. Field names and exact skill command spelling are left to the implementation; the behavioral distinctions are the guide's recommendations.

Use [the pinned sources](https://github.com/zeakd/cli-for-ai-ts/tree/7600bc09c62fed10942bb5f81de69536ef90c145/src) to assess that implementation. Keep coverage claims versioned as code changes. Verify both generated facts and authored examples before describing a behavior as supported.

---

[Contents](index.md) · [Previous](07-conditional-patterns.md)
