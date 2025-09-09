# Issue: Lag when typing a note using Obsidian.nvim

## Description
When using Obsidian.nvim for creating a new note,
there was too much lag when typing, even if all plugins
were disabled.

## Cause
When Obsidian, tried to read the vault from Windows file system,
internally there was a convertion I/O process which added to much
load to reading operations.

## Solution
After moving the vault from Windows file system to
WSL file system, the performace increased considerably.
