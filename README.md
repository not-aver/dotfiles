# dotfiles

Конфиги Arch Linux + HyDE. Hyprland, waybar, walker, hyprlock, SDDM.

---

## Стек

| Компонент | Что стоит |
|---|---|
| WM | Hyprland |
| Bar | Waybar |
| Launcher | Walker + elephant |
| Terminal | Kitty |
| Shell | Zsh (ZDOTDIR=~/.config/zsh) |
| Notifications | Dunst |
| Lock screen | Hyprlock |
| Login screen | SDDM + sddm-astronaut-theme |
| Wallpapers | swww |
| Темы | HyDE |
| Pomodoro | tomat |

---

## Структура

```
backup/
├── hypr/               # Hyprland конфиги
│   ├── hyprlock.conf
│   ├── hyprlock/       # Лейауты локскрина
│   ├── keybindings.conf
│   ├── userprefs.conf  # Автозапуск и пользовательские настройки
│   ├── monitors.conf
│   ├── windowrules.conf
│   └── hypridle.conf
├── waybar/
│   ├── config.jsonc    # Основной конфиг
│   ├── style.css
│   ├── user-style.css  # Кастомные стили (не перезаписывается HyDE)
│   └── scripts/
│       └── pomodoro.sh # Скрипт для tomat модуля
├── walker/             # Конфиг и тема лаунчера
├── tomat/              # Конфиг помодоро таймера
├── bin/                # Maintenance скрипты
└── user.zsh            # Пользовательские алиасы и настройки zsh
```

---

## Скрипты

Лежат в `~/.local/bin/`, добавлены в PATH.

### clean

Чистка системы одной командой:

```bash
clean
```

Что делает:
- `paccache -rk2` — оставляет только 2 последние версии каждого пакета в кэше pacman
- Показывает список сирот (пакеты без зависимостей) и предлагает удалить
- Предлагает очистить `~/.cache/paru`
- Показывает `df -h` после чистки

### backup-configs

Бэкап всех конфигов в `~/dotfiles/backup/`:

```bash
backup-configs
```

Перезаписывает предыдущий бэкап (история в git).

### sddm-preset

Переключение пресетов SDDM темы:

```bash
sddm-preset              # показать список и текущий пресет
sddm-preset cyberpunk    # переключить на cyberpunk
sddm-preset astronaut    # переключить на astronaut
```

Доступные пресеты: `astronaut`, `black_hole`, `cyberpunk`, `hyprland_kath`, `jake_the_dog`, `japanese_aesthetic`, `pixel_sakura`, `post-apocalyptic_hacker`, `purple_leaves`.

Вступает в силу при следующем входе в систему.

### tomat-autostart

Запускается автоматически через Hyprland. Стартует daemon и сессию 50/10:

```bash
tomat daemon start
sleep 2
tomat start --work 50 --break 10 --long-break 30 --sound-mode none
```

---

## Терминальные инструменты

### fzf — fuzzy finder

Интерактивный поиск прямо в терминале.

**Горячие клавиши:**

| Клавиши | Действие |
|---|---|
| `Ctrl+R` | Поиск по истории команд |
| `Ctrl+T` | Поиск файлов в текущей папке |
| `Alt+C` | Интерактивный переход в папку |

Просто начни печатать — fzf сам найдёт похожее. Работает с опечатками.

---

### zoxide — умный cd

Запоминает куда ты ходишь и позволяет прыгать туда по короткому запросу.

```bash
z projects          # прыгнуть в ~/projects/ (если бывал там раньше)
z doh               # прыгнуть в ~/projects/dohlebyvay/
z conf              # прыгнуть в ~/.config/
```

Первое время используй как обычный `cd` — zoxide накапливает историю. Через пару дней `z` начнёт угадывать правильно с первых букв.

**Полезные команды:**

```bash
z -                 # вернуться в предыдущую папку
zi                  # интерактивный выбор через fzf
zoxide query -l     # показать все запомненные папки
```

---

### yazi — файловый менеджер в терминале

Быстрый файловый менеджер с превью картинок, видео и кода прямо в Kitty.

Запуск (с переходом в папку при выходе):

```bash
y
```

**Основные биндинги:**

| Клавиши | Действие |
|---|---|
| `hjkl` / стрелки | Навигация |
| `Enter` | Открыть файл/папку |
| `Space` | Выделить файл |
| `y` | Копировать |
| `x` | Вырезать |
| `p` | Вставить |
| `d` | Удалить в корзину |
| `r` | Переименовать |
| `/` | Поиск |
| `.` | Показать скрытые файлы |
| `q` | Выйти (и перейти в текущую папку) |

Открывает файлы в нужном приложении автоматически. Code OSS открывается по Enter на любом текстовом файле.

---

## Автозапуск (userprefs.conf)

```ini
exec-once = elephant
exec-once = walker --gapplication-service
exec-once = tomat-autostart
```

---

## Восстановление после переустановки

```bash
# Клонировать репо
git clone https://github.com/aver/dotfiles ~/dotfiles

# Скопировать конфиги
cp -r ~/dotfiles/backup/hypr/* ~/.config/hypr/
cp -r ~/dotfiles/backup/waybar/* ~/.config/waybar/
cp -r ~/dotfiles/backup/walker/ ~/.config/
cp -r ~/dotfiles/backup/tomat/ ~/.config/
cp -r ~/dotfiles/backup/bin/* ~/.local/bin/
cp ~/dotfiles/backup/user.zsh ~/.config/zsh/user.zsh

# Сделать скрипты исполняемыми
chmod +x ~/.local/bin/*
```
