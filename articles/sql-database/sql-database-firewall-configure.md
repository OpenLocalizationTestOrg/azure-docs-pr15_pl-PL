<properties
   pageTitle="Konfigurowanie Omówienie usługi SQL server Zapora | Microsoft Azure"
   description="Dowiedz się, jak skonfigurować zaporę bazy danych SQL z reguły zapory serwera poziomie bazy danych i zarządzanie dostępem."
   keywords="zapory bazy danych"
   services="sql-database"
   documentationCenter=""
   authors="BYHAM"
   manager="jhubbard"
   editor="cgronlun"
   tags=""/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="09/14/2016"
   ms.author="rickbyh"/>

# <a name="configure-azure-sql-database-firewall-rules---overview"></a>Skonfiguruje reguły zapory bazy danych SQL Azure \- — omówienie


> [AZURE.SELECTOR]
- [Omówienie](sql-database-firewall-configure.md)
- [Azure Portal](sql-database-configure-firewall-settings.md)
- [TSQL](sql-database-configure-firewall-settings-tsql.md)
- [Programu PowerShell](sql-database-configure-firewall-settings-powershell.md)
- [INTERFEJSU API USŁUGI REST](sql-database-configure-firewall-settings-rest.md)


Microsoft Azure SQL Database zawiera usługi relacyjnej bazy danych dla Azure i innych aplikacjach internetowych. Aby chronić dane, zapory zapobieganie dostęp z serwerem bazy danych do momentu określenia, które komputery mają uprawnienia. Zapory udziela dostępu do bazy danych na podstawie źródłowego adresu IP każdego żądania.

Aby skonfigurować zaporę, możesz utworzyć reguły zapory, określające zakresy adresów IP przyjęcia. Umożliwia tworzenie reguł zapory na poziomie serwera i bazy danych.

- **Reguły zapory poziomie serwera:** Reguły te umożliwiają klientom dostępu do całego serwera Azure SQL, oznacza to, że wszystkie bazy danych na tym samym serwerze logiczne. Te reguły są przechowywane w bazie danych **wzorca** . Reguły zapory na poziomie serwera można skonfigurować za pomocą portalu lub przy użyciu instrukcji Transact-SQL.
- **Reguły zapory poziom bazy danych:** Te reguły umożliwić klientom dostępu do poszczególnych baz danych w ramach serwer bazy danych SQL Azure. Możesz utworzyć te reguły dla każdej bazy danych i są one przechowywane w pojedyncze bazy danych. (Tworzenie reguły zapory poziom bazy danych dla **wzorca** bazy danych). Reguły te mogą być pomocne w ograniczanie dostępu do niektórych (bezpieczne) bazy danych, w tym samym serwerze logiczne. Reguły zapory poziom bazy danych można skonfigurować tylko za pomocą instrukcji Transact-SQL.

**Zalecenie:** Firma Microsoft zaleca używanie reguły zapory poziom bazy danych, o ile to możliwe, aby zwiększyć bezpieczeństwo i utworzyć bardziej przenośne bazy danych. Za pomocą reguły zapory na poziomie serwera dla administratorów i gdy wiele baz danych, które mają takie same wymagania programu access i nie chcesz poświęcać czasu na konfigurowanie pojedynczo każdej bazy danych.


## <a name="firewall-overview"></a>Omówienie zapory

Początkowo języku Transact-SQL dostęp do serwera Azure SQL jest zablokowany przez zaporę. Aby rozpocząć korzystanie z serwera Azure SQL, możesz przejdź do portalu Azure i określ jedną lub więcej reguł zaporę na poziomie serwera, które umożliwiają dostęp do serwera Azure SQL. Określ, które zakresy adresów IP z Internetu są dozwolone i czy Azure aplikacji może próbować połączyć się z serwerem Azure SQL za pomocą reguły zapory.

Aby selektywne udzielić dostępu do tylko jednej z baz danych na serwerze Azure SQL, należy utworzyć regułę poziom bazy danych dla wymagane bazy danych. Określ zakres adresów IP dla reguły zapory bazy danych, który wykracza poza zakres adresów IP określony w regule zapory poziomie serwera, a następnie upewnij się, że adres IP klienta znajduje się w zakresie określonym w regule poziom bazy danych.

Próby nawiązania połączenia z Internetem i Azure musi najpierw upłynąć przez zaporę osiągnięcia serwera Azure SQL lub bazy danych SQL, jak pokazano na poniższym rysunku.

   ![Diagram opisujący konfigurację zapory.][1]

