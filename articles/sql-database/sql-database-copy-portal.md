<properties
    pageTitle="Kopiowanie bazy danych programu Azure SQL za pomocą portalu Azure | Microsoft Azure"
    description="Utwórz kopię bazy danych programu Azure SQL"
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



# <a name="copy-an-azure-sql-database-using-the-azure-portal"></a>Kopiowanie bazy danych SQL Azure za pomocą portalu Azure

> [AZURE.SELECTOR]
- [Omówienie](sql-database-copy.md)
- [Azure portal](sql-database-copy-portal.md)
- [Programu PowerShell](sql-database-copy-powershell.md)
- [T-SQL](sql-database-copy-transact-sql.md)

Poniższe kroki pokazują jak skopiować z bazą danych SQL [Azure portalu](https://portal.azure.com) na tym samym serwerze lub inny serwer.

Aby skopiować z bazą danych SQL, potrzebne następujące elementy:

- Subskrypcję usługi Azure. W razie potrzeby subskrypcji usługi Azure po prostu kliknij **Bezpłatną wersję PRÓBNĄ** u góry strony, a następnie wróć do końca w tym artykule.
- Bazy danych SQL do skopiowania. Jeśli nie masz bazy danych SQL, należy utworzyć jeden wykonując czynności opisane w tym artykule: [Tworzenie pierwszej bazy danych SQL Azure](sql-database-get-started.md).


## <a name="copy-your-sql-database"></a>Kopiowanie bazy danych SQL

Otwórz stronę bazy danych SQL dla bazy danych, które chcesz skopiować.

1.  Przejdź do [portalu Azure](https://portal.azure.com).
2.  Kliknij pozycję **więcej usług** > **bazy danych programu SQL**, a następnie kliknij odpowiednią bazę danych.
3.  Na stronie bazy danych SQL kliknij przycisk **Kopiuj**:

    ![Baza danych SQL](./media/sql-database-copy-portal/sql-database-copy.png)

1.  Na stronie **Kopiowanie** znajduje się domyślna nazwa bazy danych. Jeśli chcesz, wpisz inną nazwę (wszystkie bazy danych na serwerze muszą być unikatowe nazwy).
2.  Wybierz **serwer docelowy**. Serwer docelowy to miejsce, w którym zostanie utworzona kopia bazy danych. Możesz skopiować bazę danych na tym samym serwerze lub inny serwer. Możesz utworzyć serwer, lub zaznacz istniejący serwer z listy. 
3.  Po wybraniu **serwera docelowego**, **puli elastyczne bazy danych**i **poziomu ceny** będą dostępne opcje. Jeśli serwer ma puli, możesz skopiować bazę danych do niego.
3.  Kliknij **przycisk OK** , aby rozpocząć proces kopiowania.

    ![Baza danych SQL](./media/sql-database-copy-portal/copy-page.png)


## <a name="monitor-the-progress-of-the-copy-operation"></a>Monitorowanie postępu kopiowania

- Po uruchomieniu kopii kliknij portalu powiadomienie, aby uzyskać szczegółowe informacje.

    ![powiadomienie][3]
 
    ![monitorowanie][4]


## <a name="verify-the-database-is-live-on-the-server"></a>Upewnij się, że baza danych jest aktywna na serwerze

- Kliknij pozycję **więcej usług** > **bazy danych programu SQL** i sprawdź, czy nowej bazy danych jest w **trybie Online**.


## <a name="resolve-logins"></a>Rozwiązywanie problemów logowania

Rozwiązywać logowania po zakończeniu operacji Kopiuj, zobacz [Rozwiązywanie problemów logowania](sql-database-copy-transact-sql.md#resolve-logins-after-the-copy-operation-completes)


## <a name="next-steps"></a>Następne kroki

- Aby uzyskać omówienie kopiowania bazy danych SQL Azure, zobacz [Kopiowanie bazy danych programu Azure SQL](sql-database-copy.md) .
- Zobacz [kopii bazy danych programu Azure SQL przy użyciu programu PowerShell](sql-database-copy-powershell.md) , aby skopiować bazy danych przy użyciu programu PowerShell.
- Zobacz [kopii bazy danych programu Azure SQL za pomocą T-SQL](sql-database-copy-transact-sql.md) do skopiowania bazy danych przy użyciu języka Transact-SQL.
- Dowiedz się, [jak zarządzać zabezpieczeń bazy danych Azure SQL po awarii](sql-database-geo-replication-security-config.md) Aby uzyskać informacje o zarządzaniu użytkownikami i logowania podczas kopiowania bazy danych na innym serwerze logiczne.



## <a name="additional-resources"></a>Dodatkowe zasoby

- [Zarządzanie logowania](sql-database-manage-logins.md)
- [Nawiązywanie połączenia z bazą danych SQL z programu SQL Server Management Studio i wykonać przykładowa kwerenda T-SQL](sql-database-connect-query-ssms.md)
- [Eksportowanie PLECAK w bazie danych](sql-database-export.md)
- [Przegląd ciągłości działalności](sql-database-business-continuity.md)
- [Dokumentacja bazy danych SQL](https://azure.microsoft.com/documentation/services/sql-database/)




<!--Image references-->
[1]: ./media/sql-database-copy-portal/copy.png
[2]: ./media/sql-database-copy-portal/copy-ok.png
[3]: ./media/sql-database-copy-portal/copy-notification.png
[4]: ./media/sql-database-copy-portal/monitor-copy.png

