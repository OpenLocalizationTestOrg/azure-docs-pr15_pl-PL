<properties
    pageTitle="Konfigurowanie reguły zapory na poziomie serwera bazy danych SQL | Microsoft Azure"
    description="Dowiedz się, jak skonfigurować zaporę adresów IP dostępu do serwera Azure SQL."
    services="sql-database"
    documentationCenter=""
    authors="BYHAM"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article" 
    ms.date="08/30/2016"
    ms.author="rickbyh;carlrab"/>


# <a name="configure-an-azure-sql-database-server-level-firewall-rule-using-the-azure-portal"></a>Konfigurowanie reguły zapory na poziomie serwera bazy danych SQL Azure za pomocą portalu Azure


> [AZURE.SELECTOR]
- [Omówienie](sql-database-firewall-configure.md)
- [Azure Portal](sql-database-configure-firewall-settings.md)
- [TSQL](sql-database-configure-firewall-settings-tsql.md)
- [Programu PowerShell](sql-database-configure-firewall-settings-powershell.md)
- [INTERFEJSU API USŁUGI REST](sql-database-configure-firewall-settings-rest.md)

Azure program SQL server używa reguły zapory do zezwalania na połączenia na serwerach i baz danych. Można zdefiniować ustawienia zapory serwera i poziomu bazy danych dla głównej lub bazy danych użytkowników na serwerze logiczne Azure SQL server selektywne umożliwiają dostęp do bazy danych. W tym temacie omówiono reguły zapory na poziomie serwera.

> [AZURE.IMPORTANT] Aby umożliwić aplikacji z platformy Azure do nawiązania połączenia z serwerem Azure SQL, musi być włączony Azure połączenia. Aby dowiedzieć się, jak zapory zasady pracy, zobacz [sposobu konfigurowania zapory serwera Azure SQL \- omówienie](sql-database-firewall-configure.md). Jeśli tworzysz połączenia wewnątrz krawędzi chmura Azure, może być konieczne Otwórz pewne dodatkowe porty TCP. Aby uzyskać więcej informacji, zobacz **w wersji 12 SQL Database: poza w porównaniu z wewnątrz** sekcji [porty inne niż 1433 4,5 ADO.NET i wersji 12 bazy danych SQL](sql-database-develop-direct-route-ports-adonet-v12.md)

**Zalecenie:** Za pomocą reguły zapory na poziomie serwera dla administratorów i kiedy ma wiele baz danych, które mają takie same wymagania programu access, a nie chcesz poświęcać czasu na konfigurowanie pojedynczo każdej bazy danych. Firma Microsoft zaleca używanie reguły zapory poziom bazy danych, jeśli to możliwe, aby zwiększyć bezpieczeństwo i utworzyć bardziej przenośne bazy danych.

[AZURE.INCLUDE [Create SQL Database database](../../includes/sql-database-create-new-server-firewall-portal.md)]

## <a name="manage-existing-server-level-firewall-rules-through-the-azure-portal"></a>Zarządzanie istniejącej reguły zapory poziomie serwera za pośrednictwem portalu Azure

Powtórz te kroki, aby zarządzać regułami zaporę na poziomie serwera.

- Aby dodać bieżącego komputera, kliknij pozycję Dodaj IP klienta.
- Aby dodać dodatkowe adresy IP, wpisz nazwę reguły, adres IP początkowy i końcowy adres IP.
- Aby zmodyfikować istniejącą regułę, kliknij dowolne pola w regule i modyfikować.
- Aby usunąć istniejącą regułę, umieść wskaźnik myszy na reguły do momentu wyświetlenia X na końcu wiersza. Kliknij przycisk X, aby usunąć regułę.

Kliknij przycisk **Zapisz** , aby zapisać zmiany.

## <a name="next-steps"></a>Następne kroki

Jak artykuł na temat Tworzenie reguł zapory serwera i poziomu bazy danych za pomocą języka Transact-SQL zobacz [Konfigurowanie bazy danych SQL Azure reguły zapory serwera i poziomu bazy danych za pomocą T-SQL](sql-database-configure-firewall-settings-tsql.md). 

Dla jak do artykułów na temat tworzenia reguły zapory na poziomie serwera przy użyciu innych metod, zobacz: 

- [Skonfiguruje reguły zapory poziomie serwera bazy danych SQL Azure za pomocą programu PowerShell](sql-database-configure-firewall-settings-powershell.md)
- [Skonfiguruje reguły zapory poziomie serwera bazy danych SQL Azure za pomocą interfejsu API usługi REST](sql-database-configure-firewall-settings-rest.md)

Samouczek dotyczący tworzenia bazy danych zobacz [Tworzenie bazy danych SQL w minutach za pomocą portalu Azure](sql-database-get-started.md).
Aby uzyskać pomoc podczas łączenia się z bazą danych programu Azure SQL z Otwórz źródło lub aplikacji innych firm zobacz [klienta szybki start przykłady kodu do bazy danych SQL](https://msdn.microsoft.com/library/azure/ee336282.aspx).
Aby dowiedzieć się, jak przejść do baz danych, zobacz [Zarządzanie zabezpieczeniami bazy danych programu access i logowania](https://msdn.microsoft.com/library/azure/ee336235.aspx).


## <a name="additional-resources"></a>Dodatkowe zasoby

- [Zabezpieczanie bazy danych](sql-database-security.md)
- [Centrum zabezpieczeń dla aparatu bazy danych programu SQL Server i baza danych SQL Azure](https://msdn.microsoft.com/library/bb510589)


<!--Image references-->
[1]: ./media/sql-database-configure-firewall-settings/AzurePortalBrowseForFirewall.png
[2]: ./media/sql-database-configure-firewall-settings/AzurePortalFirewallSettings.png
<!--anchors-->

 
