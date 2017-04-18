<properties
    pageTitle="Hybrydowe na lokalnej chmury aplikacji (.NET) | Microsoft Azure"
    description="Dowiedz się, jak utworzyć aplikację na lokalnej chmury hybrydowych .NET przy użyciu przekazywania Bus usługi Azure."
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
    ms.topic="hero-article"
    ms.date="09/16/2016"
    ms.author="sethm"/>

# <a name="net-on-premisescloud-hybrid-application-using-azure-service-bus-relay"></a>.NET hybrydowego na lokalnej chmury aplikacji przy użyciu przekazywania Bus usługi Azure

## <a name="introduction"></a>Wprowadzenie

W tym artykule opisano, jak utworzyć aplikację chmury hybrydowego z Microsoft Azure i Visual Studio. Samouczek założono, że masz nie wcześniejszego doświadczenia w korzystaniu Azure. W 30 minut konieczne będzie aplikacji, która korzysta z wielu zasobów Azure i uruchamiania w chmurze.

Dowiesz się:

-   Jak utworzyć lub dostosować istniejący usługi sieci web zużycia w tym celu rozwiązania sieci web.
-   Jak używać usługi przekazującej Bus usługi Azure współużytkowanie danych między aplikacją Azure i usługi sieci web hostowaną gdzie indziej.

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="how-the-service-bus-relay-helps-with-hybrid-solutions"></a>Sposób przekazywania Bus usługi ułatwiające rozwiązania hybrydowe

Rozwiązania biznesowe zwykle składają się z kombinacji kodu niestandardowego napisanego rozwiązania nowych i unikatowych wymagań i istniejące funkcjonalność rozwiązań i systemów, które już są na miejscu.

Architekci rozwiązania rozpoczęła się do chmury łatwiejsze obsługi wymagań skali i obniżyć koszty operacyjne. W ten sposób ich znaleźć istniejące zbiory usługi, którą chce wykorzystać jako bloków konstrukcyjnych dla ich rozwiązania są wewnątrz zaporę firmową i wylogowywanie się z łatwe osiągnięcia przez rozwiązanie chmury dostępu. Wiele wewnętrznych usług nie wbudowanych i hostowana w ten sposób, że ta osoba może łatwo nastąpić przy krawędzi sieci firmowej.

Przekazywania Bus usługi jest przeznaczony dla przypadków użycia podjęcia istniejących usług sieci web systemu Windows Communication Foundation (WCF), a co tych usług bezpiecznie dostępne dla rozwiązań znajdują się spoza firmy obwód bez wprowadzania inwazyjnych zmian w firmowej infrastrukturze sieciowej. Takie usługi przekazywania Bus usługi nadal są obsługiwane w ich istniejące środowisko, ale ich pełnomocnika wykrywanie przychodzące sesje i żądania Bus usługi hostowanej w chmurze. Usługa Bus chroni także tych usług przed nieautoryzowanym dostępem za pomocą uwierzytelniania [Udostępnione podpis programu Access](../service-bus-messaging/service-bus-sas-overview.md) (SA).

## <a name="solution-scenario"></a>Rozwiązanie scenariusza

W tym samouczku utworzysz witryny sieci Web programu ASP.NET, dzięki którym będzie widoczny na liście produktów na stronie zapasów produktu.

![][0]

Samouczek założono, że masz informacje o produkcie w systemie istniejącego lokalnego i użyto przekazywania Bus usługi, aby dotrzeć do tego systemu. Symulacji usługi sieci web, która działa w aplikacji konsoli prosty i kopii według zestawu w pamięci produktów. Można uruchomić tę aplikację konsoli ze swojego własnego komputera i wdrażania ról w sieci web do Azure. W ten sposób zostanie wyświetlona jak działa w obrębie Azure centrum danych ról w sieci web będzie faktycznie połączenia do komputera, nawet jeśli na komputerze będzie znajdować się prawie na pewno związany z co najmniej jedną zaporę i warstwy translatora adresów sieciowych adres.

