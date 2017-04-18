<properties
    pageTitle="Azure reguły zapory serwera i poziomu bazy danych SQL Database, za pomocą T-SQL | Microsoft Azure"
    description="Dowiedz się, jak skonfigurować zaporę dla adresów IP, które uzyskiwać dostęp do bazy danych programu SQL Azure."
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
    ms.author="rickbyh"/>


# <a name="configure-azure-sql-database-server-level-and-database-level-firewall-rules-using-t-sql"></a>Skonfiguruje reguły zapory serwera i poziomu bazy danych bazy danych SQL Azure za pomocą T-SQL


> [AZURE.SELECTOR]
- [Omówienie](sql-database-firewall-configure.md)
- [Azure Portal](sql-database-configure-firewall-settings.md)
- [TSQL](sql-database-configure-firewall-settings-tsql.md)
- [Programu PowerShell](sql-database-configure-firewall-settings-powershell.md)
- [INTERFEJSU API USŁUGI REST](sql-database-configure-firewall-settings-rest.md)


Microsoft Azure SQL Database używa reguły zapory do zezwalania na połączenia na serwerach i baz danych. Można zdefiniować ustawienia zapory serwera i poziomu bazy danych dla głównej lub bazie danych dla użytkownika na serwerze bazy danych SQL Azure selektywne umożliwiają dostęp do bazy danych.

> [AZURE.IMPORTANT] Aby umożliwić aplikacji z platformy Azure do nawiązania połączenia z serwerem bazy danych, musi być włączony Azure połączenia. Aby uzyskać więcej informacji na temat Włączanie połączenia z platformy Azure i reguły zapory zobacz [Zapory bazy danych SQL Azure](sql-database-firewall-configure.md). Jeśli tworzysz połączenia wewnątrz krawędzi chmura Azure, może być konieczne Otwórz pewne dodatkowe porty TCP. Aby uzyskać więcej informacji, zobacz **w wersji 12 SQL Database: poza w porównaniu z wewnątrz** sekcji [porty inne niż 1433 4,5 ADO.NET i wersji 12 bazy danych SQL](sql-database-develop-direct-route-ports-adonet-v12.md)


## <a name="server-level-firewall-rules"></a>Reguły zapory na poziomie serwera

Tylko logowania głównym poziomie serwera lub administrator usługi Azure Active Directory można utworzyć reguły zapory na poziomie serwera za pomocą języka Transact-SQL.

1. Okno kwerendy i łączenie z wirtualną wzorcem bazy danych przy użyciu programu SQL Server Management Studio.
2. Reguły zapory na poziomie serwera można zaznaczone, tworzyć, zaktualizowane lub usunięte z poziomu okna kwerendy.
3. Aby utworzyć lub zaktualizować reguły zapory na poziomie serwera, wykonywanie `sp_set_firewall_rule` procedura składowana. Poniższy przykład włącza zakres adresów IP na serwerze Contoso.<br/>Zacznij od widzą, jakie zasady już istnieje.

        SELECT * FROM sys.firewall_rules ORDER BY name;

    Następnie dodaj reguły zapory.

        EXECUTE sp_set_firewall_rule @name = N'ContosoFirewallRule',
            @start_ip_address = '192.168.1.1', @end_ip_address = '192.168.1.10'

    Aby usunąć reguły zapory na poziomie serwera, wykonaj procedurę sp_delete_firewall_rule przechowywane. W następującym przykładzie regułę o nazwie ContosoFirewallRule.
 
        EXECUTE sp_delete_firewall_rule @name = N'ContosoFirewallRule'
 
 Aby uzyskać więcej informacji o tych procedur składowanych zobacz [sp_set_firewall_rule](https://msdn.microsoft.com/library/dn270017.aspx) i [sp_delete_firewall_rule](https://msdn.microsoft.com/library/dn270024.aspx).

## <a name="database-level-firewall-rules"></a>Reguły zapory poziom bazy danych

Tylko użytkownik bazy danych z uprawnieniami do **KONTROLKI** w bazie danych (na przykład tylko właściciel bazy danych) można utworzyć reguły zapory poziom bazy danych.

1. Po utworzeniu zaporę poziomie serwera dla swojego adresu IP, Uruchom okno kwerendy za pośrednictwem portalu klasyczny lub SQL Server Management Studio.
2. Nawiązywanie połączenia z bazą danych, dla której chcesz utworzyć regułę zapory poziom bazy danych.

    Aby utworzyć nową lub zaktualizować istniejącej reguły zapory poziom bazy danych, należy wykonać `sp_set_database_firewall_rule` procedura składowana. Poniższy przykład tworzy nową regułę zapory o nazwie ContosoFirewallRule.
 
        EXEC sp_set_database_firewall_rule @name = N'ContosoFirewallRule', 
            @start_ip_address = '192.168.1.11', @end_ip_address = '192.168.1.11'
 
    Aby usunąć istniejącą regułę zapory poziom bazy danych, należy wykonać `sp_delete_database_firewall_rule` procedura składowana. W następującym przykładzie regułę o nazwie ContosoFirewallRule.
`
   WYKONYWALNA sp_delete_database_firewall_rule @name = N'ContosoFirewallRule "

Aby uzyskać więcej informacji o tych procedur składowanych zobacz [sp_set_database_firewall_rule](https://msdn.microsoft.com/library/dn270010.aspx) i [sp_delete_database_firewall_rule](https://msdn.microsoft.com/library/dn270030.aspx).

## <a name="next-steps"></a>Następne kroki

Dla jak do artykułów na temat tworzenia reguły zapory na poziomie serwera przy użyciu innych metod, zobacz: 

- [Skonfiguruje reguły zapory poziomie serwera bazy danych SQL Azure za pomocą portalu Azure](sql-database-configure-firewall-settings.md)
- [Skonfiguruje reguły zapory poziomie serwera bazy danych SQL Azure za pomocą programu PowerShell](sql-database-configure-firewall-settings-powershell.md)
- [Skonfiguruje reguły zapory poziomie serwera bazy danych SQL Azure za pomocą interfejsu API usługi REST](sql-database-configure-firewall-settings-rest.md)

Samouczek dotyczący tworzenia bazy danych zobacz [Tworzenie bazy danych SQL w minutach za pomocą portalu Azure](sql-database-get-started.md).
Aby uzyskać pomoc podczas łączenia się z bazą danych programu Azure SQL z Otwórz źródło lub aplikacji innych firm zobacz [klienta szybki start przykłady kodu do bazy danych SQL](https://msdn.microsoft.com/library/azure/ee336282.aspx).
Aby dowiedzieć się, jak przejść do baz danych, zobacz [Zarządzanie zabezpieczeniami bazy danych programu access i logowania](https://msdn.microsoft.com/library/azure/ee336235.aspx).


## <a name="additional-resources"></a>Dodatkowe zasoby

- [Zabezpieczanie bazy danych](sql-database-security.md)
- [Centrum zabezpieczeń dla aparatu bazy danych programu SQL Server i baza danych SQL Azure](https://msdn.microsoft.com/library/bb510589)
