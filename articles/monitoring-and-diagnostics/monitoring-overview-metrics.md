<properties
    pageTitle="Omówienie metryki platformy Microsoft Azure | Microsoft Azure"
    description="Omówienie wskaźników i ich zastosowania w programie Microsoft Azure"
    authors="kamathashwin"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="ashwink"/>

# <a name="overview-of-metrics-in-microsoft-azure"></a>Omówienie metryki platformy Microsoft Azure 

W tym artykule opisano, co to są w Microsoft Azure metryki korzyści i jak rozpocząć korzystanie z nich.  

## <a name="what-are-metrics"></a>Co to są metryki?

Azure Monitor umożliwia używanie telemetrycznego uzyskanie wgląd wydajności i kondycji usługi obciążenia Azure. Najważniejsze typ danych telemetrycznych Azure są metryki (nazywanej też liczniki wydajności) wysyłane przez wszystkie zasoby najbardziej Azure. Monitorowanie Azure udostępnia kilka sposobów Konfigurowanie i używanie tych metryki dla monitorowania i rozwiązywania problemów.


## <a name="what-can-you-do-with-metrics"></a>Co można robić przy użyciu metryki?

Wskaźniki są przydatne źródło telemetrycznego i umożliwiają wykonywanie następujących czynności:

- **Śledzenie wydajności** zasobu (na przykład maszyn wirtualnych, witryny internetowej lub aplikacji logiczny) wykreślanie jego metryki na wykresie portalu i przypięcie takiego wykresu do pulpitu nawigacyjnego.
- **Otrzymywanie powiadomień o problemu** wpływ na wydajność zasobu gdy metryki przecina określonej wartości progowej.
- **Konfigurowanie automatycznego akcji**, takich jak autoscaling zasób lub kwerend działań aranżacji, gdy przecina metryki określonej wartości progowej.
- **Wykonywanie zaawansowanych analiz** lub zgłaszanie wydajności i użycia tendencji do zasobów.
- **Archiwum** wydajności i kondycji historii wymaganiom **zgodności i inspekcji** zasobów.

##  <a name="metric-characteristics"></a>Metryka właściwości
Metryki mają następujące cechy:

- Wszystkie metryki mają **częstotliwości 1 minuty**. Otrzymasz wartość metryki co minutę od zasobu, które zapewnia w czasie rzeczywistym wgląd w stanie i kondycji zasobu.
- Metryki są **dostępne z gotowych do bez potrzeby korzystania z** lub konfigurowania dodatkowych opcji diagnostycznych.
- Masz dostęp do **30 dni historii** dla każdego metryki. Szybkie zapoznanie się z ostatnich i miesięcznych trendów w wydajności i kondycji zasobu.

Można:

- Łatwe odnajdowanie, dostęp i **wyświetlić wszystkie metryki** przez portal Azure po wybraniu zasobu i ich przedstawione na wykresie. 
- Skonfigurować metryczne **alertu powiadomi albo automatycznego działania** , gdy przecina Metryka progu ustawionych. Autoscale jest specjalne akcję automatyczną, która umożliwia skalowania zasób spełnia przychodzących żądań lub załadować w witrynie sieci web lub obliczyć zasobów. Można skonfigurować regułę ustawienie Autoscale przeskalować w nowym lub się według metryki przekroczenia progu.
- Metryki **archiwum** dłużej lub używania ich do raportowania w trybie offline. Możesz rozesłać z blob miejsca do magazynowania, podczas konfigurowania ustawień diagnostycznych dla zasobu z danymi.
- Metryki **strumienia** do koncentratora zdarzenia, umożliwiając kierowanie ich do analizy strumieniu Azure lub niestandardowe aplikacje do czasu u rzeczywistego analizy. Można wykonać przy użyciu ustawień diagnostycznych.
- **Trasa** wszystkie metryki do analizy dziennika usługi OMS Aby odblokować błyskawiczne analizy, wyszukiwania i alerty niestandardowe metryki danych od zasobów.
- **Consume** metryki za pomocą nowego API pozostałych Monitor Azure.
- Metryki **kwerendy** przy użyciu poleceń cmdlet środowiska PowerShell lub interfejsu API usługi REST między platformami.

 ![Kierowanie miar w monitorze Azure](./media/monitoring-overview-metrics/MetricsOverview0.png)

