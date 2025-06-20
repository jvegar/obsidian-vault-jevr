To rename the current session in Tmux, you can use the following command while inside Tmux:

**Keyboard shortcut method:**
1. Press `Ctrl + B` (this is the default prefix key).
2. Then press `$` (Shift + 4).
3. You'll see a prompt at the bottom of the screen. Type your new session name and press Enter.

**Command method (from within Tmux):**
1. Press `Ctrl + B` (prefix key).
2. Type `:rename-session -t current-name new-name` and press Enter.
   - Or simply `:rename-session new-name` to rename the current session.

**From the shell (outside Tmux):**
```bash
tmux rename-session -t current-name new-name
```

**Bonus tip:** You can see all your sessions with `tmux list-sessions` or `Ctrl + B s` from within Tmux.

The session name is displayed in the status bar (bottom left by default), so renaming helps you better identify your sessions when working with multiple Tmux sessions.