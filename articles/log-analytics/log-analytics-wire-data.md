<properties
    pageTitle="Szkielety rozwiązania danych w dzienniku analizy | Microsoft Azure"
    description="Dane przewodowy są skonsolidowane dane siecią i wydajnością od komputerów z usługi OMS czynników, takich jak programu Operations Manager i czynników związanych z systemem Windows. Danych sieciowych jest używana w połączeniu z danymi dziennika ułatwiające grupowania danych."
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
    ms.date="08/11/2016"
    ms.author="banders"/>

# <a name="wire-data-solution-in-log-analytics"></a>Szkielety rozwiązania danych do analizy dziennika

Dane przewodowy są skonsolidowane dane siecią i wydajnością od komputerów z usługi OMS czynników, takich jak programu Operations Manager i czynników związanych z systemem Windows. Danych sieciowych jest używana w połączeniu z danymi dziennika ułatwiające grupowania danych. Czynniki usługi OMS zainstalowany na komputerach w danych sieć monitor infrastruktury informatycznej przesyłane do i z tych komputerów dla sieci poziomy 2-3 w [modelu OSI](https://en.wikipedia.org/wiki/OSI_model) , w tym różne protokoły i porty używane.

>[AZURE.NOTE] Rozwiązania przewodowy danych nie jest obecnie dostępny do dodania do obszarów roboczych. Klienci, którzy już rozwiązanie danych przewodowy włączone nadal korzystać rozwiązania przewodowy danych.

Domyślnie usługi OMS zbiera dane zarejestrowane dla Procesora, pamięci, dysku i dane dotyczące wydajności sieci z liczników wbudowane w systemie Windows. Sieć i innych zbierania danych odbywa się w czasie rzeczywistym dla każdego agenta, łącznie z podsieci i protokoły poziomie aplikacji używane przez komputer. Na stronie Ustawienia na karcie dzienników, możesz dodać inne liczniki wydajności.

Jeśli użyto [sFlow](http://www.sflow.org/) lub inne oprogramowanie z [protokołem NetFlow firmy Cisco](http://www.cisco.com/c/en/us/products/collateral/ios-nx-os-software/ios-netflow/prod_white_paper0900aecd80406232.html), statystyki i dane, które zobaczysz z drutu danych zostaną znane.

Oto niektóre typy wbudowane dziennika kwerend wyszukiwania:

- Czynniki dostarczających dane przewodowy
- Adres IP czynników dostarczania przewodowy danych
- Komunikacji wychodzącej według adresów IP
- Liczba bajtów wysłanych przez protokoły aplikacji
- Liczba bajtów wysłanych przez usługę aplikacji
- Bajtów odebranych przez różne protokoły
- Całkowita liczba bajtów wysyłane i otrzymywane według adresów IP
- Adresy IP, które zostały przekazane czynnikami podsieci 10.0.0.0/8
- Średni czas oczekiwania dla połączeń, które zostały w wiarygodny
- Procesów komputerowych, które zainicjowana lub odebrana ruch sieciowy
- Ilość ruchu sieciowego dla procesu

Podczas wyszukiwania przy użyciu drutu danych, można filtrować i grupowanie danych w celu wyświetlenia informacji na temat agentów górnym i protokoły górny. Lub można wyszukać go do przy niektórych komputerów (adresy MAC adresów IP) przekazywane ze sobą, jak długo trwa i ilości danych została wysłana — zasadniczo wyświetlanie metadanych dotyczących ruchu sieciowego, czyli na podstawie wyszukiwania.

Jednak ponieważ są wyświetlane metadane, nie jest niekoniecznie przydatne do szczegółowego rozwiązywania problemów. Przewodowy danych usługi OMS nie jest pełna przechwytywania danych sieciowych. Tak jest nie przeznaczona dla głębokich poziomie pakietów rozwiązywania problemów.
Korzyścią ze stosowania agentem w porównaniu z innymi metodami zbioru, to, że nie musisz zainstalować urządzenia, skonfigurowanie przełączników sieci lub preform złożonej konfiguracji. Przewodowy danych jest po prostu agenta oparty na — agent można zainstalować na komputerze i będzie monitorować ruch sieci. Inną zaletą jest, gdy chcesz monitorować obciążenia uruchomiony w chmurze dostawców lub dostawcy hostingu usługi Microsoft Azure, której użytkownik nie właścicielem warstwy tkaninie.

Natomiast nie masz pełną widoczność występujące w sieci, jeśli nie zostanie zainstalowana agentów na wszystkich komputerach w infrastruktury sieciowej.

## <a name="installing-and-configuring-the-solution"></a>Instalowanie i konfigurowanie rozwiązanie
Aby zainstalować i skonfigurować rozwiązanie, korzystając z następujących informacji.

- Rozwiązania danych przewodowy uzyskuje dane z komputera z systemem Windows Server 2012 R2, Windows 8.1 lub nowszego.
- Na komputerach, w której chcesz uzyskać przewodowy danych z jest wymagany program Microsoft .NET Framework 4.0 lub nowszym.
- Dodawanie rozwiązania przewodowy danych do obszaru roboczego usługi OMS przy użyciu procesu opisanego w [rozwiązań Dodawanie analizy dziennika z galerii rozwiązań](log-analytics-add-solutions.md).  Nie jest wymagana żadna konfiguracja dalsze.
- Jeśli chcesz wyświetlić dane przewodowy dla określonych rozwiązań, musisz mieć rozwiązanie już dodane do obszaru roboczego usługi OMS.

## <a name="wire-data-data-collection-details"></a>Szkielety szczegóły zbioru danych danych

Dane przewodowy zbiera metadanych dotyczących ruchu sieciowego przy użyciu czynników, które są włączone.

W poniższej tabeli przedstawiono metody zbioru danych i inne szczegóły dotyczące jak dane są zbierane przewodowy danych.


| platformy | Bezpośrednie agenta | SCOM agent | Azure miejsca do magazynowania | SCOM wymagane? | Dane agenta SCOM wysyłane za pośrednictwem grupy zarządzania | częstotliwość pobierania |
|---|---|---|---|---|---|---|
|Windows (2012 R2 / 8.1 lub nowszy)|![Tak](./media/log-analytics-wire-data/oms-bullet-green.png)|![Tak](./media/log-analytics-wire-data/oms-bullet-green.png)|![Brak](./media/log-analytics-wire-data/oms-bullet-red.png)|            ![Brak](./media/log-analytics-wire-data/oms-bullet-red.png)|![Brak](./media/log-analytics-wire-data/oms-bullet-red.png)| co minutę|


## <a name="combining-wire-data-with-other-solution-data"></a>Łączenie danych przewodowy na inne dane rozwiązanie

Dane zwracane przez kwerendy wbudowanych, jak pokazano powyżej może być interesujące samodzielnie. Jednak funkcjonalność danych przewodowy biznesowego połączenie na podstawie informacji z innych rozwiązań usługi OMS. Można na przykład przy użyciu zabezpieczeń zdarzenia danych zebranych przez rozwiązanie zabezpieczeń i inspekcji i połączenie go z danymi przewodowy szukać nietypowe sieci prób logowania dla nazwanych procesów.  W tym przykładzie operatorów w i DISTINCT służących do punktów danych w kwerendzie wyszukiwania.

Wymagania: Aby można było używać w poniższym przykładzie, musisz mieć zainstalowany rozwiązanie zabezpieczeń i inspekcji. Jednak dane z innych rozwiązań umożliwia połączenie z danymi przewodowy podobne wyniki.

### <a name="to-combine-wire-data-with-security-events"></a>Połączenie danych drutu z wydarzeniami zabezpieczeń

1. Na stronie Przegląd kliknij Kafelek **WireData** .
2. Na liście **Typowych kwerend WireData**kliknij pozycję **Kwota z ruchu sieciowego (w bajtach) przez proces** Aby wyświetlić listę zwróconych procesów.
    ![kwerendy wiredata](./media/log-analytics-wire-data/oms-wiredata-01.png)
3. Jeśli lista procesów jest zbyt długi, aby łatwo wyświetlać, możesz zmodyfikować kwerendę wyszukiwania do postaci zbliżonej:

    ```
    Type WireData | measure count() by ProcessName | where AggregatedValue <40
    ```
    W przykładzie poniżej procesu nosi nazwę DancingPigs.exe, które mogą być wyświetlane podejrzanych.
    ![wyniki wyszukiwania wiredata](./media/log-analytics-wire-data/oms-wiredata-02.png)

4. Używając danych zwróconych na liście, kliknij proces o określonej nazwie. W tym przykładzie DancingPigs.exe został kliknięty. Wyniki, jak pokazano poniżej opis typu ruchu sieciowego, takich jak komunikacji wychodzącej przez różne protokoły.
    ![wyniki wiredata przedstawiający proces o określonej nazwie](./media/log-analytics-wire-data/oms-wiredata-03.png)

5. Ponieważ rozwiązanie zabezpieczeń i inspekcji jest zainstalowany, możesz sondy do zdarzenia zabezpieczeń, które mają tę samą wartość pola parametr zmieniając kwerendę wyszukiwania przy użyciu operatorów kwerendy wyszukiwania w i unikatowe. Jest to następnie zarówno przewodowy danych i inne dzienniki rozwiązanie zawierających wartości w tym samym formacie. Zmodyfikuj kwerendę wyszukiwania do postaci zbliżonej:

    ```
    Type=SecurityEvent ProcessName IN {Type:WireData "DancingPigs.exe" | distinct ProcessName}
    ```    

    ![wiredata wyników połączone z danymi](./media/log-analytics-wire-data/oms-wiredata-04.png)
6. W wynikach powyżej pojawi się, że są wyświetlane informacje o koncie. Teraz możesz uściślić kwerendę wyszukiwania, aby dowiedzieć się, jak często użyto konta, zabezpieczenia i inspekcji danymi, przez proces z podobne do kwerendy:        

    ```
    Type=SecurityEvent ProcessName IN {Type:WireData "DancingPigs.exe" | distinct ProcessName} | measure count() by Account
    ```

    ![wyniki wiredata danymi konta](./media/log-analytics-wire-data/oms-wiredata-05.png)



## <a name="next-steps"></a>Następne kroki

- [Dzienniki wyszukiwania](log-analytics-log-searches.md) , aby wyświetlić szczegółowe przewodowy danych wyszukiwanie rekordów.
- Zobacz, że osoby Paweł [Publikowanie przy użyciu danych drutu w blogu operacje zarządzania pakietu dziennika wyszukiwania](http://blogs.msdn.com/b/dmuscett/archive/2015/09/09/using-wire-data-in-operations-management-suite.aspx) zawiera dodatkowe informacje o częstotliwości dane są zbierane i jak można modyfikować właściwości zbioru dla programu Operations Manager agentów.
