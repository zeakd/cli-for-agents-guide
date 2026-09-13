# 4. Results and presentation

## Choose useful defaults

Ordinary execution commands return JSON by default. Do not switch between machine and human formats because stdout happens to be a terminal or a pipe. An agent may use a terminal, and a human may redirect output.

Use `--human` to request a readable presentation of the same result:

```sh
tool repo list
tool repo list --human
```

That presentation may be prose, a table, or a list. It must not change the operation or its success/failure meaning. A framework should provide a generic readable fallback for ordinary commands so that `--human` works without a custom renderer. Authors can supply better renderers for individual commands. Human mode does not enable prompts.

Help and skills are reading surfaces and can use text or Markdown. Commands whose output is itself an artifact may declare another format: source text, CSV, binary data, or a stream. Expose that format and any supported presentation options before invocation. Reject unsupported options rather than quietly ignoring them.

## Keep channels predictable

For the default JSON contract, both success and failure results go to stdout. This gives the caller one result stream to parse for either outcome. Progress and supplementary diagnostics go to stderr, where they cannot corrupt that structured result. Exit status also signals failure: successful execution uses zero; failure uses a nonzero status.

When stdout is an artifact or payload stream, report errors on stderr. Never insert an error envelope or progress message into a file's bytes. The output declaration determines which contract applies.

These are semantic contracts, not a prescribed set of envelope field names. Expected failures and unexpected exceptions must both remain recognizable as failures. Internal exception or result-value conventions are discussed in [chapter 6](06-authoring-and-testing.md).

## Say what succeeded

Accepting a request and completing its work are different outcomes. A successful submission can exit zero while clearly reporting that work is still pending. It must not claim the requested background operation has finished.

Commands that perform several units of work, such as batch commands, may permit partial success. Their contract determines whether to continue or stop after a failure. Report the successful, failed, and unprocessed scope; do not label a partly failed execution as complete success. If any attempted unit of work fails, use a nonzero exit status by default.

## Bound output without hiding facts

Large output can bury important information and make reasoning harder. Return what is needed for the next decision, with an explicit route to more detail.

| Result | Useful default | Expansion |
| --- | --- | --- |
| Many objects | A bounded list of key fields | Next page and object details |
| Long execution log | Summary and relevant failures | Full log or a selected range |
| Large document | Requested sections or an identified summary | Original content |

Distinguish omitted items from abbreviated content. State when the response is partial or summarized. Supply the next page, detail lookup, or other expansion route, and disclose when fuller information is unavailable. Never present a truncated result as complete or hide a fact that changes its meaning.

Limits, time ranges, field selection, and detail levels can be domain-specific. The common requirement is control over volume and an honest account of what was returned.

---

[Contents](index.md) · [Previous](03-input-and-execution.md) · [Next](05-state-and-actions.md)
