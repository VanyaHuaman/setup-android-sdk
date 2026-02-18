# setup-android-sdk

A GitHub Action that downloads and configures the Android SDK on the runner, and automatically sets `ANDROID_HOME`.

## Requirements

- **Java** must be available on the runner prior to using this action. Use `actions/setup-java` (or your internal mirror) in a prior step.

## Usage

```yaml
steps:
  - uses: actions/setup-java@v4
    with:
      distribution: 'temurin'
      java-version: '17'

  - uses: your-org/setup-android-sdk@v1
```

### With custom inputs

```yaml
steps:
  - uses: your-org/setup-android-sdk@v1
    with:
      build-tools-version: '34.0.0'
      install-dir: '/opt/android-sdk'
```

### Referencing the output

```yaml
steps:
  - uses: your-org/setup-android-sdk@v1
    id: android-setup

  - run: echo "SDK installed at ${{ steps.android-setup.outputs.android-home }}"
```

## Inputs

| Input                  | Required | Default        | Description                                                                 |
|------------------------|----------|----------------|-----------------------------------------------------------------------------|
| `build-tools-version`  | **Yes**  |                | Android build-tools version to install (e.g. `34.0.0`)                     |
| `cmdline-tools-version`| No       | `10406996`     | Command-line tools build number used in the Google download URL             |
| `install-dir`          | No       | See below      | Directory to install the SDK. Defaults to `$HOME/android-sdk` on Linux/macOS or `%LOCALAPPDATA%\Android\Sdk` on Windows |
| `accept-licenses`      | No       | `Y`            | Automatically accept Android SDK licenses. `Y` or `N`                      |

## Outputs

| Output         | Description                          |
|----------------|--------------------------------------|
| `android-home` | Absolute path to the installed Android SDK |

## What it installs

- Android command-line tools (sdkmanager)
- `build-tools;<build-tools-version>`
- `platform-tools`
- `extras;google;instantapps`

## Notes

- All components skip installation if already present on the runner
- `ANDROID_HOME` is set as an environment variable available to all subsequent steps
- `platform-tools` is added to `PATH` so `adb` is available directly
- Supports Linux, macOS, and Windows runners
