# Введение

Изначально тема поиска и сопоставления в программировании крайне важная и распространенна, потому что на самом деле на этом, наверное, все программирование в той или иной степени и строится.
И для поиска и сопоставления строк есть на самом деле большое количество встроенных в каждый язык программирования методов.
Но при этом есть некий инструмент, некая серебрянная пуля, которая может поставить точку в вопросах данной задачи, так как этот инструмент позволяет делать все, что только возможно в этом направлении.
И имя этому инструменту - **Регулярные выражения**

Что это такое?
По сути это свой небольшой язык, который позволяет описать поисковый запрос, а вернее некий паттерн, с которым в дальшейшем можно сравнивать строку и получать ответ, есть ли совпадение или нет.
Главное преимущество регулярных выражений перед, скажем, встроенными функциями в языки программирования (я имею ввиду обычные методы, которые не построены на базе регулярок) - это то, что по сути условие может позволять некоторые вольности.
К примеру мы можем просто указать правила, которым должна соответствовать строка, а все остальное за тебя сделает компьютер.
Главная проблема регулярных выражений в сравнении с стандартным функционалом - они работает несколько медленнее, так что не во всех случаях резонно их использовать.
Ну а вторая по важности проблема - в том, что это по сути отдельный язык, в котором нужно разбираться, и который, желательно, использовать в своей практике почаще, для того чтобы он не забывался.

Я уже пробовал погружаться в данный вопрос, и даже писал несколько таких запросов, но, каюсь, мне всегда было проще написать что-то самописное с применением стандартных методов языка, нежели использовать такой развесистый инструмент.
Но сейчас я начинаю думать, что пора уже получше познакомиться с данным инструментом, чтобы обладать большей свободой в процессе написания программ.

## Полезные ссылки

- [Professor WEB](https://professorweb.ru/my/csharp/charp_theory/level4/4_10.php)
- [MSDN](https://docs.microsoft.com/en-us/dotnet/standard/base-types/regular-expression-language-quick-reference)
- [Metanit](https://metanit.com/sharp/tutorial/7.4.php)
- [Онлайн тестер регулярок](https://tools.icoder.uz/regex-tester.php)

# Реализация Regex в Csharp

Регулярные выражения наверное появились даже до появления самого C# (мое предположение, не факт), так что по сути то, что применяется в платформе dotnet - уже частная реализация данного языка, которая может обладать своими особенностями в исполнении.
Так же как и markdown может немного отличаться в зависимости от движка, который рендерит его в финальный результат.

Реализация **Regex** находится в пространстве имен **System.Text.RegularExpressions**, а оснавной класс в этом пространстве - **Regex**


# Синтаксис языка

## Кратко

Для начала постараюсь описать все кратко (а будет ли длинно - мы еще посмотрим), просто для того, чтобы рассказать самую суть, самые важные штуки, без которых сложнее всего врубиться в данный язык.

### Самое простое Regex выражение

Начнем с того, что любой текст - это валидный паттерн для поиска при помощи регулярных выражений.
Просто стоит учитывать тот факт, что если мы ввели конкретный текст - именно он искаться и будет.
В этом плане регулярка по сути не будет отличаться от стандартного поискового запроса.

Основные трудности же начинаются когда мы хотим, к примеру, найти все вариации этого слова.
Конечно, можно сделать несколько запросов, потом результаты каким-то образом объединить, но зачем этим всем морочиться, если по сути для этого и был придуман язык регуляных выражений?

Типичное регулярное выражение состоит из текста, который мы ищем, вперемешку с управляющими символами, которые указывают, какое количество повторов может быть, указывает опциональные части, которые могут быть, а могут и отсутствовать, а так же указание вариаций отдельных элеметов.

### Основные символы

Символы \*, ?, + являются так сказать операторами, отвечающими за количество.
Они проставляются после какого-то элемента (это может быть как символ, так и группа символов), и обозначают, какое количество этих символов может быть.
Астерикс (\*) обозначает, что элемет может встретиться от нуля до неограниченного количества раз, знак плюс (+) - от одного раза до неограниченного количества, а знак вопроса - один раз или ноль раз.

Мне кажется, что хотя бы один из этих символов всегда присутствует в регулярном выражении.

Так же есть фигурные скобочки, которые позволяют более точно выбирать количество повторов данного элемента.

На самом деле таких операторов значительно больше, но для написания регулярного выражение знание их всех может не понадобиться.
Но обязательно понадобится, когда придется читать чужое регулярное выражение, потому что никогда не знаешь, какие конструкции может знать другой человек, и какие из них ему удобнее всего применять.

### Скобочки

В данном языке много скобочек.
Я предполагаю, что в нем используются все известные человечеству скобочки, причем в некоторых регулярных выражениях они встречаются в таком количестве, что даже тошно становится.
Именно поэтому стоит про них так же рассказать

Для указания, что данный элемент может присутствовать в различных вариациях, используются квадратные скорочки
К примеру, для того, чтобы составить поисковый запрос, который найдет 200 и 300 в тексте, нужно будет прописать следующее

```
[23]00
```
Стоит отметить, что по сути каждый символ, который находится в квадратных скобочках рассматривается как вариат, кроме тире (-), так как этот символ может задавать перечисление для определенных символов.

Фигурные скорочки, как уже упоминалось выше, используются для указания количества повторений элемента, за которым они следуют

### Экранирование

Так как по сути любое регулярное выражение, которое мы пишем представляет собой смесь текста запроса с различными управляющими символами, которые дополняют запрос, то что же делать, если наш текст запроса должен включать такой специальный символ?
Для этого испльзуется символ экранирования - backslash (\\), который по сути позволяет использовать символы, которые могли быть по умолчанию распознаваться текстом запроса как управляющие символы, и наоборот управляющий символ мог стать текстом запроса.

Тимичный пример - символ точки (.) который в языке регулярных выражений обозначает любой символ в одном экземпляре.
Но если же мы хотим, чтобы в поисковом запросе была именно точка, мы должны этот символ экранировать, после чего он из управляющего превращается в текст запроса.

```
Экранирование точки в запросе
\.

Более сложный пример запроса с использованием экранированной точки
(2)
```

