# 6. Authoring and testing

## Command declarations

Inputs, descriptions, execution, and output formats form a command's definition. Generating parsing, validation, help, and schema from that definition avoids writing the same facts in several places. Require command descriptions and input contracts; provide a default output contract and require declarations for alternative formats.

Registries and feature folders are ways to implement this. A command need not fit one file. Organize definitions and usage knowledge so they can be found together when making a change.

## Assembling usage knowledge

The units used to maintain code can differ from those used to read instructions. Descriptions of `list`, `inspect`, and `logs` are easier to update near their commands, while a caller investigating a failed deployment needs guidance connecting all three. A framework can compose task guides by referencing command declarations and knowledge.

```text
deploy/
├── Shared guidance
│   └── Workflow for investigating a failed deployment
├── list
│   ├── Input and output declarations
│   └── Finding the deployment to investigate
├── inspect
│   ├── Input and output declarations
│   └── Interpreting state
└── logs
    ├── Input and output declarations
    └── Reading failure logs
```

Command names, arguments, and defaults come from declarations. Authors write applicability, cautions, and result interpretation. Skills that include the same command can reference the same knowledge fragment.

Concatenating command descriptions does not produce a workflow. Order, branches, and composition need their own explanation. A framework can expand relevant declarations and command knowledge within that explanation. File layout and assembly APIs belong to the implementation. Provide tool-level usage knowledge without requiring a separate skill for every command.

## Separating dependencies

Separate logic that computes decisions from code accessing files, networks, clocks, and output. Passing environmental dependencies through a context or function arguments makes them replaceable in tests without global patching.

Pure functions are checked through inputs and return values. Handlers receive the dependencies needed to reproduce particular errors or responses. Code that needs no environment does not need a context merely for uniformity.

Represent expected failures in a form that makes complete handling straightforward. Result values, discriminated unions, and typed exceptions are internal choices that depend on the language and implementation.

## Verification boundaries

| Target | Main checks |
| --- | --- |
| Pure function | Computation and return values for given input |
| Handler or service | Interactions with injected dependencies |
| Actual CLI | Argument forwarding, configuration loading, output channels, exit codes, persistence |

Actual CLI tests cover connections that function tests cannot establish alone. Stateful tools can use temporary homes; others can use temporary input or test endpoints.

Review authored usage knowledge and verify its behavioral examples. A skill file's existence or inclusion of a command name does not establish usefulness; mechanical checks alone cannot do that. Mechanical checks can find missing references and incorrect example results. Representative agent tasks reveal whether callers find the needed skill, avoid unrelated skills, construct valid calls, and use known combinations.

Assess call count and time alongside correctness. Distinguish skipping necessary checks from eliminating unnecessary round trips. Document repeatable verification, without requiring the same evaluation infrastructure for every tool or duplicating every case at every level.

---

[Contents](index.md) · [Previous](05-state-and-actions.md) · [Next](07-conditional-patterns.md)
