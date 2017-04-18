<properties
   pageTitle="Tworzenie frontonu sieci web dla aplikacji przy użyciu podstawowych ASP.NET | Microsoft Azure"
   description="Uwidaczniają tkaninie usługi aplikacji sieci web przy użyciu interfejsu API sieci Web Core ASP.NET projektu i komunikacji między usługą za pośrednictwem ServiceProxy."
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/11/2016"
   ms.author="seanmck"/>


# <a name="build-a-web-service-front-end-for-your-application-using-aspnet-core"></a>Tworzenie frontonu sieci web usługi aplikacji przy użyciu podstawowego programu ASP.NET

Domyślnie tkaninie usługi Azure usług nie zawierają interfejsu publicznego w sieci Web. Aby aktywować funkcje aplikacji klientom HTTP, będzie konieczne tworzenie projektu sieci web do działania jako punktu wejścia i następnie komunikowanie się z niej poszczególnych usługach.

W tym samouczku pracujemy wznowić miejsce, w którym możemy samouczka [Tworzenie pierwszej aplikacji w programie Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md) i dodawanie usługi sieci web przed usługę liczników stanowe. Jeśli jeszcze tego nie zrobiono, należy powrócić i kroków tego samouczka najpierw.

## <a name="add-an-aspnet-core-service-to-your-application"></a>Dodawanie usługi ASP.NET Core do aplikacji

Podstawowe ASP.NET jest ramy opracowywania uproszczonego, między platformami sieci web, które umożliwia tworzenie nowoczesnego interfejs użytkownika sieci web oraz interfejsy API w sieci web. Możesz dodać projekt interfejsu API sieci Web programu ASP.NET z naszych istniejącą aplikacją.

>[AZURE.NOTE] Do użycia tego samouczka, należy [zainstalować program .NET Core 1.0][dotnetcore-install].

1. W Eksploratorze rozwiązań, kliknij prawym przyciskiem myszy **usług** w projekcie aplikacji i wybierz pozycję **Dodaj > Nowy tkaninie usługi**.

    ![Dodawanie nowej usługi do istniejącej aplikacji][vs-add-new-service]

2. Na stronie **Tworzenie usługi** wybierz **ASP.NET Core** , a następnie nadaj plikowi nazwę.

    ![Wybieranie usługi sieci web programu ASP.NET w oknie dialogowym Nowy usługi][vs-new-service-dialog]

3. Następna strona zawiera zestaw podstawowych ASP.NET szablony projektów. Należy zauważyć, że są one szablony, które zostanie wyświetlony, jeśli utworzono ASP.NET projektów poza aplikacją usługi tkaninie. Ten samouczek zostanie wybrany **Interfejs API sieci Web**. Jednak możesz zastosować te same pojęcia do tworzenia aplikacji sieci web pełny.

    ![Wybieranie typu projektu programu ASP.NET][vs-new-aspnet-project-dialog]

    Po utworzeniu projektu interfejs API sieci Web, dostępne będą dwie usługi w aplikacji. Podczas tworzenia aplikacji, należy dodać więcej usług w taki sam sposób. Każdy może być niezależne wersji i uaktualnione.

