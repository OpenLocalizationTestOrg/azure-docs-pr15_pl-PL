<properties
    pageTitle="Stosowanie wielu .NET | Microsoft Azure"
    description="Samouczek .NET, który pomoże Ci opracowywania aplikacji wielu w Azure, której używa kolejek Bus usług do komunikacji między warstwy."
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="get-started-article"
    ms.date="09/01/2016"
    ms.author="sethm"/>

# <a name="net-multi-tier-application-using-azure-service-bus-queues"></a>Korzystanie z kolejek Bus usługi Azure aplikacji wielu .NET

## <a name="introduction"></a>Wprowadzenie

Projektowanie dla Microsoft Azure jest łatwe, przy użyciu programu Visual Studio i bezpłatne SDK Azure dla .NET. Ten samouczek przeprowadzi Cię przez kroki, aby utworzyć aplikację, która korzysta z wielu zasobów Azure działa w środowisku lokalnym. Procedurze założono, że masz nie wcześniejszego doświadczenia w korzystaniu Azure.

Możesz uzyskać następujące czynności:

-   Jak włączyć Azure opracowywania z jednym Pobierz i zainstaluj na komputerze.
-   Jak używać programu Visual Studio na Azure.
-   Jak utworzyć aplikację wielu platformy Azure za pomocą sieci web i pracownik role.
-   Jak komunikacji między poziomów korzystanie z usługi Bus kolejek.

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

