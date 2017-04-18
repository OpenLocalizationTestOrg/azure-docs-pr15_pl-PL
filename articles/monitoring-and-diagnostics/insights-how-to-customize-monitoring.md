<properties
    pageTitle="Omówienie metryki platformy Microsoft Azure | Microsoft Azure"
    description="Dowiedz się, jak dostosować monitorowania wykresów w Azure."
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

# <a name="overview-of-metrics-in-microsoft-azure"></a>Omówienie metryk platformy Microsoft Azure

Wszystkie usługi Azure śledzenie kluczowych metryk umożliwiają monitorowanie kondycji, wydajności, dostępności i korzystania z usług. Można wyświetlić te metryki Azure portal, a także umożliwia [Interfejsu API usługi REST](https://msdn.microsoft.com/library/azure/dn931930.aspx) lub [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights/) programowy dostęp do pełnego zestawu wskaźników.

W przypadku niektórych usług może być konieczne włączenie Diagnostyka, aby zobaczyć wszystkie metryki. Dla innych osób, takich jak maszyn wirtualnych zostanie wyświetlony podstawowy zestaw metryk, ale muszą umożliwiające pełnego zestawu metryk wysokiej częstotliwości. Zobacz [Włączanie monitorowania i diagnostyki](insights-how-to-use-diagnostics.md) , aby dowiedzieć się więcej.

## <a name="using-monitoring-charts"></a>Za pomocą wykresów monitorowania

Można dowolną metryki wykresu je w dowolnym przedziale czasu możesz wybrać.

1. W [Azure Portal](https://portal.azure.com/)kliknij przycisk **Przeglądaj**, a następnie polecenie zasobu Cię interesuje monitorowania.

2. Sekcja **Monitorowanie** zawiera najbardziej istotne metryki dla każdego zasobu Azure. Na przykład aplikacji sieci web ma **żądania i błędów**, gdzie jako maszyny wirtualnej woli **Procent Procesora** i **odczytu i zapisu dysku**:  ![obiektyw monitorowania](./media/insights-how-to-customize-monitoring/Insights_MonitoringChart.png)

3. Klikając dowolny wykres, zostanie wyświetlona karta **metryki** . Na karta, oprócz wykresu jest tabela zawierająca agregacji miar (takich jak średnia, minimum i maksimum w przedziale czasu, wybrana). Poniżej są reguły alertów dla zasobu.
    ![Karta metryczne](./media/insights-how-to-customize-monitoring/Insights_MetricBlade.png)

4. Aby dostosować linie, które są wyświetlane, kliknij przycisk **Edytuj** na wykresie lub polecenie **Edytuj wykres** na metryczne karta.

5. Na karta Edytuj zapytanie można wykonać trzy czynności:
    - Zmienianie zakresu godzin
    - Przełączanie między wygląd słupkowy i liniowy
    - Wybierz inny metics ![Edytuj zapytanie](./media/insights-how-to-customize-monitoring/Insights_EditQuery.png)

6. Zmiana przedziału czasu jest tak proste jak wybranie innego zakresu (na przykład **Ostatniej godziny**) i kliknięcie przycisku **Zapisz** u dołu karta. Możesz również wybrać **Niestandardowy**, który umożliwia wybór dowolnego okresu w ciągu ostatnich 2 tygodni. Na przykład można sprawdzić całego dwa tygodnie, lub, po prostu 1 godzina wczorajszą. Wpisz w polu tekstowym wprowadź różnych godzinę.
    ![Zakres czasu niestandardowe](./media/insights-how-to-customize-monitoring/Insights_CustomTime.png)

7. Poniżej zakres czasu możesz kanał wybierz dowolną liczbę metryki, aby pokazać na wykresie.

8. Po kliknięciu przycisku Zapisz wprowadzone zmiany zostaną zapisane dla tego zasobu. Na przykład jeśli masz dwie maszyn wirtualnych i zmienianie wykresu na jednym, będzie nie miało wpływ drugi.

## <a name="creating-side-by-side-charts"></a>Tworzenie wykresów obok siebie

Do dostosowywania zaawansowanych w portalu możesz dodać dowolną liczbę wykresów zgodnie z oczekiwaniami.

1. W menu **...** u góry karta kliknij przycisk **Dodaj Kafelki**:  
    ![Menu Dodaj](./media/insights-how-to-customize-monitoring/Insights_AddMenu.png)
2. Następnie możesz można wybierz wykres z **galerii** po prawej stronie ekranu:  ![galerii](./media/insights-how-to-customize-monitoring/Insights_Gallery.png)
3. Jeśli nie widzisz metryczne, które mają, można dodać jedną z wstępnie ustawionych metryki i **Edytowanie** wykres, aby wyświetlić metryki, która jest potrzebna.

## <a name="monitoring-usage-quotas"></a>Monitorowanie przydziałów użycia

Większość metryk pokazują trendy w czasie, ale niektóre dane, takie jak przydziałami użycia są w chwili informacji o progu.

Można też wyświetlić przydziałami użycia w karta dla zasobów, których przydziały:

![Użycie](./media/insights-how-to-customize-monitoring/Insights_UsageChart.png)

Przykład z miar, umożliwia [Interfejsu API usługi REST](https://msdn.microsoft.com/library/azure/dn931963.aspx) lub [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights/) programowy dostęp do pełnego zestawu przydziałami użycia.

## <a name="next-steps"></a>Następne kroki

* [Odbieranie alertów](insights-receive-alert-notifications.md) dotyczących metryki przecina progu.
* [Włączanie monitorowania i diagnostyki](insights-how-to-use-diagnostics.md) zbieranie szczegółowe metryki wysokiej częstotliwości o tej usługi.
* [Automatyczne skalowanie licznik wystąpień](insights-how-to-scale.md) , aby upewnić się, że usługi jest dostępny i odpowiada.
* [Monitorowanie wydajności aplikacji](../application-insights/app-insights-azure-web-apps.md) , jeśli chcesz poznać dokładnie tak, jak kod działa w chmurze.
* Użyj [Aplikacji wniosków dla aplikacji JavaScript i stron sieci web](../application-insights/app-insights-web-track-usage.md) , aby uzyskać analizy klienta informacji na temat przeglądarek, które strony sieci web.
* [Monitor dostępności i czas reakcji dowolnej strony sieci web](../application-insights/app-insights-monitor-web-app-availability.md) przy użyciu aplikacji wniosków, więc można znaleźć w przypadku strony nie działa.
