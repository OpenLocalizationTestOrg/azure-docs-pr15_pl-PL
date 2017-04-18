<properties
    pageTitle="Wyświetlanie zdarzeń i dzienników inspekcji"
    description="Dowiedz się, jak wyświetlić wszystkie zdarzenia, które pojawiają się w subskrypcji usługi Azure."
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
    ms.date="04/28/2015"
    ms.author="robb"/>

# <a name="view-events-and-audit-logs"></a>Wyświetlanie zdarzeń i dzienników inspekcji

Wszystkie operacje wykonywane na Azure zasoby są w pełni inspekcji przez Azure Menedżera zasobów, tworzenie i usuwanie udzielanie i odwoływanie dostępu. Można przeglądać te dzienniki w portalu Azure, a także umożliwia [Interfejsu API usługi REST](https://msdn.microsoft.com/library/azure/dn931927.aspx) lub [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights/) programowy dostęp do pełnego zestawu zdarzeń.

## <a name="browse-the-events-impacting-your-azure-subscription"></a>Przeglądanie zdarzeń wpływające na ochronę subskrypcji Azure

1. Zaloguj się do [portalu Azure](https://portal.azure.com/).
2. Kliknij **Przeglądaj** i wybierz pozycję **dzienników inspekcji**.  
    ![Przeglądanie Centrum](./media/insights-debugging-with-events/Insights_Browse.png)
3. Spowoduje to otwarcie się karta przedstawiający wszystkie zdarzenia, które mają wpływ dowolną subskrypcji ostatnich 7 dni. U góry jest zestawienie danych według poziomu, a pod nim pełną listę dzienników:  ![wszystkie zdarzenia](./media/insights-debugging-with-events/Insights_AllEvents.png)

>[AZURE.NOTE] 500 ostatnich zdarzeń dla danej subskrypcji mogą tylko wyświetlać w portalu Azure.

4. Możesz kliknąć dowolną pozycję dziennika, aby wyświetlić zdarzenia, które składają go. Na przykład po wdrożeniu coś do grupy zasobów wiele różnych zasobów mogą być utworzone lub zmodyfikowane. Dla każdego wpisu można wyświetlić:
    * **Poziom** zdarzenie — na przykład go może być tylko do śledzenia (**informacyjne**), lub gdy coś niepowodzenia należy wiedzieć o (**Błąd**).
    * **Stan** — stan końcowy zazwyczaj będzie **powiodło się** lub **nie powiodła się**, ale może to być również **zaakceptowane** dla długotrwałych operacji.
    * *Gdy* zdarzenie wystąpił.
    * *Kto* wykonać operację, jeśli każda osoba. Nie wszystkie operacje są wykonywane przez użytkowników, niektóre są wykonywane przez wewnętrznej bazy danych usług, nie mają oni **rozmówcy**.
    * **Identyfikator korelacji** zdarzenia — jest to unikatowy identyfikator dla tego zestawu działań.

5. Z tego miejsca możesz przejść do Karta Szczegóły, aby wyświetlić szczegółowe informacje na temat terminu.

    ![Grupy zasobów](./media/insights-debugging-with-events/Insights_EventDetails.png)

    Zdarzenia **nie powiodło się** na tej stronie zazwyczaj znajdują **podstanu** i sekcję **Właściwości** , która zawiera przydatne informacje dotyczące do debugowania.

## <a name="filter-to-specific-logs"></a>Filtrowanie do określonych dzienników

Aby można było wyświetlić zdarzenia, które są stosowane do określonego obiektu lub określonego typu, karta dzienników inspekcji można filtrować, klikając polecenie **Filtruj** . Karta filtr to umożliwia również zmienić **zakresu czasu** : karta dzienników inspekcji.

Po kliknięciu tego polecenia zostanie otwarte nowe karta:

![Filtr](./media/insights-debugging-with-events/Insights_EventFilter.png)

Istnieją cztery typy filtrów:

1. Za subskrypcję
2. **Grupa zasobów**
3. **Typ zasobu**
4. Według określonego **zasobu** — w tym należy poza pełny *Identyfikator zasobu* zasobu, który Cię interesuje

Ponadto możesz również filtrować zdarzenia przez który wykonywane zdarzenie lub, poziom zdarzenia.

Po zakończeniu, wybieranie, co chcesz wyświetlić, kliknij przycisk **Aktualizuj** u dołu karta.

## <a name="monitor-events-impacting-specific-resources"></a>Monitorować zdarzenia wpływające na ochronę określonych zasobów

1. Kliknij **Przeglądaj** , aby znaleźć zasób, który Cię interesuje. Można też wyświetlić wszystkie pliki dziennika dla całej **grupy zasobów**.
2. Na karta zasobu przewiń w dół do momentu znalezienia kafelków **zdarzeń** .  
    ![Zdarzenia kafelków](./media/insights-debugging-with-events/Insights_EventsTile.png)
3. Kliknij na tym kafelku Aby przeglądać filtrowane, aby tylko zasobu zaznaczonego zdarzenia. Za pomocą polecenia **Filtruj** zmienić zakres czasu lub stosowanie bardziej szczegółowych filtrów.

## <a name="next-steps"></a>Następne kroki

* [Odbieranie alertów](insights-receive-alert-notifications.md) dotyczących zdarzenie.
* [Monitorowanie usługi metryki](insights-how-to-customize-monitoring.md) upewnij się, że usługi jest dostępny i odpowiada.
* [Śledź kondycję usługi](insights-service-health.md) , aby dowiedzieć się, gdy Azure wystąpił wydajności przerwań obniżenie wydajności lub usługi.  