## <a name="access-metrics-via-portal"></a>Metryki dostęp za pośrednictwem portalu
Oto krótki Instruktaż tworzenia wykresu metryczne za pośrednictwem portalu Azure

### <a name="view-metrics-after-creating-a-resource"></a>Metryki widoku po utworzeniu zasobu
1. Otwórz portal Azure
2. Tworzenie nowej usługi aplikacji - witryny sieci Web.
3. Po utworzeniu witryny sieci Web, przejdź do karta Przegląd witryny sieci web.
4. Można wyświetlić nowych metryk jako kafelka "Monitorowanie". Można edytować fragmentu i wybierz pozycję więcej metryki

 ![Metryki na zasób w monitorze Azure](./media/monitoring-overview-metrics/MetricsOverview1.png)    

### <a name="access-all-metrics-in-a-single-place"></a>Dostęp do wszystkich metryki w jednym miejscu
1. Otwórz Azure portal 
2. Przejdź do nowej karcie Monitor i wybierz opcję metryki w obszarze go 
3. Twoja subskrypcja, grupa zasobów i nazwę zasobu można wybierać z listy rozwijanej. 
4. Teraz można wyświetlać na liście dostępne wskaźniki. 
5. Wybierz pozycję metryczne interesuje i kreślenia go. 
6. Można przypiąć go do pulpitu nawigacyjnego, klikając pozycję na pin w prawym górnym rogu.

 ![Dostęp do wszystkich metryki w jednym miejscu w monitorze Azure](./media/monitoring-overview-metrics/MetricsOverview2.png) 


>[AZURE.NOTE] Użytkownik ma dostęp metryki poziomie hosta z maszyny wirtualne (podstawie Menedżer zasobów Azure) i zestawów skali maszyn wirtualnych bez wszelkie dodatkowe ustawienia diagnostyczne. Tych nowych metryk poziomie hosta są dostępne dla systemu Windows i Linux. Te wskaźniki są nie należy mylić z metryki poziomu system operacyjny gościa, które mają dostęp do po włączeniu narzędzia diagnostyczne Azure na maszyny wirtualne lub VMSS. Aby dowiedzieć się więcej o konfigurowaniu diagnostyki Azure, zobacz [Co to jest Diagnostyka pakietu Microsoft Azure](../azure-diagnostics.md).

## <a name="access-metrics-via-rest-api"></a>Metryki dostępu za pośrednictwem interfejsu API usługi REST
Metryki Azure są dostępne za pośrednictwem interfejsów API Monitor Azure. Istnieją dwa interfejsy API, które ułatwiają odnajdowanie i uzyskać dostęp do metryki. Używanie: 

