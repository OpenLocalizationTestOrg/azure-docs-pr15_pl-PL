<properties
   pageTitle="Działania magazynu kwerendy w bazie danych Azure SQL"
   description="Dowiedz się, jak działają w sklepie kwerendy w bazie danych SQL Azure"
   keywords=""
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="sqldb-performance"
   ms.workload="data-management"
   ms.date="08/16/2016"
   ms.author="carlrab"/>

# <a name="operating-the-query-store-in-azure-sql-database"></a>Działania w sklepie kwerendy w bazie danych Azure SQL 

Kwerendy magazynu platformy Azure to funkcja bazy danych w pełni zarządzane nieustannie zbiera i zawierają szczegółowe informacje historyczne o wszystkich kwerend. Temat sklepu kwerendy można traktować jako podobne do Rejestrator danych lotu samolotem, które znacznie upraszcza rozwiązywanie problemów z obu chmury wydajności kwerend i klienci lokalnego. W tym artykule opisano określonych aspektów operacyjnym kwerendy magazynu platformy Azure. Przy użyciu tej kwerendy wstępnie zebranych danych, możesz szybko diagnozowanie i rozwiązywanie problemów z wydajnością i więc poświęcić więcej czasu na skoncentrowanie się na ich działalności. 

Zapytania ze sklepu została [globalnie dostępnej](https://azure.microsoft.com/updates/general-availability-azure-sql-database-query-store/) w bazie danych SQL Azure od listopada 2015 r. Magazyn kwerendy jest podstawą analizy wydajności i dostosowywanie funkcje, takie jak [Advisor bazy danych SQL i pulpit nawigacyjny wydajności](https://azure.microsoft.com/updates/sqldatabaseadvisorga/). W momencie publikowania w tym artykule sklepu kwerendy działa w bazach danych więcej niż 200 000 użytkowników w Azure, zbieranie informacji związanych z zapytania przez kilka miesięcy, bez zakłóceń.

> [AZURE.IMPORTANT] Microsoft Trwa aktywowanie sklepu kwerendy dla wszystkich baz danych programu SQL Azure (istniejących i nowy). 

## <a name="optimal-query-store-configuration"></a>Konfiguracja optymalnego magazynu kwerendy

W tej sekcji opisano ustawienia domyślne optymalną konfigurację, które pozwalają zapewnić niezawodne działanie przechowywanie kwerendy i zależne funkcje, takie jak [Advisor bazy danych SQL i pulpit nawigacyjny wydajności](https://azure.microsoft.com/updates/sqldatabaseadvisorga/). Domyślna konfiguracja jest zoptymalizowana pod kątem zbierania danych ciągłego, jest minimalnego czasu poświęconego Państw OFF-READ_ONLY.

| Konfiguracja | Opis | Domyślne | Komentarz |
| ------------- | ----------- | ------- | ------- |
| MAX_STORAGE_SIZE_MB | Określa limit miejsca danych, jaki sklepu kwerendy może zająć w bazie danych klienta z | 100 | Wymuszane dla nowych baz danych |
| INTERVAL_LENGTH_MINUTES | Określa rozmiar przedział czasu, w którym agregacją i trwała statystyki zbierane runtime do planów kwerendy. Każdy plan aktywnej kwerendy zawiera co najwyżej jeden wiersz w okresie czasu zdefiniowanego w tej konfiguracji | 60   | Wymuszane dla nowych baz danych |
| STALE_QUERY_THRESHOLD_DAYS | Zasady Oczyszczanie opartych na czasie, który steruje okres przechowywania statystyki runtime trwałe i nieaktywne kwerend | 30 | Wymuszane dla nowych baz danych i baz danych z poprzedniej domyślnym (367) |
| SIZE_BASED_CLEANUP_MODE | Określa, czy oczyszczanie automatyczne danych ma być wykonywana po rozmiar danych sklepu kwerendy zbliża się do granicy | Automatycznie | Wymuszane dla wszystkich baz danych |
| QUERY_CAPTURE_MODE | Określa, czy wszystkie kwerendy lub tylko niektóre kwerendy są śledzone | Automatycznie | Wymuszane dla wszystkich baz danych |
| FLUSH_INTERVAL_SECONDS | Określa, że okres, w którym zostały przechwycone, środowisko uruchomieniowe, które statystyki są przechowywane w pamięci, przed opróżniania na dysku | 900 | Wymuszane dla nowych baz danych |
||||||

> [AZURE.IMPORTANT] Te wartości domyślne są stosowane automatycznie w ostatnim etapie aktywacji sklepu kwerendy w wszystkie bazy danych programu SQL Azure (patrz poprzedzającego ważna uwaga). Po powyższe w górę bazy danych SQL Azure nie zmiana wartości konfiguracji przez klientów, o ile one mieć negatywny wpływ na obciążenie pracą podstawowego lub zaufanego operacji magazynu kwerendy.

Jeśli chcesz otrzymywać przy użyciu ustawień niestandardowych, do przywrócenia konfiguracji do poprzedniego stanu należy użyć [ALTER DATABASE z opcjami sklepu kwerendy](https://msdn.microsoft.com/library/bb522682.aspx) . Zapoznaj się z [Najważniejsze wskazówki ze sklepem zapytania](https://msdn.microsoft.com/library/mt604821.aspx) , aby dowiedzieć się, jak góry zignorowanych parametry optymalnego konfiguracji.

## <a name="next-steps"></a>Następne kroki

[Wgląd wydajności bazy danych SQL](sql-database-performance.md)

## <a name="additional-resources"></a>Dodatkowe zasoby

Aby uzyskać więcej informacji o wyewidencjonowanie następujące artykuły:

- [Rejestrator danych lotu dla bazy danych](https://azure.microsoft.com/blog/query-store-a-flight-data-recorder-for-your-database) 

- [Monitorowanie wydajności przy użyciu magazynu kwerendy](https://msdn.microsoft.com/library/dn817826.aspx)

- [Scenariusze użycia magazynu kwerendy](https://msdn.microsoft.com/library/mt614796.aspx)

- [Monitorowanie wydajności przy użyciu magazynu kwerendy](https://msdn.microsoft.com/library/dn817826.aspx) 
