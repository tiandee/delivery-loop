---
name: delivery-loop
description: Run an autonomous, risk-proportionate development-to-delivery loop for a software change. Use when asked to implement and verify a change, especially when it must be released, deployed, or validated through a real consumer path; also use when CI passes but users cannot use the delivered artifact.
---

# Delivery Loop

Carry a change from implementation to evidence-based acceptance. Treat a local test suite and CI as evidence, not universal proof of user success.

## Authority And Completion States

- Obtain explicit authorization in the current task before each external state change: commit, push, tag, publish, deploy, migration, or production data operation.
- Treat authorization for one action as authorization for that action only. Do not infer release authorization from permission to edit or test.
- Never expose credentials, tokens, registry configuration, or secret values in output, commits, logs, or artifacts.
- Finish as `local_verified_release_pending` when release is not authorized. Finish as `released_and_e2e_verified` only after the authorized release and real-path validation succeed.

## Select The Delivery Mode

Choose the smallest mode that matches the request and risk.

| Mode | Use for | Minimum evidence |
| --- | --- | --- |
| Local | Docs, isolated fixes, or unapproved releases | Focused tests and local acceptance path |
| Integration | Shared behavior, API contracts, or multi-service changes | Local evidence plus integration or staging validation |
| Production | Releases, migrations, paid actions, permissions, or irreversible writes | Integration evidence, rollback plan, release health, and real consumer-path validation |

Escalate the mode when a change affects data integrity, security, billing, availability, or a shared contract.

## Workflow

1. Define acceptance before editing.
   - Identify the real consumer path: CLI installation, SDK call, UI flow, container startup, worker job, scheduled task, or infrastructure operation.
   - Define success evidence, failure evidence, and the appropriate resume or recovery path for asynchronous work.

2. Inspect local conventions and current state.
   - Check the working tree and preserve unrelated changes.
   - Reuse the repository's test, versioning, package, CI, deployment, and rollback mechanisms.

3. Implement the smallest complete change.
   - Add focused regression coverage for the reported failure or changed contract.
   - Preserve idempotency, confirmation, authorization, validation, and secret-redaction safeguards.

4. Verify in proportion to the selected mode.
   - Run relevant formatting, lint, unit, contract, integration, package, and smoke checks in repository order.
   - Reproduce the critical environment when feasible: clean state, reduced PATH, container, actual launcher, staging service, or consumer integration.

5. Apply the fresh-evidence gate before completion.
   - Run or inspect evidence produced for the current working tree, commit, release, or deployment. Do not claim success from a previous run, an unrelated CI result, or an assumption.
   - Treat missing, stale, failed, or incomplete evidence as an open verification item. Record the gap and use the appropriate non-final completion state.

6. Prepare an authorized release or production operation.
   - Before a migration, destructive operation, or production deployment, identify the rollback method, compatibility window, irreversible effects, and health signals.
   - Commit, push, tag, publish, or deploy only after the user explicitly authorizes each action.
   - Monitor release jobs and investigate failures from their evidence before changing code again.

7. Validate through the real consumer path.
   - Use the released version or deployment route a user will actually consume, not merely a version check.
   - Confirm the resulting artifact, persisted state, or observable behavior. For asynchronous work, verify resume after interruption when it is part of the contract.

8. Close the loop.
   - If real-path validation fails, retain the failure evidence, fix it, and repeat the required verification and release steps.
   - State the completion status and do not claim a release was verified when it was not authorized or could not be tested.

## Reporting

Report the selected mode, change, evidence, released version or deployment when applicable, rollback status for high-risk operations, remaining risk, and one of the defined completion states.

## Specialist Skills

Use an installed, trusted specialist Skill when it matches a workflow stage, but do not make delivery-loop depend on one. Keep this Skill responsible for authorization, risk selection, completion state, and final consumer-path verification.

- Use a release Skill for versioning, changelogs, tags, and package publication.
- Use a platform deployment Skill for target-specific rollout, health checks, and rollback.
- Use a Git Skill for focused staging and commits only after explicit authorization.
- When no specialist Skill applies, follow the repository's documented tools and conventions.

## Common Triggers

- "修复并发新版"
- "提交发布后实际安装验证"
- "CI 通过但用户安装失败"
- "把开发、验证、发布做成闭环"
- "做完这个需求并按真实路径验收"
