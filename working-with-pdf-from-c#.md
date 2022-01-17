# Введение

Данное руководство было создано по причине того, что мне по работе потребовалось в автоматическом режиме ужимать pdf файлы при загрузке их на сервер.
Так как пользователи по большей части не думают по поводу размера файла при сканировании документации, зачастую они выставляют при сканировании очень большие значения **dpi**, что в итоге приводит к очень большому размеру файла (при том, что без потери качества изображения можно его сильно ужать).
Так как работа с этими **PDF** файлами планируется вестись из **DOCs**, имеет смысл написать макрос, который будет срабатывать при нажатии на кнопку "Загрузить pdf" и при превышении размера проводить различные оптимизации.

Главная проблема, с которой я столкнулся, когда начал искать решение в интернете - оказалось, что многие из подобных библиотек распространяются по лицензии, которая не позволила бы мне использовать их в своей работе.
Но все же удалось на **StackOverlow** найти пост с библиотекой, которая распространяется по **MIT** лицензии (и так же в том ответе присутствовал пример кода, который сразу можно было бы переложить в мой макрос для пробы.

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
