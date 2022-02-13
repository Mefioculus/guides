# Введение

Мне на **Medium** попалась интересная статья на глаза, в которой речь была о том, как улучшить паттерн фабрики при помощи техники Dependency Injection.
Данная тема уже давно интересует меня, так как я понимаю, что у меня определенно есть проблемы с пониманием таких понятий как паттерны программирования (и фабрика, и DI как раз относятся к паттернам).
Я понимаю, что для того, чтобы быть хорошим программистом, желательно начать применять данные подходы к разработке, ну или хотя бы понимать их.

По итогу как только я увидел знакомые слова в заголовке, мне сразу стало интересно, так как я почувствовал, что это может быть неплохой возможностью попробовать закрепить данные понятие на более хорошем уровне.
Правда, когда я начал читать статью, я понял, что написана она конечно не самым лучшим образом, но при этом сам материал в ней все равно полезный, так что я в какой-то момент просто решил попробовать переписать эту статью для себя в заметке, чтобы и разобраться в процессе получше, а так же оставить более понятный гайд.

# Паттерн фабрика (Factory pattern)

В данном паттерне на самом деле нет ничего сложного.
У нас есть разные возможности по созданию новых объектов, и самый простой метод - при помощи ключевого слова **new**.
Но это далеко не всегда на самом деле удобно, и для некоторых случаев придумывают дополнительный класс - Фабрику, единственной задачей которого является создание новых объектов.
По сути у данного класса будет один метод, **Create**, который в записимости от того, какой на его вход будет приходить параметр, будет возвращать различные новые объекты:

```csharp

// Создание объектов по классике
WordProcessor wp = new WordProcessor();
DbfProcessor dp = new DbfProcessor();
OdtProcessor op = new OdtProcessor();


// Создание объектов при помощи паттерна Фабрика
IDocumentProcessor wp = Factory.Create("wp");
IDocumentProcessor dp = Factory.Create("dp");
IDocumentProcessor op = Factory.Create("op");
```

На самом деле пример выше не очень наглядный, потому что на деле не очень понятно, чем же второй вариант более выгоден, чем первый? По вызовам все выглядит примерно похоже, но при этом для реализации паттерна фабрика нужно как минимум создать дополнительный класс и метод в нем, который будет заниматься созданием новых объектов.
Но польза данного паттерна возникает тогда, когда, допустим, создание объектов более сложный процесс, который, как раз и можно спрятать за простотой вызова метода **Create**.
Тут еще стоит отметить тот факт, что не смотря на то, что мы возвращаем разные объекты, все они приводятся к единому интерфейсу, от которого каждый предварительно должен быть унаследован.
Таким образом мы сможем универсально воспользоваться любым обработчиком, не задумываясь о той имплементации, которая задумана там под капотом.

Ниже я приложу код самой фабрики, чтобы было понятнее, что фабрика представляет из себя под капотом.
Сам код придумал не я, но я добавлю своих комментариев, а так же может быть несколько модифицирую его для понятности.

```csharp
public interface IDocumentProcessor {}

public class PdfProcessor : IDocumentProcessor {}
public class WordProcessor : IDocumentProcessor {}
public class OdtProcessor : IDocumentProcessor {}

public interface IDocumentProcessorFactory {
    IDocumentProcessor Create(string type);
}

public class DocumentProcessorFactory : IDocumentProcessorFactory {
    public IDocumentProcessor Create(string type) {
        return type switch {
            "pdf" => new PdfProcessor(),
            "odt" => new OdtProcessor(),
            "doc" => new WordProcessor(),
            _ => throw new Exception()
        };
    }
}
```

Теперь попробую объяснить код, который написан выше.
Изначально мы имеем три разных класса - обработчика различных форматов, которые наследуются от одного интерфейса.
В данном случае наследование от одного интерфейса нужно не только для того, чтобы у них были одинаковые методы, но так же и для того, чтобы можно было одним методом фабрики возвращать объекты разных по сути типов.

Так же в этом коде меня особенно удивило использование оператора **switch**, так как в таком виде я его еще не встречал.

# Depencency Injection

Пример, показанный выше впринципе уже достаточно удобный, но что делать, если наши классы обработчики зависят от других классов (то есть требуют их на вход в качестве аргументов конструктора)?
Конечно, в таком случае эти объекты можно либо создавать в самой фабрике (что только повышает связанность кода между собой), либо передавать их через конструктор в фабрику (тоже производит связанный код), а можно попробовать воспользоваться паттерном **Dependency injection**

