# 4. Results and presentation

## Default output

Ordinary execution commands return JSON by default. Output format does not change based on TTY detection. Agents can run in terminals, and humans can send output to files or other programs.

Use `--human` to select human presentation.

```sh
tool repo list
tool repo list --human
```

Presentation can use tables, lists, or prose appropriate to the result, while preserving the operation and success meaning. Frameworks provide a default readable presentation so authors add custom renderers only when needed. `--human` does not enable interactive input.

Help and skills can use text or Markdown. Commands whose output is file content declare its format, such as CSV, source text, binary, or a stream. Supported formats and presentation options must be discoverable before invocation. Reject unsupported options instead of ignoring them.

## Channels and exit codes

Under the default JSON contract, success and failure results go to stdout. Callers read the result from the same stream in either case. Progress and supplementary diagnostics go to stderr.

Commands whose stdout is a payload send errors to stderr. Inserting an error envelope among file bytes or records can corrupt the data or make it hard for consumers to distinguish. Expected failures and unexpected exceptions must both remain recognizable as failures.

Failures also use nonzero exit codes. Commands performing several units of work define whether to continue or stop after a failure and report successful, failed, and unprocessed scope. If an attempted unit fails, the default exit status is nonzero. A command promising completed work also must not exit successfully while required units are unprocessed or have unknown outcomes.

A submission-only command can return zero for successful acceptance. Its result states that the work is not yet complete.

## Output scope

Lists can return key fields and a bounded number of items first. Long logs can begin with relevant failures and a summary. Callers must be able to distinguish complete, partial, and summarized results.

```text
Returned: 20 of 143 deployments
Next page: tool deploy list --cursor page-2
```

Let callers control volume through limits, field selection, or detail levels suitable for the domain. Mark omitted items differently from abbreviated content. Provide the next lookup when pages remain, or original content and detail lookup when summarizing. State when fuller information is unavailable. Reducing output must not hide facts that change its meaning.

## Pipes and streams

Query results can include state and follow-up actions. To pass only configuration to another command, provide a dedicated `export` path or allow fields to be extracted, identifying which part satisfies the receiving command's input contract.

Stream producers describe the format and completion and failure signals. Once bytes are written they cannot be retracted; report partial output honestly when production fails. Large record sets can use a format such as JSONL, with one object per line.

```sh
tool records export --format jsonl |
  tool records validate --input-format jsonl --input-file -
```

End of input does not guarantee producer success. A producer can emit some records and then fail; the consumer may still see only the end of input.

Operations requiring complete input must establish completeness through producer exit status, a completion marker, expected counts, or another suitable check. Checking pipeline failure status does not undo changes already performed by the consumer. Mutating commands must define whether they validate all input and producer completion before acting, or process items incrementally and report partial results.

---

[Contents](index.md) · [Previous](03-input-and-execution.md) · [Next](05-state-and-actions.md)
