<properties
    pageTitle="Wstrzymywanie i wznawianie migracji danych (baza danych rozciąganie) | Microsoft Azure"
    description="Dowiedz się, jak wstrzymać lub wznowić migracji danych Azure."
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
    ms.date="06/14/2016"
    ms.author="douglasl"/>

# <a name="pause-and-resume-data-migration-stretch-database"></a>Wstrzymywanie i wznawianie migracji danych (baza danych rozciąganie)

Aby wstrzymać lub wznowić migracji danych Azure, wybierz pozycję **Rozciągnij** dla tabeli w programie SQL Server Management Studio, a następnie wybierz **Zatrzymaj wskaźnik myszy** , aby wstrzymać migracji danych lub **Życiorys** , aby wznowić migracji danych. Można również użyć Transact\-kod SQL, aby wstrzymać lub wznowić migracji danych.

Wstrzymaj migracji danych w poszczególnych tabelach, gdy chcesz Rozwiązywanie problemów na serwerze lokalnym lub maksymalizowanie dostępna przepustowość sieci.

## <a name="pause-data-migration"></a>Wstrzymaj migracji danych

### <a name="use-sql-server-management-studio-to-pause-data-migration"></a>Zatrzymaj wskaźnik myszy migracji danych za pomocą programu SQL Server Management Studio

1.  W programie SQL Server Management Studio w Eksploratorze obiektów wybierz rozciąganie\-włączone tabeli, dla której chcesz wstrzymać migracji danych.

2.  Prawo\-kliknij i wybierz **Rozciąganie**, a następnie wybierz pozycję **Wstrzymaj**.

### <a name="use-transact-sql-to-pause-data-migration"></a>Używanie Transact\-kod SQL, aby wstrzymać migracji danych
Uruchom następujące polecenie.

```tsql
USE <Stretch-enabled database name>;
GO
ALTER TABLE <Stretch-enabled table name>  
    SET ( REMOTE_DATA_ARCHIVE ( MIGRATION_STATE = PAUSED ) ) ;  
GO
```

## <a name="resume-data-migration"></a>Wznawianie migracji danych

### <a name="use-sql-server-management-studio-to-resume-data-migration"></a>Wznawianie migracji danych za pomocą programu SQL Server Management Studio

1.  W programie SQL Server Management Studio w Eksploratorze obiektów wybierz rozciąganie\-włączone tabeli, dla której chcesz wznowić migracji danych.

2.  Prawo\-kliknij i wybierz **Rozciąganie**, a następnie wybierz pozycję **Wznów**.

### <a name="use-transact-sql-to-resume-data-migration"></a>Używanie Transact\-kod SQL, aby wznowić migracji danych
Uruchom następujące polecenie.

```tsql
USE <Stretch-enabled database name>;
GO
ALTER TABLE <Stretch-enabled table name>   
    SET ( REMOTE_DATA_ARCHIVE ( MIGRATION_STATE = OUTBOUND ) ) ;  
 GO
```

## <a name="check-whether-migration-is-active-or-paused"></a>Sprawdzanie, czy migracja jest aktywny ani wstrzymanych

### <a name="use-sql-server-management-studio-to-check-whether-migration-is-active-or-paused"></a>Sprawdzanie, czy migracja jest aktywny ani wstrzymania za pomocą programu SQL Server Management Studio
W programie SQL Server Management Studio Otwórz **Rozciąganie Monitor bazy danych** i sprawdź wartość w kolumnie **Stan migracji** . Aby uzyskać więcej informacji, zobacz [Monitor i rozwiązywanie problemów z migracji danych](sql-server-stretch-database-monitor.md).

### <a name="use-transact-sql-to-check-whether-migration-is-active-or-paused"></a>Sprawdzanie, czy migracja jest aktywny ani wstrzymania za pomocą języka Transact-SQL
Kwerendy widoku wykazu **sys.remote_data_archive_tables** , a następnie sprawdź wartość w kolumnie **is_migration_paused** . Aby uzyskać więcej informacji zobacz [sys.remote_data_archive_tables](https://msdn.microsoft.com/library/dn935003.aspx).

## <a name="see-also"></a>Zobacz też

[Instrukcja ALTER TABLE (Transact-SQL)](https://msdn.microsoft.com/library/ms190273.aspx)
[Monitor i rozwiązywanie problemów z migracji danych](sql-server-stretch-database-monitor.md)
