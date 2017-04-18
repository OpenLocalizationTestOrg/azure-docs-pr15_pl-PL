<properties
    pageTitle="Wydajność rozwiązanie do zarządzania w dzienniku analizy | Microsoft Azure"
    description="Umożliwiają rozwiązanie planowania pojemności w dzienniku analizy ułatwiające zrozumienie możliwości serwery funkcji Hyper-V zarządzane przez Menedżera maszyn wirtualnych systemu Centrum"
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>

# <a name="capacity-management-solution-in-log-analytics"></a>Wydajność rozwiązanie do zarządzania w analizy dziennika


Za pomocą rozwiązanie planowania pojemności analizy dziennika ułatwiające zrozumienie możliwości zarządzane przez Menedżera maszyn wirtualnych systemu Centrum serwerów funkcji Hyper-V. To rozwiązanie wymaga zarówno System Center Operations Manager i System Center Virtual Machine Manager. Planowanie pojemności nie jest dostępna, jeśli używasz wyłącznie bezpośrednio połączone agentów. Zainstaluj rozwiązanie, aby zaktualizować agenta programu Operations Manager. Rozwiązanie odczytuje liczniki wydajności na serwerze monitorowania i wysyła danych dotyczących użycia usługę w chmurze dla przetwarzania. Logika zostanie zastosowany do danych dotyczących użycia i usług w chmurze rekordów danych. W czasie są oznaczone upodobania i planowanego wydajność, oparte na bieżące zużycie.

Na przykład rzut może zdefiniować dodatkowe rdzenie lub dodatkowej pamięci będą potrzebne dla poszczególnych serwerów. W tym przykładzie rzut może oznaczać, że 30 dni serwera muszą dodatkowej pamięci. To może ułatwić planowanie pamięć podczas następnego okna konserwacji serwera, które mogą wystąpić co dwa tygodnie.

>[AZURE.NOTE] Rozwiązanie do zarządzania pojemnością nie jest dostępna do dodania do obszarów roboczych. Klienci, którzy mają zainstalowany rozwiązanie do zarządzania pojemnością nadal korzystać rozwiązanie.  

Możliwości rozwiązanie do planowania imprez trwa aktualizowany aby adres klienta następujące zgłoszone problemów:

- Wymaganie za pomocą Menedżera maszyn wirtualnych i programu Operations Manager
- Niemożność Dostosowywanie filtru na podstawie grup
- Co godzinę agregacji danych nie wystarczająco częste
- Nie wniosków poziomu maszyn wirtualnych
- Wiarygodność danych

Zalety nowego rozwiązania możliwości:

- Obsługiwać zbierania danych szczegółowego większa niezawodność i dokładność
- Obsługa funkcji Hyper-V bez konieczności VMM
- Wizualizacja metryki w PowerBI
- Więcej informacji na poziomie wykorzystania maszyn wirtualnych


## <a name="installing-and-configuring-the-solution"></a>Instalowanie i konfigurowanie rozwiązanie
Aby zainstalować i skonfigurować rozwiązanie, korzystając z następujących informacji.

