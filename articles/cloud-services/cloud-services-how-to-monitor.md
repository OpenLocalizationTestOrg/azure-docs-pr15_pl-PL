<properties 
    pageTitle="Monitorowanie usługi w chmurze | Microsoft Azure" 
    description="Dowiedz się, jak monitorować usług w chmurze za pomocą portalu klasyczny Azure." 
    services="cloud-services" 
    documentationCenter="" 
    authors="rboucher" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/04/2015" 
    ms.author="robb"/>


# <a name="how-to-monitor-cloud-services"></a>Monitorowanie usług w chmurze

[AZURE.INCLUDE [disclaimer](../../includes/disclaimer.md)]

Można monitorować `key` metryki wydajności dla usługi cloud w portalu klasyczny Azure. Można ustawić poziom monitorowania minimalnego i pełne dla każdej usługi, a następnie można dostosować Wyświetla monitorowania. Pełne monitorowania dane są przechowywane na koncie miejsca do magazynowania, dostępnym spoza portalu. 

W portalu klasyczny Azure monitorowania Wyświetla są konfigurowane w wysoce. Możesz wybrać metryk, które chcesz monitorować na liście metryki na stronie **monitora** , a które metryki kreślenia na wykresach metryki na stronie **Monitor** i pulpitu nawigacyjnego można wybrać. 

## <a name="concepts"></a>Pojęcia

Domyślnie minimalnego monitorowania jest dostępne dla nowej usługi w chmurze za pomocą liczników wydajności zebranych systemu operacyjnego hosta wystąpień role (maszyn wirtualnych). Metryki minimalnego są ograniczone do CPU procentowa, dane w danych się, przepustowość odczytu dysków i przepustowość pisanie dysków. Konfigurując pełne monitorowania, można uzyskać dodatkowe metryki na podstawie danych wydajności w maszyn wirtualnych (roli wystąpień). Pełne metryki umożliwiają dokładniejsze analizy problemy występujące podczas operacji aplikacji.

Domyślnie dane licznika wydajności z roli wystąpień jest próbki i przekazywanego z wystąpienia ról w 3-minutowy. Po włączeniu pełne monitorowania, dane licznika wydajności nieprzetworzonych jest agregowane dla każdego wystąpienia roli i w jego wystąpieniach roli dla poszczególnych ról w odstępach 5 minut, 1 godzina i 12 godzin. Zagregowane dane zostanie usunięte po 10 dni.

Po włączeniu pełne monitorowania, zagregowane dane pochodzące z monitorowania są przechowywane w tabelach na koncie miejsca do magazynowania. Aby włączyć pełne monitorowania dla roli, należy skonfigurować parametry połączenia diagnostyki połączonego konta miejsca do magazynowania. Za pomocą kont innego miejsca do magazynowania dla różne role.

Należy zauważyć, że włączone pełne monitorowanie wzrośnie koszty magazynowania związane z magazynowanie danych, transfer danych i transakcje miejsca do magazynowania. Monitorowanie minimalnego nie wymaga konta miejsca do magazynowania. Dane dla miar, które są dostępne na poziomie monitorowania minimalnego nie są przechowywane na koncie miejsca do magazynowania, nawet jeśli ustawiono poziom monitorowania pełne.


## <a name="how-to-configure-monitoring-for-cloud-services"></a>Jak: Konfigurowanie monitorowania usług w chmurze

Skorzystaj z poniższych instrukcji, aby skonfigurować pełne lub minimalnego monitorowania w portalu klasycznym Azure. 

### <a name="before-you-begin"></a>Przed rozpoczęciem

- Utwórz konto miejsca do magazynowania do przechowywania danych monitorowania. Za pomocą kont innego miejsca do magazynowania dla różne role. Aby uzyskać więcej informacji, zobacz Pomoc dla **Kont miejsca do magazynowania**lub zobacz, [jak tworzenie konta miejsca do magazynowania](/manage/services/storage/how-to-create-a-storage-account/).

