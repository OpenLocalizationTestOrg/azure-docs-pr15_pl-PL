<properties
    pageTitle="Aktualizowanie rozwiązania do zarządzania w usługi OMS | Microsoft Azure"
    description="Ten artykuł jest przeznaczony ułatwiające zrozumienie sposobu używania tego rozwiązania do zarządzania aktualizacjami dla komputerów Windows i Linux."
    services="operations-management-suite"
    documentationCenter=""
    authors="MGoedtel"
    manager="jwhit"
    editor=""
    />
<tags
    ms.service="operations-management-suite"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/14/2016"
    ms.author="magoedte"/>

# <a name="update-management-solution-in-omsmediaoms-solution-update-managementupdate-management-solution-iconpng-update-management-solution-in-oms"></a>![Aktualizowanie rozwiązania do zarządzania w usługi OMS](./media/oms-solution-update-management/update-management-solution-icon.png) Aktualizowanie rozwiązania do zarządzania w usługi OMS

Rozwiązania zarządzania aktualizacjami w programie usługi OMS umożliwia zarządzanie aktualizacjami dla komputerów Windows i Linux.  Możesz szybko ocenić stan dostępne aktualizacje na wszystkich komputerach agenta i inicjuje proces instalacji wymagane aktualizacje dla serwerów. 

## <a name="prerequisites"></a>Wymagania wstępne

-   Agenci Windows albo musi być skonfigurowany do komunikowania się z serwerem Windows Server Update Services (WSUS) lub mieć dostęp do witryny Microsoft Update.  

    >[AZURE.NOTE] Agent systemu Windows nie można zarządzać jednocześnie przez Menedżera konfiguracji centrum systemu.  
  
