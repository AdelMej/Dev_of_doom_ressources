# Name Refactoring in Vim/Neovim

Renaming variables, functions, or any identifier quickly and safely is key to good code maintenance. Here’s how to do it effectively in Vim/Neovim.

---

## 1. Simple Rename: Using Search and Replace

To rename a variable in the current file:

```vim
:%s/\<old_name\>/new_name/gc
```

% — entire file

\< and \> — word boundaries to avoid partial matches

g — global (all matches on a line)

c — confirmation for each replacement

## 2. Rename in Visual Selection
Select text in visual mode and run a substitute command limited to selection:

Select with v or V.

Run:

```vim
:'<,'>s/\<old_name\>/new_name/gc
```

## 3. Rename Across Multiple Files (Using Vim + Arglist)
Load files into arglist:

```vim
:args **/*.c
```

Use argdo to run substitution on all files:

```vim
:argdo %s/\<old_name\>/new_name/gc | update
```

## 4. Using Plugins for Refactoring

coc.nvim or nvim-lspconfig

If you use LSP (Language Server Protocol), you can leverage rename features:

Place cursor on variable/function.

Run rename command:

```vim
:lua vim.lsp.buf.rename()
```

or

```vim
<C-c> r
(depending on plugin keymaps)
```

## 5. Tips
Always use word boundaries \< and \> to avoid partial renames.

Use c flag to confirm replacements, avoiding mistakes.

Backup files or use version control before bulk renames.

When using LSP, refactoring is safer as it understands scopes.

Refactoring names is much safer and faster with these methods — pick what fits your workflow!