# 7. Conditional patterns

## Owned settings and state

A stateless transformation may need no home directory. Tools retaining settings, caches, or authentication information benefit from replaceable storage locations. Document which resources the location covers, including any settings, state, caches, credentials, and logs.

When a test home is selected, read and write the tool's owned state there. Falling back to real user settings when values are missing makes tests depend on the user's environment. Keep user-selected work files separate from the tool's own storage.

An OS credential store or remote service is not isolated by changing a home directory. Those dependencies must also be replaceable or controllable in tests.

## Network and authentication

Replaceable network clients or endpoints allow responses and errors to be reproduced without the real service. A temporary home does not prevent real network requests without separate measures.

Distinguish connection failure, expired authentication, and insufficient permissions. For authentication requiring a person, provide the steps and access path. No single authentication protocol or credential storage method is required for every tool. Timeouts and retries follow the effects and duplicate-handling contract in [chapter 5](05-state-and-actions.md#unknown-outcomes).

## Long-running work and shared resources

Long-running work can offer acceptance, status, waiting, results, and cancellation. If a remote service owns the state, the CLI may only query it. Common behavior does not require a daemon or local job database. Expose cancellation only when supported, and describe the state left afterward.

When several callers share a resource, use stable identifiers for targets. Current UI focus or changing list positions can lead to operations on the wrong object. Apply locks, queues, or ownership records where needed, and make relevant state inspectable or controllable in tests.

Stream completion and partial failure follow the [output contract in chapter 4](04-results-and-presentation.md#pipes-and-streams).

## Distribution and updates

Align the running version with its usage knowledge and let callers identify that version.

If package managers and self-updates are both supported, define which files each owns. One mechanism must not unexpectedly overwrite another's installation. Download verification, atomic replacement, and recovery after interruption are implementation concerns for tools that update themselves.

---

[Contents](index.md) · [Previous](06-authoring-and-testing.md) · [Next](08-evidence-and-scope.md)
