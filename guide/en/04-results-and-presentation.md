# 4. Results and presentation

## Default output

Ordinary execution commands return JSON by default. Output format does not change based on TTY detection. Agents can run in terminals, and humans can send output to files or other programs.

Use `--human` to select human presentation.

```sh
tool repo list
tool repo list --human
```

Presentation can use tables, lists, or prose appropriate to the result, while preserving the operation and success meaning. Frameworks provide a default readable presentation so authors add custom renderers only when needed. `--human` does not enable interactive input.

Help and skills can use text or Markdown. Commands that export file content or records for another program put that payload directly on stdout. A list of file paths, a CSV export, or JSONL logs can be more useful in their declared format than inside a result envelope that every consumer must unwrap.

Make the payload format and supported presentation options discoverable before invocation. A payload command does not need a separate human presentation when the exported data already serves its purpose. Reject unsupported options instead of ignoring them.

## Channels and exit codes

Under the default JSON contract, success and failure results go to stdout. Callers read the result from the same stream in either case. Progress and supplementary diagnostics go to stderr.

Commands whose stdout is a payload send errors to stderr. Inserting an error or success envelope among file bytes or records changes the payload contract and can corrupt the data or make it hard for consumers to distinguish.

| Output contract | stdout | stderr |
| --- | --- | --- |
| Ordinary JSON result | Success or failure result | Progress and supplementary diagnostics |
| Payload | Data in the declared format | Errors, progress, and supplementary diagnostics |

Once an invocation identifies a payload command, validate arguments, options, and other locally checkable inputs before beginning work or emitting payload. Report those errors on stderr and keep stdout empty. Errors found only while reading or processing data follow the partial-output contract below. For routing failures where no command has been identified, follow the CLI's documented common error contract. Expected failures and unexpected exceptions must both remain recognizable as failures. If stderr includes both errors and diagnostics, describe how to distinguish them rather than promising that the whole stream is one JSON document.

A JSON export can itself be a payload; JSON syntax does not imply a result envelope. If a command offers both result envelopes and payload formats, declare the channel contract for each and where invalid format selections are reported. When all formats are payloads, the same stderr error contract applies to all of them.

Failures also use nonzero exit codes. Commands performing several units of work define whether to continue or stop after a failure and report successful, failed, and unprocessed scope. If an attempted unit fails, the default exit status is nonzero. A command promising completed work also must not exit successfully while required units are unprocessed or have unknown outcomes.

Document what callers can distinguish through exit status and error details, including completion, invalid input, application failure, internal failure, and interruption. Distinct numeric codes for every category are not required; signal termination also depends on the host environment.

A submission-only command can return zero for successful acceptance. Its result states that the work is not yet complete. A payload export that promises the complete selected scope must finish producing and writing that scope before reporting completion. Use the declared exit status to signal completion; any scope summary belongs on stderr or another declared side channel, not among the payload records.

## Output scope

Identifiers and state may suffice when selecting an object from a list. Investigating that object can require detailed configuration or logs. Separating list and detail lookup avoids making callers read every object in full at the outset. Keep the information needed for selection in the list, with identifiers and a path to detail lookup.

Long logs can likewise begin with relevant failures and a summary. In either case, callers must be able to distinguish complete, partial, and summarized results.

```text
Returned: 20 of 143 deployments
Next page: tool deploy list --cursor page-2
```

Let callers control volume through limits, field selection, or detail levels suitable for the domain. Mark omitted items differently from abbreviated content. Provide the next lookup when pages remain, or original content and detail lookup when summarizing. State when fuller information is unavailable. Reducing output must not hide facts that change its meaning.

## Pipes and streams

Query results can include state and follow-up actions. To pass only configuration to another command, provide a dedicated `export` path or allow fields to be extracted, identifying which part satisfies the receiving command's input contract.