- Włącz diagnostyki Azure dla poszczególnych ról usługi w chmurze. Zobacz [Konfigurowanie informacje diagnostyczne dla usług w chmurze](https://msdn.microsoft.com/library/azure/dn186185.aspx#BK_EnableBefore).

Upewnij się, że parametry połączenia diagnostyki znajduje się w konfiguracji roli. Nie można włączyć pełne monitorowania, aż włączyć diagnostyki Azure i obejmować parametry połączenia diagnostyki konfigurację ról.   

> [AZURE.NOTE] Projekty kierowanie 2,5 SDK Azure nie zawiera automatycznie parametry połączenia diagnostyki w szablonie projektu. Projekty te należy ręcznie dodać parametry połączenia Diagnostyka konfiguracji roli.

**Aby ręcznie dodać parametry połączenia Diagnostyka konfiguracji ról**

1. Otwórz projekt usługi w chmurze w programie Visual Studio
2. Kliknij dwukrotnie od **roli** otworzyć projektanta roli, a następnie wybierz kartę **Ustawienia**
3. Sprawdź ustawienia o nazwie **Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString**. 
4. Jeśli nie jest to ustawienie następnie kliknij przycisk **Dodaj ustawienie** , aby dodać go do konfiguracji i zmień typ nowe ustawienie na **ConnectionString**
5. Ustaw wartość w polu Parametry połączenia, klikając przycisk **...** . Zostanie otwarte okno dialogowe umożliwiające wybranie konta miejsca do magazynowania.

    ![Ustawienia programu Visual Studio](./media/cloud-services-how-to-monitor/CloudServices_Monitor_VisualStudioDiagnosticsConnectionString.png)

### <a name="to-change-the-monitoring-level-to-verbose-or-minimal"></a>Aby zmienić poziom monitorowania pełne lub minimalnego

1. W [portalu klasyczny Azure](https://manage.windowsazure.com/)Otwórz stronę **Konfigurowanie** do wdrożenia usługi w chmurze.

2. W **poziomie**kliknij pozycję **pełne** lub **minimalnego**. 

3. Kliknij przycisk **Zapisz**.

Po włączeniu pełne monitorowania, możesz rozpocząć w ciągu godziny są wyświetlane dane pochodzące z monitorowania w portalu klasycznym Azure.

Dane licznika wydajności nieprzetworzonych i zagregowane dane monitorowania są przechowywane na koncie miejsca do magazynowania w tabelach kwalifikowanym według identyfikatorów wdrożenia dla ról. 

## <a name="how-to-receive-alerts-for-cloud-service-metrics"></a>Jak: otrzymywanie alertów metryki usługi w chmurze

Możesz otrzymywać alerty oparte na usługi cloud monitorowania metryki. Na stronie **Usługi zarządzania** portalu klasyczny Azure można utworzyć reguły w celu uruchomienia alertu Metryka wybranym osiągnie wartość. Możesz również wybrać ma wiadomości e-mail wysłanych wyzwolenia alertu. Aby uzyskać więcej informacji, zobacz [jak: odbieranie alertów i zarządzanie regułami alertów platformy Azure](http://go.microsoft.com/fwlink/?LinkId=309356).

## <a name="how-to-add-metrics-to-the-metrics-table"></a>Jak: Dodawanie metryki do tabeli metryki

1. W [portalu klasyczny Azure](http://manage.windowsazure.com/)Otwórz stronę **Monitor** usług w chmurze.

    Domyślnie tabela metryki wyświetla podzbiór dostępne wskaźniki. Na ilustracji przedstawiono metryki pełne domyślne dla usługi w chmurze, które jest ograniczona do licznika wydajności MB Pamięć\Dostępne z danymi zagregowane na poziomie roli. **Dodawanie metryki** umożliwia wybór dodatkowych metryki agregujących i poziomie roli monitorowanie w portalu klasyczny Azure.

    ![Pełny ekran](./media/cloud-services-how-to-monitor/CloudServices_DefaultVerboseDisplay.png)
 
2. Aby dodać metryki do tabeli metryki:

    1. Kliknij przycisk **Dodaj metryki** otworzyć **Metryki wybierz**, jak pokazano poniżej.

        Pierwszy metryki dostępne jest rozwinięty w celu wyświetlenia dostępnych opcji. Dla każdego metryki górny opcja wyświetla zagregowane dane z monitorowania dla wszystkich ról. Ponadto możesz wybrać poszczególnych ról w celu wyświetlenia danych dla.

        ![Dodawanie wskaźników](./media/cloud-services-how-to-monitor/CloudServices_AddMetrics.png)

    2. Aby zaznaczyć metryki do wyświetlenia

        - Kliknij strzałkę w dół, metryki, aby rozwinąć opcje monitorowania.
        - Zaznacz pole wyboru dla każdej opcji monitorowania, które mają być wyświetlane.

        Można wyświetlić maksymalnie 50 metryki w tabeli metryki.

        > [AZURE.TIP] Pełne monitorowania lista metryki może zawierać dziesiątki metryki. Aby wyświetlić pasek przewijania, umieść wskaźnik na prawej części okna dialogowego. Aby filtrować listę, kliknij ikonę Wyszukaj i wprowadź tekst w polu wyszukiwania, tak jak pokazano poniżej.
    
        ![Dodawanie wyszukiwania metryk](./media/cloud-services-how-to-monitor/CloudServices_AddMetrics_Search.png)


3. Po wybraniu metryki kliknij przycisk OK (znacznik wyboru).

    Metryki zaznaczonego zostaną dodane do tabeli metryki, tak jak pokazano poniżej.

    ![metryki monitora](./media/cloud-services-how-to-monitor/CloudServices_Monitor_UpdatedMetrics.png)

 
4. Aby usunąć metryki z tabeli metryki, kliknij pozycję metryczne, aby go zaznaczyć, a następnie kliknij **Usunąć metryki**. (Możesz tylko zobacz **Usuwanie metryki** w przypadku gdy dostępne metryki zaznaczone).

### <a name="to-add-custom-metrics-to-the-metrics-table"></a>Aby dodać metryki niestandardowej do tabeli metryki

**Pełne** monitorowania poziom zawiera listę miar domyślne, które można monitorować w portalu. Oprócz tych można monitorować wszelkie niestandardowe metryki lub liczników wydajności zdefiniowanych przez aplikację za pośrednictwem portalu.

Następujące czynności przyjęto założenie, że zostały włączone **pełne** monitorowania poziomu i masz Konfigurowanie aplikacji do gromadzenia i przełączanie liczniki wydajności niestandardowe. 

Aby wyświetlić liczniki wydajności niestandardowe w portalu musisz zaktualizować konfigurację w wad sterowania kontenera:
 
1. Otwórz blob wad sterowania kontenera na koncie diagnostyki miejsca do magazynowania. Za pomocą programu Visual Studio lub innych Eksplorator magazynu w tym celu.

    ![Eksplorator serwera programu Visual Studio](./media/cloud-services-how-to-monitor/CloudServices_Monitor_VisualStudioBlobExplorer.png)

2. Przejdź ścieżkę obiektów blob, korzystając ze wzorcem **RoleInstance-DeploymentId-RoleName** odnaleźć konfiguracji wystąpienia roli. 

    ![Eksplorator magazynu programu Visual Studio](./media/cloud-services-how-to-monitor/CloudServices_Monitor_VisualStudioStorage.png)
3. Pobierz plik konfiguracji wystąpienia roli i zaktualizować go, aby uwzględnić wszystkie liczniki wydajności niestandardowe. Na przykład monitorować *zapisu bajtów/sec* *dysk C* Dodaj następujący węźle **PerformanceCounters\Subscriptions**

    ```xml
    <PerformanceCounterConfiguration>
    <CounterSpecifier>\LogicalDisk(C:)\Disk Write Bytes/sec</CounterSpecifier>
    <SampleRateInSeconds>180</SampleRateInSeconds>
    </PerformanceCounterConfiguration>
    ```
4. Zapisanie zmian i przekaż plik konfiguracji powrót do tej samej lokalizacji zastąpienie istniejącego pliku w obiekcie blob.
5. Przełącz do trybu pełnego w klasycznym Azure Konfiguracja portalu. Gdyby w trybie pełnym już masz przełączyć się do minimalnego i z powrotem na pełne.
6. Niestandardowe licznika będą dostępne obecnie w oknie dialogowym **Dodawanie metryki** . 

## <a name="how-to-customize-the-metrics-chart"></a>Jak: Dostosowywanie wykresu metryki

1. W tabeli metryki wybierz maksymalnie 6 metryki zostać przedstawione na wykresie metryki. Aby zaznaczyć metryki, kliknij pole wyboru po jego lewej stronie. Aby usunąć metryki z wykresu metryki, wyczyść jego pole wyboru w tabeli metryki.

    Po wybraniu metryki w tabeli metryki metryki zostaną dodane do wykresu metryki. Na ekranie wąskie listy rozwijanej **n więcej** zawiera nagłówki metryczne, które nie mieści się wyświetlania.

 
2. Aby przełączyć się między wyświetlaniem wartości względne (końcowej tylko dla każdej jednostki metryczne) i wartości bezwzględnej (oś Y wyświetlane), wybierz względną lub bezwzględną w górnej części wykresu.

    ![Względne i bezwzględne](./media/cloud-services-how-to-monitor/CloudServices_Monitor_RelativeAbsolute.png)

3. Aby zmienić zakres czasu wyświetla wykres metryki, wybierz 1 godzina 24 godziny lub 7 dni w górnej części wykresu.

    ![Okres ekranu monitora](./media/cloud-services-how-to-monitor/CloudServices_Monitor_DisplayPeriod.png)

    Na wykresie metryki pulpitu nawigacyjnego metody wykreślanie metryki różni się. Standardowy zestaw metryki jest dostępna, a metryki są dodawane lub usuwane, wybierając pozycję metryczne nagłówka.

### <a name="to-customize-the-metrics-chart-on-the-dashboard"></a>Aby dostosować wykres metryki na pulpicie nawigacyjnym

1. Otwórz pulpit nawigacyjny do usługi w chmurze.

2. Dodawanie lub usuwanie metryki z wykresu:

    - Kreślenia nowej jednostki metryczne, zaznacz pole wyboru metryki w nagłówkach wykresu. Na ekranie wąskie kliknij strzałkę w dół według ** *n*??metrics** kreślenia metryczne, których nie można wyświetlić obszar nagłówka wykresu.

    - Aby usunąć metryki wykreślone na wykresie, wyczyść pole wyboru, jej nagłówek.

3. Przełączanie między **odwołaniami względnymi** i **bezwzględnymi** Wyświetla.

4. Wybierz pozycję 1 godzina 24 godziny lub 7 dni danych do wyświetlenia.

## <a name="how-to-access-verbose-monitoring-data-outside-the-azure-classic-portal"></a>Jak: pełne, monitorowanie danych spoza Azure portalu klasycznego programu Access

Pełne monitorowania dane są przechowywane w tabelach w oknie konta miejsca do magazynowania, określające dla poszczególnych ról. Dla każdego rozmieszczenia usługi cloud sześć tabele są tworzone dla roli. Dwie tabele są tworzone dla każdej z nich (5 minut, 1 godzina i 12 godzin). Jedną z poniższych tabelach przechowuje agregacji poziomie roli; drugiej tabeli przechowuje agregacji dla roli wystąpień. 

Nazwy tabel mają następujący format:

```
WAD*deploymentID*PT*aggregation_interval*[R|RI]Table
```

w przypadku gdy:

- Identyfikator GUID przypisane do wdrożenia usługi cloud jest *deploymentID*

- *aggregation_interval* = 5 M, 1 H lub 12 H

- agregacje poziomie roli = R

- funkcji agregacji dla roli wystąpienia = RI

Na przykład następujących tabel chcesz przechowywać pełne dane monitorowania zagregowane w odstępach 1-godzinnym:

```
WAD8b7c4233802442b494d0cc9eb9d8dd9fPT1HRTable (hourly aggregations for the role)

WAD8b7c4233802442b494d0cc9eb9d8dd9fPT1HRITable (hourly aggregations for role instances)
```
