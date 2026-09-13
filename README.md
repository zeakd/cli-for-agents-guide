# CLI for agents

**What should a CLI designed for AI agents look like?**

A CLI gives an agent a way to run a tool. But being able to invoke it does not mean the agent has the information needed to use it well.

Command syntax may be available while guidance on when to use it lives elsewhere, and invalid options may be ignored. A result may leave it unclear whether work is complete, how much was processed, or what can follow. The caller fills those gaps through exploration and inference.

This guide treats **discovery, invocation, result interpretation, and choosing the next action** as part of CLI design. It also explains structures for building and maintaining that surface consistently.

**[Read the guide →](guide/en/index.md)** · [Korean](README.ko.md)

### Discover usage knowledge

Start with the tool name and follow help to usage skills. Learn when commands are useful, how to combine them, and what to watch out for alongside their syntax. Discover what is relevant first, then choose the details to read.

### Invoke commands correctly

Design defaults so that ordinary use does not require repeated instructions or extra options. Expose input contracts and give callers evidence to correct invalid requests. The guide covers JSON defaults for ordinary results, explicit human presentation, and execution that does not stop at unexpected prompts.

### Use results to inform the next decision

Distinguish acceptance from completion and partial results from complete ones. Report the state and scope needed to interpret a result. Offer relevant follow-up commands and hints while leaving the choice of action to the caller. When information is large, provide a useful amount and a route to more.

### Keep descriptions and behavior aligned

Generate help and validation from shared declarations, separate decisions from side effects, and test both functions and actual CLI invocations. These structures help agents building and maintaining the code reduce omissions and inconsistencies, while keeping the surface reliable for callers.

## Scope

This is a language- and framework-independent design guide. No package is required to apply its principles.

Common contracts are distinct from conditional patterns. Home directories, networking, authentication, and long-running work are covered for tools that need those facilities. Not every CLI needs the same internal structure.

The guide has eight chapters. English is canonical; the Korean edition covers the same material. Commands written as `tool ...` illustrate designs, not runnable examples of a particular package.

[Browse all chapters →](guide/en/index.md)

## Reference implementations

| Repository | Role |
| --- | --- |
| [cli-for-agents-ts](https://github.com/zeakd/cli-for-agents-ts) | TypeScript package `cli-for-agents`, with runnable examples |
| `cli-for-agents-go` — planned | Go implementation |

The TypeScript implementation offers concrete code through which to examine the guide's principles. Its current behavior also differs from the recommendations, including JSON defaults and error channels, and some contracts remain unimplemented. See [versioned implementation coverage](guide/en/08-evidence-and-scope.md#reference-implementation-coverage) for the differences.

License: [MIT](LICENSE)
