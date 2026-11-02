---
description: List recent and cut-off Claude Code sessions across all projects, with resume commands
argument-hint: "[filter words]"
allowed-tools: Bash(/usr/bin/python3:*)
---

Sessions across every project (● open now, ↯ cut off by a reboot/crash/closed terminal, ~ probably cut off):

!`/usr/bin/python3 "${CLAUDE_PLUGIN_ROOT}/bin/cc-sessions" --list -n 15 $ARGUMENTS`

Show this list to the user as a short table: marker, title, folder, when. For each cut-off (↯) session, give the exact command to resume it in a terminal: `cd <folder> && claude --resume <id>`. Mention that `cc-sessions --restore` in a terminal reopens all cut-off sessions in new tabs. Do not run anything else.
