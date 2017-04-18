<properties
    pageTitle="Kopiowanie bazy danych programu Azure SQL | Microsoft Azure"
    description="Utwórz kopię bazy danych programu Azure SQL"
    services="sql-database"
    documentationCenter=""
    authors="anosov1960"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="10/24/2016"
    ms.author="sstein; sashan"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>



# <a name="copy-an-azure-sql-database"></a>Kopiowanie bazy danych programu Azure SQL

> [AZURE.SELECTOR]
- [Omówienie](sql-database-copy.md)
- [Azure portal](sql-database-copy-portal.md)
- [Programu PowerShell](sql-database-copy-powershell.md)
- [T-SQL](sql-database-copy-transact-sql.md)

Azure [SQL Database automatycznego wykonywania kopii zapasowych](sql-database-automated-backups.md) umożliwia tworzenie kopii bazy danych SQL. Kopiowanie bazy danych używa tej samej technologii jako funkcja geo replikacji. Jednak w przeciwieństwie do replikacji geo zostaje zakończone łącza replikacji jako po zakończeniu fazy obsługi. Dlatego kopii bazy danych jest migawkę źródłowej bazy danych od czasu żądania kopii.  
Możesz utworzyć kopię bazy danych na tym samym serwerze lub inny serwer. Wydajność i warstwa poziomie usługi (warstwa cennik) kopii bazy danych różnią się od bazy danych źródłowych domyślnie. Korzystając z interfejsu API, możesz wybrać poziom wydajności różnych w tej samej warstwie usługi (Edycja). Po zakończeniu kopiowania utworzona kopia staje się w pełni funkcjonalny, niezależne bazy danych. Na tym etapie można uaktualnić lub starszą wersję go do dowolnej wersji. Niezależne można zarządzać logowania, użytkownicy i uprawnienia.  

Po skopiowaniu bazy danych na tym samym serwerze logicznych samym logowania można używać w obu bazach danych. Kopiowania bazy danych za pomocą podmiot zabezpieczeń staje się właścicielem bazy danych (DBO) na nowej bazy danych. Wszyscy użytkownicy bazy danych, jego uprawnień i ich identyfikatory zabezpieczeń (identyfikatory zabezpieczeń) są kopiowane do kopii bazy danych.  

Po skopiowaniu bazy danych na innym serwerze logiczne podmiotu na serwerze nowych zabezpieczeń staje się właściciela bazy danych dla nowej bazy danych. Jeśli korzystasz z [zawarte użytkowników bazy danych](sql-database-manage-logins.md) dla dostęp do danych gwarantuje, że zarówno głównego i pomocniczego baz danych musi zawsze działać takich samych poświadczeń użytkownika, aby po zakończeniu kopiowania możesz od razu do niego dostęp przy użyciu tych samych poświadczeń. Jeśli korzystasz z [Usługi Azure Active Directory](../active-directory/active-directory-whatis.md), można całkowicie wyeliminować konieczności zarządzania poświadczeń w polu Kopiuj. Jednak podczas kopiowania bazy danych do nowego serwera, podstawie zalogowania ogólnie nie będzie działać ponieważ logowania nie będzie istnieć na nowym serwerze. Zobacz, [jak zarządzać zabezpieczeń bazy danych Azure SQL po awarii](sql-database-geo-replication-security-config.md) informacje na temat zarządzania logowania podczas kopiowania bazy danych na innym serwerze logiczne. 

Aby skopiować z bazą danych SQL, są potrzebne następujące elementy:

- Subskrypcję usługi Azure. W razie potrzeby subskrypcji usługi Azure po prostu kliknij **Bezpłatną wersję PRÓBNĄ** u góry strony, a następnie wróć do końca w tym artykule.
- Bazy danych SQL do skopiowania. Jeśli nie masz bazy danych SQL, należy utworzyć jeden wykonując czynności opisane w tym artykule: [Tworzenie pierwszej bazy danych SQL Azure](sql-database-get-started.md).

## <a name="next-steps"></a>Następne kroki

- Zobacz [Kopiowanie bazy danych programu Azure SQL za pomocą portalu Azure](sql-database-copy-portal.md) , aby skopiować bazy danych za pomocą portalu Azure.
- Zobacz [kopii bazy danych programu Azure SQL przy użyciu programu PowerShell](sql-database-copy-powershell.md) , aby skopiować bazy danych przy użyciu programu PowerShell.
- Zobacz [kopii bazy danych programu Azure SQL za pomocą T-SQL](sql-database-copy-transact-sql.md) do skopiowania bazy danych przy użyciu języka Transact-SQL.
- Dowiedz się, [jak zarządzać zabezpieczeń bazy danych Azure SQL po awarii](sql-database-geo-replication-security-config.md) Aby uzyskać informacje o zarządzaniu użytkownikami i logowania podczas kopiowania bazy danych na innym serwerze logiczne.



## <a name="additional-resources"></a>Dodatkowe zasoby

- [Zarządzanie logowania](sql-database-manage-logins.md)
- [Nawiązywanie połączenia z bazą danych SQL z programu SQL Server Management Studio i wykonać przykładowa kwerenda T-SQL](sql-database-connect-query-ssms.md)
- [Eksportowanie PLECAK w bazie danych](sql-database-export.md)
- [Przegląd ciągłości działalności](sql-database-business-continuity.md)
- [Dokumentacja bazy danych SQL](https://azure.microsoft.com/documentation/services/sql-database/)
