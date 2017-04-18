<properties
   pageTitle="Odzyskiwanie bazy danych SQL | Microsoft Azure"
   description="Dowiedz się, jak odzyskać bazy danych z awaria regionalnego centrum danych lub błąd Azure SQL Replikacja bazy danych Active Geo- i możliwości, przywracanie Geo."
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/13/2016"
   ms.author="carlrab"/>

# <a name="restore-an-azure-sql-database-or-failover-to-a-secondary"></a>Przywracanie bazy danych SQL Azure ani przełączenie awaryjne pomocniczym

Baza danych SQL Azure oferuje następujące możliwości odzyskiwania z awarii:

- [Replikacja Geo Active](sql-database-geo-replication-overview.md)
- [Przywracanie Geo](sql-database-recovery-using-backups.md#point-in-time-restore)

Aby dowiedzieć się o scenariusze biznesowe ciągłości i funkcji obsługujących scenariuszom, zobacz [ciągłości](sql-database-business-continuity.md).

### <a name="prepare-for-the-event-of-an-outage"></a>Przygotowywanie do zdarzenia awarii

Praktyczne z odzyskiwania do innego obszaru danych przy użyciu aktywnej replikacji Geo lub zbędne geo wykonywania kopii zapasowych, należy przygotować serwer w innym centrum danych awarii do są zamieniane na nowy podstawowy serwer należy mogą pojawić się, jak również dobrze zdefiniowane kroki opisane i przetestowane w celu zapewnienia płynnego odzyskiwania. Przeprowadzania czynności przygotowawczych obejmują:

- Określ serwer logicznych w innym regionie do są zamieniane na nowy podstawowy serwer. Z aktywnej Geo replikacji, będzie co najmniej jeden i ewentualnie wszystkich serwerów pomocniczych. Do przywracania Geo jest to zazwyczaj serwer w [regionie iloczynów](../best-practices-availability-paired-regions.md) region, w którym znajduje się bazy danych.
- Identyfikowanie i opcjonalnie zdefiniować, reguły zapory na poziomie serwera potrzebne użytkownikom dostępu do nowego podstawowego bazy danych.
- Określ, jak chcesz przekierować użytkowników do nowego podstawowego serwera, takich jak, zmieniając parametry połączenia lub zmieniając wpisy DNS.
- Identyfikowanie i opcjonalnie można utworzyć, logowania, które muszą znajdować się w główną bazą danych w nowym serwerem podstawowym i upewnij się, że te logowania do odpowiednich uprawnień w bazie danych wzorca, jeśli istnieją. Aby uzyskać więcej informacji zobacz [Zabezpieczenia bazy danych SQL po awarii](sql-database-geo-replication-security-config.md)
- Identyfikowanie reguły alertów, które będzie konieczne będzie aktualizowany w celu zamapowania nowego podstawowej bazy danych.
- Dokument konfiguracji inspekcji na podstawową bazą danych
- Wykonywanie [przechodzić do awarii](sql-database-disaster-recovery-drills.md). Aby symulować awarii do przywrócenia Geo, możesz Usuń lub zmień spowodować błąd łączność aplikacji źródłowej bazy danych. Aby symulować awaria dla aktywnego Geo replikacji, możesz wyłączyć aplikacji sieci web lub maszyn wirtualnych połączony z bazy danych ani przełączenie awaryjne bazy danych powodować błędy connectity aplikacji.

## <a name="when-to-initiate-recovery"></a>Kiedy należy zainicjować odzyskiwania

Odzysk wpływa na aplikacji. Wymaga zmiany parametrów połączenia SQL lub przekierowywanie za pomocą DNS, a może spowodować utratę trwały danych. W związku z tym należy je wykonywać tylko wtedy, gdy awaria prawdopodobnie dłużej niż cel czasu odzyskiwania aplikacji. Po wdrożeniu aplikacji produkcji możesz wykonywać regularne monitorowanie kondycji aplikacji i potwierdzenia, że jest uzasadniona odzyskiwania za pomocą następujących punktów danych:

1.  Błąd trwały łączności z poziomu aplikacji do bazy danych.
2.  Azure portal zawiera alert dotyczący zdarzenia w regionie efektownych ogólne.
3.  Serwer bazy danych SQL Azure jest oznaczony o obniżonej wydajności.

W zależności od usługi na uszkodzenia aplikacji przestoje i możliwości odpowiedzialności firm należy rozważyć następujące opcje odzyskiwania.

Użyj [Uzyskiwanie odzyskania bazy danych](https://msdn.microsoft.com/library/dn800985.aspx) (*LastAvailableBackupDate*), uzyskanie najnowszych punkt replikowane Geo przywracania.

## <a name="wait-for-service-recovery"></a>Poczekaj, aż odzyskiwanie usługi

Praca Azure członkom zespołu dokładnie, aby przywrócić dostępność usługi, jak szybko jak to możliwe, ale w zależności od pierwiastek spowodować może potrwać godziny i dni.  Jeśli aplikacja przeszkadzają znaczną przestoje możesz po prostu poczekać odzyskiwania do wykonania. W tym przypadku jest wymagane żadne działanie ze strony użytkownika. Można wyświetlić bieżący stan usługi w naszym [Pulpit nawigacyjny kondycji usługi Azure](https://azure.microsoft.com/status/). Po wykonaniu odzyskiwania regionu dostępności aplikacji zostaną przywrócone.

## <a name="failover-to-geo-replicated-secondary-database"></a>Przełączanie awaryjne do replikowane geo pomocniczego bazy danych

Jeśli odpowiedzialności firm może spowodować przestoje aplikacji należy używać replikowane geo baz danych w aplikacji. Umożliwi on aplikacji w celu szybkiego przywrócenia dostępność w innym regionie w przypadku awarii. Dowiedz się, jak [skonfigurować Geo replikacji](sql-database-geo-replication-portal.md).

Aby przywrócić dostępność baz danych, aby zainicjować przełączanie awaryjne do pomocniczej replikowane geo przy użyciu jednego z obsługiwanych metod.

Użyj jednej z następujących przewodników na przełączanie awaryjne do replikowane geo pomocniczej bazy danych:

- [Przełączanie awaryjne do pomocniczym replikowane geo przy użyciu Azure Portal](sql-database-geo-replication-portal.md)
- [Przełączanie awaryjne do pomocniczym replikowane geo przy użyciu programu PowerShell](sql-database-geo-replication-powershell.md)
- [Przełączanie awaryjne do pomocniczym replikowane geo za pomocą T-SQL](sql-database-geo-replication-transact-sql.md)

## <a name="recover-using-geo-restore"></a>Odzyskiwanie przy użyciu Geo przywracania

Jeśli przestoje aplikacji nie powoduje odpowiedzialności firm umożliwia przywracanie Geo jako metoda odzyskać z baz danych aplikacji. Tworzy kopię bazy danych z jej najnowszych zbędne geo kopii zapasowej.

Użyj jednej z następujących czynności prowadzi do przywrócenia geo bazy danych do nowego regionu:

- [Geo-Przywróć bazę danych do nowego regionu za pomocą Azure Portal](sql-database-geo-restore-portal.md)
- [Geo-Przywróć bazę danych do nowego regionu przy użyciu programu PowerShell](sql-database-geo-restore-powershell.md)

## <a name="configure-your-database-after-recovery"></a>Konfigurowanie bazy danych po odzyskiwania

Jeśli korzystasz z pracą awaryjną replikacji geo lub Przywróć geo odzyskiwaniu z awarii, należy się upewnić, że łączności z nowych baz danych jest poprawnie skonfigurowany, dlatego funkcja normalnego aplikacji można wznowić. To jest lista kontrolna zadań na przygotowanie produkcji odzyskanego bazy danych.

### <a name="update-connection-strings"></a>Aktualizowanie ciągów połączeń

Ponieważ odzyskanego bazy danych będzie znajdować się w innym serwerze, musisz zaktualizować parametry połączenia aplikacji, aby wskazywały tego serwera.

Aby uzyskać więcej informacji na temat zmieniania parametry połączenia Zobacz język rozwoju odpowiedniej [biblioteki połączeń](sql-database-libraries.md).

### <a name="configure-firewall-rules"></a>Skonfiguruje reguły zapory

Potrzebujesz upewnić się, że reguły zapory skonfigurowany na serwerze i w bazie danych odpowiadają układowi i skonfigurowane na podstawowy serwer i podstawowej bazy danych. Aby uzyskać więcej informacji, zobacz [jak: Konfigurowanie ustawień zapory (bazy danych SQL Azure)](sql-database-configure-firewall-settings.md).


### <a name="configure-logins-and-database-users"></a>Konfigurowanie logowania i baz danych użytkowników

Potrzebujesz upewnić się, że wszystkie logowania używane przez aplikację istnieje na serwerze, który obsługuje odzyskanego bazy danych. Aby uzyskać więcej informacji zobacz [Konfiguracja zabezpieczeń Geo replikacji](sql-database-geo-replication-security-config.md).

>[AZURE.NOTE] Należy konfigurowanie i testowanie reguł zapory serwera i logowania (i ich uprawnień) podczas przechodzenia odzyskiwania po awarii. Tych obiektów na poziomie serwera i konfiguracją może być niedostępne podczas awarii.

### <a name="setup-telemetry-alerts"></a>Konfigurowanie alertów telemetrycznego

Musisz upewnij się, że istniejących ustawień alertu zostaną zaktualizowane w celu zamapowania odzyskanego bazy danych i inny serwer.

Aby uzyskać więcej informacji na temat reguł alertów bazy danych zobacz [Otrzymywać powiadomienia alertów](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) i [Śledź kondycję usługi](../monitoring-and-diagnostics/insights-service-health.md).

### <a name="enable-auditing"></a>Włączanie inspekcji

Jeśli inspekcja jest wymagany dostęp do bazy danych, musisz włączyć inspekcja po odzyskiwania bazy danych. Dobrym wskaźnikiem, że inspekcja jest wymagane jest, aplikacje klienckie wykorzystania ciągów bezpiecznego połączenia wzorzec *. database.secure.windows.net. Aby uzyskać więcej informacji zobacz [Wprowadzenie do inspekcji bazy danych SQL](sql-database-auditing-get-started.md).


## <a name="next-steps"></a>Następne kroki

- Aby uzyskać informacje o bazy danych SQL Azure automatycznego wykonywania kopii zapasowych, zobacz [Baza danych SQL automatycznego wykonywania kopii zapasowych](sql-database-automated-backups.md)
- Aby dowiedzieć się o ciągłości projektu i odzyskiwanie scenariusze, zobacz [ciągłości scenariusze](sql-database-business-continuity.md)
- Aby dowiedzieć się o używaniu kopie zapasowe automatycznego odzyskiwania, zobacz [Przywracanie bazy danych z kopii zapasowych zainicjowane usługi](sql-database-recovery-using-backups.md)
