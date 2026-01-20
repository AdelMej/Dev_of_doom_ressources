# Alacritty Themes (YAML)

## How to use
Add one of these themes to your `alacritty.yml` under the `colors:` section.

Config path (Arch):
- `~/.config/alacritty/alacritty.yml`

---

## Theme: Tokyo Night (Dark)

```yaml
colors:
  primary:
    background: '0x1a1b26'
    foreground: '0xc0caf5'

  normal:
    black:   '0x15161e'
    red:     '0xf7768e'
    green:   '0x9ece6a'
    yellow:  '0xe0af68'
    blue:    '0x7aa2f7'
    magenta: '0xbb9af7'
    cyan:    '0x7dcfff'
    white:   '0xa9b1d6'

  bright:
    black:   '0x414868'
    red:     '0xf7768e'
    green:   '0x9ece6a'
    yellow:  '0xe0af68'
    blue:    '0x7aa2f7'
    magenta: '0xbb9af7'
    cyan:    '0x7dcfff'
    white:   '0xc0caf5'
```

---

## Theme: Gruvbox Dark

```yaml
colors:
  primary:
    background: '0x282828'
    foreground: '0xebdbb2'

  normal:
    black:   '0x282828'
    red:     '0xcc241d'
    green:   '0x98971a'
    yellow:  '0xd79921'
    blue:    '0x458588'
    magenta: '0xb16286'
    cyan:    '0x689d6a'
    white:   '0xa89984'

  bright:
    black:   '0x928374'
    red:     '0xfb4934'
    green:   '0xb8bb26'
    yellow:  '0xfabd2f'
    blue:    '0x83a598'
    magenta: '0xd3869b'
    cyan:    '0x8ec07c'
    white:   '0xebdbb2'
```

---

## Theme: Catppuccin Mocha

```yaml
colors:
  primary:
    background: '0x1e1e2e'
    foreground: '0xcdd6f4'

  normal:
    black:   '0x45475a'
    red:     '0xf38ba8'
    green:   '0xa6e3a1'
    yellow:  '0xf9e2af'
    blue:    '0x89b4fa'
    magenta: '0xf5c2e7'
    cyan:    '0x94e2d5'
    white:   '0xbac2de'

  bright:
    black:   '0x585b70'
    red:     '0xf38ba8'
    green:   '0xa6e3a1'
    yellow:  '0xf9e2af'
    blue:    '0x89b4fa'
    magenta: '0xf5c2e7'
    cyan:    '0x94e2d5'
    white:   '0xcdd6f4'
```

---

## Pro tip
You can split themes into files and import them:

```yaml
import:
  - ~/.config/alacritty/themes/tokyonight.yml
```

Peak modular terminal engineering 😎
