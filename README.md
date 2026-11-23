# DSH Unofficial Desktop Builds

**Unofficial builds. Not affiliated with, sponsored by, or endorsed by DeepSeek.**

This repository publishes desktop installers for **DSH (DeepSeek Harness)**, compiled directly from the upstream open-source repository at a pinned tag or commit, using the upstream packaging scripts.

- Upstream source: <https://github.com/deepseek-ai/deepseek-harness>
- Upstream license: **MIT** — see [LICENSE](./LICENSE) and [THIRD_PARTY_NOTICES.md](./THIRD_PARTY_NOTICES.md)
- Packaging: upstream `package:desktop:*:unsigned` scripts (no official signing keys or branding assets are used)

## Download

Open the [Releases](../../releases) page and pick the asset for your platform:

| Platform | Asset pattern | Notes |
|---|---|---|
| Windows x64 | `DSH-<version>-win-x64.exe` | Unsigned; SmartScreen may warn |
| macOS Apple Silicon | `DSH-<version>-mac-arm64.dmg` | Unsigned; see macOS note below |
| macOS Intel | `DSH-<version>-mac-x64.dmg` | Unsigned; see macOS note below |

Every release also contains:

- `SHA256SUMS.txt`
- `LICENSE`
- `THIRD_PARTY_NOTICES.md`

## Verify a download

```bash
sha256sum -c SHA256SUMS.txt
```

On macOS:

```bash
shasum -a 256 -c SHA256SUMS.txt
```

### macOS unsigned build

These builds are not signed with an Apple Developer ID and are not notarized. Gatekeeper will block a normal double-click. Either:

- Right-click the app → **Open** → **Open** again, or
- Remove the quarantine attribute after copying it to `/Applications`:

```bash
xattr -dr com.apple.quarantine "/Applications/<app name>.app"
```

Only do this for a build whose SHA-256 you have verified.

## Versioning

Release tags follow the upstream tag plus an unofficial suffix:

```
dsh-v0.2.3-alpha.2-unofficial.1
```

The release body records the exact upstream commit SHA used for the build. No source changes are made beyond packaging; if a patch is ever required, the diff and the corresponding source will be published in the same release.

## How builds are produced

Builds run in GitHub Actions from this repository's workflow. Each job:

1. Checks out `deepseek-ai/deepseek-harness` at the requested tag/commit.
2. Installs the pinned toolchain (`pnpm`, Node.js).
3. Runs `pnpm install --frozen-lockfile`.
4. Runs `pnpm run build`.
5. Packages the desktop app with the upstream unsigned script for the target platform.
6. Uploads the installers as workflow artifacts.

A separate release job publishes the artifacts only when the `publish` input is set to `true`.

## Security notes

- No API keys, tokens, or account credentials are bundled.
- The app connects to the model endpoint you configure yourself.
- Builds are produced from public upstream source in public CI logs.
- Verify checksums before running any installer.

## License and trademark

The upstream code is licensed under the MIT License, Copyright (c) 2026 DeepSeek. The full license text is included in this repository and in every release.

“DeepSeek Harness” is a trademark of DeepSeek. This project uses the abbreviation **DSH** for its name and does not use official logos or imply official endorsement, in line with the upstream brand guidelines. Descriptive references such as “built from DeepSeek Harness source” are used only to identify the upstream project.

## Issues

Please report build/packaging problems in this repository's Issues. Report upstream application bugs to the upstream project:

<https://github.com/deepseek-ai/deepseek-harness/issues>
