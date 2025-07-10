The `stow` command is a GNU utility used for managing the installation of software packages by creating symbolic links from a central location (usually `/usr/local` or `~/`) to files in a package directory. It helps keep files organized while avoiding conflicts with system package managers (like `apt` or `yum`).

>[!SUMMARY] Table of Contents
>        - [[2025-06-19-How stow command works#How stow Works:|How stow Works:]]
>        - [[2025-06-19-How stow command works#Example: Managing Dotfiles with stow|Example: Managing Dotfiles with stow]]
>        - [[2025-06-19-How stow command works#Common Flags:|Common Flags:]]

### How `stow` Works:
1. **Directory Structure**:  
   - By default, `stow` expects packages to be organized in a specific directory structure. For example:  
     ```
     /usr/local/stow/
     ├── package1/
     │   ├── bin/
     │   ├── lib/
     │   └── share/
     └── package2/
         ├── bin/
         └── etc/
     ```
   - When you "stow" a package (e.g., `package1`), it creates symlinks from `/usr/local/bin`, `/usr/local/lib`, etc., to the files inside `package1/bin`, `package1/lib`, etc.

2. **Basic Commands**:  
   - Install (symlink) a package:  
     ```sh
     stow -v -d /usr/local/stow -t /usr/local package1
     ```
     - `-v`: Verbose mode (shows what `stow` is doing).  
     - `-d`: Directory where packages are stored (`/usr/local/stow`).  
     - `-t`: Target directory where symlinks will be placed (`/usr/local`).  
     - `package1`: The package to install.  

   - Uninstall (remove symlinks):  
     ```sh
     stow -D -v -d /usr/local/stow -t /usr/local package1
     ```
     - `-D`: Delete/remove symlinks.  

3. **Key Features**:  
   - **Conflict Avoidance**: If two packages have files with the same name, `stow` warns you.  
   - **No File Duplication**: Files remain in their original location; only symlinks are created.  
   - **Clean Uninstalls**: Running `stow -D` removes all symlinks cleanly.  

4. **Use Cases**:  
   - Managing locally compiled software (e.g., `./configure --prefix=/usr/local/stow/package1`).  
   - Dotfile management (e.g., symlinking `~/.config` files from a Git repository).  

### Example: Managing Dotfiles with `stow`
1. Organize dotfiles in a directory:  
   ```
   ~/dotfiles/
   ├── zsh/
   │   ├── .zshrc
   │   └── .zshenv
   └── tmux/
       └── .tmux.conf
   ```
2. Symlink them to `~`:  
   ```sh
   stow -v -d ~/dotfiles -t ~ zsh tmux
   ```
   This creates:  
   - `~/.zshrc` → `~/dotfiles/zsh/.zshrc`  
   - `~/.tmux.conf` → `~/dotfiles/tmux/.tmux.conf`  

### Common Flags:
| Flag | Purpose |
|------|---------|
| `-n` | Dry run (simulate without making changes) |
| `--adopt` | Allow overwriting files by renaming them |
| `--ignore=regex` | Skip files matching a pattern |

`stow` is widely used for clean, maintainable software/dotfile management. Would you like a deeper dive into any specific aspect?