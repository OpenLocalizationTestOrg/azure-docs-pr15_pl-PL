<properties
    pageTitle="Konfigurowanie reguły zapory poziomie serwera bazy danych SQL Azure za pomocą programu PowerShell | Microsoft Azure"
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


# <a name="configure-azure-sql-database-server-level-firewall-rules-by-using-powershell"></a>Konfigurowanie reguły zapory poziomie serwera bazy danych SQL Azure za pomocą programu PowerShell


> [AZURE.SELECTOR]
- [Omówienie](sql-database-firewall-configure.md)
- [Azure portal](sql-database-configure-firewall-settings.md)
- [TSQL](sql-database-configure-firewall-settings-tsql.md)
- [Programu PowerShell](sql-database-configure-firewall-settings-powershell.md)
- [INTERFEJSU API USŁUGI REST](sql-database-configure-firewall-settings-rest.md)


Baza danych SQL Azure używa reguły zapory do zezwalania na połączenia na serwerach i baz danych. Można określić ustawienia zapory serwer i baza danych poziomu wzorca bazy danych lub bazę danych użytkownika na serwerze bazy danych SQL selektywne umożliwiają dostęp do bazy danych.

> [AZURE.IMPORTANT] Aby umożliwić aplikacji z platformy Azure do nawiązania połączenia z serwerem bazy danych, musi być włączony Azure połączenia. Aby uzyskać więcej informacji na temat Włączanie połączenia z platformy Azure i reguły zapory zobacz [Zapory bazy danych SQL Azure](sql-database-firewall-configure.md). Jeśli tworzysz połączenia wewnątrz krawędzi chmura Azure, może być konieczne Otwórz pewne dodatkowe porty TCP. Aby uzyskać więcej informacji, zobacz "bazy danych SQL w wersji 12: poza w porównaniu z wewnątrz" sekcja [porty inne niż 1433 4,5 ADO.NET i wersji 12 bazy danych SQL](sql-database-develop-direct-route-ports-adonet-v12.md).


[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]

## <a name="create-server-firewall-rules"></a>Tworzenie reguł zapory, serwera

Zaporę na poziomie serwera, który można utworzyć reguły, aktualizacji i usunąć przy użyciu programu PowerShell Azure.

Aby utworzyć nową regułę zapory poziomie serwera, wykonywanie [New-AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603860(v=azure.300\).aspx) polecenia cmdlet. Poniższy przykład włącza zakres adresów IP na serwerze Contoso.

    New-AzureRmSqlServerFirewallRule -ResourceGroupName 'resourcegroup1' -ServerName 'Contoso' -FirewallRuleName "ContosoFirewallRule" -StartIpAddress '192.168.1.1' -EndIpAddress '192.168.1.10'       

Aby zmodyfikować istniejącą regułę zapory na poziomie serwera, wykonywanie [Set-AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603789(v=azure.300\).aspx) polecenia cmdlet. Poniższy przykład zmienia zakres dopuszczalnego adresów IP dla reguły o nazwie ContosoFirewallRule.

    Set-AzureRmSqlServerFirewallRule -ResourceGroupName 'resourcegroup1' –StartIPAddress 192.168.1.4 –EndIPAddress 192.168.1.10 –RuleName 'ContosoFirewallRule' –ServerName 'Contoso'

Aby usunąć istniejącą regułę zapory na poziomie serwera, wykonywanie [Usuń-AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603588(v=azure.300\).aspx) polecenia cmdlet. W następującym przykładzie regułę o nazwie ContosoFirewallRule.

    Remove-AzureRmSqlServerFirewallRule –RuleName 'ContosoFirewallRule' –ServerName 'Contoso'


## <a name="manage-firewall-rules-by-using-powershell"></a>Zarządzanie regułami zapory przy użyciu programu PowerShell

Za pomocą programu PowerShell do zarządzania regułami zapory. Aby uzyskać więcej informacji zobacz następujące tematy:

* [Nowy AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603860(v=azure.300\).aspx)
* [Usuń AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603588(v=azure.300\).aspx)
* [Zestaw AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603789(v=azure.300\).aspx)
* [Get-AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603586(v=azure.300\).aspx)


## <a name="next-steps"></a>Następne kroki

Aby uzyskać informacje o używaniu języku Transact-SQL, aby utworzyć reguły zapory serwera i poziomu bazy danych, zobacz [reguły zapory serwer i baza danych poziomu Konfigurowanie bazy danych SQL Azure za pomocą T-SQL](sql-database-configure-firewall-settings-tsql.md).

Aby dowiedzieć się, jak tworzyć reguły zapory na poziomie serwera przy użyciu innych metod zobacz:

- [Skonfiguruje reguły zapory poziomie serwera bazy danych SQL Azure za pomocą portalu Azure](sql-database-configure-firewall-settings.md)
- [Skonfiguruje reguły zapory poziomie serwera bazy danych SQL Azure za pomocą interfejsu API usługi REST](sql-database-configure-firewall-settings-rest.md)

Samouczek dotyczący tworzenia bazy danych zobacz [Tworzenie bazy danych SQL w minutach za pomocą portalu Azure](sql-database-get-started.md).
Nawiązywanie połączenia z bazą danych programu Azure SQL z Otwórz źródło lub aplikacji innych firm, zobacz [klienta szybki start przykłady kodu do bazy danych SQL](https://msdn.microsoft.com/library/azure/ee336282.aspx).
Aby dowiedzieć się, jak przejść do baz danych, zobacz [Zarządzanie zabezpieczeniami bazy danych programu access i logowania](https://msdn.microsoft.com/library/azure/ee336235.aspx).


## <a name="additional-resources"></a>Dodatkowe zasoby

- [Zabezpieczanie bazy danych](sql-database-security.md)
- [Centrum zabezpieczeń dla aparatu bazy danych programu SQL Server i baza danych SQL Azure](https://msdn.microsoft.com/library/bb510589)


<!--Image references-->
[1]: ./media/sql-database-configure-firewall-settings/AzurePortalBrowseForFirewall.png
[2]: ./media/sql-database-configure-firewall-settings/AzurePortalFirewallSettings.png
<!--anchors-->
