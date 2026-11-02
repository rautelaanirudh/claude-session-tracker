# Privacy policy: session-tracker

_Last updated: 2 November 2026_

session-tracker is a Claude Code plugin that runs entirely on your own computer.

## What it reads

- Claude Code's local session transcripts in `~/.claude/projects/`. It reads them only to show each session's
  title, folder and your last message in the session list. These transcripts can contain anything you typed into
  Claude Code, including personal data.

## What it stores

- One small record per interactive session in `~/.claude/session-tracker/<session-id>.json`, containing the session
  ID, the folder it ran in, the ID of its `claude` process, when it started and ended, and how it ended.
- These records contain no conversation content. They are deleted automatically after 14 days, and you can delete
  the folder at any time.

## What it sends

- Nothing. The plugin makes no network requests and sends no data to the author, to Anthropic, or to any other
  service. There are no analytics or telemetry.

## Contact

Questions: open an issue at https://github.com/rautelaanirudh/claude-session-tracker/issues
