---
name: windows-distribution
description: Plan, build, audit, and verify Windows desktop app releases, including installers, signing, release checks and binary protection.
---

# Windows Distribution

## Choose the work

When invoked without a specific task, inspect the project and offer a context-aware menu. Recommend two or three next actions with reasons grounded in evidence, then wait for the user to choose. Include readiness assessment, release build, release checks, binary protection, signing and distribution review, and release planning. Allow a custom choice. If the task is clear, start directly.

At consequential decisions, present two or three feasible choices when practical, recommend one with its tradeoffs, and accept a custom answer. Ask only for decisions not already established by the project or user. Continue independent authorized work while waiting. A default selection or elapsed time is not an answer. Record material decisions and outstanding questions. Avoid asking about routine implementation details.

## Shared release process

Inspect the actual stack, source revision, target architecture, release channel, build dependencies, packaging configuration and candidate artifacts. Execute the selected task and fix issues within its authorized scope. Record commands, test environment, results, source revision, final artifact hashes, limitations and evidence. Mark applicable gates pass, fail, blocked or not applicable with reasons. Missing hardware, credentials, artifact or platform is blocked or unverified. Recheck affected gates after changes and verify the final signed and packaged artifact. Distinguish repeatable builds from byte-identical builds when signatures or timestamps vary.

Define the protection threat model and compare applicable compilation, obfuscation and integrity approaches for compatibility, licensing, performance and support. Let the user decide material tradeoffs. Inspect and test the protected artifact, retain matching private debug information and describe remaining exposure. Never embed credentials in the client or promise that client-side protection guarantees secrecy.

Review dependency inventory, licenses, notices, secret and vulnerability scans, update authenticity, data migration, backup and rollback as applicable. Test clean installation, first launch, core workflows, offline use, upgrade, recovery and removal on supported configurations where available. Keep user data safe. Report unperformed checks and blockers. A successful build alone does not establish release readiness. Publishing and other external actions require the user's authorization.

## Windows verification

Identify supported Windows versions, architectures, package format and channel. Inspect final installer and installed files, required runtimes, native DLLs, plugins and architecture compatibility. Verify Authenticode signatures, certificate chain, publisher and timestamp under the appropriate trust policy for shipped executables and installer. Inspect PE mitigations for applicable native components, identifying controls that do not apply to the chosen stack. Test clean first install, standard-user launch, shortcuts, file associations, upgrade, repair where supported, uninstall and retained data. Record SmartScreen, firewall and antivirus observations for the actual artifact and environment. Validate DPI scaling and supported graphics or hardware paths. Use current Microsoft documentation for platform and channel requirements.
