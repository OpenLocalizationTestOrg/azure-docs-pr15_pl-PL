<properties
    pageTitle="Monitorowanie konto miejsca do magazynowania | Microsoft Azure"
    description="Dowiedz się, jak monitorować konta magazynu platformy Azure za pomocą Azure Portal."
    services="storage"
    documentationCenter=""
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="robinsh"/>

# <a name="monitor-a-storage-account-in-the-azure-portal"></a>Monitorowanie konto miejsca do magazynowania w Azure Portal

## <a name="overview"></a>Omówienie

Można monitorować konta miejsca do magazynowania z [Azure Portal](https://portal.azure.com). Po skonfigurowaniu konta miejsca do magazynowania dla monitorowania za pośrednictwem portalu magazyn Azure używa [Analizy miejsca do magazynowania](http://msdn.microsoft.com/library/azure/hh343270.aspx) do śledzenia metryki dla Twojego konta i rejestrowanie danych wezwanie.

> [AZURE.NOTE]Dodatkowe koszty są skojarzone z badania danych monitorowania w [Azure Portal](https://portal.azure.com). Aby uzyskać więcej informacji zobacz <a href="http://msdn.microsoft.com/library/azure/hh360997.aspx">analizy przestrzeni dyskowej i rozliczeń</a>. <br />

> Azure magazyn plików obecnie obsługuje metryki magazynowania analizy, ale jeszcze nie obsługuje rejestrowania. Możesz włączyć metryki do przechowywania plików Azure za pośrednictwem [Azure Portal](https://portal.azure.com).

> Metryki lub funkcja rejestrowania włączone w tej chwili nie jest miejsca do magazynowania konta typu replikacji z zbędne strefy przestrzeni dyskowej (ZRS). 

> Szczegółowo przewodnika przy użyciu analizy miejsca do magazynowania i innych narzędzi do identyfikowania, diagnozowanie i rozwiązywanie problemów związanych z magazyn Azure zobacz [Monitorowanie diagnozowanie i rozwiązywanie problemów z magazynem tabel platformy Microsoft Azure](storage-monitoring-diagnosing-troubleshooting.md).


## <a name="how-to-configure-monitoring-for-a-storage-account"></a>Jak: Konfigurowanie monitorowania konta miejsca do magazynowania

1. W [Azure Portal](https://portal.azure.com)kliknij pozycję **Magazyn**, a następnie kliknij nazwę konta magazynu, aby otworzyć pulpitu nawigacyjnego.

2. Kliknij przycisk **Konfiguruj**i przewiń do pozycji ustawienia **monitorowania** dla obiektów blob, tabel i usług kolejki.

    ![MonitoringOptions](./media/storage-monitor-storage-account/Storage_MonitoringOptions.png)

3. **Monitorowanie**ustawić poziom monitorowania i zasad przechowywania danych dla każdej usługi:

    -  Aby ustawić poziom monitorowania, wybierz jedną z następujących czynności:

      **Minimalne** - metryki zbiera takich jak ingress i wyjściowego, dostępność opóźnienie i sukcesu wartości procentowych, które są agregowane dla obiektów blob, tabel i usług kolejki.

      **Pełne** — oprócz minimalnego metryki zbiera ten sam zestaw metryki dla każdej operacji miejsca do magazynowania w interfejsu API usługi magazynu Azure. Pełne metryki umożliwiają dokładniejsze analizy problemy występujące podczas operacji aplikacji.

      **Wyłączanie** — powoduje wyłączenie monitorowania. Istniejące monitorowanie danych jest zachowywane do końca okres przechowywania.

- Aby ustawić zasady przechowywania danych w **przechowywania (w dniach)**, wpisz liczbę dni przechowywania od 1 do 365 dni danych. Jeśli nie chcesz ustawić zasady przechowywania, wprowadź wartość zero. W przypadku nie zasady przechowywania, jest możesz usunąć dane monitorowania. Zaleca się ustawienie zasad przechowywania oparte na jak długo trwa chcesz zachować dane analizy miejsca do magazynowania dla Twojego konta, tak aby starych i nieużywanych analizy danych można usunąć przez system bezpłatnie.

4. Po zakończeniu konfiguracji monitorowania, kliknij przycisk **Zapisz**.

Należy uruchomić, są wyświetlane monitorowanie danych na pulpicie nawigacyjnym i strona **Monitor** po około godziny.

Dopóki nie zostanie skonfigurowana monitorowania konta miejsca do magazynowania, bez monitorowania danych są zbierane i wykresy metryki na stronie pulpitu nawigacyjnego i **Monitor** są puste.

Po ustawieniu poziomy monitorowania i zasady przechowywania, można określić, które miar dostępnych do monitorowania w [Azure Portal](https://portal.azure.com)i które metryki kreślenia na wykresach metryki. Domyślny zestaw metryki jest wyświetlany na wszystkich poziomach monitorowania. **Dodawanie metryki** umożliwia dodawanie lub usuwanie metryki na liście metryki.

Metryki są przechowywane na koncie miejsca do magazynowania w czterech tabelach o nazwie $MetricsTransactionsBlob, $MetricsTransactionsTable, $MetricsTransactionsQueue i $MetricsCapacityBlob. Aby uzyskać więcej informacji zobacz [Temat metryki analizy magazynowania](http://msdn.microsoft.com/library/azure/hh343258.aspx).


## <a name="how-to-customize-the-dashboard-for-monitoring"></a>Jak: dostosowywanie pulpitu nawigacyjnego do monitorowania

Na pulpicie nawigacyjnym możesz wybrać maksymalnie sześć metryki zostać przedstawione na wykresie metryki z dziewięciu dostępne wskaźniki. Dla każdej usługi (obiektów blob, tabel i kolejki) metryki dostępności, Procent sukcesu i całkowita liczba żądań są dostępne. Dostępne na pulpicie nawigacyjnym metryki jest taka sama dla minimalnego lub pełnej kontroli.

1. W [Azure Portal](https://portal.azure.com)kliknij pozycję **Magazyn**, a następnie kliknij nazwę konta przestrzeni dyskowej, aby otworzyć pulpitu nawigacyjnego.

2. Aby zmienić miar, które są kreślone na wykresie, wykonaj jedną z następujących czynności:

    - Aby dodać nowe jednostki metryczne do wykresu, kliknij przycisk kolorowego pola wyboru obok metryczne nagłówka w tabeli poniżej wykresu.

    - Aby ukryć metryki wykreślone na wykresie, wyczyść pole wyboru kolorowego obok nagłówka metryczne.

        ![Monitoring_nmore](./media/storage-monitor-storage-account/storage_Monitoring_nmore.png)

3. Domyślnie na wykresie pokazano trendów, wyświetlając bieżącą wartość każdej jednostki metryczne (opcja **względem** w górnej części wykresu). Aby wyświetlić oś Y, dzięki czemu można zobaczyć wartości bezwzględne, wybierz pozycję **bezwzględnymi**.

4. Aby zmienić zakres czasu wyświetla wykres metryki, wybierz 6 godzin, 24 godziny lub 7 dni w górnej części wykresu.


## <a name="how-to-customize-the-monitor-page"></a>Jak: Dostosowywanie strony monitora

Na stronie **monitora** możesz wyświetlić pełny zestaw metryki dla Twojego konta miejsca do magazynowania.

- Jeśli Twoje konto miejsca do magazynowania ma minimalnego monitorowania skonfigurowane, metryki, takich jak ingress i wyjściowego, dostępność, opóźnienia i procentów sukcesu łączy się z obiektów blob, tabeli i usług kolejki.

- Jeśli Twoje konto miejsca do magazynowania ma pełne monitorowania skonfigurowane, metryki są dostępne w większą rozdzielczość operacji magazynowania poszczególnych oprócz agregacje poziomu usług.

Umożliwia wybieranie które metryki magazynowania Aby wyświetlić metryki wykresów i tabel, wyświetlanych na stronie **Monitor** poniższych procedur. Te ustawienia nie ma wpływu zbioru, agregacji i przechowywania monitorowanie danych na koncie miejsca do magazynowania.

## <a name="how-to-add-metrics-to-the-metrics-table"></a>Jak: Dodawanie metryki do tabeli metryki


1. W [Azure Portal](https://portal.azure.com)kliknij pozycję **Magazyn**, a następnie kliknij nazwę konta przestrzeni dyskowej, aby otworzyć pulpitu nawigacyjnego.

2. Kliknij przycisk **Monitor**.

    Zostanie wyświetlona strona **Monitor** . Domyślnie tabela metryki wyświetla podzbiór miar, które są dostępne dla monitorowania. Na ilustracji przedstawiono monitora domyślnego konta miejsca do magazynowania z pełne monitorowania skonfigurowane dla wszystkich trzech usług. **Dodawanie metryki** umożliwia wybranie metryk, które chcesz monitorować z wszystkie dostępne wskaźniki.

    ![Monitoring_VerboseDisplay](./media/storage-monitor-storage-account/Storage_Monitoring_VerboseDisplay.png)

    > [AZURE.NOTE] Po wybraniu metryki, warto rozważyć kosztów. Istnieją transakcji i wyjściowego koszty odświeżanie Wyświetla monitorowania. Aby uzyskać więcej informacji zobacz [analizy przestrzeni dyskowej i rozliczeń](http://msdn.microsoft.com/library/azure/hh360997.aspx).

3. Kliknij przycisk **Dodaj metryki**.

    Metryki agregacji dostępne w minimalnym monitorowania znajdowały się u góry listy. Jeśli pole wyboru jest zaznaczone, metryka zostanie wyświetlona na liście metryki.

    ![AddMetricsInitialDisplay](./media/storage-monitor-storage-account/Storage_AddMetrics_InitialDisplay.png)

4. Umieść wskaźnik na prawej części okna dialogowego, aby wyświetlić pasek przewijania, który można przeciągać do przewijania dodatkowe wskaźniki w widoku.

    ![AddMetricsScrollbar](./media/storage-monitor-storage-account/Storage_AddMetrics_Scrollbar.png)


5. Kliknij strzałkę w dół, metryki, aby rozwinąć listę operacji, które metryka jest ograniczone do uwzględnienia. Wybierz pozycję każdej operacji, które mają być wyświetlane w tabeli metryki [Azure Portal](https://portal.azure.com).

    Na poniższej ilustracji została rozszerzona metryki wartości procentowej błędu autoryzacji.

    ![ExpandCollapse](./media/storage-monitor-storage-account/Storage_AddMetrics_ExpandCollapse.png)


6. Po zaznaczeniu metryki dla wszystkich usług, kliknij przycisk OK (znacznik wyboru) aby zaktualizować konfiguracji monitorowania. Metryki zaznaczonego zostaną dodane do tabeli metryki.

7. Aby usunąć metryki z tabeli, kliknij pozycję metryczne, aby go zaznaczyć, a następnie kliknij **Usuń metryki**.

    ![DeleteMetric](./media/storage-monitor-storage-account/Storage_DeleteMetric.png)

## <a name="how-to-customize-the-metrics-chart-on-the-monitor-page"></a>Jak: Dostosowywanie wykresu metryki na stronie monitora

1. Na stronie **monitora** dla konta miejsca do magazynowania w tabeli metryki wybierz maksymalnie 6 metryki zostać przedstawione na wykresie metryki. Aby zaznaczyć metryki, kliknij pole wyboru po jego lewej stronie. Aby usunąć metryki z wykresu, wyczyść pole wyboru.

2. Aby przełączyć się na wykresie między odwołaniami względnymi wartości (końcowej wyświetlane tylko) oraz wartości bezwzględne (wyświetlane oś Y), wybierz **względne** i **bezwzględne** w górnej części wykresu.

3.  Aby zmienić czas zakresu Wyświetla wykres metryki, wybierz pozycję **6 godzin**, **24 godziny**lub **7 dni** w górnej części wykresu.



## <a name="how-to-configure-logging"></a>Jak: Konfigurowanie rejestrowania

Dla każdej z usług przechowywania dostępnych za pomocą konta miejsca do magazynowania (obiektów blob, tabel i kolejki) można zapisać dzienniki diagnostyczne dla żądania odczytu, zapisać żądania i/lub usuwanie żądania, a dla każdego z usług można ustawić zasady przechowywania danych.

1. W [Azure Portal](https://portal.azure.com)kliknij pozycję **Magazyn**, a następnie kliknij nazwę konta przestrzeni dyskowej, aby otworzyć pulpitu nawigacyjnego.

2. Kliknij przycisk **Konfiguruj**, a następnie przewiń w dół do **rejestrowania**za pomocą strzałki w dół na klawiaturze.

    ![Storagelogging](./media/storage-monitor-storage-account/Storage_LoggingOptions.png)


3. Dla każdej usługi (obiektów blob, tabel i kolejki) skonfiguruj następujące ustawienia:

    - Typy żądanie logowania: żądania odczytu, zapisać żądania i żądań Delete.

    - Liczba dni przechowywania danych zarejestrowanych. Wprowadzenie wartości zero są, jeśli nie chcesz ustawić zasady przechowywania. Jeśli nie ustawisz zasady przechowywania, to możesz usunąć pliki dziennika.

4. Kliknij przycisk **Zapisz**.

Dzienniki diagnostyczne są zapisywane w kontenerze obiektów blob o nazwie $logs na Twoim koncie miejsca do magazynowania. Aby dowiedzieć się, jak uzyskiwanie dostępu do kontenera $logs zobacz [Miejsca do magazynowania analizy rejestrowania — o](http://msdn.microsoft.com/library/azure/hh343262.aspx).
