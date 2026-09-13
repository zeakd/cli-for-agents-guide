# 3. Input and execution

## Make input discoverable

Describe positionals, options, accepted values, defaults, and required input before execution. Include stdin when a command reads piped content. Callers need to know its expected format and whether it is required.

Consider an illustrative command:

```sh
tool report export sales --format csv
```

Its contract might require a report name and accept only `json` or `csv` for `--format`. Help, schema, and execution must agree on those rules.

## Reject invalid input

Do not silently ignore unknown commands or options, missing required values, invalid types or allowed values, or positionals beyond the declared arity. Accepting only part of a request and reporting success misrepresents what happened.

An input error should identify the offending input and supply the evidence needed to correct it:

```text
Invalid value: --format yaml
Allowed values: json, csv
```

A close match can be suggested, but must not be substituted and executed automatically. Include relevant usage rather than dumping the entire help whenever a local explanation is sufficient. Structured error details let a caller use the allowed values without extracting them from prose.

## Default to non-interactive execution

An ordinary invocation must not stop at an unexpected question or menu. If required input is absent, explain what is needed and fail. A tool may offer interactive operation as an explicit choice.

`--human` changes presentation, not interactivity. Declared stdin and an explicitly requested wait are also different from an unexpected prompt. Their behavior must be discoverable before the caller selects them.

## Share the definition

For ordinary arguments, a framework should generate parsing, validation, help, and schema from one input declaration. This makes a new option one change instead of several coordinated edits.

A specialized expression language may need a custom parser. That escape hatch must still describe its grammar and enforce the same input and error contracts. Parser implementation is a choice; agreement between explanation and behavior is the requirement.

## Accept complex input

Keep common input short with arguments and options. Files or an explicitly selected stdin path may be more suitable for nested configuration or collections of items.

```sh
tool deploy create --name api --region seoul
tool deploy create --input-file deployment.json
```

Describe the input format and provide examples. If files and options can be combined, define their precedence or reject conflicts. Identify invalid values by their location within the input:

```text
Invalid value at containers[0].healthcheck.intervalSeconds
Expected: a positive integer
Received: -1
```

## Compose steps that need no new decision

Do not require the agent to intervene between steps that need no new decision. Provide compatible input and output contracts, and include common combinations in usage knowledge.

```sh
tool deploy export api |
  tool deploy validate --input-file -
```

In this example, `export` writes the deployment configuration itself and `validate` accepts that format. The `-` value explicitly selects stdin. The agent does not need to read and copy the intermediate JSON into another call.

| Relationship between steps | Composition |
| --- | --- |
| The next step accepts the output format and its completeness guarantees | A pipe with the required completeness checks |
| The next step depends only on the success guaranteed by the previous command | Run it after that success; wait or query status if acceptance alone is insufficient |
| Operations are independent | Parallel execution |
| The result determines the target or method | Inspect the result before choosing the next call |

Do not skip necessary decisions just to reduce calls. Reduce the cost of making the agent reconsider an already determined procedure. [Chapter 4](04-results-and-presentation.md#output-for-composition) covers compatible payloads and failure handling.

---

[Contents](index.md) · [Previous](02-discovery-and-skills.md) · [Next](04-results-and-presentation.md)
