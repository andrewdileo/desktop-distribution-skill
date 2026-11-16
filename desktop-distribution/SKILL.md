---
name: desktop-distribution
description: Plan, build, audit, and verify Windows and macOS desktop application releases, including packaging, signing, distribution checks, and binary protection. Use when preparing or evaluating a desktop release or its shipped artifacts.
---

# Desktop distribution

Inspect the project, release configuration, target platforms, available artifacts and existing evidence before advising. Respect the existing stack and the user’s specified scope. Use [release guidance](references/release-guidance.md) for detailed checks. Apply product-specific examples only when relevant.

## Choose the work

When invoked without a specific task, offer a context-aware menu and wait for the user to select. Recommend two or three next actions with brief reasons grounded in observed evidence. Include assess readiness, prepare a build, run release checks, improve binary protection, review signing and distribution, and plan a release. Allow a custom selection. When a task is already clear, begin it directly.

At consequential decisions, present two or three feasible choices when practical, identify a recommendation and explain its tradeoffs. Use an available question interface and accept custom answers. Ask about unknown platform targets, release channel, package format, protection objectives, signing or test environment only when relevant and unresolved. Continue independent authorized work while waiting. Never treat an unsubmitted default or elapsed time as a decision. Record the user's choices and outstanding decisions in the release evidence or handoff. Avoid repeated questions about routine implementation details.

## Execute and verify

Identify source revision, actual stack, architecture, build dependencies, existing release pipeline, target audience and candidate artifacts. Adapt the checks to the selected action and platform. Run suitable checks and fix issues within the authorized scope. Verify the final transformed, signed and packaged artifact, not only settings or an earlier build. Record commands, environment, outcomes, artifact hashes, platform, revision, limitations and supporting evidence. Mark each relevant gate pass, fail, blocked or not applicable with its reason. Missing credentials, hardware, platform or artifacts leave the relevant check blocked or unverified.

For Windows inspect package contents, target architectures, signing and timestamp, executable mitigations where applicable, and installation and upgrade behavior. For macOS inspect the bundle, nested binaries, architecture slices, signing, entitlements and hardened runtime, plus notarization, stapling and Gatekeeper where required for the chosen channel. Test on supported clean systems when available. Verify user data and rollback behavior. Protect signing credentials and do not embed secrets in distributed clients.

For protection work, define what needs protection and the threat model. Compare applicable compilation, obfuscation and integrity options against compatibility, licensing, performance and supportability. Have the user select among material tradeoffs. Inspect and test the final protected build and explain residual exposure; no client-side control guarantees secrecy.

Classify failures and rerun the smallest useful scope, followed by affected release checks. Report exact blockers and unperformed checks. A successful build or user selection alone does not establish release readiness. Keep distribution, publication and external mutations within the user's authorization.
