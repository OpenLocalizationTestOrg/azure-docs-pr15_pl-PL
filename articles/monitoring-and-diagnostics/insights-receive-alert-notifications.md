<properties
    pageTitle="Odbieranie alertów dla usług Azure | Microsoft Azure"
    description="Otrzymywać powiadomienia, gdy zostaną spełnione warunki reguły alertów."
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
    ms.date="09/08/2015"
    ms.author="robb"/>

# <a name="receive-alert-notifications"></a>Odbieranie alertów

Możesz otrzymywać alertu na podstawie monitorowania metryki dla lub zdarzeniami na usługi Azure.

Dla reguły alertu na wartość metryki gdy wartość określonej metryki przecina progu przydzielone, reguły alertu z aktywnym i wysyłać powiadomienie. Dla reguły alertu o zdarzeniach reguły można wysłać powiadomienie na *każde* zdarzenie lub, tylko wtedy, gdy liczba zdarzeń wystąpić.

Po utworzeniu reguły alertu, można wybrać opcje do wysyłania wiadomości e-mail z powiadomieniem do administratora usługi i administratorów współpracujących lub do innego administratora, który można określić. Wiadomość e-mail z powiadomieniem są wysyłane, gdy reguła stanie się aktywny, a warunek alert został rozwiązany.

Za pomocą [Interfejsu API usługi REST](https://msdn.microsoft.com/library/azure/dn931945.aspx) lub [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights/) do konfigurowania i programowy uzyskiwanie informacji na temat reguł alertów.

## <a name="create-an-alert-rule"></a>Utwórz regułę alertu

1. W [portalu](https://portal.azure.com/)kliknij przycisk **Przeglądaj**, a następnie polecenie zasobu Cię interesuje monitorowania.

2. Kliknij na kafelku **reguł alertów** w obiektyw **operacji** .

3. Kliknij polecenie **Dodaj alert** .

    ![Dodawanie alertu dotyczącego](./media/insights-receive-alert-notifications/Insights_AddAlert.png)

4. Nazwa reguły alertów i wybierz opis, który pojawi się w wiadomość e-mail z powiadomieniem.

5. Po wybraniu **metryki** będzie wybierz warunek i próg wartość metryki. To jest czas, w korzystającego z Azure monitor i kreślenia działanie alertów.

    ![Warunek i progowa.](./media/insights-receive-alert-notifications/Insights_ConditionAndThreshold.png)

6. Można także wybrać **zdarzenia**, a odbierać powiadomienie, gdy określone zdarzenie.

    ![Zdarzenia](./media/insights-receive-alert-notifications/Insights_Events.png)

7. Na koniec możesz wysyłania powiadomień e-mail do odpowiedzialności administratorów.

Po kliknięciu przycisku **Zapisz**w ciągu kilku minut będzie informowany przy każdym Metryka wybranym przekracza próg.

## <a name="managing-your-alert-rules"></a>Zarządzanie regułami alertów

Po utworzeniu reguły alertu, możesz wyświetlić podgląd alertu próg porównaniu metryki z poprzedniego dnia.

![Zdarzenia](./media/insights-receive-alert-notifications/Insights_EditAlert.png)


Oczywiście można edytować tej reguły alertu i **Włączanie** lub **Wyłączanie** go Jeśli chcesz, aby tymczasowo zrezygnować z otrzymywania powiadomień o nim.

## <a name="next-steps"></a>Następne kroki

* [Konfigurowanie webhooks na alerty](insights-webhooks-alerts.md) powiadomień trasy do różnych kanałów
* [Monitorowanie usługi metryki](insights-how-to-customize-monitoring.md) upewnij się, że usługi jest dostępny i odpowiada.
* [Włączanie monitorowania i diagnostyki](insights-how-to-use-diagnostics.md) zbieranie szczegółowe metryki wysokiej częstotliwości o tej usługi.
* [Monitor dostępności i czas reakcji dowolnej strony sieci web](../application-insights/app-insights-monitor-web-app-availability.md) przy użyciu aplikacji wniosków, więc można znaleźć w przypadku strony nie działa.
* [Monitorowanie wydajności aplikacji](../application-insights/app-insights-azure-web-apps.md) , jeśli chcesz poznać dokładnie tak, jak kod działa w chmurze.
* [Wyświetlanie zdarzeń i dzienników inspekcji](insights-debugging-with-events.md) Aby dowiedzieć się, wszystkie elementy podczas tej usługi.
* [Śledź kondycję usługi](insights-service-health.md) , aby dowiedzieć się, gdy Azure napotkał wydajności przerwań obniżenie wydajności lub usługi.