## <a name="connecting-from-the-internet"></a>Nawiązywanie połączenia z Internetem

Gdy komputer próba nawiązania połączenia z serwerem bazy danych z Internetu, zapory najpierw sprawdza źródłowym adres IP żądania przed pełny zestaw reguł zapory:

- W przypadku żądania adresu IP w jednej z zakresu określonego reguły zapory na poziomie serwera, połączenie otrzymuje się z serwerem bazy danych SQL Azure.

- Jeśli adres IP żądania nie jest w jednej z zakresu określonego reguły zapory na poziomie serwera, reguły zapory poziom bazy danych są zaznaczone. W przypadku żądania adresu IP w jednej z zakresu określonego reguły zapory poziom bazy danych, połączenie jest udzielony tylko do bazy danych, której reguły dopasowania poziom bazy danych.

- Jeśli adres IP żądania nie mieści się w do zakresu określonego w każdym z reguły zapory na poziomie serwera lub poziom bazy danych, wezwanie na połączenie kończy się niepowodzeniem.

> [AZURE.NOTE] Aby uzyskać dostęp do bazy danych SQL Azure z komputera lokalnego, upewnij się, że zapory w sieci i komputera lokalnego umożliwia komunikacji wychodzącej na TCP port 1433.


## <a name="connecting-from-azure"></a>Nawiązywanie połączenia z platformy Azure

Gdy aplikacja z platformy Azure próba nawiązania połączenia z serwerem bazy danych, zapory sprawdza, czy Azure połączenia są dozwolone. Zapora ustawienie rozpoczęcia i zakończenia adres równa 0.0.0.0 oznacza, że połączenia te są dozwolone. Jeśli próba nawiązania połączenia nie jest dozwolona, żądanie nie połączenia z serwerem bazy danych SQL Azure.

> [AZURE.IMPORTANT] Ta opcja konfiguruje zaporę, aby umożliwić wszystkich połączeń z tym połączeń Azure z subskrypcji innych odbiorców. Po zaznaczeniu tej opcji, upewnij się, że do logowania i uprawnień użytkowników ograniczysz dostęp tylko autoryzowani użytkownicy.

Można włączyć połączenia z platformy Azure na dwa sposoby:

