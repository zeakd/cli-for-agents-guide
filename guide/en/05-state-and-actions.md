# 5. State and follow-up actions

## Explain the result's meaning

Include state, scope, and provenance when their absence could mislead a caller. A cached value may need its refresh time; a write may need its target environment; a list may need to say that more results exist.

Do not add every metadata field to every result. Include information that changes how the result should be interpreted or used.

## Offer relevant next actions

An error can carry recovery instructions. A successful result can also expose useful continuations: inspect a created object, fetch another page, check a job, verify a change, or cancel work that remains cancellable.

The author defines these relationships. The CLI fills in actual identifiers and filters actions according to known state. This does not require the CLI to infer the user's plan.

An illustrative response might communicate:

```text
Result: job-42 was accepted; work is not complete.
Actions:
  Check status -> tool job status job-42
  Read result  -> tool job result job-42
  Cancel       -> tool job cancel job-42
Hint: the result becomes available after completion.
```

This describes the meaning, not a fixed wire format. Pair actions with explanations and complete invocation arguments. Use an argument template only when further caller input is genuinely required, and identify that requirement. Plain hints are useful when a command cannot express the guidance.

Offer only actions directly relevant to this result. Omit them when there is nothing useful to add. Do not dump the full command catalog or require one universal workflow.

## Describe effects without granting permission

Make read and state-changing operations distinguishable. Where relevant, explain reversibility and repeat behavior. Treat missing information as unknown, not as evidence that an action is read-only or safe.

The same information should be available for follow-up actions. A suggested command is not authorization to execute it. The calling agent must still respect the user's purpose and permissions.

## Diagnose without requiring a preflight

When a tool has useful readiness checks, it can expose them through an optional `doctor` command. Authors declare checks relevant to the tool. The report identifies problems and corrective actions; it must not perform the repairs itself. Expose repairs as separate, explicit actions.

A caller need not run doctor before every task. Each execution command must handle unmet prerequisites clearly rather than depending on a previous diagnostic run. Even a recent successful check cannot guarantee that conditions remain unchanged.

## Make repetition understandable

A missing response does not prove that a command did not run. Explain whether repeating an operation is safe. When duplicates matter, provide a result lookup, request identifier, or deduplication mechanism appropriate to the tool.

Not every command needs such machinery. A read or deterministic transformation may already be safe to repeat. Do not automatically retry an uncertain mutation merely because its acknowledgment was lost.

---

[Contents](index.md) · [Previous](04-results-and-presentation.md) · [Next](06-authoring-and-testing.md)