For file paths passed as arguments to another program, define the separator and supported filenames. One path per line cannot represent filenames containing a newline unambiguously. Plain `xargs` also interprets spaces and quotes. A NUL-delimited mode paired with `xargs -0` preserves those characters. This example uses GNU xargs; `-r` avoids running the search when the path list is empty ([option reference](https://www.gnu.org/software/findutils/manual/html_node/find_html/xargs-options.html)):

```sh
tool files --null | xargs -0 -r rg -n -- 'error'
```

The pipeline illustrates argument delivery, not a completeness check. Inspect producer and consumer failures when completeness matters. GNU xargs maps both a search command's “no matches” status and its ordinary error status to 123, so this pipeline cannot distinguish them by exit status alone ([exit-status reference](https://www.gnu.org/software/findutils/manual/html_node/find_html/Invoking-xargs.html)). Use a wrapper that preserves that distinction, or run the search directly when it matters.

Large record sets can use JSONL, with one object per physical line. Serialize each record without pretty-printing; newlines within strings are escaped. Include identifiers that let the caller retrieve the original record. Text searches match the serialized representation: escapes for newlines, quotes, backslashes, or non-ASCII characters can change what a literal search finds. Declare the encoding and escaping convention when searchability matters. TSV exports likewise need an explicit convention for tabs and newlines inside fields.

Produce records incrementally so a large export does not require building the entire result in memory. Consumers can process records as they arrive where their contract allows it; a consumer that needs to validate the whole input first can spool it to disk or declare an input-size limit. A slow receiver should slow the producer rather than cause an unbounded queue. Individual records can still be large: define any size limits or alternate way to retrieve large content instead of silently truncating it.

```sh
tool records export --format jsonl |
  tool records validate --input-format jsonl --input-file -
```

Stream producers describe their completion and failure signals. Once bytes are written they cannot be retracted. If production fails, already emitted data remains as a partial prefix, and an output failure or abrupt termination can leave the last record incomplete or lose buffered data. A record-oriented format alone does not guarantee that every record arrives atomically. For detected failures, report the error on stderr and return a nonzero exit status; abrupt termination may prevent that report. Intentional early reader closure follows the separately declared policy below. Do not append an error object to a stream of ordinary records.

End of input does not guarantee producer success. A producer can emit some records and then fail; the consumer may still see only the end of input. Bytes written by the producer also do not prove that the receiver consumed or stored them.

Operations requiring complete input must establish completeness through producer exit status when it guarantees full output, a completion marker outside the data or defined by the declared format, expected counts, or another suitable check. A syntactically complete document alone does not prove that it includes the whole requested scope. Checking pipeline failure status does not undo changes already performed by the consumer. Mutating commands must define whether they validate all input and producer completion before acting, or process items incrementally and report partial results.

A receiver may intentionally stop early, such as when selecting the first few records. Declare how the producer treats a closed pipe and stop unnecessary production while releasing resources. If the producer exits successfully after early reader closure, document that success does not guarantee full output. This is a different contract from an export that promises the entire selected scope. Under that policy, neither producer nor receiver exit status alone proves completeness. Callers needing the whole export require separate evidence, such as expected counts or a declared completion marker, or a command whose success promises the full export.

Help for an export should make the format, record meaning, channels, and completion contract visible together. The following example chooses a full-export contract that treats early reader closure as failure:

```text
Output:
  UTF-8 JSONL; one log block object per line, without pretty-printing.
  Non-ASCII text is written directly as UTF-8.
  sessionId  Session containing the block
  blockId    Identifier used to retrieve the original block
  text       Block text; embedded newlines are JSON-escaped
  stdout contains only records. Errors go to stderr.
  A failed run may leave a partial prefix, including an incomplete last record.
  Exit 0 means this command wrote all selected records.
  A closed pipe while writing is a failure; check the receiver's status too.
```

Describe fields relative to each record rather than implying a surrounding result envelope. List the supported format and presentation options alongside the inputs.

---

[Contents](index.md) · [Previous](03-input-and-execution.md) · [Next](05-state-and-actions.md)
