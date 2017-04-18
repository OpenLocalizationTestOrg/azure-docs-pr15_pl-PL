<properties
    pageTitle="Azure reguły zapory na poziomie serwera bazy danych SQL za pomocą interfejsu API usługi REST | Microsoft Azure"
    description="Dowiedz się, jak skonfigurować zaporę dla adresów IP, które uzyskiwać dostęp do bazy danych programu SQL Azure."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article" 
    ms.date="08/09/2016"
    ms.author="sstein"/>


#  <a name="configure-azure-sql-database-server-level-firewall-rules-using-the-rest-api"></a>Skonfiguruje reguły zapory poziomie serwera bazy danych SQL Azure za pomocą interfejsu API usługi REST


> [AZURE.SELECTOR]
- [Omówienie](sql-database-firewall-configure.md)
- [Azure portal](sql-database-configure-firewall-settings.md)
- [TSQL](sql-database-configure-firewall-settings-tsql.md)
- [Programu PowerShell](sql-database-configure-firewall-settings-powershell.md)
- [INTERFEJSU API USŁUGI REST](sql-database-configure-firewall-settings-rest.md)


Microsoft Azure SQL Database używa reguły zapory do zezwalania na połączenia na serwerach i baz danych. Można zdefiniować ustawienia zapory serwera i poziomu bazy danych dla głównej lub bazie danych dla użytkownika na serwerze bazy danych SQL Azure selektywne umożliwiają dostęp do bazy danych.

> [AZURE.IMPORTANT] Aby umożliwić aplikacji z platformy Azure do nawiązania połączenia z serwerem bazy danych, musi być włączony Azure połączenia. Aby uzyskać więcej informacji na temat Włączanie połączenia z platformy Azure i reguły zapory zobacz [Zapory bazy danych SQL Azure](sql-database-firewall-configure.md). Jeśli tworzysz połączenia wewnątrz krawędzi chmura Azure, może być konieczne Otwórz pewne dodatkowe porty TCP. Aby uzyskać więcej informacji, zobacz **w wersji 12 SQL Database: poza w porównaniu z wewnątrz** sekcji [porty inne niż 1433 4,5 ADO.NET i wersji 12 bazy danych SQL](sql-database-develop-direct-route-ports-adonet-v12.md)


## <a name="manage-server-level-firewall-rules-through-rest-api"></a>Zarządzanie regułami zaporę na poziomie serwera za pośrednictwem interfejsu API usługi REST
1. Zarządzanie regułami zapory za pośrednictwem interfejsu API usługi REST muszą zostać uwierzytelnione. Aby uzyskać informacje zobacz [Przewodnik po dewelopera do autoryzacji przy użyciu interfejsu API Menedżera zasobów Azure](../resource-manager-api-authentication.md).
2. Można utworzyć reguły poziomie serwera, zaktualizowane lub usunięte, za pomocą interfejsu API usługi REST

    Aby utworzyć lub zaktualizować reguły zapory na poziomie serwera, wykonaj metodzie położenie przy użyciu następujących czynności:
 
        https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/firewallRules/{rule-name}?api-version={api-version}
    
    Treść żądania

        {
         "properties": { 
            "startIpAddress": "{start-ip-address}", 
            "endIpAddress": "{end-ip-address}
            }
        } 
 

    Aby usunąć istniejącą regułę zapory na poziomie serwera, wykonywanie metody DELETE za pomocą następujących czynności:
     
        https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/firewallRules/{rule-name}?api-version={api-version}


## <a name="manage-firewall-rules-using-the-rest-api"></a>Zarządzanie regułami zapory za pomocą interfejsu API usługi REST

* [Tworzenie lub aktualizowanie reguły zapory](https://msdn.microsoft.com/library/azure/mt445501.aspx)
* [Usuwanie reguły zapory](https://msdn.microsoft.com/library/azure/mt445502.aspx)
* [Uzyskiwanie reguły zapory](https://msdn.microsoft.com/library/azure/mt445503.aspx)
* [Wyświetl wszystkie reguły zapory](https://msdn.microsoft.com/library/azure/mt604478.aspx)
 
## <a name="next-steps"></a>Następne kroki

Jak artykuł na temat Tworzenie reguł zapory serwera i poziomu bazy danych za pomocą języka Transact-SQL zobacz [Konfigurowanie bazy danych SQL Azure reguły zapory serwera i poziomu bazy danych za pomocą T-SQL](sql-database-configure-firewall-settings-tsql.md). 

Dla jak do artykułów na temat tworzenia reguły zapory na poziomie serwera przy użyciu innych metod, zobacz: 

- [Skonfiguruje reguły zapory poziomie serwera bazy danych SQL Azure za pomocą portalu Azure](sql-database-configure-firewall-settings.md)
- [Skonfiguruje reguły zapory poziomie serwera bazy danych SQL Azure za pomocą programu PowerShell](sql-database-configure-firewall-settings-powershell.md)

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

 
