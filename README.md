# ruby-prebuilt

Precompiled Ruby binaries for [mise](https://mise.jdx.dev/) with ad-hoc code signing for macOS.

## Why?

Default precompiled Ruby binaries from [jdx/ruby](https://github.com/jdx/ruby) are signed with a
Developer ID certificate. On macOS with strict code signature requirements, this can cause Team ID
mismatch errors requiring post-install re-signing.

This repository provides Ruby binaries with **ad-hoc code signing** (`codesign -s -`) which works
without additional re-signing on any Mac.

## Supported Platforms

| Platform | File | Runner |
|----------|------|--------|
| macOS arm64 (Apple Silicon) | `ruby-X.X.X.macos.tar.gz` | `macos-14` |
| Linux x86_64 | `ruby-X.X.X.x86_64_linux.tar.gz` | `ubuntu-22.04` |
| Linux arm64 | `ruby-X.X.X.arm64_linux.tar.gz` | `ubuntu-22.04-arm` |

## Usage with mise

Configure mise to use precompiled binaries from this repository:

```toml
[settings]
experimental = true

[settings.ruby]
precompiled_url = "alexey1312/ruby-prebuilt"
```

For CI environments, set the environment variable:

```yaml
env:
  MISE_GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

## Building New Versions

1. Go to [Actions → Release Ruby](../../actions/workflows/release.yml)
2. Click "Run workflow"
3. Enter the Ruby version (e.g., `3.4.8`)
4. Click "Run workflow"

The workflow will:
- Build Ruby on all platforms using `ruby-build`
- Apply ad-hoc code signing on macOS
- Create GitHub Release with tarballs

## How It Works

1. Uses [ruby-build](https://github.com/rbenv/ruby-build) to compile Ruby from source
2. Configures Ruby with `--enable-load-relative` for portable installation
3. Signs all Mach-O binaries (executables, `.bundle`, `.dylib`) with ad-hoc signature on macOS
4. Packages into tarballs and uploads to GitHub Releases

## License

Ruby is licensed under the [Ruby License](https://www.ruby-lang.org/en/about/license.txt).
Build scripts in this repository are MIT licensed.