>[AZURE.TIP] Aby dowiedzieć się więcej o tworzeniu ASP.NET Core services, zobacz [Dokumentację Core ASP.NET](https://docs.asp.net).

## <a name="run-the-application"></a>Uruchamianie aplikacji

Aby uzyskać zorientować się, co mamy już gotowe, Przejdźmy Wdrażanie nowej aplikacji i zapoznaj się domyślne zachowanie zawierającego szablon interfejs API programu ASP.NET Core sieci Web.

1. W programie Visual Studio do debugowania aplikacji, naciśnij klawisz F5.

2. Po zakończeniu wdrożenia programu Visual Studio powoduje uruchomienie przeglądarki w katalogu głównym usługi interfejs API sieci Web programu ASP.NET — na przykład http://localhost:33003. Numer portu jest losowo i mogą być różne na tym komputerze. Szablon interfejs API programu ASP.NET Core sieci Web nie zapewnia zachowanie domyślne katalogu głównego, zostanie wyświetlony komunikat o błędzie w przeglądarce.

3. Dodawanie `/api/values` do lokalizacji w przeglądarce. To wywoła `Get` metoda ValuesController w szablonie interfejs API sieci Web. Kwerenda zwróci wartość domyślna dostarczonego przez szablon — macierz JSON, która zawiera dwa ciągi:

    ![Domyślne wartości zwracane z szablonu interfejs API sieci Web Core programu ASP.NET][browser-aspnet-template-values]

    Koniec samouczka, firma Microsoft będzie zastąpiła te wartości domyślne ostatnio wartość licznika z obsługą stanowe.


## <a name="connect-the-services"></a>Łączenie usługi

Usługa tkaninie zapewnia pełną elastyczność w sposób komunikować się z usługami zaufanego. W jednej aplikacji może być usług, które są dostępne za pośrednictwem protokołu TCP, innych usług, które są dostępne za pośrednictwem interfejsu API usługi REST HTTP i nadal innych usług, które są dostępne za pośrednictwem sieci web sockets. Opcje dostępne i kompromisów związane, zobacz [Communicating z usługami](service-fabric-connect-and-communicate-with-services.md). W tym samouczku firma Microsoft będzie wykonaj jedną z metod prostsze i używanie `ServiceProxy` / `ServiceRemotingListener` klasy, które znajdują się w zestawie SDK.

W `ServiceProxy` rozwiązaniem (zaprojektowany na zdalnego wywołania procedury lub RPC), definiowanie interfejsu ma pełnić rolę zamówienia publicznego usługi. Następnie za pomocą interfejsu do generowania klasy proxy interakcji z usługą.


### <a name="create-the-interface"></a>Utwórz interfejs

Firma Microsoft rozpocznie się, tworząc interfejs, który ma pełnić rolę Umowy między usługą stanowe i jej klientów, w tym projektu ASP.NET Core.

1. W Eksploratorze rozwiązań rozwiązania kliknij prawym przyciskiem myszy i wybierz pozycję **Dodaj** > **Nowego projektu**.

2. W okienku nawigacji po lewej stronie wybierz pozycję wpis **Visual C#** , a następnie wybierz szablon **Biblioteka klas** . Upewnij się, czy .NET Framework w wersji jest ustawiona **4.5.2**.

    ![Tworzenie projektu interfejs usługi stanowe][vs-add-class-library-project]

3. Aby interfejs może być używany przez `ServiceProxy`, musi pochodzić z interfejsu IService. Ten interfejs znajduje się w jednym z pakietów NuGet tkaninie usługi. Aby dodać pakiet, kliknij prawym przyciskiem myszy z nowym projektem Biblioteka klas i wybierz pozycję **Zarządzaj NuGet pakietów**.

4. Wyszukiwanie pakietu **Microsoft.ServiceFabric.Services** i zainstaluj go.

    ![Dodawanie pakietu NuGet usług][vs-services-nuget-package]

5. W bibliotece klasy utworzenie interfejsu z jedną metodę `GetCountAsync`, i rozszerzanie interfejsie z IService.

    ```c#
    namespace MyStatefulService.Interfaces
    {
        using Microsoft.ServiceFabric.Services.Remoting;

        public interface ICounter: IService
        {
            Task<long> GetCountAsync();
        }
    }
    ```


### <a name="implement-the-interface-in-your-stateful-service"></a>Implementowanie interfejsu w usłudze stanowe

Teraz, gdy określono możemy interfejs, należy ją wdrożyć w usłudze stanowe.

1. W usłudze stanowe Dodaj odwołanie do projekt biblioteki zajęć, który zawiera interfejs.

    ![Dodawanie odwołanie do projektu biblioteki zajęć w usłudze stanowe][vs-add-class-library-reference]

2. Znajdź klasy, która dziedziczy `StatefulService`, takich jak `MyStatefulService`, i rozszerzanie go do wprowadzenia w życie `ICounter` interfejsu.

    ```c#
    using MyStatefulService.Interfaces;

    ...

    public class MyStatefulService : StatefulService, ICounter
    {        
          // ...
    }
    ```

3. Teraz zaimplementować pojedynczą metodę, która jest zdefiniowana w `ICounter` interfejsu `GetCountAsync`.

    ```c#
    public async Task<long> GetCountAsync()
    {
      var myDictionary =
        await this.StateManager.GetOrAddAsync<IReliableDictionary<string, long>>("myDictionary");

        using (var tx = this.StateManager.CreateTransaction())
        {          
            var result = await myDictionary.TryGetValueAsync(tx, "Counter");
            return result.HasValue ? result.Value : 0;
        }
    }
    ```


### <a name="expose-the-stateful-service-using-a-service-remoting-listener"></a>Udostępnienia stanowe usługi przy użyciu usługi zdalnej detektor

Z `ICounter` interfejs zaimplementowana, ostatnim krokiem Włączanie stanowe usługa jest żądanie z innych usług, należy otworzyć kanału komunikacji. Dla usług stanowe tkaninie usługi zapewnia żaden metodę o nazwie `CreateServiceReplicaListeners`. Przy użyciu tej metody można określić detektory komunikacji, na podstawie typu komunikacji, których chcesz włączyć do tej usługi.

>[AZURE.NOTE] Nosi nazwę odpowiedniej metody otwarcia kanału komunikacji do usług stateless `CreateServiceInstanceListeners`.

W tym przypadku Microsoft zastąpi istniejącą `CreateServiceReplicaListeners` metody i podaj wystąpienie `ServiceRemotingListener`, który tworzy z punktem końcowym którego jest żądanie od klientów za pomocą `ServiceProxy`.  

```c#
using Microsoft.ServiceFabric.Services.Remoting.Runtime;

...

protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
{
    return new List<ServiceReplicaListener>()
    {
        new ServiceReplicaListener(
            (context) =>
                this.CreateServiceRemotingListener(context))
    };
}
```


### <a name="use-the-serviceproxy-class-to-interact-with-the-service"></a>Użyj klasy ServiceProxy do współdziałania z usługą

Obsługą stanowe jest teraz gotowy do odbierać danych z innych usług. Dlatego wszystkie opcje, które nadal jest dodanie kodu się z nim komunikować się z usługi sieci web programu ASP.NET.

1. W projekcie programu ASP.NET, Dodaj odwołanie do biblioteki zajęć, która zawiera `ICounter` interfejsu.

2. W menu **Tworzenie** Otwórz **Menedżera konfiguracji**. Powinien zostać wyświetlony podobną do następującej:

    ![Biblioteka klas przedstawiający Menedżera konfiguracji jako AnyCPU][vs-configuration-manager]

    Należy zauważyć, że klasy biblioteki projektu, **MyStatefulService.Interface**jest skonfigurowany do tworzenia dla dowolnego Procesora. Działać poprawnie przy tkaninie usługi, go musi być jawnie przeznaczone do x64. Kliknij listę rozwijaną platformy i wybierz pozycję **Nowy**, a następnie utworzyć x64 konfiguracji platformy.

    ![Tworzenie nowej platformy dla Biblioteka klas][vs-create-platform]

3. Dodawanie pakietu Microsoft.ServiceFabric.Services do projektu programu ASP.NET, po prostu tak samo jak projektu Biblioteka klas wcześniej. Zapewnia `ServiceProxy` zajęć.

4. W folderze **kontrolery** Otwórz `ValuesController` zajęć. Należy zauważyć, że `Get` metody obecnie tylko zwraca tablicę stałe ciąg "wartość1" i "wartość2" — spełniającego możemy pokazano wcześniej w przeglądarce. Zastąp tej implementacji następujący kod:

    ```c#
    using MyStatefulService.Interfaces;
    using Microsoft.ServiceFabric.Services.Remoting.Client;

    ...

    public async Task<IEnumerable<string>> Get()
    {
        ICounter counter =
            ServiceProxy.Create<ICounter>(new Uri("fabric:/MyApplication/MyStatefulService"), new ServicePartitionKey(0));

        long count = await counter.GetCountAsync();

        return new string[] { count.ToString() };
    }
    ```

    Pierwszy wiersz kodu jest klucza. Aby utworzyć serwera proxy ICounter z usługą stanowe, konieczne jest zapewnienie dwóch rodzajów informacji: identyfikator i nazwę usługi.

    Możesz użyć przechodzenia do usług stanowe skali przez rozdzielanie stanu do różnych przedziałów oparte na kluczu zdefiniowanego, takie jak identyfikator klienta lub kod pocztowy. W naszym uproszczony aplikacji usługi stanowe tylko ma jedną partycją, więc klucz nie ma znaczenia. Dowolny klawisz dostarczone będzie powodowało partycją. Aby dowiedzieć się więcej o partycjonowaniu usług, zobacz [jak partition usługa tkaninie niezawodne usługi](service-fabric-concepts-partitioning.md).

    Nazwa usługi jest identyfikator URI materiału formularza:-&lt;nazwa_aplikacji&gt;/&lt;service_name&gt;.

    Z tych dwóch rodzajów informacji tkaninie usługi można jednoznacznie identyfikować komputera, do którego powinny być wysyłane żądania. `ServiceProxy` Klasa obsługuje także bezproblemowo wtedy, gdy komputera obsługującego partycją stanowe usługi kończy się niepowodzeniem i inny komputer musi awansować na jej miejsce. Ten abstrakcji służy do pisania kodu klienta na zajęcie się z innymi usługami znacznie upraszcza.

    Gdy mamy już serwer proxy, możemy po prostu wywołania `GetCountAsync` metody i zwraca jej wynik.

5. Naciśnij klawisz F5, aby ponownie uruchomić aplikację zmienione. Jak wcześniej, Visual Studio uruchamia automatycznie przeglądarki w katalogu głównym projektu sieci web. Dodawanie ścieżki "interfejsu api i wartości", a powinna być widoczna bieżącą wartość licznika zwracane.

    ![Wartość licznika stanowe wyświetlania w przeglądarce][browser-aspnet-counter-value]

    Odśwież przeglądarkę okresowo, aby wyświetlić wartość licznika aktualizacji.


>[AZURE.WARNING] Serwer sieci web programu ASP.NET Core opisane w szablonie znana pod nazwą Kestrel, to [nie są obecnie obsługiwane obsługi bezpośredni ruchu internetowego](https://docs.asp.net/en/latest/fundamentals/servers.html#kestrel). W przypadku scenariuszy produkcji należy rozważyć, czy hostingu punktów końcowych ASP.NET Core za [Zarządzanie interfejsu API] [ api-management-landing-page] lub innej bramy w Internecie. Zauważ, że tkaninie usługa nie jest obsługiwana do wdrożenia w programie IIS.


## <a name="what-about-actors"></a>Co z otwieraniem uczestników?

Ten samouczek ograniczony do dodawania frontonu sieci web którego prowadzona akumulującej stan usługi. Można jednak wykonać model bardzo podobne, aby porozmawiać z uczestnikami. W rzeczywistości jest nieco prostsze.

Po utworzeniu projektu Aktor Visual Studio automatycznie generuje interfejsu programu project. Za pomocą interfejsu do generowania Aktor serwera proxy w programie project web można komunikować się z aktora. Kanał komunikacji jest dostarczany automatycznie. Nie trzeba wykonywać żadnych odpowiada ustanawianie `ServiceRemotingListener` , jak to zostało akumulującej stan usługi w tym samouczku.

## <a name="how-web-services-work-on-your-local-cluster"></a>Jak działają usługi sieci web w klastrze lokalne

Zazwyczaj możesz wdrożyć dokładnie tej samej aplikacji usługi tkaninie do wielokrotnego stanowiska klastrze wdrożony w klastrze lokalne i wysoce pewność, że będzie działać zgodnie z oczekiwaniami. Jest to spowodowane lokalne klaster jest po prostu konfiguracji pięć, która zwinięciu na jednym komputerze.

Jeśli chodzi o usługi sieci web, jednak jest jeden nuance klucza. Gdy klaster znajduje się za równoważenia obciążenia, co w Azure, należy się upewnić, że usług sieci web są rozmieszczane na każdym komputerze, ponieważ równoważenia obciążenia będzie po prostu okrężnego ruch na komputerach. Można to zrobić przez ustawienie `InstanceCount` usługi specjalne wartości "-1".

Wyjątkiem jest sytuacja gdy uruchomieniem usługi sieci web lokalnie, należy się upewnić, że tylko jedno wystąpienie jest uruchomiona. Zostanie w przeciwnym razie napotkasz konfliktów z wielu procesów, które nasłuchują na tę samą ścieżkę i port. W wyniku liczba wystąpień usług sieci web powinna być równa "1" w przypadku wdrożeń lokalnych.

Aby dowiedzieć się, jak skonfigurować różne wartości dla różnych środowisko, zobacz [Zarządzanie parametry aplikacji w wielu środowiskach](service-fabric-manage-multiple-environment-app-configuration.md).

## <a name="next-steps"></a>Następne kroki

- [Utworzyć klaster w Azure do wdrażania aplikacji w chmurze](service-fabric-cluster-creation-via-portal.md)
- [Dowiedz się więcej o komunikowania się z usługami](service-fabric-connect-and-communicate-with-services.md)
- [Dowiedz się więcej o podziału stanowe usług](service-fabric-concepts-partitioning.md)

<!-- Image References -->

[vs-add-new-service]: ./media/service-fabric-add-a-web-frontend/vs-add-new-service.png
[vs-new-service-dialog]: ./media/service-fabric-add-a-web-frontend/vs-new-service-dialog.png
[vs-new-aspnet-project-dialog]: ./media/service-fabric-add-a-web-frontend/vs-new-aspnet-project-dialog.png
[browser-aspnet-template-values]: ./media/service-fabric-add-a-web-frontend/browser-aspnet-template-values.png
[vs-add-class-library-project]: ./media/service-fabric-add-a-web-frontend/vs-add-class-library-project.png
[vs-add-class-library-reference]: ./media/service-fabric-add-a-web-frontend/vs-add-class-library-reference.png
[vs-services-nuget-package]: ./media/service-fabric-add-a-web-frontend/vs-services-nuget-package.png
[browser-aspnet-counter-value]: ./media/service-fabric-add-a-web-frontend/browser-aspnet-counter-value.png
[vs-configuration-manager]: ./media/service-fabric-add-a-web-frontend/vs-configuration-manager.png
[vs-create-platform]: ./media/service-fabric-add-a-web-frontend/vs-create-platform.png


<!-- external links -->
[dotnetcore-install]: https://www.microsoft.com/net/core#windows
[api-management-landing-page]: https://azure.microsoft.com/en-us/services/api-management/
