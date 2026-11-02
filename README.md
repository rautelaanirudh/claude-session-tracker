# session-tracker

Find and resume Claude Code sessions after a reboot, crash or closed terminal, from any directory.

`claude --resume` only lists sessions started in the current folder. This plugin records each session
when it starts and how it ends. Sessions that never ended cleanly are marked "cut off" so you can resume them.

## Install

```bash
claude plugin marketplace add <path-or-git-url-of-this-repo>
claude plugin install session-tracker@session-tracker-dev
```

For the terminal picker, put `bin/cc-sessions` on your PATH, for example:

```bash
ln -s "$PWD/bin/cc-sessions" ~/.local/bin/cc-sessions
```

## Use

```bash
cc-sessions              # recent sessions from all projects; type a number to resume
cc-sessions stargate     # filter by title, folder or last message
cc-sessions --cut        # only sessions that were cut off
cc-sessions --restore    # reopen every cut-off session in a new iTerm2/Terminal tab
```

Inside Claude Code: `/session-tracker:sessions [filter]`.

Markers: `●` open now, `↯` cut off, `~` probably cut off (from before tracking was installed, guessed from timestamps).

## How it works

- `hooks/hooks.json` runs `bin/cc-session-track` on `SessionStart` and `SessionEnd`.
- It writes `~/.claude/session-tracker/<session-id>.json` with the folder, start time, and the `claude` process ID.
  `SessionEnd` adds the end reason. Records older than 14 days are deleted.
- A session counts as cut off if it has no clean end (`/exit`, `/clear`, `/resume`, logout)
  and its `claude` process is gone or started before the last boot.

macOS only (uses `sysctl kern.boottime`, `/usr/bin/python3`, and AppleScript for `--restore`).
