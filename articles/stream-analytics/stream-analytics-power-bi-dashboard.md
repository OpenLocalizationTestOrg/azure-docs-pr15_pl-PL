<properties
    pageTitle="Power BI z pulpitu nawigacyjnego na analizy strumieniu | Microsoft Azure"
    description="W czasie rzeczywistym przesyłanie strumieniowe pulpitu nawigacyjnego Power BI umożliwia gromadzenie analiz biznesowych i analizowanie dużych danych przy użyciu zadania analizy strumieniu."
    keywords="pulpit nawigacyjny analizy, w czasie rzeczywistym pulpitu nawigacyjnego"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>

#  <a name="stream-analytics--power-bi-a-real-time-analytics-dashboard-for-streaming-data"></a>Przesyłanie strumieniowe analizy i Power BI: pulpitu nawigacyjnego w czasie rzeczywistym analizy dla strumieniowego przesyłania danych

Azure analizy strumieniu pozwala korzystać z jednego z wiodących narzędzi do analizy biznesowej, Microsoft Power BI. Dowiedz się, jak za pomocą analizy strumieniu Azure analizowanie dużych przesyłanie strumieniowe danych i uzyskaj informacje w czasie rzeczywistym pulpitu nawigacyjnego analizy Power BI.

[Microsoft Power BI](https://powerbi.com/) umożliwia szybkie tworzenie live pulpitu nawigacyjnego. [Obejrzyj klip wideo przedstawiający tego scenariusza](https://www.youtube.com/watch?v=SGUpT-a99MA).

W tym artykule Dowiedz się, jak tworzyć własne narzędzi do analizy biznesowej niestandardowych przy użyciu usługi Power BI jako wynik dla zadań analizy strumieniu Azure i są używane w czasie rzeczywistym pulpitu nawigacyjnego.

## <a name="prerequisites"></a>Wymagania wstępne

* Konto Microsoft Azure
* Dane wejściowe na przesyłanie strumieniowe dane zwracane przez zadania analizy strumieniu. Analizy strumieniu przyjmuje z magazynu koncentratory zdarzenia Azure lub obiektów Blob platformy Azure.  
* Konto służbowe usługi Power BI

## <a name="create-azure-stream-analytics-job"></a>Utwórz zadanie analizy strumieniu Azure

W [Klasycznym Portal Azure](https://manage.windowsazure.com)kliknij pozycję **Nowy, usług danych, analizy strumieniu, szybkie tworzenie**.

Zmiennoprzecinkową następujące wartości, następnie kliknij pozycję **Utwórz analizy strumieniu zadania**:

* **Nazwa zadania** - wprowadź nazwę zadania. Na przykład **DeviceTemperatures**.
* **Region** — wybierz region, w którym chcesz zadania znajduje się. Należy rozważyć, umieszczając to zadanie i Centrum zdarzeń w tym samym regionie, aby zapewnić uzyskanie optymalnej wydajności i uniknąć naliczania kosztów transfer danych między regionami.
* **Konto miejsca do magazynowania** — wybierz konto miejsca do magazynowania, którego chcesz używać do przechowywania danych monitorowania dla wszystkich zadań analizy strumieniu uruchomiona w obrębie tego obszaru. Istnieje możliwość wybierz istniejące konto miejsca do magazynowania lub Utwórz nowy.

Kliknij pozycję **Analizy strumieniu** w okienku po lewej stronie, aby wyświetlić zadania analizy strumieniu.

![graphic1][graphic1]

> [AZURE.TIP] Nowe zadanie będzie wyświetlane ze stanem **Nie zostało uruchomione**. Zwróć uwagę, że przycisk **Start** na koniec strony jest wyłączona. Jest to oczekiwane zachowanie, jak należy skonfigurować zadania wejście, wynik kwerendy i tak dalej przed rozpoczęciem tego zadania.

## <a name="specify-job-input"></a>Określanie zadania wejście

Ten samouczek możemy są przy założeniu, że korzystasz z koncentratora zdarzenia jako dane wejściowe z JSON szeregowania i kodowania UTF-8.

* Kliknij nazwę zadania.
* Kliknij pozycję **wartości wejściowych** w górnej części strony, a następnie kliknij **Dodawanie danych wejściowych**. Zostanie otwarte okno dialogowe przeprowadzi Cię przez kilka czynności, aby zdefiniować zakres wejściowy.
*   Wybierz pozycję **strumienia danych**, a następnie kliknij przycisk prawy.
*   Wybierz pozycję **Centrum zdarzenia**, a następnie kliknij przycisk prawa.
*   Wpisz lub wybierz następujące wartości na trzeciej stronie:
  * **Alias wprowadzania** - Wprowadź przyjazną nazwę dla tego zadania wprowadzania. Należy zauważyć, że będziesz używać tej nazwy w kwerendzie później.
  * **Centrum zdarzenia** — Jeśli utworzono Centrum zdarzenie jest w tej samej subskrypcji jako zadanie analizy strumieniu, wybierz pozycję nazw, należącym do Centrum zdarzenia.
*   Jeśli Twoim Centrum zdarzeń znajduje się w innej subskrypcji, wybierz pozycję **Centrum zdarzeń korzystanie z innej subskrypcji** i ręcznie wprowadź informacje dotyczące **Usługi Bus Namespace**, **Nazwa centrum zdarzenia** **Nazwa zasady Centrum zdarzenia**, **Klucz zasad Centrum zdarzenia**i **Liczba Partition Centrum zdarzeń**.

> [AZURE.NOTE]  W przykładzie użyto domyślnej liczby partycje, czyli 16.

* **Nazwa centrum zdarzenia** - wybierz nazwę Centrum zdarzeń Azure masz.
* **Nazwa zasady Centrum zdarzeń** - wybierz zasady Centrum zdarzeń koncentratora zdarzenia, gdy używasz. Upewnij się, że tych zasad ma Zarządzanie uprawnieniami.
*   **Grupę dla klientów indywidualnych Centrum zdarzeń** — możesz pozostawi tę wartość pustą lub określić grupę dla klientów indywidualnych na Twoim Centrum zdarzenia. Należy zauważyć, że każdej grupy dla klientów indywidualnych koncentratora zdarzenia można tylko 5 czytników naraz. Tak zdecydować grupie prawo dla klientów indywidualnych dla zadania. Jeśli pole jest puste, użyje domyślnej grupy dla klientów indywidualnych.

*   Kliknij przycisk prawy.
*   Określ następujące wartości:
  * **Format serializatora zdarzenia** — JSON
  * **Kodowanie** - UTF8
*   Kliknij przycisk Sprawdź, aby dodać to źródło i sprawdzić, czy analizy strumieniu pomyślnie łączy do koncentratora zdarzenia.

## <a name="add-power-bi-output"></a>Dodawanie danych wyjściowych Power BI

1.  Kliknij przycisk **dane wyjściowe** z górnej części strony, a następnie kliknij **Dodaj dane wyjściowe**. Zobaczysz, że Power BI na liście opcji wyjścia.

    ![graphic2][graphic2]  

2.  Wybierz pozycję **Power BI** , a następnie kliknij przycisk prawy.
3.  Zostanie wyświetlony ekran podobnej do następującej:

    ![graphic3][graphic3]  

4.  W tym kroku podać konto służbowe w wynikach zadanie analizy strumieniu. Jeśli masz już konto usługi Power BI, wybierz pozycję **Autoryzuj teraz**. W przeciwnym razie wybierz pozycję **Utwórz konto teraz**. [Oto blogu warto Instruktaż szczegóły dotyczące tworzenia konta w usłudze Power BI](http://blogs.technet.com/b/powerbisupport/archive/2015/02/06/power-bi-sign-up-walkthrough.aspx).

    ![graphic11][graphic11]  

5.  Następnie zostanie wyświetlony ekran podobnej do następującej:

    ![graphic4][graphic4]  

Wprowadź wartości, jak pokazano poniżej:

* **Wyjściowy Alias** — można umieścić dowolną alias dane wyjściowe, który jest łatwe odwołują się do. Ten alias wynik jest szczególnie przydatne, jeśli jednak chcesz mieć wiele wyjść dla zadania. W tym przypadku należy odwołują się do tego raportu w kwerendzie. Na przykład, można użyć wartości wyjściowych alias = "OutPbi".
* **Nazwa zestawu danych** — Podaj nazwę zestawu danych, która ma usługi Power BI wyjściowy ma. Na przykład można użyć "pbidemo".
*   **Nazwa tabeli** — Podaj nazwę tabeli w obszarze zestaw danych wydruku Power BI. Załóżmy, że firma Microsoft połączeń "pbidemo". Obecnie Power BI wyniki analizy strumieniu zadania mogą mieć tylko jedną tabelę w zestawie danych.
*   **Obszar roboczy** — wybierz obszaru roboczego w dzierżawy usługi Power BI, w którym zostanie utworzony zestaw danych.

>   [AZURE.NOTE] Nie należy jawnie tworzyć tego zestawu danych i tabeli na koncie usługi Power BI. Ta osoba jest automatycznie tworzona po rozpoczęciu pracy analizy strumieniu i rozpoczęcia zadania pompującego wyjściowego do usługi Power BI. Jeśli kwerenda zadania nie zwróci żadnych wyników, zestawu danych i tabeli nie zostanie utworzony.

*   Kliknij przycisk **OK**, **Połączenie testowe** i teraz wyprowadzania konfigurację zostanie zakończone.

>   [AZURE.WARNING] Pamiętaj również, że jeśli Power BI ma już zestawu danych i tabeli o takiej samej nazwie jak podanymi w tym zadaniu analizy strumieniu, istniejące dane zostaną zastąpione.


## <a name="write-query"></a>Napisać kwerendę

Przejdź do karty **kwerendy** zadania. Napisz zapytania, których wyjściu ma w usługi Power BI. Na przykład może to być przykład poniższa kwerenda SQL:

    SELECT
        MAX(hmdt) AS hmdt,
        MAX(temp) AS temp,
        System.TimeStamp AS time,
        dspl
    INTO
        OutPBI
    FROM
        Input TIMESTAMP BY time
    GROUP BY
        TUMBLINGWINDOW(ss,1),
        dspl



Rozpoczęcie pracy. Sprawdź, czy Twoim Centrum zdarzeń otrzymuje zdarzenia, a kwerenda wygeneruje oczekiwanych wyników. Jeśli kwerenda wyświetla 0 wierszy, zestawu danych usługi Power BI i tabel nie jest automatycznie tworzona.

## <a name="create-the-dashboard-in-power-bi"></a>Tworzenie pulpitu nawigacyjnego w usłudze Power BI

Przejdź do [Powerbi.com](https://powerbi.com) i zaloguj się przy użyciu konta służbowego. Jeśli kwerenda zadanie analizy strumieniu zapisuje wyniki, zostanie wyświetlony utworzonej swojego zestawu danych jest już:

![graphic5][graphic5]

Do tworzenia pulpitu nawigacyjnego, przejdź do opcji pulpitów nawigacyjnych i Utwórz nowy pulpit nawigacyjny.

![graphic6][graphic6]

W tym przykładzie, firma Microsoft będzie etykiety go "Pokaz pulpitu nawigacyjnego".

Kliknij przycisk w zestawie danych utworzony przez zadanie analizy strumieniu (pbidemo w naszym przykładzie bieżącego). Nastąpi do strony do utworzenia wykresu u góry tego zestawu danych. Oto ale przykładem raportów, które można tworzyć:

Wybierz pozycję Σ temp i czasu pola. Zostaną one automatycznie przejdź do wartości i osi wykresu:

![graphic7][graphic7]

Dzięki temu zostanie automatycznie wyświetlony wykres, jak pokazano poniżej:

![graphic8][graphic8]

W sekcji wartości kliknij pozycję na liście w dół dla temp i **średniej** temperatury i na wykresie, kliknij pozycję **wizualizacji** lub wybrać **Wykres liniowy**:

![graphic9][graphic9]

Teraz uzyskasz wykresu liniowego średniej w czasie.  Za pomocą opcji numer pin, jak pokazano poniżej, można przypiąć to do pulpitu nawigacyjnego, uprzednio utworzony:

![graphic10][graphic10]

Podczas wyświetlania pulpitu nawigacyjnego w tym raporcie przypiętych, zostanie wyświetlone raportu aktualizacji w czasie rzeczywistym. Spróbuj zmienić dane w wydarzenia — temp kolekcji lub coś, jak i zostanie wyświetlony w czasie rzeczywistym efekt odzwierciedlane w pulpitu nawigacyjnego.

Uwaga, że ten samouczek wykazać, jak utworzyć, ale jednego typu wykresu dla zestawu danych. Power BI umożliwia tworzenie innego klienta narzędzi do analizy biznesowej dla Twojej organizacji. Na przykład innego pulpitu nawigacyjnego Power BI klipie wideo [Wprowadzenie do usługi Power BI](https://youtu.be/L-Z_6P56aas?t=1m58s) .

Aby uzyskać więcej informacji na temat konfigurowania wyjściowe Power BI i korzystanie z grup usługi Power BI Przejrzyj w [sekcji Power BI](stream-analytics-define-outputs.md#power-bi) [Wyświetla opis analizy strumieniu](stream-analytics-define-outputs.md "Wyświetla opis analizy strumieniu"). Inny zasób pomocne, aby dowiedzieć się więcej na temat tworzenia pulpitów nawigacyjnych dzięki usłudze Power BI jest [pulpitów nawigacyjnych w usłudze Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-dashboards/).

## <a name="limitations-and-best-practices"></a>Ograniczenia i najważniejsze wskazówki

Power BI wykorzystuje zarówno współbieżności i przepustowość ograniczenia opisane tutaj: [https://powerbi.microsoft.com/pricing](https://powerbi.microsoft.com/pricing "Power BI ceny")

Ze względu na te usługi Power BI wyładowuje się najczęściej w sposób naturalny do przypadków, w którym analizy strumieniu Azure czy zmniejszenia obciążenia dużej ilości danych.
Zaleca się przy użyciu TumblingWindow lub HoppingWindow, aby upewnić się, że wypychanych danych będzie co najwyżej 1 sekundę wypychanych i że zapytania wyładowuje w wymagania dotyczące przepustowości — za pomocą następującego równania do obliczenia wartości, aby nadać okna programu w sekundach:

![equation1](./media/stream-analytics-power-bi-dashboard/equation1.png)  

Jako przykład — jeśli masz 1000 urządzeń wysyłanie danych co sekundę, znajdują się na Power BI Pro używaną WERSJĘ obsługującego 1 000 000 wierszy na godzinę i chcesz uzyskać średnia danych na urządzeniu w usłudze Power BI możesz wykonać co najwyżej wypychanych co 4 sekundy na urządzeniu (jak pokazano poniżej):  

![equation2](./media/stream-analytics-power-bi-dashboard/equation2.png)  

Co oznacza, że firma Microsoft może zmienić oryginalny zapytania, aby:

    SELECT
        MAX(hmdt) AS hmdt,
        MAX(temp) AS temp,
        System.TimeStamp AS time,
        dspl
    INTO
        OutPBI
    FROM
        Input TIMESTAMP BY time
    GROUP BY
        TUMBLINGWINDOW(ss,4),
        dspl

### <a name="powerbi-view-refresh"></a>Odświeżanie widoku PowerBI

Często zadawane pytania jest "Dlaczego nie pulpitu nawigacyjnego automatycznej aktualizacji w PowerBI?".

Aby osiągnąć ten cel, PowerBI są używane aparatu pytania i odpowiedzi i Zadaj pytanie, takie jak "Wartość maksymalna przez temp miejsce, w którym sygnatura czasowa dzisiaj jest" i przypiąć niego do pulpitu nawigacyjnego.

### <a name="renew-authorization"></a>Odnawianie autoryzacji

Konieczne będzie uwierzytelnienie konta usługi Power BI, jeśli hasło zostały zmienione, ponieważ utworzonej lub ostatnia uwierzytelniony zadania. Jeśli uwierzytelnianie wieloskładnikowe (MFA) jest skonfigurowana na dzierżawy usługi Azure Active Directory (AAD), należy także odnowić autoryzacji Power BI co 2 tygodnie. Objawem tego problemu jest nie wyjścia zadania i "uwierzytelniania użytkownika błędu" dzienniki operacji:

![graphic12][graphic12]

Analogicznie jeśli zadanie próbuje uruchomić podczas wygasł tokenu, wystąpi błąd i rozpoczęcia zadania zakończy się niepowodzeniem. Komunikat o błędzie będzie wyglądało, takich jak poniżej:

![Błąd sprawdzania poprawności PowerBI](./media/stream-analytics-power-bi-dashboard/stream-analytics-power-bi-dashboard-token-expire.png) 
 

Aby rozwiązać ten problem, zatrzymanie uruchomionego zadania, a następnie przejdź do danych wyjściowych Power BI.  Kliknij łącze "Odnów autoryzacji", a następnie uruchom ponownie zadanie z zatrzymana podczas ostatniego Aby uniknąć utraty danych.

![Odnawianie sprawdzania poprawności PowerBI](./media/stream-analytics-power-bi-dashboard/stream-analytics-power-bi-dashboard-token-renew.png) 

Po odświeżeniu zezwolenie dzięki usłudze Power BI zostanie wyświetlony zielony alert w obszarze autoryzacji:

![Odnawianie sprawdzania poprawności PowerBI](./media/stream-analytics-power-bi-dashboard/stream-analytics-power-bi-dashboard-token-renewed.png) 

## <a name="get-help"></a>Uzyskiwanie pomocy
Aby uzyskać dodatkową pomoc spróbuj nasze [forum analizy strumieniu Azure](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Następne kroki

- [Wprowadzenie do analizy strumieniu Azure](stream-analytics-introduction.md)
- [Wprowadzenie do korzystania z analizy strumieniu Azure](stream-analytics-get-started.md)
- [Skalowanie Azure analizy strumieniu zadania](stream-analytics-scale-jobs.md)
- [Dokumentacja języka kwerend analizy strumieniu Azure](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Strumień Azure analizy zarządzania pozostałych interfejsu API odwołania](https://msdn.microsoft.com/library/azure/dn835031.aspx)


[graphic1]: ./media/stream-analytics-power-bi-dashboard/1-stream-analytics-power-bi-dashboard.png
[graphic2]: ./media/stream-analytics-power-bi-dashboard/2-stream-analytics-power-bi-dashboard.png
[graphic3]: ./media/stream-analytics-power-bi-dashboard/3-stream-analytics-power-bi-dashboard.png
[graphic4]: ./media/stream-analytics-power-bi-dashboard/4-stream-analytics-power-bi-dashboard.png
[graphic5]: ./media/stream-analytics-power-bi-dashboard/5-stream-analytics-power-bi-dashboard.png
[graphic6]: ./media/stream-analytics-power-bi-dashboard/6-stream-analytics-power-bi-dashboard.png
[graphic7]: ./media/stream-analytics-power-bi-dashboard/7-stream-analytics-power-bi-dashboard.png
[graphic8]: ./media/stream-analytics-power-bi-dashboard/8-stream-analytics-power-bi-dashboard.png
[graphic9]: ./media/stream-analytics-power-bi-dashboard/9-stream-analytics-power-bi-dashboard.png
[graphic10]: ./media/stream-analytics-power-bi-dashboard/10-stream-analytics-power-bi-dashboard.png
[graphic11]: ./media/stream-analytics-power-bi-dashboard/11-stream-analytics-power-bi-dashboard.png
[graphic12]: ./media/stream-analytics-power-bi-dashboard/12-stream-analytics-power-bi-dashboard.png
[graphic13]: ./media/stream-analytics-power-bi-dashboard/13-stream-analytics-power-bi-dashboard.png
