<properties
    pageTitle="Naprawianie 502 — Nieprawidłowa brama, 503 Usługa niedostępna błędy | Microsoft Azure"
    description="Rozwiązywanie problemów z 502 — Nieprawidłowa brama i 503 Usługa niedostępna błędów w aplikacji sieci web hostowanej w usłudze Azure w aplikacji."
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""
    tags="top-support-issue"
    keywords="502 — Nieprawidłowa brama 503 Usługa niedostępna, błąd 503, błąd 502"/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/06/2016"
    ms.author="cephalin"/>

# <a name="troubleshoot-http-errors-of-502-bad-gateway-and-503-service-unavailable-in-your-azure-web-apps"></a>Rozwiązywanie problemów z błędami HTTP "502 — Nieprawidłowa brama" i "503 — Usługa niedostępna" w aplikacji sieci web Azure

"502 — Nieprawidłowa brama" i "503 — Usługa niedostępna" są typowych błędów w aplikacji sieci web hostowanej usłudze [Azure aplikacji](http://go.microsoft.com/fwlink/?LinkId=529714). Ten artykuł ułatwia rozwiązywania tych problemów.

Jeśli potrzebujesz dodatkowej pomocy w dowolnym momencie, w tym artykule, można skontaktować się Azure ekspertów na [MSDN Azure i fora przepełnienie stosu](https://azure.microsoft.com/support/forums/). Ponadto można również pliku Azure pomocy technicznej zdarzenia. Przejdź do [witryny pomocy technicznej Azure](https://azure.microsoft.com/support/options/) i kliknij pozycję **Uzyskiwanie pomocy technicznej**.

## <a name="symptom"></a>Symptom

Podczas przechodzenia do aplikacji sieci web, funkcja zwraca HTTP Błąd "502 — Nieprawidłowa brama" lub HTTP o błędzie "503 Usługa niedostępna".

## <a name="cause"></a>Przyczyna

Przyczyną tego problemu jest często problemy z poziomu aplikacji, takich jak:

-   żądania zbyt długo
-   aplikacji przy użyciu wysoka pamięci i Procesor
-   Aplikacja awarii z powodu wyjątku.

## <a name="troubleshooting-steps-to-solve-502-bad-gateway-and-503-service-unavailable-errors"></a>Rozwiązywanie problemów z czynności w celu rozwiązania błędów "503 — Usługa niedostępna" i "502 — Nieprawidłowa brama"

Rozwiązywanie problemów z można podzielić na trzy różne zadania w kolejności:

1.  [Obserwowanie i monitorować zachowanie aplikacji](#observe)
2.  [Zbieranie danych](#collect)
3.  [Ograniczanie problem](#mitigate)

[Aplikacji sieci Web usługi](/services/app-service/web/) zapewnia różne opcje na poszczególnych etapach.

<a name="observe" />
### <a name="1-observe-and-monitor-application-behavior"></a>1. obserwować i monitorować zachowanie aplikacji

####    <a name="track-service-health"></a>Śledzenie kondycja usługi

Microsoft Azure publicizes każdym razem, gdy jest przerwy lub wydajności obniżenie wydajności usługi. Możesz śledzić kondycji usługi na [Azure Portal](https://portal.azure.com/). Aby uzyskać więcej informacji zobacz [śledzenie kondycji usługi](../monitoring-and-diagnostics/insights-service-health.md).

####    <a name="monitor-your-web-app"></a>Monitorowanie aplikacji sieci web

Ta opcja umożliwia Dowiedz się, jeśli masz problemy aplikacji. W aplikacji sieci web karta kliknij **żądania i błędów** . Karta **metryki** Pokaż wszystkie metryki, które można dodać.

Niektóre miar, które można monitorować dla aplikacji sieci web

-   Średnia pamięć zestaw roboczy
-   Średni czas reakcji
-   Czas Procesora
-   Zestaw roboczy pamięci
-   Żądania

![monitorowanie aplikacji sieci web do rozwiązania błędów HTTP 502 — Nieprawidłowa brama i 503 — Usługa jest niedostępna](./media/app-service-web-troubleshoot-HTTP-502-503/1-monitor-metrics.png)

Aby uzyskać więcej informacji zobacz:

-   [Monitorowanie aplikacji sieci Web w usłudze Azure aplikacji](web-sites-monitor.md)
-   [Odbieranie alertów](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)

<a name="collect" />
### <a name="2-collect-data"></a>2. zbieranie danych

####    <a name="use-the-azure-app-service-support-portal"></a>Używanie Portal Azure aplikacji usługi pomocy technicznej

Aplikacje sieci Web umożliwia rozwiązywanie problemów związanych z aplikacji sieci web, sprawdzając HTTP dzienniki, dzienniki zdarzeń, zrzuty proces i nie tylko. Można uzyskać dostęp do tych informacji za pomocą portalu pomocy technicznej w **http://&lt;nazwy aplikacji >.scm.azurewebsites.net/Support**

Portal Azure aplikacji usługa obsługuje zawiera trzy karty osobnym do obsługi trzech kroków typowy scenariusz rozwiązywania problemów:

1.  Obserwowanie bieżące zachowanie
2.  Analizowanie przez zbieranie informacji diagnostycznych i uruchamianie wbudowane narzędzia analizy serwera
3.  Ograniczanie

Jeśli problem występuje jest teraz, kliknij pozycję **Analizowanie** > **diagnostyki** > **Diagnozowanie teraz** utworzenia sesji diagnostycznych, które będzie zbierać HTTP dzienniki, podglądu dzienniki zdarzeń, pamięci zrzuty, PHP dzienników błędów i PHP przetworzyć raportu.

Gdy dane są zbierane, będzie również uruchomić analizę danych i udostępnić raport HTML.

W przypadku, gdy chcesz pobrać dane, które domyślnie, powinny być przechowywane w folderze D:\home\data\DaaS.

Aby uzyskać więcej informacji na portal Azure Obsługa usługi aplikacji zobacz [Nowe aktualizacje rozszerzenia do obsługi witryn dla witryny sieci Web Azure](/blog/new-updates-to-support-site-extension-for-azure-websites).

####    <a name="use-the-kudu-debug-console"></a>Konsola debugowania Kudu

Aplikacje sieci Web jest zawarty w konsoli debugowania używanego do debugowania, poznawanie, przekazywanie plików, a także uzyskiwanie informacji o środowisku punktów końcowych JSON. Jest to _Kudu konsoli_ lub _Menedżer sterowania usługami pulpitu nawigacyjnego_ dla aplikacji sieci web.

Dostęp do tego pulpitu nawigacyjnego, przechodząc do łącza **https://&lt;nazwy aplikacji >.scm.azurewebsites.net/**.

Czynności, które zawiera Kudu należą:

-   ustawienia środowiska aplikacji
-   strumień dziennika
-   Zrzut diagnostyczne
-   debugowanie konsoli, w którym można uruchomić polecenia cmdlet programu Powershell i podstawowych poleceń systemu DOS.


Inny przydatną funkcją Kudu jest, w przypadku, gdy aplikacja jest generowania szansy pierwszego wyjątków, możesz użyć Kudu oraz dokonuje zrzutu narzędzie SysInternals Procdump, aby utworzyć pamięci. Te zrzuty pamięci są migawek procesu i często ułatwiają rozwiązywanie problemów bardziej złożone z aplikacji sieci web.

Aby uzyskać więcej informacji na temat funkcji dostępnych w Kudu zobacz [narzędzia online Azure witryn sieci Web, które należy wiedzieć o](/blog/windows-azure-websites-online-tools-you-should-know-about/).

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

 ![Uruchom ponownie aplikację, aby naprawić błędy HTTP 502 — Nieprawidłowa brama i 503 — Usługa jest niedostępna](./media/app-service-web-troubleshoot-HTTP-502-503/2-restart.png)

Można również zarządzać aplikacji sieci web przy użyciu programu Powershell Azure. Aby uzyskać więcej informacji zobacz [Używanie Azure przy użyciu Menedżera zasobów Azure](../powershell-azure-resource-manager.md).
