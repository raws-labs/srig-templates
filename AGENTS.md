# srig-templates

CI templates for SiliconRig hardware-in-the-loop jobs; today a single GitLab CI include with extendable hidden jobs. No build step. Public, github.com/raws-labs/srig-templates; consumed as `include: remote: https://raw.githubusercontent.com/raws-labs/srig-templates/main/gitlab-ci.yml`.

## Build, test, run
- No build or tests. A change is exercised only by a GitLab pipeline in another project that includes this file at a branch or commit and has a masked `SRIG_API_KEY` in its CI/CD variables.

## Layout
- `gitlab-ci.yml`: `.siliconrig-install` (`before_script` downloads `srig` to `/usr/local/bin`), `.siliconrig-hil` (extends install; `script` captures serial to `serial-output.txt`, `after_script` ends the session, artifact `when: always`), `.siliconrig-flash` (create session, flash, end).
- Variables: `SRIG_BOARD`, `SRIG_FIRMWARE`, `SRIG_SERIAL_TIMEOUT` (default `30s`); `SRIG_API_KEY` comes from the consumer's CI settings and is read by the CLI from the environment.

## Conventions
- Consumers `extends:` a hidden job and replace `script:`; anything that must always run belongs in `before_script` or `after_script`.
- The include path is `gitlab-ci.yml` at the repo root on `main`; renaming or moving it breaks every consumer.

## Gotchas
- The CLI is fetched as the goreleaser tarball `srig_<version-without-v>_<os>_<arch>.tar.gz` from the `raws-labs/srig-cli` GitHub Releases; no bare-binary asset exists (the old `srig-<os>-<arch>` URL never worked). The download uses `curl -fsSL`, but the tag lookup still uses `curl -fsS` without `-L`: a future repo move would return a 301 JSON body and `jq` would resolve the tag to the string `null`. Use `-fsSL` for both if you touch it.
- The template pins no image; the consumer's job image must provide `curl`, `jq`, and `tar`.

## Open
- `.siliconrig-hil` never runs `srig session create` or `srig flash`, although its header comment and the README say it does; only `.siliconrig-flash` does (verify intent before documenting).
