# extended.md

## Оглавление

1. [Sway: полная настройка](#1-sway-полная-настройка)
2. [Терминал: подробно](#2-терминал-подробно)
3. [Разработка: подробно](#3-разработка-подробно)
4. [Мультимедиа: подробно](#4-мультимедиа-подробно)
5. [Диски и EFI](#5-диски-и-efi)
6. [Утилиты pacman](#6-утилиты-pacman)

---

## 1. Sway: полная настройка

Базовая установка Sway описана в `basic.md`. Здесь — полный конфиг со всеми функциями.

### 1.1. Установка компонентов

```bash
sudo pacman -S sway waybar wofi mako swaybg swayidle swaylock grim slurp wl-clipboard wl-clip-persist brightnessctl playerctl udiskie polkit lxqt-policykit pavucontrol network-manager-applet blueman bluez bluez-utils
```

> **Note:** В соответствии с вашими требованиями, описывается только базовый пакет `sway`. Waybar, Wofi, Mako и прочие компоненты устанавливаются для полноценной работы среды, но их детальная настройка вынесена в соответствующие подразделы ниже.

### 1.2. Полный конфиг Sway

Файл `~/.config/sway/config`:

```text
# ============================================
# Переменные
# ============================================
set $mod Mod4
set $left h
set $down j
set $up k
set $right l
set $term alacritty
set $menu wofi --show drun

# ============================================
# Шрифты
# ============================================
font pango:JetBrains Mono Nerd Font 10

# ============================================
# Обои
# ============================================
exec swaybg -i /usr/share/backgrounds/cachyos.png

# ============================================
# Автозапуск
# ============================================
exec waybar
exec mako
exec --no-startup-id blueman-applet
exec --no-startup-id nm-applet --indicator
exec wl-clip-persist --clipboard regular
exec udiskie --tray
exec /usr/lib/polkit-gnome/polkit-gnome-authentication-agent-1
exec lxqt-policykit-agent

# ============================================
# Раскладка клавиатуры
# ============================================
input * {
    xkb_layout "us,ru"
    xkb_options "grp:alt_shift_toggle"
}

# ============================================
# Тачпад
# ============================================
input type:touchpad {
    tap enabled
    natural_scroll enabled
    dwt enabled
    disable-while-typing enabled
}

# ============================================
# Курсор
# ============================================
seat seat0 xcursor_theme Bibata-Modern-Ice 24

# ============================================
# Мониторы и HiDPI
# ============================================
# Используйте swaymsg -t get_outputs для получения имён мониторов
output * scale 1
# output eDP-1 resolution 1920x1080 position 0,0 scale 1.5
# output HDMI-A-1 resolution 1920x1080 position 1920,0

# ============================================
# Gaps
# ============================================
gaps inner 10
gaps outer 5

# ============================================
# Borders
# ============================================
default_border pixel 2
default_floating_border pixel 2

# ============================================
# Плавающие окна
# ============================================
floating_modifier $mod normal

# ============================================
# Фокус
# ============================================
focus_follows_mouse yes
focus_on_window_activation smart

# ============================================
# Привязки клавиш: основное
# ============================================
bindsym $mod+Return exec $term
bindsym $mod+d exec $menu
bindsym $mod+Shift+q kill
bindsym $mod+f fullscreen

# ============================================
# Навигация (vim-like + стрелки)
# ============================================
bindsym $mod+$left focus left
bindsym $mod+$down focus down
bindsym $mod+$up focus up
bindsym $mod+$right focus right
bindsym $mod+Left focus left
bindsym $mod+Down focus down
bindsym $mod+Up focus up
bindsym $mod+Right focus right

# ============================================
# Перемещение окон
# ============================================
bindsym $mod+Shift+$left move left
bindsym $mod+Shift+$down move down
bindsym $mod+Shift+$up move up
bindsym $mod+Shift+$right move right
bindsym $mod+Shift+Left move left
bindsym $mod+Shift+Down move down
bindsym $mod+Shift+Up move up
bindsym $mod+Shift+Right move right

# ============================================
# Layouts
# ============================================
bindsym $mod+s layout stacking
bindsym $mod+w layout tabbed
bindsym $mod+e layout toggle split
bindsym $mod+space floating toggle

# ============================================
# Workspaces
# ============================================
bindsym $mod+1 workspace number 1
bindsym $mod+2 workspace number 2
bindsym $mod+3 workspace number 3
bindsym $mod+4 workspace number 4
bindsym $mod+5 workspace number 5
bindsym $mod+6 workspace number 6
bindsym $mod+7 workspace number 7
bindsym $mod+8 workspace number 8
bindsym $mod+9 workspace number 9
bindsym $mod+0 workspace number 10

bindsym $mod+Shift+1 move container to workspace number 1
bindsym $mod+Shift+2 move container to workspace number 2
bindsym $mod+Shift+3 move container to workspace number 3
bindsym $mod+Shift+4 move container to workspace number 4
bindsym $mod+Shift+5 move container to workspace number 5
bindsym $mod+Shift+6 move container to workspace number 6
bindsym $mod+Shift+7 move container to workspace number 7
bindsym $mod+Shift+8 move container to workspace number 8
bindsym $mod+Shift+9 move container to workspace number 9
bindsym $mod+Shift+0 move container to workspace number 10

# ============================================
# Window rules
# ============================================
for_window [app_id="firefox"] move to workspace number 2
for_window [class="Steam"] floating enable
for_window [window_role="pop-up"] floating enable
for_window [window_role="bubble"] floating enable
for_window [window_role="dialog"] floating enable
for_window [window_type="dialog"] floating enable
for_window [app_id="pavucontrol"] floating enable
for_window [app_id="blueman-manager"] floating enable
for_window [app_id="nm-connection-editor"] floating enable

# ============================================
# Scratchpad
# ============================================
bindsym $mod+minus scratchpad show
bindsym $mod+Shift+minus move scratchpad

# ============================================
# Resize mode
# ============================================
mode "resize" {
    bindsym $left resize shrink width 10px
    bindsym $down resize grow height 10px
    bindsym $up resize shrink height 10px
    bindsym $right resize grow width 10px
    bindsym Left resize shrink width 10px
    bindsym Down resize grow height 10px
    bindsym Up resize shrink height 10px
    bindsym Right resize grow width 10px
    bindsym Return mode "default"
    bindsym Escape mode "default"
}
bindsym $mod+r mode "resize"

# ============================================
# Звук (wpctl)
# ============================================
bindsym XF86AudioRaiseVolume exec wpctl set-volume @DEFAULT_AUDIO_SINK@ 5%+
bindsym XF86AudioLowerVolume exec wpctl set-volume @DEFAULT_AUDIO_SINK@ 5%-
bindsym XF86AudioMute exec wpctl set-mute @DEFAULT_AUDIO_SINK@ toggle
bindsym XF86AudioMicMute exec wpctl set-mute @DEFAULT_AUDIO_SOURCE@ toggle
bindsym XF86AudioPlay exec playerctl play-pause
bindsym XF86AudioNext exec playerctl next
bindsym XF86AudioPrev exec playerctl previous

# ============================================
# Яркость (brightnessctl)
# ============================================
bindsym XF86MonBrightnessUp exec brightnessctl set +5%
bindsym XF86MonBrightnessDown exec brightnessctl set 5%-

# ============================================
# Скриншоты (grim + slurp)
# ============================================
bindsym Print exec grim ~/Pictures/screenshot-$(date +%Y%m%d-%H%M%S).png
bindsym $mod+Print exec grim -g "$(slurp)" ~/Pictures/screenshot-$(date +%Y%m%d-%H%M%S).png
bindsym $mod+Shift+Print exec grim -g "$(slurp)" - | wl-copy

# ============================================
# Блокировка экрана
# ============================================
bindsym $mod+Escape exec swaylock -f
```

### 1.3. Waybar: полный конфиг

Файл `~/.config/waybar/config`:

```json
{
    "layer": "top",
    "position": "top",
    "height": 30,
    "spacing": 4,
    "modules-left": ["sway/workspaces", "sway/mode"],
    "modules-center": ["clock"],
    "modules-right": ["pulseaudio", "network", "cpu", "memory", "battery", "tray"],
    "sway/workspaces": {
        "disable-scroll": true,
        "all-outputs": true,
        "format": "{name}",
        "format-icons": {
            "urgent": "",
            "focused": "",
            "default": ""
        }
    },
    "sway/mode": {
        "format": "<span style=\"italic\">{}</span>"
    },
    "clock": {
        "format": "{:%H:%M | %d.%m.%Y}",
        "tooltip-format": "<big>{:%Y %B}</big>\n<tt><small>{calendar}</small></tt>"
    },
    "pulseaudio": {
        "format": "{volume}% {icon}",
        "format-bluetooth": "{volume}% {icon}",
        "format-muted": "Muted",
        "format-icons": {
            "headphones": "",
            "default": ["", "", ""]
        },
        "scroll-step": 1,
        "on-click": "pavucontrol"
    },
    "network": {
        "format-wifi": "{essid} ({signalStrength}%) ",
        "format-ethernet": "{ifname}: {ipaddr}/{cidr} ",
        "format-disconnected": "Disconnected",
        "tooltip-format": "{ifname} via {gwaddr}"
    },
    "cpu": {
        "format": "{usage}% ",
        "interval": 1
    },
    "memory": {
        "format": "{}% "
    },
    "battery": {
        "states": {
            "warning": 30,
            "critical": 15
        },
        "format": "{capacity}% {icon}",
        "format-charging": "{capacity}% ",
        "format-plugged": "{capacity}% ",
        "format-icons": ["", "", "", "", ""]
    },
    "tray": {
        "spacing": 10
    }
}
```

Файл `~/.config/waybar/style.css`:

```css
* {
    font-family: "JetBrains Mono Nerd Font";
    font-size: 13px;
    border: none;
    border-radius: 0;
}

window#waybar {
    background-color: rgba(30, 30, 46, 0.9);
    color: #cdd6f4;
    transition-property: background-color;
    transition-duration: .5s;
}

#workspaces button {
    padding: 0 5px;
    background-color: transparent;
    color: #cdd6f4;
}

#workspaces button:hover {
    background: rgba(255, 255, 255, 0.1);
}

#workspaces button.focused {
    background-color: #89b4fa;
    color: #1e1e2e;
}

#workspaces button.urgent {
    background-color: #f38ba8;
    color: #1e1e2e;
}

#clock, #battery, #cpu, #memory, #network, #pulseaudio, #tray {
    padding: 0 10px;
    margin: 0 4px;
}

#clock {
    color: #89b4fa;
}

#battery.charging, #battery.plugged {
    color: #a6e3a1;
}

#battery.critical:not(.charging) {
    color: #f38ba8;
}

#cpu {
    color: #fab387;
}

#memory {
    color: #cba6f7;
}

#network {
    color: #94e2d5;
}

#pulseaudio {
    color: #f9e2af;
}

#tray {
    background-color: transparent;
}
```

### 1.4. Wofi: полный конфиг

Файл `~/.config/wofi/style.css`:

```css
window {
    margin: 0px;
    border: 2px solid #89b4fa;
    background-color: #1e1e2e;
    border-radius: 5px;
}

#input {
    margin: 5px;
    border: none;
    color: #cdd6f4;
    background-color: #313244;
    border-radius: 3px;
}

#inner-box {
    margin: 5px;
    border: none;
    background-color: #1e1e2e;
}

#outer-box {
    margin: 5px;
    border: none;
    background-color: #1e1e2e;
}

#scroll {
    margin: 0px;
    border: none;
}

#text {
    margin: 5px;
    border: none;
    color: #cdd6f4;
}

#entry:selected {
    background-color: #89b4fa;
    color: #1e1e2e;
    border-radius: 3px;
}
```

### 1.5. Mako: полный конфиг

Файл `~/.config/mako/config`:

```ini
font=JetBrains Mono Nerd Font 10
background-color=#1e1e2e
text-color=#cdd6f4
border-color=#89b4fa
border-size=2
border-radius=5
padding=10
default-timeout=5000
anchor=top-right
max-visible=5
```

### 1.6. XDG Desktop Portal

Для корректной работы скриншотов, выбора файлов и шаринга экрана в Wayland требуются порталы.

```bash
sudo pacman -S xdg-desktop-portal xdg-desktop-portal-wlr xdg-desktop-portal-gtk
```

Автозапуск в Sway (`~/.config/sway/config`):

```text
exec /usr/lib/xdg-desktop-portal-wlr
exec /usr/lib/xdg-desktop-portal-gtk
```

Конфигурация `~/.config/xdg-desktop-portal/portals.conf`:

```ini
[preferred]
default=gtk
org.freedesktop.impl.portal.ScreenCast=wlr
org.freedesktop.impl.portal.Screenshot=wlr
```

Проверка статуса:

```bash
systemctl --user status xdg-desktop-portal
```

*Ссылки: Arch Wiki: Sway, Waybar, Wofi, Mako, Swaylock, Swayidle, Grim, Slurp, wl-clipboard, XDG Desktop Portal.*

---

## 2. Терминал: подробно

### 2.1. Alacritty

GPU-ускоренный эмулятор терминала. Конфигурация в TOML.

Файл `~/.config/alacritty/alacritty.toml`:

```toml
[env]
TERM = "xterm-256color"

[window]
padding = { x = 10, y = 10 }
opacity = 0.95
dynamic_padding = true

[font]
normal = { family = "JetBrains Mono Nerd Font", style = "Regular" }
bold = { family = "JetBrains Mono Nerd Font", style = "Bold" }
italic = { family = "JetBrains Mono Nerd Font", style = "Italic" }
size = 11.0

[colors.primary]
background = "#1e1e2e"
foreground = "#cdd6f4"

[colors.normal]
black   = "#45475a"
red     = "#f38ba8"
green   = "#a6e3a1"
yellow  = "#f9e2af"
blue    = "#89b4fa"
magenta = "#f5c2e7"
cyan    = "#94e2d5"
white   = "#bac2de"

[colors.bright]
black   = "#585b70"
red     = "#f38ba8"
green   = "#a6e3a1"
yellow  = "#f9e2af"
blue    = "#89b4fa"
magenta = "#f5c2e7"
cyan    = "#94e2d5"
white   = "#a6adc8"

[cursor]
style = { shape = "Block", blinking = "On" }
```

### 2.2. Foot

Легковесный Wayland-native терминал. Конфигурация в INI.

Файл `~/.config/foot/foot.ini`:

```ini
font=JetBrains Mono Nerd Font:size=11
pad=10x10 center

[colors]
alpha=0.95
background=1e1e2e
foreground=cdd6f4
regular0=45475a
regular1=f38ba8
regular2=a6e3a1
regular3=f9e2af
regular4=89b4fa
regular5=f5c2e7
regular6=94e2d5
regular7=bac2de
bright0=585b70
bright1=f38ba8
bright2=a6e3a1
bright3=f9e2af
bright4=89b4fa
bright5=f5c2e7
bright6=94e2d5
bright7=a6adc8

[cursor]
style=block
blink=yes
```

### 2.3. Tmux

Мультиплексор терминала. Позволяет создавать сессии, окна и панели внутри одного терминала.

Установка:

```bash
sudo pacman -S tmux
```

Файл `~/.tmux.conf`:

```text
# Убрать задержку Escape
set -sg escape-time 0

# Начинать нумерацию с 1
set -g base-index 1
setw -g pane-base-index 1

# Включить мышь
set -g mouse on

# Перезагрузка конфига
bind r source-file ~/.tmux.conf \; display "Config reloaded!"

# Разделение панелей (| и - вместо % и ")
bind | split-window -h -c "#{pane_current_path}"
bind - split-window -v -c "#{pane_current_path}"
bind c new-window -c "#{pane_current_path}"

# Навигация vim-style
bind h select-pane -L
bind j select-pane -D
bind k select-pane -U
bind l select-pane -R

# Статус-бар
set -g status-position bottom
set -g status-style 'bg=#1e1e2e fg=#cdd6f4'
set -g status-left '#[fg=#89b4fa,bold] #S '
set -g status-right '#[fg=#a6e3a1] %H:%M '
set -g status-interval 5

# Плагины через TPM (Tmux Plugin Manager)
set -g @plugin 'tmux-plugins/tpm'
set -g @plugin 'tmux-plugins/tmux-sensible'
set -g @plugin 'tmux-plugins/tmux-resurrect'
set -g @plugin 'tmux-plugins/tmux-continuum'

# Настройки resurrect
set -g @resurrect-strategy-nvim 'session'
set -g @continuum-restore 'on'

run '~/.tmux/plugins/tpm/tpm'
```

Установка TPM:

```bash
git clone https://github.com/tmux-plugins/tpm ~/.tmux/plugins/tpm
```

Основные команды:

```bash
tmux new -s имя       # создать сессию
tmux attach -t имя    # подключиться к сессии
tmux ls               # список сессий
Ctrl+b d              # отключиться (detach)
Ctrl+b c              # новое окно
Ctrl+b n              # следующее окно
Ctrl+b p              # предыдущее окно
Ctrl+b %              # разделить вертикально
Ctrl+b "              # разделить горизонтально
Ctrl+b z              # развернуть панель на весь экран
Ctrl+b I              # установить плагины TPM
```

### 2.4. Neovim

Файл `~/.config/nvim/init.lua`:

```lua
-- Опции
vim.opt.number = true
vim.opt.relativenumber = true
vim.opt.tabstop = 4
vim.opt.shiftwidth = 4
vim.opt.expandtab = true
vim.opt.clipboard = "unnamedplus"
vim.opt.termguicolors = true
vim.opt.cursorline = true
vim.opt.signcolumn = "yes"
vim.opt.scrolloff = 8
vim.opt.updatetime = 300
vim.opt.ignorecase = true
vim.opt.smartcase = true

-- Менеджер плагинов lazy.nvim
local lazypath = vim.fn.stdpath("data") .. "/lazy/lazy.nvim"
if not vim.loop.fs_stat(lazypath) then
  vim.fn.system({
    "git", "clone", "--filter=blob:none",
    "https://github.com/folke/lazy.nvim.git",
    "--branch=stable", lazypath,
  })
end
vim.opt.rtp:prepend(lazypath)

require("lazy").setup({
  -- Тема
  { "catppuccin/nvim", name = "catppuccin", priority = 1000 },
  -- Файловый менеджер
  { "nvim-telescope/telescope.nvim", dependencies = { "nvim-lua/plenary.nvim" } },
  -- Дерево файлов
  { "nvim-neo-tree/neo-tree.nvim", branch = "v3.x", dependencies = { "nvim-lua/plenary.nvim", "MunifTanjim/nui.nvim" } },
})

vim.cmd.colorscheme "catppuccin"

-- Горячие клавиши
vim.keymap.set("n", "<leader>e", ":Neotree toggle<CR>", { desc = "Toggle file explorer" })
vim.keymap.set("n", "<leader>f", ":Telescope find_files<CR>", { desc = "Find files" })
vim.keymap.set("n", "<leader>g", ":Telescope live_grep<CR>", { desc = "Live grep" })
```

### 2.5. Yazi

Файловый менеджер для терминала, написанный на Rust.

Запуск:

```bash
yazi
```

Горячие клавиши:

- `j/k` — вниз/вверх
- `l` — войти в директорию / открыть файл
- `h` — назад
- `Space` — выделить
- `y` — копировать
- `x` — вырезать
- `p` — вставить
- `d` — удалить
- `/` — поиск
- `q` — выход

### 2.6. Zsh: подробная настройка

Установка плагинов:

```bash
sudo pacman -S zsh zsh-completions zsh-syntax-highlighting zsh-autosuggestions
```

Полный `~/.zshrc`:

```bash
# Инициализация автодополнения
autoload -Uz compinit && compinit

# Подсветка синтаксиса и автоподсказки
source /usr/share/zsh/plugins/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh
source /usr/share/zsh/plugins/zsh-autosuggestions/zsh-autosuggestions.zsh

# Алиасы
alias ls='eza --icons'
alias ll='eza -l --icons --git'
alias la='eza -la --icons --git'
alias cat='bat'
alias grep='rg'
alias find='fd'
alias cd='z'
alias vi='nvim'
alias vim='nvim'
alias top='btop'
alias df='duf'
alias du='dust'

# Интеграция zoxide
eval "$(zoxide init zsh)"

# Интеграция fzf
source /usr/share/fzf/key-bindings.zsh
source /usr/share/fzf/completion.zsh

# Промпт
PROMPT='%F{green}%n@%m%f:%F{blue}%~%f$ '

# История
HISTSIZE=10000
SAVEHIST=10000
setopt SHARE_HISTORY
setopt HIST_IGNORE_DUPS
setopt HIST_IGNORE_SPACE
```

### 2.7. Современные утилиты CLI

Все эти утилиты заменяют стандартные GNU coreutils на более быстрые и функциональные аналоги.

```bash
sudo pacman -S eza bat fd ripgrep fzf zoxide btop fastfetch dust duf tldr plocate
```

- **eza** — замена `ls`. Поддерживает иконки, Git-статусы, дерево.
  ```bash
  eza --long --header --git
  eza --tree --level=2
  ```
- **bat** — замена `cat`. Подсветка синтаксиса, нумерация строк.
  ```bash
  bat файл
  ```
- **fd** — замена `find`. Быстрее, проще синтаксис.
  ```bash
  fd "*.txt"
  fd -e rs
  ```
- **ripgrep (rg)** — замена `grep`. Экстремально быстрый поиск.
  ```bash
  rg "текст"
  rg -t py "def "
  ```
- **fzf** — нечёткий поиск по истории и файлам.
  ```text
  Ctrl+r            # поиск в истории команд
  Ctrl+t            # поиск файлов
  ```
- **zoxide** — умная замена `cd`. Запоминает часто используемые директории.
  ```bash
  z название_директории
  ```
- **btop** — замена `htop`. Мониторинг CPU, RAM, дисков, сети.
  ```bash
  btop
  ```
- **fastfetch** — замена `neofetch`. Информация о системе.
  ```bash
  fastfetch
  ```
- **dust** — замена `du`. Визуальный анализ дискового пространства.
  ```bash
  dust
  ```
- **duf** — замена `df`. Информация о примонтированных дисках.
  ```bash
  duf
  ```
- **tldr** — краткие примеры использования команд (замена `man`).
  ```bash
  tldr команда
  ```
- **plocate** — замена `locate`. Быстрая индексация файлов.
  ```bash
  sudo updatedb
  plocate файл
  ```

*Ссылки: Arch Wiki: Alacritty, Foot, Tmux, Neovim, Yazi, Zsh.*

---

## 3. Разработка: подробно

### 3.1. Git: SSH, GPG, hooks

SSH-ключи:

```bash
ssh-keygen -t ed25519 -C "email@example.com"
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
cat ~/.ssh/id_ed25519.pub
```

Добавьте публичный ключ в настройки GitHub/GitLab.

GPG-подпись коммитов:

```bash
gpg --full-generate-key
gpg --list-secret-keys --keyid-format=long
git config --global user.signingkey <KEY_ID>
git config --global commit.gpgsign true
```

Hooks (хуки):

Расположение: `.git/hooks/`

Пример `pre-commit` хука (проверка форматирования):

```bash
#!/bin/bash
cargo fmt --check
if [ $? -ne 0 ]; then
  echo "Code is not formatted. Run cargo fmt."
  exit 1
fi
```

```bash
chmod +x .git/hooks/pre-commit
```

Алиасы Git:

```bash
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.lg "log --oneline --graph --decorate"
```

### 3.2. Docker: compose, networks, volumes

> **Note:** Если вы используете Podman вместо Docker, см. раздел 3.3. Синтаксис Docker Compose поддерживается Podman через `podman-compose`.

Установка:

```bash
sudo pacman -S docker docker-compose
sudo systemctl enable --now docker
sudo usermod -aG docker $USER
```

Docker Compose (`docker-compose.yml`):

```yaml
version: '3.8'
services:
  web:
    image: nginx:latest
    ports:
      - "8080:80"
    volumes:
      - ./html:/usr/share/nginx/html
    networks:
      - mynet
  db:
    image: postgres:15
    environment:
      POSTGRES_PASSWORD: secret
    volumes:
      - db_data:/var/lib/postgresql/data
    networks:
      - mynet

networks:
  mynet:
    driver: bridge

volumes:
  db_data:
```

Команды:

```bash
docker-compose up -d
docker-compose down
docker-compose logs -f
```

Networks:

```bash
docker network create mynet
docker network ls
docker network inspect mynet
```

Volumes:

```bash
docker volume create mydata
docker volume ls
docker volume inspect mydata
```

### 3.3. Podman: rootless, systemd

Podman — бездемонная альтернатива Docker. Полностью совместим с CLI Docker.

Установка:

```bash
sudo pacman -S podman buildah skopeo podman-compose
```

Rootless-режим:

Podman работает от непривилегированного пользователя по умолчанию. Для портов < 1024:

```bash
sudo sysctl net.ipv4.ip_unprivileged_port_start=80
```

Запуск контейнеров как systemd-служб:

```bash
podman generate systemd --new --name mycontainer > ~/.config/systemd/user/mycontainer.service
systemctl --user enable --now mycontainer
```

Podman Compose:

```bash
podman-compose up -d
podman-compose down
```

Buildah (сборка образов без Dockerfile):

```bash
buildah from alpine
buildah run alpine-working-container apk add curl
buildah commit alpine-working-container myimage
```

Skopeo (копирование образов между реестрами):

```bash
skopeo copy docker://docker.io/library/alpine:latest dir:/tmp/alpine
```

### 3.4. Python: venv, pipx, poetry

venv (виртуальные окружения):

```bash
python -m venv venv
source venv/bin/activate
pip install requests
deactivate
```

pipx (изоляция CLI-приложений):

```bash
sudo pacman -S python-pipx
pipx ensurepath
pipx install black
```

poetry (управление зависимостями):

```bash
pipx install poetry
poetry new проект
cd проект
poetry add requests
poetry install
poetry run python main.py
```

### 3.5. Node.js и Bun

Bun — быстрый runtime для JS/TS, замена Node.js/npm.

```bash
sudo pacman -S bun
bun init
bun install <пакет>
bun run index.ts
```

Node.js (если требуется):

```bash
sudo pacman -S nodejs npm
node --version
npm install <пакет>
```

### 3.6. Rust: cargo, rustup, clippy

rustup управляет версиями Rust.

```bash
sudo pacman -S rustup
rustup default stable
rustup component add clippy rustfmt
```

Cargo:

```bash
cargo new проект
cd проект
cargo build
cargo run
cargo test
cargo clippy
cargo fmt
```

### 3.7. Go: GOPATH, модули

```bash
sudo pacman -S go
go mod init github.com/user/project
go get github.com/sirupsen/logrus
go build
go run .
```

Переменная GOPATH:

```bash
export GOPATH=$HOME/go
export PATH=$PATH:$GOPATH/bin
```

### 3.8. Java: JAVA_HOME, Maven, Gradle

```bash
sudo pacman -S jdk-openjdk maven gradle
```

JAVA_HOME:

```bash
export JAVA_HOME=/usr/lib/jvm/default
```

Maven:

```bash
mvn archetype:generate -DgroupId=com.example -DartifactId=app -DarchetypeArtifactId=maven-archetype-quickstart
mvn package
java -jar target/app-1.0-SNAPSHOT.jar
```

Gradle:

```bash
gradle init
gradle build
gradle run
```

### 3.9. CMake, Meson, GDB

CMake (`CMakeLists.txt`):

```cmake
cmake_minimum_required(VERSION 3.10)
project(MyProject)
add_executable(myapp main.c)
```

```bash
cmake -B build
cmake --build build
./build/myapp
```

Meson (`meson.build`):

```text
project('myapp', 'c')
executable('myapp', 'main.c')
```

```bash
meson setup build
ninja -C build
./build/myapp
```

GDB (отладчик):

```bash
gcc -g main.c -o myapp
gdb ./myapp
```

Команды GDB:

```text
break main
run
next
step
print variable
quit
```

### 3.10. KiCad

EDA-система для проектирования печатных плат.

```bash
sudo pacman -S kicad kicad-library
kicad
```

Библиотеки устанавливаются автоматически через `kicad-library`. Проекты сохраняются в формате `.kicad_pro`.

*Ссылки: Arch Wiki: Git, Docker, Podman, Python, Node.js, Rust, Go, Java, GCC, Clang, CMake, KiCad.*

---

## 4. Мультимедиа: подробно

### 4.1. VLC: кодеки, ускорение, субтитры

```bash
sudo pacman -S vlc
```

Кодеки (все форматы):

```bash
sudo pacman -S ffmpeg gst-plugins-good gst-plugins-bad gst-plugins-ugly
```

Аппаратное ускорение:

Tools → Preferences → Input/Codecs → Hardware-accelerated decoding → выберите VA-API (AMD/Intel) или VDPAU/NVDEC (NVIDIA).

Субтитры:

Tools → Preferences → Subtitles/OSD → настройте шрифт, размер, кодировку (UTF-8).

### 4.2. mpv

Минималистичный видеоплеер с мощным движком.

```bash
sudo pacman -S mpv
```

Файл `~/.config/mpv/mpv.conf`:

```ini
vo=gpu-next
hwdec=auto-safe
profile=gpu-hq
scale=ewa_lanczossharp
cscale=ewa_lanczossharp
ao=pipewire
audio-channels=auto
sub-auto=fuzzy
sub-font-size=40
sub-color="#FFFFFF"
sub-border-color="#000000"
sub-border-size=2
osc=yes
osd-bar=yes
save-position-on-quit=yes
```

Горячие клавиши:

- `Space` — пауза
- `←/→` — перемотка
- `↑/↓` — громкость
- `f` — полный экран
- `s` — скриншот

### 4.3. OBS Studio: сцены, источники, стрим

```bash
sudo pacman -S obs-studio
```

Настройка сцены:

1. Sources → «+» → Screen Capture (PipeWire) → выберите монитор.
2. Sources → «+» → Audio Input Capture → выберите микрофон.
3. Sources → «+» → Audio Output Capture → выберите звук системы.

Настройка кодировщика:

Settings → Output → Output Mode: Advanced → Streaming:

- Encoder: NVENC (NVIDIA), AMF (AMD), VA-API (Intel), x264 (CPU).
- Bitrate: 6000 Kbps для 1080p60.
- Rate Control: CBR.

Настройка звука:

Settings → Audio → Sample Rate: 48 kHz.

### 4.4. Krita: кисти, планшет

Растровый редактор для цифровой живописи.

```bash
sudo pacman -S krita
```

Графические планшеты (Wacom/Huion) работают через драйвер `libinput` автоматически в Wayland.

### 4.5. Inkscape: расширения

Векторный редактор SVG.

```bash
sudo pacman -S inkscape
```

Расширения устанавливаются в `~/.config/inkscape/extensions/`.

### 4.6. GIMP

Растровый графический редактор.

```bash
sudo pacman -S gimp
```

Плагины:

```bash
sudo pacman -S gimp-plugin-gmic
paru -S gimp-plugin-resynthesizer
```

### 4.7. Blender

3D-редактор. Автоматически использует GPU через CUDA (NVIDIA) или HIP (AMD).

```bash
sudo pacman -S blender
```

### 4.8. Kdenlive

Нелинейный видеоредактор.

```bash
sudo pacman -S kdenlive
```

### 4.9. Audacity

Аудиоредактор.

```bash
sudo pacman -S audacity
```

### 4.10. Ardour

Цифровая звуковая рабочая станция (DAW). Рекомендуется использовать с PipeWire или JACK.

```bash
sudo pacman -S ardour
```

### 4.11. qBittorrent: порты, шифрование

```bash
sudo pacman -S qbittorrent
```

Настройка:

Tools → Preferences → Connection → порт.
Tools → Preferences → BitTorrent → включите шифрование (Require encryption).

### 4.12. Telegram Desktop

```bash
sudo pacman -S telegram-desktop
```

Шифрование: Secret Chats используют end-to-end шифрование. Темы: Settings → Chat Settings → Change chat background.

### 4.13. Meld: трёхстороннее сравнение

Инструмент визуального сравнения файлов и директорий.

```bash
sudo pacman -S meld
meld файл1 файл2
meld файл1 файл2 файл3
meld директория1 директория2
```

### 4.14. imv и evince

Просмотр изображений:

```bash
sudo pacman -S imv
imv изображение.png
```

Просмотр PDF:

```bash
sudo pacman -S evince
evince файл.pdf
```

*Ссылки: Arch Wiki: VLC, mpv, OBS Studio, Krita, Inkscape, GIMP, Blender, Kdenlive, Audacity, Ardour, Meld.*

---

## 5. Диски и EFI

### 5.1. Btrfs: subvolumes, compression, quotas

Субволюмы:

```bash
sudo btrfs subvolume list /
sudo btrfs subvolume create /mnt/@new
sudo mount -o subvol=@new /dev/sda2 /mnt
```

Сжатие:

```bash
sudo mount -o compress=zstd /dev/sda2 /mnt
```

В `/etc/fstab`:

```text
UUID=... / btrfs defaults,compress=zstd,noatime 0 1
```

Квоты:

```bash
sudo btrfs quota enable /
sudo btrfs qgroup show /
```

### 5.2. Snapper: конфиг, таймеры

Конфигурация `/etc/snapper/configs/root`:

```ini
TIMELINE_CREATE="yes"
TIMELINE_CLEANUP="yes"
TIMELINE_LIMIT_HOURLY="0"
TIMELINE_LIMIT_DAILY="0"
TIMELINE_LIMIT_WEEKLY="0"
TIMELINE_LIMIT_MONTHLY="0"
TIMELINE_LIMIT_YEARLY="0"
NUMBER_LIMIT="10"
NUMBER_CLEANUP="yes"
```

Таймер очистки:

```bash
sudo systemctl enable --now snapper-cleanup.timer
```

### 5.3. Btrfs Assistant

GUI для управления снапшотами.

```bash
btrfs-assistant
```

Функции: создание, удаление, откат снапшотов; настройка Snapper; сравнение.

### 5.4. limine-snapper-sync, snap-pac, cachyos-snapper-support

`/etc/limine-snapper-sync.conf`:

```ini
LIMIT_USAGE_PERCENT=85
MAX_SNAPSHOT_ENTRIES=8
RESTORE_METHOD=replace
SET_SNAPSHOT_AS_DEFAULT=no
```

`/etc/snap-pac.ini`:

```ini
[Snapper]
TIMELINE_CREATE="yes"
TIMELINE_CLEANUP="yes"
```

`cachyos-snapper-support` предоставляет шаблоны конфигов Snapper для CachyOS.

### 5.5. parted: GPT, MBR

GPT:

```bash
sudo parted /dev/sda
mklabel gpt
mkpart primary fat32 1MiB 1GiB
mkpart primary ext4 1GiB 100%
set 1 esp on
```

MBR:

```bash
sudo parted /dev/sda
mklabel msdos
mkpart primary ext4 1MiB 100%
set 1 boot on
```

### 5.6. RAID: мониторинг

```bash
cat /proc/mdstat
sudo mdadm --detail /dev/md0
sudo mdadm --monitor --mail=email@example.com --daemonise /dev/md0
```

### 5.7. fsarchiver: сжатие, шифрование

Сжатие:

```bash
sudo fsarchiver savefs -z 9 /mnt/backup.fsa /dev/sda2
```

Шифрование:

```bash
sudo fsarchiver savefs -c - /mnt/backup.fsa /dev/sda2
```

Восстановление:

```bash
sudo fsarchiver restfs /mnt/backup.fsa id=0,dest=/dev/sda2
```

### 5.8. EFI: efibootmgr, efivar, efitools

efibootmgr:

```bash
sudo efibootmgr
sudo efibootmgr -v
sudo efibootmgr -c -d /dev/sda -p 1 -L "CachyOS" -l '\EFI\limine\liminex64.efi'
sudo efibootmgr -b 0001 -B
```

efivar:

```bash
efivar -l
efivar -n 8be4df61-93ca-11d2-aa0d-00e098032b8c-BootOrder
```

efitools:

```bash
sudo pacman -S efitools
cert-to-efi-sig-list PK.crt PK.esl
sign-efi-sig-list -k PK.key -c PK.crt PK PK.esl PK.auth
```

*Ссылки: Arch Wiki: Btrfs, Snapper, Parted, RAID, fsarchiver, EFI.*

---

## 6. Утилиты pacman

### 6.1. pkgfile

Поиск файлов в пакетах (без установки).

```bash
sudo pacman -S pkgfile
sudo pkgfile --update
pkgfile <файл>
```

### 6.2. rebuild-detector

Проверка пакетов, требующих пересборки после обновления библиотек.

```bash
sudo pacman -S rebuild-detector
sudo rebuild-detector
```

### 6.3. pacutils

Утилиты для анализа дерева зависимостей.

```bash
sudo pacman -S pacutils
pactree <пакет>
paccheck
```

### 6.4. pacman-contrib

Дополнительные скрипты для обслуживания pacman.

```bash
sudo pacman -S pacman-contrib
paccache -r          # очистка кэша (оставляет последние 3 версии)
paccache -rk1        # оставить только последнюю версию
checkupdates         # проверка обновлений без синхронизации
```

### 6.5. paru: флаги и конфиг

Файл `~/.config/paru/paru.conf`:

```ini
[options]
BottomUp
SudoLoop
UpgradeMenu
NewsOnUpgrade
CleanAfter
BatchInstall
```

- `BottomUp` — результаты поиска снизу вверх.
- `SudoLoop` — кэширует пароль sudo.
- `UpgradeMenu` — позволяет выбирать, какие пакеты обновлять.
- `NewsOnUpgrade` — показывает новости Arch при обновлении.
- `CleanAfter` — удаляет исходники после сборки.
- `BatchInstall` — собирает все пакеты, затем устанавливает разом.

*Ссылки: Arch Wiki: pkgfile, pacman-contrib; tldr: pactree.*
