<properties
   pageTitle="Dodawanie niestandardowych raporty dotyczące kondycji usługi tkaninie | Microsoft Azure"
   description="W tym artykule opisano, jak wysyłać raporty dotyczące kondycji niestandardowych do jednostki kondycji tkaninie usługi Azure. Zawiera zalecenia dotyczące projektowania i implementowania raporty dotyczące kondycji jakości."
   services="service-fabric"
   documentationCenter=".net"
   authors="oanapl"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/28/2016"
   ms.author="oanapl"/>

# <a name="add-custom-service-fabric-health-reports"></a>Dodawanie niestandardowych raporty dotyczące kondycji usługi tkaninie
Azure tkaninie usługi wprowadza [modelu](service-fabric-health-introduction.md) przeznaczony do flagi nieprawidłowe klaster i aplikacja warunki określone osoby. Model kondycji używa **podmioty kondycji** (składników systemu i watchdogs). Celem jest prosta i szybka diagnozowania i naprawiania. Moduły zapisujące usługi należy wziąć pod poświęcić dotyczące kondycji. Każdego warunku, który może mieć wpływ na kondycji należy podać, zwłaszcza jeśli może ułatwić problemów flagi zbliżony głównym. Informacje o kondycji można zapisać mnóstwo czasu i nakładu pracy w debugowania i badania raz usługę i rozpocząć pracę w skali w chmurze (prywatne lub Azure).

