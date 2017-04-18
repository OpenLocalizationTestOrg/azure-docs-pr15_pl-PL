<properties
   pageTitle="Monitorowanie operacji, zdarzeń i liczniki dla równoważenia obciążenia | Microsoft Azure"
   description="Dowiedz się, jak włączyć zdarzenia alertów i sondy rejestrowanie stanu kondycji usługi równoważenia obciążenia Azure"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn"
   tags="azure-resource-manager"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />

# <a name="log-analytics-for-azure-load-balancer-preview"></a>Dziennik analizy Azure równoważenia obciążenia (wersja Preview)

Różne typy dzienników Azure umożliwia zarządzanie i rozwiązywanie problemów z urządzenia do równoważenia obciążenia. Niektóre z tych dzienników są dostępne za pośrednictwem portalu. Wszystkie dzienniki można wyodrębnionych z magazynem obiektów blob Azure i wyświetlać na różnych narzędzi, takich jak program Excel i PowerBI. Więcej informacji o różnych typów dzienników z poniższej listy można znaleźć.

- **Dzienników inspekcji:** Za pomocą [Dzienników inspekcji Azure](../../articles/monitoring-and-diagnostics/insights-debugging-with-events.md) (wcześniej nazywanego dzienniki operacyjne) aby wyświetlić wszystkie operacje przesyłany do subskrypcjach Azure i ich stanu. Dzienniki inspekcji są domyślnie włączone i mogą być wyświetlane w portalu Azure.
- **Alert dzienniki zdarzeń:** Dziennik służy do wyświetlania, jakie alertów dla równoważenia obciążenia są podniesione. Stan usługi równoważenia obciążenia są zbierane co pięć minut. Dziennik jest zapisać tylko jeśli zdarzenie alert równoważenia obciążenia zostanie zaokrąglona.
- **Dzienniki sondy kondycji:** Za pomocą tego dziennika do wyszukania sondy wyboru stanie, ile wystąpień są w trybie online w wewnętrznej równoważenia obciążenia i procent maszyn wirtualnych odbieraniem sieci danych z usługi równoważenia obciążenia. Dziennik jest zapisywany na zmienianie wydarzenia stanu sondy.

>[AZURE.IMPORTANT] Zaloguj się analizy obecnie działa tylko w przypadku dostępnych urządzenia do równoważenia obciążenia przez Internet. Dzienniki są dostępne tylko dla zasobów wdrożony w modelu wdrożenia Menedżera zasobów. Nie można użyć dzienników zasobów w modelu Klasyczny wdrożenia. Aby uzyskać więcej informacji o modelach wdrażania zobacz [wdrożenia opis Menedżera zasobów i klasyczny wdrożenia](../../articles/resource-manager-deployment-model.md).

## <a name="enable-logging"></a>Włączanie rejestrowania

Rejestrowanie inspekcji jest automatycznie włączana dla każdego zasobu Menedżera zasobów. Musisz włączyć zdarzenia i kondycji sondy rejestrowanie, aby rozpocząć pobieranie danych dostępne za pośrednictwem tych dzienników. Aby włączyć rejestrowanie, wykonaj następujące czynności.

