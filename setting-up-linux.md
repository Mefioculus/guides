# Введение

Учитывая, что мне уже не раз приходилось настраивать свое окружение в **Ubuntu**, я решил, что необходимо составить небольшой гайд по установке и настройке всего необходимого.
В любом случае в самом начале работы с новым линуксом необходимо будет произвести обновление данных о доступных пакетах,что можно сделать простой командой

```bash
sudo apt update
```

# Neofetch

- [Репозиторий проекта](https://github.com/dylanaraps/neofetch)

Полезное приложение для отображения информации о системе, на которой оно запускается.

```bash
sudo apt install neofetch
```

Кроме, собственно, установки, необходимо в конфиг файле командной оболочки (shell) прописать это приложение, чтобы оно запускалось со стартом терминала.
Для этого в `~/.bash/bashrc` или в `~/.config/fish/config.fish` необходимо прописать `neofetch`

# Exa

- [Репозиторий проекта](https://github.com/ogham/exa)

Более удобная альтернатива стандартной утилите `ls`.

```bash
sudo apt install exa
```

Далее желательно забиндить не псевдоним `ls` команду `exa -lh --icons`.
В **bash** это можно сделать командой `alias ls="exa -lh --icons"`.
В **fish** эта команда немного отличается: `alias -s ls="exa -lh --icons"`.

# Zoxide

Удобное дополнение к системной утилите **cd**.

- [Репозиторий проекта](https://github.com/ajeetdsouza/zoxide)

```bash
sudo apt install zoxide
```

Правда, после установки его нужно будет сконфигурировать.
Для bash это можно сделать следующим способом:
```bash
eval "$(zoxide init bash)"
```
Для fish это можно нужно будет дописать в конец конфиг файла `config.fish`
```bash
zoxide init fish | source
```

# FuzzyFinder

Отличная утилита для удобного и быстрого поиска по системе, которая так же необходима для корректной работы моего сетапа **neovim**

- [Репозиторий проекта](https://github.com/junegunn/fzf)

```bash
sudo apt install fzf
```

# RipGrep

Более быстрая замена стандартной утилите **grep**, которая называется **rg** (или **ripgrep**)

- [Репозиторий проекта](https://github.com/BurntSushi/ripgrep)

```bash
sudo apt install ripgrep
```

# LazyGit

Очень удобная **tui** оболочка для **git**.

- [Репозиторий проекта](https://github.com/jesseduffield/lazygit)

```bash
LAZYGIT_VERSION=$(curl -s "https://api.github.com/repos/jesseduffield/lazygit/releases/latest" | grep -Po '"tag_name": "v\K[^"]*')
curl -Lo lazygit.tar.gz "https://github.com/jesseduffield/lazygit/releases/latest/download/lazygit_${LAZYGIT_VERSION}_Linux_x86_64.tar.gz"
tar xf lazygit.tar.gz lazygit
sudo install lazygit /usr/local/bin
```

После установки можно будет реализовать удобный псевдоним:

```bash
alial -s lg="lazygit"
```

# Fish 

- [Репозиторий проекта](https://github.com/fish-shell/fish-shell)
- [Репозиторий omf](https://github.com/oh-my-fish/oh-my-fish)

C **fish** последовательность действий более сложная, так как помимо его установки так же потребуется его настроить.

1. Установка

```bash
sudo apt install fish
curl https://raw.githubusercontent.com/oh-my-fish/oh-my-fish/master/bin/install | fish

```

2. Конфигурирование

Установка темы через **oh-my-fish**

```bash
omf install simple-ass-prompt
```

Для конфигурации можно воспользоваться моим конфигом, который нужно будет расположить в директории `~/.config/fish/config.fish`.

Так же в нем нужно будет включить vi mode, для того, чтобы можно было пользоваться сочетаниями клавиш **vim**

```bash
fish_vi_key_bindings
```

3. Настройка псевдонимов

- Установить псевдоним для **exa** на ls
- Установить neofetch на старт шелла
- Прописать старт zoxide в fish

4. Установка шелла по умолчанию

```bash
chsh -s $(which fish)
```


# Dotnet

По установке **dotnet** в линукс у меня уже есть подробный гайд, который расписан в гайде по настойке **neovim**

- [Гайд по установке](https://github.com/Mefioculus/guides/blob/master/setting-up-nvim.md)

# Neovim

- [Репозиторий проекта](https://github.com/neovim/neovim)
- [Гайд по установке](https://github.com/Mefioculus/guides/blob/master/setting-up-nvim.md)

# Rust

По установке **rust** в линукс у меня уже есть подробный гайд, который расписан в гайде по настойке **neovim**

- [Гайд по установке](https://github.com/Mefioculus/guides/blob/master/setting-up-nvim.md)


