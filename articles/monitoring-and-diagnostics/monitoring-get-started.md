<properties
    pageTitle="Wprowadzenie do programu Azure Monitor | Microsoft Azure"
    description="Rozpoczynanie pracy za pomocą Azure Monitor lepszy wgląd w operacji zasobów, a następnie wykonaj czynności podstawie poza danych."
    authors="johnkemnetz"
    manager="rboucher"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="johnkem"/>

# <a name="get-started-with-azure-monitor"></a>Wprowadzenie do programu Azure Monitor

Azure Monitor to usługa platformę, która zapewnia jedno źródło monitorowania Azure zasobów. Za pomocą monitora Azure można wizualizacji, kwerendy, rozsyłanie, archiwizowanie i wykonywanie akcji na stronie metryki i dzienniki pochodzące z zasobów w Azure. Można pracować z tymi danymi przy użyciu Monitor portalu karta, [Poleceń cmdlet środowiska PowerShell Monitor](./insights-powershell-samples.md), [Polecenie między platformami](insights-cli-samples.md)lub [Interfejsów API pozostałych Monitor Azure](https://msdn.microsoft.com/library/dn931943.aspx). W tym artykule przeprowadzimy przez kilka kluczowe składniki Monitor Azure, aby zobaczyć pokaz za pomocą portalu.

1. W portalu przejdź do **innych usług** i znajdź opcję **Monitor** . Kliknij ikonę gwiazdy, aby dodać tę opcję, do listy Ulubione, tak aby była zawsze łatwo dostępne na pasku nawigacyjnym po lewej stronie.

    ![Monitorowanie na liście usługi](./media/monitoring-get-started/monitor-more-services.png)

2. Kliknij opcję **Monitor** otwarcie karta **Monitor** . Ta karta zgromadzono wszystkie monitorowania ustawienia i dane w jeden widok skonsolidowane. Pierwszym otwarciu sekcji **dziennika aktywności** .

    ![Monitorowanie karta nawigacji](./media/monitoring-get-started/monitor-blade-nav.png)

    > [AZURE.WARNING] Opcje **powiadomienia usługi** i **grup powiadomienie** wyświetlane powyżej będą wyświetlane tylko do tych, którzy dołączyli prywatnych podglądu z tych funkcji.

    Monitorowanie Azure obejmuje trzy podstawowe kategorie monitorowanie danych: **Dziennik**, **metryki**i **Dzienniki diagnostyczne**.

3. Kliknij przycisk **Zaloguj aktywności** upewnij się, że sekcji dziennik aktywności jest wyświetlany.

    ![Karta dziennika aktywności](./media/monitoring-get-started/monitor-act-log-blade.png)

    [**Dziennik**](./monitoring-overview-activity-logs.md) w tym artykule opisano wszystkie operacje wykonywane na zasoby w ramach subskrypcji. Korzystając z dziennika aktywności, można określić "co, kto i kiedy" dla dowolnych tworzonych, aktualizowanie lub usuwanie operacji na zasoby w ramach subskrypcji. Na przykład dziennik informuje, kiedy zatrzymano aplikacji sieci web i kto zatrzymania. Zdarzeń dziennika aktywności są przechowywane na platformie i można wyszukiwać 90 dni.
   
    Możesz utworzyć i zapisać kwerend dla typowe filtry, a następnie przypiąć najważniejszych kwerendy do portalu pulpitu nawigacyjnego, aby będzie zawsze wiedzieć, jeśli wystąpiły zdarzenia, które spełniają podane kryteria.

4. Filtrowanie widoku do określonej grupy zasobów w ostatnim tygodniu, a następnie kliknij przycisk **Zapisz** .

    ![Zapisz kwerendę dziennika aktywności](./media/monitoring-get-started/monitor-act-log-save.png)

5. Teraz kliknij przycisk **Przypnij** .

    ![Kliknij polecenie Przypnij do dziennika aktywności](./media/monitoring-get-started/monitor-act-log-pin.png)

    Większość widoków w tym instruktażu może być przypięta do pulpitu nawigacyjnego. Pomaga utworzyć pojedyncze źródło danych dla danych operacyjnych na swoich usług. 

6. Wróć do pulpitu nawigacyjnego. Teraz widać, że kwerenda (i liczba wyników) są wyświetlane w pulpitu nawigacyjnego. Jest to przydatne, jeśli chcesz szybko wyświetlić wszystkie akcje wysokiej jakości, które wystąpiły ostatnio w ramach subskrypcji, np. przypisano nowej roli lub maszyny został usunięty.

    ![Dziennik przypięta do pulpitu nawigacyjnego](./media/monitoring-get-started/monitor-act-log-db.png)

7. Wróć do fragmentu **Monitor** i kliknij sekcji **metryki** . Musisz najpierw wybierz zasób, filtrowania i wybierając pozycję za pomocą listy rozwijanej Opcje u góry karta.

    ![Filtrowanie zasobów miar](./media/monitoring-get-started/monitor-met-filter.png)

    Wszystkie zasoby Azure wysyłać [**metryki**](./monitoring-overview-metrics.md). W tym widoku zgromadzono wszystkie metryki w jednym okienku szkła, można łatwo zrozumieć, jak działają zasobów.

8. Po wybraniu zasobu wszystkie dostępne wskaźniki są wyświetlane po lewej stronie karta. Można wykresu wielu metryki jednocześnie przez wybranie metryki i modyfikować zakres wykresu typu i godziny. Możesz także wyświetlić wszystkie alerty metryczne ustawić dla tego zasobu.

    ![Karta metryczne](./media/monitoring-get-started/monitor-metric-blade.png)

    > [AZURE.NOTE] Niektóre metryki są dostępne tylko po włączeniu [Wniosków aplikacji](../application-insights/app-insights-overview.md) i/lub systemem Windows lub Linux Azure Diagnostyka na zasób.

9. Po zakończeniu edycji z wykresu, możesz za pomocą przycisk **Przypnij** przypiąć go do pulpitu nawigacyjnego.

10. Wróć do karta **Monitor** , a następnie kliknij pozycję **Dzienniki diagnostyczne**.

    ![Karta dzienniki diagnostyczne](./media/monitoring-get-started/monitor-diaglogs-blade.png)

    [**Dzienniki diagnostyczne**](monitoring-overview-of-diagnostic-logs.md) są dzienniki wysyłane *przez* zasób dostarczać dane dotyczące działania tego zasobu. Na przykład liczniki reguły grupy zabezpieczeń sieci i logiki aplikacji przepływu pracy dzienniki są obydwa rodzaje dzienniki diagnostyczne. Dzienniki te mogą przechowywane na koncie miejsca do magazynowania, strumieniowo do koncentratora zdarzenie lub wysyłane do [Analizy dziennika](../log-analytics/log-analytics-overview.md). Analizy dziennika jest produktem informacji operacyjnych firmy Microsoft dla zaawansowane wyszukiwanie i alerty.
   
    W portalu można wyświetlać i filtrować listę wszystkich zasobów w ramach subskrypcji, aby ustalić, czy mają one dzienniki diagnostyczne włączone.

11. Kliknij pozycję zasób w karta dzienniki diagnostyczne. Jeśli dzienniki diagnostyczne są przechowywane w konto miejsca do magazynowania, zobaczysz listę godzinowe dzienników, które można pobrać bezpośrednio.

    ![Dzienniki diagnostyczne dla jednego zasobu](./media/monitoring-get-started/monitor-diaglogs-detail.png)

    Możesz również kliknąć pozycję **Ustawień diagnostycznych**, dzięki czemu można skonfigurować lub modyfikowanie ustawień archiwizacji do konta miejsca do magazynowania, streaming do koncentratorów zdarzenie lub wysyłając je do obszaru roboczego analizy dziennika.

    ![Włączanie dzienników diagnostycznych](./media/monitoring-get-started/monitor-diaglogs-enable.png)

    Jeśli masz skonfigurowane dzienniki diagnostyczne w celu analizy dziennika, można wyszukiwać je w sekcji **Wyszukiwanie dziennika** portalu.

12. Przejdź do sekcji **alerty** karta Monitor.

    ![karta alertów dla publicznej](./media/monitoring-get-started/monitor-alerts-nopp.png)

    W tym miejscu możesz zarządzać wszystkie [**alerty**](./monitoring-overview-alerts.md) na Azure zasoby. Ta opcja uwzględnia alertów dotyczących metryki, zdarzeń dziennika czynności (w podglądzie prywatnych) wniosków aplikacji sieci web testów (lokalizacja) i diagnostyki aktywne wniosków aplikacji. Alerty można wyzwalanie wiadomości e-mail, aby można było wysyłać lub wpisu HTTP do adresu URL webhook.
   
13. Kliknij przycisk **Dodaj metryczne alert** , aby utworzyć alert.

    ![Dodawanie alertu dotyczącego metryczne](./media/monitoring-get-started/monitor-alerts-add.png)

    Następnie możesz przypiąć alertu do pulpitu nawigacyjnego w celu ułatwienia przeglądania stanu w dowolnym momencie.

14. Sekcja Monitor też zawiera łącza do aplikacji [Wniosków aplikacji](../application-insights/app-insights-overview.md) i rozwiązań zarządzania [Analizy dziennika](../log-analytics/log-analytics-overview.md) . Innych produktów firmy Microsoft jest ścisła integracja z Azure Monitor.

15. Jeśli nie korzystasz z aplikacji wniosków lub analizy dziennika, prawdopodobnie oznacza to, że Azure Monitor ma powiązania z bieżącego monitorowania, rejestrowania i alertów produktów. Zobacz nasze [strony partnerów](./monitoring-partners.md) pełną listę i instrukcje dotyczące integracji.

Poniżej przedstawiono procedurę i przypięcie wszystkie Kafelki odpowiednich do pulpitu nawigacyjnego, można utworzyć pełna widoków aplikacji i infrastruktury podobny do tego:

![Azure Monitor pulpitu nawigacyjnego](./media/monitoring-get-started/monitor-final-dash.png)

## <a name="next-steps"></a>Następne kroki
- Zapoznanie się z [Omówienie Azure Monitor](./monitoring-overview.md)
