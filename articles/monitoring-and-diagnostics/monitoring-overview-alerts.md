<properties
    pageTitle="Omówienie alertów w programie Microsoft Azure | Microsoft Azure"
    description="Alerty umożliwiają monitorowanie metryki Azure zasobów, wydarzenia lub dzienniki i otrzymywać powiadomienia o spełnienia warunku przez użytkownika."
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
    ms.date="09/24/2016"
    ms.author="robb"/>

# <a name="overview-of-alerts-in-microsoft-azure"></a>Omówienie alertów w programie Microsoft Azure


Ten artykuł zawiera opis alertów, jakie są ich korzyści i jak wprowadzenie do korzystania z nich.  

## <a name="what-are-alerts"></a>Co to są alerty?
Alerty są metody monitorowania metryki Azure zasobów, zdarzeń, lub dzienniki i następnie będą powiadamiane o zadanej spełnienia warunku.

Możesz otrzymywać alerty na podstawie:

- **Wartości metryki**: ten alert uaktywnia wartość określonej metryki przecina próg przypisywanie w dowolnym kierunku. Oznacza to, że wyzwalane oba po spełnienia warunku, a następnie później gdy warunek jest już nie są spełnione.
- **Zdarzeń dziennika aktywności**: ten alert może spowodować na każde zdarzenie lub tylko w przypadku, gdy występują pewne zdarzenia.

## <a name="alerts-in-different-azure-services"></a>Alerty w różnych usługach Azure

Alerty są dostępne w różnych usługach, takich jak:

- **Aplikacja wniosków**: umożliwia web testowych i jednostki metryczne alerty. Zobacz [Ustawianie alertów w aplikacji wniosków](../application-insights/app-insights-alerts.md) i [Monitor dostępności i czas reakcji dowolnej witryny sieci Web](../application-insights/app-insights-monitor-web-app-availability.md).
- **Analizy dziennika (pakiet zarządzania operacje)**: umożliwia routing dzienniki diagnostyczne w celu analizy dziennika. Pakiet administracyjny operacje umożliwia metryczne, dziennik i inne typy alertów. Aby uzyskać więcej informacji zobacz [alerty w dzienniku analizy](../log-analytics/log-analytics-alerts.md).  
- **Monitorowanie Azure**: umożliwia alertów na podstawie zarówno metryczne i zdarzeń dziennika aktywności. Azure Monitor zawiera [Interfejsu API usługi REST Monitor Azure](https://msdn.microsoft.com/library/dn931943.aspx).  Aby uzyskać więcej informacji zobacz [Używanie Azure portal, programu PowerShell lub interfejsu wiersza polecenia do tworzenia alertów](insights-alerts-portal.md).

## <a name="alert-actions"></a>Akcje alertu
Możesz skonfigurować alert, wykonaj następujące czynności:

- Wysyłanie powiadomienia e-mail, administrator usługi, administratorów współpracujących lub kolejnych adresów e-mail przez użytkownika.
- Zadzwoń do webhook, który umożliwia uruchamianie automatyzacji dodatkowe akcje.

 ![Omówienie alertów](./media/monitoring-overview-alerts/AlertsOverviewResource3.png)


## <a name="next-steps"></a>Następne kroki

Uzyskiwanie informacji na temat reguł alertów i konfigurując je przy użyciu:

- [Azure portal](insights-alerts-portal.md)
- [Programu PowerShell](insights-alerts-powershell.md)
- [Interfejs wiersza polecenia (polecenie)](insights-alerts-command-line-interface.md)
- [Monitorowanie Azure interfejsu API usługi REST](https://msdn.microsoft.com/library/azure/dn931945.aspx)
