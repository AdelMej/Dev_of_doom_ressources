# Alacritty Customization Cheat Sheet

This guide focuses on **practical, day-to-day Alacritty customization** for developers.
Alacritty is intentionally minimal: no tabs, no UI — pair it with **tmux** for maximum power.

---

## 📁 Config file location

```bash
~/.config/alacritty/alacritty.yml
```

Create it if it does not exist.

---

## 🖥️ Window behavior

### Start in fullscreen

```yaml
window:
  startup_mode: Fullscreen
```

Other options:
- `Windowed`
- `Maximized`
- `Fullscreen`

---

### Padding (inner spacing)

```yaml
window:
  padding:
    x: 6
    y: 6
```

---

### Dynamic title

```yaml
window:
  dynamic_title: true
```

Lets tmux / shell update the window title.

---

## 🔤 Font configuration

### Set font family & size

```yaml
font:
  normal:
    family: JetBrains Mono
    style: Regular
  bold:
    family: JetBrains Mono
    style: Bold
  italic:
    family: JetBrains Mono
    style: Italic
  size: 11.0
```

Recommended fonts:
- JetBrains Mono
- FiraCode
- Cascadia Code
- Source Code Pro

---

### Enable ligatures (font-dependent)

Ligatures are enabled automatically if the font supports them.
(No config needed.)

---

## 🎨 Colors & theme

### Import a theme file

```yaml
import:
  - ~/.config/alacritty/themes/gruvbox_dark.yml
```

Themes are just YAML files.
You can keep multiple and swap instantly.

---

### Cursor style

```yaml
cursor:
  style:
    shape: Block    # Block | Underline | Beam
    blinking: On
```

---

## ⌨️ Key bindings

### Example: spawn new tmux window

```yaml
key_bindings:
  - { key: T, mods: Control|Shift, command: { program: "tmux", args: ["new-window"] } }
```

⚠️ Most navigation should live in **tmux**, not Alacritty.

---

## 🧠 Shell integration

### Default shell

```yaml
shell:
  program: /bin/zsh
```

---

### Auto-start tmux (recommended)

Put this in `~/.bashrc` or `~/.zshrc`:

```bash
if [[ -z "$TMUX" ]]; then
  tmux attach || tmux new
fi
```

Alacritty → tmux instantly.

---

## 🖱️ Mouse & scrolling

```yaml
mouse:
  hide_when_typing: true

scrolling:
  history: 10000
  multiplier: 3
```

Scrollback works even with tmux.

---

## 🚀 Performance tips

- Alacritty is GPU accelerated (OpenGL)
- No tabs = less overhead
- Let tmux handle multiplexing
- Avoid heavy terminal themes

---

## 🔥 Recommended combo (battle-tested)

- **Alacritty** → fullscreen, fast, clean
- **tmux** → tabs, panes, sessions
- **Neovim / VS Code** inside tmux

This setup scales from laptop → server → SSH → WireGuard without changes.

---

## 📌 Debugging

Reload config without restart:

```bash
alacritty --print-events
```

Check config errors:

```bash
alacritty --config-file ~/.config/alacritty/alacritty.yml
```

---

Happy hacking 🚀