- Programu Operations Manager jest wymagana rozwiązanie do zarządzania pojemnością.
- Menedżer maszyn wirtualnych jest wymagane na potrzeby rozwiązanie do zarządzania pojemnością.
- Operacje Menedżera łączność z Virtual Machine Manager (VMM) jest wymagana. Aby uzyskać dodatkowe informacje o nawiązywaniu połączeń z systemami zobacz [jak połączyć VMM z programu Operations Manager](http://technet.microsoft.com/library/hh882396.aspx).
- Kierownik musi być połączony z analizy dziennika.
- Dodaj rozwiązanie do zarządzania zdolności do obszaru roboczego usługi OMS przy użyciu procesu opisanego w [rozwiązań Dodawanie analizy dziennika z galerii rozwiązań](log-analytics-add-solutions.md).  Nie jest wymagana żadna konfiguracja dalsze.


## <a name="capacity-management-data-collection-details"></a>Wydajność szczegóły zbioru danych zarządzania

Zarządzanie pojemności zbiera dane dotyczące wydajności, metadanych i dane o stanie za pomocą czynników, które są włączone.

W poniższej tabeli przedstawiono metody zbioru danych i inne szczegóły dotyczące jak dane są zbierane dla zarządzanie wydajnością.

| platformy | Bezpośrednie agenta | SCOM agent | Azure miejsca do magazynowania | SCOM wymagane? | Dane agenta SCOM wysyłane za pośrednictwem grupy zarządzania | częstotliwość pobierania |
|---|---|---|---|---|---|---|
|Systemu Windows|![Brak](./media/log-analytics-capacity/oms-bullet-red.png)|![Tak](./media/log-analytics-capacity/oms-bullet-green.png)|![Brak](./media/log-analytics-capacity/oms-bullet-red.png)|            ![Tak](./media/log-analytics-capacity/oms-bullet-green.png)|![Tak](./media/log-analytics-capacity/oms-bullet-green.png)| co godzina|

W poniższej tabeli pokazano przykłady typów danych zebranych przez zarządzanie wydajnością:

|**Typ danych**|**Pola**|
|---|---|
|Metadane|BaseManagedEntityId, ObjectStatus, jednostka organizacyjna, ActiveDirectoryObjectSid, PhysicalProcessors, NazwaSieci, adres IP, ForestDNSName, NetbiosComputerName, VirtualMachineName, LastInventoryDate, HostServerNameIsVirtualMachine, adres IP, NetbiosDomainName, LogicalProcessors, Nazwa_dns, DisplayName, DomainDnsName, ActiveDirectorySite, PrincipalName, OffsetInMinuteFromGreenwichTime|
|Wydajność|Nazwa obiektu, CounterName, PerfmonInstanceName, PerformanceDataId, PerformanceSourceInternalID, SampleValue, TimeSampled, TimeAdded|
|Województwo|StateChangeEventId, StateId, NewHealthState, OldHealthState, kontekst, TimeGenerated, TimeAdded, StateId2, BaseManagedEntityId, MonitorId, HealthState, Ostatnia modyfikacja, LastGreenAlertGenerated, DatabaseTimeModified|

## <a name="capacity-management-page"></a>Strona zarządzania wydajności


 Po planowaniu rozwiązanie jest zainstalowany, możesz wyświetlić możliwości monitorowane serwerów przy użyciu kafelka **Planowania pojemności** na stronie **Omówienie** w usługi OMS.

![Obraz kafelka planowania pojemności](./media/log-analytics-capacity/oms-capacity01.png)

Kafelek zostanie wyświetlona na pulpicie nawigacyjnym **Zarządzanie wydajnością** miejsce, w którym można wyświetlić podsumowanie wydajność serwera. Na stronie są wyświetlane następujące segmenty, które można kliknąć:

- *Liczba maszyn wirtualnych*: Pokazuje liczbę dni pozostałych dla możliwości maszyn wirtualnych
- *Obliczanie*: zawiera rdzenie procesora i pamięci
- *Miejsca do magazynowania*: przedstawiono użycie miejsca na dysku i Średni czas oczekiwania na dysku
- *Wyszukiwanie*: Eksplorator danych, który służy do wyszukiwania danych w systemie usługi OMS

![Obraz pulpitu nawigacyjnego zarządzanie wydajnością](./media/log-analytics-capacity/oms-capacity02.png)


### <a name="to-view-a-capacity-page"></a>Aby wyświetlić stronę wydajność

- Na stronie **Przegląd** kliknij pozycję **Zarządzanie wydajnością**, a następnie kliknij **obliczyć** lub **miejsce do magazynowania**.

## <a name="compute-page"></a>Obliczanie strony

Pulpit nawigacyjny **obliczenia** w usługi Microsoft Azure OMS służy do wyświetlania zdolności informacji na temat wykorzystania planowanego dni pojemności i efektywności związane z infrastruktury. Obszar **wykorzystania** umożliwia wyświetlanie procesora core i pamięci w hosty maszyn wirtualnych. Narzędzie rzut do oszacowania, ile zdolności ma być dostępna dla podanego zakresu dat.. Obszar **wydajności** umożliwia Zobacz, jak efektywne są hosty maszyn wirtualnych. Możesz wyświetlić szczegółowe informacje o połączonych elementów, klikając je.

Istnieje możliwość wygenerowania skoroszytu programu Excel dla następujących kategorii:

- Górny hosts z najwyższą wykorzystania core
- Górny hosts z najwyższą wykorzystanie pamięci
- Górny hosts nieefektywne maszyn wirtualnych
- Górny hosts przez wykorzystanie
- Hosts dołu przez wykorzystanie

![Obraz strony obliczyć zarządzania wydajność](./media/log-analytics-capacity/oms-capacity03.png)


Następujące obszary są wyświetlane na pulpicie nawigacyjnym **obliczyć** :

**Wykorzystanie**: widok Procesora core i pamięci wykorzystania w hosty maszyn wirtualnych.

- *Używane rdzenie*: Suma dla wszystkich hostów (% wykorzystania procesora pomnożoną przez liczbę fizycznie rdzenie na hoście).
- *Bezpłatne rdzenie*: Suma fizycznie rdzenie minus rdzenie używanych.
- *Dostępne rdzenie wartości procentowej*: wolny fizycznie rdzenie podzielona przez całkowitą liczbę fizycznie rdzenie.
- *Wirtualna rdzeni w jednym maszyn wirtualnych*: Suma rdzenie wirtualnej w systemie podzielona przez całkowitą liczbę maszyn wirtualnych systemu.
- *Wirtualna rdzenie do fizycznie współczynnika rdzenie*: stosunek Całkowita fizycznie rdzeni do fizycznie rdzenie, które są używane przez maszyn wirtualnych systemu.
- *Liczba dostępnych rdzenie wirtualna*: wirtualna core do współczynnika fizycznie rdzenie pomnożoną przez dostępne rdzenie fizycznie.
- *Używane pamięci*: suma pamięci, która jest wykorzystywane przez wszystkie hosty.
- *Pamięci*: suma pamięci fizycznej minus użycie pamięci.
- *Procent pamięci dostępne*: zwolnienia pamięci fizycznej podzielona przez pamięć fizyczna.
- *Pamięć wirtualna na maszyn wirtualnych*: pamięć wirtualna w systemie podzielona przez całkowitą liczbę maszyn wirtualnych systemu.
- *Pamięć wirtualna do fizycznie współczynnika pamięci*: pamięć wirtualna w systemie podzielona przez pamięć fizyczna w systemie.
- *Dostępna pamięć wirtualna*: pamięć wirtualna do współczynnika pamięci fizycznej pomnożoną przez dostępną pamięć fizycznym.

**Narzędzie rzutu**

Za pomocą narzędzia rzutowania dla swojego wykorzystania zasobów można wyświetlać trendów w historii. Ta opcja uwzględnia trendów zastosowania dla maszyn wirtualnych, pamięci podstawowych i miejsca do magazynowania. Funkcja rzut używa algorytmu rzut ułatwiające wiedzieć, kiedy kończy się każdy z zasobów. Pomaga to obliczanie pisane z wielkiej litery zdolności planowania, aby można wiedzieli, gdy trzeba kupić więcej możliwości (na przykład pamięci, rdzenie lub miejsca do magazynowania).

**Wydajność**

- *Idle maszyn wirtualnych*: przy użyciu mniej niż 10% pamięci Procesora i 10% dla określonego okresu.
- *Overutilized maszyn wirtualnych*: za pomocą więcej niż 90% pamięci Procesora i 90% dla określonego okresu.
- *Idle hosta*: przy użyciu mniej niż 10% pamięci Procesora i 10% dla określonego okresu.
- *Overutilized hosta*: za pomocą więcej niż 90% pamięci Procesora i 90% dla określonego okresu.

### <a name="to-work-with-items-on-the-compute-page"></a>Praca z elementami na stronie obliczeń

1. Na pulpicie **obliczenia** w obszarze **wykorzystania** wyświetlanie informacji pojemności o rdzenie Procesora i pamięci.
2. Kliknij element, aby otworzyć go na stronie **wyszukiwania** i wyświetlić szczegółowe informacje o tym.
3. W narzędziu **rzut** Przesuń suwak daty, aby wyświetlić rzut możliwości, który będzie używany w wybranym dniu.
4. W obszarze **Wydajność** wyświetlanie zdolności efektywności informacji o maszyn wirtualnych i hosts maszyn wirtualnych.

## <a name="direct-attached-storage-page"></a>Bezpośrednie strony masowa

Pulpit nawigacyjny **Bezpośredni masowa** w usługi OMS służy do wyświetlania zdolności informacji o wykorzystanie miejsca do magazynowania, wydajność dysku i przewidywane dni pojemności dysku. Obszar **wykorzystania** umożliwia wyświetlanie użycie miejsca na dysku w hosty maszyn wirtualnych. Obszar **Wydajności dysku** umożliwia wyświetlanie dysku przepustowość i opóźnienie w hosty maszyn wirtualnych. Za pomocą narzędzia rzut do oszacowania, ile zdolności ma być dostępna dla podanego zakresu dat.. Możesz wyświetlić szczegółowe informacje o połączonych elementów, klikając je.

Istnieje możliwość wygenerowania skoroszytu programu Excel z tych informacji wydajność dla następujących kategorii:

- Użycie miejsca na dysku górny przez hosta
- Górny Średni czas oczekiwania przez hosta

Na stronie **Magazyn** są wyświetlane następujące zagadnienia:

- *Wykorzystanie*: wyświetlanie użycie miejsca na dysku w hosty maszyn wirtualnych.
- *Całkowita ilość miejsca na dysku*: Suma (miejsca na dysku logicznego) dla wszystkich hostów
- *Używane miejsca na dysku*: Suma (zajętego logiczne miejsca na dysku) dla wszystkich hostów
- *Dostępnego miejsca na dysku*: całkowitego miejsca na dysku minus zajętego miejsca na dysku
- *Wartość procentową dysk używany*: używane miejsca na dysku podzielona przez całkowitą ilość miejsca na dysku
- *Dostępne dysku wartości procentowej*: wolnego miejsca na dysku podzielona przez całkowitą ilość miejsca na dysku

![Obraz strony zarządzania bezpośredni dołączone pojemności](./media/log-analytics-capacity/oms-capacity04.png)

**Wydajność dysku**

Za pomocą usługi OMS, możesz wyświetlić trendy historycznych użycie miejsca na dysku. Funkcja rzut używa algorytmu do zastosowania przyszłych projektów. Dla wykorzystanie miejsca w szczególności, funkcja rzut umożliwia projektu, gdy może brakować miejsca na dysku. Dzięki temu planowanie pisane z wielkiej litery miejsca do magazynowania i dowiedzieć się, gdy chcesz kupić więcej licencji.

**Narzędzie rzut**

Za pomocą narzędzia rzutowania dla wykorzystanie miejsca na dysku można wyświetlać trendów w historii. Funkcja rzut pozwala także na projektu, gdy są zaczyna brakować miejsca na dysku. Dzięki temu Planowanie pojemności pisane z wielkiej litery i dowiedzieć się, gdy chcesz kupić więcej pojemności.

### <a name="to-work-with-items-on-the-direct-attached-storage-page"></a>Praca z elementami na stronie bezpośredni masowa

1. Na pulpicie nawigacyjnym **Bezpośredni masowa** w obszarze **wykorzystania** możesz wyświetlać informacje dotyczące wykorzystania dysku.
2. Kliknij pozycję połączony element, aby otworzyć go na stronie **wyszukiwania** i wyświetlić szczegółowe informacje o tym.
3. W obszarze **Wydajność dysku** możesz wyświetlać informacji o przepustowość i opóźnienie dysku.
4. W **narzędziu rzut**Przesuń suwak daty, aby wyświetlić rzut możliwości, który będzie używany w wybranym dniu.


## <a name="next-steps"></a>Następne kroki

- Umożliwia wyświetlanie szczegółowe dane zarządzania [wyszukiwania dziennika analizy dziennika](log-analytics-log-searches.md) .
