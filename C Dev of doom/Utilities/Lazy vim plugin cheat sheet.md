# LazyVim Plugin Cheat Sheet

## nvim-surround
- **Add surround:** `ysiw)` → `(word)`  
- **Delete surround:** `ds"` → removes quotes  
- **Change surround:** `cs'"` → `'word'` → `"word"`  

## nvim-autopairs
- Auto-closes `()`, `{}`, `[]`, `"`, `'`  
- Type opening bracket/quote → closing is inserted automatically  

## Comment.nvim
- **Comment line:** `gcc`  
- **Comment motion:** `gc{motion}`  
- **Block comment:** `gb{motion}`  
- **Visual mode:** select lines + `gc`  

## telescope-fzf-native.nvim
- **Find files:** `:Telescope find_files`  
- **Search text:** `:Telescope live_grep`  
- **Open buffers:** `:Telescope buffers`  
- **Search help:** `:Telescope help_tags`  

## symbols-outline.nvim
- **Toggle sidebar:** `:SymbolsOutline`  
- **Navigate:** Enter → jump to symbol  
- **Close sidebar:** `:q`  

## nvim-treesitter-context
- Shows current function/class at the top of the window automatically  
- No extra commands needed  

---

💡 **Tips**
- Focus first on `nvim-surround`, `nvim-autopairs`, and `Comment.nvim` to speed up coding.  
- Use `Telescope` to quickly navigate projects.  
- Keep `symbols-outline` and `treesitter-context` open in large files.  
