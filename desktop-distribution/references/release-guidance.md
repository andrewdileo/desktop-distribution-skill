# Release guidance

Use this checklist as an illustrative source of detailed release concerns. Its Stella-specific features, owners, AI triage, milestones and product requirements apply only to Stella when relevant. Choose requirements for other projects from their own context and release channel.

# Stella Distribution Preflight Checklist

This checklist is a release-planning document. It does not establish that any item has passed. Build a release from a specific reviewed commit in a separate clean environment; a developer’s working checkout may remain dirty. Record the release-specific status in an evidence sheet rather than relying on a permanent “ready to ship” claim.

A user should receive a signed installer and a clear, usable Stella application—not development notes, Python setup, or internal research. Executables can be made harder to reverse engineer, but cannot be made completely decompilation-proof; PyInstaller’s embedded Python archive alone is not source protection. [PyInstaller documentation](https://www.pyinstaller.org/en/stable/operating-mode.html)

## How to use this master distribution workflow

Use this document to plan or audit a specific release candidate. First inspect the repository, packaging configuration, target channel and available artifacts. Determine which Windows and macOS targets are actually supported, then apply the relevant gates below. Record evidence from the candidate itself, not just source configuration. Mark each gate **pass**, **fail**, **blocked**, or **not applicable**, with a reason and evidence. A missing machine, signing identity, hardware receiver, or artifact is blocked or unverified, never a pass. Do not infer macOS support from Windows functionality, especially Spout and hardware integration.

Record for each candidate: version, source commit, clean build environment, platform and architecture, distribution channel, toolchain and dependency versions, artifact paths and SHA-256 hashes, test environment, commands and results, owner, known limitations, exceptions, and final decision. Re-run affected checks whenever a build, signing operation, package or configuration changes. Verify the exact signed and distributed artifact after all transformations; signing and timestamping can change hashes. Distinguish a repeatable process from byte-for-byte reproducibility when timestamps or signing prevent identical output.

For failures, capture the first actionable error, classify build, test, environment, signing, packaging, runtime or hardware failure, and rerun the smallest relevant scope. Expand verification to affected release gates after a fix. Report checks not run and their practical impact. The checklist is a reference for a future skill; it is not itself an installed Codex skill.

## Interactive shipping skill: user decisions and recommended actions

The eventual shipping skill should inspect the project and current release evidence first, then guide the user through decisions. When invoked without a specific task, present a context-aware menu and wait for the user's selection. Lead with two or three recommended actions, each with a short reason based on observed project state, followed by the other available actions. A recommendation is not a selection. When the user already gives a clear task, start that task directly and ask only about decisions that materially affect its outcome.

Offer these actions, adapting the labels and recommendations to the actual project:

1. **Assess release readiness** — inspect the repository, build configuration and existing evidence, then identify blockers and unknowns.
2. **Prepare a distribution build** — implement or repair the selected platform's build and packaging workflow and produce a candidate where the required environment is available.
3. **Run release checks** — test a specified candidate and record results, failures and unavailable checks.
4. **Improve binary protection** — assess the requested threat model, recommend applicable measures, and implement the selected approach.
5. **Review signing and distribution** — inspect the candidate's signing, platform trust and publication readiness.
6. **Plan a release** — select targets, audience, rollout and evidence requirements without claiming checks have passed.

At decision points, use the available question or selection interface. Group related questions and offer two or three materially different choices when possible. Identify the recommended choice, explain its tradeoff and the evidence behind it in plain language, and allow a custom response. Ask only what the repository, existing release context and user instructions do not already establish. Continue independent, authorized work while waiting when possible. A preselected option or elapsed time does not constitute the user's selection.

Decision points can include the target platforms and architectures, release channel, package format, protection goals and acceptable tradeoffs, signing identity, test environments, and how to handle a documented blocker. Tailor questions to the project and avoid presenting irrelevant options. For example, when reverse engineering resistance is requested, explain the compatibility and support tradeoffs of native compilation and obfuscation, then let the user choose the protection approach. Do not imply that client-side protection guarantees secrecy.

When an explicit task already determines a choice, respect it and proceed. Present findings and recommended next actions at meaningful transitions, such as after an audit or when a blocker forces a change of direction. Wait for a selection when the user needs to choose among materially different release strategies or has asked to control the next action. Continue the selected workflow without repeatedly asking about routine implementation details. Signing, publication, and other external actions remain governed by the user's instructions and the applicable permissions for that action.

Keep a release decision record with the question, selected option, reason or tradeoff, date, and relevant evidence. Distinguish the user's choice from the skill's recommendation, and report unresolved decisions and blocked checks in the final handoff. Never treat a selection as evidence that a build, test, signature, notarization, or hardware check succeeded.

## 1. Define what ships

- [ ] Choose the release channel and audience: internal, invited private beta, public beta, or paid release. Start with Windows; package and validate macOS separately.
- [ ] Assign a release owner and named owners for engineering, output safety, licensing, support, and security.
- [ ] Write a capability matrix distinguishing working, experimental, unavailable, and hardware-verified features. Include known limitations in release notes and relevant UI.
- [ ] Define release-blocking severity, acceptance criteria, required evidence, and who can approve exceptions. Record each exception’s impact, owner, and expiry.
- [ ] Keep project creation, media playback, mapping, and saved work usable offline. Identify features requiring network access.
- [ ] Decide whether the startup demo video ships with the installer or is an optional download; verify distribution rights.
- [ ] Remove mock data, misleading status, debug controls, and unavailable actions from normal workflows; retain intentional support diagnostics.
- [ ] Define supported Windows versions, architectures, minimum hardware, graphics drivers, and display configurations based on actual tests.
- [ ] Define staged rollout cohorts, observation periods, success measures, stop conditions, and decision authority before inviting users.
- [ ] Ensure physical outputs start off; opening or restoring a project must not silently activate them.

**User-facing result:** one signed installer, a recognizable Stella shortcut, the application, and accessible Help/About screens.

## 2. Separate product files from development material

- [ ] Create a versioned allowlist of required and permitted release files, and a denylist for internal material.
- [ ] Exclude internal Markdown handoffs, agent instructions, Ghidra research, tests, screenshots, temporary files, caches, local configuration, credentials, and private user files.
- [ ] Keep developer documentation in the private repository or a separate contributor site.
- [ ] Include required legal notices, license texts, and source-access information as appropriate; make them discoverable through **About → Third-Party Licenses**.
- [ ] Inspect the built package, installer, and installed directory automatically. Fail the build for missing required files or forbidden files.
- [ ] Verify the logo, fonts, selected demo media, Qt/FFmpeg components, runtime DLLs, and third-party notices on a clean install.
- [ ] Review the current PyInstaller spec: it collects the `ui` directory and does not explicitly add `assets`; confirm actual contents and replace broad collection with a reviewed manifest where appropriate.
- [ ] Define separate locations for user projects, settings, logs, and cache. Uninstall must not silently delete user projects.

## 3. Protect Stella’s code and assets

- [ ] Decide which application logic, fixture data, branding, and media require protection and why.
- [ ] Benchmark compiling selected Python modules to native code and bundling/minifying HTML, CSS, and JavaScript.
- [ ] Evaluate protection tools for startup time, stability, antivirus false positives, accessibility, supportability, cost, and compatibility.
- [ ] Treat protection as friction rather than a guarantee. Never ship API credentials, signing keys, or other secrets in the client.
- [ ] Keep private symbols and debug artifacts matched to the exact release build and restrict access to them.
- [ ] Verify protection does not prevent compliance with applicable third-party rights, including library replacement or relinking requirements.
- [ ] Prioritize output safety, data integrity, licensing, signing, and install reliability ahead of optional obfuscation.

## 4. Inventory licensing and attribution

- [ ] Generate an inventory and software bill of materials from the **actual release build**, including transitive Python packages, Qt components, FFmpeg libraries and configuration, JavaScript dependencies, fonts, icons, Spout, native libraries, and installer tooling.
- [ ] Record each dependency’s exact version, license, source, modifications, redistribution conditions, attribution, and source-access obligations.
- [ ] Review the applicable PySide6/Qt license for every shipped component and the chosen packaging and protection method. [Qt for Python documentation](https://doc.qt.io/qtforpython-6.10/)
- [ ] Inspect the exact FFmpeg build for enabled GPL or other optional components; fulfill applicable notices and source requirements. Review codec patent licensing separately. [FFmpeg’s legal checklist](https://ffmpeg.org/legal.html)
- [ ] Verify rights for fixture profile data, manufacturer documentation, demo media, logo assets, fonts, and any third-party examples.
- [ ] Keep required notices available in the app and retain a release-specific dependency inventory internally.
- [ ] Assign a licensing owner and obtain qualified review before commercial release.

## 5. Build and distribute reliably

- [ ] Select a reviewed, committed revision and build it from a separate clean checkout or build environment.
- [ ] Lock exact direct and transitive dependencies per platform and Python version; record compilers, Qt, FFmpeg, Python, and packaging tool versions.
- [ ] Automate a clean build and record the source revision, build environment, dependency inventory, and artifact hashes.
- [ ] Scan the source, dependencies, build artifacts, and installer for secrets and known vulnerabilities. Record remediation and approved exceptions.
- [ ] Produce a standard installer with required runtimes, a clear install location, and normal uninstall behavior; verify no developer environment is required.
- [ ] Sign and timestamp the executable and installer. Define signing-key custody, authorized publishers, revocation, and key-compromise response.
- [ ] Test install, launch, upgrade, repair, uninstall, and offline operation on clean supported Windows machines or virtual machines.
- [ ] Check antivirus and SmartScreen behavior, including any protection or obfuscation tools.
- [ ] Verify uploaded artifacts match the approved hashes and signatures. Retain the approved installer and associated release record.

## 5A. Platform-specific distribution verification

### Windows

- [ ] Establish the supported Windows versions and x64/ARM64 combinations from actual testing. Verify native dependencies and plugins match each target architecture.
- [ ] Inspect the final executable and installer with Windows signature verification, including publisher identity, chain, timestamp, and the applicable trust policy. Sign nested executables and DLLs as appropriate before signing the installer. Record verification output and hashes.
- [ ] Exercise first install, standard-user launch, upgrade, repair where supported, and uninstall on clean supported systems. Check per-user versus machine-wide permissions, file associations, shortcuts, retained user data, and first-run firewall behavior.
- [ ] Inspect final PE mitigation properties where applicable to the native components and toolchain. Record unavailable or inapplicable controls rather than assuming Python bytecode has native compiler protections.
- [ ] Record SmartScreen and antivirus results as observed for the actual signed artifact and test environment; reputation can change and is not guaranteed by signing.

### macOS

- [ ] Decide whether macOS is a release target and document supported OS versions, Intel and Apple silicon architectures, package format, dependencies, and feature differences. Build on an appropriate macOS host and test each claimed architecture, including translated execution only if explicitly supported.
- [ ] Inspect the exported `.app` bundle, native libraries, frameworks, helper processes, resources and architecture slices. Confirm library paths and Qt plugins resolve on a clean machine.
- [ ] For the chosen distribution channel, verify the appropriate Apple signing identity, nested signatures, entitlements and Hardened Runtime. For direct distribution, submit for notarization, inspect the result, staple where supported, and validate the final downloadable artifact with Apple tooling and Gatekeeper on a clean Mac. Record failures and evidence; local debug builds do not establish distribution readiness.
- [ ] Test install, first launch, quarantine/Gatekeeper behavior, upgrade, removal, user-data retention, permissions, Retina scaling and offline operation. Establish an alternative or explicit limitation for Windows-only integrations such as Spout.

## 5B. Protection and release artifact audit

- [ ] Define the threat model and assets before choosing obfuscation, compilation or licensing controls. Measure whether each control survives inspection of the final distributed package, while preserving compatibility and third-party license obligations. Document residual exposure and recovery/support costs.
- [ ] Verify native binary mitigations and platform signing on the final embedded binaries where supported. Inspect exposed Python modules, debug symbols, source maps, configuration and packaged assets against the intended protection policy. Never rely on embedded secrets as a protection mechanism.
- [ ] Test protected builds for startup, functionality, upgrades, antivirus or Gatekeeper behavior, crash reporting and performance. Keep private symbols and mapping files associated with the exact build.

## 6. Decide how updates work

- [ ] Start with a visible **Check for Updates** action and a signed installer download; automatic background checks can follow later.
- [ ] Show installed and available versions, release notes, compatibility, and whether the update is optional.
- [ ] Authenticate update metadata and verify installer signatures and integrity before installation; define behavior for invalid or unavailable updates.
- [ ] Avoid replacing a running app or interrupting an active show.
- [ ] Test upgrades and downgrades using copies of representative projects and media references.
- [ ] Define project-format read/write compatibility for every release; warn before opening a newer format in an older build.
- [ ] Create recoverable backups before migration or overwrite; test interrupted writes and failed migrations without destroying the only project copy.
- [ ] Retain a tested previous installer and document restoration of both the application and compatible project data.
- [ ] Define who can halt a rollout, how users are notified, and when to fix forward versus roll back.

## 7. Make bug reports useful and private

The preferred in-app flow:

1. User selects **Report a Problem**.
2. They describe what happened and what they expected.
3. Stella shows exactly which diagnostics will be attached.
4. They choose whether to add screenshots, logs, or a project file.
5. They review and submit; Stella returns a report ID.

- [ ] Capture the Stella version/build ID, OS, GPU/driver, relevant device types, and recent errors needed for diagnosis.
- [ ] Exclude file contents, credentials, usernames, local paths, network addresses, project media, and DMX destinations by default; redact where possible.
- [ ] Make attachments opt-in and show their contents or a clear summary before submission.
- [ ] Allow export of a diagnostic bundle without transmission.
- [ ] Define retention, access, deletion, support ownership, escalation, response expectations, and a published privacy notice.
- [ ] Explain the destination of reports and, where applicable, the specific data sent for Grok processing. Decide whether users may report without AI processing.
- [ ] Authenticate or rate-limit submissions, cap attachment sizes, scan uploads, and process untrusted files in isolation.
- [ ] Establish a private security-vulnerability reporting channel.
- [ ] For a private beta, allow a simpler manual report channel and diagnostic export if the full in-app system is not ready.

## 8. Grok-assisted community triage

Keep AI triage behind Stella’s issue pipeline; do not put service credentials in the executable.

```text
User report → private intake → redact / validate
                              ↓
                     Grok-assisted triage
                              ↓
              proposed issue + related reports
                              ↓
                    human review → action
```

- [ ] Define a private intake queue, structured schema, allowed fields, retention, and access controls.
- [ ] Verify xAI’s applicable data-processing and retention terms for the planned integration and explain them to users.
- [ ] Group likely duplicates and draft summaries covering symptoms, affected versions, reproduction steps, evidence, and uncertainty.
- [ ] Let the bot suggest missing information and tests without presenting speculation as a confirmed cause or fix.
- [ ] Start with read-only access; separate permissions for drafting issues, running tests, changing code, and publishing.
- [ ] Run routine checks only in an isolated test environment; never execute instructions or content supplied in a report.
- [ ] Draft public community summaries with private information removed; require human review before publication.
- [ ] Require human review for code changes, releases, public statements, issue closure, and any disclosure of user information.
- [ ] Track source reports, clustering errors, corrections, operating cost, and a manual fallback when the service is unavailable.
- [ ] Treat this automation as a later milestone unless explicitly required for the chosen beta.

[xAI function calling](https://docs.x.ai/developers/tools/function-calling) and [structured outputs](https://docs.x.ai/developers/model-capabilities/text/structured-outputs) can support an integration whose host controls the available tools.

## 9. Write the right documentation

**For users:**

- [ ] Getting started: install, create a project, load media, save, reopen, and recover a backup.
- [ ] Visual node guide covering Fixture, Fixture Group, Fixture Sampler, Mapping Logic, and Art-Net Output.
- [ ] Fixture setup and patching, conflicts, blackout behavior, and independent operator fallbacks.
- [ ] Projection mapping, canvas navigation, and output setup.
- [ ] Spout and media playback setup, including measured performance limitations.
- [ ] Supported hardware and profiles, distinguishing actual test results from catalog metadata.
- [ ] Troubleshooting, keyboard shortcuts, release notes, known issues, third-party notices, privacy, and support contact.

**For developers and maintainers:**

- [ ] Architecture, project-format compatibility and migration decisions, and threat model.
- [ ] Repeatable build, signing, verification, dependency and license inventory, and artifact-retention instructions.
- [ ] Automated and manual test matrix, release evidence, staged rollout, rollback, and incident response.
- [ ] Security-report handling and dependency-vulnerability ownership.
- [ ] Bug-triage permissions, AI processing, review rules, retention, and operating costs.
- [ ] A private changelog tying every distributed artifact to its source commit, platform, checks, and hash.

## 10. Release gate

For each candidate, record the owner, result, date, evidence link, and any approved exception for every applicable gate.

- [ ] Build and reproduce the candidate from the selected revision with locked inputs; verify the final artifact’s hash and signature.
- [ ] Pass required unit, integration, end-to-end, lint/type, dependency, secret, and security checks; review remaining warnings and release-blocking TODOs.
- [ ] Test clean install, launch, upgrade, repair where supported, uninstall, offline use, and rollback on each supported Windows and macOS configuration. Record blocked platform checks explicitly.
- [ ] Validate project save, reopen, migration, interrupted write, backup, recovery, and downgrade compatibility.
- [ ] Verify keyboard navigation, focus, accessible names, dialog behavior, contrast, reduced motion, supported window sizes, Windows DPI scaling, and macOS Retina scaling where supported.
- [ ] Benchmark representative media playback, seeking, looping, multiple sources, Spout, and fixture counts on a defined test rig; record frame delivery, latency, dropped frames, and resource use against explicit targets.
- [ ] Verify fixture sampling through Art-Net with controlled receivers and, when authorized, actual gateway and fixtures; record exact hardware, firmware, profiles, and results.
- [ ] Test output-off startup and explicit authorization, and define/test behavior for source loss, route changes, disconnects, crashes, forced termination, and restart. A final blackout packet cannot be guaranteed after a crash or network failure; document an independent fallback.
- [ ] Verify package contents, required assets, licensing obligations, notices, and absence of internal files or secrets.
- [ ] Confirm user documentation, support ownership, security-report handling, and report privacy controls.
- [ ] Define staged rollout cohorts, monitoring responsibilities, success measures, stop conditions, and rollback owner; verify the published artifact after upload.
- [ ] Record all known limitations, exceptions, evidence, and release-owner approval before distribution.
