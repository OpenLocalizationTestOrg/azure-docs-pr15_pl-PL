<properties
    pageTitle="Optymalizowanie środowiska z rozwiązanie tkaninie usługi w dzienniku analizy | Microsoft Azure"
    description="Rozwiązanie tkaninie usługi można stosować do oceniania ryzyka i kondycji aplikacji usługi tkaninie, micro usług, węzły i klastrów."
    services="log-analytics"
    documentationCenter=""
    authors="niniikhena"
    manager="jochan"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/21/2016"
    ms.author="nini"/>



# <a name="service-fabric-solution-in-log-analytics"></a>Rozwiązania tkaninie usługi w analizy dziennika

> [AZURE.SELECTOR]
- [Menedżer zasobów](log-analytics-service-fabric-azure-resource-manager.md)
- [Programu PowerShell](log-analytics-service-fabric.md)

W tym artykule opisano, jak za pomocą rozwiązanie tkaninie usługi w dzienniku analizy identyfikowanie i rozwiązywanie problemów przez klaster tkaninie usługi.

Rozwiązanie tkaninie usługa korzysta z danych diagnostyki Azure z usługi maszyny wirtualne tkaninie usługi przez zbieranie danych z tabel Azure WAD. Następnie analizy dziennika odczytuje tkaninie usługi framework wydarzenia, w tym **Zaufanego zdarzenia usługi**, **Zdarzeń Aktor** **Zdarzenia operacyjne**i **zdarzenia ETW niestandardowe**. Z pulpitu nawigacyjnego rozwiązanie jest możliwość wyświetlania godne uwagi problemów i istotnych zdarzeń w środowisku usługi tkaninie.

Aby rozpocząć pracę z rozwiązanie, należy połączyć klaster tkaninie usług do obszaru roboczego analizy dziennika. Oto trzy scenariusze brać pod uwagę:

1. Klaster tkaninie usługa nie została wdrożona, wykonaj czynności podane w ***Rozmieszczanie klastrze tkaninie usługi podłączony do obszaru roboczego analizy dziennika*** wdrażać nowy klaster i został skonfigurowany do raportu w celu analizy dziennika.

2. Jeśli chcesz uzyskać liczniki wydajności za pomocą usługi hosts, aby użyć innych rozwiązań usługi OMS, takich jak zabezpieczeń w klastrze tkaninie usługi, postępuj zgodnie z instrukcjami ***Rozmieszczanie klastrze tkaninie usługi połączone z obszarem roboczym usługi OMS z rozszerzeniem maszyn wirtualnych zainstalowany.***

3. Jeśli już wdrożono usługi Klaster tkaninie usługi, aby połączyć go z dziennika analizy, postępuj zgodnie z instrukcjami ***Dodawanie istniejącego konta miejsca do magazynowania do analizy dziennika.***


##<a name="deploy-a-service-fabric-cluster-connected-to-a-log-analytics-workspace"></a>Wdrażanie usługi Klaster tkaninie podłączony do obszaru roboczego analizy dziennika.
Ten szablon wykonuje następujące czynności:


1. Wdraża klaster tkaninie usługi Azure już podłączony do obszaru roboczego analizy dziennika. Masz opcję, aby utworzyć nowy obszar roboczy podczas wdrażania szablonu lub wprowadzania nazwę istniejącego już obszaru roboczego analizy dziennika.
2. Dodaje konto diagnostyczne miejsca do magazynowania do obszaru roboczego analizy dziennika.
3. Umożliwia rozwiązanie tkaninie usługi w obszarze roboczym analizy dziennika.

