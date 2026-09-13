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

## Help, schemas, and skills

The three surfaces provide different information.

| Surface | Main contents |
| --- | --- |
| Help | Command descriptions, invocation syntax, options, short examples |
| Schema | Structured representation of declared contracts, including input and output |
| Usage skill | Applicability, workflow, result interpretation, command relationships |

A deployment tool's help describes inputs to `list`, `inspect`, and `logs`. Its diagnosis skill explains how to find a deployment, identify the failed step, and read the relevant logs.

Help must expose a route to reading skills. The tool chooses the exact command name. A small tool can provide one skill; a larger tool should list names and applicability, then let the caller select a skill and follow references for details.

## Selective discovery and full lookup

Inspecting one deployment does not require the entire command tree.

```sh
tool
tool deploy
tool deploy inspect --help
```

For a tool with this many command levels, top-level help supplies enough information to choose what to read next. It shows capability names and applicability, leaving each command's arguments and examples to its detailed help. After selecting a command, the caller reads its detailed input and output contract and follows a related skill when a workflow is needed.

More discovery steps are not inherently better. One help call may suffice for a small tool. As capabilities grow, keep relevant information reachable without requiring unrelated descriptions to be read first.

Full lookup is useful for tool audits or generating integrations.

```sh
tool schema --full
```

The full schema can be saved to a file and filtered programmatically. A tool can provide selective discovery as the normal path while supporting full lookup separately.

Help, schemas, and skills must describe the running version, which must also be queryable. Shipping them together helps maintain alignment but does not establish the accuracy of authored examples. Rediscovery does not remove compatibility needs; the tool's callers and distribution model inform that policy.

---

[Contents](index.md) · [Previous](01-design-goals.md) · [Next](03-input-and-execution.md)
