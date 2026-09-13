# 1. Design goals

Using a CLI involves more than executing commands. An agent finds a capability, constructs input, interprets the result, and continues the task. When information is missing at one of these steps, it must search external documentation or make additional calls to discover it.

The CLI can explain much of this directly. Help and schemas describe accepted input, usage skills explain workflows, and results carry facts observed during execution. Creating an object can return both its identifier and a command to inspect it.

Providing this guidance does not require the CLI to infer the user's goal. The author defines capabilities and relationships; the CLI fills in values obtained during execution. The calling agent chooses actions according to the user's request.

## Defaults and the cost of calling

Defaults have the greatest effect on frequently used paths. If reading ordinary results structurally requires an extra flag, or useful values must be extracted from unnecessarily large output, every invocation adds work.

This guide recommends JSON as the default format for ordinary execution results. Human presentation is an explicit choice, and large results expose a useful scope first. Invalid input returns an error with evidence for correction instead of silently accepting part of the request.

Design also affects call count. Executing a known procedure one step at a time makes the agent read each intermediate result and construct the next command. Compatible inputs and outputs, together with documented combinations, can reduce round trips that require no new decision.

## Authoring and maintenance

Adding an option changes parsing, validation, help, and schema together. Maintaining these separately makes partial updates easy. Generating them from shared declarations and keeping related usage knowledge close to the command makes the information needed for a change easier to find.

Caller behavior and the structure that maintains it are connected. Accurate help requires description and implementation to change together; reliable error contracts require checking actual output and exit codes. The following chapters address both aspects.

---

[Contents](index.md) · [Next](02-discovery-and-skills.md)
