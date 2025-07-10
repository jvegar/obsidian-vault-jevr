#Tmux 
To list all panes in a **tmux** session, you can use the following methods:

  ### 1. List Panes in the Current Session (Inside **tmux**)

  Run this command in a **tmux** pane:
``
```shell
tmux list-panes
```

  or use the shorter alias:

```Shell
tmux lsp
```

  ### 2. List Panes in All Sessions

  To list panes across all **tmux** sessions:

```shell
tmux list-panes -a
```

  ### 3. List Panes with Additional Details

  For more detailed information (including pane IDs, window indexes, and commands
  running in them):

```shell
tmux list-panes -F "#{session_name}:#{window_index}.#{pane_index} -
  #{pane_current_command}"
```

  ### 4. List Panes in a Specific Session

  If you want to list panes from a specific session (replace  session_name  with
  your session's name):

```shell
tmux list-panes -s -t session_name
```

  ### 5. Key Binding (Inside tmux)

  By default, pressing  Prefix  +  q  briefly displays pane numbers in the current
  window.

  ### Example Output

  A typical output may look like:

```shell
0:0.0    [bash] (80x24)
0:0.1    [vim]  (80x24)
```
  •  0:0.0  = Session 0, Window 0, Pane 0
  •  [bash]  = Current process in the pane
  •  (80x24)  = Pane dimensions

  Let me know if you'd like to format the output differently or extract specific
  details!
