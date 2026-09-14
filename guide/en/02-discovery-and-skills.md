# 2. Discovery and usage knowledge

## Tool names and help

Invoking the tool name alone provides the same help as `--help`.

```sh
tool
tool --help
```

Command groups support the same discovery path.

```sh
tool deploy
tool deploy --help
```

`tool deploy` lists deployment commands. Groups have no implicit execution; exploring one does not start a deployment or create an object.

Execution commands are distinct from groups. A command such as `tool status`, whose required input is already determined, can run without additional arguments. An execution command missing required input returns an input error.

Small tools can expose commands directly; larger tools can use meaningful groups. Authors choose the structure. Adding more features must not automatically change existing command paths.

## Help and usage skills

Help is the default place to discover a command's contract. It describes how to invoke the command and the input and result information needed to use it. A caller should not need a separate schema lookup to learn essential invocation details.

| Surface | Main contents |
| --- | --- |
| Help | Command descriptions, invocation syntax, inputs, relevant result structure, short examples |
| Usage skill | Applicability, workflow, result interpretation, command relationships |

A deployment tool's help describes inputs to `list`, `inspect`, and `logs`. Its diagnosis skill explains how to find a deployment, identify the failed step, and read the relevant logs.

Help must expose a route to reading skills. The tool chooses the exact command name. A small tool can provide one skill; a larger tool should list names and applicability, then let the caller select a skill and follow references for details.

## Read the information needed for the task

Inspecting one deployment does not require the entire command tree.

```sh
tool
tool deploy
tool deploy inspect --help
```

For a tool with this many command levels, top-level help supplies enough information to choose what to read next. It shows capability names and applicability, leaving each command's arguments and examples to its detailed help. After selecting a command, the caller reads its detailed input and output contract and follows a related skill when a workflow is needed.

More discovery steps are not inherently better. One help call may suffice for a small tool. As capabilities grow, keep relevant information reachable without requiring unrelated descriptions to be read first.

Usage guidance can include the contract details needed at each step. A workflow that extracts deployment IDs can show the relevant list field beside the list command, then reference the inspection command that accepts an ID. Include the fields needed for that workflow rather than attaching the entire tool specification. Generate command syntax and field descriptions from shared declarations where possible, so the guidance does not become a separately maintained copy.

A separate global schema command is not required. Tools with integrations that consume structured contracts may provide a programmatic API or an export for that purpose. Choose the scope and format for the consumer; ordinary command use should remain understandable through help. This guide does not establish that adding a schema lookup improves agent performance over sufficient help and usage guidance.

Help and usage skills must describe the running version, which must also be queryable. Any structured contract offered must agree with it too. Shipping them together helps maintain alignment but does not establish the accuracy of authored examples. Rediscovery does not remove compatibility needs; the tool's callers and distribution model inform that policy.

---

[Contents](index.md) · [Previous](01-design-goals.md) · [Next](03-input-and-execution.md)
