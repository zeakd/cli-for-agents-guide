# CLI for agents

**What should a CLI designed for AI agents look like?**

The CLI is the most familiar way for agents to run tools. Even with an unfamiliar tool, an agent can follow help to find commands, supply input, and connect results to the next task. In an environment with shell access, an existing CLI can be used without wrapping it in a separate MCP interface, and several steps can run together through command composition.

Building an AI-agent-friendly CLI calls for a somewhat different design focus from building one for people. When selecting a tool, the agent needs capabilities and applicability; when invoking a command, inputs and usage; when receiving a result, processing status and possible next actions. Rather than supplying all instructions in advance, design for **the context needed at each point in execution to arrive at that point**.

When context arrives affects where the agent directs its attention. Instead of putting all usage knowledge into context at the start and relying on it to be retained, provide information suited to the current command and its result. Follow-up commands filled with actual identifiers reduce the need to retrieve earlier instructions and reconstruct a call. Pipes and command composition pass intermediate data directly to the next program when no new decision is needed.

This guide covers CLI design that supports that flow. It explains discovery through help and usage skills, useful defaults and explicit input contracts, result interpretation and follow-up actions, and composition through pipes. It also examines how to connect and verify command declarations and usage knowledge so descriptions and actual behavior stay aligned.

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
