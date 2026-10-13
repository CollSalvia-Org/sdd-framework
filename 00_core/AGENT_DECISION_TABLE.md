# Agent Decision Table

> **Purpose:** give agents a deterministic rule for deciding where a change belongs.

## How to use

Before acting, classify the change into one of these four cases.  
Do not guess. If the case is unclear, treat it as a gap and stop.

| Case                | What it means                                                                                      | What the agent should do                                | What must come back to SDD                                                                                               | Do not do                                                    |
| ------------------- | -------------------------------------------------------------------------------------------------- | ------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------ |
| **Code adjustment** | The behavior is already defined; the implementation just needs to work better or stop failing      | Change code, add or update tests, keep scope narrow     | Update task status, note implementation progress, update design only if the implementation details now differ materially | Do not create a new spec just because code changed           |
| **Contract change** | The observable behavior changes or the current spec/design is no longer true                       | Stop and align the contract first                       | Update spec and/or design so the contract matches reality                                                                | Do not silently change runtime behavior and leave docs stale |
| **Parking lot gap** | The idea is real, but not yet ready for a full spec or implementation                              | Record it as backlog, gap, or future work               | Parking lot entry, ADR note, or audit note if needed                                                                     | Do not force it into SDD as if it were already a feature     |
| **New capability**  | This is a reusable capability with clear scope, validation, and user-facing or system-facing value | Start the SDD flow: feature record, design, spec, tasks | Full SDD chain for the new feature                                                                                       | Do not implement first and spec later                        |

## Decision Rules

1. If the change only makes an existing contract behave correctly, treat it as **code adjustment**.
2. If the change makes the contract itself different, treat it as **contract change**.
3. If the change is useful but not ready to become a feature, treat it as **parking lot gap**.
4. If the change is a stable reusable capability, treat it as **new capability**.

## Archived corrective successor

A correction that restores behavior already specified by an archived feature remains a correction, not a new capability. When the original feature is archived and no declared correction route applies, the correction may proceed as a corrective successor through the ordinary lifecycle, with its own feature record and evidence. The successor must reference the archived predecessor, remain limited to the already-specified behavior, and inherit no validation, verification, audit, archive, or other PASS from the predecessor.

This rule does **not** authorize new capability, behavior absent from the predecessor's authoritative specification, mutation or reopening of the archived feature, or any transition out of `ARCHIVE`.

## Escalation Rule

If an agent cannot clearly classify the change:

- stop
- report the ambiguity
- do not implement
- do not rewrite docs blindly

## SDD Return Path

- **Code adjustment** → tasks, tests, maybe design notes
- **Contract change** → spec and design
- **Parking lot gap** → parking lot or ADR
- **New capability** → full SDD flow

## Hard Rule

Agents must not convert every small fix into a new spec. An archived corrective successor is permitted only by the bounded rule above when the ordinary correction route is unavailable.
Agents must not leave a real contract change undocumented.
Agents must not invent a feature from an implementation patch.
