<properties
   pageTitle="Używanie Elasticsearch jako magazynem tkaninie usługi aplikacji śledzenia | Microsoft Azure"
   description="W tym artykule opisano, jak aplikacje usług tkaninie Elasticsearch i umożliwia Kibana przechowywania, indeks i wyszukiwania do śledzenia aplikacji (Dzienniki)"
   services="service-fabric"
   documentationCenter=".net"
   authors="karolz-ms"
   manager="adegeo"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/09/2016"
   ms.author="karolz@microsoft.com"/>

# <a name="use-elasticsearch-as-a-service-fabric-application-trace-store"></a>Używanie Elasticsearch jako śledzenia aplikacji tkaninie usługi przechowywania
## <a name="introduction"></a>Wprowadzenie
W tym artykule opisano, jak aplikacje [Tkaninie usługi Azure](https://azure.microsoft.com/documentation/services/service-fabric/) mogą używać **Elasticsearch** i **Kibana** dla aplikacji śledzenia miejsca do magazynowania, indeksowanie i wyszukiwanie. [Elasticsearch](https://www.elastic.co/guide/index.html) jest Otwórz źródło, rozłożone i skalowalna w czasie rzeczywistym wyszukiwania i analizy aparatu nadają się do tego zadania. Czy można zainstalować na Windows i Linux maszyn wirtualnych z systemem platformy Microsoft Azure. Elasticsearch wydajność może przetworzyć *strukturalnych* śledzenia wyprodukowane przy użyciu technologii, takich jak **Zdarzenia śledzenia systemu Windows (ETW)**.

Funkcja ETW jest używany przez środowisko uruchomieniowe usługi tkaninie do źródła informacji diagnostycznych (śledzenia). Jest zalecana metoda dla aplikacji usługi tkaninie zbyt źródła ich informacje diagnostyczne. Za pomocą ten sam mechanizm umożliwia korelacji między dostarczonym środowisko uruchomieniowe i aplikacja dostarczona śledzenia i wprowadzić w nim rozwiązania problemu. Szablony projektów tkaninie usługi w programie Visual Studio zawiera interfejs API rejestrowania (oparty na klasie.NET **źródła zdarzeń** ), który emituje śledzenia ETW domyślnie. Ogólne omówienie śledzenia aplikacji usługi tkaninie przy użyciu ETW zobacz [Monitorowanie i diagnozowania usług w ustawieniach rozwoju komputer lokalny](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).

Do śledzenia się pojawić Elasticsearch muszą przechwytywane w węzłach tkaninie usługi w czasie rzeczywistym (po uruchomieniu aplikacji) oraz wysyłane do punktu końcowego Elasticsearch. Istnieją dwa główne sposoby śledzenia rejestrowania:

+ **W trakcie rejestrowania śledzenia**  
Aplikacji lub bardziej precyzyjne proces usługi jest odpowiedzialny za wysyłanie danych diagnostycznych w magazynie śledzenia (Elasticsearch).

+ **Przechwytywanie w nowym oknie proces śledzenia**  
Agent osobnych jest przechwytywanie śledzenia z procesu usługi lub procesów i wysyłając je w magazynie śledzenia.

Poniżej firma Microsoft opisano, jak skonfigurować Elasticsearch Azure, omówiono zalety i wady dla obu opcji przechwytywania i wyjaśniono, jak skonfigurować usługę tkaninie usługi wysyłanie danych do Elasticsearch.


## <a name="set-up-elasticsearch-on-azure"></a>Konfigurowanie Elasticsearch Azure
Jest najbardziej proste sposób konfigurowania usługi Elasticsearch Azure za pomocą [**Menedżera zasobów Azure szablonów**](../azure-resource-manager/resource-group-overview.md). Pełna [szablon Menedżera zasobów Azure Szybki Start dla Elasticsearch](https://github.com/Azure/azure-quickstart-templates/tree/master/elasticsearch) jest dostępny w repozytorium szablonów Azure Szybki Start. Ten szablon używa oddzielnego przechowywania kont dla skali (grupy węzłów). Można go również obsługi administracyjnej osobnych klienta i węzłach serwerów w różnych konfiguracjach i różne liczby dołączone dyski danych.

W tym miejscu firma Microsoft korzysta z innego szablonu o nazwie **ES MultiNode** z [repozytorium Azure narzędzia diagnostyczne](https://github.com/Azure/azure-diagnostics-tools). Ten szablon jest łatwiejsze w użyciu i tworzy klaster Elasticsearch chroniony przez HTTP uwierzytelniania podstawowego. Przed kontynuowaniem, Pobierz repozytorium ze GitHub na komputerze (przez klonowanie repozytorium lub pobieranie pliku zip). Szablon ES MultiNode znajduje się w folderze o takiej samej nazwie.

### <a name="prepare-a-machine-to-run-elasticsearch-installation-scripts"></a>Przygotowywanie komputera do uruchamiania skryptów instalacji Elasticsearch
Najprostszym sposobem na używanie szablonu ES MultiNode jest podany skrypt programu PowerShell Azure o nazwie `CreateElasticSearchCluster`. Aby użyć tego skryptu, należy zainstalować moduły programu PowerShell i narzędzie **openssl**. Te ostatnie są potrzebne do tworzenia klucza SSH, który może być używany do zdalnego administrowania klaster Elasticsearch.

`CreateElasticSearchCluster`skrypt jest przeznaczony dla ułatwienia pracy z szablonem ES MultiNode na komputerze z systemem Windows. Użytkownik może korzystać z szablonu na komputerze — Windows, ale tego scenariusza wykracza poza zakres tego artykułu.

1. Jeśli jeszcze nie zainstalowano ich już, należy zainstalować [**moduły Azure programu PowerShell**](http://aka.ms/webpi-azps). Po wyświetleniu monitu kliknij przycisk **Uruchom**, następnie **zainstalować**. Wymagane jest Azure PowerShell 1.3 lub nowszego.

2. Narzędzie **openssl** jest dołączone do dystrybucji [**Cyfra dla systemu Windows**](http://www.git-scm.com/downloads). Jeśli jeszcze tego nie zrobiono, zainstaluj teraz [Cyfra dla systemu Windows](http://www.git-scm.com/downloads) . (Domyślne opcje instalacji są OK).

3. Przy założeniu, że cyfra został zainstalowany, ale nie są uwzględniane w ścieżce systemu, Otwórz okno programu Microsoft Azure programu PowerShell i uruchom następujące polecenia:

    ```powershell
    $ENV:PATH += ";<Git installation folder>\usr\bin"
    $ENV:OPENSSL_CONF = "<Git installation folder>\usr\ssl\openssl.cnf"
    ```

    Zamienianie `<Git installation folder>` z lokalizacji cyfra na tym komputerze; Wartość domyślna to **"C:\Program Files\Git"**. Uwaga średnika na początku pierwszej ścieżki.

4. Upewnij się, że użytkownik jest zalogowany do Azure (za pośrednictwem [`Add-AzureRmAccount`](https://msdn.microsoft.com/library/mt619267.aspx) polecenia cmdlet), jeśli wybrano subskrypcję, do której należy utworzyć klaster elastyczne wyszukiwania. Możesz sprawdzić poprawne subskrypcji jest zaznaczone, przy użyciu `Get-AzureRmContext` i `Get-AzureRmSubscription` poleceń cmdlet.

5. Jeśli nie zrobiono tego wcześniej, zmienić bieżącego katalogu do folderu ES MultiNode.

### <a name="run-the-createelasticsearchcluster-script"></a>Uruchamianie skryptu CreateElasticSearchCluster
Przed uruchomieniem skryptu Otwórz `azuredeploy-parameters.json` plików i sprawdź i podaj wartości parametrów skryptu. Dostępne są następujące parametry:

|Nazwa parametru           |Opis|
|-----------------------  |--------------------------|
|dnsNameForLoadBalancerIP |Nazwa, która służy do tworzenia widoczna publicznie nazwę DNS klaster elastyczne wyszukiwania (dołączając do podanej nazwy domeny Azure regionu). Jeśli wartość parametru jest "myBigCluster" i wybranym region Azure jest zachód USA, wynikowy nazwę DNS klaster jest myBigCluster.westus.cloudapp.azure.com. <br /><br />Ta nazwa służy także nazwa głównego dla wielu artefaktów skojarzone z klastrem elastyczne wyszukiwania, takie jak nazwy węzłów danych.|
|adminUsername           |Nazwa konta administratora dla zarządzania klastrem elastyczne wyszukiwania (odpowiednie klucze SSH powstają automatycznie).|
|dataNodeCount           |Liczba węzłów w klastrze elastyczne wyszukiwania. Bieżąca wersja skrypt nie rozróżnia węzłów danych i kwerendy; wszystkie węzły odtwarzanie obie role. Domyślnie węzły 3.|
|dataDiskSize            |Rozmiar danych dyski (GB) przydzielona dla każdego węzła danych. Każdy węzeł otrzymuje 4 dyski danych, przeznaczone wyłącznie do usługi wyszukiwania elastyczne.|
|region                  |Nazwa Azure region, gdzie powinien znajdować się klaster elastyczne wyszukiwania.|
|esUserName              |Nazwa użytkownika użytkownika, który jest skonfigurowany do mają dostęp do klastrów ES (naliczone uwierzytelnianie podstawowe HTTP). Hasło nie jest częścią pliku parametrów i należy podać kiedy `CreateElasticSearchCluster` skrypt zostanie wywołany.|
|vmSizeDataNodes         |Rozmiar Azure maszyn wirtualnych węzłach elastyczne wyszukiwania. Domyślnie Standard_D2.|

Teraz możesz przystąpić do uruchomienia skryptu. Wydaj następujące polecenie:

```powershell
CreateElasticSearchCluster -ResourceGroupName <es-group-name> -Region <azure-region> -EsPassword <es-password>
```

gdzie 

|Nazwa parametru skryptu    |Opis|
|-----------------------  |--------------------------|
|`<es-group-name>`        |Nazwa grupy zasobów Azure, zawierające wszystkie zasoby klaster elastyczne wyszukiwania.|
|`<azure-region>`         |Nazwa obszaru Azure miejsce, w którym można utworzyć klaster elastyczne wyszukiwania.|         
|`<es-password>`          |Hasło użytkownika elastyczne wyszukiwania.|

>[AZURE.NOTE] Jeśli otrzymasz NullReferenceException z polecenia cmdlet Test AzureResourceGroup zapomnieli zalogować się do Azure (`Add-AzureRmAccount`).

Jeśli zostanie wyświetlony komunikat o błędzie na uruchamianie skryptu i określić komunikat o błędzie była spowodowana wartości parametru szablonu problem, popraw pliku parametrów i ponownie uruchom skrypt przy użyciu innego zasobu nazwy grupy. Można także ponownie użyć tej samej nazwie zasobu i skrypt Oczyszczanie starą przez dodanie `-RemoveExistingResourceGroup` parametr wywołanie skryptu.

### <a name="result-of-running-the-createelasticsearchcluster-script"></a>Wynik uruchamianie skryptu CreateElasticSearchCluster
Po uruchomieniu `CreateElasticSearchCluster` skrypt, poniższe artefakty głównym zostaną utworzone. W tym przykładzie przyjęto założenie, że zostały użyte "myBigCluster" jako wartość `dnsNameForLoadBalancerIP` parametru i że region, w którym utworzono klaster jest zachód USA.

|Artefaktu|Nazwa, lokalizacja i uwagi|
|----------------------------------|----------------------------------|
|Klucz SSH dla administracji zdalnej |Plik myBigCluster.key (w katalogu, z którego zostało uruchomione CreateElasticSearchCluster). <br /><br />Ten plik klucza można połączyć do węzła administratora i (za pośrednictwem węzła administrator) węzłów danych w grupie.|
|Węzeł administratora                        |myBigCluster admin.westus.cloudapp.azure.com <br /><br />Dedykowane maszyn wirtualnych dla administracji zdalnej klaster Elasticsearch — jedyną osobą, która zezwala na zewnętrzne połączenia SSH. Zostanie uruchomiony w tej samej sieci wirtualnej jako wszystkie węzły Elasticsearch, ale go nie można uruchomić usługi Elasticsearch.|
|Węzły danych                        |myBigCluster1... myBigCluster*N* <br /><br />Węzły danych, które Elasticsearch i Kibana usługi są uruchomione. Możesz nawiązać połączenie przy użyciu SSH do każdego węzła, ale tylko za pośrednictwem węzła administratora.|
|Klaster Elasticsearch             |http://myBigCluster.westus.cloudapp.Azure.com/ES/ <br /><br />Podstawowy punkt końcowy dla klaster Elasticsearch (Zwróć uwagę sufiks /es). Jest chroniony przez uwierzytelnianie podstawowe HTTP (poświadczenia były określonych parametrów esUserName i esPassword szablonu ES MultiNode). Klaster ma również głowy wtyczki zainstalowany (http://myBigCluster.westus.cloudapp.azure.com/es/_plugin/head) dla administracji klaster podstawowe.|
|Usługa Kibana                    |http://myBigCluster.westus.cloudapp.Azure.com <br /><br />Usługa Kibana jest skonfigurowana umożliwia wyświetlanie danych z utworzonym klastrze Elasticsearch. Jest chroniony przez tych samych poświadczeń uwierzytelniania jako klastrem.|

## <a name="in-process-versus-out-of-process-trace-capturing"></a>W trakcie i rejestrowania w nowym oknie proces śledzenia
Wprowadzenie, wspomniano dwa podstawowe sposoby zbierania danych diagnostycznych: w trakcie i out of process. Każda ma zalet i słabych.

Zaletami **Przechwytywanie śledzenia w trakcie** :

1. *Łatwe Konfiguracja i wdrożenie*

    * Konfiguracja zbioru danych diagnostycznych jest tylko część konfiguracji aplikacji. Łatwiej zawsze trzymać "synchronizacji" z pozostałą część aplikacji.

    * Konfiguracja aplikacji lub na usługi jest łatwe osiągnięcia.

    * Zazwyczaj przechwytywanie śledzenia w nowym oknie proces wymaga oddzielnych wdrażania i konfigurowania agentem diagnostyczne jest zadanie dodatkowe administracyjne i źródła potencjalne błędy. Technologia określoną agent umożliwia często tylko jednego wystąpienia agenta na maszyn wirtualnych (węzeł). Oznacza to tej konfiguracji zbiór konfigurację diagnostyki współużytkowany wszystkich aplikacji i usług uruchomionych na tym węźle.

2. *Elastyczność*

    * Aplikacja można wysyłać dane w miejsce, w którym należy przejść, dopóki jest biblioteka klienta, który obsługuje system magazynowania wybranych danych. Można dodawać nowe pochłaniacze pożądane.

    * Może być realizowana złożonych reguł przechwytywanie, filtrowania i agregacja danych.

    * Przechwytywanie śledzenia w nowym oknie proces często jest ograniczona przez pochłaniacze danych, obsługiwanych przez agenta. Niektóre agenci są extensible.

3. *Dostęp do danych aplikacji wewnętrznych i kontekstowe*

    * Diagnostyczne podsystemu uruchomiony proces aplikacji i usług łatwo można rozszerzyć śledzenia informacji kontekstowych.

    * W ujęciu out of process danych zostanie wysłana do agenta za pośrednictwem niektórych mechanizmu komunikacji między procesami, takich jak śledzenie zdarzeń dla systemu Windows. Ten mechanizm można nałożyć dodatkowe ograniczenia.

Zalety **Przechwytywanie śledzenia w nowym oknie proces** obejmują:

1. *Możliwość monitorowania zrzuty awarie aplikacji oraz odbiór*

    * Przechwytywanie śledzenia w trakcie może się nie powieść, jeśli aplikacji nie można uruchomić lub ulega awarii. Niezależny agent ma wiele zwiększa prawdopodobieństwo Przechwytywanie ważnych informacji dotyczących rozwiązywania problemów.<br /><br />

2. *Data_spłaty, niezawodności i wydajności sprawdzone*

    * Agent opracowanych przez sprzedawcę platformy (na przykład agenta Diagnostyka pakietu Microsoft Azure) zostało objęte dokładnych badania i funkcjonalności walki.

    * Przy użyciu śledzenia w trakcie Przechwytywanie musi być zadbać o zapewnienie aktywności przesyłania danych diagnostycznych z procesu aplikacji nie zakłócał wyrazistości główne zadania w aplikacji lub wprowadzić problemy z wydajnością lub termin. Niezależne uruchomionego agenta jest mniej podatne na tych problemów i przeznaczone do ograniczania ich wpływu na system.

Istnieje możliwość łączenia i korzystać z obu metod. W rzeczywistości być może jest najlepszym rozwiązaniem dla wielu aplikacji.

W tym miejscu, firma Microsoft korzysta z **biblioteki Microsoft.Diagnostic.Listeners** i śledzenia w trakcie rejestrowania wysyłanie danych z aplikacji usługi tkaninie klaster Elasticsearch.

## <a name="use-the-listeners-library-to-send-diagnostic-data-to-elasticsearch"></a>Wysyłanie danych diagnostycznych do Elasticsearch za pomocą biblioteki detektory
Biblioteka Microsoft.Diagnostic.Listeners jest częścią PartyCluster przykładowej tkaninie usługi aplikacji. Używanie tej funkcji:

1. Pobieranie [przykładowych PartyCluster](https://github.com/Azure-Samples/service-fabric-dotnet-management-party-cluster) z GitHub.

2. Skopiuj projekty Microsoft.Diagnostics.Listeners i Microsoft.Diagnostics.Listeners.Fabric (całe foldery) z katalogu próbki PartyCluster do folderu rozwiązania aplikacji, która ma zostać dane są wysyłane do Elasticsearch.

3. Otwórz rozwiązanie docelowej, kliknij prawym przyciskiem myszy węzeł rozwiązanie w Eksploratorze rozwiązań i wybierz pozycję **Dodaj istniejącego projektu**. Dodanie projektu Microsoft.Diagnostics.Listeners do rozwiązania. Powtórzenie tej samej projektu Microsoft.Diagnostics.Listeners.Fabric.

4. Dodawanie odwołanie projektu z usługi projekty usług do dwa projekty dodane. (Każdej usługi, który ma zostać wysyłanie danych do Elasticsearch powinna odwoływać się Microsoft.Diagnostics.EventListeners i Microsoft.Diagnostics.EventListeners.Fabric).

    ![Odwołania do bibliotek Microsoft.Diagnostics.EventListeners i Microsoft.Diagnostics.EventListeners.Fabric][1]

### <a name="service-fabric-general-availability-release-and-microsoftdiagnosticstracing-nuget-package"></a>Dostępność usługi tkaninie ogólne wersji oraz pakiet Microsoft.Diagnostics.Tracing Nuget
Aplikacji utworzonych za pomocą usługi tkaninie ogólne dostępności wersji (2.0.135 wydane 31 marca 2016) kierować **.NET Framework 4.5.2**. Ta wersja jest najwyższą wersję programu .NET Framework w chwili wydania GA obsługiwana przez Azure. Niestety ta wersja ramach brakuje niektórych interfejsy API EventListener, wymagającego biblioteki Microsoft.Diagnostics.Listeners. Ponieważ źródła zdarzeń (składnik podstawą rejestrowania w aplikacjach tkaninie interfejsy API) i EventListener ściśle związane, co projekt, który korzysta z biblioteki Microsoft.Diagnostics.Listeners, należy użyć alternatywnej implementacji źródła zdarzeń. Ta implementacja jest udostępniany przez **pakiet Microsoft.Diagnostics.Tracing Nuget**, utworzony przez firmę Microsoft. Pakiet jest w pełni zgodny z poprzednimi wersjami z źródła zdarzeń uwzględniona w ramach, więc żadne zmiany kodu powinny być konieczne niż zmiany nazw odwołania.

Aby rozpocząć korzystanie z protokołu Microsoft.Diagnostics.Tracing klasy źródła zdarzeń, wykonaj następujące czynności dla każdego projektu usługa, która wymaga wysyłanie danych do Elasticsearch:

1. Kliknij prawym przyciskiem myszy nad projektem usługi i wybierz pozycję **Zarządzaj Nuget pakietów**.

2. Przełącz się do źródła pakietu nuget.org (Jeśli nie została jeszcze zaznaczona), a następnie wyszukaj "**Microsoft.Diagnostics.Tracing**".

3. Instalowanie `Microsoft.Diagnostics.Tracing.EventSource` pakietu (i jego zależności).

4. Otwórz plik **ServiceEventSource.cs** lub **ActorEventSource.cs** w projekcie usługi i zamienić `using System.Diagnostics.Tracing` dyrektywy u góry pliku `using Microsoft.Diagnostics.Tracing` dyrektywy.

Poniższe czynności, nie będzie konieczne, gdy **program .NET Framework 4.6** jest obsługiwana przez Microsoft Azure.

### <a name="elasticsearch-listener-instantiation-and-configuration"></a>Wystąpienia odbiornika Elasticsearch i konfiguracji
Ostatnim krokiem wysyłania danych diagnostycznych Elasticsearch jest utworzenie wystąpienia `ElasticSearchListener` i skonfiguruj go z Elasticsearch połączenia danych. Odbiornik automatycznie rejestruje wszystkie zdarzenia wywoływane za pośrednictwem klas źródła zdarzeń określonych w programie project usługi. Wymagane jest aktywności w czasie użytkowania usługi, dlatego najlepiej utworzyć go w kodzie inicjowanie usługi. Poniżej opisano, jak kod inicjowanie stateless usługi może wyglądać po konieczne zmiany (dodatki wskazano w komentarzach, zaczynając od `****`):

```csharp
using System;
using System.Diagnostics;
using System.Fabric;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.ServiceFabric.Services.Runtime;

// **** Add the following directives
using Microsoft.Diagnostics.EventListeners;
using Microsoft.Diagnostics.EventListeners.Fabric;

namespace Stateless1
{
    internal static class Program
    {
        /// <summary>
        /// This is the entry point of the service host process.
        /// </summary>        
        private static void Main()
        {
            try
            {
                // **** Instantiate ElasticSearchListener
                var configProvider = new FabricConfigurationProvider("ElasticSearchEventListener");
                ElasticSearchListener esListener = null;
                if (configProvider.HasConfiguration)
                {
                    esListener = new ElasticSearchListener(configProvider);
                }

                // The ServiceManifest.XML file defines one or more service type names.
                // Registering a service maps a service type name to a .NET type.
                // When Service Fabric creates an instance of this service type,
                // an instance of the class is created in this host process.

                ServiceRuntime.RegisterServiceAsync("Stateless1Type", 
                    context => new Stateless1(context)).GetAwaiter().GetResult();

                ServiceEventSource.Current.ServiceTypeRegistered(Process.GetCurrentProcess().Id, typeof(Stateless1).Name);

                // Prevents this host process from terminating so services keep running.
                Thread.Sleep(Timeout.Infinite);

                // **** Ensure that the ElasticSearchListner instance is not garbage-collected prematurely
                GC.KeepAlive(esListener);
            }
            catch (Exception e)
            {
                ServiceEventSource.Current.ServiceHostInitializationFailed(e);
                throw;
            }
        }
    }
}
```

Elasticsearch połączenia danych należy umieścić w osobnej sekcji w pliku konfiguracji usługi (**PackageRoot\Config\Settings.xml**). Nazwa sekcji musi odpowiadać na wartość przekazywana do `FabricConfigurationProvider` konstruktora, na przykład:

```xml
<Section Name="ElasticSearchEventListener">
  <Parameter Name="serviceUri" Value="http://myBigCluster.westus.cloudapp.azure.com/es/" />
  <Parameter Name="userName" Value="(ES user name)" />
  <Parameter Name="password" Value="(ES user password)" />
  <Parameter Name="indexNamePrefix" Value="myapp" />
</Section>
```
Wartości `serviceUri`, `userName` i `password` parametrów odpowiadają adres końcowy klaster Elasticsearch, Elasticsearch nazwę użytkownika i hasło, odpowiednio. `indexNamePrefix`jest prefiks indeksów Elasticsearch; Biblioteka Microsoft.Diagnostics.Listeners codziennie tworzy nowy indeks dla wraz z danymi.

### <a name="verification"></a>Weryfikacja
To wszystko! Teraz gdy jest uruchomiona usługa, rozpoczyna się wysyłanie śledzenia z usługą Elasticsearch określony w konfiguracji. Można sprawdzić, czy to przez otwarcie Kibana interfejsu użytkownika skojarzonego z wystąpieniem Elasticsearch docelowej. W naszym przykładzie adres strony jest http://myBigCluster.westus.cloudapp.azure.com/. Sprawdź, czy indeksy z prefiksem nazwy wybrany dla `ElasticSearchListener` wystąpienie faktycznie zostały utworzone i zawierają dane.

![Pokazywanie Kibana zdarzenia aplikacji PartyCluster][2]

## <a name="next-steps"></a>Następne kroki
- [Dowiedz się więcej o diagnozowania i monitorowanie usługi tkaninie usługi](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

<!--Image references-->
[1]: ./media/service-fabric-diagnostics-how-to-use-elasticsearch/listener-lib-references.png
[2]: ./media/service-fabric-diagnostics-how-to-use-elasticsearch/kibana.png
