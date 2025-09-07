# LazyVim Quickstart Tutorial

**LazyVim** is a starter config for Neovim focused on fast setup, good defaults, and modern plugins using [lazy.nvim](https://github.com/folke/lazy.nvim) for plugin management.

---

## 1. Installation

1. **Install Neovim (0.8 or newer)**

```bash
# On Ubuntu/Debian
sudo apt install neovim

# Or on Fedora
sudo dnf install neovim
```

Clone LazyVim config:
```bash
git clone https://github.com/LazyVim/starter ~/.config/nvim
```

Open Neovim
LazyVim will auto-install plugins on first start.

```bash
nvim
```

## 2. Basic usage
Open a file:

```vim
:e path/to/file
```

Save a file:
```vim
:w
```

Quit:

```vim
:q
```

Save and quit:

```vim
:wq
```

Open command palette:
Press \<leader\> + p (usually \ + p or space + p depending on config)

Find files:
Press \<leader\> + f

## 3. Plugin management with lazy.nvim
LazyVim uses lazy.nvim to load plugins on demand.

To add a plugin, add it in ~/.config/nvim/lua/plugins/init.lua and restart Neovim.

Example adding vim-commentary:

```lua
{
  'tpope/vim-commentary',
  lazy = false,
}
```

## 4. Useful default keymaps

Action	Keybinding
Save file	\<leader\>w
Quit	\<leader\>q
Toggle file explorer	\<leader\>e
Format code	\<leader\>f
Toggle comments	gcc (normal mode)
Search text	/

\<leader\> key is usually set to space or backslash (\).

## 5. Customization
Config files live in ~/.config/nvim/lua/

You can create your own Lua files and require them in init.lua

Modify keymaps, settings, plugins here.

## 6. Updating plugins

Open Neovim and run:
```vim
:LvimUpdate
```

or from the terminal:
```bash
nvim --headless -c 'Lazy sync' -c 'qa'
```

## 7. Troubleshooting

If plugins don’t install, run inside Neovim:
```vim
:Lazy sync
```

Check :messages for errors.