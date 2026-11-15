# Desktop stream protocol 13 — 0.2.4 build 42

Verified November 14, 2026 (America/New_York). Release tag:
`v0.2.4-preview.42`. Installed Codex: build 27194.

## Failure and repair

The previous panel repeatedly logged `unsupported_version_resync` with
`protocolVersion: 13`. IPC initialization and quota reads succeeded, but the
stream gate accepted only version 11. Live conversations were invalidated and
the panel displayed `Connected · Limited` without detecting running tasks.

Inspection of the installed desktop bundle confirmed version 13 and the same
snapshot/revision/patch envelope. `acceptedTextChanges` is outside the retained
status metadata. The adapter now accepts verified versions 11 and 13. Owner,
local host, account scope, required status fields, and revision continuity
checks remain in effect. Unverified protocol versions still invalidate streams.

This release is built from build 40 plus this compatibility fix. Local build 41
Touch Bar and approval-wait changes are excluded from both source and binary.

## Validation

- `scripts/test-core.sh --no-parallel -j 1`: **167 tests passed**. The first
  parallel clean build encountered a Clang module-definition conflict between
  system SQLite and bundled SQLite in the test target. Serial compilation and
  execution passed without changing source or disabling validation.
- New tests cover v13 running → waiting → completed transitions, silent
  baselines, revision-gap recovery, unknown runtime-state rejection, and
  rejection of unverified protocol versions 12, 14 and 99.
- Streaming-decoder coverage verifies that text-only changes are discarded
  while revisions advance and subsequent completion remains observable.
- Universal Release packaging passed for **arm64 and x86_64**, with bundle
  version **0.2.4 / 42** and minimum macOS **15.0**.
- ZIP integrity and extracted-app strict ad-hoc signature verification passed.
  Every extracted file matches the packaged app; bundled legal files match
  source. The actual extracted release app connected to Codex build 27194,
  detected **two processing tasks** and **four covered live conversations**,
  and reported live tasks, usage, history recovery and agent list available.
- Startup remained silent. The local development application was restored
  after the release smoke check.

Artifact: `StatusBoard-0.2.4-build42-macOS-universal.zip`.

SHA-256: `78488749e2ac9e869ae144041d2d2ffcfdcd9e999c6f5210f0f91e2a964c8d10`.

Generated evidence stays local in the isolated release worktree under
`.build/release-validation/` (`core-tests-serial.log`, `package.log`, and
aggregate-only `live.jsonl`). No live account IDs, titles or conversation bodies
are included in the release sources or attachments.

## Limits

The application remains an ad-hoc-signed preview without Developer ID signing
or notarization. Runtime verification was on Intel; universal compilation does
not establish full Apple Silicon runtime acceptance. This fixes the verified
v13 contract, not future unverified protocols or account-wide task coverage.
