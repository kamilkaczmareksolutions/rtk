# PR #1718 — supporting screenshots

Branch parking lot for screenshots referenced from
[rtk-ai/rtk#1718](https://github.com/rtk-ai/rtk/pull/1718).

This branch is **not meant to be merged** anywhere. It only exists so the PR
description can embed images via `raw.githubusercontent.com` URLs without
polluting the actual code-change diff.

## Files

- `screenshots/before-rtk-hook-cursor.png` — Cursor `preToolUse` panel
  on `master`: hook command is `rtk hook cursor`, panel collapses to
  `Output: {}` for a `git status` Shell call. The rewrite is silently
  not happening (Cursor runs raw `git status`); the panel makes it look
  like the hook fired correctly.
- `screenshots/before-tracer-confirmed.png` — same symptom reproduced
  through a Python tracer wrapper around `rtk hook cursor`. The wrapper
  proved Cursor was prepending **two UTF-8 BOMs** (`EF BB BF EF BB BF`)
  to stdin, which serde_json refused to parse, so RTK bailed into the
  "no command" branch and returned `{}` for every invocation.
- `screenshots/after-rewrite-visible.png` — same Cursor build, same
  conversation, after rebuilding `rtk` with the patch from PR #1718.
  Panel now shows `continue: true`, `permission: "allow"`,
  `updated_input.command` with `rtk` injected at the right segment of a
  compound command (`... && rtk cargo build --release ...`). Verified
  via `rtk gain --history` that subsequent shell calls hit the rewrite
  path with 30–94% token reduction.