Poniżej przedstawiono zrzut ekranu strony początkowej aplikacji sieci web zakończone.

![][1]

## <a name="set-up-the-development-environment"></a>Konfigurowanie środowiska projektowego

Przed rozpoczęciem tworzenia aplikacji Azure narzędzia i konfigurowanie środowiska programowania.

1.  Zainstaluj zestaw SDK Azure dla środowiska .NET na stronie [Pobierz narzędzia i zestaw SDK][] .

2.  Kliknij przycisk **Zainstaluj zestaw SDK** dla wersji programu Visual Studio używasz. Kroki opisane w tym samouczku za pomocą programu Visual Studio 2015 r.

4.  Gdy zostanie wyświetlony monit, aby uruchomić, czy zapisać Instalatora pakietu, kliknij przycisk **Uruchom**.

5.  W **Instalatora platformy sieci Web**kliknij przycisk **Zainstaluj** i kontynuować instalację.

6.  Po zakończeniu instalacji trzeba wszystko, co jest potrzebne do uruchomienia opracowywaniu aplikacji. Zestaw SDK zawiera narzędzia, które umożliwiają łatwe opracowywania aplikacji Azure w programie Visual Studio. Jeśli nie masz programu Visual Studio zainstalowany, zestawu SDK jest instalowany również program bezpłatne Visual Studio Express.

## <a name="create-a-namespace"></a>Tworzenie obszaru nazw

Aby rozpocząć korzystanie z funkcji Bus usługi w Azure, należy najpierw utworzyć obszar nazw usługi. Przestrzeń nazw zawiera kontenerze obejmującym adresowania Bus usługi zasobów w aplikacji.

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="create-an-on-premises-server"></a>Tworzenie lokalnego serwera

Najpierw zostanie utworzona system wykazu produktów lokalnych (zasymulować). Zostanie on dość proste; może to widzieć reprezentujące system wykazu produktów rzeczywisty lokalnego z powierzchni wykonania usługi możemy próbujesz zintegrować.

Ten projekt jest aplikacji konsoli programu Visual Studio i używa [pakietu NuGet Bus usługi Azure](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) uwzględnienie bibliotek Bus usługi i ustawienia konfiguracji.

### <a name="create-the-project"></a>Tworzenie projektu

1.  Za pomocą uprawnień administratora, uruchom program Microsoft Visual Studio. Aby uruchomić program Visual Studio z uprawnieniami administratora, kliknij prawym przyciskiem myszy ikonę programu **Visual Studio** , a następnie kliknij **polecenie Uruchom jako administrator**.

2.  W programie Visual Studio, w menu **plik** kliknij polecenie **Nowy**, a następnie kliknij **Projekt**.

3.  Z **Zainstalowane szablony**w obszarze **Visual C#**, kliknij **Aplikację konsoli**. W polu **Nazwa** wpisz nazwę **ProductsServer**:

    ![][11]

4.  Kliknij **przycisk OK** , aby utworzyć projekt **ProductsServer** .

7.  Jeśli masz już zainstalowany Menedżer pakietów NuGet programu Visual Studio, przejdź do następnego kroku. W przeciwnym razie odwiedź stronę [NuGet][] i kliknij przycisk [Zainstaluj NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c). Postępuj zgodnie z instrukcjami, aby zainstalować Menedżera pakietów NuGet, a następnie ponownie uruchom program Visual Studio.

7.  W Eksploratorze rozwiązań projektu **ProductsServer** kliknij prawym przyciskiem myszy, a następnie kliknij pozycję **Zarządzaj pakiety NuGet**.

8.  Kliknij kartę **Przeglądaj** , a następnie wyszukaj `Microsoft Azure Service Bus`. Kliknij przycisk **Zainstaluj**i zaakceptuj warunki użytkowania.

    ![][13]

    Uwaga teraz odwołuje zestawów wymagany klient.

9.  Dodaj nową klasę dla swojego zamówienia produktu. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy **ProductsServer** projektu i kliknij przycisk **Dodaj**, a następnie kliknij **zajęć**.

