<properties
    pageTitle="Monitorowanie, diagnozowanie i rozwiązywanie problemów z miejsca do magazynowania | Microsoft Azure"
    description="Używanie funkcji, takich jak analizy miejsca do magazynowania, rejestrowanie po stronie klienta i inne narzędzia innych firm, aby zidentyfikować, diagnozowanie i rozwiązywanie problemów związanych z magazyn Azure."
    services="storage"
    documentationCenter=""
    authors="jasonnewyork"
    manager="tadb"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/22/2016"
    ms.author="jahogg"/>

# <a name="monitor-diagnose-and-troubleshoot-microsoft-azure-storage"></a>Monitorowanie, diagnozowanie i rozwiązywanie problemów z magazynem tabel platformy Microsoft Azure

[AZURE.INCLUDE [storage-selector-portal-monitoring-diagnosing-troubleshooting](../../includes/storage-selector-portal-monitoring-diagnosing-troubleshooting.md)]

## <a name="overview"></a>Omówienie

Diagnozowanie i rozwiązywanie problemów z w aplikacji rozproszonej obsługiwany w środowisku chmury może być bardziej złożony niż w środowiskach tradycyjny. Aplikacje mogą być rozmieszczone w infrastrukturze PaaS lub IaaS w środowisku lokalnym, na urządzeniu przenośnym lub w kombinacji tych. Zazwyczaj ruch sieciowy aplikacji może być przechodzenie przez sieci publiczne i prywatne i aplikacja może używać wielu technologii miejsca do magazynowania, takich jak Microsoft Azure magazyn tabel, obiektów blob i kolejek lub pliki oprócz inne dane są przechowywane, takie jak relacyjnych baz danych dokumentów i.

Zarządzanie takie aplikacje pomyślnie możesz monitorować je z wyprzedzeniem i zrozumienie sposobu diagnozowanie i rozwiązywanie problemów z wszystkimi aspektami je i ich technologie zależne. Jako użytkownik usługi Magazyn Azure należy stale monitorowanie usług miejsca do magazynowania używanych przez aplikację dla dowolnego nieoczekiwane zachowanie (na przykład mniejsza niż czas reakcji normalny) i rejestrowanie za pomocą zbieranie bardziej szczegółowych danych a problemu szczegóły dotyczące. Informacje diagnostyczne, którą można uzyskać z monitorowania i rejestrowania pomaga określić przyczynę aplikacji napotkał problem. Następnie można rozwiązać problem i określić odpowiednie czynności, które można wykonać, aby go korygowania. Azure magazyn jest podstawowa usługa Azure i stanowi część ważne większości rozwiązania, które klienci wdrażać Azure infrastruktury. Azure magazynowania zawiera funkcje, które ułatwiają monitorowanie, diagnozowanie i rozwiązywanie problemów z magazynu w chmurze aplikacji.

> [AZURE.NOTE] Metryki lub funkcja rejestrowania włączone w tej chwili nie jest miejsca do magazynowania konta typu replikacji z zbędne strefy przestrzeni dyskowej (ZRS). Ponadto usługa Azure pliki nie obsługuje rejestrowania w tej chwili.

Praktycznych przewodnikiem do końca do końca dotyczących rozwiązywania problemów w aplikacjach magazyn Azure zobacz [Rozwiązywanie problemów przy użyciu metryki magazynowania Azure i rejestrowanie, AzCopy i Analizator wiadomości zakończenia do zakończenia](storage-e2e-troubleshooting.md).

+ [Wprowadzenie]
    + [Przegląd organizacji elementów tego przewodnika]
+ [Monitorowanie usługi miejsca do magazynowania]
    + [Monitorowanie kondycji usługi]
    + [Zdolności monitorowania]
    + [Dostępność monitorowania]
    + [Monitorowanie wydajności]
+ [Diagnozowanie problemów związanych z miejsca do magazynowania]
    + [Problemy dotyczące kondycji usługi]
    + [Problemy z wydajnością]
    + [Diagnozowanie problemów]
    + [Problemy emulatora miejsca do magazynowania]
    + [Narzędzia rejestrowania miejsca do magazynowania]
    + [Za pomocą narzędzia rejestrowania sieci]
+ [Śledzenie end-to-end]
    + [Korelacji danych dziennika]
    + [Identyfikator klienta]
    + [Identyfikator żądania serwera]
    + [Sygnatury czasowe]
+ [Wskazówki dotyczące rozwiązywania problemów]
    + [Metryki Pokaż AverageE2ELatency wysokiej i niskiej AverageServerLatency]
    + [Metryki Pokaż niskim AverageE2ELatency i niskiej AverageServerLatency, ale klient występuje duże opóźnienia]
    + [Metryki Pokaż wysoka AverageServerLatency]
    + [Występują nieoczekiwane opóźnień dostarczenia wiadomości w kolejce]
    + [Metryki pokazywanie wzrost w PercentThrottlingError]
    + [Metryki pokazywanie wzrost w PercentTimeoutError]
    + [Metryki pokazywanie wzrost w PercentNetworkError]
    + [Klient otrzymuje wiadomości HTTP 403 (Dostęp zabroniony)]
    + [Klient otrzymuje wiadomości HTTP 404 (nie można odnaleźć)]
    + [Klient otrzymuje wiadomości HTTP 409 (konflikt)]
    + [Metryki pokazywanie niskim PercentSuccess lub wpisy dziennika analizy mają operacje ze stanem transakcji ClientOtherErrors]
    + [Metryki wydajność pokazywanie wzrost nieoczekiwane w wykorzystania możliwości magazynu]
    + [Występują nieoczekiwane ponownego uruchamiania maszyn wirtualnych, mające dużej liczby dołączony wirtualnych dysków twardych]
    + [Problem wynika z przy użyciu emulatora miejsca do magazynowania dla projektowego lub testowego]
    + [Wystąpią problemy z instalacją Azure SDK dla środowiska .NET]
    + [Masz inny problem z usługą magazynu]
    + [Rozwiązywanie problemów z konfiguracją Azure plików z systemem Windows i Linux](storage-troubleshoot-file-connection-problems.md)
+ [Dodatki]
    + [Dodatek 1: Przy użyciu Fiddler ruch HTTP i HTTPS]
    + [Dodatek 2: Za pomocą Wireshark Przechwyć ruch sieciowy]
    + [Dodatek 3: Przy użyciu analizatora wiadomości Microsoft Przechwyć ruch sieciowy]
    + [Dodatek 4: Za pomocą programu Excel, aby wyświetlić metryki i rejestrowanie danych]
    + [Dodatek 5: Monitorowanie za pomocą wniosków aplikacji dla programu Visual Studio Team Services]

## <a name="introduction"></a>Wprowadzenie

Ten przewodnik pokazano, jak za pomocą funkcji, takich jak Azure analizy miejsca do magazynowania, klienta rejestrowanie w bibliotece Azure miejsca do magazynowania klienta i inne narzędzia innych firm zidentyfikować, diagnozowanie i rozwiązywanie problemów z magazyn Azure problemy związane z.

![][1]

*Rysunek 1 monitorowania diagnostyki i rozwiązywania problemów*

Ten przewodnik jest przeznaczony do odczytu przede wszystkim przez programistów usług online, korzystających z usługi Azure miejsca do magazynowania i informatyków osoby odpowiedzialne za zarządzanie tych usług online. Cele w tym przewodniku są:

- Aby zachować zdrowia i wydajności konta magazynu platformy Azure.
- Aby udostępnić potrzeby procesów i narzędzi pomagające zdecydować, jeśli problemu w aplikacji odnosi się do miejsca do magazynowania Azure.
- Aby udostępnić sankcji wskazówki dotyczące rozwiązywania problemów związanych z magazynem Azure.

### <a name="how-this-guide-is-organized"></a>Przegląd organizacji elementów tego przewodnika

Sekcja "[monitorowania usługi Magazyn]" opisano, jak monitorowanie kondycji i wydajności usługi Magazyn Azure za pomocą metryki analizy Azure magazynowania (metryki magazynowania).

Sekcja "[Diagnozowanie problemów magazynowania]" opisano sposób diagnozowanie problemów za pomocą Azure magazynowania analizy rejestrowania (rejestrowanie magazynowania). Zawiera również opis jak włączyć rejestrowanie po stronie klienta przy użyciu funkcji w jednym z bibliotek klienta takiej jak biblioteka klienta miejsca do magazynowania dla .NET lub SDK Azure dla języka Java.

W sekcji "[śledzenia End-end]" opisano, jak można dostosować informacje zawarte w różnych plików dziennika i metryk danych.

Sekcja "[wskazówki dotyczące rozwiązywania problemów]" zawiera wskazówki dotyczące rozwiązywania problemów dla niektórych typowych problemów związanych z miejsca do magazynowania, które można napotkać.

"[Dodatki]" zawierają informacje o korzystaniu z innych narzędzi, takich jak Netmon i Wireshark do analizy danych pakietu sieci, Fiddler do analizowania wiadomości protokołu HTTP/HTTPS i analizowania wiadomości firmy Microsoft dla korelacji danych dziennika.


## <a name="monitoring-your-storage-service"></a>Monitorowanie usługi miejsca do magazynowania

