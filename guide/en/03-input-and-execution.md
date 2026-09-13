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

---

[Contents](index.md) · [Previous](02-discovery-and-skills.md) · [Next](04-results-and-presentation.md)
