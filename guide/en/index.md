# Designing CLIs for AI agents

This guide proposes common CLI contracts for AI callers and structures that help AI maintainers implement them consistently. It is language- and framework-independent.

Start with useful defaults: discovery from the bare tool name, explicit input errors, ordinary JSON results, and usage knowledge available from the CLI. Add stateful or asynchronous facilities only where needed.

The chapters describe recommended contracts, not the current API of a particular package. All `tool` invocations are illustrative. See [reference implementation coverage](08-evidence-and-scope.md#reference-implementation-coverage) before using the TypeScript package as an example.

## Chapters

1. [Design goals](01-design-goals.md)
2. [Discovery and usage knowledge](02-discovery-and-skills.md)
3. [Input and execution](03-input-and-execution.md)
4. [Results and presentation](04-results-and-presentation.md)
5. [State and follow-up actions](05-state-and-actions.md)
6. [Authoring and testing](06-authoring-and-testing.md)
7. [Conditional patterns](07-conditional-patterns.md)
8. [Evidence and scope](08-evidence-and-scope.md)

## Reading paths

For the caller-facing surface, read chapters 2–5. For authoring and verification, read chapter 6, then the relevant cases in chapter 7. Chapter 8 separates evidence, implementation choices, and coverage limits.

[Korean](../ko/index.md)
