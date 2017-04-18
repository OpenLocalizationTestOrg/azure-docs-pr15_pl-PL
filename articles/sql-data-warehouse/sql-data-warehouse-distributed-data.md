<properties
   pageTitle="Rozkładu danych i distributed Opcje tabeli dla systemów znacznym stopniu równoległego przetwarzania (MPP) magazynu danych SQL i Parallel Data Warehouse | Microsoft Azure"
   description="Dowiedz się, jak dane są przesyłane w znacznym stopniu równoległego przetwarzania (MPP) i opcji dystrybucji tabel w magazynie danych SQL Azure i Parallel Data Warehouse."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="10/10/2016"
   ms.author="barbkess"/>


# <a name="distributed-data-and-distributed-tables-for-massively-parallel-processing-mpp"></a>Rozłożone danych i tabele rozłożone dla znacznym stopniu równoległego przetwarzania (MPP)

Dowiedz się, jak dane użytkownika są przesyłane w magazynu danych SQL Azure i Parallel Data Warehouse, które są systemy znacznym stopniu równoległego przetwarzania (MPP) firmy Microsoft. Projektowanie magazynu danych Aby efektywnie używać danych rozłożone ułatwia uzyskanie przetwarzania zalety architektury MPP kwerend. Kilka opcji projekt bazy danych może mieć wpływ na temat zwiększania wydajności zapytania.  

>[AZURE.NOTE] Azure SQL Data Warehouse i Parallel Data Warehouse za pomocą samego projektu znacznym stopniu równoległego przetwarzania (MPP), ale mają one kilka różnic ze względu na platformie źródłowych. Program SQL Data Warehouse jest platformą jako usługa (PaaS), która nie działa na Azure. Parallel Data Warehouse działa na analizy systemu platformy (dostępu), czyli urządzenia lokalne uruchamianej na serwerze systemu Windows.

## <a name="what-is-distributed-data"></a>Co to są rozłożone dane?

W magazynie danych SQL i Parallel Data Warehouse rozłożone danych odwołuje się do danych użytkownika, który jest przechowywany w wielu lokalizacjach systemie MPP. Każdą z tych lokalizacji działa jako jednostki uruchamianej kwerend na jego część danych i niezależnych magazynowania. Rozłożone danych jest podstawą do wykonywania kwerend równolegle uzyskanie kwerendy wysokiej wydajności.

Aby rozmieścić dane, magazynu danych przypisuje każdego wiersza tabeli użytkownika na jedną lokalizację rozłożone.  Można rozmieścić tabele z metody dystrybucji skrótu lub okrężny. Metody dystrybucji jest określona w instrukcji CREATE TABLE. 

## <a name="hash-distributed-tables"></a>Distributed mieszania tabel
  
Na poniższym diagramie przedstawiono, jak podwójne (Tabela distributed) są przechowywane jako tabelę distributed skrótu. Funkcji deterministycznych przypisuje każdego wiersza, do której będzie należał jednym rozkładzie. W definicji tabeli jednej z kolumn jest wyznaczany jako kolumnę rozkładu. Funkcja mieszania używa wartości w kolumnie rozkładu przypisanie każdego wiersza do rozkładu.

Istnieją zagadnienia dotyczące wydajności do zaznaczania kolumnę rozkładu, takie jak odrębności, funkcja skośność danych i typy kwerend uruchamiać w systemie.
  
![Tabela rozłożone] (media/sql-data-warehouse-distributed-data/hash-distributed-table.png "Tabela rozłożone")  
  
-   Każdy wiersz należy do jednego rozkładu.  
  
-   Algorytm skrótu deterministyczne przypisuje każdy wiersz jednego rozkładowi.  
  
-   Liczba wierszy tabeli na rozkład różni się wskazywanych przez różne rozmiary tabel.

## <a name="round-robin-distributed-tables"></a>Tabele rozłożone karuzeli

Tabeli rozłożone karuzeli rozdziela wiersze przypisując każdego wiersza do rozkładu w sposób sekwencyjne. Jest szybkie załadować dane do tabeli karuzeli, ale wydajność kwerendy może być mniejsza.  Dołączanie do tabeli karuzeli zazwyczaj wymaga losowego grupowania wiersze, które chcesz włączyć kwerendy da wynik dokładności, które czas.

## <a name="distributed-storage-locations-are-called-distributions"></a>Rozłożone lokalizację są nazywane dystrybucji

