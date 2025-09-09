# Issue: Lag when typing a note using Obsidian.nvim

## Description
When using Obsidian.nvim for creating a new note,
there was too much lag when typing, even if all plugins
were disabled.

## Cause
When NeoVim tries to read the vault from Windows file system,
it has to go trough a translation layer to access the Windows NT file system.
Thi I/O overhead on every keystroke can turn a tiny delay into a noticeable lag.

## Solution
After moving the vault from Windows file system to the
WSL file system, the performace increased considerably, 
as a result the lag disappeared.