Użytkownicy zaznajomieni z monitorowania wydajności systemu Windows, możesz myśleć o metryki magazynowania jako równowartość magazyn Azure liczników monitora wydajności systemu Windows. W metryki magazynowania znajdziesz pełnego zestawu metryki (liczniki terminologia Monitor wydajności systemu Windows), takich jak usługa dostępności, liczba żądań do usługi lub procent pomyślnie wykonanych żądań do usługi. Aby uzyskać pełną listę dostępnych metryki zobacz [Schematu tabeli metryki analizy miejsca do magazynowania](http://msdn.microsoft.com/library/azure/hh343264.aspx). Można określić, czy chcesz, aby Usługa magazynu do gromadzenia i agregowanie metryki co godzinę lub co minutę. Aby uzyskać więcej informacji na temat włączania metryki i Monitoruj swoje konta przestrzeni dyskowej zobacz [Włączanie metryki magazynowania i przeglądania danych metryki](http://go.microsoft.com/fwlink/?LinkId=510865).

Możesz wybrać, które godzinowe metryki chcesz wyświetlić w [Azure Portal](https://portal.azure.com) i skonfigurować reguły, które powiadamiania administratorów przez wiadomości e-mail przy każdym metrykę godzinowe przekracza określony próg. Aby uzyskać więcej informacji zobacz [Otrzymywać powiadomienia alertów](../monitoring-and-diagnostics/insights-receive-alert-notifications.md). 

Usługa magazynu zbiera metryki przy użyciu starań, ale nie można rejestrować każdej operacji miejsca do magazynowania.

Portal Azure możesz wyświetlić miar, takich jak dostępność, całkowita liczba żądań i numery Średni czas oczekiwania konta przestrzeni dyskowej. Reguła powiadomienia zawiera również skonfigurować alert administratora, jeśli dostępność pomija poniżej pewnego poziomu. Wyświetlanie tych danych, jednego obszaru możliwe do badania jest to procent sukcesu usługi tabeli jest poniżej 100% (Aby uzyskać więcej informacji, zobacz sekcję "[metryki pokazywanie niskim PercentSuccess lub wpisy dziennika analizy mieć operacji z stan ClientOtherErrors]").

Należy stale monitorować Azure aplikacjom upewnij się, że są one prawidłowy i wydajności, zgodnie z oczekiwaniami przez:

- Ustanawianie niektórych metryki według planu bazowego dla aplikacji, która umożliwi porównać bieżące dane i zidentyfikować wszelkich istotnych zmian w zachowanie Azure miejsca do magazynowania i aplikacji. W większości przypadków wartości z metryki według planu bazowego będzie określone dla aplikacji i należy je ustanowić po zakończeniu testowania aplikacji.
- Nagrywanie minuta metryki i używanie ich monitorowanie aktywnie nieoczekiwanych błędów i różnic w odniesieniu takich jak tych najwyższych wartościach błąd zlicza lub żądań.
- Nagrywanie godzinowe metryki i używanie ich monitorowanie średnie wartości, takie jak obliczanie średniej wartości błędu liczników i żądań.
- Badanie potencjalnych problemów za pomocą narzędzia Diagnostyka, zgodnie z opisem w dalszej części w sekcji "[Diagnozowanie problemów miejsca do magazynowania]".

Wykresy na rysunku 3 poniżej przedstawiono, jak średnia, występujący w przypadku metryk godzinowego można ukryć tych najwyższych wartościach w działaniu. Aby pokazywać stałej stopy żądania, podczas minuta metryki wyświetlić zmianami, których naprawdę odbywają się są wyświetlane godzinowe metryki.

![][3]

Pozostała część w tej sekcji opisano metryki, jakie należy monitorować i dlaczego.

### <a name="monitoring-service-health"></a>Monitorowanie kondycji usługi

[Azure Portal](https://portal.azure.com) umożliwia przeglądanie informacji o kondycji usługi miejsca do magazynowania (i innych usług Azure) we wszystkich regionach Azure na świecie. Umożliwia widoczne od razu, jeśli problem poza kontrolę nad pogarsza Usługa magazynu w regionie, których używasz aplikacji.

[Azure Portal](https://portal.azure.com) oferuje również powiadomienia o zdarzenia, które mają wpływ na różnych usług Azure.
Uwaga: Te informacje było wcześniej dostępne, wraz z danych historycznych na [Pulpit nawigacyjny usługi Azure](http://status.azure.com).

Gdy [Azure Portal](https://portal.azure.com) zbiera informacje o kondycji z wewnątrz Azure centrach danych (w nowym oknie wewnątrz monitorowanie), możesz również przyjęcie metody poza do generowania transakcji syntetycznych okresowo uzyskać dostęp do aplikacji sieci web hostowanej Azure z wielu lokalizacji. Usług oferowanych przez [Dynatrace](http://www.dynatrace.com/en/synthetic-monitoring) i wniosków aplikacji dla programu Visual Studio Team Services są przykładem na zewnątrz. Aby uzyskać więcej informacji na temat wniosków aplikacji dla programu Visual Studio Team Services, zobacz dodatku "[dodatek 5: monitorowanie za pomocą wniosków aplikacji dla programu Visual Studio Team Services](#appendix-5)."

### <a name="monitoring-capacity"></a>Zdolności monitorowania

Metryki magazynowania są przechowywane metryki wydajność usługi obiektów blob ponieważ obiektów blob zwykle kont największą część danych (podczas pisania, nie jest możliwe za pomocą metryki magazynowania monitorować wydajność tabel i kolejek). Jeśli włączono monitorowanie usługi obiektów Blob, możesz znaleźć te dane w tabeli **$MetricsCapacityBlob** . Metryki magazynowania zapisuje te dane raz dziennie, a wartość **RowKey** służy do określenia, czy wiersz zawiera jednostkę, która odnosi się do użytkownika (wartości **danych**) lub analizy danych (wartość **analizy**). Każdy obiekt przechowywane zawiera informacje o ilość użytego magazynu (**zdolności** mierzony w bajtach) i bieżąca liczba kontenerów (**ContainerCount**) i obiektami BLOB (**ObjectCount**) używany na koncie przestrzeni dyskowej. Aby uzyskać więcej informacji na temat metryki wydajność przechowywane w tabeli **$MetricsCapacityBlob** zobacz [Schematu tabeli metryk analizy przestrzeni dyskowej](http://msdn.microsoft.com/library/azure/hh343264.aspx).

> [AZURE.NOTE] Należy monitorować te wartości dla wczesne ostrzeżenie, że Zbliżasz zdolności konta miejsca do magazynowania. W Azure Portal można dodać reguł alertów powiadomień o używana agregacji magazynowania przekracza lub przypada poniżej progi przez użytkownika.

Aby uzyskać pomoc szacowanie rozmiaru różnych obiektów miejsca do magazynowania, takich jak obiektów blob Zobacz blogu [Opis Azure miejsca do magazynowania rozliczenia — przepustowości, transakcje i wydajności](http://blogs.msdn.com/b/windowsazurestorage/archive/2010/07/09/understanding-windows-azure-storage-billing-bandwidth-transactions-and-capacity.aspx).

### <a name="monitoring-availability"></a>Dostępność monitorowania

Należy monitorować dostępność usług miejsca do magazynowania na koncie miejsca do magazynowania przez monitorowanie wartości w kolumnie **dostępność** w tabelach metryki godzinowe lub minuta — **$MetricsHourPrimaryTransactionsBlob**, **$MetricsHourPrimaryTransactionsTable**, **$MetricsHourPrimaryTransactionsQueue**, **$MetricsMinutePrimaryTransactionsBlob**, **$MetricsMinutePrimaryTransactionsTable**, **$MetricsMinutePrimaryTransactionsQueue**, **$MetricsCapacityBlob**. Kolumna **dostępność** zawiera wartość procentową, która wskazuje dostępność usługi lub operacja interfejsu API reprezentowanego przez wiersz ( **RowKey** wskazuje, czy wiersz zawiera metryki w usłudze jako całość lub dla konkretnego działania interfejsu API).

Dowolną wartość mniejszą niż 100% oznacza występują niektóre żądania miejsca do magazynowania. Można zobaczyć, dlaczego błędy sprawdzając innych kolumn danych metryki pokazać numery żądania z typami błędu, takich jak **ServerTimeoutError**. Możesz się spodziewać do wyświetlania **dostępności** przekraczają tymczasowo 100% powodów, takich jak limity czasu serwera przejściowych podczas usługę przenosi partycje do lepszego równoważenia obciążenia żądanie; Logika ponawiania w aplikacji klienckiej powinien obsługiwać warunkach przejściowymi. Artykuł [operacji rejestrowane analizy magazynu i komunikaty o stanie](http://msdn.microsoft.com/library/azure/hh343260.aspx) zawiera listę typów transakcji, które zawiera metryki magazynowania w obliczeniu **dostępności** .

W [Azure Portal](https://portal.azure.com)można dodać reguł alertów powiadomień o **dostępności** usługi przypada poniżej wartości progowej określonym przez użytkownika.

Sekcję "[wskazówki dotyczące rozwiązywania problemów]" w tym przewodniku opisano niektóre typowe problemy usługi miejsca do magazynowania związane z dostępności.

### <a name="monitoring-performance"></a>Monitorowanie wydajności

Monitorowanie wydajności usługi Magazyn, umożliwia następujące metryki z tabel metryki co godzinę i minuty.

- Wartości w kolumnach **AverageE2ELatency** i **AverageServerLatency** Pokaż Średni czas Usługa magazynu lub typu operacji interfejsu API trwa do przetwarzaj wezwania. **AverageE2ELatency** jest miarą opóźnienie zakończenia do końca, która zawiera czas potrzebny do żądania czytanie i wysyłanie odpowiedzi oprócz podczas przetwarzania żądania (dlatego w tym opóźnień sieci po żądanie osiągnie Usługa magazynu); **AverageServerLatency** jest miarą tylko podczas przetwarzania i dlatego nie obejmuje wszelkie opóźnień sieci związane z komunikowania się z klientem. Zobacz sekcję "[metryki Pokaż AverageE2ELatency wysokiej i niskiej AverageServerLatency]" w dalszej części tego przewodnika, aby uzyskać informacje dotyczące, dlatego może być znacząca różnica między tymi dwoma wartościami.
- Wartości w kolumnach **TotalIngress** i **TotalEgress** umożliwiają wyświetlanie łącznej ilości danych w bajtach przychodzących na i przechodząc wylogowywanie się z usługą magazynu lub za pośrednictwem określonego typu operacji interfejsu API.
- Wartości w kolumnie **TotalRequests** Pokaż całkowitą liczbę żądań, które Usługa magazynu działania interfejsu API otrzymuje. **TotalRequests** jest całkowita liczba żądań otrzymywanych Usługa magazynu.

Zazwyczaj możesz będzie monitorować nieoczekiwane w dowolnej z następujących wartości jako wskazówkę, że masz problem, który wymaga dochodzenia.

W [Azure Portal](https://portal.azure.com)można dodać reguł alertów w celu powiadamiania o ewentualne miar wydajności dla tej usługi Jesień poniżej lub przekracza progu określonym przez użytkownika.

Sekcja "[wskazówki dotyczące rozwiązywania problemów]" Ten przewodnik w tym artykule opisano niektóre typowe problemy usługi miejsca do magazynowania, związanych z wydajnością.


## <a name="diagnosing-storage-issues"></a>Diagnozowanie problemów związanych z miejsca do magazynowania

Istnieje kilka sposobów może się świadomość problemu lub problem w aplikacji, należą:

- Awaria głównych, która powoduje, że aplikacja awarię lub przestaną działać.
- Istotne zmiany z wartościami planu bazowego w metryki monitorowania zgodnie z opisem w poprzedniej sekcji "[monitorowania usługi przechowywania]".
- Raporty od użytkowników aplikacji niektórych określonego operacja nie została ukończona, zgodnie z oczekiwaniami lub nie działa niektórych funkcji.
- Błędy generowane w aplikacji pojawiających się w plikach dziennika lub za pośrednictwem innej metody powiadomienie.

Zazwyczaj problemy związane z usługi Magazyn Azure podzielić na cztery kategorie:

- Aplikacja ma problem z wydajnością, zgłaszane przez użytkowników albo wynika zmian w miar wydajności.
- Występuje problem z infrastrukturą magazyn Azure w jednej lub większej liczbie regionów.
- Aplikacja występują błąd, zgłaszane przez użytkowników albo wynika wzrost w jednym z metryki liczba błędów, które możesz monitorować.
- Podczas tworzenia i testowania możesz korzystać z magazynu lokalnego emulatora; mogą wystąpić niektóre problemy związane z zastosowania emulatora miejsca do magazynowania.

W poniższych sekcjach opisano kroki należy wykonać diagnozowanie i rozwiązywanie problemów w każdej z tych czterech kategoriach. Sekcja "[wskazówki dotyczące rozwiązywania problemów]" w dalszej części tego przewodnika zawiera więcej szczegółów dotyczących kilka typowych problemów, które mogą wystąpić.

### <a name="service-health-issues"></a>Problemy dotyczące kondycji usługi

Problemy z usługą kondycji są zwykle poza kontrolę. [Azure Portal](https://portal.azure.com) zawiera informacje dotyczące bieżących problemów z usługami Azure, łącznie z usługami miejsca do magazynowania. Jeśli możesz dwóch przypadkach do przechowywania zbędne Geo odczytu po utworzeniu konta przestrzeni dyskowej, następnie wypadku danych są dostępne w lokalizacji podstawowej aplikacji można przełączać się tymczasowo kopia tylko do odczytu w lokalizacji pomocniczej. Aby to zrobić, aplikacja musi przełączać między korzystaniem z lokalizacji magazynu głównego i pomocniczego i pracować w trybie zmniejszonej funkcjonalności danych tylko do odczytu. Bibliotek klienta miejsca do magazynowania Azure umożliwia opracowanie zasad ponów próbę, który może zostać odczytany z magazynu pomocniczego w przypadku odczytu z magazynu podstawowego kończy się niepowodzeniem. Aplikacja należy również zauważyć, że dane w lokalizacji pomocniczej jest ostatecznie spójne. Aby uzyskać więcej informacji zobacz w blogu [Opcje nadmiarowości przechowywania Azure i przechowywania zbędne Geo dostęp do odczytu](https://blogs.msdn.microsoft.com/windowsazurestorage/2013/12/11/windows-azure-storage-redundancy-options-and-read-access-geo-redundant-storage/).

### <a name="performance-issues"></a>Problemy z wydajnością

Wydajność aplikacji może być subiektywne, zwłaszcza z perspektywy użytkownika. W związku z tym, należy mieć dostępne ułatwiające określanie metryki według planu bazowego w przypadku, gdy być może wystąpił problem z wydajnością. Wiele czynników mogą mieć wpływ na wydajność usługi Azure miejsca do magazynowania z punktu widzenia aplikacji klienta. Następujących czynników może działać w tej usłudze miejsca do magazynowania, klienta lub infrastruktury sieciowej; w związku z tym należy przyjąć strategię identyfikacji pochodzenia problemu z wydajnością.

Po zidentyfikowaniu lokalizacji prawdopodobne przyczyny problemu z wydajnością z metryki następnie służy plików dziennika Aby znaleźć szczegółowe informacje, które diagnozowanie i rozwiązywanie problemu dalej.

Mogą wystąpić sekcji, którą "[wskazówki dotyczące rozwiązywania problemów]" w dalszej części tego przewodnika zawiera więcej informacji na temat niektóre typowe wydajność powiązaną problemów.

### <a name="diagnosing-errors"></a>Diagnozowanie problemów

Użytkownicy aplikacji może powiadomić o błędach zgłaszane przez aplikację klienta. Metryki magazynowania rejestruje także liczby typów błędu z usługi Magazyn, takich jak **NetworkError**, **ClientTimeoutError**lub **AuthorizationError**. Podczas metryki magazynowania rekordy tylko liczby typów błędu, można uzyskać więcej informacji na temat poszczególnych żądania sprawdzając dzienników sieci, po stronie klienta i po stronie serwera. Zazwyczaj zwrócony przez usługę magazynowania kod stanu HTTP będzie przedstawiać niepowodzenia wezwanie.

> [AZURE.NOTE] Pamiętaj, że możesz należy się spodziewać przejściowymi błędy: na przykład błędów z powodu warunków sieci lub błędy aplikacji.

Przydatne w przypadku opis magazynowaniem stanu i błędów kodów są następujące zasoby:

- [Typowe kody błędów interfejs API REST](http://msdn.microsoft.com/library/azure/dd179357.aspx)
- [Kody błędów usługi obiektów blob](http://msdn.microsoft.com/library/azure/dd179439.aspx)
- [Kody błędów usługi kolejki](http://msdn.microsoft.com/library/azure/dd179446.aspx)
- [Kody błędów usługi tabeli](http://msdn.microsoft.com/library/azure/dd179438.aspx)
- [Kody błędów usługi pliku](https://msdn.microsoft.com/library/azure/dn690119.aspx)

### <a name="storage-emulator-issues"></a>Problemy emulatora miejsca do magazynowania

Zestaw SDK Azure zawiera emulatora miejsca do magazynowania, można uruchamiać na roboczej rozwoju. Ten emulatora symuluje większość zachowanie usług Azure magazynu i jest przydatne podczas tworzenia i testowania, co pozwala na uruchamianie aplikacji, które korzystają z usług Azure miejsca do magazynowania bez potrzeby korzystania z subskrypcji usługi Azure i konto Azure miejsca do magazynowania.

Sekcja "[wskazówki dotyczące rozwiązywania problemów]" Ten przewodnik w tym artykule opisano niektóre typowe problemy napotykane, za pomocą emulatora miejsca do magazynowania.

### <a name="storage-logging-tools"></a>Narzędzia rejestrowania miejsca do magazynowania

Rejestrowanie miejsca do magazynowania zapewnia rejestrowanie po stronie serwera żądań miejsca do magazynowania na koncie Azure miejsca do magazynowania. Aby uzyskać więcej informacji na temat włączyć rejestrowanie po stronie serwera i uzyskać dostęp do danych dziennika zobacz [Włączanie rejestrowania miejsca do magazynowania i uzyskiwanie dostępu do danych dziennika](http://go.microsoft.com/fwlink/?LinkId=510867).

Biblioteka klienta miejsca do magazynowania dla środowiska .NET umożliwia zbieranie danych dziennika po stronie klienta, odpowiadającą przestrzeni dyskowej operacje wykonywane przez aplikację. Aby uzyskać więcej informacji zobacz [rejestrowanie po stronie klienta z biblioteką klienta .NET miejsca do magazynowania](http://go.microsoft.com/fwlink/?LinkId=510868).

> [AZURE.NOTE] W niektórych przypadkach (na przykład błędy autoryzacji skojarzenia zabezpieczeń) użytkownik może komunikat o błędzie, dla którego bez danych żądania można znaleźć w dziennikach po stronie serwera miejsca do magazynowania. Możesz skorzystać z funkcji rejestrowania Biblioteka klienta miejsca do magazynowania, którzy chcą Jeśli przyczyny tego problemu jest na komputerze klienckim lub badanie z siecią za pomocą narzędzia do monitorowania sieci.

### <a name="using-network-logging-tools"></a>Za pomocą narzędzia rejestrowania sieci

Możesz przechwycić komunikacji między klientem i serwerem zawierają szczegółowe informacje o dane, które są wymiany klienta i serwera i podstawowe parametry sieci. Narzędzia rejestrowania przydatne sieci obejmują:

- [Fiddler](http://www.telerik.com/fiddler) jest bezpłatne internetowy debugowania serwera proxy, która umożliwia sprawdzenie nagłówki i danych ładunku HTTP i HTTPS wiadomości i odpowiadania na wezwania. Aby uzyskać więcej informacji, zobacz [dodatku 1: przy użyciu Fiddler ruch HTTP i HTTPS](#appendix-1).
- [Monitor sieci firmy Microsoft (monitora sieci)](http://www.microsoft.com/download/details.aspx?id=4865) i [Wireshark](http://www.wireshark.org/) są bezpłatne sieci protokół programy do analizowania umożliwiających wyświetlanie pakietów szczegółowe informacje dotyczące szeroką gamę protokołów sieciowych. Aby uzyskać więcej informacji o programie Wireshark, zobacz "[dodatku 2: za pomocą Wireshark Przechwyć ruch sieciowy](#appendix-2)".
- Microsoft wiadomości Analyzer jest narzędziem firmy Microsoft, który zastępuje Netmon i że oprócz rejestrowania danych pakietów sieciowych, umożliwia wyświetlanie i analizowanie danych dziennika pobranych z innych narzędzi. Aby uzyskać więcej informacji, zobacz "[dodatku 3: za pomocą analizatora wiadomości Microsoft Przechwyć ruch sieciowy](#appendix-3)".
- Jeśli chcesz wykonać test łączności podstawowe, aby sprawdzić, czy komputerze klienta można połączyć się z usługą Azure przestrzeni dyskowej w sieci, nie możesz tego zrobić przy użyciu narzędzia do standardowego **ping** na komputerze klienckim. Jednak [Narzędzie **tcping** ](http://www.elifulkerson.com/projects/tcping.php) umożliwia sprawdzenie połączenia.

W większości przypadków danych dziennika od miejsca do magazynowania rejestrowania i Biblioteka klienta miejsca do magazynowania będzie wystarczające do diagnozowania problemu, ale w niektórych przypadkach może być konieczne bardziej szczegółowe informacje, które umożliwiają te narzędzia rejestrowania sieci. Na przykład za pomocą Fiddler do wyświetlania wiadomości HTTP i HTTPS umożliwia wyświetlanie danych nagłówka i ładunku przesyłane do i z usługami miejsca do magazynowania, który chcesz umożliwia sprawdzenie, jak aplikacja klienta prób operacji miejsca do magazynowania. Programy do analizowania protokołu takie jak Wireshark działać na poziomie pakietów, co umożliwia wyświetlanie danych TCP, które umożliwią rozwiązywanie tracone pakietów i problemy z łącznością. Analizatora wiadomości może działać na warstwach HTTP i TCP.

## <a name="end-to-end-tracing"></a>Śledzenie end-to-end

Śledzenie zakończenia do końca za pomocą różnych plików dziennika jest przydatne technika badanie potencjalnych problemów. Za pomocą danych Data/godzina z danych metryki jako wskazanie gdzie rozpocząć wyszukiwanie w plikach dziennika, aby uzyskać szczegółowe informacje, które mogą pomóc rozwiązać problem.

### <a name="correlating-log-data"></a>Korelacji danych dziennika

Podczas wyświetlania dzienniki w aplikacjach klienckich, sieci śledzi i żądania rejestrowania po stronie serwera magazynu należy mieć możliwość grupowania wielu plików dziennika różnych. Dzienniki zawierają wiele różnych pól, które są przydatne jako identyfikatory korelacji. Identyfikator żądania klienta jest najbardziej przydatna pole za pomocą przeniesionym wpisy w różnych dzienników. Jednak czasami może być przydatne użycie identyfikator żądania serwera lub sygnatury czasowe. Więcej informacji na temat tych opcji można znaleźć w poniższych sekcjach.

### <a name="client-request-id"></a>Identyfikator klienta

Biblioteka klienta miejsca do magazynowania umożliwia automatyczne generowanie identyfikatora żądania klienta unikatowe dla każdego żądania.

- W dzienniku po stronie klienta, która umożliwia utworzenie biblioteki klienta miejsca do magazynowania identyfikator żądania klienta pojawi się w polu **Identyfikator żądania klienta** każdym wpisie dziennika odnoszących się do wezwania.
- W śledzeniu sieci takiej jak przechwycone przez Fiddler jako wartość nagłówek **x-ms klienta żądanie id** protokołu HTTP jest widoczny w wiadomościach wezwanie na identyfikator żądania klienta.
- W dzienniku miejsca do magazynowania rejestrowania po stronie serwera identyfikator żądania klienta pojawi się w kolumnie Identyfikator klienta wezwanie.

> [AZURE.NOTE]Istnieje możliwość dla wielu żądań udostępniać ten sam identyfikator żądania klienta, ponieważ klienta można przypisać wartość (chociaż Biblioteka klienta miejsca do magazynowania automatycznie przypisuje nową wartość). W przypadku próby w kliencie wszystkie próby udostępnianie tej samej żądania klienta. W przypadku partii wysłanych od klienta partii ma identyfikator żądania jednego klienta.


### <a name="server-request-id"></a>Identyfikator żądania serwera

Usługa magazynu umożliwia automatyczne generowanie identyfikatorów wezwanie na serwerze.

- W dzienniku magazynowania rejestrowania po stronie serwera identyfikator żądania serwera zostanie wyświetlony **identyfikator żądania nagłówek** kolumny.
- W śledzeniu sieci takiej jak przechwycone przez Fiddler identyfikator żądania serwera jest wyświetlana w wiadomości odpowiedzi jako wartość nagłówek **x-ms żądanie id** protokołu HTTP.
- W dzienniku po stronie klienta, która umożliwia utworzenie biblioteki klienta miejsca do magazynowania identyfikator żądania serwera pojawi się w kolumnie **Operacji tekstu** wpis dziennika przedstawiający szczegóły odpowiedź serwera.

> [AZURE.NOTE] Usługa magazynu zawsze przypisuje serwer Unikatowy identyfikator żądania każdego żądania otrzymywanych, więc co ponawiania próby w kliencie i każdej operacji wchodzące w skład partii identyfikator żądania unikatowe serwera.

Jeśli biblioteka klienta miejsca do magazynowania zgłasza **StorageException** na komputerze klienckim, właściwość **RequestInformation** zawiera obiekt **RequestResult** , który zawiera właściwość **ServiceRequestID** . Można również uzyskać dostęp obiektu **RequestResult** z wystąpieniem **obiekt OperationContext** .

Przykładowy kod poniżej przedstawiono sposób ustaw wartość niestandardową **ClientRequestId** dołączając obiektu **obiekt OperationContext** żądania z usługą Magazyn. Ponadto przedstawia sposób pobierania wartości **ServerRequestId** w wiadomości odpowiedzi.

    //Parse the connection string for the storage account.
    const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Create an Operation Context that includes custom ClientRequestId string based on constants defined within the application along with a Guid.
    OperationContext oc = new OperationContext();
    oc.ClientRequestID = String.Format("{0} {1} {2} {3}", HOSTNAME, APPNAME, USERID, Guid.NewGuid().ToString());

    try
    {
        CloudBlobContainer container = blobClient.GetContainerReference("democontainer");
        ICloudBlob blob = container.GetBlobReferenceFromServer("testImage.jpg", null, null, oc);  
        var downloadToPath = string.Format("./{0}", blob.Name);
        using (var fs = File.OpenWrite(downloadToPath))
        {
            blob.DownloadToStream(fs, null, null, oc);
            Console.WriteLine("\t Blob downloaded to file: {0}", downloadToPath);
        }
    }
    catch (StorageException storageException)
    {
        Console.WriteLine("Storage exception {0} occurred", storageException.Message);
        // Multiple results may exist due to client side retry logic - each retried operation will have a unique ServiceRequestId
        foreach (var result in oc.RequestResults)
        {
                Console.WriteLine("HttpStatus: {0}, ServiceRequestId {1}", result.HttpStatusCode, result.ServiceRequestID);
        }
    }


### <a name="timestamps"></a>Sygnatury czasowe

Za pomocą sygnatury czasowe odszukaj wpisy dziennika pokrewne, ale Uważaj, aby z dowolnego pochylanie zegara między klienta i serwera, który może znajdować się. Czy wyszukiwanie plus lub minus 15 minut, aż pasujące wpisy po stronie serwera według sygnaturę czasową na komputerze klienckim. Należy pamiętać, że metadane obiektów blob dla blob zawierający metryki wskazują zakres czasu metryki przechowywanych w obiektów blob; jest to przydatne, jeśli masz wiele blob metryki dla tego samego minutę lub godzinę.

## <a name="troubleshooting-guidance"></a>Wskazówki dotyczące rozwiązywania problemów

Ta sekcja może pomóc diagnozowania i rozwiązywanie problemów związanych z niektórych typowych problemów dotyczących aplikacji mogą wystąpić podczas korzystania z usług Azure miejsca do magazynowania. Użyj poniższej listy, aby zlokalizować informacje dotyczące określonego problemu.

**Rozwiązywanie problemów z drzewa decyzji**

----------

Problemu związek między wykonanie jednej z usługi Magazyn?

- [Metryki Pokaż AverageE2ELatency wysokiej i niskiej AverageServerLatency]
- [Metryki Pokaż niskim AverageE2ELatency i niskiej AverageServerLatency, ale klient występuje duże opóźnienia]
- [Metryki Pokaż wysoka AverageServerLatency]
- [Występują nieoczekiwane opóźnień dostarczaniem wiadomości w kolejce]

----------

Problemu związek między dostępność usług miejsca do magazynowania?

- [Metryki pokazywanie wzrost w PercentThrottlingError]
- [Metryki pokazywanie wzrost w PercentTimeoutError]
- [Metryki pokazywanie wzrost w PercentNetworkError]

----------

Aplikacji klienckiej otrzymuje odpowiedź HTTP 4XX (na przykład 404) z usługą magazynu?

- [Klient otrzymuje wiadomości HTTP 403 (Dostęp zabroniony)]
- [Klient otrzymuje wiadomości HTTP 404 (nie można odnaleźć)]
- [Klient otrzymuje wiadomości HTTP 409 (konflikt)]

----------

[Metryki pokazywanie niskim PercentSuccess i pozycje dziennika analizy mają operacje ze stanem transakcji ClientOtherErrors]

----------

[Metryki wydajność pokazywanie wzrost nieoczekiwane w wykorzystania możliwości magazynu]

----------

[Występują nieoczekiwane ponownego uruchamiania maszyn wirtualnych, mające dużej liczby dołączony wirtualnych dysków twardych]

----------

[Problem wynika z przy użyciu emulatora miejsca do magazynowania dla projektowego lub testowego]

----------

[Wystąpią problemy z instalacją Azure SDK dla środowiska .NET]

----------

[Masz inny problem z usługą magazynu]

----------

### <a name="metrics-show-high-AverageE2ELatency-and-low-AverageServerLatency"></a>Metryki Pokaż AverageE2ELatency wysokiej i niskiej AverageServerLatency

Ilustracja poniżej z [Azure Portal](https://portal.azure.com) narzędzi do monitorowania przedstawiono przykład, w przypadku znacznie większą niż **AverageServerLatency** **AverageE2ELatency** .

![][4]

Warto zauważyć, że Usługa magazynu tylko oblicza metrykę **AverageE2ELatency** dla pomyślnego żądania, w odróżnieniu od **AverageServerLatency**zawiera czas klienta dane wysyłania i odbierania potwierdzenia z usługą Magazyn. W związku z tym różnica między **AverageE2ELatency** i **AverageServerLatency** może być z powodu z aplikacją kliencką powolnych odpowiedzieć lub ze względu na warunki w sieci.

> [AZURE.NOTE] Możesz także wyświetlić **E2ELatency** i **ServerLatency** dla operacji poszczególnych miejsca do magazynowania w ramach rejestrowania miejsca do magazynowania danych dziennika.

#### <a name="investigating-client-performance-issues"></a>Badanie problemów z wydajnością klienta

Możliwe przyczyny klienta odpowiada powoli zawierać nie ograniczoną liczbę dostępnych połączeń lub wątki lub jest mało zasobów, takich jak przepustowości Procesora, pamięci lub sieci. Można rozwiązać ten problem, zmieniając kod klienta efektywność (na przykład za pomocą asynchroniczne połączenia z usługą Magazyn) lub przy użyciu większe maszyny wirtualnej (rdzeni i więcej pamięci).

Usługi tabeli i kolejki algorytm Nagle'a mogą powodować wysokiej **AverageE2ELatency** w porównaniu z **AverageServerLatency**: Aby uzyskać więcej informacji, zobacz wpis [algorytm Nagle'a osoby jest nie przyjazny kierunku małych żądania](http://blogs.msdn.com/b/windowsazurestorage/archive/2010/06/25/nagle-s-algorithm-is-not-friendly-towards-small-requests.aspx). Algorytm Nagle'a w kodzie można wyłączyć za pomocą klasy **Element ServicePointManager** w obszarze nazw **System.Net** . Należy to zrobić, zanim wprowadzą wezwań do tabeli lub otworzyć kolejki usług w aplikacji, ponieważ nie wpływa połączenia, które już znajdują się. Poniższy przykład pochodzi z metody **Application_Start** w roli pracownika.

    var storageAccount = CloudStorageAccount.Parse(connStr);
    ServicePoint tableServicePoint = ServicePointManager.FindServicePoint(storageAccount.TableEndpoint);
    tableServicePoint.UseNagleAlgorithm = false;
    ServicePoint queueServicePoint = ServicePointManager.FindServicePoint(storageAccount.QueueEndpoint);
    queueServicePoint.UseNagleAlgorithm = false;

Sprawdź dzienniki po stronie klienta, aby zobaczyć, ile żądań, przesyłania aplikację klienta i sprawdź, czy ogólne .NET powiązane gardła wydajności w Twoim kliencie, takich jak Procesora, śmieci .NET, wykorzystania sieci lub pamięci. Jako punkt wyjścia do rozwiązywania problemów z aplikacji klienckich .NET zobacz [Debugowanie śledzenie i profilowanie](http://msdn.microsoft.com/library/7fe0dd2y).

#### <a name="investigating-network-latency-issues"></a>Badanie problemów z siecią opóźnienie

Zazwyczaj duże opóźnienia zakończenia do końca spowodowane przez sieć jest z powodu przejściowych warunków. Za pomocą narzędzi, takich jak Wireshark lub analizatora wiadomości programu Microsoft może analizować problemy oba problemy przejściowych i trwałych sieci, takich jak porzucone pakiety.

Aby uzyskać więcej informacji na temat rozwiązywania problemów z siecią za pomocą programu Wireshark, zobacz "[dodatek 2: Przechwytywanie ruchu w sieci za pomocą programu Wireshark]."

Aby uzyskać więcej informacji na temat rozwiązywania problemów z siecią za pomocą analizatora wiadomości programu Microsoft zobacz "[dodatku 3: przy użyciu analizatora Microsoft Message Przechwyć ruch sieciowy]."

### <a name="metrics-show-low-AverageE2ELatency-and-low-AverageServerLatency"></a>Metryki Pokaż niskim AverageE2ELatency i niskiej AverageServerLatency, ale klient występuje duże opóźnienia

W tym scenariuszu najbardziej prawdopodobną przyczyną jest opóźnienia w żądania magazynu osiągnięcia Usługa magazynu. Należy sprawdzić, dlaczego żądania od klienta są nie co za pośrednictwem usługi obiektów blob.

Jedna z możliwych przyczyn dla klienta opóźnienia, wysyłanie wezwań są dostępne ograniczoną liczbę dostępnych połączeń lub wątki.

Należy również sprawdzić, czy klient wykonuje wiele prób i badanie przyczyny, jeśli jest to możliwe. Aby określić, czy klient wykonuje wiele prób, można wykonywać następujące czynności:

- Sprawdź dzienniki analizy przestrzeni dyskowej. Jeśli są występuje wiele prób, zostanie wyświetlony wielu operacji z tym samym Identyfikatorem żądania klienta, ale wezwaniem na innym serwerze identyfikatorów.
- Sprawdź dzienniki klienta. Pełne rejestrowanie oznacza, że wystąpił ponów próbę.
- Debugowanie kodu, a następnie zaznacz właściwości obiektu **obiekt OperationContext** skojarzonej z żądaniem. Jeśli ma próby wykonania tej operacji właściwość **RequestResults** będzie zawierać wiele żądanie serwera unikatowych identyfikatorów. Możesz również sprawdzić godziny rozpoczęcia i zakończenia dla każdego żądania. Aby uzyskać więcej informacji zobacz przykładowy kod w sekcji [identyfikator żądania serwera].

Jeśli występują problemy na komputerze klienckim, należy zbadać potencjalnych problemów sieciowych, takich jak utraty pakietów. Narzędzi, takich jak Wireshark lub Microsoft wiadomości Analyzer umożliwia badanie problemów z siecią.

Aby uzyskać więcej informacji na temat rozwiązywania problemów z siecią za pomocą programu Wireshark, zobacz "[dodatek 2: Przechwytywanie ruchu w sieci za pomocą programu Wireshark]."

Aby uzyskać więcej informacji na temat Rozwiązywanie problemów z siecią za pomocą analizatora wiadomości firmy Microsoft, zobacz "[dodatku 3: przy użyciu analizatora Microsoft Message Przechwyć ruch sieciowy]."

### <a name="metrics-show-high-AverageServerLatency"></a>Metryki Pokaż wysoka AverageServerLatency

W przypadku wysokiej **AverageServerLatency** dla obiektów blob żądań pobierania należy sprawdzić, czy są powtarzających się wniosków o tym samym obiektów blob (lub zestawu obiektów blob) za pomocą dzienników rejestrowania miejsca do magazynowania. Dla obiektów blob przekazania żądania, należy badanie, jakie blok korzysta z rozmiar klienta (na przykład blokuje mniejszy niż 64 KB w rozmiarze może spowodować koszty ogólne, chyba że odczytywane są również w mniej niż 64 KB fragmentów), a wielu klientów przekazywania bloków konstrukcyjnych do samej obiektów blob równolegle. Należy również sprawdzić metryki na minutę maksymalnej liczby żądań, które powodują przekraczają na drugim skalowalność elementów docelowych: Zobacz też "[metryki Pokaż wzrost PercentTimeoutError]".

Jeśli widzisz wysokiej **AverageServerLatency** dla obiektów blob Pobierz żądania, gdy powtarzających się żądania tej samej obiektów blob lub zestawu obiektów blob, następnie należy rozważyć pamięci podręcznej tych obiektów blob przy użyciu pamięci podręcznej Azure lub sieci dostarczania zawartości (CDN) Azure. Żądania przekazywania można zwiększyć przepustowość za pomocą rozmiar bloku. W przypadku kwerend do tabel użytkownik może również do wykonania po stronie klienta pamięci podręcznej na komputerach klienckich, które wykonują kwerendy operacji i miejsce, w którym dane nie zmienia się często.

Wartości maksymalne **AverageServerLatency** można także objawem źle zaprojektowany tabele lub kwerendy, że wynik w operacji skanowania lub że postępuj zgodnie ze wzorcem ochrony przed Dołącz/dołączy. Aby uzyskać więcej informacji, zobacz "[metryki Pokaż wzrost PercentThrottlingError]".

> [AZURE.NOTE] Pełna lista kontrolna wydajności listy kontrolnej w tym miejscu można znaleźć: [Microsoft Azure miejsca do magazynowania wydajność i skalowalność — Lista kontrolna](storage-performance-checklist.md).

### <a name="you-are-experiencing-unexpected-delays-in-message-delivery"></a>Występują nieoczekiwane opóźnień dostarczaniem wiadomości w kolejce

Występują opóźnienia między czasu, aplikacja dodaje wiadomości do kolejki i razem, gdy będzie dostępny do czytania z kolejki, należy wykonać następujące czynności, aby diagnozowanie problemu:

- Sprawdź, czy aplikacja jest pomyślnie dodanie wiadomości do kolejki. Sprawdź, czy aplikacja jest nie ponawianie metody **AddMessage** kilka razy przed następnych. Biblioteka klienta miejsca do magazynowania dzienniki zawierają wszystkie kolejne próby operacji miejsca do magazynowania.
- Sprawdź zegar nie jest skośność między roli Pracownik, który zapewnia wiadomość do kolejki i roli Pracownik, który odczytuje wiadomość z kolejki, który ułatwia pojawiają się tak, jakby występuje opóźnienie przetwarzania.
- Sprawdź, czy jest możliwe roli Pracownik, który odczytuje wiadomości z kolejki. Jeśli klient kolejki wywołuje metodę **GetMessage** , ale nie odpowiada potwierdzenia, wiadomość pozostanie niewidoczne w kolejce do wygaśnięcia okresu **invisibilityTimeout** . W tym momencie wiadomość staje się dostępna dla przetwarzania ponownie.
- Sprawdzanie, jeśli długość kolejki rośnie w czasie. To może wystąpić, jeśli nie masz wystarczających pracowników dostępne dla wszystkich wiadomości, które inni pracownicy umieszcza się w kolejce procesu. Należy również sprawdzić metryki, aby zobaczyć, jeśli żądania delete błędu i liczba kolejki wiadomości, które mogą wskazywać powtarzają się nie powiodło się próby usunięcia wiadomości.
- Sprawdź dzienniki rejestrowania miejsca do magazynowania dla wszystkie operacje kolejki jest większa niż oczekiwane wartości **E2ELatency** i **ServerLatency** w dłuższym okresie czasu niż zwykle.


### <a name="metrics-show-an-increase-in-PercentThrottlingError"></a>Metryki pokazywanie wzrost w PercentThrottlingError

Ograniczanie błędów po przekroczeniu celów skalowalność Usługa magazynu. Usługa magazynu oznacza to, aby upewnić się, że nie pojedynczego klienta lub dzierżawy za pomocą usługi koszt innych osób. Aby uzyskać więcej informacji zobacz [skalowalność miejsca do magazynowania Azure i cele](storage-scalability-targets.md) szczegółowe informacje na temat elementów docelowych skalowalność w przypadku kont miejsca do magazynowania i cele wydajności dla partycje w ramach konta miejsca do magazynowania.

Metryka **PercentThrottlingError** pokazać wzrost w procentach żądań, które występują z błędem ograniczania, musisz badanie jednej z dwóch sytuacji:

- [Przejściowych wzrost PercentThrottlingError]
- [Trwały wzrost PercentThrottlingError błędu]

Wzrost **PercentThrottlingError** często występuje w tym samym czasie co większa liczba żądań miejsca do magazynowania lub po początkowym ładowanie testowania aplikacji. To może również pojawiają się w kliencie jako "503 Serwer zajęty" lub "500 limit czasu operacji" HTTP komunikaty o stanie z operacji przestrzeni dyskowej.

#### <a name="transient-increase-in-PercentThrottlingError"></a>Przejściowych wzrost PercentThrottlingError

Jeśli widzisz tych najwyższych wartościach w wartości **PercentThrottlingError** , które pokrywają się z okresów wysoce stosowania wykonała wykładniczego (nie liniowe) wycofywania strategię ponowne próby w Twoim kliencie: to zmniejszenia obciążenia natychmiastowej partycją i pomocy aplikacji wygładzanie tych najwyższych wartościach w ruchu. Aby uzyskać więcej informacji o sposobie implementacji ponów próbę zasady używania biblioteki klienta miejsca do magazynowania zobacz [Namespace Microsoft.WindowsAzure.Storage.RetryPolicies](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.retrypolicies.aspx).

> [AZURE.NOTE] Może także zostać wyświetlony tych najwyższych wartościach w wartości **PercentThrottlingError** , które nie pokrywają się z okresów wysoce stosowania: najbardziej prawdopodobną przyczyną jest Usługa magazynu przenoszenia partycje, aby poprawić równoważenia obciążenia.

#### <a name="permanent-increase-in-PercentThrottlingError"></a>Trwały wzrost PercentThrottlingError błędu

Jeśli widzisz wysoką wartość dla następujących **PercentThrottlingError** trwały wzrost wielkości swojej transakcji lub podczas przeprowadzania programu ładowania początkowego testów w swojej aplikacji, a następnie należy oceny, jak aplikacja używa partycje miejsca do magazynowania i czy zbliża celów skalowalność konta miejsca do magazynowania. Na przykład jeśli widzisz ograniczania błędy w kolejce (który liczy jako jedną partycją), następnie należy rozważyć rozciągnąć transakcji wielu partycje za pomocą dodatkowe kolejki. Jeśli widzisz ograniczania błędy w tabeli, należy wziąć pod uwagę rozciągnąć swoje transakcje wielu partycje za pomocą szerszej grupie wartości klucza partition przy użyciu inny schemat podziału. Jedną z typowych przyczyn tego problemu jest prepend/dołączanie wzorcem ochrony przed miejsce, w którym wybierz datę jako klucz partition, a następnie wszystkie dane w określonym dniu jest zapisywany jedną partycją: obciążeniu, może to powodować gardło zapisu. Należy rozważyć inny projekt podziału lub sprawdzić, czy użycie magazyn obiektów blob może być lepszym rozwiązaniem. Należy sprawdzić, czy ograniczania dotyczył wyniku tych najwyższych wartościach w ruchu i badanie sposobów wygładzanie deseniu żądań.

Rozpowszechniasz swoje transakcje u wielu partycje nadal należy pamiętać o limitów skalowalność, ustaw dla konta miejsca do magazynowania. Na przykład jeśli użyto dziesięć kolejek przetwarzania maksymalnie 2000 wiadomości 1KB na sekundę, będzie w ogólny limit 20 000 wiadomości dla drugiego konta miejsca do magazynowania. Jeśli potrzebujesz więcej niż 20 000 jednostek na sekundę procesu, należy rozważyć, użycie wielu kont miejsca do magazynowania. Możesz powinien mieć na uwadze, że rozmiar żądania i jednostki ma wpływ na kiedy usługa magazynu ogranicza klientów: Jeśli masz większe żądania i jednostki, może być możesz wcześniej ograniczenie.

Projekt kwerendy nieefektywne może również prowadzić trafienie limity skalowalność partycje tabeli. Na przykład kwerendy przy użyciu filtru, który zaznacza tylko jednego procenta podmiotów w partycją, ale która skanuje wszystkie jednostki w partycją musisz uzyskać dostęp do każdego obiektu. Każdy podmiot, przeczytaj będą uwzględniane całkowita liczba transakcji w tym partition; w związku z tym można łatwo osiągnąć celów skalowalność.

> [AZURE.NOTE] Testowanie wydajności okaże się żadnego projektu kwerendy nieefektywne w aplikacji.

### <a name="metrics-show-an-increase-in-PercentTimeoutError"></a>Metryki pokazywanie wzrost w PercentTimeoutError

Usługi metryki pokazywanie wzrost w **PercentTimeoutError** dla jednej z usług przechowywania. W tym samym czasie którym klient otrzyma dużej liczby komunikaty o stanie "500 limit czasu operacji" HTTP z operacji miejsca do magazynowania.

> [AZURE.NOTE] Można zobaczyć błędy Przekroczono limit czasu tymczasowo jako usługa magazynu Ładowanie sald żądania, przenosząc partycją do nowego serwera.

Metryka **PercentTimeoutError** jest agregacją następujące metryki: **ClientTimeoutError**, **AnonymousClientTimeoutError** **SASClientTimeoutError**, **ServerTimeoutError**, **AnonymousServerTimeoutError**i **SASServerTimeoutError**.

Limity czasu serwera są spowodowane błąd na serwerze. Limity czasu klienta mieć następujące przyczyny operacji na serwerze przekroczono limit czasu określony przez klienta; na przykład klienta przy użyciu magazynu Biblioteka klienta można ustawić limit czasu dla operacji przy użyciu właściwości **ServerTimeout** klasy **QueueRequestOptions** .

Limity czasu serwera wskazywać na problem z usługą Magazyn, która wymaga dalszego dochodzenia. Za pomocą metryki chcesz wyświetlić, jeśli są naciśnięcie limity skalowalność usługi i określić dowolny z tych najwyższych wartościach w ruchu, które mogą powodować ten problem. Jeśli problem jest przerw, może to być spowodowane równoważenia obciążenia działań w usłudze. Jeśli problem jest trwała, nie jest powodowany przez naciśnięcie limity skalowalność usługi aplikacji powinna podnieść problem pomocy technicznej. Limity czasu klienta należy zdecydować, jeśli limit czasu jest ustawiona na odpowiednią wartość w kliencie i tych zmian, ustaw wartość limitu czasu w kliencie lub badanie, jak można poprawić wydajność operacji w usłudze miejsca do magazynowania, na przykład optymalizowania zapytań tabeli lub zmniejszyć rozmiar wiadomości.

### <a name="metrics-show-an-increase-in-PercentNetworkError"></a>Metryki pokazywanie wzrost w PercentNetworkError

Usługi metryki pokazywanie wzrost w **PercentNetworkError** dla jednej z usług przechowywania. Metryka **PercentNetworkError** jest agregacją następujące metryki: **NetworkError**, **AnonymousNetworkError**i **SASNetworkError**. Te występują, gdy Usługa magazynu wykrywa błąd sieciowy klient wysyła żądanie przestrzeni dyskowej.

Najczęstsze przyczyny tego błędu jest klientem odłączanie przekroczenie limitu czasu wygaśnięcia w usłudze miejsca do magazynowania. Kod należy badanie w Twoim kliencie, aby dowiedzieć się, dlaczego i kiedy klient odłącza od usługi miejsca do magazynowania. Aby zbadać problemy z łącznością sieciową w kliencie, można użyć programu Wireshark, Microsoft Analyzer wiadomości lub Tcping. Te narzędzia są opisane w [dodatkach].

### <a name="the-client-is-receiving-403-messages"></a>Klient otrzymuje wiadomości HTTP 403 (Dostęp zabroniony)

Jeśli aplikacji klienckiej jest generowania błędów HTTP 403 (Dostęp zabroniony), prawdopodobną przyczyną jest klient korzysta wygasłe udostępnione dostępu podpisu (SA) podczas wysyłania żądania magazynu (Chociaż inne możliwe przyczyny są zegar skośność, nieprawidłowe klucze i nagłówki puste). Jeśli przyczyną jest wygasłej klucz skojarzeń zabezpieczeń, nie zobaczą wpisów w danych dziennika miejsca do magazynowania rejestrowania po stronie serwera. W poniższej tabeli pokazano przykładowe w dzienniku po stronie klienta wygenerowane przez Biblioteka klienta miejsca do magazynowania, który przedstawia ten problem występuje:

Źródła|Szczegółowości|Szczegółowości|Identyfikator klienta|Operacja tekstu
---|---|---|---|---
Microsoft.WindowsAzure.Storage|Informacje o|3|85d077ab-...|Uruchamianie operacji z lokalizacją głównego tryb lokalizacji PrimaryOnly na.
Microsoft.WindowsAzure.Storage|Informacje o|3|85d077ab-...|Uruchamianie synchroniczne żądanie https://domemaildist.blob.core.windows.netazureimblobcontainer/blobCreatedViaSAS.txt?sv=2014-02-14&amp;sr = c&amp;tożsamości usługi = mypolicy&amp;podpis = 2BmcR6zbudIH2F5Ikm % 2FyhNYZEmJNQ OFnd4Rd7z01fIvh % 3D&amp;wersja api = 2014-02-14.
Microsoft.WindowsAzure.Storage|Informacje o|3|85d077ab-...|Oczekiwanie na odpowiedź.
Microsoft.WindowsAzure.Storage|Ostrzeżenie|2|85d077ab-...|Wyjątek podczas oczekiwanie na odpowiedź: serwer zdalny zwrócił błąd: dostęp zabroniony (403).
Microsoft.WindowsAzure.Storage|Informacje o|3|85d077ab-...|Odebrane odpowiedzi. Kod stanu = 403, identyfikator żądania = 9d67c64a-64ed-4b0d-9515-3b14bbcdc63d, MD5 zawartości = ETag =.
Microsoft.WindowsAzure.Storage|Ostrzeżenie|2|85d077ab-...|Wyjątek podczas operacji: serwer zdalny zwrócił błąd: dostęp zabroniony (403).
Microsoft.WindowsAzure.Storage|Informacje o|3 |85d077ab-...|Sprawdzanie, jeśli wykonana operacji należy ponownie. Liczba prób = 0, kod stanu HTTP = 403, wyjątek = Serwer zdalny zwrócił błąd: dostęp zabroniony (403).
Microsoft.WindowsAzure.Storage|Informacje o|3|85d077ab-...|Następnej lokalizacji została ustawiona jako podstawowego na podstawie trybu lokalizacji.
Microsoft.WindowsAzure.Storage|Błąd|1|85d077ab-...|Zasady ponów próbę zezwala na ponowienie próby. Niepowodzenie z serwer zdalny zwrócił błąd: dostęp zabroniony (403).

W tym scenariuszu należy sprawdzić, dlaczego token skojarzeń zabezpieczeń wygasa, zanim klient wysyła tokenu na serwerze:

- Zazwyczaj nie należy ustawić czas rozpoczęcia podczas tworzenia skojarzenia zabezpieczeń dla klienta do używania natychmiast. W przypadku małych zegar różnice między hosta generowania skojarzeń zabezpieczeń przy użyciu bieżącej godziny i usługą Magazyn, a następnie jest możliwe do otrzymania skojarzeń zabezpieczeń, który nie jest jeszcze ważny usługę miejsca do magazynowania.
- Nie należy ustawiać czasu wygaśnięcia bardzo krótki na skojarzenia zabezpieczeń. Ponownie małym zegarem różnice między hosta Generowanie skojarzeń zabezpieczeń i Usługa magazynu może prowadzić do skojarzenia zabezpieczeń pozornie wygasa wcześniej niż przewidywano.
- Czy parametr wersji w kluczu skojarzeń zabezpieczeń (na przykład **OHRP = 2015-04-05**) odpowiada wersji Biblioteka klienta miejsca do magazynowania używasz? Zaleca się zawsze używać najnowszej wersji [Biblioteka klienta miejsca do magazynowania](https://www.nuget.org/packages/WindowsAzure.Storage/).
- Jeśli możesz ponownie wygenerować klawiszy dostępu do magazynowania, to powodować wszelkie istniejące tokeny skojarzeń zabezpieczeń. Może to być problem, jeśli wygenerować tokeny skojarzenia zabezpieczeń z czas wygaśnięcia długie aplikacje klienckie pamięci podręcznej.

Jeśli używasz biblioteki klienta miejsca do magazynowania do generowania tokenów skojarzeń zabezpieczeń, następnie jest proste utworzyć prawidłowy token. Jednak jeśli korzystasz z interfejsu API usługi REST magazynowania i ręcznie konstruowanie tokenów skojarzeń zabezpieczeń należy uważnie przeczytaj temat [Delegowanie dostępu za pomocą podpisu dostępu udostępnione](http://msdn.microsoft.com/library/azure/ee395415.aspx).

### <a name="the-client-is-receiving-404-messages"></a>Klient otrzymuje wiadomości HTTP 404 (nie można odnaleźć)
Jeśli z aplikacją kliencką odbiera wiadomości HTTP 404 (nie można odnaleźć) na serwerze, oznacza to, że obiekt, który klient próbował używać (na przykład jednostki, tabeli, obiektów blob, kontener lub kolejka) nie istnieje w usłudze miejsca do magazynowania. Istnieje kilka możliwych przyczyn tego, takie jak:

- [Klient lub inny proces poprzednio usunięte obiektu]
- [Problem autoryzacji podpisu udostępnionych programu Access (SA)]
- [Kod JavaScript po stronie klienta nie ma uprawnień dostępu do obiektu]
- [Błąd sieci]

#### <a name="client-previously-deleted-the-object"></a>Klient lub inny proces poprzednio usunięte obiektu
W sytuacjach, gdy klient próbować odczytywanie, aktualizowanie lub usuwanie danych w usłudze miejsca do magazynowania jest zazwyczaj można z łatwością do identyfikowania w dziennikach po stronie serwera poprzedniej operacji usuniętego danego obiektu z usługą Magazyn. Bardzo często danych dziennika pokazano, że innego użytkownika lub proces usunął obiekt. W dzienniku miejsca do magazynowania rejestrowania po stronie serwera typu operacji i zażądania obiektu-kolumn klucza Pokaż usunięcia obiektu przez klienta.

W scenariuszu miejsce, w którym klient próbuje Wstawianie obiektu go może nie być natychmiast pewne Dlaczego efektem odpowiedź HTTP 404 (nie można odnaleźć) zakładając, że klient jest utworzenie nowego obiektu. Jednak klient jest tworzenie obiektów blob należy może znaleźć kontenera obiektów blob klient tworzy komunikat informujący o tym, że muszą mieć możliwość wyszukiwania kolejki, a klient jest dodanie wiersza muszą mieć możliwość znalezienia tabeli.

Aby uzyskać bardziej szczegółowe poznanie gdy klient wysyła próśb z usługą Magazyn służy dziennik po stronie klienta od Biblioteka klienta przestrzeni dyskowej.

Dziennik po stronie klienta następujące wygenerowane przez Biblioteka klienta miejsca do magazynowania ilustruje problem, gdy klient nie może znaleźć kontenera dla obiektów blob, który jest utworzenie. Dziennik zawiera szczegóły dotyczące następujących czynności:

Identyfikator żądania|Operacja
---|---
07b26a5d-...|Metoda **DeleteIfExists** usunąć kontenera obiektów blob. Należy zauważyć, że operacja zawiera żądanie **głowy** , aby sprawdzić występowanie kontenerze.
e2d06d78...|Metodę **CreateIfNotExists** w celu utworzenia kontenera obiektów blob. Zauważ, że operacja zawiera żądanie **głowy** sprawdza kontenera. **Szef** zwraca 404 wiadomości, ale będzie nadal występował.
de8b1c3c-...|Metodę **UploadFromStream** w celu utworzenia obiektów blob. Żądanie **umieszczenie** kończy się niepowodzeniem z komunikatem 404

Wpisy dziennika:

Identyfikator żądania |  Operacja tekstu
---|---
07b26a5d-...|Uruchamianie synchroniczne żądanie https://domemaildist.blob.core.windows.net/azuremmblobcontainer.
07b26a5d-...|StringToSign = HEAD...x-ms-client-request-id:07b26a5d-...x-ms-date:Tue, 03 2014 cze 10:33:11 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer.restype:container.
07b26a5d-...|Oczekiwanie na odpowiedź.
07b26a5d-... | Odebrane odpowiedzi. Kod stanu = 200, identyfikator żądania = eeead849... Zawartość MD5 = ETag = &quot;0x8D14D2DC63D059B&quot;.
07b26a5d-... | Nagłówki odpowiedzi zostały przetworzone pomyślnie, kontynuowanie pozostałą część tej operacji.
07b26a5d-... | Pobieranie treść odpowiedzi.
07b26a5d-... | Operacja ukończona pomyślnie.
07b26a5d-... | Uruchamianie synchroniczne żądanie https://domemaildist.blob.core.windows.net/azuremmblobcontainer.
07b26a5d-... | StringToSign = DELETE...x-ms-client-request-id:07b26a5d-...x-ms-date:Tue, 03 2014 cze 10:33:12 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer.restype:container.
07b26a5d-... | Oczekiwanie na odpowiedź.
07b26a5d-... | Odebrane odpowiedzi. Kod stanu = 202, identyfikator żądania = 6ab2a4cf-..., MD5 zawartości = ETag =.
07b26a5d-... | Nagłówki odpowiedzi zostały przetworzone pomyślnie, kontynuowanie pozostałą część tej operacji.
07b26a5d-... | Pobieranie treść odpowiedzi.
07b26a5d-... | Operacja ukończona pomyślnie.
e2d06d78-... | Uruchamianie asynchroniczne żądanie https://domemaildist.blob.core.windows.net/azuremmblobcontainer.</td>
e2d06d78-... | StringToSign = HEAD...x-ms-client-request-id:e2d06d78-...x-ms-date:Tue, 03 2014 cze 10:33:12 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer.restype:container.
e2d06d78-...| Oczekiwanie na odpowiedź.
de8b1c3c-... | Uruchamianie synchroniczne żądanie https://domemaildist.blob.core.windows.net/azuremmblobcontainer/blobCreated.txt.
de8b1c3c-... |  StringToSign = położenie... 64.qCmF+TQLPhq/YYK50mP9ZQ==...x-MS-blob-type:BlockBlob.x-MS-Client-Request-ID:de8b1c3c-...x-MS-Date:TUE, 03 2014 cze 10:33:12 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer/blobCreated.txt.
de8b1c3c-... | Przygotowanie do zapisywania danych wezwanie.
e2d06d78-... | Wyjątek podczas oczekiwanie na odpowiedź: serwer zdalny zwrócił błąd: (404) nie można odnaleźć.
e2d06d78-... | Odebrane odpowiedzi. Kod stanu = 404, identyfikator żądania = 353ae3bc-..., MD5 zawartości = ETag =.
e2d06d78-... | Nagłówki odpowiedzi zostały przetworzone pomyślnie, kontynuowanie pozostałą część tej operacji.
e2d06d78-... | Pobieranie treść odpowiedzi.
e2d06d78-... | Operacja ukończona pomyślnie.
e2d06d78-... | Uruchamianie asynchroniczne żądanie https://domemaildist.blob.core.windows.net/azuremmblobcontainer.
e2d06d78-...|StringToSign = położenie... 0...x-MS-Client-Request-ID:e2d06d78-...x-MS-Date:TUE, 03 2014 cze 10:33:12 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer.restype:container.
e2d06d78-... | Oczekiwanie na odpowiedź.
de8b1c3c-... | Dane żądania pisania.
de8b1c3c-... | Oczekiwanie na odpowiedź.
e2d06d78-... | Wyjątek podczas oczekiwanie na odpowiedź: serwer zdalny zwrócił błąd: konflikt (409).
e2d06d78-... | Odebrane odpowiedzi. Kod stanu = 409, identyfikator żądania = c27da20e-..., MD5 zawartości = ETag =.
e2d06d78-... | Pobieranie treść odpowiedzi błędu.
de8b1c3c-... | Wyjątek podczas oczekiwanie na odpowiedź: serwer zdalny zwrócił błąd: (404) nie można odnaleźć.
de8b1c3c-... | Odebrane odpowiedzi. Kod stanu = 404, identyfikator żądania = 0eaeab3e-..., MD5 zawartości = ETag =.
de8b1c3c-...| Wyjątek podczas operacji: serwer zdalny zwrócił błąd: (404) nie można odnaleźć.
de8b1c3c-... | Zasady ponów próbę zezwala na ponowienie próby. Niepowodzenie z serwer zdalny zwrócił błąd: (404) nie można odnaleźć.
e2d06d78-... | Zasady ponów próbę zezwala na ponowienie próby. Niepowodzenie z serwer zdalny zwrócił błąd: konflikt (409).

W tym przykładzie dziennik pokazano, że klient jest przeplataniem żądania od metody **CreateIfNotExists** (żądanie identyfikator e2d06d78...) za pomocą żądania metody **UploadFromStream** (de8b1c3c...); Dzieje się tak, ponieważ z aplikacją kliencką wywoływanie asynchroniczne tych metod. Należy zmodyfikować kod asynchroniczny na komputerze klienckim, aby upewnić się, który tworzy kontener przed podjęciem próby przekazać wszelkie dane do obiektów blob w danym kontenerze. Najlepiej, jeśli należy utworzyć wszystkich kontenerów z wyprzedzeniem.

#### <a name="SAS-authorization-issue"></a>Problem autoryzacji podpisu udostępnionych programu Access (SA)

Jeśli z aplikacją kliencką próbuje użyć klucza skojarzeń zabezpieczeń, który nie ma uprawnień wymaganych do działania, Usługa magazynu zwraca komunikat HTTP 404 (nie można odnaleźć) do klienta. W tym samym czasie pojawi się także wartość różną od zera dla **SASAuthorizationError** w metryki.

W poniższej tabeli przedstawiono Przykładowa wiadomość dziennika po stronie serwera z pliku dziennika magazynowania rejestrowania:

<table>
  <tr>
    <td>Czas rozpoczęcia żądania</td>
    <td>2014-05-30T06:17:48.4473697Z</td>
  </tr>
  <tr>
    <td>Typ operacji</td>
    <td>GetBlobProperties</td>
  </tr>
  <tr>
    <td>Stan żądania</td>
    <td>SASAuthorizationError</td>
  </tr>
  <tr>
    <td>Kod stanu HTTP</td>
    <td>404</td>
  </tr>
  <tr>
    <td>Typ uwierzytelniania</td>
    <td>Skojarzenia zabezpieczeń</td>
  </tr>
  <tr>
    <td>Typ usługi</td>
    <td>Obiektów blob</td>
  </tr>
  <tr>
    <td>Adres URL żądania</td>
    <td>
    https://domemaildist.blob.Core.Windows.NET/azureimblobcontainer/blobCreatedViaSAS.txt?sv=2014-02-14&amp;amp; sr = c&amp;amp; tożsamości usługi = mypolicy&amp;amp; podpis = XXXXX&amp;amp; wersja api = 2014-02-14&amp;amp;</td>
  </tr>
  <tr>
    <td>Żądanie nagłówka identyfikator</td>
    <td>a1f348d5-8032-4912-93ef-b393e5252a3b</td>
  </tr>
  <tr>
    <td>Identyfikator klienta</td>
    <td>2d064953-8436-4ee0-aa0c-65cb874f7929</td>
  </tr>
</table>

Należy sprawdzić, dlaczego aplikacji klienckiej próbuje wykonywanie operacji, które go nie udzielono uprawnień dla.

#### <a name="JavaScript-code-does-not-have-permission"></a>Kod JavaScript po stronie klienta nie ma uprawnień dostępu do obiektu

Jeśli używasz klienta JavaScript i Usługa magazynu zwraca HTTP 404 wiadomości, można sprawdzić następujące błędy języka JavaScript w przeglądarce:

    SEC7120: Origin http://localhost:56309 not found in Access-Control-Allow-Origin header.
    SCRIPT7002: XMLHttpRequest: Network Error 0x80070005, Access is denied.

> [AZURE.NOTE] Za pomocą narzędzia Projektant F12 w programie Internet Explorer do śledzenia wiadomości wymienianych między przeglądarką i Usługa magazynu podczas rozwiązywania problemów JavaScript po stronie klienta.

Te błędy, ponieważ przeglądarki sieci web wprowadza ograniczenia zabezpieczeń [tych samych zasad pochodzenia](http://www.w3.org/Security/wiki/Same_Origin_Policy) zapobiega nawiązywania połączeń z interfejsem API innej domeny z domeny, które strony pochodzi od strony sieci web.

Aby obejść ten problem JavaScript, można skonfigurować krzyżowe Origin zasobów udostępniania (CORS) w usłudze miejsca do magazynowania, który klient uzyskuje dostęp do. Aby uzyskać więcej informacji zobacz [Udostępnianie zasobów między Origin (CORS) obsługę usługi Azure miejsca do magazynowania](http://msdn.microsoft.com/library/azure/dn535601.aspx).

Poniższy przykład kodu przedstawiono sposób konfigurowania usługi obiektów blob, aby umożliwić JavaScript systemem w domenie firmy Contoso uzyskać dostęp do obiektów blob w usłudze magazyn obiektów blob:

    CloudBlobClient client = new CloudBlobClient(blobEndpoint, new StorageCredentials(accountName, accountKey));
    // Set the service properties.
    ServiceProperties sp = client.GetServiceProperties();
    sp.DefaultServiceVersion = "2013-08-15";
    CorsRule cr = new CorsRule();
    cr.AllowedHeaders.Add("*");
    cr.AllowedMethods = CorsHttpMethods.Get | CorsHttpMethods.Put;
    cr.AllowedOrigins.Add("http://www.contoso.com");
    cr.ExposedHeaders.Add("x-ms-*");
    cr.MaxAgeInSeconds = 5;
    sp.Cors.CorsRules.Clear();
    sp.Cors.CorsRules.Add(cr);
    client.SetServiceProperties(sp);

#### <a name="network-failure"></a>Błąd sieci

W niektórych okolicznościach pakietów sieciowych utracone może prowadzić do Usługa magazynu zwracania wiadomości HTTP 404 do klienta. Na przykład gdy aplikacji klienckiej jest usuwanie obiektu z usługi tabeli pojawi się klienta zostać zgłoszony zgłoszenie wyjątku miejsca do magazynowania "HTTP 404 (nie można odnaleźć)" komunikat o stanie w usłudze tabeli. Badanie tabeli w usłudze tabeli miejsca do magazynowania, zobacz, usługa została usunięta jednostka zgodnie z żądaniem.

Szczegóły wyjątku w kliencie obejmują identyfikator żądania (7e84f12d...), przypisane przez usługę tabeli żądania: za pomocą tych informacji do znajdowania informacje dotyczące żądania w dziennikach po stronie serwera magazynu przez wyszukiwanie w kolumnie **nagłówka identyfikator żądania** w pliku dziennika. Można także użyć metryki do identyfikowania podczas występują błędy, takich jak, a następnie wyszukaj pliki dziennika, na podstawie czasu metryki zarejestrowanego ten błąd. Ten wpis dziennika pokazano, że usunięcie nie powiodło się komunikat o stanie "Klienta inny błąd HTTP (404)". Sam wpis dziennika zawiera również identyfikatorem żądania wygenerowane przez klienta w kolumnie **identyfikator klienta — żądanie** (813ea74f...).

Dziennik po stronie serwera umożliwia także innego wpisu o tej samej **klienta żądanie identyfikator** wartości (813ea74f...) dla operacji usuwania pomyślnego dla tej samej jednostki i z tego samego klienta. Operacja usuwania pomyślnego nastąpiło wkrótce przed nie powiodło się usuwanie zaproszenia.

Najbardziej prawdopodobną przyczyną tego scenariusza to, że klient wysyłane żądanie usunięcia obiektu do usługę tabeli, która zakończyła się pomyślnie, ale nie dotarła potwierdzenie z serwera (prawdopodobnie ze względu na to problem tymczasowy sieci). Klient następnie automatycznie po ponownych próbach operacji (przy użyciu tego samego **identyfikatora w przypadku żądania klienta**), a ten ponów próbę zakończone niepowodzeniem, ponieważ jednostka już zostało usunięte.

Jeśli ten problem występuje często, należy badanie, dlaczego klient jest możliwe do odbierania potwierdzenia w usłudze tabeli. W przypadku przerw problem powinien przechwytywania komunikat o błędzie "Nie znaleziono HTTP (404)" i zaloguj się w kliencie, ale zezwalaj klienta kontynuować.

### <a name="the-client-is-receiving-409-messages"></a>Klient otrzymuje wiadomości HTTP 409 (konflikt)

W poniższej tabeli przedstawiono wyciąg z dwóch operacje klienckie w dzienniku po stronie serwera: **DeleteIfExists** bezpośrednio po **CreateIfNotExists** przy użyciu tej samej nazwy kontenera obiektów blob. Należy zauważyć, że wyniki operacji każdego klienta w dwóch żądania przesyłane do serwera, najpierw żądanie **GetContainerProperties** , aby sprawdzić, czy istnieje kontenera, a po nim **DeleteContainer** lub **tworzony kontener** wezwanie.

Sygnatura czasowa|Operacja|Wynik|Nazwa kontenera|Identyfikator klienta
---|---|---|---|---
05:10:13.7167225|GetContainerProperties|200|mmcont|c9f52c89-...
05:10:13.8167325|DeleteContainer|202|mmcont|c9f52c89-...
05:10:13.8987407|GetContainerProperties|404|mmcont|bc881924-...
05:10:14.2147723|Tworzony kontener|409|mmcont|bc881924-...

Kod w aplikacji klienckiej usuwa i od razu odtwarza kontenera obiektów blob pod tą samą nazwą: metody **CreateIfNotExists** (żądania klienta ID bc881924-...) po pewnym czasie kończy się niepowodzeniem z powodu błędu HTTP 409 (konflikt). Kiedy klienta powoduje usunięcie obiektów blob kontenery, tabel lub kolejek jest krótki okres przed nazwą staje się dostępna ponownie.

Z aplikacją kliencką należy używać kontenera unikatowych nazw, gdy tworzy nowe kontenerów jeśli deseń Usuń/ponownie utwórz jest typowych.

### <a name="metrics-show-low-percent-success"></a>Metryki pokazywanie niskim PercentSuccess i pozycje dziennika analizy mają operacje ze stanem transakcji ClientOtherErrors

Metryka **PercentSuccess** przechwytuje wartości procentowej operacji, które zostały pomyślnie na podstawie ich kodu stanu HTTP. Operacje przy użyciu kodów stanu 2XX zliczanie jako pomyślnego operacji przy użyciu kodów stanu w zakresach 3XX, 4XX i 5XX są liczone jako niepowodzeniem i zmniejsz wartość metryki **PercentSucess** . W plikach dziennika magazynu po stronie serwera te operacje są rejestrowane ze stanem transakcji **ClientOtherErrors**.

Należy zauważyć, że te operacje zostały wykonane pomyślnie i dlatego nie ma wpływu na inne wskaźniki, takie jak dostępność. Przykłady działań, które wykonana pomyślnie, ale którą może powodować niepowodzenie kody stanu HTTP obejmują:
- **ResourceNotFound** (Nie można odnaleźć 404), na przykład z żądania GET do obiektów blob, który nie istnieje.
- **ResouceAlreadyExists** (Konflikt 409), na przykład z operacji **CreateIfNotExist** , w którym zasób już istnieje.
- **ConditionNotMet** (Nie został zmodyfikowany 304), na przykład z warunkowego operacji, takich jak gdy klient wysyła wartość **ETag** i nagłówek HTTP **Jeżeli żaden-dopasowania** , aby zażądać obrazu tylko wtedy, gdy została zaktualizowana od ostatniej operacji.

Można znaleźć listę typowych kody błędów interfejsu API usługi REST zwracające na stronie [Typowe kody błędów interfejs API pozostałych](http://msdn.microsoft.com/library/azure/dd179357.aspx)usług magazynu.

### <a name="capacity-metrics-show-an-unexpected-increase"></a>Metryki wydajność pokazywanie wzrost nieoczekiwane w wykorzystania możliwości magazynu


Jeśli widzisz przerwa, nieoczekiwane możliwości użycia na koncie miejsca do magazynowania w możesz przejrzeć przyczyny sprawdzając metryki swojej dostępności; na przykład zwiększenie liczby usuwania zakończone niepowodzeniem, które żądania może prowadzić do podwyższona magazyn obiektów blob, używanym w trakcie operacji oczyszczania określonej aplikacji, oczekiwane może mają być zwalnianie miejsca może nie działać zgodnie z oczekiwaniami (na przykład, ponieważ wygasły tokeny skojarzeń zabezpieczeń na potrzeby zwolnienia miejsca).

### <a name="you-are-experiencing-unexpected-reboots"></a>Występują nieoczekiwane ponownego uruchamiania programu Azure maszyn wirtualnych, które dużej liczby dołączony wirtualnych dysków twardych

Jeśli Azure maszyn wirtualnych (maszyn wirtualnych) zawiera dużej liczby dołączony wirtualnych dysków twardych, które znajdują się w tym samym kontem miejsca do magazynowania, może przekroczyć celów skalowalność dla konta miejsca do magazynowania poszczególnych powodujące maszyn wirtualnych kończy się niepowodzeniem. Należy sprawdzić minuta metryki dla konta przestrzeni dyskowej (**TotalRequests**/**TotalIngress**/**TotalEgress**) maksymalnej, które wykraczają poza cele skalowalność konta miejsca do magazynowania. Zobacz sekcję "[metryki Pokaż wzrost PercentThrottlingError]" pomoc w określaniu, czy ograniczania wystąpił na Twoim koncie miejsca do magazynowania.

Na ogół każdego poszczególnych wprowadzania lub wynik operacji wirtualnego dysku twardego z maszyny wirtualnej przekłada się na **Pobieranie strony** lub **Umieszczenie** symulacji źródłowych obiektów blob strony. W związku z tym szacowany operacji i/o na SEKUNDĘ w środowisku usługi umożliwia dostosowywanie wirtualnych dysków twardych ile możesz może mieć z jednego miejsca do magazynowania konta na podstawie określonych działanie aplikacji. Zaleca się o więcej niż 40 dysków z jednego miejsca do magazynowania konta. [Skalowalność magazynowania Azure i cele](storage-scalability-targets.md) szczegółowe informacje znajdują bieżącego celów skalowalność dla kont miejsca do magazynowania, w szczególności liczby całkowitej żądań i przepustowości dla rodzaju konta przestrzeni dyskowej, którego używasz.
Jeśli są przekraczają celów skalowalność dla Twojego konta miejsca do magazynowania, należy umieścić usługi wirtualnych dysków twardych w wielu kont innego miejsca do magazynowania w celu zmniejszenia aktywności w każdym danego konta.

### <a name="your-issue-arises-from-using-the-storage-emulator"></a>Problem wynika z przy użyciu emulatora miejsca do magazynowania dla projektowego lub testowego

Zwykle podczas opracowywania za pomocą emulatora miejsca do magazynowania i przetestować, aby uniknąć wymagania konto Azure miejsca do magazynowania. Typowe problemy, które mogą wystąpić podczas korzystania z emulatora przestrzeni dyskowej są:

- [Funkcja "znak X" nie działa w emulatorze miejsca do magazynowania]
- [Błąd "wartość dla jednego z nagłówków HTTP nie jest poprawny" podczas korzystania z emulatora miejsca do magazynowania]
- [Uruchamianie emulatora miejsca do magazynowania wymaga uprawnienia administracyjne]

#### <a name="feature-X-is-not-working"></a>Funkcja "znak X" nie działa w emulatorze miejsca do magazynowania

Emulator magazynowania nie obsługuje wszystkich funkcji usług Azure magazynowania, takich jak usługa plików. Aby uzyskać więcej informacji zobacz [Używanie Azure emulatora miejsca do magazynowania dla rozwoju i testowanie](storage-use-emulator.md).

Tych funkcji, które nie obsługuje emulatora miejsca do magazynowania za pomocą usługi Azure magazynu w chmurze.

#### <a name="error-HTTP-header-not-correct-format"></a>Błąd "wartość dla jednego z nagłówków HTTP nie jest poprawny" podczas korzystania z emulatora miejsca do magazynowania

Testujesz aplikacja użyć biblioteki klienta magazynowania przed emulatora magazynu lokalnego i metody połączenia, takie jak **CreateIfNotExists** kończą się niepowodzeniem z powodu błędu "wartość dla jednego z nagłówków HTTP nie jest poprawny." Oznacza to, że wersji emulatora miejsca do magazynowania, którego używasz nie obsługuje wersji Biblioteka klienta miejsca do magazynowania, którego używasz. Biblioteka klienta miejsca do magazynowania dodaje nagłówek **x ms wersji** na żądania, które ułatwia. Jeśli emulator miejsca do magazynowania nie rozpoznaje wartość w nagłówku **x-ms wersji** , odrzuci żądanie.

Dzienniki magazynowania biblioteki klienta umożliwia wyświetlenie wartości **Nagłówek x ms wersji** , którą ją wysyła. Również widać wartość **Nagłówek x ms wersji** , jeśli Fiddler umożliwia śledzenie żądania za pomocą aplikacji klienta.

W tym scenariuszu zwykle występuje, jeśli Instalowanie i używanie najnowszej wersji biblioteki klienta miejsca do magazynowania bez aktualizowania emulatora miejsca do magazynowania. Należy zainstalować najnowszą wersję emulatora miejsca do magazynowania lub Użyj magazynu w chmurze zamiast emulatorze do tworzenia i testowania.

#### <a name="storage-emulator-requires-administrative-privileges"></a>Uruchamianie emulatora miejsca do magazynowania wymaga uprawnienia administracyjne

Zostanie wyświetlony monit o poświadczenia administratora po uruchomieniu emulatora miejsca do magazynowania. Tylko dzieje się tak, gdy są inicjowania emulatora magazynowania po raz pierwszy. Po już zainicjować emulatora miejsca do magazynowania, nie trzeba uprawnień administracyjnych, uruchom go ponownie.

Aby uzyskać więcej informacji zobacz [Używanie Azure emulatora miejsca do magazynowania dla rozwoju i testowanie](storage-use-emulator.md). Należy zauważyć, że należy także zainicjować emulatora miejsca do magazynowania w programie Visual Studio, który wymaga także uprawnienia administracyjne.

### <a name="you-are-encountering-problems-installing-the-Windows-Azure-SDK"></a>Wystąpią problemy z instalacją Azure SDK dla środowiska .NET

Podczas próby instalowania zestawu SDK, nie jest on próby zainstalowania emulatora miejsca do magazynowania na komputerze lokalnym. Dziennik zawiera jedną z następujących komunikatów:

- CAQuietExec: Błąd: nie można uzyskać dostępu wystąpienie programu SQL
- CAQuietExec: Błąd: nie można utworzyć bazy danych

Przyczyną jest problem z istniejącej instalacji LocalDB. Domyślnie emulatora magazynu używa LocalDB pozostać danych, gdy go symuluje usług Azure magazynu. Możesz zresetować wystąpienia LocalDB, uruchamiając następujące polecenia w oknie wiersza polecenia przed podjęciem próby instalowania zestawu SDK.

    sqllocaldb stop v11.0
    sqllocaldb delete v11.0
    delete %USERPROFILE%\WAStorageEmulatorDb3*.*
    sqllocaldb create v11.0

Polecenie **Usuń** usuwa poprzednie instalacje emulatora miejsca do magazynowania wszystkie stare pliki bazy danych.

### <a name="you-have-a-different-issue-with-a-storage-service"></a>Masz inny problem z usługą magazynu

Jeśli poprzednich sekcjach dotyczących rozwiązywania problemów nie powinno obejmować są problemy z usługą magazynu, należy przyjąć następujące podejście do diagnozowania i rozwiązywania problemu.

- Sprawdzanie swojej wskaźniki do sprawdzenia, czy jest zmiany z Twojej oczekiwane zachowanie linii bazowej. Z metryki można określić, czy problem został przejściowych lub stałe i operacje miejsca do magazynowania ma wpływ na ten problem.
- Za pomocą informacji metryki ułatwiające wyszukiwania danych dziennika po stronie serwera uzyskać więcej szczegółowych informacji o błędach występują. Te informacje mogą ułatwić rozwiązywanie problemów z i rozwiązać ten problem.
- Jeśli informacje w dziennikach po stronie serwera nie jest wystarczające, aby rozwiązać problem pomyślnie, umożliwia dzienniki po stronie klienta Biblioteka klienta magazynowania badania zachowań aplikacji klienckiej i narzędzi, takich jak Fiddler, Wireshark i Analizator wiadomości programu Microsoft badanie sieci.

Aby uzyskać więcej informacji na temat korzystania z Fiddler, zobacz "[dodatku 1: Przechwytywanie ruchu HTTP i HTTPS za pomocą Fiddler]."

Aby uzyskać więcej informacji na temat korzystania z programu Wireshark, zobacz "[dodatek 2: Przechwytywanie ruchu w sieci za pomocą programu Wireshark]."

Aby uzyskać więcej informacji o przy użyciu analizatora wiadomości firmy Microsoft, zobacz "[dodatku 3: przy użyciu analizatora Microsoft Message Przechwyć ruch sieciowy]."

## <a name="appendices"></a>Dodatki

Dodatki opisano kilka narzędzi, które mogą być przydatne podczas diagnozowanie i rozwiązywanie problemów z magazyn Azure (i innymi usługami). Te narzędzia nie są częścią magazyn Azure a inne produkty innych firm. Jako takie narzędzia omówiony w tych załącznikach nie są objęte wszelkie umowy dotyczącej pomocy technicznej, który może być Microsoft Azure lub magazyn Azure, a więc w ramach procesu oceny należy przejrzeć licencjonowanie i opcje pomocy technicznej dostępne od dostawców tych narzędzi.

### <a name="appendix-1"></a>Dodatek 1: Przy użyciu Fiddler ruch HTTP i HTTPS

[Fiddler](http://www.telerik.com/fiddler) jest przydatne narzędzie do analizowania HTTP i HTTPS komunikacji między aplikację klienta i usługą Azure magazyn, którego używasz.

> [AZURE.NOTE] Fiddler można dekodowanie ruchu HTTPS; należy przeczytać dokumentację Fiddler starannie, aby dowiedzieć się, jak to działa i aby zrozumieć zależności zabezpieczeń.

Ten dodatek zawiera krótki Instruktaż sposobu konfigurowania Fiddler do przechwytywania ruchu między komputer lokalny, w którym zainstalowano Fiddler i usługi Azure miejsca do magazynowania.

Po uruchomieniu Fiddler rozpocznie Przechwytywanie ruchu HTTP i HTTPS na komputerze lokalnym. Poniżej przedstawiono niektóre przydatne polecenia służące do kontrolowania Fiddler:

- Zatrzymaj i uruchom Przechwytywanie ruchu. W menu głównym przejdź do **pliku** , a następnie kliknij przycisk **Przechwyć ruch** do przełączania się między Przechwytywanie Włączanie i wyłączanie funkcji.
- Zapisz przechwycony ruch danych. W menu głównym, przejdź do **pliku**, kliknij przycisk **Zapisz**, a następnie kliknij pozycję **Wszystkie sesje**: temu będzie można zapisać dane w pliku archiwum sesji. Załaduj ponownie z archiwum sesji później na potrzeby analizy lub wysyłanie go, jeśli wymagane do pomocy technicznej firmy Microsoft.

Aby ograniczyć ilość danych, które znajdują się Fiddler, można użyć filtrów, które można skonfigurować na karcie **Filtry** . Następujące zrzut ekranu przedstawia filtru, który przechwytuje tylko dane przesyłane do punktu końcowego miejsca do magazynowania **contosoemaildist.table.core.windows.net** :

![][5]

### <a name="appendix-2"></a>Dodatek 2: Za pomocą Wireshark Przechwyć ruch sieciowy

[Wireshark](http://www.wireshark.org/) jest analizatora protokołów sieciowych umożliwiający wyświetlanie pakietów szczegółowe informacje dotyczące szeroką gamę protokołów sieciowych.

Poniższa procedura pokazano, jak przechwytywanie informacji szczegółowych pakietów dla ruchu z komputera lokalnego zainstalowaną Wireshark z usługą tabeli na koncie Azure miejsca do magazynowania.

1.  Uruchom program Wireshark na komputerze lokalnym.
2.  W sekcji **Rozpocznij** wybierz interfejsu sieci lokalnej lub interfejsów, które są połączone z Internetem.
3.  Kliknij przycisk **Opcje rejestrowania**.
4.  Dodawanie filtru do pola tekstowego **Filtr przechwytywania** . Na przykład **contosoemaildist.table.core.windows.net hosta** skonfiguruje Wireshark do przechwytywania tylko pakiety wysyłane do lub z punktu końcowego usługi tabeli na koncie **contosoemaildist** miejsca do magazynowania. Zapoznaj się z [pełną listę Przechwytywanie filtrów](http://wiki.wireshark.org/CaptureFilters).

    ![][6]

5.  Kliknij przycisk **Start**. Wireshark będzie teraz przechwytywać wszystkich wysyłanych pakietów z punktu końcowego usługi tabeli lub podczas korzystania z aplikacji klienckiej na komputerze lokalnym.
6.  Gdy skończysz, w menu głównym kliknij **przechwycić** , a następnie **Zatrzymaj**.
7.  Aby zapisać przechwycone dane w programie Wireshark Przechwytywanie pliku, w menu głównym kliknij **plik** , a następnie **Zapisz**.

WireShark wyróżni wszystkie błędy, które znajdują się w oknie **packetlist** . Można też użyć okna **Informacje eksperta** (kliknij pozycję **Analiza**, a następnie **Informacje eksperta**) aby wyświetlić podsumowanie błędów i ostrzeżeń.

![][7]

Możesz również wybrać do wyświetlania danych TCP jako warstwy aplikacji widzi ją prawym przyciskiem myszy dane TCP i wybierając, **Wykonaj strumienia TCP**. To jest szczególnie przydatne, jeśli przechwycone usługi zrzutu bez filtru przechwytywania. Aby uzyskać więcej informacji, zobacz [po strumienie TCP] (http://www.wireshark.org/docs/wsug_html_chunked/ChAdvFollowTCPSection.html).

![][8]

> [AZURE.NOTE] Aby uzyskać więcej informacji na temat korzystania z programu Wireshark zobacz [Przewodnik użytkownika Wireshark](http://www.wireshark.org/docs/wsug_html_chunked).

### <a name="appendix-3"></a>Dodatek 3: Przy użyciu analizatora wiadomości Microsoft Przechwyć ruch sieciowy

Przechwytywanie ruchu HTTP i HTTPS w podobny sposób jak Fiddler za pomocą analizatora wiadomości firmy Microsoft i Przechwytywanie ruchu sieciowego w podobny sposób jak Wireshark.

#### <a name="configure-a-web-tracing-session-using-microsoft-message-analyzer"></a>Konfigurowanie sesji śledzenia sieci web przy użyciu analizatora wiadomości firmy Microsoft

Aby skonfigurować sesję śledzenia sieci web ruchu HTTP i HTTPS przy użyciu analizatora wiadomości firmy Microsoft, uruchom aplikację Microsoft Analyzer wiadomości, a następnie w menu **plik** kliknij **Przechwytywania i śledzenia**. Na liście dostępne śledzenia scenariusze wybierz **Serwer Proxy sieci Web**. Następnie w panelu **Konfiguracji scenariusz śledzenia** w polu tekstowym **HostnameFilter** Dodaj nazwy punktów końcowych miejsca do magazynowania (można wyszukiwać te nazwy w [Azure Portal](https://portal.azure.com)). Na przykład nazwę konta magazynu platformy Azure jest **contosodata**, należy dodać następujące do pola tekstowego **HostnameFilter** :

    contosodata.blob.core.windows.net contosodata.table.core.windows.net contosodata.queue.core.windows.net

> [AZURE.NOTE] Znak spacji rozdzielający nazwy hostów.

Gdy rozpocząć zbieranie danych śledzenia, kliknij przycisk **Start z** .

Aby uzyskać więcej informacji na temat śledzenia analizatora wiadomości Microsoft **Proxy usług sieci Web** zobacz [Microsoft-PEF-WebProxy dostawcy](http://technet.microsoft.com/library/jj674814.aspx).

Wbudowane śledzenia **Serwer Proxy sieci Web** w programie Microsoft Analyzer wiadomości jest oparty na Fiddler; można go przechwycić ruchu HTTPS po stronie klienta i wyświetlić niezaszyfrowanym wiadomości HTTPS. Śledzenie **Serwer Proxy sieci Web** polega na konfigurowanie lokalnego serwera proxy dla całego ruchu HTTP i HTTPS mu dostęp do wiadomości niezaszyfrowanym.

#### <a name="diagnosing-network-issues-using-microsoft-message-analyzer"></a>Diagnozowanie problemów z siecią za pomocą analizatora wiadomości firmy Microsoft

Oprócz rejestrowania szczegółów ruchu protokołu HTTP/HTTPs między aplikacją kliencką a usługą Magazyn za pomocą śledzenia analizatora wiadomości Microsoft **Proxy usług sieci Web** , umożliwia także wbudowane śledzenia **Lokalne warstwy łącza** do przechwytywania informacji dotyczących pakietu sieci. Ta umożliwia przechwytywanie danych podobne do tych, które można przechwycić z Wireshark i diagnozowanie sieci problemy takie jak porzucone pakiety.

Następujące zrzut ekranu przedstawia przykładową śledzenia **Lokalne warstwy łącza** z **niektóre komunikaty** w kolumnie **DiagnosisTypes** . Klikając ikonę w kolumnie **DiagnosisTypes** zawiera szczegółowe informacje o komunikacie. W tym przykładzie serwer ponownie wysłane wiadomości #305 ponieważ nie dotarła potwierdzenie od klienta:

![][9]

Po utworzeniu sesji śledzenia w Microsoft Analyzer wiadomości można określić filtrów w celu zmniejszenia ilości hałasu w wynikach śledzenia. Na stronie **Przechwytywanie / śledzenia** , gdzie definiowanie śledzenia kliknij łącze **Konfiguruj** obok pozycji **Microsoft-Windows-NDIS-PacketCapture**. Następujące zrzut ekranu przedstawia konfiguracji, która filtruje ruch TCP dla adresów IP trzy usługi miejsca do magazynowania:

![][10]

Aby uzyskać więcej informacji na temat śledzenia Microsoft wiadomości analizatora lokalne warstwy łącza zobacz [Dostawcy firmy Microsoft — PEF-NDIS-PacketCapture](http://technet.microsoft.com/library/jj659264.aspx).

### <a name="appendix-4"></a>Dodatek 4: Za pomocą programu Excel, aby wyświetlić metryki i rejestrowanie danych

Wiele narzędzi umożliwiają pobieranie danych metryki magazynowania z magazynem tabel platformy Azure w formacie rozdzielanego, który ułatwia załadować dane do programu Excel w celu przeglądania i analizy. Miejsca do magazynowania rejestrowanie danych z magazynem obiektów blob Azure jest już rozdzielany formatu, który można załadować do programu Excel. Należy dodać odpowiednie nagłówki kolumn na informacji na [Format dziennika analizy miejsca do magazynowania](http://msdn.microsoft.com/library/azure/hh343259.aspx) i [Schematu tabeli metryk analizy miejsca do magazynowania](http://msdn.microsoft.com/library/azure/hh343264.aspx).

Aby zaimportować rejestrowania miejsca do magazynowania danych do programu Excel po pobraniu z magazynem obiektów blob:

- W menu **dane** kliknij pozycję **Z tekstu**.
- Przejdź do pliku dziennika, który chcesz wyświetlić, a następnie kliknij przycisk **Importuj**.
- W kroku 1 **Kreatora importu tekstu**wybierz pozycję **rozdzielany**.

W kroku 1 **Kreatora importu tekstu**, wybierz pozycję **średnik** jako tylko ogranicznika i wybierz pozycję podwójnego cudzysłowu jako **Kwalifikator tekstu**. Następnie kliknij przycisk **Zakończ** i wybierz lokalizację umieszczenia danych w skoroszycie.

### <a name="appendix-5"></a>Dodatek 5: Monitorowanie za pomocą wniosków aplikacji dla programu Visual Studio Team Services

Umożliwia także funkcję wniosków aplikacji dla programu Visual Studio Team Services jako część wydajności i monitorowania dostępności. To narzędzie wykonywać następujące czynności:

- Upewnij się, że usługa sieci web jest dostępny i odpowiada. Czy aplikacja jest witryną sieci web lub aplikacji dla urządzeń, która korzysta z usługi sieci web, je przetestować adresu URL co kilka minut z lokalizacji na całym świecie, a informujące, jeśli występuje problem.
- Szybkie diagnozowanie problemów z wydajnością ani wyjątki w danej usługi sieci web. Dowiedz się, jeśli są rozciągnięta Procesora lub inne zasoby, możesz uzyskać śledzenia stosu od wyjątki i łatwe wyszukiwanie za pomocą śledzenia dziennika. Jeśli wydajności aplikacji jest mniejsza niż dopuszczalne wartości graniczne, firma Microsoft mogą wysyłać wiadomości e-mail. Można monitorować zarówno .NET i Java usług sieci web.

Można znaleźć więcej informacji na [Co to jest aplikacja wniosków?](../application-insights/app-insights-overview.md).

<!--Anchors-->
[Wprowadzenie]: #introduction
[Przegląd organizacji elementów tego przewodnika]: #how-this-guide-is-organized

[Monitorowanie usługi miejsca do magazynowania]: #monitoring-your-storage-service
[Monitorowanie kondycji usługi]: #monitoring-service-health
[Zdolności monitorowania]: #monitoring-capacity
[Dostępność monitorowania]: #monitoring-availability
[Monitorowanie wydajności]: #monitoring-performance

[Diagnozowanie problemów związanych z miejsca do magazynowania]: #diagnosing-storage-issues
[Problemy dotyczące kondycji usługi]: #service-health-issues
[Problemy z wydajnością]: #performance-issues
[Diagnozowanie problemów]: #diagnosing-errors
[Problemy emulatora miejsca do magazynowania]: #storage-emulator-issues
[Narzędzia rejestrowania miejsca do magazynowania]: #storage-logging-tools
[Za pomocą narzędzia rejestrowania sieci]: #using-network-logging-tools

[Śledzenie end-to-end]: #end-to-end-tracing
[Korelacji danych dziennika]: #correlating-log-data
[Identyfikator klienta]: #client-request-id
[Identyfikator żądania serwera]: #server-request-id
[Sygnatury czasowe]: #timestamps

[Wskazówki dotyczące rozwiązywania problemów]: #troubleshooting-guidance
[Metryki Pokaż AverageE2ELatency wysokiej i niskiej AverageServerLatency]: #metrics-show-high-AverageE2ELatency-and-low-AverageServerLatency
[Metryki Pokaż niskim AverageE2ELatency i niskiej AverageServerLatency, ale klient występuje duże opóźnienia]: #metrics-show-low-AverageE2ELatency-and-low-AverageServerLatency
[Metryki Pokaż wysoka AverageServerLatency]: #metrics-show-high-AverageServerLatency
[Występują nieoczekiwane opóźnień dostarczaniem wiadomości w kolejce]: #you-are-experiencing-unexpected-delays-in-message-delivery

[Metryki pokazywanie wzrost w PercentThrottlingError]: #metrics-show-an-increase-in-PercentThrottlingError
[Przejściowych wzrost PercentThrottlingError]: #transient-increase-in-PercentThrottlingError
[Trwały wzrost PercentThrottlingError błędu]: #permanent-increase-in-PercentThrottlingError
[Metryki pokazywanie wzrost w PercentTimeoutError]: #metrics-show-an-increase-in-PercentTimeoutError
[Metryki pokazywanie wzrost w PercentNetworkError]: #metrics-show-an-increase-in-PercentNetworkError

[Klient otrzymuje wiadomości HTTP 403 (Dostęp zabroniony)]: #the-client-is-receiving-403-messages
[Klient otrzymuje wiadomości HTTP 404 (nie można odnaleźć)]: #the-client-is-receiving-404-messages
[Klient lub inny proces poprzednio usunięte obiektu]: #client-previously-deleted-the-object
[Problem autoryzacji podpisu udostępnionych programu Access (SA)]: #SAS-authorization-issue
[Kod JavaScript po stronie klienta nie ma uprawnień dostępu do obiektu]: #JavaScript-code-does-not-have-permission
[Błąd sieci]: #network-failure
[Klient otrzymuje wiadomości HTTP 409 (konflikt)]: #the-client-is-receiving-409-messages

[Metryki pokazywanie niskim PercentSuccess i pozycje dziennika analizy mają operacje ze stanem transakcji ClientOtherErrors]: #metrics-show-low-percent-success
[Metryki wydajność pokazywanie wzrost nieoczekiwane w wykorzystania możliwości magazynu]: #capacity-metrics-show-an-unexpected-increase
[Występują nieoczekiwane ponownego uruchamiania maszyn wirtualnych, mające dużej liczby dołączony wirtualnych dysków twardych]: #you-are-experiencing-unexpected-reboots
[Problem wynika z przy użyciu emulatora miejsca do magazynowania dla projektowego lub testowego]: #your-issue-arises-from-using-the-storage-emulator
[Funkcja "znak X" nie działa w emulatorze miejsca do magazynowania]: #feature-X-is-not-working
[Błąd "wartość dla jednego z nagłówków HTTP nie jest poprawny" podczas korzystania z emulatora miejsca do magazynowania]: #error-HTTP-header-not-correct-format
[Uruchamianie emulatora miejsca do magazynowania wymaga uprawnienia administracyjne]: #storage-emulator-requires-administrative-privileges
[Wystąpią problemy z instalacją Azure SDK dla środowiska .NET]: #you-are-encountering-problems-installing-the-Windows-Azure-SDK
[Masz inny problem z usługą magazynu]: #you-have-a-different-issue-with-a-storage-service

[Dodatki]: #appendices
[Dodatek 1: Przy użyciu Fiddler ruch HTTP i HTTPS]: #appendix-1
[Dodatek 2: Za pomocą Wireshark Przechwyć ruch sieciowy]: #appendix-2
[Dodatek 3: Przy użyciu analizatora wiadomości Microsoft Przechwyć ruch sieciowy]: #appendix-3
[Dodatek 4: Za pomocą programu Excel, aby wyświetlić metryki i rejestrowanie danych]: #appendix-4
[Dodatek 5: Monitorowanie za pomocą wniosków aplikacji dla programu Visual Studio Team Services]: #appendix-5

<!--Image references-->
[1]: ./media/storage-monitoring-diagnosing-troubleshooting/overview.png
[3]: ./media/storage-monitoring-diagnosing-troubleshooting/hour-minute-metrics.png
[4]: ./media/storage-monitoring-diagnosing-troubleshooting/high-e2e-latency.png
[5]: ./media/storage-monitoring-diagnosing-troubleshooting/fiddler-screenshot.png
[6]: ./media/storage-monitoring-diagnosing-troubleshooting/wireshark-screenshot-1.png
[7]: ./media/storage-monitoring-diagnosing-troubleshooting/wireshark-screenshot-2.png
[8]: ./media/storage-monitoring-diagnosing-troubleshooting/wireshark-screenshot-3.png
[9]: ./media/storage-monitoring-diagnosing-troubleshooting/mma-screenshot-1.png
[10]: ./media/storage-monitoring-diagnosing-troubleshooting/mma-screenshot-2.png
