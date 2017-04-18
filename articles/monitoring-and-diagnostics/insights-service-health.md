<properties
    pageTitle="Śledź kondycję usługi Azure za pomocą dzienników aktywności Monitor Azure | Microsoft Azure"
    description="Dowiedz się, gdy Azure napotkał wydajności przerwań obniżenie wydajności lub usługi. "
    authors="rboucher"
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
    ms.date="10/20/2016"
    ms.author="robb"/>

# <a name="track-azure-service-health-using-azure-monitor-activity-logs"></a>Śledź kondycję usługi Azure za pomocą dzienników aktywności Monitor Azure

Azure publicizes każdym razem, gdy jest przerwy lub wydajności obniżenie wydajności usługi. Można przeglądać te zdarzenia w portalu Azure, a także umożliwia [Interfejsu API usługi REST](https://msdn.microsoft.com/library/azure/dn931927.aspx) lub [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights/) programowy dostęp do pełnego zestawu zdarzeń.

## <a name="browse-the-service-health-logs-for-your-subscription"></a>Przeglądanie dzienniki kondycji usługi dla subskrypcji

1. Zaloguj się do [portalu Azure](https://portal.azure.com/).

2. W **domu** powinna być widoczna kafelka o nazwie **Kondycja usługi**. Kliknij na niej.

    ![Narzędzia główne](./media/insights-service-health/Insights_Home.png)

3. Zostanie wyświetlona lista wszystkich regionów w Azure. Kliknij dowolny region, aby wyświetlić kwerendę dziennika aktywności, która zawiera zdarzenia usługi, które mają wpływ dowolnego z subskrypcji w ciągu ostatnich 24 godzin.

    ![Kondycja usługi subskrypcji dziennika aktywności](./media/insights-service-health/AzureActivityLogServiceHealth3.png)

4. Aby wyświetlić szczegółowe informacje o pojedynczych zdarzenia, należy kliknąć to zdarzenie w tabeli.

5. Zmienianie **przedziału czasu** , aby wyświetlić okres dłuższy.

## <a name="next-steps"></a>Następne kroki

* [Monitor dostępności i czas reakcji dowolnej strony sieci web](../application-insights/app-insights-monitor-web-app-availability.md) przy użyciu aplikacji wniosków, więc można znaleźć w przypadku strony nie działa.