W tym samouczku możesz utworzyć i uruchomić aplikację wielu w usłudze w chmurze Azure. Fronton będzie ról w sieci web programu ASP.NET MVC i wewnętrzna będzie rolę pracownika korzystającego z kolejki Bus usługi. Możesz utworzyć tej samej aplikacji wielu z fronton projektu sieci web, który jest wdrożony w witrynie sieci Web Azure zamiast usługi w chmurze. Aby uzyskać instrukcje dotyczące inaczej na fronton Azure witryny sieci Web Zobacz sekcję [Następne kroki](#nextsteps) . Można także wypróbować samouczek [aplikacji na lokalnej chmury hybrydowego .NET](../service-bus-relay/service-bus-dotnet-hybrid-app-using-service-bus-relay.md) .

Na poniższym zrzucie ekranu przedstawiono aplikację złożonym.

![][0]

## <a name="scenario-overview-inter-role-communication"></a>Omówienie scenariusza: komunikacji między roli

Aby przesłać zamówienie do przetwarzania, składnika frontonu interfejsu użytkownika, działa w roli web musi współdziałać z logiki warstwa środkowa działa w roli pracownika. W tym przykładzie użyto Bus usługi brokered wiadomości dla komunikacji między warstw.

Przy użyciu brokered wiadomości między w sieci web i drugie imię poziomów oddzielono dwa składniki. W odróżnieniu od bezpośredniej wiadomości (oznacza to, że TCP lub HTTP), warstwa nie nawiązać warstwa środkowa bezpośrednio; Zamiast tego go umieszcza jednostki pracy, jako wiadomości do Bus usługa, która niezawodne zachowuje, aż Środkowa warstwa jest gotowy do używania i procesu ich.

Usługa Bus udostępnia dwie jednostki do obsługi wiadomości brokered: kolejki i tematów. Z kolejki każda wiadomość wysłana do kolejki jest zużywanej przez jednego odbiorcy. Tematy pomocy technicznej publikowania/subskrypcji deseniu, w którym każdej wiadomości opublikowanych były dostępne dla subskrypcji zarejestrowanych w temacie. Każdej subskrypcji zachowuje logicznie kolejki wiadomości. Subskrypcje można skonfigurować w taki sposób, filtr reguł, które ograniczyć ilość komunikaty przesyłane do kolejki subskrypcji do tych, które są zgodne z filtr. W poniższym przykładzie użyto kolejek Bus usługi.

![][1]

Ten mechanizm komunikacji zawiera kilka zalet bezpośredni wiadomości:

-   **Rozdzielenie czasowy.** Z wzorcem wiadomości asynchroniczne producentów i konsumentów nie musi być online w tym samym czasie. Usługa Bus niezawodne przechowuje wiadomości, dopóki dużo strona jest gotowa do ich odbierać. Dzięki temu części aplikacji rozproszonej rozłączenia, albo, na przykład obsługi lub z powodu awarii składnika bez wpływania system jako całość. Ponadto dużo aplikacji może być konieczne tylko do trybu online pewnych porach dnia.

-   **Wyrównywanie obciążenia.** Wiele aplikacji obciążenia systemu różni się w czasie, gdy przetwarzanie czas dla każdej jednostki pracy jest zazwyczaj stałej. Intermediating producentów wiadomości i konsumentów z kolejki oznacza, że aplikacja dużo (pracownik) musi tylko być tak, aby zezwalały na średnie obciążenie zamiast ładowania Szczyt. Głębokość kolejki rozwoju i zamówień podczas ładowania przychodzące zmienia się. Zapisuje bezpośrednio pieniądze pod względem ilości infrastruktury wymagane do obsługi obciążenia aplikacji.

-   **Równoważenia obciążenia.** Jak załadować podwyżki, można dodać więcej procesy do czytania z kolejki. Każda wiadomość jest przetwarzana przez tylko jeden procesy. Ponadto ten równoważenia obciążenia opartego na pobieraj umożliwia optymalnego wykorzystania możliwości na komputerach pracowników nawet wtedy, gdy na komputerach pracowników różnią się pod względem możliwości przetwarzania jako wprowadzi wiadomości własne maksymalna szybkość. Ten wzorzec jest często określane jako deseń *uczestniczących w zawodach dla klientów indywidualnych* .

    ![][2]

W poniższych sekcjach omówiono kod, który wykonuje tej architektury.

## <a name="set-up-the-development-environment"></a>Konfigurowanie środowiska projektowego

Przed rozpoczęciem tworzenia aplikacji Azure narzędzia i konfigurowanie środowiska programowania.

1.  Zainstaluj Azure SDK dla środowiska .NET na [Uzyskiwanie narzędzia i zestaw SDK][].

2.  Kliknij przycisk **Zainstaluj zestaw SDK** dla wersji programu Visual Studio używasz. Kroki opisane w tym samouczku za pomocą programu Visual Studio 2015 r.

4.  Gdy zostanie wyświetlony monit, aby uruchomić, czy zapisać Instalatora pakietu, kliknij przycisk **Uruchom**.

5.  W **Instalatora platformy sieci Web**kliknij przycisk **Zainstaluj** i kontynuować instalację.

6.  Po zakończeniu instalacji trzeba wszystko, co jest potrzebne do uruchomienia opracowywaniu aplikacji. Zestaw SDK zawiera narzędzia, które umożliwiają łatwe opracowywania aplikacji Azure w programie Visual Studio. Jeśli nie masz programu Visual Studio zainstalowany, zestawu SDK jest instalowany również program bezpłatne Visual Studio Express.

## <a name="create-a-namespace"></a>Tworzenie obszaru nazw

Następnym krokiem jest tworzenie nazw usługi i uzyskać klucz udostępniony podpis programu Access (SA). Przestrzeń nazw zawiera granic aplikacji dla każdej aplikacji dostępne za pośrednictwem usługi Bus. Klucz skojarzeń zabezpieczeń jest generowany przez system po utworzeniu przestrzeń nazw. Kombinacja nazw i klucza skojarzeń zabezpieczeń zawiera poświadczenia dla usługi Bus do uwierzytelnienia dostęp do aplikacji.

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="create-a-web-role"></a>Tworzenie roli sieci web

W tej sekcji można tworzyć fronton aplikacji. Najpierw należy utworzyć stron, które są wyświetlane w aplikacji.
Po wykonaniu tej Dodaj kod, który przesyła elementy do kolejki Bus usługi i wyświetla informacje dotyczące kolejki stanu.

### <a name="create-the-project"></a>Tworzenie projektu

1.  Za pomocą uprawnień administratora, uruchom program Microsoft Visual Studio. Aby uruchomić program Visual Studio z uprawnieniami administratora, kliknij prawym przyciskiem myszy ikonę programu **Visual Studio** , a następnie kliknij **polecenie Uruchom jako administrator**. Emulatorze Azure obliczeń omówiony w dalszej części tego artykułu wymaga uruchomienia programu Visual Studio z uprawnieniami administratora.

    W programie Visual Studio, w menu **plik** kliknij polecenie **Nowy**, a następnie kliknij **Projekt**.

2.  Z **Zainstalowane szablony**w obszarze **Visual C#**kliknij **chmurze** , a następnie kliknij pozycję **Usługa w chmurze Azure**. Nazwa projektu **MultiTierApp**. Kliknij przycisk **OK**.

    ![][9]

3.  **.NET Framework 4,5** roli kliknij dwukrotnie **Ról w sieci Web programu ASP.NET**.

    ![][10]

4.  Umieść wskaźnik myszy na **WebRole1** w obszarze **usługi w chmurze Azure rozwiązanie**, kliknij ikonę ołówka i Zmień nazwę ról w sieci web na **FrontendWebRole**. Kliknij przycisk **OK**. (Upewnij się, że wprowadzone "Frontend" z małe "e," nie "FrontEnd").

    ![][11]

5.  W oknie dialogowym **Nowy projekt ASP.NET** na liście **Wybierz szablon** kliknij **MVC**.

    ![][12]

6. Nadal w oknie dialogowym **Nowy projekt programu ASP.NET** kliknij przycisk **Zmień uwierzytelniania** . W oknie dialogowym **Zmienianie uwierzytelniania** kliknij pozycję **Brak uwierzytelniania**, a następnie kliknij **przycisk OK**. Dla tego samouczka jest instalowany aplikację, która nie musi logowania użytkownika.

    ![][16]

7. W oknie dialogowym **Nowy projekt programu ASP.NET** kliknij **przycisk OK** , aby utworzyć projekt.

6.  W **Eksploratorze rozwiązań**w programie project **FrontendWebRole** kliknij prawym przyciskiem myszy **odwołania**, a następnie kliknij pozycję **Zarządzaj pakiety NuGet**.

7.  Kliknij kartę **Przeglądaj** , a następnie wyszukaj `Microsoft Azure Service Bus`. Kliknij przycisk **Zainstaluj**i zaakceptuj warunki użytkowania.

    ![][13]

    Zauważ, że teraz odwołuje się zestawów wymagany klient i dodano kilka nowych plików kodu.

9.  W **Eksploratorze rozwiązań**kliknij prawym przyciskiem myszy **modeli** i kliknij przycisk **Dodaj**, a następnie kliknij pozycję **Class**. W polu **Nazwa** wpisz nazwę **OnlineOrder.cs**. Następnie kliknij przycisk **Dodaj**.

### <a name="write-the-code-for-your-web-role"></a>Pisanie kodu dla ról w sieci web

W tej sekcji możesz utworzyć różnych stron, które są wyświetlane w aplikacji.

1.  W pliku OnlineOrder.cs w programie Visual Studio należy zastąpić istniejącą definicję nazw następujący kod:

    ```
    namespace FrontendWebRole.Models
    {
        public class OnlineOrder
        {
            public string Customer { get; set; }
            public string Product { get; set; }
        }
    }
    ```

2.  W **Eksploratorze rozwiązań**kliknij dwukrotnie **Controllers\HomeController.cs**. Dodaj poniższe instrukcje **przy użyciu** u góry pliku, aby uwzględnić nazw w modelu, który właśnie utworzono, a także Bus usługi.

    ```
    using FrontendWebRole.Models;
    using Microsoft.ServiceBus.Messaging;
    using Microsoft.ServiceBus;
    ```

3.  Również w pliku HomeController.cs w programie Visual Studio zastąpić istniejącą definicję nazw poniższy kod. Ten kod zawiera metody obsługi przesyłania elementów do kolejki.

    ```
    namespace FrontendWebRole.Controllers
    {
        public class HomeController : Controller
        {
            public ActionResult Index()
            {
                // Simply redirect to Submit, since Submit will serve as the
                // front page of this application.
                return RedirectToAction("Submit");
            }
    
            public ActionResult About()
            {
                return View();
            }
    
            // GET: /Home/Submit.
            // Controller method for a view you will create for the submission
            // form.
            public ActionResult Submit()
            {
                // Will put code for displaying queue message count here.
    
                return View();
            }
    
            // POST: /Home/Submit.
            // Controller method for handling submissions from the submission
            // form.
            [HttpPost]
            // Attribute to help prevent cross-site scripting attacks and
            // cross-site request forgery.  
            [ValidateAntiForgeryToken]
            public ActionResult Submit(OnlineOrder order)
            {
                if (ModelState.IsValid)
                {
                    // Will put code for submitting to queue here.
    
                    return RedirectToAction("Submit");
                }
                else
                {
                    return View(order);
                }
            }
        }
    }
    ```

4.  W menu **Tworzenie** kliknij przycisk **Utworzyć rozwiązanie** , aby sprawdzić dokładność pracy pory.

5.  Teraz Utwórz widok do `Submit()` metody został utworzony wcześniej. Kliknij prawym przyciskiem myszy `Submit()` metody (przeciążeń z `Submit()` którego nie ma parametrów), a następnie wybierz pozycję **Dodaj widok**.

    ![][14]

6.  Zostanie wyświetlone okno dialogowe tworzenia widoku. Na liście **szablon** wybierz pozycję **Utwórz**. Na liście **Klasa modelu** kliknij klasy **OnlineOrder** .

    ![][15]

7.  Kliknij przycisk **Dodaj**.

8.  Teraz zmienić wyświetlana nazwa aplikacji. W **Eksploratorze rozwiązań**, kliknij dwukrotnie **Views\Shared\\_Layout.cshtml** plik, aby otworzyć go w Edytorze Visual Studio.

9.  Zamienić wszystkie wystąpienia **Moja aplikacja ASP.NET** z **produktami firmy przykładowa**.

10. Usuwanie łączy ** **dla użytkowników domowych**i **kontaktu** **. Usuwanie wyróżniony kodu:

    ![][28]

11. Na koniec Modyfikuj stronę przesyłania, aby uwzględnić pewne informacje na temat kolejki. W **Eksploratorze rozwiązań**kliknij dwukrotnie plik **Views\Home\Submit.cshtml** , aby otworzyć go w Edytorze Visual Studio. Dodaj następujący wiersz po `<h2>Submit</h2>`. Na obecnym `ViewBag.MessageCount` jest pusta. Będą wypełniać go później.

    ```
    <p>Current number of orders in queue waiting to be processed: @ViewBag.MessageCount</p>
    ```

12. Możesz teraz wdrożono interfejsu użytkownika. Można nacisnąć klawisz **F5** , aby uruchomić aplikację i upewnij się, że wygląda zgodnie z oczekiwaniami.

    ![][17]

### <a name="write-the-code-for-submitting-items-to-a-service-bus-queue"></a>Pisanie kodu składania elementy do kolejki Bus usługi

Dodanie kodu składania elementy do kolejki. Najpierw należy utworzyć klasę, która zawiera informacje o połączeniu kolejki Bus usługi. Następnie zainicjować połączenie z Global.aspx.cs. Na koniec aktualizacji kodu przesyłania został utworzony wcześniej w HomeController.cs faktycznie przesłania elementy do kolejki Bus usługi.

1.  W **Eksploratorze rozwiązań**kliknij prawym przyciskiem myszy **FrontendWebRole** (kliknij prawym przyciskiem myszy projektu, nie roli). Kliknij przycisk **Dodaj**, a następnie kliknij pozycję **zajęć**.

2.  Nazwa klasy **QueueConnector.cs**. Kliknij przycisk **Dodaj** , aby utworzyć klasy.

3.  Teraz Dodaj kod, który obejmuje informacje o połączeniu i inicjuje połączenie kolejki Bus usługi. Zamienić całą zawartość QueueConnector.cs poniższy kod, a następnie wprowadź wartości dla `your Service Bus namespace` (Twoja nazwa nazw) i `yourKey`, która jest **klucz podstawowy** uzyskanego poprzednio z portalu usługi Azure.

    ```
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using Microsoft.ServiceBus.Messaging;
    using Microsoft.ServiceBus;
    
    namespace FrontendWebRole
    {
        public static class QueueConnector
        {
            // Thread-safe. Recommended that you cache rather than recreating it
            // on every request.
            public static QueueClient OrdersQueueClient;
    
            // Obtain these values from the portal.
            public const string Namespace = "your Service Bus namespace";
    
            // The name of your queue.
            public const string QueueName = "OrdersQueue";
    
            public static NamespaceManager CreateNamespaceManager()
            {
                // Create the namespace manager which gives you access to
                // management operations.
                var uri = ServiceBusEnvironment.CreateServiceUri(
                    "sb", Namespace, String.Empty);
                var tP = TokenProvider.CreateSharedAccessSignatureTokenProvider(
                    "RootManageSharedAccessKey", "yourKey");
                return new NamespaceManager(uri, tP);
            }
    
            public static void Initialize()
            {
                // Using Http to be friendly with outbound firewalls.
                ServiceBusEnvironment.SystemConnectivity.Mode =
                    ConnectivityMode.Http;
    
                // Create the namespace manager which gives you access to
                // management operations.
                var namespaceManager = CreateNamespaceManager();
    
                // Create the queue if it does not exist already.
                if (!namespaceManager.QueueExists(QueueName))
                {
                    namespaceManager.CreateQueue(QueueName);
                }
    
                // Get a client to the queue.
                var messagingFactory = MessagingFactory.Create(
                    namespaceManager.Address,
                    namespaceManager.Settings.TokenProvider);
                OrdersQueueClient = messagingFactory.CreateQueueClient(
                    "OrdersQueue");
            }
        }
    }
    ```

4.  Teraz upewnij się, że jest wywoływana metodę **inicjowania** . W **Eksploratorze rozwiązań**kliknij dwukrotnie **Global.asax\Global.asax.cs**.

5.  Dodaj następujący wiersz kodu na końcu metody **Application_Start** .

    ```
    FrontendWebRole.QueueConnector.Initialize();
    ```

6.  Na koniec zaktualizuj kod w sieci web utworzonej wcześniej, aby przesłać elementów do kolejki. W **Eksploratorze rozwiązań**kliknij dwukrotnie **Controllers\HomeController.cs**.

7.  Aktualizacja `Submit()` metody (przeciążeń nie ma parametrów) w następujący sposób, aby uzyskać statystykę wiadomości dla kolejki.

    ```
    public ActionResult Submit()
    {
        // Get a NamespaceManager which allows you to perform management and
        // diagnostic operations on your Service Bus queues.
        var namespaceManager = QueueConnector.CreateNamespaceManager();
    
        // Get the queue, and obtain the message count.
        var queue = namespaceManager.GetQueue(QueueConnector.QueueName);
        ViewBag.MessageCount = queue.MessageCount;
    
        return View();
    }
    ```

8.  Aktualizacja `Submit(OnlineOrder order)` metody (przeciążeń przyjmuje jeden parametr) w następujący sposób, aby przesłać informacje o zamówieniach do kolejki.

    ```
    public ActionResult Submit(OnlineOrder order)
    {
        if (ModelState.IsValid)
        {
            // Create a message from the order.
            var message = new BrokeredMessage(order);
    
            // Submit the order.
            QueueConnector.OrdersQueueClient.Send(message);
            return RedirectToAction("Submit");
        }
        else
        {
            return View(order);
        }
    }
    ```

9.  Teraz możesz uruchomić ponownie aplikację. Każdym przesłaniu zamówienia, zwiększa liczba wiadomości.

    ![][18]

## <a name="create-the-worker-role"></a>Tworzenie roli pracownika

Teraz możesz utworzyć roli Pracownik przetwarzający przesłanych elementów kolejności. W tym przykładzie użyto szablon projektu programu Visual Studio **Roli Pracownik kolejki Bus usługi** . Już uzyskać wymagane poświadczenia z portalu.

1. Upewnij się, że masz połączenie programu Visual Studio Azure konta.

2.  W programie Visual Studio w **Eksploratorze rozwiązań** kliknij prawym przyciskiem myszy folder **ról** w obszarze Projekt **MultiTierApp** .

3.  Kliknij przycisk **Dodaj**, a następnie kliknij przycisk **Nowy projekt roli pracownika**. Zostanie wyświetlone okno dialogowe **Dodawanie nowego projektu roli** .

    ![][26]

4.  W oknie dialogowym **Dodawanie nowego projektu roli** kliknij **Rolę pracownika z kolejki Bus usługi**.

    ![][23]

5.  W polu **Nazwa** wpisz nazwę projektu **OrderProcessingRole**. Następnie kliknij przycisk **Dodaj**.

6.  Skopiuj parametry połączenia, który został pobrany w kroku 9 sekcja "Tworzenie nazw Bus usługi" do Schowka.

7.  W **Eksploratorze rozwiązań**, kliknij prawym przyciskiem myszy **OrderProcessingRole** utworzony w kroku 5 (Upewnij się, kliknij prawym przyciskiem **OrderProcessingRole** w obszarze **role**, a nie klasy). Następnie kliknij polecenie **Właściwości**.

8.  Na karcie **Ustawienia** w oknie dialogowym **Właściwości** kliknij w polu **wartość** dla **Microsoft.ServiceBus.ConnectionString**, a następnie wklej wartość punktu końcowego, który został skopiowany w kroku 6.

    ![][25]

9.  Tworzenie klasy **OnlineOrder** reprezentuje zamówienia procesu z kolejki. Można ponownie użyć klasy, które zostały już utworzone. W **Eksploratorze rozwiązań**kliknij prawym przyciskiem myszy klasy **OrderProcessingRole** (kliknij prawym przyciskiem myszy ikonę klasy nie roli). Kliknij przycisk **Dodaj**, a następnie kliknij pozycję **Istniejący element**.

10. Przejdź do podfolderu dla **FrontendWebRole\Models**, a następnie kliknij dwukrotnie **OnlineOrder.cs** , aby dodać go do tego projektu.

11. W **WorkerRole.cs**, zmień wartość zmiennej **NazwaKolejki** z `"ProcessingQueue"` do `"OrdersQueue"` zgodnie z poniższym kodem.

    ```
    // The name of your queue.
    const string QueueName = "OrdersQueue";
    ```

12. Dodaj następujący za pomocą instrukcji u góry pliku WorkerRole.cs.

    ```
    using FrontendWebRole.Models;
    ```

13. W `Run()` funkcja wewnątrz `OnMessage()` połączeń, zastąpić zawartość `try` klauzula z następujący kod.

    ```
    Trace.WriteLine("Processing", receivedMessage.SequenceNumber.ToString());
    // View the message as an OnlineOrder.
    OnlineOrder order = receivedMessage.GetBody<OnlineOrder>();
    Trace.WriteLine(order.Customer + ": " + order.Product, "ProcessingMessage");
    receivedMessage.Complete();
    ```

14. Aplikacja została ukończona. Istnieje możliwość przetestowania pełnego stosowania prawym przyciskiem myszy projektu MultiTierApp w Eksploratorze rozwiązań, zaznaczając, **Ustaw jako projekt startowy**, a następnie naciskając klawisz F5. Zauważ, że liczba wiadomości nie zwiększa, ponieważ roli Pracownik przetwarza elementy z kolejki i oznacza je jako ukończone. Można wyświetlić wyniki śledzenia swojej roli Pracownik, korzystając Azure obliczyć emulatora Interfejsie użytkownika. Możesz to zrobić, klikając prawym przyciskiem myszy ikonę emulatora w obszarze powiadomień paska zadań i wybierając **Pokaż obliczyć emulatora elementy interfejsu użytkownika**.

    ![][19]

    ![][20]

## <a name="next-steps"></a>Następne kroki  

Aby dowiedzieć się więcej na temat usługi Bus, zobacz następujące zasoby:  

* [Usługa Azure Bus][sbmsdn]  
* [Strona usługi Bus usługi][sbwacom]  
* [Jak używać kolejek Bus usług][sbwacomqhowto]  

Aby dowiedzieć się więcej o wielu scenariuszy, zobacz:  

* [.NET wielu aplikacji przy użyciu tabel miejsca do magazynowania, kolejek i obiektów blob][mutitierstorage]  

  [0]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-01.png
  [1]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-100.png
  [2]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-101.png
  [Pobierz narzędzia i zestaw SDK]: http://go.microsoft.com/fwlink/?LinkId=271920


  [GetSetting]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.getsetting.aspx
  [Microsoft.WindowsAzure.Configuration.CloudConfigurationManager]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.aspx
  [NamespaceMananger]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx

  [QueueClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx

  [TopicClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx

  [EventHubClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubclient.aspx

  [9]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-10.png
  [10]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-11.png
  [11]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-02.png
  [12]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-12.png
  [13]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-13.png
  [14]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-33.png
  [15]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-34.png
  [16]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-14.png
  [17]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-36.png
  [18]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-37.png

  [19]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-38.png
  [20]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-39.png
  [23]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/SBWorkerRole1.png
  [25]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/SBWorkerRoleProperties.png
  [26]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/SBNewWorkerRole.png
  [28]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-40.png

  [sbmsdn]: http://msdn.microsoft.com/library/azure/ee732537.aspx  
  [sbwacom]: /documentation/services/service-bus/  
  [sbwacomqhowto]: service-bus-dotnet-get-started-with-queues.md  
  [mutitierstorage]: https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36
  