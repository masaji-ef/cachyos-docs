# basic.md

## Оглавление

1. [Введение](#1-введение)
2. [Установка CachyOS](#2-установка-cachyos)
3. [Первичная настройка системы](#3-первичная-настройка-системы)
4. [Пакеты и ядро](#4-пакеты-и-ядро)
5. [Графика, звук и драйверы](#5-графика-звук-и-драйверы)
6. [Сеть и подключения](#6-сеть-и-подключения)
7. [Восстановление системы](#7-восстановление-системы)

---

## 1. Введение

### 1.1. Что такое Linux

Linux — это ядро операционной системы, созданное Линусом Торвальдсом в 1991 году. Ядро управляет аппаратными ресурсами: процессором, памятью, дисками, сетью. Вокруг ядра строится дистрибутив — набор программ, утилит, пакетного менеджера и системы инициализации.

Основные компоненты любого Linux-дистрибутива:

- **Ядро (kernel)** — связующее звено между программами и железом.
- **Пакетный менеджер** — инструмент установки, обновления и удаления программ. В CachyOS это `pacman`.
- **Система инициализации (init)** — запускает службы при загрузке. В CachyOS используется `systemd`.
- **Файловая система** — способ организации данных на диске. В CachyOS по умолчанию рекомендуется `Btrfs`.
- **Оболочка (shell)** — интерфейс для ввода команд. По умолчанию в CachyOS — `bash`, но в этом руководстве используется `zsh`.
- **Графическая среда** — Wayland-композитор (в этой книге — `Sway`).

### 1.2. Что такое CachyOS

CachyOS — это дистрибутив на базе Arch Linux, ориентированный на производительность и современное железо. Ключевые особенности:

- **Оптимизированные репозитории** — пакеты собраны с поддержкой x86-64-v3 и x86-64-v4.
- **CachyOS Kernel** — оптимизированное ядро с патчами.
- **sched-ext** — поддержка подключаемых планировщиков задач.
- **Btrfs по умолчанию** — с автоматическими снапшотами через Snapper.
- **limine** — современный загрузчик по умолчанию.
- **chwd** — автоматическое определение и установка драйверов.
- **cachy-chroot** — утилита для восстановления системы.

CachyOS следует философии Arch: rolling release, KISS, do-it-yourself.

### 1.3. Терминал и базовые команды

Навигация:

```bash
pwd                  # показать текущую директорию
ls                   # список файлов
ls -la               # список с правами и скрытыми файлами
cd /путь             # перейти в директорию
cd ..                # на уровень выше
cd ~                 # в домашнюю директорию
```

Работа с файлами:

```bash
cp файл копия        # копировать
mv файл /путь        # переместить или переименовать
rm файл              # удалить файл
rm -rf директория    # удалить директорию рекурсивно
mkdir имя            # создать директорию
touch файл           # создать пустой файл
```

Права доступа:

```bash
chmod 755 файл       # изменить права
chown user:group файл # изменить владельца
sudo команда         # выполнить от имени root
```

Просмотр и поиск:

```bash
cat файл             # вывести содержимое
less файл            # постраничный просмотр
grep "текст" файл    # поиск строки
find /путь -name "*.txt" # поиск файлов
```

Пакеты:

```bash
pacman -S пакет      # установить
pacman -R пакет      # удалить
pacman -Syu          # обновить систему
```

Текстовый редактор `vim` — основной инструмент редактирования в этом руководстве. Базовые команды:

```text
vim файл             # открыть файл
i                    # режим вставки
Esc                  # выход в командный режим
:w                   # сохранить
:q                   # выйти
:wq                  # сохранить и выйти
:q!                  # выйти без сохранения
```

*Ссылки: Arch Wiki: General recommendations, Core utilities; Gentoo Handbook: Working with Gentoo.*

---

## 2. Установка CachyOS

### 2.1. Подготовка

Перед установкой необходимо:

1. Скачать ISO-образ CachyOS с официального сайта.
2. Проверить контрольную сумму (SHA256) и подпись.
3. Записать образ на USB-накопитель (минимум 8 ГБ) через `dd` или Rufus.
4. Загрузиться с USB, выбрав в BIOS/UEFI режим UEFI (Secure Boot отключён).

Требования:

- 64-битный процессор x86-64.
- Минимум 4 ГБ ОЗУ (рекомендуется 8 ГБ и больше).
- Минимум 32 ГБ дискового пространства (рекомендуется 64 ГБ и больше).
- Интернет-соединение (для загрузки пакетов).

### 2.2. GUI-установщик

CachyOS использует графический установщик Calamares. Пошагово:

1. **Язык и локаль** — выберите русский или английский.
2. **Клавиатура** — выберите раскладку.
3. **Разметка диска** — выберите «Стереть диск» для чистой установки или «Ручная разметка» для Btrfs.
4. **Файловая система** — выберите Btrfs. Установщик автоматически создаст субволюмы `@`, `@home`, `@root`, `@srv`, `@cache`, `@tmp`, `@log`.
5. **Загрузчик** — выберите `limine` (основной) или GRUB2 (альтернатива).
6. **Пользователь** — создайте пользователя и пароль. Пароль root можно оставить пустым (`sudo` будет настроен автоматически).
7. **Рабочий стол** — выберите Sway (единственный DE/WM в этой книге).
8. **Ядро** — выберите CachyOS Kernel (по умолчанию).
9. **Дополнительные пакеты** — при необходимости отметьте нужные.
10. **Установка** — нажмите «Установить» и дождитесь завершения.

> **Warning:** Шифрование LUKS и dual-boot с Windows не рассматриваются в этом руководстве.

### 2.3. Первая загрузка

После установки система загрузится через `limine`. В меню загрузчика выберите CachyOS. При первом входе введите пароль пользователя.

*Ссылки: CachyOS Wiki: Installation, Boot Managers, Filesystem; Arch Wiki: Installation guide.*

---

## 3. Первичная настройка системы

### 3.1. Обновление и зеркала

Первое действие после установки — обновление системы:

```bash
sudo pacman -Syu
```

Для ускорения загрузки пакетов настройте зеркала:

```bash
sudo cachyos-rate-mirrors
```

Оптимизированные репозитории (x86-64-v3 и v4) включены по умолчанию. Проверить поддержку CPU можно так:

```bash
/lib/ld-linux-x86-64.so.2 --help | grep supported
```

Если вывод содержит `x86-64-v3` или `x86-64-v4`, ваш процессор поддерживает соответствующий уровень. Репозитории уже настроены в `/etc/pacman.conf`.

### 3.2. Локаль, время и hostname

Локаль:

```bash
sudo localectl set-locale LANG=ru_RU.UTF-8
```

Часовой пояс:

```bash
sudo timedatectl set-timezone Europe/Moscow
sudo timedatectl set-ntp true
```

Hostname:

```bash
sudo hostnamectl set-hostname cachyos
```

Пользователи и sudo:

Пользователь, созданный при установке, уже добавлен в группу `wheel`. Проверьте:

```bash
groups
```

Если `wheel` отсутствует, добавьте:

```bash
sudo usermod -aG wheel имя_пользователя
```

Настройка sudo:

```bash
sudo visudo
```

Раскомментируйте строку:

```text
%wheel ALL=(ALL:ALL) ALL
```

### 3.3. chwd и cachy-chroot

`chwd` (CachyOS Hardware Detection) — утилита автоматической установки драйверов, написанная на Rust. Она автоматически определяет аппаратные компоненты и применяет оптимальные профили драйверов.

Запускается автоматически при установке. Вручную:

```bash
sudo chwd -a
```

`cachy-chroot` — утилита для входа в систему из live-USB. Используется для восстановления. Она сканирует все доступные разделы, поддерживает BTRFS-субволюмы и LUKS-шифрование.

```bash
sudo cachy-chroot
```

### 3.4. Zsh и настройка оболочки

Установите zsh:

```bash
sudo pacman -S zsh zsh-completions
```

Сделайте zsh оболочкой по умолчанию:

```bash
chsh -s /bin/zsh
```

Базовый файл конфигурации `~/.zshrc`:

```bash
autoload -Uz compinit && compinit
alias ls='ls --color=auto'
alias ll='ls -la'
alias grep='grep --color=auto'
PROMPT='%F{green}%n@%m%f:%F{blue}%~%f$ '
```

*Ссылки: CachyOS Wiki: Post Install Recommendations.*

---

## 4. Пакеты и ядро

### 4.1. Pacman

`pacman` — основной пакетный менеджер CachyOS.

Поиск:

```bash
pacman -Ss запрос
```

Установка:

```bash
sudo pacman -S пакет
```

Удаление:

```bash
sudo pacman -R пакет
sudo pacman -Rns пакет   # с зависимостями и конфигами
```

Обновление:

```bash
sudo pacman -Syu
```

Очистка кэша:

```bash
sudo pacman -Sc
```

Список установленных:

```bash
pacman -Q
```

### 4.2. AUR и paru

AUR (Arch User Repository) — репозиторий пользовательских пакетов. Для работы с ним используется помощник `paru` (быстрее `yay`).

Установка paru:

```bash
sudo pacman -S --needed base-devel git
git clone https://aur.archlinux.org/paru.git
cd paru
makepkg -si
```

Использование:

```bash
paru -S пакет
paru -Syu
paru -Ss запрос
```

`yay` — альтернативный помощник, упоминается для совместимости.

### 4.3. CachyOS Kernel и Kernel Manager

CachyOS предлагает несколько ядер:

- `linux-cachyos` — основное оптимизированное ядро.
- `linux-cachyos-bore` — с планировщиком BORE.
- `linux-cachyos-eevdf` — с EEVDF.
- `linux-cachyos-lts` — долгосрочная поддержка.
- `linux-cachyos-rt` — реального времени.

Установка:

```bash
sudo pacman -S linux-cachyos linux-cachyos-headers
```

Kernel Manager (GUI) — утилита управления ядрами. Позволяет устанавливать, удалять и переключать ядра, а также конфигурировать и собирать кастомные ядра CachyOS.

Доступные опции конфигурации кастомного ядра:

- Scheduler (BORE, RC, RT, RT+BORE, EEVDF, BMQ)
- Включение конфигурации CachyOS
- Настройка через nconfig, menuconfig, xconfig, gconfig
- NUMA (вкл/выкл)
- Modprobed-db (вкл/выкл)
- KBUILD CFLAGS (-O3 или -O2)
- Регулятор производительности по умолчанию
- BBR3 (вкл/выкл)
- Частота тактирования (100Hz, 250Hz, 300Hz, 500Hz, 600Hz, 750Hz, 1000Hz)
- Tickless mode (idle, periodic, full)
- Preemption (full, voluntary, server)
- Transparent Hugepages (always, madvise)
- DAMON (вкл/выкл)
- Автоопределение архитектуры CPU
- Оптимизация ядра для конкретных архитектур
- LTO (full, thin, none)
- Сборка ZFS-модуля
- Сборка проприетарного NVIDIA-модуля
- Сборка открытого NVIDIA-модуля
- Включение vmlinux с отладочными символами
- Загрузка/сохранение настроек Kernel Manager
- Управление патчами ядра (удалённое и локальное)

Собранные пакеты ядра сохраняются в `~/.cache/cachyos-km/`.

### 4.4. Параметры загрузки limine

`limine` — загрузчик по умолчанию. Конфигурация разделена на две части:

- `/boot/limine.conf` — настройки меню загрузки.
- `/etc/default/limine` — параметры ядра.

Пример `/boot/limine.conf`:

```text
timeout: 5
default_entry: 1

/CachyOS
    protocol: linux
    kernel_path: boot():/vmlinuz-linux-cachyos
    kernel_cmdline: root=UUID=... rw quiet nowatchdog
    module_path: boot():/initramfs-linux-cachyos.img
```

Настройка параметров ядра в `/etc/default/limine`:

Отредактируйте переменную `KERNEL_CMDLINE`:

```text
KERNEL_CMDLINE="root=UUID=... rw quiet nowatchdog"
```

После сохранения примените изменения:

```bash
sudo limine-mkinitcpio
```

Эта команда запускает mkinitcpio с хуком `limine-mkinitcpio-hook`, который обновляет записи загрузчика.

Автоматическое управление записями: в CachyOS записи ядра управляются автоматически через `limine-entry-tool`. При установке или удалении ядра записи обновляются в фоне.

### 4.5. Модули ядра

```bash
lsmod                    # список загруженных
modprobe модуль          # загрузить
modprobe -r модуль       # выгрузить
```

Чёрный список модулей: `/etc/modprobe.d/blacklist.conf`

```text
blacklist модуль
```

Автозагрузка: `/etc/modules-load.d/имя.conf`

```text
модуль
```

### 4.6. sched-ext планировщики

`sched-ext` позволяет подключать планировщики задач. CachyOS предоставляет несколько планировщиков.

Установка:

```bash
sudo pacman -S scx-scheds
```

Запуск планировщика:

```bash
sudo scx_lavd
sudo scx_bpfland
sudo scx_rusty
```

Управление через Kernel Manager: кнопка `sched-ext scheduler config` в главном окне. Позволяет переключать планировщики, включать/отключать службу, устанавливать флаги и профили.

Конфигурация сохраняется в `/etc/scx_loader.toml`. `scx_loader` запускает выбранный планировщик с выбранным профилем и обеспечивает сохранение после перезагрузки.

`scx_flow` — budget-based планировщик, сортирующий задачи в четыре O(1) FIFO tier'а на основе оставшегося бюджета. Задачи, пробуждающиеся с бюджетом, получают приоритет и направляются на CPU, где они работали последний раз (cache-warm). Задачи без бюджета попадают в Deficit tier, но не голодают благодаря ротации tier'ов при диспетчеризации. Планировщик детерминирован, не требует настройки, показывает live-метрики на `http://localhost:50005`.

### 4.7. CachyOS Settings (cachyos-settings)

Пакет `cachyos-settings` содержит множество оптимизаций sysctl, правил modprobe и вспомогательных скриптов.

Расположение конфигураций: `/usr/lib/sysctl.d/99-cachyos-settings.conf`. Для изменения скопируйте нужную строку в новый файл в `/etc/sysctl.d/`.

Ключевые оптимизации:

- **ZRAM swappiness** — более агрессивное использование ZRAM для кэша.
- **HPET permissions** — доступ к rtc0 и hpet для группы audio.
- **SATA power management** — режим max_performance.
- **I/O Scheduler rules** — автоматический выбор планировщика для HDD/SSD/NVMe.
- **hdparm rules** — максимальная производительность SATA/IDE HDD.
- **NVIDIA RTD3** — динамическое управление питанием для GPU Turing.
- **AMDGPU force** — принудительное использование драйвера AMDGPU для Southern Islands (GCN 1.0) и Sea Islands (GCN 2.0).
- **Watchdog blacklist** — отключение модулей watchdog.
- **THP Shrinker** — max_ptes_none = 409.
- **systemd journal max size** — 50 MB.
- **ZRAM Generator** — ZRAM размером с RAM с ZSTD-сжатием.

Вспомогательные скрипты:

- `cachyos-bugreport.sh` — сбор логов (inxi, dmesg, journalctl) для диагностики.
- `game-performance` — переключение на профиль производительности по требованию.
- `kerver` — информация о текущем ядре.
- `ksmctl / ksmstats` — управление Kernel Samepage Merging.
- `paste-cachyos` — вставка вывода терминала.
- `sbctl-batch-sign` — пакетная подпись ядер и EFI-бинарников для Secure Boot.
- `topmem` — статистика RAM/swap/KSM по процессам.
- `dlss-swapper` — принудительное использование новейшего DLSS в играх.
- `pci-latency` — снижение latency для PCI-звуковых карт.
- `zink-run` — запуск OpenGL через Zink Gallium.

NTP: предпочтительный сервер — Cloudflare, резервные — Google и Arch Linux.

### 4.8. amd-pstate (управление частотами AMD CPU)

Современные AMD CPU/APU используют драйвер `amd-pstate` на основе CPPC (Collaborative Processor Performance Control).

Три режима работы:

- Non-Autonomous Mode: `amd_pstate=passive` — платформа получает желаемый уровень производительности от ОС.
- Guided-Autonomous Mode: `amd_pstate=guided` — платформа учитывает текущую нагрузку и лимиты ОС.
- Autonomous Mode (EPP): `amd_pstate=active` — платформа учитывает только min/max performance и Energy Performance Preference.

Доступные governors: `powersave` (рекомендуется) и `performance`.

```bash
sudo cpupower frequency-set -g powersave
```

AMD Core Performance Boost (CPB): управляется через `power-profiles-daemon`. В профиле powersave CPB отключён, в balanced/performance — включён.

*Ссылки: CachyOS Wiki: General System Tweaks.*

---

## 5. Графика, звук и драйверы

### 5.1. Wayland

CachyOS использует Wayland по умолчанию. X11 не рассматривается, кроме обратной совместимости через Xwayland.

Проверка:

```bash
echo $XDG_SESSION_TYPE
```

### 5.2. Sway

Sway — тайловый Wayland-композитор. Единственный DE/WM, описываемый в этой книге.

Установка:

```bash
sudo pacman -S sway
```

Базовая настройка `~/.config/sway/config`:

```text
set $mod Mod4
font pango:monospace 10
exec swaybg -i /usr/share/backgrounds/cachyos.png
bindsym $mod+Return exec alacritty
bindsym $mod+d exec wofi --show drun
bindsym $mod+Shift+q kill
```

Запуск:

```bash
sway
```

### 5.3. Драйверы GPU

NVIDIA (proprietary):

```bash
sudo pacman -S nvidia nvidia-utils nvidia-settings
```

NVIDIA (open):

```bash
sudo pacman -S nvidia-open nvidia-utils
```

AMD:

```bash
sudo pacman -S mesa vulkan-radeon libva-mesa-driver
```

Intel:

```bash
sudo pacman -S mesa vulkan-intel intel-media-driver
```

Гибридная графика (PRIME Offload):

В CachyOS не требуется дополнительная настройка — `nvidia-utils` и `cachyos-settings` уже содержат всё необходимое.

Запуск приложения на дискретной GPU:

```bash
prime-run <программа>
```

Пакет `nvidia-prime` предоставляет `prime-run` как обёртку над переменными окружения.

Для open-source драйверов (AMD+AMD, AMD+Intel, Intel+Nouveau):

```bash
DRI_PRIME=1 <программа>
```

> **Warning:** Не используйте `optimus-manager`, `nvidia-xrun`, `Bumblebee` — они устарели и не поддерживаются.

### 5.4. Звук

CachyOS использует PipeWire. Проверка:

```bash
systemctl --user status pipewire
```

Установка компонентов:

```bash
sudo pacman -S pipewire pipewire-pulse pipewire-alsa wireplumber
```

Управление:

```bash
pactl list sinks
wpctl status
```

### 5.5. Аппаратное ускорение

VA-API (Intel, AMD):

```bash
sudo pacman -S libva-utils
vainfo
```

VDPAU (NVIDIA):

```bash
sudo pacman -S libvdpau-va-gl
vdpauinfo
```

NVDEC/NVENC: поддерживается драйвером NVIDIA автоматически.

Firefox:

В `about:config` включите:

```text
media.ffmpeg.vaapi.enabled = true
media.hardware-video-decoding.enabled = true
```

Chrome/Chromium:

Найдите файл флагов вашего браузера (например, `~/.config/chrome-flags.conf`) и добавьте:

```text
--enable-features=VaapiVideoDecoder --use-gl=desktop
```

Проверка: откройте `chrome://gpu` и убедитесь, что в разделах «Video Acceleration Information» и «Graphics Feature Status» указан статус «Hardware accelerated».

Discord:

Для включения аппаратного ускорения запустите Discord с флагами:

```bash
discord --ignore-gpu-blocklist --enable-features=VaapiVideoDecoder --use-gl=desktop --enable-gpu-rasterization --enable-zero-copy
```

*Ссылки: CachyOS Wiki: Enabling Hardware Acceleration in Google Chrome; Arch Wiki: Hardware video acceleration, Discord.*

---

## 6. Сеть и подключения

### 6.1. NetworkManager (nmcli)

NetworkManager управляет сетевыми подключениями. `nmcli` — командный интерфейс.

Просмотр подключений:

```bash
nmcli connection show
nmcli device status
```

Wi-Fi:

```bash
nmcli device wifi list
nmcli device wifi connect "SSID" password "пароль"
```

Статический IP:

```bash
nmcli connection modify "Wired connection 1" ipv4.addresses 192.168.1.100/24
nmcli connection modify "Wired connection 1" ipv4.gateway 192.168.1.1
nmcli connection modify "Wired connection 1" ipv4.dns "8.8.8.8 8.8.4.4"
nmcli connection modify "Wired connection 1" ipv4.method manual
nmcli connection up "Wired connection 1"
```

### 6.2. iwd (Wireless Daemon)

`iwd` — альтернативный Wi-Fi демон. В CachyOS может использоваться как бэкенд NetworkManager.

Включение как бэкенда NM:

Создайте файл `/etc/NetworkManager/conf.d/wifi_backend.conf`:

```ini
[device]
wifi.backend=iwd
```

Перезапустите NetworkManager:

```bash
sudo systemctl daemon-reload
sudo systemctl restart NetworkManager
```

> **Warning:** Не включайте `iwd.service` одновременно с NetworkManager — это конфликт.

### 6.3. SSH

Установка:

```bash
sudo pacman -S openssh
```

Запуск сервера:

```bash
sudo systemctl enable --now sshd
```

Подключение:

```bash
ssh user@192.168.1.100
```

Ключи:

```bash
ssh-keygen -t ed25519
ssh-copy-id user@192.168.1.100
```

### 6.4. wpa_supplicant

```bash
sudo pacman -S wpa_supplicant
```

Используется как альтернативный бэкенд для Wi-Fi.

*Ссылки: CachyOS Wiki: Network Configuration; Arch Wiki: NetworkManager, iwd, OpenSSH.*

---

## 7. Восстановление системы

### 7.1. cachy-chroot

Загрузитесь с live-USB CachyOS и выполните:

```bash
sudo cachy-chroot
```

Утилита автоматически смонтирует разделы и выполнит chroot. Она поддерживает BTRFS-субволюмы и LUKS-шифрование.

Пример выбора для BTRFS: при запросе введите `y` для использования пресета CachyOS BTRFS. Это автоматически смонтирует корневой субволюм и другие важные субволюмы (`/home`, `/var/tmp`, `/srv`).

### 7.2. Восстановление limine

Если загрузчик повреждён, из live-USB:

```bash
sudo mount /dev/nvme0n1p2 /mnt
sudo mount /dev/nvme0n1p1 /mnt/boot
sudo cachy-chroot
limine-install
```

Регенерация записей загрузчика после удаления снапшотов:

Limine: `sudo limine-update`

### 7.3. Откат снапшота Btrfs

Из live-USB:

```bash
sudo mount /dev/nvme0n1p2 /mnt
sudo snapper -c root list
sudo snapper -c root rollback номер
```

### 7.4. Частые проблемы

Wi-Fi не работает:

```bash
sudo pacman -S linux-firmware
sudo modprobe имя_модуля
```

Звук не работает:

```bash
systemctl --user restart pipewire
wpctl status
```

NVIDIA: чёрный экран:

Добавьте в параметры ядра limine:

```text
nvidia-drm.modeset=1
```

Обновление конфликтует:

```bash
sudo pacman -Syu --overwrite '*'
sudo pacman -Qkk
```

*Ссылки: CachyOS Wiki: cachy-chroot, Boot Manager Configuration, FAQ & Troubleshooting; Arch Wiki: General troubleshooting.*
