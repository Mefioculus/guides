# Введение

Данное руководство было создано по причине того, что мне по работе потребовалось в автоматическом режиме ужимать pdf файлы при загрузке их на сервер.
Так как пользователи по большей части не думают по поводу размера файла при сканировании документации, зачастую они выставляют при сканировании очень большие значения **dpi**, что в итоге приводит к очень большому размеру файла (при том, что без потери качества изображения можно его сильно ужать).
Так как работа с этими **PDF** файлами планируется вестись из **DOCs**, имеет смысл написать макрос, который будет срабатывать при нажатии на кнопку "Загрузить pdf" и при превышении размера проводить различные оптимизации.

Главная проблема, с которой я столкнулся, когда начал искать решение в интернете - оказалось, что многие из подобных библиотек распространяются по лицензии, которая не позволила бы мне использовать их в своей работе.
Но все же удалось на **StackOverlow** найти пост с библиотекой, которая распространяется по **MIT** лицензии (и так же в том ответе присутствовал пример кода, который сразу можно было бы переложить в мой макрос для пробы.

# Варианты решения проблемы

## **PdfSharp**
- [Ссылка на полезный ответ на StackOverlow](https://stackoverflow.com/a/57386379)
- [PdfSharp](http://www.pdfsharp.net/)

```csharp
public static void CompressPdf(string targetPath) {

    using (var stream = new MemoryStream(File.ReadAllBytes(targetPath)) { Position = 0 } )
    using (var source = PdfReader.Open(stream, PdfDocumentOpenMode.Import))
    using (var document = new PdfDocument()) {

        var options = document.Options;
        options.FlateEncodeMode = PdfFlateEncodeMode.BestCompression;
        options.UseFlateDecoderForJpegImages = PdfUseFlateDecoderForJpegImages.Automatic;
        options.CompressContentStreams = true;
        options.NoCompression = false;

        foreach (var page in source.Pages) {
            document.AddPage(page);
        }

        document.Save(targetPath);
    }
}
```

## **GhostScript**

-[GhostScript official site](https://www.ghostscript.com/)
-[GhostScript PDF Tips](https://milan.kupcevic.net/ghostscript-ps-pdf/)

Данный вариант не совсем относится к **C#**, так как это просто утилита, но так как утилиты можно вызывать из кода **C#**, я думаю данный вариант вполне себе неплохой, особенно если учитывать то, что здесь можно сжимать файлы одной командой:

```bash
gs -dNOPAUSE -dQUIET -dBATCH -sDEVICE=pdfwrite -dPDFSETTINGS=/ebook -sOutputFile=output.pdf input.pdf
```

Установка приложения возможно через **apt**

```bash
sudo apt install ghostscript

# Получить короткую справку по возможностям применения
curl cheat.sh/gs
```

Самое интересное - вызов данного приложения из кода **C#**

[Пример кода для запуска приложения](https://victorz.ru/20170413476)

```csharp
using System.Diagnostics;

Process iStartProcess = new Process(); // Новый процесс
iStartProcess.StartInfo.FileName = @"C:\program.exe"; // Путь к исполняемому файлу
iStartProcess.StartInfo.Arguments = " -i 192.198.10.12 -p 10568"; // Эта строка указывается , если программа запускается с параметрами
iStartProcess.StartInfo.WindowStyle = ProcessWindowStyle.Hidden; // Это строку указываем, если хотим запустить программу в скрытом виде
iStartProcess.StartInfo.WorkingDirectory = @"Path/To/Working/Directory"; // В том примере данный параметр не был задан
// Но в комментариях было сказано, что не все приложения смогут так запуститься
iStartProcess.Start(); // Запускаем программу
iStartProcess.WaitForExit(120000); // эту строку указываем, если нам надо будет ждать завершения программы определенное время,
// пример - 2 минуты в миллисекундах
```
