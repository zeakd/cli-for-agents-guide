# 1. Design goals

Using a CLI involves more than executing commands. An agent finds a capability, constructs input, interprets the result, and continues the task. When information is missing at one of these steps, it must search external documentation or make additional calls to discover it.

The information a CLI provides changes with the execution stage. Selecting a tool requires capabilities and applicability; constructing a call requires an input contract. After execution, the caller needs the actual result and next actions. Providing this information when needed, rather than requiring it all to be read first, lets callers work with context relevant to the current decision.

Designing for attention concerns timing as well as volume. Tool descriptions enter a context that already contains the user's request and earlier work. Organize help and skills around the relevant task, and results around values that become known only after execution. Returning an inspection command with the actual identifier of a newly created object is one example.

Providing this guidance does not require the CLI to infer the user's goal. The author defines capabilities and relationships; the CLI fills in values obtained during execution. The calling agent chooses actions according to the user's request.

## Use, authoring and change

The design serves agents using the CLI and agents helping build it. A caller needs
to find the relevant contract and act without guessing. An author needs to express
that contract in declarations that are easy to read, compose and check. Maintaining
the tool adds a third requirement: descriptions and execution must change together.

These goals do not imply maximizing the number of features or the strictness of
types. A declaration system that requires extensive special knowledge for a simple
command can make authorship harder. Prefer defaults and checks that prevent likely
mistakes while leaving ordinary commands straightforward.

Assess a design by the work it removes: unnecessary discovery or calls for the
user, repeated implementation choices for the author, and opportunities for drift
when a command changes. The CLI should report what it knows without claiming to
infer the caller's intent or to know outcomes it did not observe.

## Defaults and the cost of calling

Defaults have the greatest effect on frequently used paths. If reading ordinary results structurally requires an extra flag, or useful values must be extracted from unnecessarily large output, every invocation adds work.

This guide recommends JSON as the default format for ordinary execution results. Human presentation is an explicit choice, and large results expose a useful scope first. Invalid input returns an error with evidence for correction instead of silently accepting part of the request.

Design also affects call count. Executing a known procedure one step at a time makes the agent read each intermediate result and construct the next command. Compatible inputs and outputs, together with documented combinations, can reduce round trips that require no new decision.

## Authoring and maintenance

Adding an option changes parsing, validation, and help together. If the tool also exposes a structured contract, that representation must stay aligned. Maintaining these separately makes partial updates easy. Generating them from shared declarations and keeping related usage knowledge close to the command makes the information needed for a change easier to find.

Caller behavior and the structure that maintains it are connected. Accurate help requires description and implementation to change together; reliable error contracts require checking actual output and exit codes. The following chapters address both aspects.

---

[Contents](index.md) · [Next](02-discovery-and-skills.md)