Monitorowanie podmioty tkaninie usługi określone warunki zainteresowania. Sprawozdanie one warunki, na podstawie ich lokalnych widoku. [Przechowywanie kondycji](service-fabric-health-introduction.md#health-store) agreguje dane kondycji wysyłane przez wszystkie podmioty w celu ustalenia, czy jednostki są globalnie prawidłowy. Model ma być sformatowany, elastyczne i łatwy w użyciu. Jakość raporty dotyczące kondycji określa dokładność widoku kondycji klaster. Wyniki fałszywie dodatnie błędnie pokazujące nieprawidłowe problemów może negatywny wpływ uaktualnienia lub innych usług, które używają danych kondycji. Przykładem usług są naprawy i mechanizmy alertów. Dlatego rozwagą jest potrzebna do zapewnienia raportów, które warunki odsetki w najlepszy sposób.

Do projektowania i implementacji kondycji raportowania, watchdogs i składniki systemu musi:

- Zdefiniuj warunek, w jakiej znajdują się one w sposób, który jest monitorowane i wpływu na funkcję klaster lub aplikacji. Na podstawie tych informacji, decyzje dotyczące właściwości raportu zdrowia i stanu kondycji.

- Określanie [jednostki](service-fabric-health-introduction.md#health-entities-and-hierarchy) , który dotyczy raport.

- Ustalanie, gdzie raportowania jest gotowe, z w usłudze lub watchdog wewnętrznych i zewnętrznych.

- Zdefiniuj źródła używane do identyfikowania reporter.

- Wybierz pozycję strategię raportowania, okresowo lub na przejścia. Zalecany sposób jest okresowo wymaga prostsze kodu i jest mniej podatne na błędy.

- Określ, jak długo raport na nieprawidłowe warunki powinny pozostać w magazynie kondycji i jak wyczyszczone. Za pomocą tych informacji, określ czas wygaśnięcia raportu i Usuń na wygasania zachowanie.

Jak wspomniano, raportowania można zrobić na:

- Monitorowane replice usługi tkaninie usługi.

- Wewnętrzna watchdogs używany jako usługa tkaninie usługi (na przykład tkaninie usługi stateless Usługa monitoruje warunków i problemy dotyczące raportów). Watchdogs może być używany wszystkie węzły lub może być affinitized z usługą monitorowane.

- Wewnętrzna watchdogs, uruchom w węzłach tkaninie usług, które są *nie* jest zaimplementowana jako usługi tkaninie usługi.

- Watchdogs zewnętrznych, których sondy zasób z *poza* klaster tkaninie usługi (na przykład monitorowania usługi, takiej jak Gomez).

> [AZURE.NOTE] Okno klaster zostanie wypełniona raporty dotyczące kondycji wysyłane przez składniki systemu. Dowiedz się więcej na [Używanie raportów dotyczących kondycji systemu do rozwiązywania problemów](service-fabric-understand-and-troubleshoot-with-system-health-reports.md). Raporty użytkownika należy przesłać [podmioty kondycji](service-fabric-health-introduction.md#health-entities-and-hierarchy) zostały już utworzone przez system.

Raz kondycji raportowania, że projekt jest wyczyszczone, raporty dotyczące kondycji mogą zostać wysłane łatwe. [FabricClient](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.aspx) służy do raportu kondycji Jeżeli klaster nie jest [bezpieczny](service-fabric-cluster-security.md) lub jeśli klient tkaninie ma uprawnienia administratora. Można to zrobić za pośrednictwem interfejsu API przy użyciu [FabricClient.HealthManager.ReportHealth](https://msdn.microsoft.com/library/system.fabric.fabricclient.healthclient.reporthealth.aspx), przy użyciu programu PowerShell lub pozostałych. Konfiguracja pokręteł partii raportów dla zwiększona wydajność.

> [AZURE.NOTE] Raport kondycji jest synchroniczne i przedstawia Praca w sprawdzania poprawności po stronie klienta. Fakt, że raport zostanie odebrane przez klienta kondycji lub `Partition` lub `CodePackageActivationContext` obiektów nie oznacza, że jest ono stosowane w magazynie. Jest wysyłana asynchroniczne i ewentualnie przetwarzany wsadowo razem z innymi raportami. Przetwarzanie na serwerze nadal może zakończyć się niepowodzeniem: numerem może być przestarzałe, obiektu, na którym musi być stosowana jako raport został usunięty, itd.

## <a name="health-client"></a>Kondycja klienta
Raporty dotyczące kondycji są wysyłane do sklepu kondycji za pośrednictwem klienta kondycja, która znajduje się w kliencie tkaninie. Klient kondycji można skonfigurować w następujących zakresach:

- **HealthReportSendInterval**: opóźnienie między czas raport zostanie dodany do klienta i są wysyłane do sklepu kondycji. Używane do raportów partii do pojedynczej wiadomości zamiast wysyłania jednej wiadomości dla każdego raportu. Tworzeniu partii zwiększa wydajność. Wartość domyślna: 30 sekund.

- **HealthReportRetrySendInterval**: interwał co kondycji klient wysyła ponownie zakumulowaną kondycji raportów w magazynie kondycji. Wartość domyślna: 30 sekund.

- **HealthOperationTimeout**: limit czasu dla raportu wiadomości wysyłane do sklepu kondycji. Jeśli przekroczenie limitu czasu wiadomości, kondycji klient będzie ponawiał próbę go do momentu sklepu kondycji potwierdza przetwarzane raportu. Wartość domyślna: dwie minuty.

> [AZURE.NOTE] Gdy raporty są przetwarzany wsadowo, klienta tkaninie musi znajdować się aktywności dla co najmniej HealthReportSendInterval, aby upewnić się, że są wysyłane. Jeśli wiadomość jest tracone lub w sklepie kondycji nie można zastosować ich z powodu błędów przejściowych, klienta tkaninie musi znajdować się aktywności już aby nadać mu możliwość ponów próbę.

Buforowanie na komputerze klienckim trwa unikatowość raportów pod uwagę. Na przykład jeśli określonego reporter nieprawidłowe zgłoszenie 100 raportów na sekundę na tej samej właściwości tej samej jednostki, raporty są zastępowane ostatnią wersję. Tylko jeden taki raport istnieje co najwyżej w kolejce klienta. Jeśli skonfigurowano tworzeniu partii liczbę raportów wysyłane do sklepu kondycji jest tylko jeden interwał Wyślij. Ten raport jest ostatni raport dodano odzwierciedla najbardziej aktualnej jednostki.
Wszystkie parametry konfiguracji mogą być określony, kiedy `FabricClient` jest tworzona przekazując [FabricClientSettings](https://msdn.microsoft.com/library/azure/system.fabric.fabricclientsettings.aspx) z żądane wartości dla wpisów dotyczących zdrowia.

Poniższy tworzy klienta tkaninie i określa, że raporty powinny być wysyłane w przypadku, gdy są one dodawane. Limity czasu i błędy, które mogą być wycofane ponowne próby wystąpić co 40 sekund.

```csharp
var clientSettings = new FabricClientSettings()
{
    HealthOperationTimeout = TimeSpan.FromSeconds(120),
    HealthReportSendInterval = TimeSpan.FromSeconds(0),
    HealthReportRetrySendInterval = TimeSpan.FromSeconds(40),
};
var fabricClient = new FabricClient(clientSettings);
```

Po utworzeniu połączenia z klastrem przy użyciu programu PowerShell można określić same parametry. Poniższy rozpoczyna się połączenie lokalne klastrem:

```powershell
PS C:\> Connect-ServiceFabricCluster -HealthOperationTimeoutInSec 120 -HealthReportSendIntervalInSec 0 -HealthReportRetrySendIntervalInSec 40
True

ConnectionEndpoint   :
FabricClientSettings : {
                       ClientFriendlyName                   : PowerShell-1944858a-4c6d-465f-89c7-9021c12ac0bb
                       PartitionLocationCacheLimit          : 100000
                       PartitionLocationCacheBucketCount    : 1024
                       ServiceChangePollInterval            : 00:02:00
                       ConnectionInitializationTimeout      : 00:00:02
                       KeepAliveInterval                    : 00:00:20
                       HealthOperationTimeout               : 00:02:00
                       HealthReportSendInterval             : 00:00:00
                       HealthReportRetrySendInterval        : 00:00:40
                       NotificationGatewayConnectionTimeout : 00:00:00
                       NotificationCacheUpdateTimeout       : 00:00:00
                       }
GatewayInformation   : {
                       NodeAddress                          : localhost:19000
                       NodeId                               : 1880ec88a3187766a6da323399721f53
                       NodeInstanceId                       : 130729063464981219
                       NodeName                             : Node.1
                       }
```

> [AZURE.NOTE] Aby upewnić się, że nieautoryzowanych usług nie można zgłosić zdrowia przed podmioty w klastrze, serwer można skonfigurować do akceptowania wezwań na tylko z klientami zabezpieczonego. `FabricClient` Na potrzeby raportowania, musi mieć z włączonymi zabezpieczeniami mieć możliwość komunikowania się z klastrem (na przykład z uwierzytelniania Kerberos lub certyfikatu). Dowiedz się więcej o [zabezpieczeniach klaster](service-fabric-cluster-security.md).

## <a name="report-from-within-low-privilege-services"></a>Raport z poziomu usług niskim uprawnień
Z poziomu usług tkaninie usług, które nie mają dostępu administracyjnego do klastrów, możesz to zgłosić kondycji na podmioty z bieżącego kontekstu za pośrednictwem `Partition` lub `CodePackageActivationContext`.

- Bezstanowa usług za pomocą [IStatelessServicePartition.ReportInstanceHealth](https://msdn.microsoft.com/library/system.fabric.istatelessservicepartition.reportinstancehealth.aspx) raportowania w bieżącym wystąpieniu usług.

- Dla pełnowartościową usług za pomocą [IStatefulServicePartition.ReportReplicaHealth](https://msdn.microsoft.com/library/system.fabric.istatefulservicepartition.reportreplicahealth.aspx) raportowania w replice bieżącego.

- Raportowanie informacji dotyczących bieżącego obiektu partition za pomocą [IServicePartition.ReportPartitionHealth](https://msdn.microsoft.com//library/system.fabric.iservicepartition.reportpartitionhealth.aspx) .

- Raportowanie informacji dotyczących bieżącej aplikacji za pomocą [CodePackageActivationContext.ReportApplicationHealth](https://msdn.microsoft.com/library/system.fabric.codepackageactivationcontext.reportapplicationhealth.aspx) .

- Używanie [CodePackageActivationContext.ReportDeployedApplicationHealth](https://msdn.microsoft.com/library/system.fabric.codepackageactivationcontext.reportdeployedapplicationhealth.aspx) raportowania w bieżącej aplikacji wdrożony w bieżącym węźle.

- Używanie [CodePackageActivationContext.ReportDeployedServicePackageHealth](https://msdn.microsoft.com/library/system.fabric.codepackageactivationcontext.reportdeployedservicepackagehealth.aspx) informowanie o pakiet usług dla bieżącej aplikacji wdrożony w bieżącym węźle.

> [AZURE.NOTE] Wewnętrznie `Partition` oraz `CodePackageActivationContext` przytrzymaj klienta kondycji — można to skonfigurować przy użyciu ustawień domyślnych. Ta sama uwaga wyjaśnione [kondycji](service-fabric-report-health.md#health-client) klienta zastosowanie - raporty są przetwarzany wsadowo i wysyłane na czasomierz, dzięki czemu obiekty powinny być zachowywane aktywności mieć możliwość wysłać raport.

## <a name="design-health-reporting"></a>Projektowanie raportów dotyczące kondycji
Pierwszy krok do generowania raportów wysokiej jakości identyfikujący warunki, które mogą mieć wpływ kondycji usługi. Każdego warunku, które mogą być pomocne problemów flagi w usłudze lub klaster podczas jego uruchamiania — lub jeszcze lepiej, zanim się dzieje problemu — mogą potencjalnie Zapisz miliardy złotych. Zalety mniej czasu, mniejszej liczby godzin nocy wydatków wykrywanie i naprawianie problemów i nowszy zadowolenia klientów.

Po zidentyfikowaniu warunki autorzy watchdog chcesz się dowiedzieć się najlepszym sposobem monitorować ich równowagi między ogólnych i użyteczności. Na przykład można rozważyć usługa, która ma złożone obliczenia, które używają niektóre pliki tymczasowe w udziale. Watchdog można monitorować Udostępnij, aby upewnić się, że miejsca jest dostępna. Można go nasłuchują powiadomień o zmianach pliku lub katalogu. Można go zgłosić ostrzeżenie po osiągnięciu progu poświęcić i zgłaszać błąd, gdy polecenie Udostępnij jest pełny. Na ostrzeżenie, system napraw można uruchomić oczyszczanie starszych plików w udziale. W przypadku błędu systemu naprawy można przenieść replice usługi do innego węzła. Uwaga opisu województw warunek pod względem kondycji: stan warunku, który można uznać prawidłowy lub nieprawidłowe (ostrzeżenie lub błąd).

Po ustawieniu monitorowania szczegóły watchdog writer musi ustalanie, jak wdrażać watchdog. Jeśli warunki można ustalić z w ramach usługi, watchdog może być częścią samej usługi monitorowane. Na przykład kod usługi można sprawdzić zastosowania udostępnianie i raportować przy użyciu klienta lokalnego tkaninie każdorazowo spróbuje zapisać plik. Zaletą tej metody jest raportowanie prosty. Musi być zadbać o uniemożliwić usterek watchdog jednak wpływ na działanie usługi.

Raportowanie z poziomu usługi monitorowane nie zawsze jest odpowiednią opcję. Watchdog w usłudze nie można wykryć warunki. Nie mają logiczny lub dane, oznaczanie. Ogólnych monitorowania warunków może być duży. Warunki również nie mogą być specyficzne dla usługi, ale zamiast tego wpływa na interakcje między usługami. Innym rozwiązaniem jest watchdogs w klastrze jako oddzielne procesy. Watchdogs po prostu monitorowanie warunków i raportem, bez wpływu na głównym usług w jakikolwiek sposób. Na przykład te watchdogs może realizowane jako stateless usługi w tej samej aplikacji wdrożone na wszystkich węzłach lub na tym samym węzłach co usługa.

Czasami watchdog uruchamiania w klastrze jest opcja albo. Jeśli monitorowane warunek jest dostępności lub funkcje usługi, jak użytkownicy widzą go, jest najlepszym rozwiązaniem jest watchdogs w tym samym miejscu jako klienci użytkownika. Ich przetestować operacje w taki sam sposób, połączenie użytkowników. Na przykład możesz mieć watchdog, który znajduje się poza klastrem problemy żądania usługi, a następnie sprawdza opóźnienie i poprawności wyniku. (Usługi Kalkulatora, na przykład, czy 2 + 2 zwraca 4 w odpowiednim ilość czasu?)

Gdy szczegóły watchdog zostały ostatecznych, powinny zdecydować o identyfikator źródła, która jednoznacznie identyfikuje go. Jeśli wiele watchdogs tego samego typu mieszkających w klastrze, albo ich musisz zgłosić na różnych obiektów lub, jeśli oni raport na tej samej jednostki, muszą one zapewnić, że różni się identyfikator źródła lub właściwości. W ten sposób ich raporty mogą występować. Właściwość raport dotyczący kondycji należy przechwycić monitorowane warunku. (Na przykład powyżej, właściwość może być **ShareSize**). Jeśli wiele raportów odnosi się do tego samego warunku, właściwość powinny zawierać niektóre informacje dynamiczne, które umożliwiają raporty współistnienie. Na przykład jeśli wiele udziałów powinny być monitorowane, nazwa właściwości może być **udziału ShareSize**.

> [AZURE.NOTE] Należy w sklepie kondycji *nie* jest używany do przechowywania informacji o stanie. Tylko informacji dotyczących kondycji należy podać jako zdrowia, jak oceny kondycji jednostki wpływa na te informacje. W sklepie kondycji nie został zaprojektowany jako ogólnego przeznaczenia magazynu. Logika oceny kondycji używa sumować dane do stanu kondycji. Wysyłanie informacji o niepowiązane zdrowia (na przykład raportowania stanu przy użyciu stanu kondycji OK) nie będzie miało wpływ zagregowane kondycji, ale go mieć negatywny wpływ na wydajność magazynu kondycji.

Następny punktów decyzyjnych jest które jednostką do raportu na. W większości przypadków, jest to oczywiste, na podstawie warunku. Należy wybrać jednostkę z najlepsze możliwe szczegółowości. Jeśli warunek wpływa na wszystkie repliki w partycją, sprawozdanie partycją, a nie w usłudze. Istnieją przypadki rogu miejsce, w którym więcej myśli jest potrzebne, mimo że. Jeśli warunek ma wpływ obiekt, taki jak replice, ale woli ma warunek flagą więcej niż czas trwania życia replice, a następnie należy podać na partycją. W przeciwnym razie po usunięciu replice wszystkie raporty skojarzone z nim będzie można oczyścić z magazynu. Oznacza to, że autorzy watchdog również należy wziąć pod uwagę istnienia podmiotu i raportu. Musi to być Wyczyść, gdy można wyczyścić raportu z magazynu (na przykład gdy błąd raportowanie jednostka nie ma zastosowania).

Przyjrzyjmy się przykład umieszcza razem punktów, które można opisem. Należy rozważyć, czy aplikacji usługi tkaninie składający się z głównego stanowe trwałych pomocniczej stateless usługi i używany we wszystkich węzłach (jeden typ pomocniczej usługi dla każdego typu zadania). Wybrany wzorzec ma kolejki przetwarzania, która zawiera polecenia, które ma być wykonane przez pomocnicze. Pomocnicze wykonywania przychodzących żądań i Wyślij sygnały potwierdzenia Wstecz. Jeden warunek, który może być monitorowane jest długość kolejki przetwarzania wzorca. Jeśli długość kolejki wzorcowej osiągnie próg, jest zgłaszany ostrzeżenie. Ostrzeżenie wskazuje, że pomocnicze nie obsługuje Załaduj. Jeśli kolejce osiągnie maksymalna długość i poleceń nie są wyświetlane, zwróci błąd, jak usługa nie można odzyskać. Raporty można we właściwościach **QueueStatus**. Watchdog znajduje się wewnątrz usługę i są wysyłane okresowo w głównym replice podstawowego. Time to live jest dwie minuty i są wysyłane okresowo co 30 sekund. Jeśli podstawowy ulegnie uszkodzeniu, raport jest automatycznie czyszczone z magazynu. Jeśli w replice usługi jest uruchomiony, ale jest on zakleszczone lub inne problemy raport wygaśnie w magazynie kondycji. W tym przypadku jednostki są obliczane błędu.

Inny warunek, który można monitorować jest czas wykonywania zadania. Wzorzec rozdziela zadań w celu pomocnicze, w zależności od typu zadania. W zależności od projektu wzorcu może skorzystać pomocnicze dla o stanie zadań. Można też zaczekać, aż pomocnicze wysłać potwierdzenie wstecz sygnalizacji po wykonaniu. W drugim przypadku musi być zadbać o wykryć sytuacje, w którym struktury pomocnicze lub wiadomości zostaną utracone. Jedną z opcji jest wzorca wysłać żądanie ping do tego samego pomocniczą, która odsyła stanu. Odebranie nie stanu wzorcu uznaje awarii i zmienia harmonogram zadania. To zachowanie przyjęto założenie, że zadania są idempotent.

Jeśli zadanie nie odbywa się w określonym czasie (**t1**, na przykład 10 minut) można tłumaczyć monitorowane warunek jako ostrzeżenie. Jeśli zadanie nie zostało ukończone w czasie (**t2**, na przykład 20 minut), można tłumaczyć monitorowane warunek jako błąd. Ten raportowania można zrealizować na różne sposoby:

- Główny replice podstawowego raportów na samej okresowo. Możesz mieć jedną właściwość dla wszystkich zadań oczekujących w kolejce. Jeśli co najmniej jedno zadanie trwa dłużej, raportować stan właściwości **PendingTasks** jest ostrzeżenie lub błąd, zależnie od potrzeb. Jeśli nie ma żadnych oczekujących zadań lub wszystkie zadania rozpoczęte wykonanie, raportować stan jest przycisk OK. Zadania są trwałych. Podstawowy przejście w dół, można kontynuować raport prawidłowo nowo utworzonego podstawowa.

- Inny proces watchdog (w chmurze lub zewnętrzne) sprawdza zadania (z zewnątrz na podstawie wyniku żądane zadanie) aby zobaczyć, jeśli są one wypełnione. Jeśli nie przestrzegania progów, raportu są wysyłane w usłudze wzorca. Raport są również wysyłane dla każdego zadania, która zawiera identyfikator zadania, takie jak **PendingTask + taskId**. Raporty powinna być wysyłana tylko na nieprawidłowe stany. Ustawianie czasu na żywo kilka minut i oznaczanie raporty ma zostać usunięty, gdy wygasną zapewnienie oczyszczanie.

- Pomocniczego, który wykonuje zadania raportów, gdy trwa dłużej niż oczekiwano, aby go uruchomić. Przekazuje w wystąpieniu usług we właściwościach **PendingTasks**. Raport określa wystąpienie usługi, które występują problemy, ale nie Przechwytywanie sytuacji, w której wystąpienie umiera. Raporty są następnie czyszczone. Można go raportować w usłudze pomocniczą. Jeśli pomocniczej wykonuje zadanie, pomocniczej wystąpienie czyści raportu z magazynu. Raport nie przechwytywać sytuację miejsce, w którym komunikacie potwierdzenia zostaną utracone, a zadanie nie zostało zakończone z głównego punktu widzenia.

Jednak raportowania odbywa się w przypadkach opisanych powyżej, raporty są przechwytywane kondycji aplikacji gdy szacowana jest funkcja kondycji.

## <a name="report-periodically-vs-on-transition"></a>Sprawozdanie okresowo a przejścia
Przy użyciu modelu raportowania kondycji watchdogs można wysłać raporty okresowo lub na przejścia. Zalecany sposób raportowaniu watchdog jest okresowe, ponieważ kod jest o wiele łatwiejsze i mniej podatne na błędy. Watchdogs dążyć musi być tak proste, jak to możliwe uniknąć błędów wywołujących niepoprawne raportów. Niepoprawne *nieprawidłowe* raporty wpływają na ocen kondycji i scenariusze oparte na zdrowie, włączając uaktualnienia. Raporty niepoprawne *prawidłowy* ukryć problemy w klastrze, który nie jest wymagana.

Na potrzeby raportowania okresowych watchdog może nastąpić z zegarem. Watchdog zwrotnego czasomierz, można sprawdzić stan i wysłać raport, na podstawie bieżącego stanu. Istnieje bez konieczności Zobacz, który raport została wysłana wcześniej lub optymalizacji w odniesieniu do wiadomości. Klient kondycji ma, tworzeniu partii logiki do pomocy z wydajnością. Gdy klient kondycji jest życiu, go prób wewnętrznie do momentu raport zostanie potwierdzone w sklepie kondycji lub watchdog generuje nowszego raport o tej samej jednostki, właściwości i źródła.

Raportowanie na przejścia wymaga staranne obsługi o stanie. Watchdog monitoruje niektóre warunki w raportach i zmienić tylko wtedy, gdy warunki. Odwróć z tej metody to, że mniej raporty są potrzebne. Wadą podwyższonego jest złożone logiki watchdog. Watchdog muszą utrzymać warunki lub raportów, dlatego może być kontrolowane ustalenie zmian stanu. Na awaryjnym przeniesieniu należy zachować ostrożność po wysłaniu raportu którą może nie zostały wysłane wcześniej (w kolejce, ale jeszcze nie zostały wysłane do sklepu kondycji). Numerem musi być stale rosnącej. W przeciwnym razie raportów odrzucone jako stałe. Czasami miejsce, w którym poniesione utracie danych synchronizacja może być potrzebne między stanem reporter i stanu magazynie kondycji.

Raportowanie na przejścia ma sens dla usług raportowania na siebie, podczas `Partition` lub `CodePackageActivationContext`. Gdy lokalny obiekt (replice lub wdrożonym usługi pakietu / wdrożyć aplikację) jest usunięte, wszystkie jej raporty usuwane są również. Ten automatycznego oczyszczania dotyczących potrzebę synchronizacji między reporter i kondycji magazynu. Jeśli raport jest partycją nadrzędnego lub aplikacji nadrzędnej, należy na pracy awaryjnej w celu uniknięcia starych raportów w magazynie kondycji. Logika musi zostać dodana do utrzymywania poprawnego stanu i wyczyść raportu z magazynu, gdy nie jest już potrzebna.

## <a name="implement-health-reporting"></a>Implementowanie raportowania kondycji
Po jednostki i Raport szczegółów Wyczyść, wysyłając raporty dotyczące kondycji można przeprowadzić za pośrednictwem interfejsu API programu PowerShell i pozostałe.

### <a name="api"></a>INTERFEJS API
Aby zgłosić za pośrednictwem interfejsu API, należy utworzyć raport dotyczący kondycji specyficzne dla typu obiektu, które mają być sprawozdanie. Określ raportu do klienta kondycji. Możesz również utworzyć informacje dotyczące kondycji i przekazać go, aby poprawić metod zgłaszania na `Partition` lub `CodePackageActivationContext` informowanie o bieżącej jednostki.

W poniższym przykładzie pokazano okresowych raportów z watchdog w klastrze. Watchdog sprawdza, czy zasób zewnętrzne są dostępne z poziomu węzła. Zasób jest potrzebny manifestu usługi aplikacji. Zasób jest niedostępny, inne usługi w aplikacji można nadal działać prawidłowo. Dlatego raportu są wysyłane w jednostce pakietu usługi wdrożonym co 30 sekund.

```csharp
private static Uri ApplicationName = new Uri("fabric:/WordCount");
private static string ServiceManifestName = "WordCount.Service";
private static string NodeName = FabricRuntime.GetNodeContext().NodeName;
private static Timer ReportTimer = new Timer(new TimerCallback(SendReport), null, 30 * 1000, 30 * 1000);
private static FabricClient Client = new FabricClient(new FabricClientSettings() { HealthReportSendInterval = TimeSpan.FromSeconds(0) });

public static void SendReport(object obj)
{
    // Test whether the resource can be accessed from the node
    HealthState healthState = this.TestConnectivityToExternalResource();

    // Send report on deployed service package, as the connectivity is needed by the specific service manifest
    // and can be different on different nodes
    var deployedServicePackageHealthReport = new DeployedServicePackageHealthReport(
        ApplicationName,
        ServiceManifestName,
        NodeName,
        new HealthInformation("ExternalSourceWatcher", "Connectivity", healthState));

    // TODO: handle exception. Code omitted for snippet brevity.
    // Possible exceptions: FabricException with error codes
    // FabricHealthStaleReport (non-retryable, the report is already queued on the health client),
    // FabricHealthMaxReportsReached (retryable; user should retry with exponential delay until the report is accepted).
    Client.HealthManager.ReportHealth(deployedServicePackageHealthReport);
}
```

### <a name="powershell"></a>Programu PowerShell
Wyślij raporty dotyczące kondycji z *obiektu dla *ServiceFabric Wyślij**HealthReport **.

W poniższym przykładzie pokazano okresowych raportów o wartościach Procesora w węźle. Raporty mają być wysyłane co 30 sekund, a mają czasu wygaśnięcia dwie minuty. Jeśli wygasną, reporter występują problemy, więc węzeł jest obliczane błędu. Gdy Procesor znajduje się powyżej progu, raport zawiera kondycję ostrzeżenia. Gdy CPU pozostaje powyżej progu więcej niż skonfigurowanego czasu, jest zgłaszany jako błąd. W przeciwnym razie reporter wysyła stanu kondycji przycisk OK.

```powershell
PS C:\> Send-ServiceFabricNodeHealthReport -NodeName Node.1 -HealthState Warning -SourceId PowershellWatcher -HealthProperty CPU -Description "CPU is above 80% threshold" -TimeToLiveSec 120

PS C:\> Get-ServiceFabricNodeHealth -NodeName Node.1
NodeName              : Node.1
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='PowershellWatcher', Property='CPU', HealthState='Warning', ConsiderWarningAsError=false.

HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 5
                        SentAt                : 4/21/2015 8:01:17 AM
                        ReceivedAt            : 4/21/2015 8:02:12 AM
                        TTL                   : Infinite
                        Description           : Fabric node is up.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/21/2015 8:02:12 AM

                        SourceId              : PowershellWatcher
                        Property              : CPU
                        HealthState           : Warning
                        SequenceNumber        : 130741236814913394
                        SentAt                : 4/21/2015 9:01:21 PM
                        ReceivedAt            : 4/21/2015 9:01:21 PM
                        TTL                   : 00:02:00
                        Description           : CPU is above 80% threshold
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Warning = 4/21/2015 9:01:21 PM
```

Poniższy przykład raportów przejściowych ostrzeżenie w replice. Go najpierw pobiera identyfikator partition, a następnie identyfikator replice usługę, którą jest chcą. Następnie wysyła raportu z **PowershellWatcher** we właściwościach **ResourceDependency**. Raport jest potrzebnych dla tylko dwie minuty, a zostanie usunięty z magazynu automatycznie.

```powershell
PS C:\> $partitionId = (Get-ServiceFabricPartition -ServiceName fabric:/WordCount/WordCount.Service).PartitionId

PS C:\> $replicaId = (Get-ServiceFabricReplica -PartitionId $partitionId | where {$_.ReplicaRole -eq "Primary"}).ReplicaId

PS C:\> Send-ServiceFabricReplicaHealthReport -PartitionId $partitionId -ReplicaId $replicaId -HealthState Warning -SourceId PowershellWatcher -HealthProperty ResourceDependency -Description "The external resource that the primary is using has been rebooted at 4/21/2015 9:01:21 PM. Expect processing delays for a few minutes." -TimeToLiveSec 120 -RemoveWhenExpired

PS C:\> Get-ServiceFabricReplicaHealth  -PartitionId $partitionId -ReplicaOrInstanceId $replicaId


PartitionId           : 8f82daff-eb68-4fd9-b631-7a37629e08c0
ReplicaId             : 130740415594605869
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='PowershellWatcher', Property='ResourceDependency', HealthState='Warning', ConsiderWarningAsError=false.

HealthEvents          :
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 130740768777734943
                        SentAt                : 4/21/2015 8:01:17 AM
                        ReceivedAt            : 4/21/2015 8:02:12 AM
                        TTL                   : Infinite
                        Description           : Replica has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/21/2015 8:02:12 AM

                        SourceId              : PowershellWatcher
                        Property              : ResourceDependency
                        HealthState           : Warning
                        SequenceNumber        : 130741243777723555
                        SentAt                : 4/21/2015 9:12:57 PM
                        ReceivedAt            : 4/21/2015 9:12:57 PM
                        TTL                   : 00:02:00
                        Description           : The external resource that the primary is using has been rebooted at 4/21/2015 9:01:21 PM. Expect processing delays for a few minutes.
                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : ->Warning = 4/21/2015 9:12:32 PM
```

### <a name="rest"></a>POZOSTAŁE
Wyślij raporty dotyczące kondycji za pomocą usługi REST żądaniami WPIS, przejdź do odpowiedniej jednostki, które w treści opis raportu kondycji. Na przykład Zobacz, jak można wysłać pozostałych [Raporty dotyczące kondycji klaster](https://msdn.microsoft.com/library/azure/dn707640.aspx) lub [Raporty dotyczące kondycji usługi](https://msdn.microsoft.com/library/azure/dn707640.aspx). Wszystkie obiekty są obsługiwane.

## <a name="next-steps"></a>Następne kroki

Na podstawie danych kondycji, autorzy usługi i Administratorzy klaster i aplikacji można traktować sposoby wykorzystać informacje. Na przykład ich można skonfigurować alerty na podstawie stanu kondycji efektywnej duże problemy przed ich powodować dostawie. Administratorzy mogą również skonfigurować systemów naprawy Aby automatycznie rozwiązać problemy.

[Wprowadzenie do usługi tkaninie kondycji monitorowania](service-fabric-health-introduction.md)

[Wyświetlanie raportów dotyczących kondycji usługi tkaninie](service-fabric-view-entities-aggregated-health.md)

[Jak zgłaszać i sprawdzanie kondycji usługi](service-fabric-diagnostics-how-to-report-and-check-service-health.md)

[Używanie raportów dotyczących kondycji systemu do rozwiązywania problemów](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)

[Monitorowanie i diagnozowanie usług lokalnie](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Uaktualnianie aplikacji tkaninie usługi](service-fabric-application-upgrade.md)
