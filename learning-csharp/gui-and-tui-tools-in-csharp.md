# Пользовательские интерфейсы

## Введение

Пользовательские интерфейсы для меня всегда были большой проблемой, так как никогда не было ясности в том, каким инструментом пользоваться для того, чтобы реализовать пользовательский интерфейс для своего приложение.
Более того, обстоятельства сложились таким образом, что мне и не приходилось большую часть времени заниматься самостоятельным построением пользовательских интерфейсов, так как по большей части я либо занимался бэкэндом, либо пользовался встроенными механизмами, которые предоставляли легконастраиваемые стандартные элементы.
Плюсы у этого, несомненно, были, так как я мог полностью сконцентрироваться на одной области приложения программирования.
Но так же и минусы у этого есть, так как на данный момент опыта в построении пользовательских интерфейсов крайне мало, а эта область очень важная при построении приложения с нуля.

Еще одна проблема, как мне кажется - это выбор языка, так как платформа **dotnet** долгое время разрабатывалась исключительно под **Windows**, большая часть уже сложившихся инструментов для построения графических пользовательских интерфейсов не работают в мультиплатформе.
Хотя, возможно, это проблема вообще общая (у других языков возможно вообще нет такого различных библиотек для графических пользовательских интерфейсов).

В данной заметке я бы хотел перечислить те инструменты, которые у меня получится найти, дать различные полезные ссылки для изучения каждого из них, а по прошествии времени дополнять информацию своими заметками к данным инструментам после знакомства с ним.

## GUI tools

Классический вариант взаимодействия с пользователем, так как работа с консолью от большинства пользователей очень далека.
Следовательно, если разрабатываемое приложение рассчитано на широкий круг пользователей, реализовывать графический интерфейс является не рекомендацией, а необходимостью.
Сначала я постараюсь перечислить фрэймворки, которые относятся к платформе **dotnet**, а затем сторонние разработки.

### Forms

#### Ссылки

- [Официальная документация](https://learn.microsoft.com/ru-ru/dotnet/desktop/winforms/?view=netdesktop-7.0)
- [Курс по Forms на Metanit](https://metanit.com/sharp/windowsforms/1.1.php)

#### Короткое описание

Классический вариант, с которым я знаком на достаточно уверенном уровне.
Простой, как пробка, так как не требует использования никаких других дополнительных инструментов.
Так же наверное является самым первым инструментом.
Одним из минусов является только то, что работает он только в **Windows**.

По данному инструменту особо не имеет смысла прикладывать большое количество ссылок, так как материалов по нему, мало того, что большое множество, так и для использования его достаточно простой документации и понимания самого языка C#. Так как язык не использует **XAML**, то проблем с изучением данной технологии не возникает совсем. Все крайне легко тестировать просто в блокноте, данный фреймворк не требует от пользователя разворачивания целой среды, работы с кучей файлов просто для того, чтобы открыть helloworld-окно.

#### Достоинства и недостатки

Плюсы:
- Большое количество материалов
- Простая для изучения структура
- Относительно малая база требуемых знаний для применения

Минусы:
- Работает только на Windows
- Не поддерживается
- Устаревший фрэймворк, использование которого не рекомендуется

### WPF

#### Ссылки
#### Короткое описание
Следующий после 

#### Достоинства и недостатки

### UWP
#### Ссылки
#### Короткое описание
#### Достоинства и недостатки

### MAUI
#### Ссылки
#### Короткое описание
#### Достоинства и недостатки

### Avalonia
#### Ссылки
#### Короткое описание
#### Достоинства и недостатки

### Electron
#### Ссылки
#### Короткое описание
#### Достоинства и недостатки

## TUI tools