- W [portalu Microsoft Azure](https://portal.azure.com/)zaznacz pole wyboru **Zezwalaj usług azure do serwera dostępu** podczas tworzenia nowego serwera.

- W [portalu klasyczny](http://go.microsoft.com/fwlink/p/?LinkID=161793)na karcie **Konfigurowanie** na serwerze, w sekcji **Dozwolone usług** dla **Usługi Microsoft Azure**kliknij przycisk **Tak** .

## <a name="creating-the-first-server-level-firewall-rule"></a>Tworzenie pierwszej reguły zapory na poziomie serwera

Pierwsze ustawienie zaporę na poziomie serwera można tworzyć za pomocą [Azure portal](https://portal.azure.com/) lub programowo przy użyciu interfejsu API usługi REST lub Azure programu PowerShell. Reguły kolejnych zapory poziomie serwera mogą być tworzone i zarządzane za pomocą tych metod, a także jak za pomocą języka Transact-SQL. Aby zwiększyć wydajność, reguły zapory na poziomie serwera tymczasowo w pamięci podręcznej na poziomie bazy danych. Aby odświeżyć pamięci podręcznej, zobacz [DBCC FLUSHAUTHCACHE](https://msdn.microsoft.com/library/mt627793.aspx). Aby uzyskać więcej informacji dotyczących reguły zapory na poziomie serwera, zobacz [jak: Konfigurowanie zapory serwera Azure SQL za pomocą portalu Azure](sql-database-configure-firewall-settings.md).

## <a name="creating-database-level-firewall-rules"></a>Tworzenie reguły zapory poziom bazy danych

Po skonfigurowaniu pierwszego zapory poziomie serwera, można ograniczyć dostęp do niektórych baz danych. Jeśli określony zakres adresów IP w regule zapory poziom bazy danych, który jest spoza zakresu określonego w poziomie serwera reguła, tylko tych klientów, którzy mają adresy IP w zakresie poziom bazy danych można uzyskać dostęp do bazy danych. Możesz mieć maksymalnie reguł 128 zapory poziom bazy danych w bazie danych. Reguły zapory bazy danych na poziomie wzorca i użytkownika bazy danych można tworzyć i zarządzać za pomocą języka Transact-SQL. Aby uzyskać więcej informacji na temat konfigurowania reguły zapory poziom bazy danych zobacz [sp_set_database_firewall_rule (baz danych programu SQL Azure)](https://msdn.microsoft.com/library/dn270010.aspx).

## <a name="programmatically-managing-firewall-rules"></a>Zarządzanie programowy reguły zapory

Oprócz portal Azure można zarządzać reguły zapory programowo przy użyciu języka Transact-SQL, interfejsu API usługi REST i Azure programu PowerShell. W poniższych tabelach opisano zestaw poleceń dostępnych dla każdej z tych metod.


### <a name="transact-sql"></a>Transact-SQL

| Widok wykazu lub procedura przechowywana                                                           | Poziom     | Opis                                          |
|--------------------------------------------------------------------------------------------|-----------|------------------------------------------------------|
| [sys.firewall_rules](https://msdn.microsoft.com/library/dn269980.aspx)                   | Serwer    | Wyświetla bieżący reguły zapory na poziomie serwera     |
| [sp_set_firewall_rule](https://msdn.microsoft.com/library/dn270017.aspx)             | Serwer    | Tworzenie lub aktualizowanie reguły zapory na poziomie serwera       |
| [sp_delete_firewall_rule](https://msdn.microsoft.com/library/dn270024.aspx)          | Serwer    | Usuwa reguły zapory na poziomie serwera                  |
| [sys.database_firewall_rules](https://msdn.microsoft.com/library/dn269982.aspx)        | Bazy danych  | Wyświetla bieżący reguły zapory poziom bazy danych   |
| [sp_set_database_firewall_rule](https://msdn.microsoft.com/library/dn270010.aspx)    | Bazy danych  | Tworzenie lub aktualizowanie reguł zapory poziom bazy danych |
| [sp_delete_database_firewall_rule](https://msdn.microsoft.com/library/dn270030.aspx) | Bazy danych | Usuwa reguły zapory poziom bazy danych                |

### <a name="rest-api"></a>INTERFEJSU API USŁUGI REST

| INTERFEJS API                  | Poziom  | Opis                                                      |
|----------------------|--------|------------------------------------------------------------------|
| [Lista reguł zapory](https://msdn.microsoft.com/library/azure/dn505715.aspx)  | Serwer | Wyświetla bieżący reguły zapory na poziomie serwera                 |
| [Tworzenie reguły zapory](https://msdn.microsoft.com/library/azure/dn505712.aspx) | Serwer | Tworzenie lub aktualizowanie reguły zapory na poziomie serwera                   |
| [Ustawianie reguły zapory](https://msdn.microsoft.com/library/azure/dn505707.aspx)    | Serwer | Aktualizuje właściwości istniejącej reguły zapory na poziomie serwera |
| [Usuwanie reguły zapory](https://msdn.microsoft.com/library/azure/dn505706.aspx) | Serwer | Usuwa reguły zapory na poziomie serwera                              |


### <a name="azure-powershell"></a>Azure programu PowerShell

| Polecenie cmdlet                                                                                              | Poziom  | Opis                                                      |
|-----------------------------------------------------------------------------------------------------|--------|------------------------------------------------------------------|
| [Get-AzureSqlDatabaseServerFirewallRule](https://msdn.microsoft.com/library/azure/dn546731.aspx)    | Serwer | Zwraca bieżącą reguły zapory poziomie serwera                  |
| [Nowy AzureSqlDatabaseServerFirewallRule](https://msdn.microsoft.com/library/azure/dn546724.aspx)    | Serwer | Umożliwia utworzenie nowej reguły zapory poziomie serwera                         |
| [Ustawianie AzureSqlDatabaseServerFirewallRule](https://msdn.microsoft.com/library/azure/dn546739.aspx)    | Serwer | Aktualizuje właściwości istniejącej reguły zapory na poziomie serwera |
| [Usuń AzureSqlDatabaseServerFirewallRule](https://msdn.microsoft.com/library/azure/dn546727.aspx) | Serwer | Usuwa reguły zapory na poziomie serwera                              |

> [AZURE.NOTE] Mogą występować w górę jak 5 minut opóźnienia zmiany w ustawieniach zapory została uwzględniona.

## <a name="troubleshooting-the-database-firewall"></a>Rozwiązywanie problemów z zaporą bazy danych

Jeśli dostęp do usługi Microsoft Azure SQL Database nie działają zgodnie z oczekiwaniami, rozważ następujące wskazówki:

- **Konfiguracja zapory lokalnej:** Przed na komputerze można uzyskać dostęp do bazy danych SQL Azure, może być konieczne tworzenie wyjątku zapory na komputerze TCP port 1433. Jeśli tworzysz połączenia wewnątrz krawędzi chmura Azure, może być konieczne Otwórz dodatkowe porty. Aby uzyskać więcej informacji, zobacz **w wersji 12 SQL Database: poza w porównaniu z wewnątrz** sekcji [porty inne niż 1433 4,5 ADO.NET i wersji 12 bazy danych SQL](sql-database-develop-direct-route-ports-adonet-v12.md).

- **Adres translatora adresów sieciowych:** Ze względu na translatora adresów Sieciowych adres IP używany przez komputer nawiązywania połączenia z bazą danych SQL Azure może być inny niż adres IP wyświetlane w ustawieniach konfiguracji adresów IP komputera. Aby wyświetlić adres IP, który korzysta komputer nawiązać Azure, zaloguj się do portalu i przejdź do karty **Konfigurowanie** na serwerze, który obsługuje bazy danych. W sekcji **Dozwolonych adresów IP** jest wyświetlany **Bieżący adres IP klienta** . Kliknij przycisk **Dodaj** do **Dozwolonych adresów IP** umożliwia tego komputera, aby uzyskać dostęp do serwera.

- **Zmiany do listy dozwolonych nie zostały uwzględnione jeszcze:** Może to być jak 5 minut opóźnienia zmiany w konfiguracji zapory bazy danych SQL Azure została uwzględniona.

- **Logowania nie jest autoryzowany lub użyto niepoprawnego hasła:** Jeśli logowanie nie ma uprawnień na serwerze bazy danych SQL Azure lub hasło używane jest nieprawidłowa, odmowa połączenia z serwerem bazy danych SQL Azure. Tworzenie ustawień zapory zapewnia tylko klientów z szansą nawiązania połączenia z serwerem; Każdy komputer kliencki należy podać poświadczenia zabezpieczeń to konieczne. Aby uzyskać więcej informacji na temat przygotowywania logowania Zobacz Zarządzanie bazami danych, logowania i użytkowników w bazie danych SQL Azure.

- **Dynamicznego adresu IP:** Jeśli masz połączenie z Internetem z dynamiczne adresy IP i występują problemy z konfiguracją przez zaporę, może spróbuj wykonać jedną z następujących rozwiązań:

 - Poproś usługodawcy internetowego (ISP) dla zakresu adresów IP przydzielonych na komputerach klienckich łączących się z serwerem bazy danych SQL Azure, a następnie Dodaj zakres adresów IP jako reguły zapory.

 - Uzyskiwanie statyczne adresy IP zamiast tego dla komputerów klienckich, a następnie dodaj adresy IP jako reguły zapory.

## <a name="next-steps"></a>Następne kroki

Jak do artykułów na temat tworzenia reguły zapory serwera i poziomu bazy danych, zobacz:

- [Skonfiguruje reguły zapory poziomie serwera bazy danych SQL Azure za pomocą portalu Azure](sql-database-configure-firewall-settings.md)
- [Skonfiguruje reguły zapory serwera i poziomu bazy danych bazy danych SQL Azure za pomocą T-SQL](sql-database-configure-firewall-settings-tsql.md)
- [Skonfiguruje reguły zapory poziomie serwera bazy danych SQL Azure za pomocą programu PowerShell](sql-database-configure-firewall-settings-powershell.md)
- [Skonfiguruje reguły zapory poziomie serwera bazy danych SQL Azure za pomocą interfejsu API usługi REST](sql-database-configure-firewall-settings-rest.md)

Samouczek dotyczący tworzenia bazy danych zobacz [Tworzenie bazy danych SQL w minutach za pomocą portalu Azure](sql-database-get-started.md).
Aby uzyskać pomoc podczas łączenia się z bazą danych programu Azure SQL z Otwórz źródło lub aplikacji innych firm zobacz [klienta szybki start przykłady kodu do bazy danych SQL](https://msdn.microsoft.com/library/azure/ee336282.aspx).
Aby dowiedzieć się, jak przejść do baz danych, zobacz [Zarządzanie zabezpieczeniami bazy danych programu access i logowania](https://msdn.microsoft.com/library/azure/ee336235.aspx).



## <a name="additional-resources"></a>Dodatkowe zasoby

- [Zabezpieczanie bazy danych](sql-database-security.md)
- [Centrum zabezpieczeń dla aparatu bazy danych programu SQL Server i baza danych SQL Azure](https://msdn.microsoft.com/library/bb510589)

<!--Image references-->
[1]: ./media/sql-database-firewall-configure/sqldb-firewall-1.png
