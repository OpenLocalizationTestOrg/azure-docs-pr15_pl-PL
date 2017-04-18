<properties
    pageTitle="Dane wyjściowe JSON analizy strumieniu | Microsoft Azure"
    description="Dowiedz się, jak analizy strumieniu można kierować Azure DocumentDB wyników JSON, archiwizowanie danych i kwerendy małym opóźnieniem niestrukturalne danych JSON."
    keywords="Wynik JSON"
    documentationCenter=""
    services="stream-analytics,documentdb"
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

# <a name="target-azure-documentdb-for-json-output-from-stream-analytics"></a>DocumentDB Azure docelowej dla JSON wyniki analizy strumieniu

Analizy strumieniu można kierować [Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) dla JSON wyprowadzenia, włączanie archiwizacji i małym opóźnieniem kwerend danych bez struktury danych JSON. Dowiedz się, jak najlepiej wdrażać integracja.

Dla osób, które nie zna DocumentDB zapoznaj się [ścieżka nauki DocumentDB i](https://azure.microsoft.com/documentation/learning-paths/documentdb/) rozpocząć pracę.

## <a name="basics-of-documentdb-as-an-output-target"></a>Podstawowe informacje o DocumentDB jako obiekt docelowy format wyjściowy
Wynik Azure DocumentDB w analizy strumieniu umożliwia zapisywanie strumienia przetwarzania wyniki jako wynik JSON do swojego zbiory DocumentDB. Analizy strumieniu nie tworzy zbiory w bazie danych, zamiast tego wymaga utworzone ustalonymi. Jest tak, aby były niewidoczne dla Ciebie rozliczeń kosztów DocumentDB zbiorów i dlatego można dostosować wydajność, spójności i pojemność kolekcji bezpośrednio przy użyciu [Interfejsów API DocumentDB](https://msdn.microsoft.com/library/azure/dn781481.aspx). Zalecamy korzystanie z jednej DocumentDB bazy danych na przesyłanie strumieniowe zadanie logicznie oddzielanie kolekcji przesyłanie strumieniowe zadania.

Niektóre opcje zbioru DocumentDB szczegółowo opisano niżej.

## <a name="tune-consistency-availability-and-latency"></a>Dostosowywanie spójności, dostępność i opóźnienie

Zgodnie z wymaganiami aplikacji, DocumentDB pozwala precyzyjnie dostosować bazy danych i zbiorów i korzystnych rozwiązań między spójności, dostępność i opóźnienie. W zależności od poziomy odczytu spójności potrzeb scenariusz przed i zapis opóźnienie, możesz wybrać poziom spójności na Twoim koncie bazy danych. Domyślnie DocumentDB umożliwia także synchroniczne indeksowania na każdej operacji OBSŁUGIWAŁ do kolekcji. To jest inną opcję przydatne do sterowania wydajność odczytu/zapisu w DocumentDB. Aby uzyskać więcej informacji na ten temat przeczytaj artykuł [Zmienianie poziomów spójności z bazy danych i kwerendy](../documentdb/documentdb-consistency-levels.md) .

## <a name="choose-a-performance-level"></a>Wybierz poziom wydajności

Kolekcje DocumentDB mogą być tworzone w 3 różne poziomy wydajności (S1, S2 lub S3), które określają przepustowość dostępna dla CRUDs do tego zbioru. Ponadto wydajność wpływa poziomów indeksowania spójności w kolekcji. Zobacz opis poziomów tych wydajności szczegółowo [w tym artykule](../documentdb/documentdb-performance-levels.md) .

## <a name="upserts-from-stream-analytics"></a>Upserts z analizy strumieniu

Integracja analizy strumieniu z DocumentDB umożliwia wstawianie lub zaktualizuj rekordy do kolekcji DocumentDB na podstawie określonej kolumny Identyfikatora dokumentu. Jest to także nazywane *Upsert*.

Analizy strumieniu wykorzystuje optymistyczny rozwiązaniem Upsert, w którym aktualizacje są tylko wykonane, gdy Wstawianie kończy się niepowodzeniem z powodu konflikt identyfikatorów dokumentów. Tej aktualizacji jest wykonywane przez analizy strumieniu jako poprawki, więc umożliwia częściowej aktualizacji do dokumentu, to znaczy dodanie nowe właściwości lub zastąpieniu stopniowo zapasowej istniejącej właściwości. Pamiętaj, że zmiany w polu wartości właściwości tablicy w wyniku dokumentu JSON w całej tabeli wprowadzenie zastąpione, to znaczy tablicy nie są scalane.

## <a name="data-partitioning-in-documentdb"></a>Dane podziału w DocumentDB

Kolekcje DocumentDB umożliwiają dzielenia danych na podstawie wzorców kwerendy i wymagań wydajności aplikacji. Każda z kolekcji może zawierać do 10GB danych (maksymalnie) i obecnie nie istnieje metoda do rozbudowy (lub przepełnienia) zbioru. Skalowanie zewnętrzne, analizy strumieniu umożliwia do wielu zbiorów z danego prefiksu zapisu (zobacz poniżej informacje dotyczące sposobu użycia). Analizy strumieniu używa spójnych strategii [Rozpoznawania nazw Partition mieszania](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.partitioning.hashpartitionresolver.aspx) na podstawie użytkownika opisane PartitionKey kolumny, aby podzielić swoje rekordy dane wyjściowe. Liczba zbiorów z danego prefiksu podczas uruchamiania przesyłanie strumieniowe zadania jest używany jako liczba partition wynik, do którego zadanie zapisuje równolegle (zbiory DocumentDB = wynik partycje). Dla pojedynczego S3 zbiór z opóźnieniem praktyce indeksowania tylko wstawia, o 0,4, których można się spodziewać przepustowość zapisu MB/s. Przy użyciu wielu zbiorów umożliwia osiągnięcia wyższej przepustowości i lepszą wydajność.

Jeśli zamierzasz zwiększyć licznik partition w przyszłości, może być konieczne zatrzymanie zadania, partycje danych z istniejącej kolekcji do nowej kolekcji, a następnie ponownie uruchom zadanie analizy strumieniu. Więcej informacji na temat używania PartitionResolver i ponownie podziału wraz z przykładowy kod, zostaną uwzględnione we wpisie monitowania. Artykuł [podziału w DocumentDB](../articles/documentdb-partition-data.md#developing-a-partitioned-application) są także szczegółowe informacje na temat.

## <a name="documentdb-settings-for-json-output"></a>Ustawienia DocumentDB JSON wyników

Tworzenie DocumentDB jako wynik w analizy strumieniu generuje monit o podanie informacji, jak pokazano poniżej. Ta sekcja zawiera wyjaśnienie definicji właściwości.

![ekran wynik analizy strumieniu documentdb](media/stream-analytics-documentdb-output/stream-analytics-documentdb-output.png)  

-   **Wyjściowy Alias** — tego raportu w kwerendzie ASA odwoływanie się aliasu  
-   **Nazwa konta** — nazwy lub identyfikatora URI konta DocumentDB punktu końcowego.  
-   **Klucz konta** — klucz udostępniony dostępu do konta DocumentDB.  
-   **Bazy danych** — DocumentDB Nazwa bazy danych.  
-   **Symbolu wieloznacznego zbioru** — wzorca nazwy zbioru dla zbiorów ma być używany. Format nazwy zbioru może być wykonane przy użyciu tokenu opcjonalne {partition}, gdzie partycje zaczynać się od 0. Poniżej przedstawiono przykładowy prawidłowych danych wejściowych:  
   1\) MyCollection — musi istnieć w jednej kolekcji o nazwie "MyCollection".  
   2\) MyCollection {partition} — takie zbiory, musi istnieć — "MyCollection0", "MyCollection1", "MyCollection2" i tak dalej.  
-   **Klucz partition** — nazwę pola wydarzeń dane wyjściowe używane do określania klucza do podziału dane wyjściowe między zbiorami. Wyników pojedynczą kolekcję dowolnej kolumny dowolnego dane wyjściowe mogą być używane PartitionId np.  
-   **Identyfikator dokumentu** — opcjonalnie. Nazwa pola wyjściowe wydarzeń można określić klucz podstawowy, na które insert lub update operacji są oparte.  
