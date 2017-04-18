<properties
    pageTitle="Baza danych SQL samouczek: wprowadzenie do zabezpieczeń"
    description="Dowiedz się, jak utworzyć konta użytkowników dostęp do i zarządzanie bazą danych."
    keywords=""
    services="sql-database"
    documentationCenter=""
    authors="CarlRabeler"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="08/17/2016"
    ms.author="carlrab"/>

# <a name="sql-database-tutorial-create-sql-database-user-accounts-to-access-and-manage-a-database"></a>Baza danych SQL samouczek: Tworzenie konta użytkowników w celu dostępu i zarządzanie bazą danych bazy danych SQL


> [AZURE.SELECTOR]
- [Samouczek Wprowadzenie Get](sql-database-get-started-security.md)
- [Udzielanie dostępu](sql-database-manage-logins.md)

W tym samouczku dowiesz się, jak używać programu SQL Server Management Studio (SSMS) do:

- Zaloguj się przy użyciu identyfikatora logowania głównym poziomie serwera bazy danych SQL.
- Utwórz konto użytkownika bazy danych SQL.
- Udziel użytkownikowi [uprawnienia db_owner](https://msdn.microsoft.com/library/ms189121.aspx#Anchor_0)bazy danych SQL.
- Nawiązywanie połączenia z bazą danych SQL przy użyciu konta użytkownika, który nie jest kapitału poziomie serwera.

[AZURE.INCLUDE [Login](../../includes/azure-getting-started-portal-login.md)]


[AZURE.INCLUDE [Create SQL Database logical server](../../includes/sql-database-sql-server-management-studio-connect-server-principal.md)]


[AZURE.INCLUDE [Create SQL Database database](../../includes/sql-database-create-new-database-user.md)]


[AZURE.INCLUDE [Create SQL Database database](../../includes/sql-database-grant-database-user-dbo-permissions.md)]


[AZURE.INCLUDE [Create SQL Database database](../../includes/sql-database-sql-server-management-studio-connect-user.md)]


## <a name="next-steps"></a>Następne kroki
Po zakończeniu tego samouczka bazy danych SQL i utworzyć konto użytkownika i uprawnienia użytkownika konta dbo, możesz przystąpić dowiedzieć się więcej na temat [zabezpieczeń bazy danych SQL](sql-database-manage-logins.md).


