# Shared Docker image CI

Keep builds, runtime tests, scanners, supported tags and downstream dispatch in
each image repository. Pin all four shared actions to the same reviewed commit.

## Pipeline

1. Run generation checks and export the complete expected stable-tag list.
2. Use `image-build` on native `ubuntu-24.04` and/or `ubuntu-24.04-arm` jobs.
   Its Docker driver keeps same-job parents available. Pull upstream base images;
   do not force-pull derived parents. `image-parent` checks their IDs and layers.
3. After all tests/scans pass, call `image-stage` with a JSON array of `{tag,id}`
   pairs. It verifies local IDs/platforms, pushes those IDs and records Docker's
   returned digests in current-run/attempt artifacts without remote inspections.
4. A publish job needs all checks and native jobs. Call `image-publish` with the
   complete `tags` and `architectures` JSON arrays. It validates the artifact set
   before login or stable updates, then creates indexes from the tested digests.
5. Downstream dispatch jobs need successful publication.

Staging/publication require `canonical-repository`, `default-branch`, Docker
`repository`, `username` and `password`. Stage also takes `arch`, `images` and a
unique matrix `group`. Publish defaults to amd64+arm64; set `architectures` to
`["amd64"]` for single-platform releases. Node retains its historical
`-x86_64`/`-aarch64` aliases with `architecture-aliases: true`.

## Release rules

Call staging/publication only on the canonical repository's default branch for
non-PR events. The scripts repeat this check before login or publication. Do not
pass release credentials to build/test steps.

The publisher rejects incomplete, duplicate, foreign, stale or malformed records.
It trusts staging's local platform/ID checks and push digests. Single-platform
tags use one-entry indexes; this does not add architecture support. Docker Hub
reads still consume pull allowance.

After failure, **Re-run all jobs**. There is no cross-attempt reuse or resume
workflow. Stable updates are sequential, not atomic; downstream dispatch stays
blocked on failure. Staging tags accumulate until separately cleaned up.

## Tests

```bash
bash image-stage/tests/release.sh
bash image-stage/tests/registry.sh
```

The first checks release/parent contracts with mocked Docker. The second verifies
real loopback-registry pushes, config IDs, platform membership and aliases using
scratch fixtures. Native application execution belongs in consumer CI.
