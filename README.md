# CLI for agents

**What should a CLI designed for AI agents look like?**

A CLI is a familiar way for agents to run tools. They can explore commands, supply input, and connect output to other programs.

Being able to run a command, however, does not mean all the information needed to use it is available. Help may describe syntax without explaining a workflow, and a result may leave completion unclear. An agent may also invoke commands separately when the work could be handled by connecting them.

This guide covers **CLI design from discovery and execution to result interpretation and follow-up actions**. It explains how a CLI can provide information that agents otherwise have to search for or infer.

**[Read the guide →](guide/en/index.md)** · [Korean](README.ko.md)

### Discovery and usage knowledge

Start with the tool name and find the commands and guidance needed for the task. Help explains syntax; skills explain workflows. Organize growing command sets so callers can read the relevant parts first.

### Input and execution

Explore defaults that simplify common work, input errors that provide evidence for correction, and complex input through files or stdin. Steps requiring no new decision can run together through pipes or command composition.

### Results and next actions

Cover JSON and human presentation, output volume and pagination, partial failure, and asynchronous state. Provide relevant follow-up commands while keeping retrieved external content separate from guidance authored by the tool.

### Authoring and testing

Generate help and validation from command declarations, and assemble knowledge kept close to features into task guides. Separate decisions from side effects and verify behavior from functions through actual CLI invocations.

## Scope

The guide is independent of language and framework. No package is required to apply its principles.

Home directories, networking, authentication, and long-running work are patterns for tools that need them. Commands and responses illustrate designs; exact command names and field names are not a universal specification.

English is canonical; the Korean edition covers the same material.

## Reference implementations

| Repository | Role |
| --- | --- |
| [cli-for-agents-ts](https://github.com/zeakd/cli-for-agents-ts) | TypeScript package `cli-for-agents`, with runnable examples |
| `cli-for-agents-go` — planned | Go implementation |

The TypeScript implementation illustrates some principles in code. Its current behavior differs from the guide's recommendations in areas such as JSON defaults and error channels, and some contracts remain unimplemented. See [versioned coverage](guide/en/08-evidence-and-scope.md#reference-implementation-coverage) for the differences.

License: [MIT](LICENSE)
