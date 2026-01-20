# tmux Cheat Sheet

## Basics
- Start tmux: `tmux`
- Attach session: `tmux attach`
- Detach session: `Ctrl+b d`
- List sessions: `tmux ls`
- Kill session: `tmux kill-session -t <name>`

---

## Prefix
All commands start with:
```
Ctrl + b
```

---

## Windows (Tabs)
- New window: `Ctrl+b c`
- Next window: `Ctrl+b n`
- Previous window: `Ctrl+b p`
- List windows: `Ctrl+b w`
- Rename window: `Ctrl+b ,`
- Close window: `Ctrl+b &`
- Switch to window N: `Ctrl+b 0..9`

---

## Panes (Splits)
- Vertical split: `Ctrl+b %`
- Horizontal split: `Ctrl+b "`
- Switch panes: `Ctrl+b ← → ↑ ↓`
- Close pane: `Ctrl+b x`
- Zoom pane: `Ctrl+b z`
- Swap panes: `Ctrl+b {` / `Ctrl+b }`

---

## Copy Mode
- Enter copy mode: `Ctrl+b [`
- Scroll: arrow keys / PgUp / PgDn
- Start selection: `Space`
- Copy selection: `Enter`
- Paste: `Ctrl+b ]`

---

## Layouts
- Next layout: `Ctrl+b Space`
- Even horizontal: `Ctrl+b M-1`
- Even vertical: `Ctrl+b M-2`
- Tiled: `Ctrl+b M-5`

---

## Sessions
- New session: `tmux new -s name`
- Switch session: `Ctrl+b s`
- Rename session: `Ctrl+b $`
- Kill session: `tmux kill-session -t name`

---

## Useful Commands
- Reload config: `Ctrl+b :source-file ~/.tmux.conf`
- Show shortcuts: `Ctrl+b ?`
- Command prompt: `Ctrl+b :`

---

## Pro Tips
- Use tmux instead of terminal tabs
- One tmux session per project
- Let Alacritty handle fullscreen, tmux handle layout
- Muscle memory > mouse

---

Happy multiplexing 🚀
