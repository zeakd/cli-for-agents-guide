# 7. Conditional patterns

Common contracts do not imply identical internal machinery. Choose facilities according to the state and dependencies the tool actually owns.

## Tools with settings and persistent state

Let a tool's owned settings and state use a replaceable location. A home directory is one way to collect configuration, state, caches, credentials, and logs. Document which resources it covers.

When a test home is selected, do not silently fall back to the real user's settings or storage. Keep explicit user work targets separate: selecting an output file is not the same as choosing the tool's own home.

A stateless transformation may need no home at all. If state also lives in an OS credential store or another external service, replacing a directory alone is insufficient; provide an appropriate test boundary for that dependency too.

## Network and authentication

Put external access behind replaceable boundaries. Tests can choose a different endpoint or client. A temporary home does not prevent real network traffic.

Report connectivity problems, authentication state, and rejected requests in ways callers can distinguish. If a procedure requires a human, say what is needed and how to proceed. Choose the authentication protocol and credential storage according to the service and environment; neither one protocol nor token-printing is a universal requirement.

Timeouts and retries must reflect the operation's effects. A lost response to a mutation is not sufficient evidence that retrying is harmless. Use the repetition contract described in [chapter 5](05-state-and-actions.md).

## Long-running work and streams

Long-running work may expose status, waiting, results, and cancellation. A framework can offer an optional common contract without requiring every command to have a daemon or job database. Describe accepted, running, completed, and failed states as appropriate; expose cancellation only when it is supported.

A stream is not a completed value. Once bytes have been written, a failure cannot retract them. Document completion and failure signaling, report errors separately from payload stdout, and preserve an honest account of partial output. Waiting for a known operation is distinct from unexpectedly opening an input prompt.

## Shared resources

When several callers use one resource, make the target explicit and consider concurrent access. Identifiers should resolve to the intended object rather than a changing UI focus or guessed list position.

Locks, queues, daemons, and ownership records are possible techniques. Choose them for the resource's needs, not because every AI tool needs a long-lived process. Whatever the mechanism, expose relevant state and provide a way to isolate or control it in tests.

## Distribution and updates

Choose a distribution and update mechanism suitable for the users and platform. Expose the running version and keep its usage knowledge aligned. Package managers and self-updaters must not unexpectedly overwrite resources another mechanism owns.

A standalone updater may need download validation, atomic replacement, and recovery after interruption. Those are implementation concerns for tools that update themselves, not requirements for all CLI frameworks. This guide does not prescribe a package manager or a single-binary format.

---

[Contents](index.md) · [Previous](06-authoring-and-testing.md) · [Next](08-evidence-and-scope.md)
