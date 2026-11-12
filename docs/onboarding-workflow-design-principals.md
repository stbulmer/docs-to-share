# Temporal Workflow Design Principles

These principles define how we build workflows in Temporal: simple, self-contained workflows that own their own retries, failure handling and compensation. They apply to any orchestration engine, with Temporal-specific notes where they matter.

## 1. Keep each workflow self-contained

Each workflow owns its full flow: it calls activities directly, and its retries, timeouts and compensation live inside it. Temporal's durable execution makes this practical, so there's no need to split a process into nested workflows to manage state or recovery.

- Model each step as an activity with its own retry policy, not as a separate workflow.
- Keep failure handling and compensation in the same workflow that did the work.
- Model each onboarding request as one workflow.

Use a child workflow only as an exception: when another team owns that logic and it needs its own lifecycle, or when the event history would otherwise grow too large. Write down the reason.

**Nesting depth:** none by default. One level is the exception, and it needs a written justification.

## 2. Use business identifiers as workflow IDs

Use meaningful, deterministic workflow IDs, for example `onboarding-{customerId}` or `onboarding-{customerId}-provision-email`.

- This makes starting a workflow idempotent. Temporal rejects a duplicate run with the same ID.
- Set the workflow ID reuse policy explicitly on every workflow.
- Child workflow IDs should be built from the parent's ID, so the relationship is clear from the ID alone.

## 3. Make every activity idempotent

Temporal retries activities automatically, and by default it keeps retrying indefinitely.

- Every activity must be safe to run more than once. For example, check whether a resource already exists before creating it.
- Pass idempotency keys to external APIs wherever they support them.
- Set retry policies explicitly: maximum attempts, backoff, and which errors should not be retried.
- Set activity timeouts on purpose (start-to-close and schedule-to-close).

## 4. Never let a failure be silent

- A child workflow failure reaches the parent as an exception. The parent has to handle it explicitly: compensate, retry or fail. It must never swallow it.
- Set the parent close policy on purpose. The default (TERMINATE) is usually right, because it stops orphaned children running on after the parent finishes.
- A parent must never complete successfully while a required child has failed or is still running.
- Don't mark work as optional unless the business process really allows it. For onboarding, nothing is optional.

## 5. Plan for undoing work (all-or-nothing)

Onboarding has to be all-or-nothing. Use the saga pattern:

- After each step succeeds, register a compensating action, for example: account created, so the undo is to deactivate the account.
- If a later step fails, run the compensations in reverse order.
- Compensations must be idempotent too.
- The customer should only ever see onboarding as complete, or as cleanly rolled back. Never half-done.

## 6. Write down contracts and ownership

- **The owning team** owns a shared child workflow's input and output contract, its versioning and its internal behaviour.
- **The calling team** owns how failures are handled and how compensation works on its side.
- Use Temporal's versioning (worker versioning or patching) for changes, so running workflows don't break.
- Document each shared workflow: purpose, inputs, outputs, failure modes and owner.

## 7. Make every workflow tree observable

- Add search attributes such as `CustomerId` and `OnboardingId` to the parent and to every child.
- You should be able to find every related workflow with a single visibility query.
- Use the workflow's memo or a query handler to show the current step and status in a readable way.
- Alert when a workflow has been running too long or has failed, at the parent level.

## 8. Recovery by automation

- Automated recovery should act on the parent and let Temporal's semantics flow down to the children. Don't act on children directly.
- Prefer letting retry policies and compensation handle failures over manual resets.
- If a reset is needed, reset the parent to a known event, and rely on idempotent activities to make re-running safe.

## Review checklist

Before merging a new workflow, confirm that:

- Every child workflow has a written reason to exist.
- The workflow is self-contained, with no child workflows unless the exception is justified.
- Workflow IDs are deterministic and built from business IDs.
- Every activity is idempotent and has an explicit retry policy and timeouts.
- Every child failure is handled explicitly in the parent.
- The parent close policy is set on purpose.
- Compensations are defined for every step that changes something.
- Search attributes are set for correlation.
- The owner and contract are documented.