Zaloguj się do [portalu Azure](http://portal.azure.com). Jeśli nie masz jeszcze równoważenia obciążenia, przed kontynuowaniem [Utwórz równoważenia obciążenia](load-balancer-get-started-internet-arm-ps.md) .

1. W portalu kliknij przycisk **Przeglądaj**.
2. Wybierz pozycję **urządzenia do równoważenia obciążenia**.

    ![Portal - równoważenia obciążenia](./media/load-balancer-monitor-log/load-balancer-browse.png)

3. Zaznacz istniejący równoważenia obciążenia >> **Wszystkie ustawienia**.
4. Po prawej stronie okna dialogowego pod nazwą równoważenia obciążenia przewiń do **monitorowania**, kliknij pozycję **Diagnostyka**.

    ![Portal — ustawień w przypadku usługi równoważenia obciążenia](./media/load-balancer-monitor-log/load-balancer-settings.png)

5. W okienku **diagnostyki** w obszarze **stanu**wybierz pozycję **na**.
6. Kliknij **konto, miejsca do magazynowania**.
7. W obszarze **DZIENNIKI**zaznacz istniejącego konta miejsca do magazynowania, lub Utwórz nowy. Aby ustalić, ile dni zdarzenia dat będą przechowywane w dzienniku zdarzeń za pomocą suwaka. 8. Kliknij przycisk **Zapisz**.

    ![Portal - dzienniki diagnostyczne](./media/load-balancer-monitor-log/load-balancer-diagnostics.png)

>[AZURE.INFORMATION] dzienników inspekcji nie wymaga konta oddzielnie. Korzystanie z miejsca do magazynowania dla zdarzenia i kondycji rejestrowanie sondy będzie powodowało opłat za usługi.

## <a name="audit-log"></a>Dziennik inspekcji

Dziennik inspekcji jest generowany domyślnie. Dzienniki są zachowywane przez 90 dni w sklepie dzienniki zdarzeń w Azure. Więcej informacji o tych dzienników, czytając artykuł [Wyświetlanie zdarzeń i dzienników inspekcji](../../articles/monitoring-and-diagnostics/insights-debugging-with-events.md) .

## <a name="alert-event-log"></a>Alert dziennika zdarzeń

Dziennik jest generowana tylko, jeśli został włączony w na podstawie równoważenia obciążenia. Zdarzenia są rejestrowane w formacie JSON i przechowywane w określonym przez Ciebie podczas włączone rejestrowanie konta miejsca do magazynowania. Poniżej przedstawiono przykład zdarzenia.

    {
    "time": "2016-01-26T10:37:46.6024215Z",
    "systemId": "32077926-b9c4-42fb-94c1-762e528b5b27",
    "category": "LoadBalancerAlertEvent",
    "resourceId": "/SUBSCRIPTIONS/XXXXXXXXXXXXXXXXX-XXXX-XXXX-XXXXXXXXX/RESOURCEGROUPS/RG7/PROVIDERS/MICROSOFT.NETWORK/LOADBALANCERS/WWEBLB",
    "operationName": "LoadBalancerProbeHealthStatus",
    "properties": {
        "eventName": "Resource Limits Hit",
        "eventDescription": "Ports exhausted",
        "eventProperties": {
            "public ip address": "40.117.227.32"
        }
    }

Wynik JSON zawiera właściwość *eventname* , które będą opisują powód równoważenia obciążenia utworzenia alertu. W tym przypadku alert wygenerowane było z powodu wyczerpania port TCP spowodowane przez źródło translatora adresów IP limity (SNAT).

## <a name="health-probe-log"></a>Kondycja sondy dziennika

Dziennik jest generowana tylko, jeśli został włączony w na ładowanie równoważenia podstawa określonych powyżej. Dane są przechowywane w określonym przez Ciebie podczas włączone rejestrowanie rachunku miejsca do magazynowania. Kontener o nazwie "wniosków dzienniki loadbalancerprobehealthstatus" jest tworzona i są rejestrowane poniższe dane:

    {
        "records":
        {
            "time": "2016-01-26T10:37:46.6024215Z",
            "systemId": "32077926-b9c4-42fb-94c1-762e528b5b27",
            "category": "LoadBalancerProbeHealthStatus",
            "resourceId": "/SUBSCRIPTIONS/XXXXXXXXXXXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXX/RESOURCEGROUPS/RG7/PROVIDERS/MICROSOFT.NETWORK/LOADBALANCERS/WWEBLB",
            "operationName": "LoadBalancerProbeHealthStatus",
            "properties": {
                "publicIpAddress": "40.83.190.158",
                "port": "81",
                "totalDipCount": 2,
                "dipDownCount": 1,
                "healthPercentage": 50.000000
            }
        },
        {
            "time": "2016-01-26T10:37:46.6024215Z",
            "systemId": "32077926-b9c4-42fb-94c1-762e528b5b27",
            "category": "LoadBalancerProbeHealthStatus",
            "resourceId": "/SUBSCRIPTIONS/XXXXXXXXXXXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXX/RESOURCEGROUPS/RG7/PROVIDERS/MICROSOFT.NETWORK/LOADBALANCERS/WWEBLB",
            "operationName": "LoadBalancerProbeHealthStatus",
            "properties": {
                "publicIpAddress": "40.83.190.158",
                "port": "81",
                "totalDipCount": 2,
                "dipDownCount": 0,
                "healthPercentage": 100.000000
            }
        }
    }

Wynik JSON w polu właściwości zawiera podstawowe informacje dotyczące stanu sondy. Właściwość *dipDownCount* zawiera całkowitą liczbę wystąpień wewnętrznej, które nie są odbierane ruchu sieciowego z powodu odpowiedzi sondy nie powiodło się.

## <a name="view-and-analyze-the-audit-log"></a>Wyświetlanie i analizowanie dziennika inspekcji

Można wyświetlać i analizowanie danych dziennika inspekcji przy użyciu jednej z następujących metod:

- **Azure narzędzia:** Pobieraj informacje z dzienników inspekcji za pośrednictwem programu PowerShell Azure, interfejs wiersza polecenia Azure (polecenie), interfejsu API usługi REST Azure lub portal Azure preview. Instrukcje krok po kroku dla każdej metody wyszczególniono w artykule [inspekcji operacji przy użyciu Menedżera zasobów](../../articles/resource-group-audit.md) .
- **Power BI:** Jeśli nie masz konta [Usługi Power BI](https://powerbi.microsoft.com/pricing) , możesz spróbować je bezpłatnie. Za pomocą [dzienników inspekcji Azure zawartości pakiet dla usługi Power BI](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-audit-logs), podczas analizowania danych za pomocą wstępnie skonfigurowane pulpity nawigacyjne lub można dostosować widoków do własnych potrzeb.

## <a name="view-and-analyze-the-health-probe-and-event-log"></a>Wyświetlanie i analizowanie kondycji sondy i dziennika zdarzeń

Musisz połączyć się z kontem miejsca do magazynowania i pobieranie JSON pozycje dziennika zdarzeń i kondycji dzienniki sondy. Po pobraniu plików JSON, możesz je przekonwertować CSV i widoku w programie Excel, PowerBI lub innego narzędzia do wizualizacji danych.

>[AZURE.TIP] Użytkownicy zaznajomieni z programu Visual Studio i podstawowe pojęcia zmianę wartości stałych i zmiennych w języku C#, można użyć [Narzędzia konwertera dziennika](https://github.com/Azure-Samples/networking-dotnet-log-converter) dostępnych w Github.

## <a name="additional-resources"></a>Dodatkowe zasoby

- Wpis w blogu [Wizualizacja Azure dzienników inspekcji dzięki usłudze Power BI](http://blogs.msdn.com/b/powerbi/archive/2015/09/30/monitor-azure-audit-logs-with-power-bi.aspx) .
- [Wyświetlanie i analizowanie Azure dzienników inspekcji w usłudze Power BI i innych](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/) wpis w blogu.

## <a name="next-steps"></a>Następne kroki

[Opis sondy równoważenia obciążenia](load-balancer-custom-probe-overview.md)
