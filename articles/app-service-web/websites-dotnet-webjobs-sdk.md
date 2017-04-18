<properties 
    pageTitle="Co to jest Azure WebJobs SDK" 
    description="Wprowadzenie do zestawu SDK WebJobs Azure. W tym miejscu wyjaśniono, co zestawu SDK jest, typowe scenariusze są przydatne w przypadku i kod próbki." 
    services="app-service\web, storage" 
    documentationCenter=".net" 
    authors="tdykstra" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="06/01/2016" 
    ms.author="tdykstra"/>

# <a name="what-is-the-azure-webjobs-sdk"></a>Co to jest Azure WebJobs SDK

## <a id="overview"></a>Omówienie

W tym artykule wyjaśniono, co to jest zestaw SDK WebJobs, monitoruje kilka typowych scenariuszy, są przydatne w przypadku i zestawienie jak z niej korzystać w kodzie.

[WebJobs](websites-webjobs-resources.md) to funkcja usługi aplikacji Azure, która umożliwia uruchomienie programu lub skryptu w tym samym kontekście jako aplikacji sieci web, interfejs API aplikacji lub aplikacji dla urządzeń przenośnych. Przedmiotem [WebJobs SDK](websites-webjobs-resources.md) jest w celu uproszczenia tworzonego umożliwiającą wykonywanie typowych zadań, że WebJob można wykonać, takie jak przetwarzanie obrazu, w kolejce przetwarzania kodu, RSS agregacji konserwacja plików i wysyłanie wiadomości e-mail. WebJobs SDK zawiera wbudowane funkcje do pracy z magazyn Azure i Bus usługi, planowanie zadań i obsługi błędów i wiele innych typowych scenariuszy. Ponadto został zaprojektowany tak być extensible. [WebJobs SDK jest Otwórz źródło](https://github.com/Azure/azure-webjobs-sdk/), i jest [Otwórz źródło repozytorium rozszerzenia](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview).

WebJobs SDK zawiera następujące składniki:

* **Pakiety NuGet**. Pakiety NuGet, które możesz dodać do projektu programu Visual Studio aplikacji konsoli zapewniają ramy używanego w kodzie przez dekoracji Twoje metody z atrybutami WebJobs SDK.
  
* **Pulpit nawigacyjny**. Część zestawu SDK WebJobs znajduje się w usłudze Azure aplikacji i przewiduje programy korzystające z pakietów NuGet sformatowanego monitorowania i diagnostyki. Nie trzeba napisać kod, aby korzystać z tych funkcji monitorowania i diagnostyki.

## <a id="scenarios"></a>Scenariusze

Oto kilka typowych scenariuszy, które można łatwiej obsługiwać z zestawu SDK WebJobs Azure:

* Obraz pracy obciążenie Procesora przetwarzania lub innych. Typowe funkcji witryn sieci Web jest możliwość Przekaż obrazy lub klipy wideo. Często mają do manipulowania zawartości po zostało przesłane, ale nie chcesz, aby wprowadzić Zaczekaj, aż do użytkownika.

* Przetwarzanie kolejki. Typowa dla serwera sieci Web w sieci web można komunikować się z usługą wewnętrznej bazy danych jest użycie kolejek. Jeśli witryny sieci Web musi Ci w pracy, umieszcza wiadomości na kolejkę. Usługa wewnętrznej bazy danych pobiera wiadomości z kolejki i działa. Można użyć kolejki przetwarzania obrazów: na przykład, gdy użytkownik wysyła liczbę plików, umieść nazwy plików w kolejce wiadomości w celu pobrania przez wewnętrznej bazy danych do przetwarzania. Można także użyć kolejkach Aby zwiększyć czas reakcji witryny. Na przykład zamiast wpisywać tekst bezpośrednio do bazy danych SQL, pisanie do kolejki, przekazać użytkownikowi, który będzie już gotowe, i umożliwić wewnętrznej bazy danych usługi uchwyt wysokie opóźnienia relacyjnej bazy danych pracy. Na przykład kolejki przetwarzania z procesem obraz zobacz [Samouczek WebJobs SDK rozpocząć pracę](websites-dotnet-webjobs-sdk-get-started.md).

* Agregacji RSS. Jeśli witryna przechowuje listę źródeł danych RSS, można pobierać we wszystkich tych artykułów ze źródeł danych w tle.

* Konserwacja plików, takie jak agregowania lub czyszczenie plików dziennika.  Może być tworzona przez kilka witryn lub dla zakresów razem, które chcesz połączyć w celu uruchomienia zadań analizy na ich pliki dziennika. Możesz też zaplanować zadania tygodniowy czyszczenie starych plików dziennika.

* Wchodzącego do tabel Azure. Może być pliki przechowywane i obiektów blob i chcesz przeanalizować ich i przechowywania danych w tabelach. Funkcja ingress może pisania dużą liczbą wierszy (miliony w niektórych przypadkach), a WebJobs SDK pozwala na łatwe zaimplementować tę funkcję. Zestaw SDK udostępnia w czasie rzeczywistym monitorowania wskaźniki postępu, takie jak liczba wierszy w tabeli.

* Inne zadania długotrwałe, które chcesz uruchomić w tle wątku, takie jak [wysłanie wiadomości e-mail](https://github.com/victorhurdugaci/AzureWebJobsSamples/tree/master/SendEmailOnFailure). 

* Wszystkie zadania, które chcesz uruchomić zgodnie z harmonogramem, takie jak wykonywania operacji tworzenia kopii zapasowych każdej nocy.

Wiele z tych scenariuszy można skalować aplikacji sieci web do uruchomienia na wielu maszyny wirtualne, które można uruchomić jednocześnie wiele WebJobs. W niektórych przypadkach może to spowodować te same dane przetwarzane wielokrotnie, ale nie jest to problem podczas korzystania z wbudowanych kolejki, obiektów blob i Bus usługi wyzwalaczy WebJobs SDK. Zestaw SDK gwarantuje, że funkcje będą przetwarzane tylko raz dla każdej wiadomości lub blob.

WebJobs SDK również ułatwia obsługę obsługi scenariusze dotyczące typowych błędów. Można ustawić alerty wysyłane powiadomienia, gdy funkcja kończy się niepowodzeniem, a następnie limity czasu można ustawić tak, aby funkcji automatycznie jest anulowany, jeśli nie zakończyła się w ciągu określonego przedziału czasu.

## <a id="code"></a>Przykłady kodu

Kod do obsługi typowych zadań, które współpracują z magazyn Azure jest proste. W aplikacji konsoli `Main` metody tworzenia `JobHost` obiekt, który współrzędne połączeń metod pisania. Framework WebJobs SDK wie, kiedy nawiązać połączenie z metod i co wartości parametru do używania na podstawie atrybutów WebJobs SDK używanych w nich. Zestaw SDK udostępnia *wyzwalaczy* Określ, co funkcja ma być wywoływana powodować warunków i *skoroszytach* Określ sposób uzyskać informacje o do i wylogowywanie się z parametrów metod.

Na przykład atrybut [QueueTrigger](websites-dotnet-webjobs-sdk-storage-queues-how-to.md) powoduje, że funkcja ma być wywoływana po otrzymaniu wiadomości w kolejce, a jeśli wiadomość jest w formacie JSON dla tablicy bajtów lub typ niestandardowy, wiadomość jest automatycznie rozszeregowanym. Atrybut [BlobTrigger](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md) uruchamia proces przy każdym nowych obiektów blob zostanie utworzona przy użyciu konta z miejsca do magazynowania Azure.

Oto prosty program, który sprawdza kolejki i tworzy obiektów blob w przypadku każdej odebranej wiadomości kolejki:

        public static void Main()
        {
            JobHost host = new JobHost();
            host.RunAndBlock();
        }

        public static void ProcessQueueMessage([QueueTrigger("webjobsqueue")] string inputText, 
            [Blob("containername/blobname")]TextWriter writer)
        {
            writer.WriteLine(inputText);
        }

`JobHost` Obiekt jest kontener zestaw funkcji tła. `JobHost` Obiekt monitoruje funkcji zegarki dla zdarzeń wywołujących je i wykonuje zadania po wystąpieniu zdarzenia wyzwalacza. Połączenia `JobHost` sposobem wskazuje, czy Proces kontenera do działania na bieżący wątek lub wątek w tle. W tym przykładzie `RunAndBlock` metody uruchamia proces stale w bieżącym wątku.

Ponieważ `ProcessQueueMessage` metoda w tym przykładzie ma `QueueTrigger` atrybutów wyzwalacza dla tej funkcji jest otrzymaniu nowej wiadomości w kolejce. `JobHost` Obiekt oczekuje na nowe wiadomości w kolejce na określonej kolejki ("webjobsqueue" w tym przykładzie) i kiedy zostanie znalezione, wywołuje `ProcessQueueMessage`. 

`QueueTrigger` Atrybutów wiązania `inputText` parametr na wartość kolejki wiadomości. I `Blob` atrybutów wiązania `TextWriter` obiekt blob o nazwie "blobname" w kontenerze o nazwie "Nazwa_kontenera".  

        public static void ProcessQueueMessage([QueueTrigger("webjobsqueue")]] string inputText, 
            [Blob("containername/blobname")]TextWriter writer)

Funkcja następnie korzysta z tych parametrów do zapisu wartość wiadomości kolejki to:

        writer.WriteLine(inputText);

Funkcje wyzwalacza i spinacza WebJobs SDK znacznie uprościć kod, który trzeba napisać. Kod niższego poziomu wymagane przetwarzania kolejek, obiektów blob lub plików lub aby zainicjować zaplanowanych zadań, jest zrobiona Framework WebJobs SDK. Na przykład ramach tworzy kolejki, które nie istnieje jeszcze otwarty kolejki odczytuje kolejki wiadomości, usuwa kolejki wiadomości, gdy przetwarzanie zostanie zakończone, tworzy kontenerów obiektów blob, które nie są zachowywane, ale zapisuje obiektów blob i tak dalej.

W poniższym przykładzie pokazano różne wyzwalaczy w jednym WebJob: `QueueTrigger`, `FileTrigger`, `WebHookTrigger`, i `ErrorTrigger`. 

```
    public class Functions
    {
        public static void ProcessQueueMessage([QueueTrigger("queue")] string message,
        TextWriter log)
        {
            log.WriteLine(message);
        }

        public static void ProcessFileAndUploadToBlob(
            [FileTrigger(@"import\{name}", "*.*", autoDelete: true)] Stream file,
            [Blob(@"processed/{name}", FileAccess.Write)] Stream output,
            string name,
            TextWriter log)
        {
            output = file;
            file.Close();
            log.WriteLine(string.Format("Processed input file '{0}'!", name));
        }

        [Singleton]
        public static void ProcessWebHookA([WebHookTrigger] string body, TextWriter log)
        {
            log.WriteLine(string.Format("WebHookA invoked! Body: {0}", body));
        }

        public static void ProcessGitHubWebHook([WebHookTrigger] string body, TextWriter log)
        {
            dynamic issueEvent = JObject.Parse(body);
            log.WriteLine(string.Format("GitHub WebHook invoked! ('{0}', '{1}')",
                issueEvent.issue.title, issueEvent.action));
        }

        public static void ErrorMonitor(
        [ErrorTrigger("00:01:00", 1)] TraceFilter filter, TextWriter log,
        [SendGrid(
            To = "admin@emailaddress.com",
            Subject = "Error!")]
         SendGridMessage message)
        {
            // log last 5 detailed errors to the Dashboard
            log.WriteLine(filter.GetDetailedMessage(5));
            message.Text = filter.GetDetailedMessage(1);
        }
    }
```

## <a id="schedule"></a>Planowanie

`TimerTrigger` Atrybut zapewnia możliwości do funkcji wyzwalacza zgodnie z harmonogramem. Można zaplanować WebJob całej Azure lub Planowanie poszczególnych funkcji WebJob przy użyciu zestawu SDK WebJobs `TimerTrigger`. Poniżej przedstawiono przykładowy kod.

```
public class Functions
{
    public static void ProcessTimer([TimerTrigger("*/15 * * * * *", RunOnStartup = true)]
    TimerInfo info, [Queue("queue")] out string message)
    {
        message = info.FormatNextOccurrences(1);
    }
}
```

Więcej przykładowy kod zobacz [TimerSamples.cs](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/ExtensionsSample/Samples/TimerSamples.cs) w repozytorium azure-webjobs-sdk rozszerzenia na GitHub.com.

## <a name="extensibility"></a>Rozszerzanie

Nie jest ograniczone do wbudowanej funkcji — WebJobs SDK umożliwia pisanie niestandardowych wyzwalaczami i skoroszytach.  Na przykład można napisać Wyzwalacze dla zdarzenia pamięci podręcznej i harmonogramy okresowych. [Otwórz repozytorium źródła](https://github.com/Azure/azure-webjobs-sdk-extensions) zawiera [szczegółowe Przewodnik po rozszerzeń WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview) i przykładowe kod ułatwiające rozpoczęcie pracy tworzeniu własnych wyzwalaczami i skoroszytach.

## <a id="workerrole"></a>Przy użyciu zestawu SDK WebJobs poza WebJobs

Program, który używa zestawu SDK WebJobs jest standardowej aplikacji konsoli oraz można uruchamiać w dowolnym miejscu — nie ma być uruchamiany WebJob. Testowanie programu lokalnie na komputerze dewelopera, a produkcji możesz uruchomić ją w roli Pracownik usługi w chmurze lub usługi Windows Jeśli wolisz używać jednej z tych środowiskach. 

Jednak pulpitu nawigacyjnego jest dostępna tylko jako rozszerzenie Azure aplikacji usługi aplikacji sieci web. Jeśli chcesz uruchomić poza WebJob i nadal korzystać z pulpitu nawigacyjnego, można skonfigurować aplikację sieci web, aby używać tego samego konta miejsca do magazynowania ciąg połączenia pulpitu nawigacyjnego SDK WebJobs odwołujące się do i pulpit nawigacyjny WebJobs aplikacji sieci web spowoduje wyświetlenie danych o wykonanie funkcji z uruchomionym programem gdzieś program. Zostanie wyświetlony na pulpicie nawigacyjnym przy użyciu.scm.azurewebsites.net/azurejobs/#/functions*{webappname}*https:// adresu URL. Aby uzyskać więcej informacji zobacz [Uzyskiwanie pulpitu nawigacyjnego rozwoju lokalne z zestawu SDK WebJobs](http://blogs.msdn.com/b/jmstall/archive/2014/01/27/getting-a-dashboard-for-local-development-with-the-webjobs-sdk.aspx), ale należy zauważyć, że wpis w blogu przedstawia starymi nazwami parametry połączenia. 

## <a id="nostorage"></a>Funkcje pulpitu nawigacyjnego

WebJobs SDK ma wiele zalet, nawet jeśli nie korzystasz z wyzwalaczami WebJobs SDK lub skoroszytach:

* Można wywołać funkcje z pulpitu nawigacyjnego.
* Czy Odtwórz funkcje z pulpitu nawigacyjnego.
* Można wyświetlić dzienniki na pulpicie nawigacyjnym połączone z określonego WebJob (Dzienniki aplikacji napisane przy użyciu Console.Out, Console.Error, śledzenia itp.) lub połączone z wywołania określoną funkcję, który wygenerował je (Dzienniki zapisane przy użyciu `TextWriter` obiekt, który zestawu SDK przekazuje do funkcji jako parametr). 

Aby uzyskać więcej informacji zobacz [jak ręcznie wywołania funkcji](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#manual) oraz [jak zapisać dzienniki](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#logs) 

## <a id="nextsteps"></a>Następne kroki

Aby uzyskać więcej informacji na temat zestawu SDK WebJobs zobacz [Azure WebJobs zalecane zasoby](http://go.microsoft.com/fwlink/?linkid=390226).

Aby uzyskać informacji na temat najnowszej ulepszenia WebJobs SDK zobacz [Informacje o wersji](https://github.com/Azure/azure-webjobs-sdk/wiki/Release-Notes).
 