- [Metryka Monitor azure definicji interfejsu API usługi REST](https://msdn.microsoft.com/library/mt743621.aspx) dostępu do listy metryki dostępne usługi.
- [Interfejsu API usługi REST metryki Monitor azure](https://msdn.microsoft.com/library/mt743622.aspx) uzyskiwać dostęp do danych metryki rzeczywista

>[AZURE.NOTE] W tym artykule opisano, jak metryki przy użyciu [nowego interfejsu API metryki](https://msdn.microsoft.com/library/dn931930.aspx) Azure zasobów. Wersja interfejsu API dostępność nowych definicji metryki interfejsu API jest 2016-03-01 a wersji dla metryki interfejsu API jest 2016-09-01. Starsze definicje metryczne i metryki jest możliwy przy użyciu wersji interfejsu api 2014-04-01.

Aby uzyskać bardziej szczegółowe instrukcje za pomocą interfejsów API pozostałych Monitor Azure zobacz [Instruktaż interfejsu API pozostałych Monitor Azure](monitoring-rest-api-walkthrough.md).

## <a name="export-options-for-metrics"></a>Opcje metryki eksportu
Można przejdź do karta dzienniki diagnostyczne na karcie Monitor i wyświetlić opcje eksportowania dla miar. Można wybrać metryki (i dzienniki diagnostyczne) przesłana magazyn obiektów Blob koncentratory zdarzenie lub analizy dziennika usługi OMS dla przypadków użycia wspomniano wcześniej w tym artykule. 

 ![Opcje eksportowania metryki w monitorze Azure](./media/monitoring-overview-metrics/MetricsOverview3.png)   

Można to skonfigurować przy użyciu Menedżera zasobów szablony [programu PowerShell](insights-powershell-samples.md), [polecenie](insights-cli-samples.md) lub [Interfejsy API pozostałych](https://msdn.microsoft.com/library/dn931943.aspx). 

## <a name="take-action-on-metrics"></a>Wykonywanie akcji na metryki
Możesz otrzymywać powiadomienia lub wykonać akcję automatyczną na dane. Możesz skonfigurować reguły alertów lub ustawienia Autoscale to zrobić.

### <a name="alert-rules"></a>Reguły alertów
Na metryki można skonfigurować reguły alertów. Jeśli metryki występują skrzyżowane określonej wartości progowej i powiadomienia pocztą e-mail i uruchomienie webhook, które można wykonać dowolną skryptu niestandardowego, można sprawdzić tych reguł alertów. Za pomocą webhook do konfigurowania 3 integracji produktu.

 ![Metryki i reguł alertów w monitorze Azure](./media/monitoring-overview-metrics/MetricsOverview4.png)

### <a name="autoscale"></a>Autoscale
Niektóre Azure zasoby pomocy technicznej wielu wystąpień przeskalować się lub na obsługę usługi obciążenia. Autoscale dotyczy aplikacji usług (aplikacje sieci Web), zestawy skali maszyn wirtualnych (VMSS) i klasyczny usług w chmurze. Można skonfigurować reguły autoscale przeskalować się lub w przy niektórych metryki wpływające na ochronę z pracą przecina progu zadanej. Aby uzyskać więcej informacji zobacz [Omówienie autoscaling](monitoring-overview-autoscale.md).

 ![Metryki i autoscale w monitorze Azure](./media/monitoring-overview-metrics/MetricsOverview5.png)

## <a name="supported-services-and-metrics"></a>Obsługiwane usługi i wskaźniki
Azure Monitor jest nową infrastrukturę metryki. Następujące usługi Azure w Azure portal i nowa wersja interfejsu API Monitor Azure go zapewnia obsługę:

- Maszyny wirtualne (podstawie Menedżer zasobów Azure)
- Zestawy skali maszyn wirtualnych
- Partii
- Przestrzeń nazw Centrum zdarzenia 
- Przestrzeń nazw Bus usługi (tylko premium SKU)
- SQL (wersja 12)
- Pula elastyczne SQL
- Witryny sieci Web
- Farmy serwerów sieci Web
- Aplikacje warunków logicznych
- Koncentratory IoT
- Redis pamięci podręcznej
- Sieci: Bram aplikacji
- Wyszukiwanie

Można wyświetlić szczegółowy wykaz wszystkich obsługiwanych usług i ich metryki na [monitorze Azure metryki - obsługiwane metryki na typ zasobu](monitoring-supported-metrics.md). 


## <a name="next-steps"></a>Następne kroki

Skorzystaj z łączy w tym artykule. Ponadto informacje:  

- informacje o [typowych metryki dla autoscaling](insights-autoscale-common-metrics.md)
- jak [tworzyć reguły alertów](insights-alerts-portal.md)




