<properties
    pageTitle="Zmniejszyć wydajność aplikacji sieci web w aplikacji usługi | Microsoft Azure"
    description="Ten artykuł ułatwia rozwiązać problemy z wydajnością aplikacji wolno działającej sieci web w usłudze Azure aplikacji."
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""
    tags="top-support-issue"
    keywords="wydajność aplikacji sieci Web, aplikacja działa wolno aplikacji działa wolno"/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/06/2016"
    ms.author="cephalin"/>

# <a name="troubleshoot-slow-web-app-performance-issues-in-azure-app-service"></a>Rozwiązać problemy z wydajnością aplikacji wolno działającej sieci web w usłudze Azure aplikacji

Ten artykuł ułatwia rozwiązać problemy z wydajnością aplikacji wolno działającej sieci web w [Usłudze Azure aplikacji](http://go.microsoft.com/fwlink/?LinkId=529714).

Jeśli potrzebujesz dodatkowej pomocy w dowolnym momencie, w tym artykule, można skontaktować się Azure ekspertów na [MSDN Azure i fora przepełnienie stosu](https://azure.microsoft.com/support/forums/). Ponadto można również pliku Azure pomocy technicznej zdarzenia. Przejdź do [witryny pomocy technicznej Azure](https://azure.microsoft.com/support/options/) i kliknij pozycję **Uzyskiwanie pomocy technicznej**.

## <a name="symptom"></a>Symptom

Podczas przeglądania aplikacji web app ładowania stron powoli i czasami limitu czasu.

## <a name="cause"></a>Przyczyna

Przyczyną tego problemu jest często problemy z poziomu aplikacji, takich jak:

-   żądania zbyt długo
-   aplikacji przy użyciu wysoka pamięci i Procesor
-   Aplikacja awarii z powodu wyjątku.

## <a name="troubleshooting-steps"></a>Procedura rozwiązywania problemów

Rozwiązywanie problemów z można podzielić na trzy różne zadania w kolejności:

1.  [Obserwowanie i monitorować zachowanie aplikacji](#observe)
2.  [Zbieranie danych](#collect)
3.  [Ograniczanie problem](#mitigate)

[Aplikacji sieci Web usługi](/services/app-service/web/) zapewnia różne opcje na poszczególnych etapach.

<a name="observe" />
### <a name="1-observe-and-monitor-application-behavior"></a>1. obserwować i monitorować zachowanie aplikacji

#### <a name="track-service-health"></a>Śledzenie kondycja usługi

Microsoft Azure publicizes każdym razem, gdy jest przerwy lub wydajności obniżenie wydajności usługi. Możesz śledzić kondycji usługi na [Azure Portal](https://portal.azure.com/). Aby uzyskać więcej informacji zobacz [śledzenie kondycji usługi](../monitoring-and-diagnostics/insights-service-health.md).

#### <a name="monitor-your-web-app"></a>Monitorowanie aplikacji sieci web

Ta opcja umożliwia Dowiedz się, jeśli masz problemy aplikacji. W aplikacji sieci web karta kliknij **żądania i błędów** . Karta **metryki** Pokaż wszystkie metryki, które można dodać.

Niektóre miar, które można monitorować dla aplikacji sieci web

-   Średnia pamięć zestaw roboczy
-   Średni czas reakcji
-   Czas Procesora
-   Zestaw roboczy pamięci
-   Żądania

![Monitorowanie wydajności aplikacji sieci web](./media/app-service-web-troubleshoot-performance-degradation/1-monitor-metrics.png)

Aby uzyskać więcej informacji zobacz:

-   [Monitorowanie aplikacji sieci Web w usłudze Azure aplikacji](web-sites-monitor.md)
-   [Odbieranie alertów](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)

#### <a name="monitor-web-endpoint-status"></a>Stan punktu końcowego monitora sieci web

Jeśli używasz aplikacji sieci web w **standardowe** ceny warstwy, aplikacje sieci Web umożliwia monitorowanie 2 punkty końcowe z 3 lokalizacji geograficznej.

Monitorowanie punkt końcowy konfiguruje testy sieci web z lokalizacji geo distributed, które testują czas reakcji i przestoje adresów URL sieci web. Test wykonuje operację HTTP GET, adres URL sieci web, aby określić czas reakcji i czas pracy w każdym miejscu. Każdy skonfigurowanej lokalizacji uruchamia test co pięć minut.

Czas pracy jest monitorowane przy użyciu kodów odpowiedzi HTTP, a czas reakcji jest mierzony w milisekundach. Test monitorowania zakończy się niepowodzeniem, jeśli kod odpowiedzi protokołu HTTP jest większe niż lub równe 400 lub odpowiedź trwa dłużej niż 30 sekund. Punkt końcowy jest traktowany jako dostępne, jeśli monitorowania badań działa z określonych lokalizacji.

Aby go skonfigurować, zobacz [jak: monitorowanie stan punktu końcowego web](web-sites-monitor.md#webendpointstatus).

Zobacz też [witryn sieci Web Azure zachowanie w górę oraz monitorowanie punktu końcowego — za pomocą Schackow Stefana](/documentation/videos/azure-web-sites-endpoint-monitoring-and-staying-up/) Film przedstawiający monitorowania punktu końcowego.

#### <a name="application-performance-monitoring-using-extensions"></a>Monitorowanie wydajności aplikacji przy użyciu rozszerzenia

Można również monitorować wydajność aplikacji, korzystając z _witryny rozszerzenia_.

Punkt końcowy extensible zarządzania, który pozwala korzystać z zaawansowanych zestaw narzędzi używany jako rozszerzenia witryny znajdują się w każdej aplikacji sieci web aplikacji usługi. Te narzędzia z zakresu od edytory kod źródła, takie jak [Visual Studio Team Services](https://www.visualstudio.com/products/what-is-visual-studio-online-vs.aspx) do narzędzia do zarządzania dla połączonego zasobów, takich jak bazy danych MySQL podłączony do aplikacji sieci web.

[Azure aplikacji wniosków](/services/application-insights/) i [Nowe Relic](/marketplace/partners/newrelic/newrelic/) są dwa rozszerzenia witryny, które są dostępne do monitorowania wydajności. Aby użyć nowego Relic, należy zainstalować agenta w czasie rzeczywistym. Aby użyć wniosków aplikacji Azure, odbudowanie kodu przy użyciu zestawu SDK, a możesz również zainstalować wewnętrzny, który zapewnia dostęp do dodatkowych danych. Zestaw SDK umożliwia pisanie kodu monitorowanie użycia i wydajności aplikacji bardziej szczegółowo.

Aby użyć aplikacji więcej informacji, zobacz [Monitorowanie wydajności w aplikacjach sieci web](../application-insights/app-insights-web-monitor-performance.md).

Aby użyć nowego Relic, zobacz [Nowy zarządzania wydajnością aplikacji Relic Azure](../store-new-relic-cloud-services-dotnet-application-performance-management.md).

<a name="collect" />
### <a name="2-collect-data"></a>2. zbieranie danych

####    <a name="enable-diagnostics-logging-for-your-web-app"></a>Włącz rejestrowanie diagnostyczne dla aplikacji sieci web

Środowisko aplikacji Web Apps zapewnia funkcje diagnostyczne dla informacji rejestrowania zarówno z serwerem sieci web i aplikacji sieci web. Logicznie są one podzielone na diagnostyki serwer sieci web i diagnostyce aplikacji.

##### <a name="web-server-diagnostics"></a>Narzędzia diagnostyczne serwera sieci Web

Możesz włączyć lub wyłączyć następujące rodzaje dzienników:

-   **Włączenie rejestrowania szczegółowych informacji o błędzie** — szczegółowe informacje o błędzie kodów stanu HTTP, wskazujące awarii (kod stanu 400 lub nowszej). To może zawierać informacje, które mogą być pomocne ustalić, dlaczego serwer zwrócił kod błędu.
-   **Niepowodzenie śledzenie żądania** — szczegółowe informacje o niepowodzeniu żądania, w tym śledzenia składników usług IIS używane do przetwarzania żądania oraz czas każdego składnika. Może to być przydatne, jeśli próbujesz zwiększenie wydajności aplikacji sieci web lub określić, co powoduje konkretnego problemu HTTP.
-   **Rejestrowanie serwera sieci web** - informacji na temat transakcji HTTP za pomocą formatu pliku dziennika rozszerzonego W3C. Jest to przydatne podczas ustalania ogólnego Metryka aplikacji sieci web, takie jak liczba żądań obsłużonych lub liczbę żądań pochodzą z określonego adresu IP.

##### <a name="application-diagnostics"></a>Narzędzia diagnostyczne aplikacji

Narzędzia diagnostyczne aplikacji pozwala na przechwytywanie informacji generowanych przez aplikację sieci web. Korzystając z aplikacji ASP.NET `System.Diagnostics.Trace` klasy rejestrowanie informacji w dzienniku diagnostyki aplikacji.

Aby uzyskać szczegółowe instrukcje dotyczące konfigurowania aplikacji do logowania zobacz [Włączanie diagnostyki rejestrowanie dla aplikacji sieci web w usłudze Azure aplikacji](web-sites-enable-diagnostic-log.md).

#### <a name="use-remote-profiling"></a>Za pomocą zdalnego profilowania

W usłudze Azure aplikacji Web Apps, aplikacje interfejsu API i WebJobs można zdalnie sprofilować. Jeśli procesu działa wolniej niż oczekiwano, lub oczekiwania żądania HTTP jest większa niż normalny i użycie Procesora przez proces również jest wysoki, możesz zdalnie profilu procesie i uzyskać stosy wywołań przy próbkowaniu Procesora do analizowania działania procesu i ścieżki ciepłej kodu.

Aby uzyskać więcej informacji na zobacz [zdalny profilowanie obsługi w Azure aplikacji usługi](/blog/remote-profiling-support-in-azure-app-service).


#### <a name="use-the-azure-app-service-support-portal"></a>Używanie Portal Azure aplikacji usługi pomocy technicznej

Aplikacje sieci Web umożliwia rozwiązywanie problemów związanych z aplikacji sieci web, sprawdzając HTTP dzienniki, dzienniki zdarzeń, zrzuty proces i nie tylko. Można uzyskać dostęp do tych informacji za pomocą portalu pomocy technicznej w **http://&lt;nazwy aplikacji >.scm.azurewebsites.net/Support**

Portal Azure aplikacji usługa obsługuje zawiera trzy karty osobnym do obsługi trzech kroków typowy scenariusz rozwiązywania problemów:

1.  Obserwowanie bieżące zachowanie
2.  Analizowanie przez zbieranie informacji diagnostycznych i uruchamianie wbudowane narzędzia analizy serwera
3.  Ograniczanie

Jeśli problem występuje jest teraz, kliknij pozycję **Analizowanie** > **diagnostyki** > **Diagnozowanie teraz** utworzenia sesji diagnostycznych, które będzie zbierać HTTP dzienniki, podglądu dzienniki zdarzeń, pamięci zrzuty, PHP dzienników błędów i PHP przetworzyć raportu.

Gdy dane są zbierane, będzie również uruchomić analizę danych i udostępnić raport HTML.

W przypadku, gdy chcesz pobrać dane, które domyślnie, powinny być przechowywane w folderze D:\home\data\DaaS.

Aby uzyskać więcej informacji na portal Azure Obsługa usługi aplikacji zobacz [Nowe aktualizacje rozszerzenia do obsługi witryn dla witryny sieci Web Azure](/blog/new-updates-to-support-site-extension-for-azure-websites).

#### <a name="use-the-kudu-debug-console"></a>Konsola debugowania Kudu

Aplikacje sieci Web jest zawarty w konsoli debugowania używanego do debugowania, poznawanie, przekazywanie plików, a także uzyskiwanie informacji o środowisku punktów końcowych JSON. Jest to _Kudu konsoli_ lub _Menedżer sterowania usługami pulpitu nawigacyjnego_ dla aplikacji sieci web.

Dostęp do tego pulpitu nawigacyjnego, przechodząc do łącza **https://&lt;nazwy aplikacji >.scm.azurewebsites.net/**.

Czynności, które zawiera Kudu należą:

-   ustawienia środowiska aplikacji
-   strumień dziennika
-   Zrzut diagnostyczne
-   debugowanie konsoli, w którym można uruchomić polecenia cmdlet programu Powershell i podstawowych poleceń systemu DOS.


Inny przydatną funkcją Kudu jest, w przypadku, gdy aplikacja jest generowania szansy pierwszego wyjątków, możesz użyć Kudu oraz dokonuje zrzutu narzędzie SysInternals Procdump, aby utworzyć pamięci. Te zrzuty pamięci są migawek procesu i często ułatwiają rozwiązywanie problemów bardziej złożone z aplikacji sieci web.

Aby uzyskać więcej informacji na temat funkcji dostępnych w Kudu zobacz [Narzędzia Azure witryny sieci Web zespołu usług, które należy wiedzieć o](/blog/windows-azure-websites-online-tools-you-should-know-about/).

<a name="mitigate" />
### <a name="3-mitigate-the-issue"></a>3. ten problem zmniejszeniu

####    <a name="scale-the-web-app"></a>Skala aplikacji sieci web

W Azure aplikacji usługi Aby zwiększyć wydajność i przepustowość, można dostosować skali, na którym użytkownicy korzystający z aplikacji. Skalowanie wewnętrzne aplikacji sieci web wymaga dwóch powiązanych akcji: wprowadzanie planu usług aplikacji wyższego poziomu cen i konfigurowanie określonych ustawień po przełączeniu do wyższego poziomu cennik.

Aby uzyskać więcej informacji o skalowania zobacz [skalowanie aplikacji sieci web w usłudze Azure aplikacji](web-sites-scale.md).

Ponadto można uruchamiać aplikacji na więcej niż jedno wystąpienie. To nie tylko oferuje więcej możliwości przetwarzania, ale umożliwia także niektóre ilość odporność na uszkodzenia. Jeśli proces awarii na jedno wystąpienie, inne wystąpienie będzie nadal obsługiwać żądania.

Możesz ustawić skalowania ręcznie lub automatycznie.

####    <a name="use-autoheal"></a>Za pomocą AutoHeal

AutoHeal odtwarza proces roboczy dla aplikacji na podstawie ustawień wybrane (na przykład zmian w konfiguracji, żądania, na podstawie wielkości pamięci limity lub czas potrzebny do wykonania żądania). W większości przypadków, Kosz procesu jest najszybszym sposobem na odzyskiwanie z problem. Chociaż można zawsze ponownie uruchomić aplikację sieci web z bezpośrednio z poziomu Azure Portal, AutoHeal będzie robi to automatycznie. To wszystko, co należy zrobić, dodać kilka wyzwalaczy w web.config głównego dla aplikacji sieci web. Należy zauważyć, że te ustawienia będzie działać w taki sam sposób, nawet jeśli aplikacji nie jest jedną .net.

Aby uzyskać więcej informacji zobacz [korygujący automatyczne Azure witryn sieci Web](/blog/auto-healing-windows-azure-web-sites/).

####    <a name="restart-the-web-app"></a>Ponownie uruchom aplikację sieci web

Często jest najprostszym sposobem odzyskiwanie z jednorazowego problemów. W [Azure Portal](https://portal.azure.com/)na karta aplikacji sieci web, masz odpowiednie opcje, aby zatrzymać lub ponownie uruchom aplikację.

 ![ponownie uruchom aplikację sieci web, aby rozwiązać problemy z wydajnością](./media/app-service-web-troubleshoot-performance-degradation/2-restart.png)

Można również zarządzać aplikacji sieci web przy użyciu programu Powershell Azure. Aby uzyskać więcej informacji zobacz [Używanie Azure przy użyciu Menedżera zasobów Azure](../powershell-azure-resource-manager.md).
