# AGENTS.md

This repository builds the Bork browser runtime from Firefox. It is a large,
security-sensitive upstream-derived codebase. Keep changes narrow, preserve
Firefox invariants, and treat upstream synchronization as a first-class
constraint.

## Agent roles and routing

Role instructions under `agents/` complement this guide. They do not replace
it. Choose the smallest suitable role and read its document before beginning
work:

- [Project Lead](agents/project-lead.md) coordinates work that genuinely spans
  multiple roles or repositories.
- [Software Architect](agents/software-architect.md) owns runtime boundaries,
  integration contracts, technical design coherence, and architecture notes.
- [Research Architect](agents/research-architect.md) owns exploratory
  research, upstream comparisons, validation plans, and research handoffs.
- [UI Specialist](agents/ui-specialist.md) owns browser chrome visual design,
  interaction, accessibility, usability, and cross-surface consistency.

## Task discipline

- Do only the requested task. Do not add adjacent cleanup or reformatting.
- Preserve unrelated changes in this checkout. Stage and commit only files
  owned by the current task.
- Do not run, launch, quit, kill, or stop the browser unless explicitly asked.
- Do not infer a person's mood or intent from their wording.
- Obtain project-owner approval before significant architectural, behavioural,
  security, visual-identity, or release changes.
- Follow existing Firefox and local Bork patterns. Prefer the smallest safe
  change that resolves the stated issue.

## Runtime and feature-layer boundary

`bork-build` owns the Firefox-derived runtime: Gecko and Firefox integration,
browser chrome integration points, runtime configuration, branding, build
configuration, packaging, signing, and upstream synchronization.

The companion Bork feature layer owns product features such as tab stacks and
their product-specific UI, state, and user workflows. Do not duplicate that
feature logic in the runtime. When a feature needs runtime support, define a
small, versioned integration contract and keep the browser-facing behaviour in
the feature layer. Document any new cross-repository contract with its owner,
callers, lifecycle, compatibility requirements, and validation plan.

Changes crossing this boundary require project-owner approval. Do not make a
runtime fork the default home for a feature merely because it is convenient to
patch Firefox code.

## Upstream synchronization

This source tree is periodically refreshed from Firefox. Before editing,
inspect the local customization layout, recent history, and the upstream-sync
workflow. Keep Bork-owned changes isolated from upstream code whenever the
repository's existing layout permits it. Do not move broad upstream code or
make style-only edits that enlarge future merge conflicts.

Read `README.md` and relevant files under `.github/` before modifying the
sync process or custom-source layout. Never overwrite user changes or assume a
clean worktree.

## Firefox tooling and search

Firefox is too large for unbounded local search. For C++, Rust, and Java code,
use `searchfox-cli` to find definitions and identifier references. Use `rg`
only when restricted to a known relevant path, or when looking for local
uncommitted changes that Searchfox cannot contain. Do not search the whole tree
blindly.

Do not use identifier search for front-end JavaScript names, CSS classes, or
HTML custom-element names. `searchfox-cli --path` accepts one path expression;
combine paths and file extensions with globs when needed, and add `--regexp`
for regular expressions. If the relevant code is not found quickly, ask rather
than expanding into an unbounded search.

Examples:

```sh
searchfox-cli --define 'AudioContext::AudioContext'
searchfox-cli --id AudioSink -l 150 --cpp
searchfox-cli --path 'browser/**/*.sys.mjs' -q 'SearchTerm'
```

`./mach` is the main build and developer interface. Use `./mach help` and
`./mach <command> --help` before unfamiliar operations. Common commands:

- `./mach format` formats modified files.
- `./mach build` builds the runtime; it can take many minutes.
- `./mach test --auto` selects tests automatically.
- `./mach run` launches the browser when explicitly authorized.
- `./mach doc --no-serve --no-open` builds documentation.
- `./mach python --virtualenv <name>` runs Python in the virtual environment
  registered by the relevant Mach command.

Use the available Mozilla MCP resources for Bugzilla bugs and Phabricator
revisions when the task names one. For review-comment work, read the revision
and its comments first; inspect `jj log -T builtin_log_detailed` or
`git log -v -l 10` to locate the review identifier when needed. `treeherder-cli`
is available to inspect results for an approved Try push.

Ask before running a full build, broad test suite, browser launch, Try push,
or any other expensive operation. When a slow command is approved, redirect
its complete output to a file under `artifacts/`; do not pipe it through
`head`, `tail`, or `grep`.

For front-end-only changes, do not build C++ or Rust unnecessarily. For
C++/Rust-only changes, use the repository's narrow binary build path when
appropriate. `./mach build faster` is useful for approved Android/Desktop
front-end changes that need a build, while `./mach build binaries` avoids
unneeded front-end work for C++/Rust-only changes. Run `./mach format` and
focused validation for files you changed before handing work off.

## Browser engineering and security

Browser work is security-sensitive. Treat origin isolation, process boundaries,
privileges, permissions, CSP, sandboxing, navigation, URL parsing, download
handling, storage, private browsing, telemetry, crash reporting, update,
signing, and profile migration as high-risk areas. Do not weaken an invariant
or add privileged content access without an explicit threat model and approval.

Keep front-end/browser-chrome, content-process, parent-process, and platform
code responsibilities explicit. Prefer established Firefox IPC, lifecycle,
preference, localization, telemetry, and accessibility patterns over new
cross-process shortcuts.

## Code and review conventions

- Keep comments to non-obvious behaviour; preserve existing comments unless
  the logic they describe changes.
- Do not use emoji in source or documentation.
- Follow local formatting, linting, localization, and accessibility conventions.
- Do not submit patches to Phabricator, push a Try run, publish an artifact, or
  make a release without explicit approval.
- After approved local testing, use `mach try auto` only with explicit approval
  to schedule CI. Group reviewers use `r?#group-name`; individual reviewers do
  not use the `#` prefix.
- Do not include `DONTBUILD` or `CLOSED TREE` in a `mach try` message for a run
  intended to schedule builds.

## Completion

Report the affected ownership boundary, the focused validation performed, and
any deliberately unrun expensive checks. Call out compatibility, security, or
upstream-sync risk rather than silently expanding scope to resolve it.
