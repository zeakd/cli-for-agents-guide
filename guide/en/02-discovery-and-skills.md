# 2. Discovery and usage knowledge

## Start with the name

The bare tool name and its help option provide the same help:

```sh
tool
tool --help
```

Apply the same rule to command groups:

```sh
tool repo
tool repo --help
```

A group has no implicit action. Exploring it must not create an object or launch an application. An execution command is different: a command requiring no arguments may execute immediately; one missing required input reports an input error. Do not turn every argument-free execution command into help.

Small tools can expose commands directly. Larger tools can introduce meaningful groups. Avoid unnecessary levels, and do not automatically change existing command paths when the number of features grows. The author chooses the structure explicitly.

## Help, skills, and schema

Help describes available commands and their invocation syntax. A schema exposes the declared contract as structured data. Skills teach the caller how to use the tool: when it is useful, how commands fit together, what results mean, and which mistakes to avoid.

Help must identify the entry point for reading that usage knowledge. The spelling of the skill command is a tool choice; its purpose and access path must be clear.

A small tool may have one skill. A larger tool should let callers list skills with their names and applicability, select one, and follow references for more detail.

```text
help -> skill names and when to read them -> selected skill -> references
```

An operation reference generated from declarations is useful, but does not replace authored workflows and guidance. Conversely, a skill should not duplicate every mechanical detail when it can point to current help or a schema.

## Describe the installed behavior

Help, skills, and schema must describe the running version. Shipping usage knowledge with the CLI helps keep them aligned. The tool should also expose its version so callers can identify what they are using.

Version alignment does not prove that authored instructions are accurate. Verify their examples and behavioral claims. Nor does rediscovery remove the need for compatibility: the tool's author chooses that policy according to its callers and distribution model.

## Discover only what is needed

Make the whole tool inspectable without requiring callers to read everything before using one part. For a large command set, show capabilities and their applicability first, then let callers narrow down to a command's contract.

```sh
tool
tool deploy
tool deploy inspect --help
```

The first call lists capabilities with short descriptions. The second lists deployment commands. The third explains one command's inputs, outputs, and examples.

A tool can also offer an explicit full lookup:

```sh
tool schema --full
```

A complete schema is useful for tool audits or generating integrations. It can be saved to a file or filtered programmatically rather than loaded into the agent's context in full. Its availability does not make it a prerequisite for every task.

Apply the same approach to skills: names and applicability first, then a selected skill and its references.

---

[Contents](index.md) · [Previous](01-design-goals.md) · [Next](03-input-and-execution.md)
