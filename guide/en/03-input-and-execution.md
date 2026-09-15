# 3. Input and execution

## Input validation

Before invocation, callers must be able to discover positionals, options, accepted values, defaults, and required input.

```sh
tool report export sales --format csv
```

If this command requires a report name and supports only `json` and `csv`, help and execution must follow the same rules. A structured contract, if provided, must describe those rules too.

Unknown commands or options, missing values, invalid types, and extra positionals are errors. Accepting only part of a request and returning success can mislead callers into believing the intended work was performed.

```text
Invalid value: --format yaml
Allowed values: json, csv
```

Errors identify the offending input and provide the information needed to correct it. Close matches can be suggested but must not be substituted and executed automatically. A local explanation need not include the entire help. Structured details such as allowed values save callers from extracting them from prose.

Shared declarations are useful for generating parsing, validation, and descriptions of ordinary arguments. A custom parser for a specialized expression language must describe its grammar and enforce the same input and error contracts.

## Validation before target lookup

Reject what can be decided from the invocation before locating a session, opening
a store or contacting a service. If `wait --until provider:sleeping` names a state
outside a locally known vocabulary, report that input error even when the target
does not exist. If the vocabulary itself comes from the target, explain that the
check depends on execution rather than pretending it is local syntax.

A command that accepts one target rejects `read a b` before reading `a`. Do not
guess that the caller intended a quoted multiword target; identify the extra input
and the accepted arity. Multiple targets are valid only when the command declares
them.

Describe incompatible options and dependencies along with individual values.
Opposing explicit presentation requests should not silently override one another.
If `--timeout` requires `--wait`, state and check that relationship. Distinguish an
option being supplied from its effective value: zero and false can be meaningful
inputs, and a default is not evidence that the caller explicitly supplied an option.
Exact syntax for declaring these rules belongs to each implementation.

## Files and stdin

Nested configuration is often easier to handle as a file than as many options.

```sh
tool deploy create --input-file deployment.json
```

Provide supported formats and examples. If files and options can be combined, define precedence or reject conflicts.

```text
Invalid value at containers[0].healthcheck.intervalSeconds
Expected: a positive integer
Received: -1
```

Commands supporting stdin describe its format, whether it is required, and when it is read. A call that does not select stdin must not wait for an open input pipe to reach EOF. Validate the invocation first, then read and validate selected input before creating resources for the operation. In this example, `--input-file -` selects stdin.

```sh
tool deploy export api |
  tool deploy validate --input-file -
```

Ordinary invocations do not open unexpected questions or menus, including confirmation prompts. Missing required input is explained and causes failure. Interactive operation can be offered as a separate, explicit choice. Declared stdin and explicitly requested waiting are distinct from such prompts. Their behavior must be discoverable before selection.

## Command composition

When an agent invokes commands separately, it reads each result and constructs the next request between calls. This is necessary when output requires a new decision. For a fixed sequence that only passes data between steps, it adds unnecessary round trips.

In the export and validation example, the agent checks the final validation result without copying intermediate JSON. Include common combinations in task skills alongside individual command help.

A step that depends only on success can run after that success. Independent queries can run in parallel. Successful asynchronous submission, however, does not mean the work is complete. If the next step needs completed results, query status or wait first.

Pipelines also require compatible formats and failure handling. The [next chapter](04-results-and-presentation.md#pipes-and-streams) covers those conditions.

---

[Contents](index.md) · [Previous](02-discovery-and-skills.md) · [Next](04-results-and-presentation.md)
