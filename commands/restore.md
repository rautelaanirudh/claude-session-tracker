---
description: Reopen every Claude Code session that a reboot, crash or closed terminal cut off, each in a new terminal tab
allowed-tools: Bash(${CLAUDE_PLUGIN_ROOT}/bin/cc-sessions --cut --list)
---

Sessions that never exited cleanly:

!`${CLAUDE_PLUGIN_ROOT}/bin/cc-sessions --cut --list`

If the output says there are no cut-off sessions, tell the user and stop.

Otherwise list them briefly (title and folder), then ask the user whether to reopen all of them in new terminal tabs. Only after they say yes, run exactly this (Claude Code will ask the user to approve it, since it opens terminal tabs):

```bash
"${CLAUDE_PLUGIN_ROOT}/bin/cc-sessions" --restore --yes
```

Report what it printed. It opens tabs in iTerm2 or Terminal.app on macOS, or windows inside tmux. If it says it can't open tabs here, show the user the `cd … && claude --resume …` commands it printed so they can run them.