Każda lokalizacja rozłożone nosi nazwę rozkładu. Po uruchomieniu zapytania równolegle każdego rozkładu wykonuje zapytania SQL w jego części danych. SQL Data Warehouse używa bazy danych SQL, aby uruchomić kwerendę. Parallel Data Warehouse używa programu SQL Server, aby uruchomić kwerendę. Ten projekt architektura udostępnione żaden element nie jest do osiągnięcia poza skalowanie równoległego korzystania z komputera.

### <a name="can-i-view-the-distributions"></a>Czy można wyświetlić dystrybucji?

Każdy dystrybucyjnych ma identyfikator dystrybucji i jest widoczny w widokach systemu, które dotyczą magazynu danych SQL i Parallel Data Warehouse. Identyfikator dystrybucji umożliwia rozwiązywanie problemów z wydajnością kwerend i innych problemów. Aby uzyskać listę widoków systemowych zobacz [widoku systemu MPP](sql-data-warehouse-reference-tsql-statements.md).

## <a name="difference-between-a-distribution-and-a-compute-node"></a>Różnica między rozkładu i węzeł obliczeń

Rozkładu to podstawowa jednostka rozłożone dane są przechowywane i przetwarzania zapytania równolegle. Rozkład są pogrupowane w węzłach obliczeń. Każdy węzeł obliczeń śledzi dystrybucji jeden lub więcej.  

-   System platformy analiz używa węzły obliczeń jako główny składnik możliwości sprzętowe architektura i skala w nowym oknie. Używa zawsze osiem dystrybucji na węzeł obliczeń, jak pokazano na diagramie distributed mieszania tabel. Liczby węzłów obliczeń i w związku z tym liczba dystrybucji, zależy od liczby węzłów obliczeń, które zakupu dla urządzenia. Na przykład w przypadku zakupu osiem węzłów obliczeń, możesz uzyskać 64 dystrybucji (8 obliczeń węzły x 8 dystrybucji węzeł). 

-   Magazyn danych SQL ma stałej liczby 60 dystrybucji i elastyczne liczby węzłów obliczeń. Węzły obliczeń są stosowane Azure zasoby przetwarzania i miejsca do magazynowania. Liczby węzłów obliczeń można zmienić zgodnie z pracą usługi wewnętrznej bazy danych i wydajności obliczeniowej (DWUs), można określić dla magazynu danych. Po zmianie liczby węzłów obliczeń liczba dystrybucji na węzeł obliczeń również zmian. 

### <a name="can-i-view-the-compute-nodes"></a>Czy można wyświetlić węzły obliczeń?

Każdy węzeł obliczeń ma identyfikator węzła i jest widoczny w widokach systemu, które dotyczą magazynu danych SQL i Parallel Data Warehouse.  Węzeł obliczeń widoczny, wyświetlając w kolumnie node_id w widokach systemu, których nazwiska zaczynają się od sys.pdw_nodes. Aby uzyskać listę widoków systemowych zobacz [widoku systemu MPP](sql-data-warehouse-reference-tsql-statements.md).

## <a name="Replicated"></a>Replikowane tabel dla magazynu równoległe danych 
  
Dotyczy: równoległa magazynu danych

Oprócz przy użyciu tabel rozłożone, Parallel Data Warehouse oferuje opcję, aby odtworzyć tabele. *Replikowane tabeli* jest tabelą, która znajduje się w całości na każdym węźle obliczeń. Replikacja tabeli usuwa konieczność przeniesienia jego wiersze tabeli między węzły obliczeń przed użyciem tabeli w połączeniu lub agregacji. Tabele replikowane są tylko możliwe z tabelami małych ze względu na dodatkowe miejsce do magazynowania wymaganych do przechowania pełny tabeli w każdym węźle obliczeń.  
  
Na poniższym diagramie przedstawiono zreplikowanej tabeli, który jest przechowywany w każdym węźle obliczeń. Zreplikowanej tabeli są przechowywane na wszystkich dyskach przypisane do węzła obliczeń. Niniejsza strategia dysku jest wykonywana przy użyciu grup programu SQL Server.  
  
![Tabela zreplikowany] (media/sql-data-warehouse-distributed-data/replicated-table.png "Tabela zreplikowany") 
  
## <a name="next-steps"></a>Następne kroki
  
Efektywnego korzystania z rozkładem tabel, zobacz [tabele Dystrybucja w magazynie danych SQL](sql-data-warehouse-tables-distribute.md)  
  



  
