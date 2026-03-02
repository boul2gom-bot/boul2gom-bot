<h2 align="center">🤖 A GitHub App powering CI automation for boul2gom's repositories</h2>

<div align="center">This account is a GitHub App used across the <a href="https://github.com/boul2gom">boul2gom</a> repositories to keep code formatted, dependencies up to date, releases automated, and stale issues cleaned up automatically.</div>

<br>
<div align="center">⚠️ All commits, pull requests and comments from this account are fully automated. Please do not assign issues or request reviews from this account.</div>
<br>

---

<p align="center">
  <a href="https://crates.io/crates/yt-dlp">
    <img src="https://img.shields.io/crates/v/yt-dlp?label=yt-dlp&logo=Rust" alt="yt-dlp crate"/>
  </a>
  <a href="https://crates.io/crates/media-seek">
    <img src="https://img.shields.io/crates/v/media-seek?label=media-seek&logo=Rust" alt="media-seek crate"/>
  </a>
</p>

## 📂 Repositories

| Repository | Description |
|------------|-------------|
| [boul2gom/yt-dlp](https://github.com/boul2gom/yt-dlp) | Async Rust library wrapping yt-dlp and ffmpeg, with auto dependency installation |
| [boul2gom/ffmpeg-builds](https://github.com/boul2gom/ffmpeg-builds) | Pre-built FFmpeg binaries |

---

## 🔌 Example of use cases

### 📦 Automated releases

Every **Monday at midnight UTC** the bot checks whether unreleased commits exist since the last tag. If the development CI passed and at least one relevant commit is found, it:

1. Runs `cargo-semver-checks` to detect breaking changes and resolve the version bump level (`patch` / `minor`) automatically.
2. Publishes the affected crate(s) — `yt-dlp` and/or `media-seek` — to [crates.io](https://crates.io) via `cargo release`.
3. Creates a **GitHub Release** with auto-generated release notes, categorised into features, bug fixes, refactors and dependency updates.

The version bump level can be overridden manually via `workflow_dispatch` inputs for exceptional cases.

**Workflow:** [`auto-release.yml`](https://github.com/boul2gom/yt-dlp/blob/develop/.github/workflows/auto-release.yml)

### 🎨 Code formatting

On every push to `develop` or `master`, the bot runs `cargo +nightly fmt --all` across the entire workspace and commits the result directly to the branch — so the history stays clean without requiring contributors to run the formatter locally.

Trailing blank lines at the end of every `.rs` file are also stripped in the same pass.

Every formatting commit is **GPG-signed** using the bot's dedicated key and carries the `[skip ci]` tag to avoid triggering a redundant CI run.

**Workflow:** [`ci-fmt.yml`](https://github.com/boul2gom/yt-dlp/blob/develop/.github/workflows/ci-fmt.yml)

### 🧹 Stale issues & pull requests

Every day at midnight UTC the bot scans all open issues and pull requests. Items with no activity for **30 days** are labelled `stale` with a friendly warning comment. If they remain inactive for a further **7 days** they are automatically closed.

**Workflow:** [`stale.yml`](https://github.com/boul2gom/yt-dlp/blob/develop/.github/workflows/stale.yml)

---

## 🔐 Security

The bot authenticates via a **GitHub App** installation token generated at the start of each workflow run (`actions/create-github-app-token`). The token is scoped to the minimum permissions needed for each job and expires after the run completes — no long-lived personal access tokens are used.

All commits produced by the bot are **GPG-signed** with the key below (`5E92A8E6013D4CFF`), stored as an encrypted repository secret. The signing identity is `boul2gom-bot <actions-bot@boul2gom.com>`. The public key is available at [github.com/boul2gom-bot.gpg](https://github.com/boul2gom-bot.gpg).

```
Fingerprint: AE94 D47C DEAE ED7E D986  BB85 5E92 A8E6 013D 4CFF
```

---

## 📄 License

The workflows driving this bot are part of the [boul2gom/yt-dlp](https://github.com/boul2gom/yt-dlp) repository, licensed under the [GPL-3.0 License](https://github.com/boul2gom/yt-dlp/blob/develop/LICENSE.md).
