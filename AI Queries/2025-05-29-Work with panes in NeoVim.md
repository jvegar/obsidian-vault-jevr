#neovim #panes
Working with panes (splits) in Neovim is a powerful way to manage multiple files or views within a single window. Here's a comprehensive guide:

### **Basic Pane Management**
1. **Split Horizontally**  
   `:split` or `Ctrl-w s` - Split the current window horizontally.

2. **Split Vertically**  
   `:vsplit` or `Ctrl-w v` - Split the current window vertically.

3. **Close Current Pane**  
   `:q` or `Ctrl-w q` - Close the current pane.

4. **Close All Other Panes**  
   `:only` or `Ctrl-w o` - Keep only the current pane.

---

### **Navigating Between Panes**
1. **Move to Pane Left/Right/Up/Down**  
   `Ctrl-w h` (left)  
   `Ctrl-w j` (down)  
   `Ctrl-w k` (up)  
   `Ctrl-w l` (right)

2. **Cycle Through Panes**  
   `Ctrl-w w` - Move to the next pane.  
   `Ctrl-w W` - Move to the previous pane.

---

### **Resizing Panes**
1. **Increase/Decrease Height**  
   `Ctrl-w +` (increase)  
   `Ctrl-w -` (decrease)

2. **Increase/Decrease Width**  
   `Ctrl-w >` (increase)  
   `Ctrl-w <` (decrease)

3. **Equalize Pane Sizes**  
   `Ctrl-w =` - Make all panes equal in size.

---

### **Moving Panes**
1. **Move Current Pane to New Tab**  
   `Ctrl-w T` - Move the current pane to a new tab.

2. **Swap Panes**  
   `Ctrl-w r` - Rotate panes clockwise.  
   `Ctrl-w R` - Rotate panes counter-clockwise.

---

### **Advanced Pane Operations**
1. **Open a File in a New Pane**  
   `:vsplit filename` (vertical)  
   `:split filename` (horizontal)

2. **Move Buffer to Another Pane**  
   `Ctrl-w H` (move left)  
   `Ctrl-w J` (move down)  
   `Ctrl-w K` (move up)  
   `Ctrl-w L` (move right)

3. **Toggle Between Last Used Panes**  
   `Ctrl-w p` - Switch between the current and last pane.

---

### **Custom Key Mappings (Optional)**
To make pane navigation easier, you can add these to your `init.lua` (Neovim) or `.vimrc` (Vim):
```lua
-- Easier pane navigation (Neovim)
vim.keymap.set('n', '<C-h>', '<C-w>h')
vim.keymap.set('n', '<C-j>', '<C-w>j')
vim.keymap.set('n', '<C-k>', '<C-w>k')
vim.keymap.set('n', '<C-l>', '<C-w>l')
```

```vim
" Easier pane navigation (Vim)
nnoremap <C-h> <C-w>h
nnoremap <C-j> <C-w>j
nnoremap <C-k> <C-w>k
nnoremap <C-l> <C-w>l
```

---

### **Plugins for Enhanced Pane Management**
- **tmux.nvim** (Neovim + Tmux integration)  
- **vim-tmux-navigator** (Seamless pane switching between Vim and Tmux)  
- **vim-windowswap** (Swap window positions easily)  

Would you like help with any specific pane-related workflow?