Исходные данные (добавим зависимостей):

```csharp

// Дополняем класс WordProcessor зависимостью WordReader
public class WordProcessor : IDocumentProcessor {
    
    private readonly IWordReader wordReader;

    public WordProcessor(IWordReader wordReader) {
        this.wordReader = wordReader;
    }

    public string Process() {
        return string.Format("Process {0}", wordReader.GetWordData());
    }
}

public WordReader : IWordReader {
    publis string GetWordData() {
        return "Word Reader Provider";
    }
}

public interface IWordReader {
    string GetWordData();
}

// Тоже самое делаем для класса OdtProcessor
public class OdtProcessor : IDocumentProcessor {
    
    private readonly IOdtReader odtReader;

    public OdtProcessor(IOdtReader odtReader) {
        this.OdtReader = odtReader;
    }

    public string Process() {
        return string.Format("Process {0}", odtReader.GetWordData());
    }
}

public OdtReader : IOdtReader {
    publis string GetOdtData() {
        return "Odt Reader Provider";
    }
}

public interface IOdtReader {
    string GetOdtData();
}

// И повторяем для аналогичные дополнения для последнего класса
public class PdfProcessor : IDocumentProcessor {
    
    private readonly IPdfReader pdfReader;

    public PdfProcessor(IPdfReader pdfReader) {
        this.PdfReader = pdfReader;
    }

    public string Process() {
        return string.Format("Process {0}", pdfReader.GetWordData());
    }
}

public PdfReader : IPdfReader {
    publis string GetPdfData() {
        return "Pdf Reader Provider";
    }
}

public interface IPdfReader {
    string GetPfdData();
}
```

Как я уже говорил, если не использовать инверсию зависимостей, то данный паттерн можно реализовать через фабрику, правда в этом случае она будет зависима от данных дополнительных классов (Reader'ов)

```csharp
public class DocumentProcessorFactory : IDocumentProcessorFactory {
    private readonly IPdfReader pdfReader;
    private readonly IWordReader wordReader;
    private readonly IOdtReader odtReader;

    public DocumentProcessorFactory(IPdfReader pdfReader, IWordReader wordReader, IOdtReader odtReader) {
        this.pdfReader = pdfReader;
        this.wordReader = wordReader;
        this.odtReader = odtReader;
    }

    public IDocumentProcessor Create(string type) {
        return type switch {
            "pdf" => new PdfProcessor(pdfReader),
            "doc" => new WordProcessor(pdfReader),
            "odt" => new OdtProcessor(pdfReader),
            _ => throw new Exception();
        };
    }
}
```

Данный код по сути не слишком сильно отличается от кода, который был дан с самого начала, появились только дополнительные поля в классе фабрике, и теперь конструктор данного класса требует передачи в него зависимостей.
Так же сразу видно, что данный класс теперь зависим от классов ридеров.

А теперь давайте посмотрим, что можно сделать с классом фабрикой, чтобы он стал работать через Dependency Injection:

```csharp
public class DocProcessorFactoryWithDI : IDocumentProcessorFactory {

    // Основной момент - словарь, который будет хранить функции по созданию объектов
    private readonly IDictionary<string, Func<IDocumentProcessor>> _factories;

    public DocProcessorFactoryWithDI(IDictionary<string, Func<IDocumentProcessor>> factories) {
        _factories = factories;
    }

    public IDocumentProcessor  Create(string type) { if (!_factories.TryGetValue(type, out var factory) || factory is null)
            throw new Exception();
        return factory();
    }
}
```

И остается приложить код, где мы производим создание колекции сервисов и инициируем фабрику:

```csharp
var factories = new Dictionary<string , Func<IDocumentProcessor>>() {
    ["pdf"] = () => new PdfProcessor(new PdfReader()),
    ["doc"] = () => new WordProcessor(new WordReader()),
    ["odt"] = () => new OdtProcessor(new OdtReader())
};

var docProcessorFactory = new DocProcessorFactoryWithDI(factories);
```

Как видно, теперь мы можем просто добавлять новые объекты в словарь, и при этом можем больше не трогать реализацию самой фабрики, так как она теперь не зависит от того, что в нее передано
