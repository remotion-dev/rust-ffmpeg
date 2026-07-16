---
name: update-rust-ffmpeg-sys
description: Update rust-ffmpeg's ffmpeg-sys-next Git revision to a published rust-ffmpeg-sys commit, validate it, and open a draft pull request. Use after publish-ffmpeg-sys-binaries is merged; hand off to Remotion only after this PR is merged to main.
---

# Update Rust FFmpeg Sys

Require the full published `rust-ffmpeg-sys` commit SHA from `$publish-ffmpeg-sys-binaries`.

## Prepare the branch

Run from the `rust-ffmpeg` repository root. Fetch `origin` and start a dedicated `agent/update-rust-ffmpeg-sys` branch from an up-to-date `origin/main`. If that branch or its pull request already exists, inspect it and stop rather than duplicating or rewriting it. Never rebase or force-push.

Allow this skill's own tracked `.agents/skills/update-rust-ffmpeg-sys` files to exist unchanged. Stop on any pre-existing change that is not explicitly part of the requested pull request.

Fetch `https://github.com/remotion-dev/rust-ffmpeg-sys` and verify that the supplied full SHA is reachable from its `main` branch. Do not pin an unpublished or abbreviated commit.

## Update and validate

In `Cargo.toml`, change only `[dependencies.ffmpeg-sys-next].rev` to the full supplied SHA. Preserve the Git URL and `default-features` setting.

Run:

```sh
cargo fmt -- --check
cargo check --examples
git diff --check
```

Confirm that the only tracked diff is the one-line `Cargo.toml` revision change. An ignored local `Cargo.lock` may be produced; do not commit it.

## Commit and open a draft PR

Stage only `Cargo.toml` and any intentional update to this skill. Commit on the review branch with:

```text
Update rust-ffmpeg-sys to <short-sys-sha>
```

Push once with `git push -u origin HEAD`. Never force-push. If the push is rejected, stop and report it.

Open a draft pull request against `main`. Include the full `rust-ffmpeg-sys` SHA, the merged sys PR URL, the one-line dependency change, and validation results in the body.

## Hand off to Remotion

Report the full `rust-ffmpeg-sys` SHA, branch commit, pull request URL, and validation results. Stop until the pull request has been reviewed and merged.

After merge, fetch `origin/main`, identify the full commit on `main` containing this update, and then instruct the next agent:

```text
Open /Users/jonathanburger/Documents/Codex/2026-07-16/this/work/remotion-rust-ffmpeg-update and use
$update-remotion-rust-ffmpeg with rust-ffmpeg commit <full SHA>.
```

Do not edit Remotion before the pull request is merged. Stop after the handoff.
