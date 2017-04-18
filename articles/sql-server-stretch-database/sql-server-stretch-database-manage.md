<properties
    pageTitle="Zarządzanie i rozwiązywanie problemów z bazy danych rozciąganie | Microsoft Azure"
    description="Dowiedz się, jak zarządzanie i rozwiązywanie problemów z rozciąganie bazy danych."
    services="sql-server-stretch-database"
    documentationCenter=""
    authors="douglaslMS"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-server-stretch-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/27/2016"
    ms.author="douglasl"/>

# <a name="manage-and-troubleshoot-stretch-database"></a>Zarządzanie i rozwiązywanie problemów z rozciąganie bazy danych

Zarządzanie i rozwiązywanie problemów z rozciąganie bazy danych, korzystając z metod opisanych w tym temacie i narzędzi.

## <a name="manage-local-data"></a>Zarządzanie dane lokalne

### <a name="LocalInfo"></a>Uzyskiwanie informacji na temat lokalnej bazy danych i tabele włączone dla rozciąganie bazy danych
Otwórz widoków wykazu **sys.databases** i **sys.tables** , aby wyświetlić informacje o rozciąganie\-włączone bazy danych programu SQL Server i tabele. Aby uzyskać więcej informacji zobacz [sys.databases (w języku Transact-SQL)](https://msdn.microsoft.com/library/ms178534.aspx) i [sys.tables (w języku Transact-SQL)](https://msdn.microsoft.com/library/ms187406.aspx).

Aby sprawdzić, ile miejsca rozciąganie\-w programie SQL Server, uruchom następującą instrukcję korzysta z obsługą tabeli.

 ```tsql
USE <Stretch-enabled database name>;
GO
EXEC sp_spaceused '<Stretch-enabled table name>', 'true', 'LOCAL_ONLY';
GO
 ```
## <a name="manage-data-migration"></a>Zarządzanie migracji danych

### <a name="check-the-filter-function-applied-to-a-table"></a>Sprawdź funkcję filtr zastosowany do tabeli
Otwieranie widoku katalogu **sys.remote\_danych\_archiwum\_tabele** i sprawdź wartość **filtru\_predykat** kolumny do identyfikowania funkcji, która używa rozciąganie bazy danych wybierz wiersze do migrowania. Jeśli wartość jest pusta, całej tabeli jest uprawniony do migracji. Aby uzyskać więcej informacji zobacz [sys.remote_data_archive_tables (w języku Transact-SQL)](https://msdn.microsoft.com/library/dn935003.aspx) i [Zaznacz wiersze, aby przeprowadzić migrację przy użyciu funkcji Filtr](sql-server-stretch-database-predicate-function.md).

### <a name="Migration"></a>Sprawdzanie stanu migracji danych
Wybierz pozycję zadania **| Rozciąganie | Monitorowanie** dla z bazą danych programu SQL Server Management Studio monitorowanie migracji danych w rozciąganie Monitor bazy danych. Aby uzyskać więcej informacji, zobacz [Monitor i rozwiązywanie problemów z migracji danych (baza danych rozciąganie)](sql-server-stretch-database-monitor.md).

Lub Otwórz view dynamiczne zarządzanie **sys.dm\_db\_rda\_migracji\_stanu** aby zobaczyć, ile partie i wierszy danych przeprowadzeniu migracji.

### <a name="Firewall"></a>Rozwiązywanie problemów z migracji danych
Do rozwiązywania problemów sugestie, zobacz [Monitor i rozwiązywanie problemów z migracji danych (baza danych rozciąganie)](sql-server-stretch-database-monitor.md).

## <a name="manage-remote-data"></a>Zarządzanie danymi zdalnych

### <a name="RemoteInfo"></a>Uzyskiwanie informacji na temat zdalnej bazy danych i tabele używane przez rozciąganie bazy danych
Otwórz widoków wykazu **sys.remote\_danych\_archiwum\_baz danych** i **sys.remote\_danych\_archiwum\_tabel** celu wyświetlenia informacji na temat zdalnej bazy danych i tabele, w których przechowywane są migrowane dane. Aby uzyskać więcej informacji zobacz [sys.remote_data_archive_databases (w języku Transact-SQL)](https://msdn.microsoft.com/library/dn934995.aspx) i [sys.remote_data_archive_tables (w języku Transact-SQL)](https://msdn.microsoft.com/library/dn935003.aspx).

Aby sprawdzić, ile miejsca tabeli z obsługą rozciąganie korzysta z platformy Azure, uruchom następującą instrukcję.

 ```tsql
USE <Stretch-enabled database name>;
GO
EXEC sp_spaceused '<Stretch-enabled table name>', 'true', 'REMOTE_ONLY';
GO
 ```

### <a name="delete-migrated-data"></a>Usuwanie danych migracją  
Jeśli chcesz usunąć dane, które już została poddana migracji Azure, wykonaj kroki opisane w [sys.sp_rda_reconcile_batch](https://msdn.microsoft.com/library/mt707768.aspx).

## <a name="manage-table-schema"></a>Zarządzanie schematem tabeli

### <a name="dont-change-the-schema-of-the-remote-table"></a>Nie należy zmieniać schemat tabeli zdalnej
Nie należy zmieniać schemat tabeli Azure zdalnej, który jest skojarzony z tabelą programu SQL Server skonfigurowane dla rozciąganie bazy danych. W szczególności nie Modyfikuj nazwy lub typu danych kolumny. Funkcja rozciąganie bazy danych służy do różnych założenia dotyczące schematu programu tabeli zdalnej względem schematu tabeli programu SQL Server. Jeśli zmienisz zdalnego schematu rozciąganie bazy danych przestaje uruchamianiem zmienioną tabelę.

### <a name="reconcile-table-columns"></a>Uzgadnianie kolumn tabeli  
Jeśli przypadkowo usunięte kolumn z tabeli zdalnej, uruchom **sp_rda_reconcile_columns** , aby dodać kolumny do tabeli zdalnej, która istnieje w rozciąganie\-włączone tabeli programu SQL Server, ale nie w tabeli zdalnej. Aby uzyskać więcej informacji zobacz [sys.sp_rda_reconcile_columns](https://msdn.microsoft.com/library/mt707765.aspx).  

  > [!IMPORTANT] **Sp_rda_reconcile_columns** odtwarza kolumn, które przypadkowemu usunięciu z tabeli zdalnej, nie powoduje przywrócenia danych, który był wcześniej w kolumnach usunięte.

**sp_rda_reconcile_columns** nie powoduje usunięcia kolumn z tabeli zdalnej, która istnieje w tabeli zdalnej, ale nie w rozciąganie\-włączone tabeli programu SQL Server. W przypadku kolumn w tabeli Azure zdalnego, które nie są już istnieje w rozciąganie\-włączone programu SQL Server tabeli, te dodatkowe kolumny nie uniemożliwia bazy danych rozciąganie normalnego działania. Opcjonalnie można ręcznie usunąć dodatkowe kolumny.  

## <a name="manage-performance-and-costs"></a>Zarządzanie kosztów i wydajności  

### <a name="troubleshoot-query-performance"></a>Rozwiązywanie problemów z wydajnością kwerend
Zapytania, które uwzględnić rozciąganie\-enabled tabele mają wykonywać wolniej, niż jak przed tabele zostały włączone dla rozciąganie. Jeśli znacznie oznacza spadek wydajności kwerendy, przejrzyj następujące możliwe problemy.

-   Zawiera serwer Azure innego regionu geograficznego niż program SQL Server? Skonfiguruj serwer Azure znajdować się w tym samym regionu geograficznego co program SQL Server w celu zmniejszenia czasu oczekiwania w sieci.

-   Warunki w sieci mogą mieć obniżonej wydajności. Aby uzyskać informacje o najnowszych problemów lub dostawie, skontaktuj się z administratorem sieci.

### <a name="increase-azure-performance-level-for-resource-intensive-operations-such-as-indexing"></a>Zwiększanie wydajności Azure poziomu dla zasobu\-intensywnie operacji, takich jak indeksowania
Podczas tworzenia, odbudowanie lub zmienianie struktury indeksu dla dużej tabeli, która jest skonfigurowany do bazy danych rozciąganie i przewiduje się dużą ilością wyszukiwanie migracją danych platformy Azure w tym czasie, należy rozważyć zwiększenie poziom wydajności odpowiednich zdalna baza danych Azure na czas wykonywania operacji. Aby uzyskać więcej informacji o poziomach wydajności oraz ceny zobacz [Ceny bazy danych rozciąganie programu SQL Server](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/).

### <a name="you-cant-pause-the-sql-server-stretch-database-service-on-azure"></a>Nie można wstrzymać usługę bazy danych programu SQL Server rozciąganie Azure  
 Upewnij się, wybierz odpowiednią wydajność i ceny poziom. Jeśli Zwiększ poziom wydajności tymczasowo dla zasobu\-intensywnie operacji go przywrócić do poprzedniego poziomu po zakończeniu operacji. Aby uzyskać więcej informacji o poziomach wydajności oraz ceny zobacz [Ceny bazy danych rozciąganie programu SQL Server](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/).  

## <a name="change-the-scope-of-queries"></a>Zmienianie zakresu kwerendy  
 Zapytań rozciąganie\-enabled tabele zwracają dane lokalnych i zdalnych domyślnie. Możesz zmienić zakres kwerendy dla wszystkich kwerend przez wszystkich użytkowników lub tylko dla jednej kwerendzie przez administratora.  

### <a name="change-the-scope-of-queries-for-all-queries-by-all-users"></a>Zmienianie zakresu kwerendy dla wszystkich kwerend przez wszystkich użytkowników  
 Aby zmienić zakres wszystkich kwerend przez wszystkich użytkowników, uruchom procedura składowana **sys.sp_rda_set_query_mode**. Możesz zmniejszyć zakres kwerendy tylko dane lokalne, wyłącz wszystkie kwerendy lub przywrócić ustawienia domyślne. Aby uzyskać więcej informacji zobacz [sys.sp_rda_set_query_mode](https://msdn.microsoft.com/library/mt703715.aspx).  

### <a name="queryHints"></a>Zmienić zakres kwerendy dla pojedynczego zapytania przez administratora  
 Aby zmienić zakres kwerendy pojedynczej należący do roli db_owner, dodać * *z \( REMOTE_DATA_ARCHIVE_OVERRIDE = *wartość* \)** wskazówki kwerendy do instrukcji SELECT. Wskazówka dotycząca kwerendy REMOTE_DATA_ARCHIVE_OVERRIDE może mieć następujące wartości.  
 -   **LOCAL_ONLY**. Kwerendy tylko dane lokalne.  

 -   **REMOTE_ONLY**. Kwerendy tylko dane zdalne.  

 -   **STAGE_ONLY**. Kwerendy tylko dane w tabeli, w której etapów rozciąganie bazy danych wierszy kwalifikuje się do migracji i zachowuje migracją wierszy określony po migracji. Wskazówka kwerendy jest jedynym sposobem kwerendy tabeli etapów.  

Na przykład poniższa kwerenda zwraca tylko lokalne wyniki.  

 ```tsql  
 USE <Stretch-enabled database name>;
 GO
 SELECT * FROM <Stretch_enabled table name> WITH (REMOTE_DATA_ARCHIVE_OVERRIDE = LOCAL_ONLY) WHERE ... ;
 GO
```  

## <a name="adminHints"></a>Aktualizacje administracyjne i usuwa  
 Domyślne nie można zaktualizować lub usunąć wiersze, które kwalifikuje się do migracji lub wiersze, które już przeprowadzeniu migracji w rozciąganie\-włączone tabeli. Jeśli musisz rozwiązać problem, jest członkiem roli db_owner może zostać uruchomiony operacji aktualizacji lub usunięcia, dodając * *z \( REMOTE_DATA_ARCHIVE_OVERRIDE = *wartość* \)** wskazówki kwerendy do instrukcji. Wskazówka dotycząca kwerendy REMOTE_DATA_ARCHIVE_OVERRIDE może mieć następujące wartości.  
 -   **LOCAL_ONLY**. Aktualizowanie lub usuwanie tylko dane lokalne.  

 -   **REMOTE_ONLY**. Aktualizowanie lub usuwanie tylko dane zdalne.  

 -   **STAGE_ONLY**. Aktualizowanie lub usuwanie tylko dane w tabeli, w której etapów rozciąganie bazy danych wierszy kwalifikuje się do migracji i zachowuje migracją wierszy określony po migracji.  

## <a name="see-also"></a>Zobacz też

[Monitorowanie rozciąganie bazy danych](sql-server-stretch-database-monitor.md)

[Kopii zapasowej bazy danych, które są włączone rozciąganie](sql-server-stretch-database-backup.md)

[Przywracanie z obsługą rozciąganie baz danych](sql-server-stretch-database-restore.md)
