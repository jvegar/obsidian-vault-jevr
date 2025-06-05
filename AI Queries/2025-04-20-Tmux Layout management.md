#Tmux #layout_management
Tmux (Terminal Multiplexer) manages its layout through a hierarchical structure of sessions, windows, and panes, combined with a powerful tiling system. Here's a detailed breakdown:

### 1. **Hierarchy**
   - **Sessions**: The outermost container (a collection of windows).
   - **Windows**: Tabs within a session (each window occupies the entire terminal).
   - **Panes**: Split regions within a window (like tiles in a grid).

### 2. **Layout Management**
   - Panes are arranged in a **tree structure**, where each split (vertical/horizontal) creates a new branch.
   - Tmux uses **layout algorithms** to dynamically resize panes when the terminal dimensions change.

### 3. **Predefined Layouts**
   Tmux offers several built-in layouts (toggle with `prefix` + `space`):
   - **Even-horizontal**: Panes spread horizontally.
   - **Even-vertical**: Panes stacked vertically.
   - **Main-horizontal**: Large top pane + smaller panes below.
   - **Main-vertical**: Large left pane + smaller panes on the right.
   - **Tiled**: Balanced grid-like arrangement.

### 4. **Custom Layouts**
   - Panes can be manually split (`prefix` + `%` for vertical, `prefix` + `"` for horizontal).
   - Resized interactively (`prefix` + `Ctrl-arrow`).
   - Swapped or moved (`prefix` + `{` or `}`).

### 5. **Layout Persistence**
   - Tmux saves the state of sessions/windows/panes, allowing detach/re-attach without losing layout.
   - Scriptable via commands (`tmux split-window`, `tmux select-layout`).

### 6. **Under the Hood**
   - Each pane runs a separate pseudo-terminal (PTY).
   - The layout is stored as a textual representation (e.g., `dac5,238x61,0,0[238x30,0,0,0,238x30,0,31,1]`).

### Example Workflow:
1. Create a window with 3 panes:
   ```sh
   tmux new-window -n "MyWindow"
   tmux split-window -h
   tmux split-window -v
   ```
2. Cycle layouts:
   - Press `prefix` + `space` to toggle between layouts.

### Key Commands:
- `prefix` + `%`: Split vertically.
- `prefix` + `"`: Split horizontally.
- `prefix` + `arrow`: Navigate panes.
- `tmux list-windows`: Show current layouts.

Tmux's layout system is designed for keyboard-driven efficiency, enabling complex terminal workflows without a GUI. Would you like a deeper dive into a specific aspect?