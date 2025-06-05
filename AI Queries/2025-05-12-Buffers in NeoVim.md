# Buffers in Neovim

Buffers in Neovim (and Vim) are the fundamental containers that hold the text you're editing. They represent open files in memory, but they're more flexible than just files.

## Key Characteristics of Buffers:

1. **File Representation**: Each buffer typically corresponds to a file on disk, but buffers can also be:
   - Empty (new unsaved files)
   - Scratch buffers (temporary workspaces)
   - Special buffers (help files, command outputs