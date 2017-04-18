<properties
   pageTitle="Uzyskaj więcej informacji z Centrum zabezpieczeń Azure danych za pomocą usługi Power BI | Microsoft Azure"
   description="Pakiet zawartości Azure zabezpieczeń Centrum Power BI ułatwia znajdowanie alertów zabezpieczeń, zalecenia, zaatakowany zasobów i trendy, opartych na zestaw danych utworzony dla raportowaniem."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/22/2016"
   ms.author="yurid"/>

# <a name="get-insights-from-azure-security-center-data-with-power-bi"></a>Uzyskaj więcej informacji z Centrum zabezpieczeń Azure danych za pomocą usługi Power BI
[Pulpit nawigacyjny programu Power BI](http://aka.ms/azure-security-center-power-bi) dla Centrum zabezpieczeń Azure umożliwia wizualizowanie oraz ich analizowania i filtrowania zalecenia i alerty zabezpieczeń z dowolnego miejsca, w tym na urządzeniu przenośnym. Za pomocą usługi Power BI pulpitu nawigacyjnego do przedstawienia trendów i ataki desenie — widok alertów zabezpieczeń, zasobów lub źródłowy adres IP i unaddressed ryzyko związane z zabezpieczeniami, zasobów lub wiek. 

Możesz można również zestawiania zalecenia Centrum zabezpieczeń i alerty zabezpieczeń z innych danych w różnych sposobów, na przykład za pomocą danych z [Dzienników inspekcji Azure](https://powerbi.microsoft.com/blog/monitor-azure-audit-logs-with-power-bi/) i [Inspekcja bazy danych SQL Azure](https://powerbi.microsoft.com/blog/monitor-your-azure-sql-database-auditing-activity-with-power-bi/). Dostępne pulpity nawigacyjne usługi Power BI, a te dane można eksportować do programu Excel, łatwo raportowania stanu zabezpieczeń zasobów chmury.

##<a name="using-azure-security-center-dashboard-to-access-power-bi"></a>Aby uzyskać dostęp do usługi Power BI za pomocą pulpitu nawigacyjnego Centrum zabezpieczeń Azure
Aby uzyskać dostęp do raportów programu Power BI umożliwia także pulpit nawigacyjny Centrum zabezpieczeń Azure. Postępuj zgodnie z instrukcjami, aby wykonać to zadanie: 

1. Na pulpicie nawigacyjnym **Azure Centrum zabezpieczeń** kliknij przycisk **Eksploruj w usłudze Power BI** .

    ![Nawiązywanie połączenia z Centrum zabezpieczeń Azure za pomocą usługi Power BI](./media/security-center-powerbi/security-center-powerbi-fig1-new10.png) 

2. Karta **Eksploruj w usłudze Power BI** zostanie otwarty po prawej stronie, jak pokazano na poniższym obrazie:

    ![Nawiązywanie połączenia z Centrum zabezpieczeń Azure za pomocą usługi Power BI](./media/security-center-powerbi/security-center-powerbi-fig1-new2.png)

3. Jeśli po raz pierwszy tworzysz pulpitu nawigacyjnego Power BI, można wybierać jedną z następujących opcji w karta **Eksploruj w usłudze Power BI** : 

    - **Pulpit nawigacyjny wniosków zabezpieczeń**: Wybierz tę opcję, jeśli chcesz utworzyć pulpit nawigacyjny zawierający stan zabezpieczeń, wątków i wykryć. Ta opcja jest więcej wspólny dla roli DevOps, która jest odpowiedzialny za analizowanie ich stanu ochrony i wykryte alerty w subskrypcjach.
    - **Pulpit nawigacyjny zarządzania zasadami**: Wybierz tę opcję, jeśli chcesz poznać zasady zarządzania i wymuszania.  Ta opcja jest więcej wspólny dla centralnej IT, które są bardziej ograniczony do zarządzania. Mogą używać tego pulpitu nawigacyjnego do uzyskania widoczność i wniosków na przestrzeganie zasad zabezpieczeń w całej organizacji.
    - Jeśli masz już pulpitu nawigacyjnego programu Power BI, kliknij przycisk **Przejdź do pulpitu nawigacyjnego bieżącego Power BI**.

4. W tym przykładzie kliknij opcję **pulpitu nawigacyjnego wniosków zabezpieczeń** . Jeśli jest to po raz pierwszy tworzysz pulpitu nawigacyjnego programu Power BI dla Centrum zabezpieczeń monit o zainstalowanie pakietu zawartości. Kliknij przycisk **Pobierz** , w oknie **zawartości pakiety usługi Power BI** , jak pokazano na poniższym obrazie:

    ![Azure zabezpieczeń Centrum zabezpieczeń wniosków pulpitu nawigacyjnego](./media/security-center-powerbi/security-center-powerbi-fig1-new3.png)

5. Wyświetlone okno **Nawiązywanie połączenia z Azure zabezpieczeń Centrum zabezpieczeń wnioski** . Upewnij się, metody **uwierzytelniania** jest **Uwierzytelnianie oAuth2** , tak jak pokazano poniżej i kliknij przycisk **Zaloguj** .
    
    ![Uwierzytelnianie](./media/security-center-powerbi/security-center-powerbi-fig1-new4.png)

6. Możesz otrzymać prośbę do uwierzytelniania ponownie Azure poświadczenia. Jest tworzona po uwierzytelnieniu pulpitu nawigacyjnego. Po utworzeniu pulpitu nawigacyjnego możesz zobaczyć raport o strukturze podobnie, jak pokazano na poniższym obrazie:

    ![Pulpit nawigacyjny programu Power BI](./media/security-center-powerbi/security-center-powerbi-fig1-new5.png)


> [AZURE.NOTE] Odświeżenie raportu zaplanowano może być wykonywana codziennie. W przypadku, gdy wystąpi błąd podczas odświeżania tego, w tym artykule [Odświeżanie problemów z Centrum Azure zabezpieczeń usługi Power BI](https://blogs.msdn.microsoft.com/azuresecurity/2016/04/07/azure-security-center-power-bi-refresh-fails/), aby uzyskać więcej informacji na temat rozwiązywania problemów z.

W tym miejscu można zawiera liczbę alertów zabezpieczeń i zalecenia, a także liczbę maszyny wirtualne, bazy danych programu SQL Azure i zasobów sieciowych monitorowane przez Centrum zabezpieczeń Azure.

Łącze do Centrum zabezpieczeń Azure przekierowuje do portalu Azure. Wykresy ułatwia wizualizowanie informacji na temat zalecenia dotyczące zabezpieczeń i alertami, między innymi:

- Stan zabezpieczeń zasobów
- Oczekiwanie na zalecenia
- Zalecenia dotyczące maszyn wirtualnych
- Alerty w czasie
- Zaatakowanych zasobów
- Adresy IP usługi zaatakowanych

Za każdym wykresie istnieją dodatkowe wnioski. Zaznacz pole, aby zobaczyć więcej informacji. Na przykład kafelków **Stanu zabezpieczeń zasobu** zawiera dodatkowe informacje o oczekujących zalecenia przez wszystkie zasoby jak pokazano na poniższym obrazie:

![Zalecenia](./media/security-center-powerbi/security-center-powerbi-fig1-new6.png)

Kliknięcie w dowolnym wierszu ten wykres, inne osoby będą wygaszanie i skupić się tylko na ten, który wybrano. Aby powrócić do pulpitu nawigacyjnego, kliknij ikonę **Centrum zabezpieczeń Azure** w obszarze polecenia **pulpitów nawigacyjnych** w okienku po lewej stronie tej strony.

> [AZURE.NOTE] Jeśli chcesz dostosować raportów, dodając dodatkowe pola lub zmienić istniejące wizualizacji, można edytować raport. Aby uzyskać więcej informacji, przeczytaj [Interakcja z raportem w widoku do edycji w usłudze Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-interact-with-a-report-in-editing-view/) .

Kafelki **alertów czasie zaatakowany zasobów** i **Adresy IP atakująca** uzyskuje podobne dane wyjściowe, po kliknięciu w każdej z nich go. Dzieje się tak, ponieważ raport agreguje informacje dotyczące tych trzech zmiennych i połączeń go **zasobów w ramach atakiem** , jak pokazano na poniższym obrazie:

![Zasoby w obszarze atakiem](./media/security-center-powerbi/security-center-powerbi-fig1-new7.png)

Na tym etapie można również zapisać jego kopię tego raportu, wydrukować go lub publikować w sieci web za pomocą opcji dostępnych w menu **plik** .

![Menu Plik](./media/security-center-powerbi/security-center-powerbi-fig8.png)

## <a name="exploring-your-azure-security-center-data-with-power-bi-services"></a>Eksplorowanie danych Centrum zabezpieczeń Azure za pomocą usługi Power BI

Łączenie się z [Usługi Power BI zawartością Pack](https://msit.powerbi.com/groups/me/getdata/services) w usłudze Power BI i wykonaj następujące czynności:

1. W oknie **Pakiet usługi Power BI** będą widoczne dwie opcje, tak jak pokazano poniżej.

    ![Pakiet usługi Power BI](./media/security-center-powerbi/security-center-powerbi-fig1-new.png)

    >[AZURE.NOTE] Jeśli już wykonane pierwszej części tego artykułu zostanie wyświetlona tylko jedna opcja, która jest zarządzanie zasadami Centrum zabezpieczeń Azure.

2. W celu w tym przykładzie kliknij przycisk **Pobierz** kafelku **Zarządzanie zasadami Centrum zabezpieczeń Azure** .

3. W oknie **Nawiązywanie połączenia z Zarządzanie zasadami Centrum zabezpieczeń Azure** upewnij się zaznaczyć **Uwierzytelnianie oAuth2** w obszarze **Metody uwierzytelniania** listy rozwijanej, jak pokazano poniżej, a następnie kliknij przycisk **Zaloguj** .

    ![Zasady zarządzania okna](./media/security-center-powerbi/security-center-powerbi-fig1-new8.png)

4. Nastąpi przekierowanie do strony uwierzytelniania miejsce, w którym należy wpisać poświadczenia, których używasz nawiązywania połączenia z Centrum zabezpieczeń Azure. Po ukończeniu procesu uwierzytelniania usługi Power BI rozpocznie się importowanie danych do tworzenia raportów. W tym czasie w prawym rogu przeglądarki może zostać wyświetlony następujący komunikat:

    ![Nawiązywanie połączenia z Centrum zabezpieczeń Azure za pomocą usługi Power BI](./media/security-center-powerbi/security-center-powerbi-fig4.png)

    >[AZURE.NOTE] gdy pulpit nawigacyjny jest tworzony po raz pierwszy może potrwać dłużej niż zazwyczaj głównie dla scenariuszy, w którym masz wiele subskrypcji. 

5. Po ukończeniu procesu pulpitu nawigacyjnego Azure zabezpieczeń Centrum Power BI pobierze razem z raportem **Zarządzania zasadami** podobny do tego, jak pokazano poniżej:

    ![Pulpit nawigacyjny zarządzania zasad](./media/security-center-powerbi/security-center-powerbi-fig1-new9.png)

## <a name="see-also"></a>Zobacz też
W tym dokumencie wiesz, jak używać usługi Power BI w Centrum zabezpieczeń Azure. Aby dowiedzieć się więcej na temat Centrum zabezpieczeń Azure, zobacz następujące artykuły:

- [Planowanie Centrum zabezpieczeń Azure i przewodnik](security-center-planning-and-operations-guide.md) — Dowiedz się, jak zaplanowanie wdrożenia Centrum zabezpieczeń Azure.
- [Ustawianie zasad zabezpieczeń w Centrum zabezpieczeń Azure](security-center-policies.md) — Dowiedz się, jak skonfigurować ustawienia zabezpieczeń w Centrum zabezpieczeń Azure
- [Zarządzanie notatkami i odpowiadanie na alerty zabezpieczeń w Centrum zabezpieczeń Azure](security-center-managing-and-responding-alerts.md) — Dowiedz się, jak zarządzać i odpowiadanie na alerty zabezpieczeń
- [Często zadawane pytania dotyczące Azure zabezpieczeń Centrum](security-center-faq.md) — znajdowanie często zadawane pytania dotyczące korzystania z usługi
- [Blog dotyczący zabezpieczeń Azure](http://blogs.msdn.com/b/azuresecurity/) — znajdowanie blogu wpisy o Azure zabezpieczenia i zgodność
