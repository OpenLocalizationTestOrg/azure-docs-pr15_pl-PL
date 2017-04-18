<properties 
   pageTitle="Korzystanie z widoku wykonanie wierzchołek w narzędziach Lake danych dla programu Visual Studio | Microsoft Azure" 
   description="Dowiedz się, jak używać widoku wykonanie wierzchołka do egzaminów analizy Lake danych zadań." 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="mumian" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="10/13/2016"
   ms.author="jgao"/>

# <a name="use-the-vertex-execution-view-in-data-lake-tools-for-visual-studio"></a>Korzystanie z widoku wykonanie wierzchołek w narzędziach Lake danych dla programu Visual Studio

Dowiedz się, jak za pomocą widoku wykonanie wierzchołka do egzaminów analizy Lake danych zadań.

## <a name="prerequisites"></a>Wymagania wstępne

- Niektóre podstawowe znajomość projektowanie skryptów U SQL za pomocą narzędzia Lake danych dla programu Visual Studio.  Zobacz [Samouczek: projektowania skryptów U SQL przy użyciu narzędzia Lake danych dla programu Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).

## <a name="open-the-vertex-execution-view"></a>Otwórz widok wykonanie wierzchołka

Zadania można kliknij łącze "Wyświetl wykonanie wierzchołek" w lewym dolnym rogu. Może zostać wyświetlony monit załadować profile i może upłynąć trochę czasu, w zależności od połączenia sieciowego.

![Wierzchołek wykonanie widoku narzędzia analizy Lake danych](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-open-vertex-execution-view.png)

## <a name="understand-vertex-execution-view"></a>Opis widoku wykonanie wierzchołka

Po wprowadzeniu widoku wykonanie wierzchołek, dostępne są trzy elementy:

![Wierzchołek wykonanie widoku narzędzia analizy Lake danych](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view.png)

- Selektor wierzchołek: po lewej stronie jest selektor wierzchołek.  Możesz wybrać wierzchołki przez funkcje (takie jak danych 10 głównych czytanie lub wybierz według etapów).

    Jest jednym z najczęściej używanych filtrów wierzchołki na ścieżce krytycznej. Ścieżka krytyczna jest najdłuższego ścieżka zadania U-SQL. Okazuje się przydatne dotyczące optymalizowania zadań, zaznaczając pole wyboru, które wierzchołek zajmuje najdłuższego czasu.

- Okienko górnej:

    ![Wierzchołek wykonanie widoku narzędzia analizy Lake danych](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view-pane2.png)

    W tym widoku wyświetlane również stanu działania wszystkich wierzchołki. Konwertuje czas w związku z tym do komputera lokalnego i pokazuje inny stan w różnych kolorach.

- Dolnym okienku Centrum:

    ![Wierzchołek wykonanie widoku narzędzia analizy Lake danych](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view-pane3.png)

    - Nazwa procesu: Nazwa wystąpienia wierzchołek. Składa się z różnych części w Nazwa etapu | VertexName | VertexRunInstance. Na przykład wierzchołek [62] .v1 SV7_Split oznacza drugi uruchomionego wystąpienia (.v1, indeks, począwszy od 0) liczby wierzchołek 62 w SV7_Split sceny.
    - Całkowity danych odczytu/zapisu: Dane zostały przeczytane/napisane przez ten wierzchołek.
    - Stan Stan i Zakończ: Stan końcowy po zakończeniu wierzchołek.
    - Typ wystąpił błąd i kodu wyjścia: Błąd podczas wierzchołek nie powiodło się.
    - Przyczyna utworzenia: Dlaczego wierzchołek został utworzony.
    - Zasób opóźnienie/proces opóźnienie-PN kolejki opóźnienie: czas wierzchołek czekać na zasoby, przetwarzania danych i pozostawanie w kolejce.
    - Identyfikator GUID proces-Kreator: Identyfikator GUID bieżącego wierzchołek uruchomionego lub twórcy.
    - Wersja: N-tego wystąpienia uruchomionego wierzchołek (system zaplanować nowe wystąpienia wierzchołka dla wielu powodów, na przykład awaryjnym przeniesieniu obliczyć nadmiarowości itp.)
    - Wersja jest tworzona godzina.
    - Procesu tworzenie rozpoczęcia procesu i czasu kolejce procesu i czasu rozpoczęcia procesu i czasu wykonano czasu: podczas tworzenia, uruchamiania procesu wierzchołka podczas uruchamiania procesu wierzchołek w kolejce; Gdy rozpocznie się ten proces wierzchołka; Po wykonaniu niektórych wierzchołek.

## <a name="next-steps"></a>Następne kroki

- Aby uzyskać omówienie analizy Lake danych, zobacz [Omówienie analizy Lake danych Azure](data-lake-analytics-overview.md).
- Aby rozpocząć tworzenie aplikacji U SQL, zobacz [skryptów można opracowywać U-SQL przy użyciu narzędzia Lake danych dla programu Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).
- Aby dowiedzieć się U SQL, zobacz [Wprowadzenie do języka Azure danych Lake analizy U-SQL](data-lake-analytics-u-sql-get-started.md).
- Do zadań zarządzania zobacz [Zarządzanie analizy Lake danych Azure za pomocą portalu Azure](data-lake-analytics-manage-use-portal.md).
- Aby rejestrować informacje diagnostyczne, zobacz [Uzyskiwanie dostępu do dzienników Diagnostyka Azure danych Lake analiz](data-lake-analytics-diagnostic-logs.md)
- Aby uzyskać bardziej złożonych kwerend, zobacz [Dzienniki analiza witryny sieci Web przy użyciu analizy Lake danych Azure](data-lake-analytics-analyze-weblogs.md).
- Aby wyświetlić szczegóły zadania, zobacz [Używanie przeglądarki zadania i widok zadania dla zadań analizy lake danych Azure](data-lake-analytics-data-lake-tools-view-jobs.md)