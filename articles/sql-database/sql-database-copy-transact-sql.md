<properties 
    pageTitle="Kopiowanie bazy danych programu Azure SQL za pomocą języka Transact-SQL | Microsoft Azure" 
    description="Utwórz kopię bazy danych programu Azure SQL za pomocą języka Transact-SQL" 
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="09/19/2016"
    ms.author="sstein"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="copy-an-azure-sql-database-using-transact-sql"></a>Kopiowanie bazy danych programu Azure SQL za pomocą języka Transact-SQL


> [AZURE.SELECTOR]
- [Omówienie](sql-database-copy.md)
- [Azure portal](sql-database-copy-portal.md)
- [Programu PowerShell](sql-database-copy-powershell.md)
- [T-SQL](sql-database-copy-transact-sql.md)


Ten poniższe kroki pokazują jak skopiować do tego samego serwera lub inny serwer bazy danych SQL za pomocą języka Transact-SQL. Operacja kopii bazy danych użyto instrukcji [CREATE DATABASE](https://msdn.microsoft.com/library/ms176061.aspx) .

Do wykonania kroków w tym artykule są potrzebne następujące elementy:

- Subskrypcję usługi Azure. W razie potrzeby subskrypcji usługi Azure po prostu kliknij **Bezpłatną wersję PRÓBNĄ** u góry strony, a następnie wróć do końca w tym artykule.
- Baza danych SQL Azure. Jeśli nie masz bazy danych SQL, należy utworzyć jeden wykonując czynności opisane w tym artykule: [Tworzenie pierwszej bazy danych SQL Azure](sql-database-get-started.md).
- [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/ms174173.aspx). Jeśli nie masz SSMS lub funkcje opisane w tym artykule są niedostępne, [pobrać najnowszą wersję](https://msdn.microsoft.com/library/mt238290.aspx).


## <a name="copy-your-sql-database"></a>Kopiowanie bazy danych SQL

Zaloguj się do wzorca bazy danych za pomocą logowania głównym poziomie serwera lub logowania, która utworzyła bazę danych, które chcesz skopiować. Aby skopiować baz danych logowania, które nie są kapitału poziomie serwera musi być członkowie roli dbmanager. Aby uzyskać więcej informacji na temat logowania i łączenie się z serwerem zobacz [Zarządzanie logowania](sql-database-manage-logins.md).

Rozpocznij kopiowanie źródłowej bazy danych przy użyciu instrukcji [Tworzenie bazy danych](https://msdn.microsoft.com/library/ms176061.aspx) . Wykonanie tej instrukcji inicjuje proces kopiowania bazy danych. Kopiowanie bazy danych jest proces asynchroniczny, przed zakończeniem bazy danych, kopiując zwraca wartość instrukcja CREATE DATABASE.


### <a name="copy-a-sql-database-to-the-same-server"></a>Skopiuj do tego samego serwera bazy danych SQL

Zaloguj się do wzorca bazy danych za pomocą logowania głównym poziomie serwera lub logowania, która utworzyła bazę danych, które chcesz skopiować. Aby skopiować baz danych logowania, które nie są kapitału poziomie serwera musi być członkowie roli dbmanager.

To polecenie kopii Database1 do nowej bazy danych o nazwie Database2 na tym samym serwerze. W zależności od rozmiaru bazy danych operacja kopiowania może potrwać do wykonania.

    -- Execute on the master database.
    -- Start copying.
    CREATE DATABASE Database1_copy AS COPY OF Database1;

### <a name="copy-a-sql-database-to-a-different-server"></a>Kopiowanie na inny serwer bazy danych SQL

Zaloguj się do wzorca bazy danych serwera docelowego serwera bazy danych SQL Azure miejsce, w którym ma być utworzony nowej bazy danych. Za pomocą identyfikatora, do którego ma tę samą nazwę i hasło, jak właściciel bazy danych (DBO) źródłowej bazy danych na serwerze bazy danych SQL Azure źródłowym. Identyfikator logowania na serwerze docelowym również musi należeć do roli dbmanager lub być logowania głównym poziomie serwera.

To polecenie kopiuje Database1 na serwer1 — do nowej bazy danych o nazwie Database2 na serwerze server2. W zależności od rozmiaru bazy danych operacja kopiowania może potrwać do wykonania.


    -- Execute on the master database of the target server (server2)
    -- Start copying from Server1 to Server2
    CREATE DATABASE Database1_copy AS COPY OF server1.Database1;
    

## <a name="monitor-the-progress-of-the-copy-operation"></a>Monitorowanie postępu kopiowania

Monitorowanie procesu kopiowania przez badanie widoki sys.databases i sys.dm_database_copies. Podczas kopiowania jest w toku, kolumnie state_desc widoku sys.databases dla nowej bazy danych jest ustawiona na kopiowanie.


- Jeśli kopiowania nie powiedzie się, kolumnie state_desc widoku sys.databases dla nowej bazy danych jest równa PODEJRZANE. W tym przypadku wykonać instrukcja DROP dla nowej bazy danych i spróbuj ponownie później.
- Jeżeli kopiowania zostanie pomyślnie, kolumna state_desc widoku sys.databases dla nowej bazy danych jest ustawiona w trybie ONLINE. W tym przypadku kopiowania zostało zakończone i Nowa baza danych jest zwykłej bazie danych, mogą być zmieniane niezależnie od źródłowej bazy danych.

> [AZURE.NOTE] -Jeśli zdecydujesz się anulować kopiowanie, gdy jest w toku, należy wykonać instrukcji [DROP DATABASE](https://msdn.microsoft.com/library/ms178613.aspx) dla nowej bazy danych. Możesz też wykonywania instrukcji Usuń bazę danych w bazie danych źródłowych powoduje także anulowanie procesu kopiowania.


## <a name="resolve-logins-after-the-copy-operation-completes"></a>Rozwiązywanie problemów logowania po zakończeniu operacji Kopiuj

Po nowej bazy danych jest w trybie online na serwerze docelowym, za pomocą instrukcja [ALTER USER](https://msdn.microsoft.com/library/ms176060.aspx) ponownie zamapować użytkowników z nowej bazy danych do logowania na serwerze docelowym. Aby rozwiązać pozbawiony użytkowników, zobacz [Rozwiązywanie problemów z pozbawiony użytkowników](https://msdn.microsoft.com/library/ms175475.aspx). Zobacz też [jak zarządzać zabezpieczeń bazy danych Azure SQL po awarii](sql-database-geo-replication-security-config.md).

Wszystkich użytkowników do nowej bazy danych zachować uprawnienia, które miały w bazie danych źródłowych. Użytkownik, który zainicjowane kopii bazy danych staje się właścicielem bazy danych nowej bazy danych i przypisano nowych zabezpieczeń identyfikator. Po pomyślnym kopiowania i przed innymi użytkownikami są mapowane ponownie tylko logowania zainicjowane, kopiowanie, właściciel bazy danych (DBO), można zalogować się do nowej bazy danych.


## <a name="next-steps"></a>Następne kroki

- Aby uzyskać omówienie kopiowania bazy danych SQL Azure, zobacz [Kopiowanie bazy danych programu Azure SQL](sql-database-copy.md) .
- Zobacz [Kopiowanie bazy danych programu Azure SQL za pomocą portalu Azure](sql-database-copy-portal.md) , aby skopiować bazy danych za pomocą portalu Azure.
- Zobacz [kopii bazy danych programu Azure SQL przy użyciu programu PowerShell](sql-database-copy-powershell.md) , aby skopiować bazy danych przy użyciu programu PowerShell.
- Dowiedz się, [jak zarządzać zabezpieczeń bazy danych Azure SQL po awarii](sql-database-geo-replication-security-config.md) Aby uzyskać informacje o zarządzaniu użytkownikami i logowania podczas kopiowania bazy danych na innym serwerze logiczne.



## <a name="additional-resources"></a>Dodatkowe zasoby

- [Zarządzanie logowania](sql-database-manage-logins.md)
- [Nawiązywanie połączenia z bazą danych SQL z programu SQL Server Management Studio i wykonać przykładowa kwerenda T-SQL](sql-database-connect-query-ssms.md)
- [Eksportowanie PLECAK w bazie danych](sql-database-export.md)
- [Przegląd ciągłości działalności](sql-database-business-continuity.md)
- [Dokumentacja bazy danych SQL](https://azure.microsoft.com/documentation/services/sql-database/)


