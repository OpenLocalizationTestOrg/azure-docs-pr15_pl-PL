<properties
    pageTitle="Włącz monitorowania i diagnostyki platformy Microsoft Azure | Microsoft Azure "
    description="Dowiedz się, jak skonfigurować informacje diagnostyczne dla zasobów w Azure."
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

# <a name="enable-monitoring-and-diagnostics"></a>Włączanie monitorowania i diagnostyki

W [Azure Portal](https://portal.azure.com)można skonfigurować zaawansowanej, częste, monitorowania i diagnostyki danych o zasobach. Aby skonfigurować diagnostyki programowy, możesz używać [Interfejsu API usługi REST](https://msdn.microsoft.com/library/azure/dn931932.aspx) lub [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights/) .

Diagnostyka, monitorowanie i jednostki metryczne dane platformy Azure są zapisywane na konto miejsca do magazynowania wybranych przez użytkownika. Pozwala na korzystanie, niezależnie od narzędzia chcesz Odczyt danych, Eksplorator magazynu usługi Power BI do narzędzia innych firm.

## <a name="when-you-create-a-resource"></a>Po utworzeniu zasobu

Większość usług umożliwiają włączanie diagnostyki po ich utworzeniu w [Azure Portal](https://portal.azure.com).

1. **Nowy** i wybierz zasób, który Cię interesuje.

2. Wybierz pozycję **opcjonalnym**.
    ![Karta Narzędzia diagnostyczne](./media/insights-how-to-use-diagnostics/Insights_CreateTime.png)

3. Wybierz pozycję **Diagnostyka**, a następnie kliknij **polecenie**. Konieczne będzie wybierz konto miejsca do magazynowania, który powinien diagnostyki do zapisania. Możesz zostanie naliczona szybkości normalnego danych do przechowywania i transakcje podczas wysyłania diagnostyki na koncie miejsca do magazynowania.

4. Kliknij **przycisk OK** , a następnie utworzyć zasób.

## <a name="change-settings-for-an-existing-resource"></a>Zmienianie ustawień dla istniejącego zasobu

Jeśli utworzono już zasobu i chcesz zmienić ustawienia diagnostyki (Aby na przykład zmienić na poziomie zbioru danych), możesz wykonać tego bezpośrednio w portalu Azure.

1. Przejdź do zasobu, a następnie kliknij polecenie **Ustawienia** .

2. Wybierz pozycję **Diagnostyka**.

3. Karta **Narzędzia diagnostyczne** zawiera wszystkie możliwe narzędzia diagnostyczne i monitorowania zbioru danych dla tego zasobu. Dla niektórych zasobów możesz również wybrać zasad **przechowywania** danych, aby oczyścić z Twojego konta miejsca do magazynowania.
    ![Narzędzia diagnostyczne miejsca do magazynowania](./media/insights-how-to-use-diagnostics/Insights_StorageDiagnostics.png)

4. Po wybraniu ustawienia, kliknij polecenie **Zapisz** . Może upłynąć trochę podczas do monitorowania danych się pojawić Jeśli włączono jej po raz pierwszy.

### <a name="categories-of-data-collection-for-virtual-machines"></a>Kategorie zbierania danych na potrzeby maszyn wirtualnych
Dla maszyn wirtualnych wszystkich wskaźników i dzienniki będą rejestrowane w odstępach, co pozwoli zawsze najbardziej aktualne informacje o tym komputerze.

- **Podstawowe metryki** : metryki kondycji informacji o komputerze wirtualnych, takich jak procesora i pamięci
- **Metryki sieci i w sieci web** : metryki o połączeń sieciowych i usług sieci web
- **Metryki .NET** : metryki o aplikacjach .NET i ASP.NET, na komputerze wirtualnych
- **Metryki SQL** : Jeśli korzystasz z usługi SQL firmy Microsoft, jej wskaźniki
- **Dzienniki aplikacji zdarzeń systemu Windows** : zdarzeń systemu Windows, które są wysyłane do kanału aplikacji
- **Dzienniki systemu zdarzeń systemu Windows** : zdarzeń systemu Windows, które są wysyłane do kanału, system. Obejmuje to także wszystkie zdarzenia w [Programie Microsoft ochrony przed złośliwym oprogramowaniem](http://go.microsoft.com/fwlink/?LinkID=404171&clcid=0x409).
- **Dzienniki zabezpieczeń zdarzeń systemu Windows** : zdarzeń systemu Windows, które są wysyłane do bezpiecznego kanału
- **Dzienniki infrastruktury diagnostyki** : rejestrowanie dotyczące infrastruktury zbioru diagnostyki
- **Dzienniki programu IIS** : dzienniki dotyczące serwer usług IIS

Zauważ, że w tym momencie niektórych podziału Linux nie są obsługiwane i agenta gościa musi być zainstalowana na komputerze wirtualnych.

## <a name="next-steps"></a>Następne kroki

* [Odbieranie alertów](insights-receive-alert-notifications.md) przy każdym operacyjne zdarzeń lub metryki krzyżowe progu.
* [Monitorowanie usługi metryki](insights-how-to-customize-monitoring.md) upewnij się, że usługi jest dostępny i odpowiada.
* [Automatyczne skalowanie licznik wystąpień](insights-how-to-scale.md) , aby się upewnić się, że na skalę usługi oparte na żądanie.
* [Monitorowanie wydajności aplikacji](../application-insights/app-insights-azure-web-apps.md) , jeśli chcesz poznać dokładnie tak, jak kod działa w chmurze.
* [Wyświetlanie zdarzeń i dzienników inspekcji](insights-debugging-with-events.md) Aby dowiedzieć się, wszystkie elementy podczas tej usługi.
* [Śledź kondycję usługi](insights-service-health.md) , aby dowiedzieć się, gdy Azure napotkał wydajności przerwań obniżenie wydajności lub usługi.