[![Wdrażanie Azure](./media/log-analytics-service-fabric/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fazure%2Fazure-quickstart-templates%2Fmaster%2Fservice-fabric-oms%2F%2Fazuredeploy.json)


Po wybraniu przycisku rozmieszczanie powyżej zostaną dostarczone w portalu Azure z parametrami do edycji. Pamiętaj utworzyć nową grupę zasobów, jeśli wprowadzisz nazwę nowego obszaru roboczego analizy dziennika: ![tkaninie usługi](./media/log-analytics-service-fabric/2.png)

![Usługa tkaninie](./media/log-analytics-service-fabric/3.png)

Zaakceptuj warunki prawne i kliknij przycisk "Tworzenie", aby rozpocząć wdrożenia. Po zakończeniu rozmieszczania powinien zostać wyświetlony nowego obszaru roboczego i klaster utworzone i WADServiceFabric * zdarzenie, WADWindowsEventLogs i WADETWEvent tabele dodane:

![Usługa tkaninie](./media/log-analytics-service-fabric/4.png)

##<a name="deploy-a-service-fabric-cluster-connected-to-an-oms-workspace-with-vm-extension-installed"></a>Wdrażanie klastrze tkaninie usługi połączone z obszarem roboczym usługi OMS z rozszerzeniem maszyn wirtualnych zainstalowany.
Ten szablon wykonuje następujące czynności:

1. Wdraża klaster tkaninie usługi Azure już podłączony do obszaru roboczego analizy dziennika. Możesz utworzyć nowy obszar roboczy lub użycie istniejącej.
2. Dodaje konta diagnostyczne miejsca do magazynowania do obszaru roboczego analizy dziennika.
3. Umożliwia rozwiązanie tkaninie usługi w obszarze roboczym analizy dziennika.
4. Instaluje rozszerzenia agenta MMA w każdej skali maszyn wirtualnych w klastrze tkaninie usługi. Z agentem MMA zainstalowany możesz będą mogli wyświetlać wskaźniki o węzły.


[![Wdrażanie Azure](./media/log-analytics-service-fabric/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fazure%2Fazure-quickstart-templates%2Fmaster%2Fservice-fabric-vmss-oms%2F%2Fazuredeploy.json)


Po te same kroki powyżej wymaganych parametrów wejściowych i rozpocząć wdrożeniu. Ponownie powinien zostać wyświetlony nowy obszar roboczy, klaster i tabele WAD wszystkich utworzonych:

![Usługa tkaninie](./media/log-analytics-service-fabric/5.png)

###<a name="viewing-performance-data"></a>Przeglądanie danych dotyczących wydajności

Aby wyświetlić danych wydajności z węzły:
</br>
- Uruchamianie obszaru roboczego analizy dziennika z portalu Azure.

![Usługa tkaninie](./media/log-analytics-service-fabric/6.png)

- Przejdź do strony Ustawienia w lewym okienku, a następnie zaznacz dane >> liczników wydajności systemu Windows >> "Dodawanie liczników wydajności zaznaczonego": ![tkaninie usługi](./media/log-analytics-service-fabric/7.png)

- W polu Wyszukaj dziennika umożliwiają delve do kluczowych miar o węzły następujące kwerendy:
</br>

    . Porównanie średniej użycie Procesora przez wszystkie węzły w ostatnim godzinę węzły, które mają problemy i jakie interwałem czasu węzeł sprzedał kolekcji:

    ``` Type=Perf ObjectName=Processor CounterName="% Processor Time"|measure avg(CounterValue) by Computer Interval 1HOUR. ```

    ![Usługa tkaninie](./media/log-analytics-service-fabric/10.png)


    b. Wyświetlanie podobnych wykresów liniowych dla dostępną pamięć w każdym węźle poniższe zapytanie:

    ```Type=Perf ObjectName=Memory CounterName="Available MBytes Memory" | measure avg(CounterValue) by Computer Interval 1HOUR.```

    Aby wyświetlić listę wszystkich węzły, przedstawiający dokładnej wartości średniej dla MB dostępne dla każdego węzła, użyj tej kwerendy:

    ```Type=Perf (ObjectName=Memory) (CounterName="Available MBytes") | measure avg(CounterValue) by Computer ```

    ![Usługa tkaninie](./media/log-analytics-service-fabric/11.png)


    c. W przypadku, gdy chcesz przechodzić do określonego węzła sprawdzając godzinowe średnia, minimalną, maksymalną i 75 percentyl użycie Procesora możesz to zrobić za pomocą tej kwerendy (Zastąp pole komputera):

    ```Type=Perf CounterName="% Processor Time" InstanceName=_Total Computer="BaconDC01.BaconLand.com"| measure min(CounterValue), avg(CounterValue), percentile75(CounterValue), max(CounterValue) by Computer Interval 1HOUR```

    ![Usługa tkaninie](./media/log-analytics-service-fabric/12.png)

    Przeczytaj więcej informacji na temat wskaźniki w dzienniku analizy [tutaj]. (https://blogs.technet.microsoft.com/msoms/tag/metrics/)


##<a name="adding-an-existing-storage-account-to-log-analytics"></a>Dodawanie istniejącego konta miejsca do magazynowania do analizy dziennika

Ten szablon po prostu dodaje istniejącego konta miejsca do magazynowania do nowego lub istniejącego obszaru roboczego analizy dziennika.
</br>

[![Wdrażanie Azure](./media/log-analytics-service-fabric/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Foms-existing-storage-account%2Fazuredeploy.json)

>[AZURE.NOTE] W wyborze grupa zasobów, jeśli pracujesz z istniejącym workspace analizy dziennika, wybierz "Użyj istniejącej" i wyszukaj grupę zasobów, zawierającą obszaru roboczego usługi OMS. Tworzenie nowego Jeżeli jeden inny sposób.
![Usługa tkaninie](./media/log-analytics-service-fabric/8.png)

Po wdrożeniu tego szablonu, można wyświetlić konta miejsca do magazynowania podłączony do obszaru roboczego analizy dziennika. W takim przypadku można dodać jedno konto więcej miejsca do magazynowania do obszaru roboczego programu Exchange, utworzonego powyżej.
![Usługa tkaninie](./media/log-analytics-service-fabric/9.png)

## <a name="view-service-fabric-events"></a>Przeglądanie zdarzeń tkaninie usługi

Po zakończeniu wdrożenia i rozwiązanie tkaninie usługi został włączony w obszarze roboczym, wybierz Kafelek **Tkaninie usługi** w portalu analizy dziennika do uruchamiania usługi tkaninie pulpitu nawigacyjnego. Pulpit nawigacyjny zawiera kolumny w tabeli poniżej. Każda kolumna zawiera listę górny dziesięć zdarzeń według liczby spełniające kryteria tej kolumny dla określonego czasu. Można przeprowadzić wyszukiwanie dziennika zawierającego całą listę przez kliknięcie **wyświetlić wszystkie** w prawej dolnej części każdej kolumny, lub klikając jej nagłówek.

| **Usługa tkaninie zdarzenia** | **Opis** |
| --- | --- |
| Problemy z godne uwagi | Wyświetlanie problemów, takie jak RunAsyncFailures RunAsynCancellations i rozwijane węzeł. |
| Zdarzenia operacyjne | Godne uwagi operacyjne zdarzenia, na przykład uaktualnienia aplikacji i wdrożenia. |
| Zdarzenia usługi zaufanego | Zdarzenia usługi zaufanego godne uwagi takich Runasyncinvocations. |
| Aktor zdarzenia | Aktor godne uwagi zdarzenia wygenerowane przez micro usług, takich jak wyjątki generowane przez metodę aktora, aktor aktywacji i deactivations i tak dalej. |
| Zdarzenia aplikacji | Wszystkie niestandardowe ETW zdarzenia generowane przez aplikacje. |

![Pulpit nawigacyjny tkaninie usługi](./media/log-analytics-service-fabric/sf3.png)

![Pulpit nawigacyjny tkaninie usługi](./media/log-analytics-service-fabric/sf4.png)


W poniższej tabeli przedstawiono metody zbioru danych i inne szczegóły dotyczące sposobu dane są zbierane tkaninie usługi.

| platformy | Bezpośrednie agenta | SCOM agent | Azure miejsca do magazynowania | SCOM wymagane? | Dane agenta SCOM wysyłane za pośrednictwem grupy zarządzania | częstotliwość pobierania |
|---|---|---|---|---|---|---|
|Systemu Windows|![Brak](./media/log-analytics-malware/oms-bullet-red.png)|![Brak](./media/log-analytics-malware/oms-bullet-red.png)| ![Tak](./media/log-analytics-malware/oms-bullet-green.png)|            ![Brak](./media/log-analytics-malware/oms-bullet-red.png)|![Brak](./media/log-analytics-malware/oms-bullet-red.png)|10 minut |


>[AZURE.NOTE] Zakres tych zdarzeń w rozwiązaniu tkaninie usługi można zmienić, klikając pozycję **dane oparte na okres ostatnich 7 dni** u góry pulpitu nawigacyjnego. Można również wyświetlać zdarzenia generowane w ciągu ostatnich 7 dni, 1 dzień lub sześć godzin. Lub wybrać **niestandardowe** umożliwiają określenie niestandardowego zakresu dat.


## <a name="next-steps"></a>Następne kroki

- Umożliwia wyświetlanie szczegółowych danych zdarzenia tkaninie usługi [Wyszukiwania dziennika analizy dziennika](log-analytics-log-searches.md) .
