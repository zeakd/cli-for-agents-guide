# 1. Design goals

A CLI has two kinds of AI users: the agent calling its commands and the agent maintaining its code. The caller needs to discover capabilities, construct valid input, interpret results, and choose its next action. The maintainer needs to change behavior without leaving its description, validation, or tests behind.

Both benefit when a task requires less unrelated context. A caller should not have to read every command to use one feature. A maintainer should not have to update the same fact in several files. Neither should have to infer facts the tool can report directly.

## Make ordinary use easy

The most common correct use should be a short, natural invocation. A tool name opens discovery. Ordinary results are structured by default. Invalid input fails clearly. Reading output for a human is an explicit choice.

When usage instructions repeatedly say “always add this flag” or “remember to check this,” ask whether a default or a validation rule can carry that responsibility. Keep instructions for judgments the tool cannot make mechanically.

The aim is not to print everything the tool knows. Give callers enough information for the next decision, with a way to discover and request more. Excess output can obscure the facts that matter.

## Give the caller evidence

The CLI exposes declared capabilities, authored usage knowledge, and observed execution facts. The agent chooses actions using that information and the user's request. The CLI need not infer intent or manage an arbitrary plan.

For example, a tool can return a created object's identifier and the command to inspect it. It can do so because the author defined that relationship, without reasoning about the user's wider goal.

## Separate the contract from its implementation

This guide distinguishes:

- **Common contracts:** behavior callers can rely on, such as explicit input errors and discoverable output formats.
- **Authoring structure:** ways to keep those contracts consistent, such as generating help and validation from one declaration.
- **Conditional patterns:** facilities needed only by some tools, such as a replaceable home for persistent state or cancellation for a long-running job.

A stateless filter should not need a home directory or a job database. A tool with those needs should be able to add them without losing the common contracts. The principles are independent of language and framework.

---

[Contents](index.md) · [Next](02-discovery-and-skills.md)