10. W polu **Nazwa** wpisz nazwę **ProductsContract.cs**. Następnie kliknij przycisk **Dodaj**.

11. W **ProductsContract.cs**Zamień definicji nazw następujący kod, który definiuje dot.

    ```
    namespace ProductsServer
    {
        using System.Collections.Generic;
        using System.Runtime.Serialization;
        using System.ServiceModel;
    
        // Define the data contract for the service
        [DataContract]
        // Declare the serializable properties.
        public class ProductData
        {
            [DataMember]
            public string Id { get; set; }
            [DataMember]
            public string Name { get; set; }
            [DataMember]
            public string Quantity { get; set; }
        }
    
        // Define the service contract.
        [ServiceContract]
        interface IProducts
        {
            [OperationContract]
            IList<ProductData> GetProducts();
    
        }
    
        interface IProductsChannel : IProducts, IClientChannel
        {
        }
    }
    ```

12. W plik Program.cs Zamień definicji nazw następujący kod, który dodaje Usługa profilu i hosta dla niego.

    ```
    namespace ProductsServer
    {
        using System;
        using System.Linq;
        using System.Collections.Generic;
        using System.ServiceModel;
    
        // Implement the IProducts interface.
        class ProductsService : IProducts
        {
    
            // Populate array of products for display on website
            ProductData[] products =
                new []
                    {
                        new ProductData{ Id = "1", Name = "Rock",
                                         Quantity = "1"},
                        new ProductData{ Id = "2", Name = "Paper",
                                         Quantity = "3"},
                        new ProductData{ Id = "3", Name = "Scissors",
                                         Quantity = "5"},
                        new ProductData{ Id = "4", Name = "Well",
                                         Quantity = "2500"},
                    };
    
            // Display a message in the service console application
            // when the list of products is retrieved.
            public IList<ProductData> GetProducts()
            {
                Console.WriteLine("GetProducts called.");
                return products;
            }
    
        }
    
        class Program
        {
            // Define the Main() function in the service application.
            static void Main(string[] args)
            {
                var sh = new ServiceHost(typeof(ProductsService));
                sh.Open();
    
                Console.WriteLine("Press ENTER to close");
                Console.ReadLine();
    
                sh.Close();
            }
        }
    }
    ```

13. W oknie Eksplorator rozwiązań kliknij dwukrotnie plik **App.config** , aby otworzyć go w Edytorze Visual Studio. W dolnej części ** &lt;system. ServiceModel&gt; ** elementu (, ale nadal w &lt;system. ServiceModel&gt;), Dodaj następujący kod XML. Pamiętaj zamienić *yourServiceNamespace* o nazwie nazw i *yourKey* klawisz skojarzeń zabezpieczeń, który wcześniej pobrane z portalu:

    ```
    <system.serviceModel>
    ...
      <services>
         <service name="ProductsServer.ProductsService">
           <endpoint address="sb://yourServiceNamespace.servicebus.windows.net/products" binding="netTcpRelayBinding" contract="ProductsServer.IProducts" behaviorConfiguration="products"/>
         </service>
      </services>
      <behaviors>
         <endpointBehaviors>
           <behavior name="products">
             <transportClientEndpointBehavior>
                <tokenProvider>
                   <sharedAccessSignature keyName="RootManageSharedAccessKey" key="yourKey" />
                </tokenProvider>
             </transportClientEndpointBehavior>
           </behavior>
         </endpointBehaviors>
      </behaviors>
    </system.serviceModel>
    ```
14. Nadal w App.config, w ** &lt;appSettings&gt; ** element, Zamień wartość ciągu połączenia przy użyciu parametrów połączenia uzyskanego poprzednio z portalu. 

    ```
    <appSettings>
    <!-- Service Bus specific app settings for messaging connections -->
    <add key="Microsoft.ServiceBus.ConnectionString"
           value="Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey"/>
    </appSettings>
    ```