-   Czynniki Linux musi mieć dostęp do repozytorium aktualizacji.  Agent usługi OMS Linux można pobrać z [GitHub](https://github.com/microsoft/oms-agent-for-linux). 

## <a name="configuration"></a>Konfiguracja

Wykonaj poniższe czynności, aby dodać rozwiązanie do zarządzania aktualizacji do obszaru roboczego usługi OMS i dodać agentów Linux.  Czynniki systemu Windows są automatycznie dodawane żadne dodatkowe czynności konfiguracyjne.

1.  Dodaj rozwiązanie do zarządzania aktualizacji do obszaru roboczego usługi OMS przy użyciu procesu opisanego w [Dodawanie usługi OMS rozwiązań](../log-analytics/log-analytics-add-solutions.md) z galerii rozwiązań.  
2.  W portalu usługi OMS wybierz **Ustawienia** , a następnie **Połączonych źródeł**.  Zanotuj **Identyfikator obszaru roboczego** i **klucza podstawowego** lub **klucza pomocniczego**.
3.  Na każdym komputerze Linux, należy wykonać następujące czynności.

    .  Zainstaluj najnowszą wersję programu Agent usługi OMS Linux, uruchamiając następujące polecenia.  Zamienianie <Workspace ID> z Identyfikatorem obszaru roboczego i <Key> z kluczem podstawowym lub pomocniczym.

        cd ~
        wget https://github.com/Microsoft/OMS-Agent-for-Linux/releases/download/v1.2.0-75/omsagent-1.2.0-75.universal.x64.sh
        sudo bash omsagent-1.2.0-75.universal.x64.sh --upgrade -w <Workspace ID> -s <Key>

     b. Aby usunąć agenta, uruchom następujące polecenie.

        sudo bash omsagent-1.2.0-75.universal.x64.sh --purge

## <a name="management-packs"></a>Pakiety zarządzania

Grupa zarządzania programu System Center Operations Manager jest podłączone do obszaru roboczego usługi OMS, następnie następujące pakiety zarządzania zostanie zainstalowany w programie Operations Manager po dodaniu tego rozwiązania. Istnieje konfiguracji lub konserwacji te pakiety narzędzi zarządzania wymagane. 

-   Program Microsoft System Center Advisor Update ocena analizy dodatkiem Service Pack (Microsoft.IntelligencePacks.UpdateAssessment)
-   Microsoft.IntelligencePack.UpdateAssessment.Configuration (Microsoft.IntelligencePack.UpdateAssessment.Configuration)
-   Aktualizowanie wdrożenia CR

Aby uzyskać więcej informacji na sposób aktualizowania pakietów zarządzania rozwiązanie zobacz [Łączenie analizy dziennika programu Operations Manager](../log-analytics/log-analytics-om-agents.md).

## <a name="data-collection"></a>Zbieranie danych

### <a name="supported-agents"></a>Obsługiwane agentów

W poniższej tabeli opisano połączonych źródeł, które są obsługiwane przy użyciu tego rozwiązania.

Połączone źródła | Obsługiwane | Opis|
----------|----------|----------|
Czynniki systemu Windows | Tak | Rozwiązanie gromadzi informacje o aktualizacji systemu Windows czynnikami i inicjuje instalacji wymagane aktualizacje.|
Czynniki Linux | Tak | Rozwiązanie gromadzi informacje o aktualizacji systemu czynnikami Linux.|
Grupa Zarządzanie menedżera operacji | Tak | Rozwiązanie gromadzi informacje o aktualizacji systemu czynnikami w grupie Zarządzanie połączonego.<br>Bezpośrednie połączenie z agentem programu Operations Manager analizy dziennika nie jest wymagane. Dane są przesyłane w grupie Zarządzanie do repozytorium usługi OMS.|
Konto Azure miejsca do magazynowania | Brak | Nie zawiera informacji na temat aktualizacji systemu Azure miejsca do magazynowania.|  

### <a name="collection-frequency"></a>Częstotliwość pobierania

Dla każdego zarządzanych komputera systemu Windows skanowanie jest wykonywane dwa razy dziennie.  Po zainstalowaniu aktualizacji jego informacje są aktualizowane w ciągu 15 minut.  

Dla każdego zarządzanych komputera Linux skanowanie jest przeprowadzane co 3 godziny.  

## <a name="using-the-solution"></a>Za pomocą rozwiązanie

Po dodaniu rozwiązania do zarządzania aktualizacji do obszaru roboczego usługi OMS kafelków **Zarządzania aktualizacjami** zostanie dodana do pulpitu nawigacyjnego usługi OMS. Tym kafelku jest wyświetlana liczba i graficzna reprezentacja liczby komputerów w środowisku obecnie wymagających aktualizacji systemu.<br><br>
![Aktualizowanie zarządzania podsumowanie kafelków](media/oms-solution-update-management/update-management-summary-tile.png)  

## <a name="viewing-update-assessments"></a>Wyświetlanie aktualizacji ocen

Kliknij Kafelek **Zarządzania aktualizacjami** , aby otworzyć pulpit nawigacyjny **Zarządzania aktualizacji** . Pulpit nawigacyjny zawiera kolumny w tabeli poniżej. Każda kolumna zawiera listę do dziesięciu elementów spełniających kryteria tej kolumny dla określonego zakresu i przedział czasu. Można przeprowadzić wyszukiwanie dziennika, która zwraca wszystkie rekordy, klikając u dołu kolumny **wyświetlić wszystkie** lub klikając nagłówek kolumny.

Kolumny | Opis|
----------|----------|
**Komputery brakujących aktualizacji** ||
Krytyczne lub aktualizacje zabezpieczeń | List, które aktualizacje dziesięć komputerów, które nie są widoczne u góry posortowane według liczba aktualizacji, są one widoczne. Kliknij nazwę komputera, aby uruchomić wyszukiwanie dziennika zwracanie wszystkich aktualizacji rekordów dla tego komputera.|
Krytyczne lub aktualizacje zabezpieczeń starsze niż 30 dni| Określa liczbę komputerów, które są pogrupowane według określonego czasu, po podpisaniu aktualizacji aktualizacji zabezpieczeń lub brak krytyczne. Kliknij jedną z pozycji, aby uruchomić wyszukiwanie dziennika zwracanie wszystkie brakujące i krytyczne aktualizacje.|
**Wymagane aktualizacje brakuje**||
Krytyczne lub aktualizacje zabezpieczeń | Lista klasyfikacji aktualizacje, że komputery brakuje posortowane według liczby komputerów brakujących aktualizacji w kategorii. Kliknij pozycję klasyfikacji, aby uruchomić wyszukiwanie dziennika zwracanie wszystkich aktualizacji rekordów dla tej klasyfikacji.|
**Aktualizowanie wdrożenia**||
Aktualizowanie wdrożenia | Liczba wdrożeń obecnie zaplanowanej aktualizacji i czasu trwania do Następne zaplanowane uruchomienie.  Kliknij na kafelku widok harmonogramów, uruchomiony i złożonym aktualizacje lub aby zaplanować nowe wdrożenia.|  
<br>  
![Aktualizowanie zarządzania podsumowanie pulpitu nawigacyjnego](./media/oms-solution-update-management/update-management-deployment-dashboard.png)<br>  
<br>
![Aktualizowanie widoku komputera pulpit nawigacyjny zarządzania](./media/oms-solution-update-management/update-management-assessment-computer-view.png)<br>  
<br>
![Aktualizowanie widoku pakiet zarządzania pulpitu nawigacyjnego](./media/oms-solution-update-management/update-management-assessment-package-view.png)<br>  

## <a name="installing-updates"></a>Instalowanie aktualizacji

Po aktualizacji oceniono na wszystkich komputerach z systemem Windows w środowisku, może mieć wymagane aktualizacji zainstalowanych przez utworzenie *Wdrażanie aktualizacji*.  Wdrażanie aktualizacji jest zaplanowana instalacja wymagane aktualizacje dla jednego lub kilku komputerach z systemem Windows.  Możesz określić datę i godzinę do wdrożenia oprócz komputer lub grupę komputerów, które powinny być dołączane.  

Aktualizacje są instalowane przez runbooks w automatyzacji Azure.  Obecnie nie można wyświetlić te runbooks, a ich nie wymaga konfiguracji.  Po utworzeniu wdrożenia usługi aktualizacji tworzy serii rozłożonych w czasie w tym uruchomienia działań aranżacji aktualizacji wzorca w określonym czasie dla komputerów uwzględniane.  Ten wzorca działań aranżacji rozpoczęcie działań aranżacji podrzędne na poszczególnych agenta systemu Windows, który wykonuje instalację wymagane aktualizacje.  

### <a name="viewing-update-deployments"></a>Wyświetlanie aktualizacji wdrożenia

Kliknij Kafelek **Wdrażanie aktualizacji** , aby wyświetlić listę istniejących wdrożenia aktualizacji.  Są one pogrupowane według stanu — **Zaplanowane**, **uruchomiony**i **wykonane**.<br><br> ![Aktualizowanie strony harmonogram wdrożenia](./media/oms-solution-update-management/update-updatedeployment-schedule-page.png)<br>  

W poniższej tabeli opisano właściwości wyświetlane dla każdej wdrażanie aktualizacji.

Właściwość | Opis|
----------|----------|
Nazwa | Nazwa wdrażanie aktualizacji.|
Harmonogram | Typ harmonogramu.  *OneTime* jest obecnie tylko możliwe wartości.|
Czas rozpoczęcia|Data i czas zaplanowany wdrażanie aktualizacji, aby rozpocząć.|
Czas trwania | Liczba minut, który może być uruchamiane wdrażanie aktualizacji.  Jeśli wszystkie aktualizacje nie są zainstalowane w tym czasie trwania, pozostałych aktualizacji muszą czekać do następnego wdrażanie aktualizacji.|
Serwery | Liczba komputerów dotyczy wdrażania aktualizacji.|
Stan | Bieżący stan wdrażania aktualizacji.<br><br>Możliwe wartości są następujące:<br>-Nie zostało uruchomione<br>-Uruchomiony<br>— Koniec|  

Kliknij pozycję na wdrażanie aktualizacji, aby wyświetlić jego ekran szczegółów, który zawiera kolumny w tabeli poniżej.  Tych kolumn nie zostaną wypełnione, jeśli wdrażanie aktualizacji nie został jeszcze rozpoczęty.<br>

Kolumny | Opis|
----------|----------|
**Wyniki komputera**||
Ukończona pomyślnie | Wyświetla liczbę komputerów w ramach wdrożenia aktualizacji według stanu.  Kliknij pozycję status, aby uruchomić wyszukiwanie dziennika zwracanie wszystkich aktualizacji rekordów o stanie wdrożenia aktualizacji.|
Komputer stanu instalacji| Wyświetla listę komputerów zajmujących się wdrażanie aktualizacji i procent aktualizacje, które pomyślnie zainstalowane. Kliknij jedną z pozycji, aby uruchomić wyszukiwanie dziennika zwracanie wszystkie brakujące i krytyczne aktualizacje.|
**Aktualizowanie wyników wystąpienia**||
Stan instalacji wystąpienia | Lista klasyfikacji aktualizacje, że komputery brakuje posortowane według liczby komputerów brakujących aktualizacji w kategorii. Kliknij pozycję komputer, aby uruchomić wyszukiwanie dziennika zwracanie wszystkich aktualizacji rekordów dla tego komputera.|  
<br><br> ![Omówienie wyniki wdrożenia aktualizacji](./media/oms-solution-update-management/update-la-updaterunresults-page.png)

### <a name="creating-an-update-deployment"></a>Tworzenie wdrażanie aktualizacji

Tworzenie nowego wdrażanie aktualizacji, klikając przycisk **Dodaj** w górnej części ekranu, aby otworzyć stronę **Nowy wdrażanie aktualizacji** .  Należy podać wartości dla właściwości w poniższej tabeli.

Właściwość | Opis|
----------|----------|
Nazwa | Unikatową nazwę identyfikującą wdrażanie aktualizacji.|
Strefa czasowa | Strefa czasowa służących do momentu rozpoczęcia.|
Czas rozpoczęcia | Data i godzina, aby rozpocząć wdrażanie aktualizacji.|
Czas trwania | Liczba minut, który może być uruchamiane wdrażanie aktualizacji.  Jeśli wszystkie aktualizacje nie są zainstalowane w tym czasie trwania, pozostałych aktualizacji muszą czekać do następnego wdrażanie aktualizacji.|
Komputery | Nazwy komputerów lub grup komputerów, aby uwzględnić w ramach wdrożenia aktualizacji.  Wybierz jeden lub więcej wpisów z listy rozwijanej.|
<br><br> ![Nowa strona wdrażania aktualizacji](./media/oms-solution-update-management/update-newupdaterun-page.png)

### <a name="time-range"></a>Zakres czasu

Domyślnie zakresu danych analizowane w rozwiązanie do zarządzania aktualizacji jest ze wszystkich grup połączonego zarządzania wygenerowany wewnątrz ostatniego dnia 1. 

Aby zmienić zakres czasu danych, zaznacz **dane na podstawie** u góry pulpitu nawigacyjnego. Możesz wybrać rekordy utworzona lub zaktualizowana w ciągu ostatnich 7 dni, 1 dzień lub 6 godzin. Lub można wybrać **Niestandardowy** i określ niestandardowy zakres dat.<br><br> ![Opcja Niestandardowy zakres czasu](./media/oms-solution-update-management/update-la-time-range-scope-databasedon.png)  

## <a name="log-analytics-records"></a>Rekordy analizy dziennika

Rozwiązanie do zarządzania aktualizacji tworzy dwa typy rekordów w repozytorium usługi OMS.

### <a name="update-records"></a>Aktualizowanie rekordów

Rekord z typem **Update** jest tworzony dla każdej aktualizacji jest zainstalowany lub potrzebne na każdym komputerze. Aktualizować rekordy mają właściwości w poniższej tabeli.

Właściwość | Opis|
----------|----------|
Typ | *Aktualizacja*|
SourceSystem | Źródło, które zatwierdzonych instalacji aktualizacji.<br>Możliwe wartości są następujące:<br>-Microsoft Update<br>— Aktualizacja systemu Windows<br>-SCCM<br>— Serwery Linux (pobrane z menedżerów pakietu)|
Zatwierdzony | Określa, czy zatwierdzono aktualizację instalacji.<br> Dla serwerów Linux jest obecnie opcjonalne jako poprawki nie zarządza usługi OMS.|
Klasyfikacja dla systemu Windows | Klasyfikacja aktualizacji.<br>Możliwe wartości są następujące:<br>-Aplikacje<br>— Aktualizacje krytyczne<br>— Aktualizacje definicji<br>-Feature Pack<br>— Aktualizacje zabezpieczeń<br>-Dodatki service pack<br>-Pakiety zbiorcze aktualizacji<br>— Aktualizacje|
Klasyfikacja Linux | Cassification aktualizacji.<br>Możliwe wartości są następujące:<br>— Aktualizacje krytyczne<br>— Aktualizacje zabezpieczeń<br>-Pozostałe aktualizacje|
Komputer | Nazwa komputera.|
InstallTimeAvailable | Określa, czy czas instalacji jest dostępna z innymi czynnikami zainstalowane w ramach tej samej aktualizacji.|
InstallTimePredictionSeconds | Szacowany czas w sekundach oparte na innych czynników zainstalowane aktualizacja.|
KBID | Identyfikator artykuł z bazy wiedzy, zawierającego opis aktualizacji.|
ManagementGroupName | Nazwa grupy zarządzania SCOM agentów.  W przypadku innych czynników jest AOI -<workspace ID>.|
MSRCBulletinID | Identyfikator biuletynu opisem aktualizacji.|
MSRCSeverity | Waga biuletyn zabezpieczeń firmy Microsoft.<br>Możliwe wartości są następujące:<br>— Krytyczne<br>-Ważne<br>— Średni|
Opcjonalne | Określa, czy aktualizacja jest opcjonalne.|
Produktu | Nazwa produktu, którego aktualizacji.  Kliknij przycisk **Widok** , aby otworzyć tego artykułu w przeglądarce.|
PackageSeverity | Waga luka ustalonych w tej aktualizacji zgłoszone przez dostawców distro Linux. | 
PublishDate | Data i godzina aktualizacja została zainstalowana.|
RebootBehavior | Określa, jeśli aktualizacja wymusza Uruchom ponownie komputer.<br>Możliwe wartości są następujące:<br>-canrequestreboot<br>-neverreboots|
RevisionNumber | Numer poprawki aktualizacji.|
SourceComputerId | Identyfikator GUID jednoznacznie komputera.|
TimeGenerated | Data i godzina ostatniej aktualizacji rekordu.|
Tytuł | Tytuł aktualizacji.|
Identyfikator aktualizacji | Identyfikator GUID jednoznacznie aktualizacji.|
UpdateState | Określa, czy aktualizacja jest zainstalowana na tym komputerze.<br>Możliwe wartości są następujące:<br>-Zainstalowana — aktualizacja jest zainstalowana na tym komputerze.<br>-Potrzeby - aktualizacji nie jest zainstalowany i jest wymagane na tym komputerze.|  

<br>
Podczas wykonywania wyszukiwania dziennika zwracające rekordy typu **aktualizacji** możesz wybrać widok **aktualizacje** , w którym wyświetlane są zestaw kafelków podsumowywania aktualizacje zwrócone przez wyszukiwanie. Możesz kliknąć pozycje w **Brak i zastosowaniu aktualizacji** i **aktualizacje wymaganych i opcjonalnych** kafelków do zakresu widoku, aby ten zestaw aktualizacji. Wybierz widok **listę** lub **tabelę** , aby zwrócić poszczególnych rekordów.<br> 

![Dziennik wyszukiwania aktualizacji widoku z aktualizacji typ rekordu](./media/oms-solution-update-management/update-la-view-updates.png)  

W widoku **tabeli** można kliknąć przycisk na **KBID** dla rekord, aby otworzyć przeglądarki zawierające artykuł z bazy wiedzy. Pozwala na szybkie zapoznanie się z szczegóły określonego aktualizacji.<br> 

![Widok tabeli wyszukiwania dziennika za pomocą aktualizacji typu rekordu kafelków](./media/oms-solution-update-management/update-la-view-table.png)

W widoku **listy** kliknij pozycję łącze **Wyświetl** obok KBID, aby otworzyć artykuł z bazy wiedzy.<br>

![Widok listy wyszukiwania dziennika za pomocą aktualizacji typu rekordu kafelków](./media/oms-solution-update-management/update-la-view-list.png)


###<a name="updatesummary-records"></a>Rekordy UpdateSummary

Rekord z typem **UpdateSummary** jest tworzony na każdym komputerze agenta systemu Windows. Ten rekord jest aktualizowany zawsze, gdy komputer jest zeskanowany aktualizacje. **UpdateSummary** rekordy mają właściwości w poniższej tabeli.

Właściwość | Opis|
----------|----------|
Typ | UpdateSummary|
SourceSystem | OpsManager |
Komputer | Nazwa komputera.|
CriticalUpdatesMissing | Liczba krytycznych aktualizacji Brak na tym komputerze.|
ManagementGroupName | Nazwa grupy zarządzania SCOM agentów. W przypadku innych czynników jest AOI -<workspace ID>.|
NETRuntimeVersion | Wersja środowisko uruchomieniowe .NET zainstalowana na tym komputerze.|
OldestMissingSecurityUpdateBucket | Opublikowano kolorem kategoryzowania czas, ponieważ aktualizacja najstarszych Brak zabezpieczeń na tym komputerze.<br>Możliwe wartości są następujące:<br>-Starsze<br>-180 dni temu<br>-150 dni temu<br>-120 dni temu<br>-90 dni temu<br>– 60 dni temu<br>-Przejdź 30 dni<br>-Ostatnio używane|
OldestMissingSecurityUpdateInDays | Liczba dni od najstarszych zabezpieczeń brakujących aktualizacji na tym komputerze został opublikowany.|
Element OsVersion | Wersja systemu operacyjnego zainstalowana na tym komputerze.|
OtherUpdatesMissing | Liczba innych aktualizacji Brak na tym komputerze.|
SecurityUpdatesMissing | Liczba aktualizacji zabezpieczeń brakuje na komputerze.|
SourceComputerId | Identyfikator GUID jednoznacznie komputera.|
TimeGenerated | Data i godzina ostatniej aktualizacji rekordu.|
TotalUpdatesMissing |Całkowita liczba aktualizacji Brak na tym komputerze.|
WindowsUpdateAgentVersion | Numer wersji programu Windows Update agent na tym komputerze.|
WindowsUpdateSetting | Ustawienie sposób instalowania aktualizacji ważne w komputerze.<br>Możliwe wartości są następujące:<br>-Wyłączone<br>-Powiadom przed instalacją<br>-Zaplanowanej instalacji|
WSUSServer | Adres URL WSUS serwer, jeśli komputer jest skonfigurowany do korzystania z jednego.|  

## <a name="sample-log-searches"></a>Przykładowy dziennik wyszukiwania

Poniższa tabela zawiera przykładowy dziennik wyszukuje aktualizować rekordy zebrane przy użyciu tego rozwiązania. 

Kwerendy | Opis|
----------|----------|
Wszystkie komputery z brakujące aktualizacje | Typ = Update UpdateState = potrzeby opcjonalny = false & #124; Wybierz pozycję komputer, tytuł, KBID, klasyfikacji, UpdateSeverity, PublishedDate|
Brakujące aktualizacje dla komputera "COMPUTER01.contoso.com" (zamień nazwę komputera) | Typ = UpdateState aktualizacji = potrzeby opcjonalny = komputer FAŁSZ = "COMPUTER01.contoso.com" & #124; Wybierz pozycję komputer, tytuł, KBID, produktu, UpdateSeverity, PublishedDate|
Wszystkie komputery z brakującym krytyczne i aktualizacje zabezpieczeń | Typ = Update UpdateState = potrzeby opcjonalny = false (klasyfikacji = klasyfikacja lub "Aktualizacji zabezpieczeń" = "Aktualizacje krytyczne")|
Aktualizacje krytyczne lub zabezpieczeń wymagane przez maszyny miejsce, w którym aktualizacje są stosowane ręcznie | Typ = UpdateState aktualizacji = potrzeby opcjonalny = false (klasyfikacji = klasyfikacja lub "Aktualizacji zabezpieczeń" = "Aktualizacje krytyczne") w komputerze {typ = UpdateSummary WindowsUpdateSetting = ręczne & #124; Różne komputerze} & #124; Różne KBID|
Błąd dla komputerów, na których brakuje krytycznych lub zabezpieczeń wymagane aktualizacje | Typ = EventLevelName zdarzenia = błąd w komputerze {typ = aktualizacji (klasyfikacji = klasyfikacja lub "Aktualizacji zabezpieczeń" = "Aktualizacje krytyczne") UpdateState = potrzeby opcjonalny = false & #124; Różne komputerze}|
Aktualizowanie wszystkich komputerach z brakującym zestawienia | Typ = aktualizacji opcjonalny = Klasyfikacja FAŁSZ = "Pakiety zbiorcze aktualizacji" UpdateState = potrzeby & #124; Wybierz pozycję komputer, tytuł, KBID, klasyfikacji, UpdateSeverity, PublishedDate|
Brakujące aktualizacje różnią się na wszystkich komputerach | Typ = Update UpdateState = potrzeby opcjonalny = false & #124; Tytuł DISTINCT|
Członkostwo komputera WSUS | Typ = UpdateSummary & #124; Zmierz count() przez WSUSServer|
Konfiguracja automatyczna aktualizacja | Typ = UpdateSummary & #124; Zmierz count() przez WindowsUpdateSetting|
Komputerach z automatyczna aktualizacja wyłączone | Typ = UpdateSummary WindowsUpdateSetting = ręczne|  
Listę wszystkich komputerów Linux, które mają pakiet dostępna jest aktualizacja | Typ = aktualizacji i OSType = Linux oraz UpdateState! = "Nie potrzebne" & #124; Zmierz count() przez komputer|
Listę wszystkich komputerach Linux, które mają pakiet dostępna jest aktualizacja, która dotyczy luka krytyczne lub zabezpieczeń | Typ = aktualizacji i OSType = Linux oraz UpdateState! = "Nie potrzebne" i (klasyfikacji = klasyfikacja lub "Aktualizacje krytyczne" = "Aktualizacji zabezpieczeń") & #124; Zmierz count() przez komputer|
Lista wszystkich pakietów, które mają dostępna jest aktualizacja | Typ = aktualizacji i OSType = Linux oraz UpdateState! = "Nie potrzebne"|
Listę wszystkich pakietów, które mają dostępna jest aktualizacja, która dotyczy luka krytyczne lub zabezpieczeń | Typ = aktualizacji i OSType = Linux oraz UpdateState! = "Nie potrzebne" i (klasyfikacji = klasyfikacja lub "Aktualizacje krytyczne" = "Aktualizacji zabezpieczeń")|
Listę wszystkich komputerów "Ubuntu" z dostępna aktualizacja | Typ = aktualizacji i OSType = Linux oraz OSName = Ubuntu & #124; Zmierz count() przez komputer|

## <a name="next-steps"></a>Następne kroki

- Wyświetlanie szczegółowych aktualizowania danych przy użyciu wyszukiwania dziennika do [Analizy dziennika](../log-analytics/log-analytics-log-searches.md) .

- [Tworzenie własnych pulpitów nawigacyjnych](../log-analytics/log-analytics-dashboards.md) przedstawiający aktualizacji zgodności dla zarządzanych komputerów.

- [Tworzenie alertów](../log-analytics/log-analytics-alerts.md) , gdy zostaną wykryte krytyczne aktualizacje jako brakuje komputerach lub na komputerze jest wyłączona, funkcja Aktualizacje automatyczne.  




