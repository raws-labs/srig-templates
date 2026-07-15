# srig-templates

CI/CD templates for [siliconrig](https://siliconrig.dev) hardware-in-the-loop testing.

## GitLab CI

Include the template in your `.gitlab-ci.yml`:

```yaml
include:
  - remote: 'https://raw.githubusercontent.com/raws-labs/srig-templates/main/gitlab-ci.yml'

hil-test:
  extends: .siliconrig-hil
  variables:
    SRIG_BOARD: esp32-s3
    SRIG_FIRMWARE: build/firmware.bin
  script:
    - pytest tests/hil/ -v
```

### Available templates

| Template | Description |
|---|---|
| `.siliconrig-install` | Installs `srig-cli` binary |
| `.siliconrig-hil` | Full HIL workflow — session, flash, serial capture, cleanup |
| `.siliconrig-flash` | Flash-only workflow — session, flash, cleanup |

### Variables

| Variable | Description |
|---|---|
| `SRIG_API_KEY` | API key (set in CI/CD settings, masked) |
| `SRIG_BOARD` | Board type (e.g., `esp32-s3`) |
| `SRIG_FIRMWARE` | Path to firmware binary |
| `SRIG_SERIAL_TIMEOUT` | Serial capture duration (default: `30s`) |
