---
date: 2025-09-17
tags:
  - #macros
hubs:
  - "[[neovim]]"
urls:
  -
---
# 2025-09-17_Using-NeoVim-macros

## Recording a macro
- Start Recording: Press `q` followed by a register name (e.g. `a` for register `a`). A message like "recording @a" will appear in the status bar.
- Perform Actions: Execute the sequence of commands and edits you want to record. This can include movements, insertions, and other Vim commands.
- Stop Recording: Press `q` again

## Playing a macro
- Execute Once: Press `@` followed by the register name (e.g. `@a`).
- Repeat Last Macro: Press `@@` to execute the last played macro.
- Execute Multiple Times: Prepend a number to the execution command (e.g. `5@a` to run macro `a` five times).
