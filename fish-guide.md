# Введение

Так как **fish** имеет крайне сильные отличия от bash, я решил записывать сюда все решения проблем, с которыми я буду сталкиваться на протяжении своего программисткого пути

# Работа с переменными окружения

[Данный гайд подсмотрен на данной странице StackOverflow](https://stackoverflow.com/questions/26208231/modifying-path-with-fish-shell)

Если в bash, когда мы хотим добавить какую либо переменную в путь, мы могли прописать это в файле **bashrc**, с fish такая штука не пройдет.
Пути, которые мы добавляем дополнительно находятся в специальное переменной fish, которая называется `$fish_user_paths`.

```bash
# Для того, чтобы посмотреть, что находится в данной переменной, можно воспользоваться следующей командой:
echo $fish_user_paths | tr " " "\n" | nl
#Эта команда отобразит все добавленные ранее пути (в виде пронумерованного списка)

#Удаление пути, добавленного по ошибке
set --erase fish_user_paths[1] # Где 1 - номер в списке

#Добавление пути
set -U fish_user_paths /usr/local/bin $fish_user_paths

#Добавление переменной
set -xU NameOfVariable ValueOfVariable
```

# Добавление псевдонима

Здесь так же есть большие отличия от bash, так как псевдонимы в fish - это функции, которые нужно писать.
Ну или по крайней мере позволить за тебя написать специальной утилите - alias, которая отличается в fish относительно той же утилиты в bash.

Для того, чтобы добавить новый alias, потребуется сделать следующее:

```bash
alias -s ls="exa -lh"
```

Ключ -s в данном примере неспроста, так как без него текущий псевдоним сбросится при следующем входе в терминал.
Данный ключ же создает функцию в директории `~/.config/fish/functions/`, которая в дальнейшем будет вгружаться при запуске.
Так же в этой директории хранятся файлы тем.

**UPD**

Как выяснилось чуть позже, псевдонимы так же можно задавать классическим способом через файл конфига `config.fish`.
Путь к этому файлу должен находиться в директории `~/.config/fish/` (так же этот путь можно посмотреть при помощи утилиты **env** в переменной **OMF_CONFIG**.
Причем если alias был сделан через команду, а так же через конфиг файл - приоритет будет отдан конфиг файлу (alias будет перезаписан)

# Изменение темы

Свою тему (к примеру цветовую схему) можно применить через файл `config.fish`.
Параметры prompt должны быть определены в специальной функции с названием `fish_prompt`.

- [Информация про prompt](https://fishshell.com/docs/current/interactive.html#prompt)

> When fish waits for input, it will display a prompt by evaluating the [fish_prompt](https://fishshell.com/docs/current/cmds/fish_prompt.html#cmd-fish-prompt) and [fish_right_prompt](https://fishshell.com/docs/current/cmds/fish_right_prompt.html#cmd-fish-right-prompt) functions.
> The output of the former is displayed on the left and the latter's output on the right side of the terminal.
> The output of [fish_mode_prompt](https://fishshell.com/docs/current/cmds/fish_mode_prompt.html#cmd-fish-mode-prompt) will be prepended on the left, though the default function only does this when in [vi-mode](https://fishshell.com/docs/current/interactive.html#vi-mode).

Из выдержки выше можно почерпнуть всю информацию
