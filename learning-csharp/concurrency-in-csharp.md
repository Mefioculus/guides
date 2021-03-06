# Введение

Данная тема меня уже довольно долгое время как притягивает, так и отталкивает, потому что не смотря на то, сколько я уже прочитал по этой теме, она все еще кажется мне крайне сложной и непонятной.
Параллельное вычисление - крайне запутанная тема, особенно учитывая то, что языки программирования изначально все синхронные.
Вобщем. сколько бы я не читал, вопрос все равно оставался туманным, а у меня еще пока не было такой задачи, на которой я бы мог опробовать все это на практике.
А, как уже давно известно, две совершенно разные вещи - попробовать все это сделать самому, или просто переписать то, что было написано в учебнике (заведомо правильно).

# Параллельное копирование файлов

Данная задача часто выполняется параллельно просто потому что данная задача занимате достаточно много процессорного времени, и если она будет работать синхронно, тогда она будет работать только на одном ядре процессора, что не очень выгодно.
Поэтому для подобной задачи использовать асинхронное выполнение метода копирование - самое то.
На самом деле данную задачу можно решить несолькими способами (это кстати и отпугивает в изучении данной темы), причем один из методов - воспользоваться уже написанным разработчиками асинхронным методом.
В таком случае даже не нужно какую-то особеную логику писать.
Я приведу этот код в пример здесь, но скорее для того, чтобы он не пропал и всегда можно было на него посмотреть.

```csharp
public async Task CopyFileAsync(string sourcePath, string destinationPath) {
    using (Stream source = File.Open(sourcePath)) {
        using (Stream destination = File.Create(destinationPath)) {
            await source.CopyToAsync(destination);
        }
    }
}
```

Данный метод использует асинхронный метод `CopyToAsync`, который уже присутствует в классе Stream.
Для того, чтобы его можно было в методе "ожидать", используется ключевое слово await, которое всегда работает совместно с объектом класса **Task**.
Так как метод асинхронный, предполагается, что он возвращает класс Task.
Так же стоит отметить, что так как мы использовали в коде метода ключевое слово `await`, мы обязаны так же в заголовке метода указать ключевое слово `async`, которое позволяет использовать ожидание.

Код, написанный мной для изучения асинхронного выполнения

```csharp
using System.Threading;
using System.Threading.Tasks;

void Main() {
    List<string> testTables = new List<string>() {"1", "2", "3"}; // Исходные данные, работа с которыми будет проводиться параллельно

    DownloadFiles(testTables); // Запуск обработки исходных даных

    Console.WriteLine("Дальнейшее выполнение метода Main");
}

public void DownloadFiles(List<string> tables) {
    List<Task> tasks = new List<Task>();

    foreach (string table in tables)
        task.Add(Task.Run(() => DownloadFiles(table)));

        Console.WriteLine("До ожидания выполнения всех потоков в методе DownloadFiles");

        Task.WaitAll(tasks.ToArray<Task>());

        Console.WreteLine("Дальнейшее выполнение метода DownloadFiles");
}

public async Task DownloadFiles(string path) {
    await Task.Delay(3000);
    Console.WriteLine("Произведено скачивание файла {0}", path);
    return;
}
```