14. Naciśnij klawisze **Ctrl + Shift + B** lub z menu **Tworzenie** kliknij pozycję **Utworzyć rozwiązanie** do tworzenia aplikacji i pory sprawdzić dokładność pracy.

## <a name="create-an-aspnet-application"></a>Tworzenie aplikacji programu ASP.NET

W tej sekcji zostanie utworzona prostych aplikacji ASP.NET, która zawiera dane pobrane z usługi produktu.

### <a name="create-the-project"></a>Tworzenie projektu

1.  Upewnij się, że Visual Studio działa z uprawnieniami administratora.

2.  W programie Visual Studio, w menu **plik** kliknij polecenie **Nowy**, a następnie kliknij **Projekt**.

3.  Z **Zainstalowane szablony**w obszarze **Visual C#**, kliknij **Aplikacji sieci Web programu ASP.NET**. Nazwa projektu **ProductsPortal**. Kliknij przycisk **OK**.

    ![][15]

4.  Na liście **Wybierz szablon** kliknij **MVC**. 

6.  Zaznacz pole wyboru dla **hosta w chmurze**.

    ![][16]

5. Kliknij przycisk **Zmień uwierzytelniania** . W oknie dialogowym **Zmienianie uwierzytelniania** kliknij pozycję **Brak uwierzytelniania**, a następnie kliknij **przycisk OK**. Dla tego samouczka jest instalowany aplikację, która nie musi logowania użytkownika.

    ![][18]

6.  Upewnij się, że jest zaznaczona opcja **hosta w chmurze** i że na liście rozwijanej wybrano **Aplikacji usługi** w sekcji **Platformy Microsoft Azure** w oknie dialogowym **Nowy projekt programu ASP.NET** .

    ![][19]

7. Kliknij **przycisk OK**. 

