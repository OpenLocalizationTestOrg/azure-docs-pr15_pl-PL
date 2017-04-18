<properties
    pageTitle="Scenariusze zaawansowanej analizy proces i technologia Azure maszynowego uczenia | Microsoft Azure"
    description="Wybierz odpowiednie scenariusze robić zaawansowane analizy przewidywanych w procesie nauki danych zespołu."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na" 
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016" 
    ms.author="bradsev" />


# <a name="scenarios-for-advanced-analytics-in-azure-machine-learning"></a>Scenariusze zaawansowanej analizy w nauki maszynowego Azure

W tym artykule omówiono różne przykładowe źródła danych i scenariusze docelowej, które są obsługiwane przez [Zespół danych nauki proces (TDSP)](data-science-process-overview.md). TDSP zawiera systematyczną podejście do członkom zespołu do współpracy nad tworzenia aplikacji inteligentnego. Scenariusze przedstawione poniżej przedstawiono opcje dostępne w ramach przepływu pracy przetwarzania danych, zależnych od właściwości danych, lokalizacje źródłowe i repozytoria docelowej platformy Azure.

**Drzewa decyzji** do zaznaczania przykładowe scenariusze odpowiedni dla danych i celem jest przedstawiona w ostatniej sekcji.

>[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


Każdy z poniższych sekcjach przedstawiono przykładowy scenariusz. Dla każdego scenariusza nauki danych lub zaawansowanej analizy przepływu i pomocniczych Azure zasoby są wyświetlane.

>[AZURE.NOTE]**Wszystkie następujące scenariusze, musisz:**
><br/>
>* [Tworzenie konta miejsca do magazynowania](../storage/storage-create-storage-account.md)
><br/>
>* [Tworzenie obszaru roboczego Azure maszynowego uczenia](machine-learning-create-workspace.md)


## <a name="smalllocal"></a>Scenariusz \#1: małych i średnich tabelaryczny zestawu danych w plikach lokalnych

![Małych i średnich pliki lokalne][1]

#### <a name="additional-azure-resources-none"></a>Dodatkowe zasoby Azure: Brak

1.  Zaloguj się do [komputera Azure nauki Studio](https://studio.azureml.net/).

2.  Przekaż zestawu danych.

3.  Tworzenie przepływu doświadczenia Azure maszynowego uczenia, zaczynając od opublikowanych przekazane.

## <a name="smalllocalprocess"></a>Scenariusz \#2: małych i średnich zestawu danych lokalnych plików, które wymagają przetwarzania

![Małych i średnich pliki lokalne przetwarzania][2]

#### <a name="additional-azure-resources-azure-virtual-machine-ipython-notebook-server"></a>Dodatkowe zasoby Azure: Azure maszyn wirtualnych (notesu IPython server)

1.  Tworzenie Azure wirtualnych komputerze, na którym działa IPython notesu.

2.  Przekazywanie danych do kontenera magazynu Azure.

3.  Wstępne przetwarzanie i wyczyścić dane w notesie IPython dostępu do danych z kontenera magazynu Azure.

4.  Przekształcanie danych czyszczenia tabelarycznej.

5.  Zapisywanie danych przekształconych w Azure obiektów blob.

6.  Zaloguj się do [komputera Azure nauki Studio](https://studio.azureml.net/).

7.  Odczyt danych Azure blob przy użyciu [Importowanie danych] [ import-data] modułu.

8. Tworzenie przepływu doświadczenia Azure maszynowego uczenia, zaczynając od zasysanego opublikowanych.

## <a name="largelocal"></a>Scenariusz \#3: dużego zestawu danych lokalnych plików kierowanie obiektów blob platformy Azure

![Duże pliki lokalne][3]

#### <a name="additional-azure-resources-azure-virtual-machine-ipython-notebook-server"></a>Dodatkowe zasoby Azure: Azure maszyn wirtualnych (notesu IPython server)

1.  Tworzenie Azure wirtualnych komputerze, na którym działa IPython notesu.

2.  Przekazywanie danych do kontenera magazynu Azure.

3.  Wstępne przetwarzanie i wyczyścić dane w notesie IPython dostępu do danych z obiektami blob Azure.

4.  Przekształcanie danych czyszczenia formie tabelarycznej, w razie potrzeby.

5.  Eksplorowanie danych oraz tworzenie funkcje zgodnie z potrzebami.

6.  Wyodrębnianie przykładowych danych małych i średnich.

7.  Zapisz dane próbki w Azure obiektów blob.

8. Zaloguj się do [komputera Azure nauki Studio](https://studio.azureml.net/).

9. Odczyt danych Azure blob przy użyciu [Importowanie danych] [ import-data] modułu.

10. Tworzenie przepływu doświadczenia Azure maszynowego uczenia począwszy od zasysanego opublikowanych.


## <a name="smalllocaltodb"></a>Scenariusz \#4: małych i średnich zestawu danych lokalnych plików kierowanie programu SQL Server na komputerze Virtal Azure

![Małych i średnich pliki lokalne bazy danych SQL platformy Azure][4]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a>Dodatkowe zasoby Azure: Azure maszyn wirtualnych (program SQL Server i serwer notesu IPython)

1.  Tworzenie Azure wirtualnych komputerze, na którym działa program SQL Server + IPython Notes.

2.  Przekazywanie danych do kontenera magazynu Azure.

3.  Wstępne przetwarzanie i wyczyścić dane w kontenerze magazynowania Azure za pomocą IPython notesu.

4.  Przekształcanie danych czyszczenia formie tabelarycznej, w razie potrzeby.

5.  Zapisywanie danych w plikach lokalnych maszyn wirtualnych (notesu IPython pracuje maszyn wirtualnych, lokalnych dyskach twardych odwołują się do maszyn wirtualnych dyski).

6.  Ładowanie danych do bazy danych programu SQL Server uruchomionych maszyn wirtualnych Azure.

    Opcja \#1: przy użyciu programu SQL Server Management Studio.

    - Zaloguj się do programu SQL Server maszyn wirtualnych
    - Uruchom program SQL Server Management Studio.
    - Tworzenie tabel bazy danych i docelowej.
    - Użyj jednej z zbiorcze importowanie metod, aby załadować dane z lokalnego maszyn wirtualnych plików.

    Opcja \#2: za pomocą IPython notesu — nie jest zalecane dla średnich i większe zestawy danych<!-- -->    
    - Aby uzyskać dostęp do programu SQL Server na maszyn wirtualnych za pomocą parametry połączenia ODBC.
    - Tworzenie tabel bazy danych i docelowej.
    - Użyj jednej z zbiorcze importowanie metod, aby załadować dane z lokalnego maszyn wirtualnych plików.

7.  Eksplorowanie danych, tworzenie funkcje, stosownie do potrzeb. Zauważ, że funkcje nie muszą być materialized w tabelach bazy danych. Uwaga tylko niezbędne zapytania, aby je utworzyć.

8. Decyduje o rozmiarze przykładowe dane, jeśli wymagane i/lub konieczne.

9. Zaloguj się do [komputera Azure nauki Studio](https://studio.azureml.net/).

10. Przeczytaj dane bezpośrednio z programu SQL Server przy użyciu [Importowanie danych] [ import-data] modułu. Wklej niezbędne kwerendę, która wyodrębnia pola, tworzy funkcje i próbki danych, w razie potrzeby bezpośrednio w oknie [Importowanie danych] [ import-data] kwerendy.

11. Tworzenie przepływu doświadczenia Azure maszynowego uczenia począwszy od zasysanego opublikowanych.

## <a name="largelocaltodb"></a>Scenariusz \#5: dużego zestawu danych w plikach lokalnych, docelowy program SQL Server w maszyn wirtualnych Azure

![Duże pliki lokalne bazy danych SQL platformy Azure][5]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a>Dodatkowe zasoby Azure: Azure maszyn wirtualnych (program SQL Server i serwer notesu IPython)

1.  Tworzenie maszyny wirtualnej Azure z programu SQL Server i server IPython notesu.

2.  Przekazywanie danych do kontenera magazynu Azure.

3.  (Opcjonalnie) Wstępne przetwarzanie i wyczyścić dane.

    .  Wstępne przetwarzanie i wyczyścić dane w notesie IPython dostępu do danych z obiektami blob Azure.

    b.  Przekształcanie danych czyszczenia formie tabelarycznej, w razie potrzeby.

    c.  Zapisywanie danych w plikach lokalnych maszyn wirtualnych (notesu IPython pracuje maszyn wirtualnych, lokalnych dyskach twardych odwołują się do maszyn wirtualnych dyski).

4.  Ładowanie danych do bazy danych programu SQL Server uruchomionych maszyn wirtualnych Azure.

    .  Zaloguj się do programu SQL Server maszyn wirtualnych.

    b.  Jeśli dane nie zapisane już, pobierania plików danych z kontenera magazynu Azure maszyn wirtualnych lokalnego folderu.

    c.  Uruchom program SQL Server Management Studio.

    d.  Tworzenie tabel bazy danych i docelowej.

    e.  Użyj jednej z zbiorcze importowanie metod, aby załadować dane.

    f.  Jeśli tabela sprzężenia są wymagane, Utwórz indeksy, aby usprawnić sprzężenia.

     > [AZURE.NOTE] Szybsze ładowanie danych dużych rozmiarów, zalecane jest, tworzyć tabele podzielone na partycje i dane równolegle importu zbiorczego. Aby uzyskać więcej informacji zobacz [Równoległe importowanie danych do tabel na partycje SQL](machine-learning-data-science-parallel-load-sql-partitioned-tables.md).

5.  Eksplorowanie danych, tworzenie funkcje, stosownie do potrzeb. Zauważ, że funkcje nie muszą być materialized w tabelach bazy danych. Uwaga tylko niezbędne zapytania, aby je utworzyć.

6.  Decyduje o rozmiarze przykładowe dane, jeśli wymagane i/lub konieczne.

7.  Zaloguj się do [komputera Azure nauki Studio](https://studio.azureml.net/).

8. Przeczytaj dane bezpośrednio z programu SQL Server przy użyciu [Importowanie danych] [ import-data] modułu. Wklej niezbędne kwerendę, która wyodrębnia pola, tworzy funkcje i próbki danych, w razie potrzeby bezpośrednio w oknie [Importowanie danych] [ import-data] kwerendy.

9. Prosty przepływ doświadczenia Azure maszynowego uczenia, począwszy od przekazane zestawu danych

## <a name="largedbtodb"></a>Scenariusz \#6: dużego zestawu danych w programu SQL Server bazy danych lokalna, kierowanie programu SQL Server w maszyn wirtualnych Azure

![Duże SQL DB lokalna do bazy danych SQL platformy Azure][6]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a>Dodatkowe zasoby Azure: Azure maszyn wirtualnych (program SQL Server i serwer notesu IPython)

1.  Tworzenie maszyny wirtualnej Azure z programu SQL Server i server IPython notesu.

2.  Użyj jednej z danych wyeksportować metod, aby wyeksportować dane z programu SQL Server do pliku.

    > [AZURE.NOTE] Jeśli zdecydujesz się na przeniesienie wszystkich danych z bazy danych lokalna alternatywna metoda (szybsze), aby przenieść wystąpienie programu SQL Server w Azure pełnej bazy danych. Pomiń kroki eksportowania danych, utworzyć bazę danych, załaduj/importu danych na docelową bazę danych i wykonaj alternatywna metoda.

3.  Przekazywanie pliku do kontenera magazynu Azure.

4.  Ładowanie danych do bazy danych programu SQL Server uruchomiony na maszyn wirtualnych Azure.

    .  Zaloguj się do programu SQL Server maszyn wirtualnych.

    b.  Pobierz pliki danych z kontenerem Azure miejsca do magazynowania do folderu maszyn wirtualnych lokalny.

    c.  Uruchom program SQL Server Management Studio.

    d.  Tworzenie tabel bazy danych i docelowej.

    e.  Użyj jednej z zbiorcze importowanie metod, aby załadować dane.

    f.  Jeśli tabela sprzężenia są wymagane, Utwórz indeksy, aby usprawnić sprzężenia.

    > [AZURE.NOTE] Szybsze ładowanie danych dużych rozmiarów, tworzyć tabele podzielone na partycje i aby zbiorczo importować dane równolegle. Aby uzyskać więcej informacji zobacz [Równoległe importowanie danych do tabel na partycje SQL](machine-learning-data-science-parallel-load-sql-partitioned-tables.md).

5.  Eksplorowanie danych, tworzenie funkcje, stosownie do potrzeb. Zauważ, że funkcje nie muszą być materialized w tabelach bazy danych. Uwaga tylko niezbędne zapytania, aby je utworzyć.

6.  Decyduje o rozmiarze przykładowe dane, jeśli wymagane i/lub konieczne.

7.  Zaloguj się do [komputera Azure nauki Studio](https://studio.azureml.net/).

8. Przeczytaj dane bezpośrednio z programu SQL Server przy użyciu [Importowanie danych] [ import-data] modułu. Wklej niezbędne kwerendę, która wyodrębnia pola, tworzy funkcje i próbki danych, w razie potrzeby bezpośrednio w oknie [Importowanie danych] [ import-data] kwerendy.

9. Prosty przepływ doświadczenia Azure maszynowego uczenia począwszy od przekazane zestawu danych.

### <a name="alternate-method-to-copy-a-full-database-from-an-on-premises--sql-server-to-azure-sql-database"></a>Alternatywna metoda, aby skopiować pełnej bazy danych z lokalnego programu SQL Server do bazy danych SQL Azure

![Odłączanie lokalnej bazy danych i dołączyć do bazy danych SQL platformy Azure][7]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a>Dodatkowe zasoby Azure: Azure maszyn wirtualnych (program SQL Server i serwer notesu IPython)

Aby odtworzyć całej bazy danych programu SQL Server w maszyn wirtualnych usługi SQL Server, należy kopiować bazy danych z jednej lokalizacji/serwer do innego, przy założeniu, że baza danych może być podjęta tymczasowo w trybie offline. W tym Eksplorator obiektów SQL Server Management Studio lub za pomocą odpowiednich poleceń w języku Transact-SQL.

1. Odłącz bazę danych w lokalizacji źródłowej. Aby uzyskać więcej informacji zobacz [Odłącz bazy danych](https://technet.microsoft.com/library/ms191491(v=sql.110).aspx).
2. W oknie Eksploratora Windows lub wiersza polecenia systemu Windows skopiuj plik odłączony bazy danych lub pliki i pliku dziennika lub pliki do lokalizacji docelowej w maszyn wirtualnych serwera SQL platformy Azure.
3. Dołącz pliki, skopiowany wystąpienie programu SQL Server docelowej. Aby uzyskać więcej informacji zobacz [Dołączanie bazy danych](https://technet.microsoft.com/library/ms190209(v=sql.110).aspx).

[Przenoszenie bazy danych przy użyciu Odłączanie i dołączanie (Transact-SQL)](https://technet.microsoft.com/library/ms187858(v=sql.110).aspx)

## <a name="largedbtohive"></a>Scenariusz \#7: duży danych w plikach lokalnych docelowej bazy danych gałęzi w klastrów Azure HDInsight Hadoop

![Duży danych w docelowej lokalne gałęzi][9]

#### <a name="additional-azure-resources-azure-hdinsight-hadoop-cluster-and-azure-virtual-machine-ipython-notebook-server"></a>Dodatkowe zasoby Azure: Azure HDInsight Hadoop klaster i maszyn wirtualnych Azure (notesu IPython server)

1.  Tworzenie maszyny wirtualnej Azure usługami IPython notesu.

2.  Utwórz klaster Azure HDInsight Hadoop.

3.  (Opcjonalnie) Wstępne przetwarzanie i wyczyścić dane.

    .  Wstępne przetwarzanie i wyczyścić dane w notesie IPython dostępu do danych z obiektami blob Azure.

    b.  Przekształcanie danych czyszczenia formie tabelarycznej, w razie potrzeby.

    c.  Zapisywanie danych w plikach lokalnych maszyn wirtualnych (notesu IPython pracuje maszyn wirtualnych, lokalnych dyskach twardych odwołują się do maszyn wirtualnych dyski).

4.  Przekazywanie danych do domyślnego kontenera klaster Hadoop wybrany w kroku 2.

5.  Ładowanie danych do bazy danych programu Hive w klastrze Azure HDInsight Hadoop.

    .  Zaloguj się do węzła głównego klastrze Hadoop

    b.  Otwórz okno wiersza polecenia Hadoop.

    c.  Wprowadź katalog główny gałęzi przy użyciu polecenia `cd %hive_home%\bin` w wierszu polecenia Hadoop.

    d.  Uruchamianie kwerend gałęzi, aby utworzyć bazę danych i tabele, a następnie załadować dane z magazynem obiektów blob do tabel gałęzi.

    > [AZURE.NOTE] Jeśli dane nie jest duży, użytkownicy mogą tworzyć tabelę programu Hive z partycje. Następnie, użytkownicy mogą używać `for` pętli w Hadoop wiersza polecenia węzła głównego aby załadować dane do tabeli gałęzi podzielona według partycją.

6.  Eksplorowanie danych i Utwórz funkcje, w razie potrzeby w wierszu polecenia Hadoop. Zauważ, że funkcje nie muszą być materialized w tabelach bazy danych. Uwaga tylko niezbędne zapytania, aby je utworzyć.

    .  Zaloguj się do węzła głównego klastrze Hadoop

    b.  Otwórz okno wiersza polecenia Hadoop.

    c.  Wprowadź katalog główny gałęzi przy użyciu polecenia `cd %hive_home%\bin` w wierszu polecenia Hadoop.

    d.  Uruchamianie kwerendy gałęzi w wierszu polecenia Hadoop głowy węzeł klaster Hadoop eksplorować dane i tworzyć funkcje zgodnie z potrzebami.

7.  Jeśli wymagane i/lub potrzeby, przykładowych danych do dopasowania Azure maszynowego uczenia Studio.

8.  Zaloguj się do [komputera Azure nauki Studio](https://studio.azureml.net/).

9. Odczytać dane bezpośrednio z `Hive Queries` przy użyciu [Importowanie danych] [ import-data] modułu. Wklej niezbędne kwerendę, która wyodrębnia pola, tworzy funkcje i próbki danych, w razie potrzeby bezpośrednio w oknie [Importowanie danych] [ import-data] kwerendy.

10. Prosty przepływ doświadczenia Azure maszynowego uczenia począwszy od przekazane zestawu danych.

## <a name="decisiontree"></a>Algorytm scenariuszy wyboru
------------------------

Poniższy diagram przedstawia scenariusze opisanych powyżej i proces zaawansowane analizy i wybór technologii wprowadzone, które prowadzą do każdego z wyszczególnione scenariuszy. Należy zauważyć, że przetwarzania danych, poszukiwanie, funkcja techniczny i pobierania może potrwać umieścić w metody środowiska — u źródła, średniozaawansowany, lub środowiskach docelowej — i można kontynuować wielokrotnie powtarzane, stosownie do potrzeb. Diagram tylko służy jako zapoznać się z ilustracją niektóre możliwe przepływów i nie umożliwia wyliczenie pełnego.

![Przykładowe Zasadami proces Instruktaż scenariusze][8]

### <a name="advanced-analytics-in-action-examples"></a>Zaawansowane analizy w działaniu przykładów

Aby uzyskać instruktaży Azure maszynowego uczenia zakończenia do końca proces zaawansowane analizy i za pomocą zestawy danych publicznych Zobacz:


* [Proces nauki danych zespołu w działaniu: przy użyciu programu SQL Server](machine-learning-data-science-process-sql-walkthrough.md).
* [Proces nauki danych zespołu w działaniu: używania klastrów HDInsight Hadoop](machine-learning-data-science-process-hive-walkthrough.md).


[1]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-small-in-aml.png
[2]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-local-with-processing.png
[3]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-local-large.png
[4]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-local-to-db.png
[5]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-large-to-db.png
[6]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-db-to-db.png
[7]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-attach-db.png
[8]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-sample-scenarios.png
[9]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-local-to-hive.png


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
