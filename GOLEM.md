# golemfactory/iroh: branch `golem-release`

Builds of iroh for golem-p2p until an upstream release has the same fixes.

* Base: branch `relay-recovery` (upstream PR n0-computer/iroh#4686): upstream
  `main` plus bounded relay actor waits and recovery of stalled relay tasks
  (`RELAY_RECOVERY.md`). Keep that branch free of fork-only changes; this
  branch adds only CI and release workflows on top.
* Upstream's workflows are removed here: they need n0's runners and secrets.
  `golem-ci.yml` runs fmt, clippy and the `iroh` / `iroh-relay` tests on
  every push; `golem-release.yml` builds `iroh-relay` for Linux x86_64 and
  aarch64 (glibc 2.35+ and static musl).
* Release: tag `golem-v<iroh version>-<n>` (e.g. `golem-v1.5.0-1`, never
  `v*`, which is upstream's pattern) and push the tag. The workflow publishes
  a GitHub pre-release with the archives and `SHA256SUMS`.
* Updating: rebase `relay-recovery` onto new upstream `main`, then rebase this
  branch onto it, and tag the next `-<n>`.

golem-p2p uses these crates through `[patch.crates-io]` with a git `rev`.