8. Teraz należy skonfigurować Azure zasoby dla nowej aplikacji sieci web. Wykonaj wszystkie kroki opisane w sekcji [Konfigurowanie Azure zasoby dla nowej aplikacji sieci web](../app-service-web/web-sites-dotnet-get-started.md#configure-azure-resources-for-a-new-web-app). Następnie wróć do tego samouczka i przejdź do następnego kroku.

5.  W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy **modeli** kliknij przycisk **Dodaj**, a następnie kliknij pozycję **Class**. W polu **Nazwa** wpisz nazwę **Product.cs**. Następnie kliknij przycisk **Dodaj**.

    ![][17]

### <a name="modify-the-web-application"></a>Modyfikowanie aplikacji sieci web

1.  W pliku Product.cs w programie Visual Studio należy zastąpić istniejącą definicję nazw poniższy kod.

    ```
    // Declare properties for the products inventory.
    namespace ProductsWeb.Models
    {
        public class Product
        {
            public string Id { get; set; }
            public string Name { get; set; }
            public string Quantity { get; set; }
        }
    }
    ```

2.  W oknie Eksplorator rozwiązań rozwiń folder **kontrolery** , a następnie kliknij dwukrotnie plik **HomeController.cs** , aby go otworzyć w programie Visual Studio.

3. W **HomeController.cs**należy zastąpić istniejącą definicję nazw poniższy kod.

    ```
    namespace ProductsWeb.Controllers
    {
        using System.Collections.Generic;
        using System.Web.Mvc;
        using Models;
    
        public class HomeController : Controller
        {
            // Return a view of the products inventory.
            public ActionResult Index(string Identifier, string ProductName)
            {
                var products = new List<Product>
                    {new Product {Id = Identifier, Name = ProductName}};
                return View(products);
            }
         }
    }
    ```

3.  W oknie Eksplorator rozwiązań rozwiń Views\Shared folder, a następnie kliknij dwukrotnie **_Layout.cshtml** , aby otworzyć go w Edytorze Visual Studio.

5.  Zmienić wszystkie wystąpienia **Moja aplikacja ASP.NET** **produktów PRZYKŁADOWEJ firmy**.

6. Usuwanie łączy ** **dla użytkowników domowych**i **kontaktu** **. W poniższym przykładzie usunąć wyróżniony kodu.

    ![][41]

7.  W oknie Eksplorator rozwiązań rozwiń Views\Home folder, a następnie kliknij dwukrotnie **Index.cshtml** , aby otworzyć go w Edytorze Visual Studio.
    Zamień całą zawartość plik poniższy kod.

    ```
    @model IEnumerable<ProductsWeb.Models.Product>
    
    @{
            ViewBag.Title = "Index";
    }
    
    <h2>Prod Inventory</h2>
    
    <table>
            <tr>
                <th>
                    @Html.DisplayNameFor(model => model.Name)
                </th>
                  <th></th>
                <th>
                    @Html.DisplayNameFor(model => model.Quantity)
                </th>
            </tr>
    
    @foreach (var item in Model) {
            <tr>
                <td>
                    @Html.DisplayFor(modelItem => item.Name)
                </td>
                <td>
                    @Html.DisplayFor(modelItem => item.Quantity)
                </td>
            </tr>
    }
    
    </table>
    ```

9.  Aby sprawdzić dokładność pracy pory, możesz nacisnąć **Klawisze Ctrl + Shift + B** , aby utworzyć projekt.


### <a name="run-the-app-locally"></a>Uruchom aplikację lokalnie

Uruchom aplikację, aby sprawdzić, czy działa.

1.  Upewnij się, że **ProductsPortal** jest aktywnego projektu. Kliknij prawym przyciskiem myszy nazwę projektu w Eksploratorze rozwiązań i wybierz **Jako projekt startowy**.
2.  W programie Visual Studio naciśnij klawisz F5.
3.  Aplikacja powinna zostać wyświetlona działa w przeglądarce.

    ![][21]

## <a name="put-the-pieces-together"></a>Składanie części

Następnym krokiem jest Podłączanie lokalnego serwera produktów z aplikacją ASP.NET.

1.  Jeśli nie jest jeszcze otwarta, w programie Visual Studio ponownie otworzyć projekt **ProductsPortal** , który został utworzony w sekcji [Tworzenie aplikacji programu ASP.NET](#create-an-aspnet-application) .

2.  Podobne do czynności w sekcji "Tworzenie lokalnej na serwer" Dodawanie pakietu NuGet z odwołaniami do projektu. W Eksploratorze rozwiązań projektu **ProductsPortal** kliknij prawym przyciskiem myszy, a następnie kliknij pozycję **Zarządzaj pakiety NuGet**.

3.  Wyszukaj "Usługi Bus" i wybierz element, **Microsoft Azure usługi Bus** . Następnie zakończyć instalację i zamknij to okno dialogowe.

4.  W oknie Eksplorator rozwiązań projektu **ProductsPortal** kliknij prawym przyciskiem myszy, a następnie kliknij przycisk **Dodaj**, następnie **Istniejący element**.

5.  Przejdź do pliku **ProductsContract.cs** z projektem konsoli **ProductsServer** . Kliknij, aby wyróżnić ProductsContract.cs. Kliknij strzałkę w dół obok przycisku **Dodaj**, a następnie kliknij przycisk **Dodaj jako łącze**.

    ![][24]

6.  Teraz Otwórz plik **HomeController.cs** w Edytorze Visual Studio i zamienić definicji nazw poniższy kod. Pamiętaj zamienić *yourServiceNamespace* nazwę usługi nazw i *yourKey* przy użyciu klucza skojarzeń zabezpieczeń. Spowoduje to włączenie klienta do połączenia usługę lokalnego zwraca wynik połączenia.

    ```
    namespace ProductsWeb.Controllers
    {
        using System.Linq;
        using System.ServiceModel;
        using System.Web.Mvc;
        using Microsoft.ServiceBus;
        using Models;
        using ProductsServer;
    
        public class HomeController : Controller
        {
            // Declare the channel factory.
            static ChannelFactory<IProductsChannel> channelFactory;
    
            static HomeController()
            {
                // Create shared access signature token credentials for authentication.
                channelFactory = new ChannelFactory<IProductsChannel>(new NetTcpRelayBinding(),
                    "sb://yourServiceNamespace.servicebus.windows.net/products");
                channelFactory.Endpoint.Behaviors.Add(new TransportClientEndpointBehavior {
                    TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider(
                        "RootManageSharedAccessKey", "yourKey") });
            }
    
            public ActionResult Index()
            {
                using (IProductsChannel channel = channelFactory.CreateChannel())
                {
                    // Return a view of the products inventory.
                    return this.View(from prod in channel.GetProducts()
                                     select
                                         new Product { Id = prod.Id, Name = prod.Name,
                                             Quantity = prod.Quantity });
                }
            }
        }
    }
    ```

7.  W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy rozwiązanie **ProductsPortal** (Upewnij się, że kliknij prawym przyciskiem myszy rozwiązanie, nie projektu). Kliknij przycisk **Dodaj**, a następnie kliknij pozycję **Istniejącego projektu**.

8.  Przejdź do programu project **ProductsServer** , a następnie kliknij dwukrotnie plik rozwiązanie **ProductsServer.csproj** , aby go dodać.

9.  Aby można było wyświetlić dane na **ProductsPortal**musi być zainstalowany **ProductsServer** . W oknie Eksplorator rozwiązań rozwiązanie **ProductsPortal** kliknij prawym przyciskiem myszy, a następnie kliknij polecenie **Właściwości**. Zostanie wyświetlone okno dialogowe **Właściwości strony** .

10. Po lewej stronie kliknij pozycję **Projekt uruchamiania**. Po prawej stronie kliknij **wiele projektów uruchamiania**. Upewnij się, że **ProductsServer** i **ProductsPortal** mają być wyświetlane, w takiej kolejności z **uruchomić** określonych akcji dla obu.

      ![][25]

11. W oknie dialogowym **Właściwości** kliknij **Współzależności projektów** po lewej stronie.

12. Na liście **Projekty** kliknij pozycję **ProductsServer**. Upewnij się, że **ProductsPortal** jest **zaznaczona** .

14. Na liście **projektów** kliknij **ProductsPortal**. Upewnij się, że jest zaznaczona opcja **ProductsServer** . 

    ![][26]

15. Kliknij przycisk **OK** w oknie dialogowym **Właściwości strony** .

## <a name="run-the-project-locally"></a>Uruchamianie projektu lokalnie

Aby przetestować aplikacji lokalnie, w programie Visual Studio, naciśnij klawisz **F5**. Lokalnego serwera (**ProductsServer**) powinna rozpoczynać się pierwszy, a następnie aplikacja **ProductsPortal** powinna działać w oknie przeglądarki. Tym razem pojawi się że magazynie produktów wyświetla dane pobrane z lokalnym systemem usługi produktu.

![][10]

Naciśnij klawisz **odświeżania** na stronie **ProductsPortal** . Zawsze Odśwież stronę, zostanie wyświetlona aplikacja serwera był wyświetlany komunikat po `GetProducts()` z **ProductsServer** nosi nazwę.

Zamknij obie aplikacje przed przejściem do następnego kroku.

## <a name="deploy-the-productsportal-project-to-an-azure-web-app"></a>Wdrażanie projektu ProductsPortal Azure w przeglądarce

Następnym krokiem jest aby przekonwertować **ProductsPortal** frontend Azure w przeglądarce. Wdrożenie projektu **ProductsPortal** , zgodnie z krokami w sekcji [Rozmieszczanie projektu sieci web do aplikacji sieci Azure web](../app-service-web/web-sites-dotnet-get-started.md#deploy-the-web-project-to-the-azure-web-app). Po zakończeniu wdrożenia powrócić do tego samouczka i przejdź do następnego kroku.

> [AZURE.NOTE] Po **ProductsPortal** projektu sieci web jest uruchamiane automatycznie po wdrożeniu może zostać wyświetlony komunikat o błędzie w oknie przeglądarki. To jest oczekiwane i występuje, ponieważ nie jest jeszcze uruchomiona aplikacja **ProductsServer** .

Skopiuj adres URL aplikacji web wdrożonym, jak będzie potrzebny jest adres URL w następnym kroku. W oknie wykonania usługi Azure aplikacji w programie Visual Studio, można również uzyskać ten adres URL:

![][9] 

### <a name="set-productsportal-as-web-app"></a>Ustawianie ProductsPortal jako aplikacji sieci web

Przed uruchomieniem aplikacji w chmurze, należy się upewnić, że **ProductsPortal** jest uruchomiona w programie Visual Studio jako aplikacji sieci web.

1. W programie Visual Studio kliknij prawym przyciskiem myszy **ProjectsPortal** projektu, a następnie kliknij przycisk **Właściwości**.

3. W kolumnie po lewej stronie kliknij pozycję **sieć Web**.

5. W sekcji **Rozpoczęcie akcji** kliknij przycisk **Rozpocznij adres URL** , a w polu tekstowym wprowadź adres URL dla aplikacji sieci web wcześniej wdrożonym; na przykład `http://productsportal1234567890.azurewebsites.net/`.

    ![][27]

6. Z menu **plik** w programie Visual Studio kliknij przycisk **Zapisz wszystko**.

7. Z menu kompilacji programu Visual Studio kliknij przycisk **Odbudowanie rozwiązanie**.

## <a name="run-the-application"></a>Uruchamianie aplikacji

2.  Naciśnij klawisz F5, aby utworzyć i uruchomić aplikację. Lokalnego serwera (Aplikacja konsoli **ProductsServer** ) powinna rozpoczynać się pierwszy, a następnie aplikacja **ProductsPortal** powinna działać w oknie przeglądarki, jak pokazano na poniższym obrazie zrzut. Powiadomienie o ponownie magazynie produktów wyświetla dane pobrane z lokalnym systemem usługi produktu i wyświetla je w aplikacji sieci web. Zaznacz adres URL, aby upewnić się, że **ProductsPortal** działa w chmurze, jako Azure w przeglądarce. 

    ![][1]

    > [AZURE.IMPORTANT] Aplikacja konsoli **ProductsServer** musi być uruchomiony i można udostępniać dane do aplikacji **ProductsPortal** . Jeśli przeglądarka zostanie wyświetlony komunikat o błędzie, zaczekaj kilka sekund więcej **ProductsServer** na ładowanie i wyświetlenie następującego komunikatu. Naciśnij **Odśwież** w przeglądarce.

    ![][37]

3. W przeglądarce naciśnij klawisz **odświeżania** na stronie **ProductsPortal** . Zawsze Odśwież stronę, zostanie wyświetlona aplikacja serwera był wyświetlany komunikat po `GetProducts()` z **ProductsServer** nosi nazwę.

    ![][38]

## <a name="next-steps"></a>Następne kroki  

Aby dowiedzieć się więcej na temat usługi Bus, zobacz następujące zasoby:  

* [Usługa Azure Bus][sbwacom]  
* [Jak używać kolejek Bus usług][sbwacomqhowto]  


  [0]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hybrid.png
  [1]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App2.png
  [Pobierz narzędzia i zestaw SDK]: http://go.microsoft.com/fwlink/?LinkId=271920
  [NuGet]: http://nuget.org
  
  [11]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-con-1.png
  [13]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-multi-tier-13.png
  [15]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-2.png
  [16]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-4.png
  [17]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-7.png
  [18]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-5.png
  [19]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-6.png
  [9]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-9.png
  [10]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App3.png

  [21]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App1.png
  [24]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-12.png
  [25]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-13.png
  [26]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-14.png
  [27]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-8.png
  
  [36]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App2.png
  [37]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-service1.png
  [38]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-service2.png
  [41]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-multi-tier-40.png
  [43]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-hybrid-43.png


  [sbwacom]: /documentation/services/service-bus/  
  [sbwacomqhowto]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md

