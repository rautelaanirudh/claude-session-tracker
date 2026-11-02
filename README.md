# session-tracker

**Reopen the Claude Code sessions your reboot killed.**

Restart your laptop with five Claude sessions open across three repos, and they are gone from your terminals.
Claude Code still has the transcripts, and `claude --resume` (press `Ctrl+A` for all projects) lets you find them
one at a time. What it can't tell you is *which* sessions were still open when the machine went down.

session-tracker records every interactive session as it starts and how it ends. Afterwards it can tell you
exactly which sessions were cut off, by a reboot, a crash or a closed terminal, and reopen all of them in
new tabs, each in its own folder.

```
$ cc-sessions --cut
  1 ↯ Fix flaky checkout tests
      ~/code/shop-api  2h ago · 3f2a9c1e-7b4d-4e8a-9c21-5d6e7f8a9b0c
      last: run the integration suite again
  2 ↯ Dark mode for settings page
      ~/code/shop-web  2h ago · 8c1d4e2f-0a3b-4c5d-8e6f-7a8b9c0d1e2f
      last: the PR is ready for review, tighten the description

$ cc-sessions --restore      # reopens both in new iTerm2 tabs
```

## Install

In Claude Code:

```
/plugin marketplace add rautelaanirudh/claude-session-tracker
/plugin install session-tracker@claude-session-tracker
```

Or from a shell:

```bash
claude plugin marketplace add rautelaanirudh/claude-session-tracker
claude plugin install session-tracker@claude-session-tracker
```

Tracking starts with the next session you open. Optional: to use the `cc-sessions` command in your
terminal as well, link it onto your PATH:

```bash
ln -s ~/.claude/plugins/marketplaces/claude-session-tracker/bin/cc-sessions ~/.local/bin/cc-sessions
```

## Use

Inside Claude Code:

| Command | What it does |
|---|---|
| `/session-tracker:sessions [filter]` | Recent sessions from every project, with cut-off ones marked and their resume commands |
| `/session-tracker:restore` | Lists the cut-off sessions and, after you confirm, reopens them all in new tabs |

In a terminal (after the optional link above):

```bash
cc-sessions              # recent sessions from all projects; type a number to resume one
cc-sessions stargate     # filter by title, folder or last message
cc-sessions --cut        # only the sessions that were cut off
cc-sessions --restore    # reopen every cut-off session in a new tab (asks first)
```

Markers: `●` open now · `↯` cut off · `~` probably cut off (from before tracking started, guessed from timestamps).

## Requirements

- **macOS** (iTerm2 or Terminal.app) or **Linux**. `--restore` opens tabs with AppleScript on macOS, or new
  windows when you run it inside **tmux**. Anywhere else it prints the `cd … && claude --resume …` commands.
- `python3` (3.6 or newer) on your PATH. No other dependencies.
- On other platforms the hook does nothing.

## How it works

- A `SessionStart` hook writes `~/.claude/session-tracker/<session-id>.json` with the session's folder, start
  time and the process ID of its `claude` process. A `SessionEnd` hook adds how it ended.
- A session counts as **cut off** when it never ended cleanly (`/exit`, `/clear`, switching with `/resume`,
  logout) and its `claude` process is gone or started before the last boot.
- Headless runs (`claude -p`) are not tracked. Records older than 14 days are deleted.
- Titles and last messages are read from Claude Code's transcripts in `~/.claude/projects`. That format is
  internal to Claude Code, so this part is best effort: if a release changes it, sessions still list and
  resume, but titles may fall back to the first prompt.

## Privacy

Everything stays on your machine. The plugin makes no network requests and sends nothing anywhere. It only
reads Claude Code's local transcripts and writes its own small records under `~/.claude/session-tracker/`.

## License

MIT
