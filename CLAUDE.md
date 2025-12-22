# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This repository builds precompiled Ruby binaries with ad-hoc code signing for use with [mise](https://mise.jdx.dev/). The binaries use ad-hoc signatures (`codesign -s -`) instead of Developer ID certificates, avoiding Team ID mismatch errors on macOS.

## Architecture

The entire build system is contained in `.github/workflows/release.yml`:

1. **Build Matrix**: Builds Ruby on macOS arm64 (macos-14), Linux x86_64 (ubuntu-22.04), and Linux arm64 (ubuntu-22.04-arm)
2. **Compilation**: Uses [ruby-build](https://github.com/rbenv/ruby-build) with `--enable-load-relative` and `--disable-install-doc`
3. **Code Signing**: Signs all Mach-O binaries (executables, `.bundle`, `.dylib`) with ad-hoc signatures on macOS
4. **Testing**: Validates core gems and YJIT (for Ruby 3.1+)
5. **Distribution**: Creates GitHub Release with tarballs for mise API access

## Building New Versions

Releases are triggered manually via GitHub Actions:

1. Go to Actions → Release Ruby
2. Enter Ruby version (e.g., `3.4.8`)
3. Optionally mark as latest release
4. Run workflow

There are no local build commands - all builds happen in CI.

## Output Artifacts

Published as GitHub Release assets:
- `ruby-X.X.X.macos.tar.gz` - macOS arm64
- `ruby-X.X.X.x86_64_linux.tar.gz` - Linux x86_64
- `ruby-X.X.X.arm64_linux.tar.gz` - Linux arm64
