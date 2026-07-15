# Android Emulator Runner Retry Smoke Test

Tiny repo for repeatedly running two separate GitHub Actions workflows:

- `Upstream Emulator Runner`: `reactivecircus/android-emulator-runner@v2`
- `Patched Emulator Runner`: patched `android-emulator-runner` with `emulator-startup-retries: 1`

The patched workflow uses https://github.com/ReactiveCircus/android-emulator-runner/pull/481.
It checks out `jpnurmi/android-emulator-runner@feat/emulator-startup-retries`
and runs it as a local action after `npm ci`, because that branch does not
include the committed runtime dependencies present in the upstream `v2` tag.

Each workflow runs every 30 minutes, staggered by 15 minutes, and can also be started manually. Each run fans out across API levels 21, 23, 26, 29, 31, 33 and 35. Keeping the workflows separate makes red upstream runs and patched retry attempts easier to browse in the Actions UI.

## What To Look For

The user script is intentionally just `echo hello`. Any interesting output should come from emulator startup itself:

- upstream workflow: startup failures without built-in retry
- patched workflow: `Emulator startup attempt ...` and retry behavior before `hello` is printed
