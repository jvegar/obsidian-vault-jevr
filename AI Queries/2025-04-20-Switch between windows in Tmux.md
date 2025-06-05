#Tmux #switch_windows
In Tmux, you can switch between windows using various key combinations. Here are the most common methods:

### Basic Window Navigation
1. **Next Window**: `Ctrl-b n` (press Ctrl+b, release, then press n)
2. **Previous Window**: `Ctrl-b p` (press Ctrl+b, release, then press p)
3. **Switch to Window by Number**: `Ctrl-b <number>` (e.g., `Ctrl-b 0` for window 0, `Ctrl-b 1` for window 1, etc.)
4. **Last Used Window**: `Ctrl-b l` (lowercase L, for "last")

### Advanced Navigation
5. **List All Windows**: `Ctrl-b w` (shows interactive list to select from)
6. **Find Window by Name**: `Ctrl-b f` (then type part of the window name)
7. **Move to Window by Index**: `Ctrl-b '` (then type the window number and press Enter)

### Renaming Windows (Helpful for Navigation)
- Rename current window: `Ctrl-b ,` (then type new name and press Enter)

### Additional Tips
- You can customize these key bindings in your `~/.tmux.conf` file if needed.
- Windows are numbered starting from 0 by default, but you can renumber them with `Ctrl-b .` (though this is less common).

Note: All commands start with the Tmux prefix key (`Ctrl-b` by default), followed by the specific key for the action.