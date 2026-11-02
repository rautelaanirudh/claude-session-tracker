---
description: List recent Claude Code sessions from every project, marking the ones a reboot or crash cut off
argument-hint: "[filter words]"
allowed-tools: Bash(python3:*)
---

Sessions across every project, newest first (● open now, ↯ cut off by a reboot/crash/closed terminal, ~ probably cut off):

!`python3 "${CLAUDE_PLUGIN_ROOT}/bin/cc-sessions" --list -n 15 $ARGUMENTS`

Each session is printed as: number, marker (blank when none), title; then folder, age and session id (after the `·`); then the last message.

Show this to the user as a compact table: marker, title, folder, when, session id. Copy each marker exactly as printed and leave it blank when the output has none; never infer or add a marker yourself. Then:

- For each ↯ session, give the command that resumes it from any terminal: `cd <folder> && claude --resume <session id>`.
- If there are ↯ sessions, mention that `/session-tracker:restore` reopens all of them in new terminal tabs.
- If the output says no sessions matched, say so and suggest a broader filter.

Don't run any other commands.
