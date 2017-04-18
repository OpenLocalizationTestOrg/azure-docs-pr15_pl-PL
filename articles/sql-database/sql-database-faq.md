<properties 
   pageTitle="Baza danych SQL Azure — często zadawane pytania" 
   description="Odpowiedzi na typowe pytania klientów zapytaj o baz danych w chmurze i bazy danych SQL Azure, system zarządzania relacyjnymi bazami danych firmy Microsoft (RDBMS) i bazy danych jako usługa w chmurze." 
   services="sql-database" 
   documentationCenter="" 
   authors="CarlRabeler" 
   manager="jhubbard" 
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-management" 
   ms.date="08/16/2016"
   ms.author="sashan;carlrab"/>

# <a name="sql-database-faq"></a>Baza danych SQL — często zadawane pytania

## <a name="how-does-the-usage-of-sql-database-show-up-on-my-bill"></a>Jak zastosowania bazy danych SQL są wyświetlane na rachunku? 
Baza danych SQL rozliczenia na przewidywalne stawki godzinowe oparte na warstwie usługi + poziom wydajności dla pojedynczego baz danych lub eDTUs na pulę elastyczne bazy danych. Użycie rzeczywiste jest obliczane i proporcjonalny co godzinę, dzięki czemu rachunku mogą być wyświetlane ułamki godziny. Na przykład jeśli istnieje bazy danych na 12 godzin w ciągu miesiąca, rachunku zawiera zastosowania 0,5 dni. Ponadto warstwy usługi + poziom wydajności i eDTUs na pulę są podzielone na rachunku, aby ułatwić użytkownikom liczbę dni bazy danych, używanej dla każdej z nich w jeden miesiąc.

## <a name="what-if-a-single-database-is-active-for-less-than-an-hour-or-uses-a-higher-service-tier-for-less-than-an-hour"></a>Co zrobić, jeśli jednej bazie danych jest aktywna mniej niż przez godzinę lub używa wyższego poziomu usług dla mniej niż godzinę?
Konta dla każdego godzinę, która istnieje bazy danych przy użyciu najwyższego poziomu usługi + poziom wydajności, zastosowany ciągu danej godziny, niezależnie od zastosowania i czy baza danych była aktywna mniej niż przez godzinę. Jeśli tworzysz jednej bazie danych i usuń ją później pięć minut rachunku odzwierciedla opłatę przez godzinę jedną bazę danych. 

Przykłady
    
- Jeśli tworzenie podstawowej bazy danych, a następnie natychmiast uaktualnić go do standardowego S1, są naliczane stawki standardowej S1 na pierwszej godziny.

- Po uaktualnieniu bazy danych przy użyciu Basic do Premium 10 o godzinie: 00 i zakończeniu uaktualniania 1:35 o godzinie w następnym dniu jest rozliczana według stawki Premium, począwszy od 1:00 

- Jeśli o godzinie 11:00 starszą wersję bazy danych z Premium podstawowe Projekt zostanie zrealizowany w godzinie 2:15, a następnie bazy danych jest rozliczana według stawki Premium do 3:00 PM, po upływie którego jest rozliczana według stawki podstawowe.

## <a name="how-does-elastic-database-pool-usage-show-up-on-my-bill-and-what-happens-when-i-change-edtus-per-pool"></a>Jak użycie puli elastyczne bazy danych są wyświetlane na rachunku oraz co się stanie, gdy zostanie zmieniony eDTUs na pulę?
Opłaty puli elastyczne bazy danych wyświetlanych na rachunku jako elastyczną DTUs (eDTUs) etapami widoczny pod eDTUs na pulę na [stronie cennik](https://azure.microsoft.com/pricing/details/sql-database/). Istnieje bezpłatne pul elastyczne bazy danych na bazę danych. Konta dla każdej godziny istnieje puli na najwyższym eDTU, niezależnie od zastosowania i czy puli była aktywna mniej niż przez godzinę. 

Przykłady

- Po utworzeniu puli standardowy elastyczne bazy danych z 200 eDTUs o godzinie 11:18 dodanie pięć baz danych do puli, naliczanego za usługę 200 eDTUs całego godzinę, zaczyna się o godzinie 11 do końca dnia.
- Na dzień 2, 5 o godzinie: 05 bazy danych 1 zaczyna się przez inne 50 eDTUs i przytrzymuje stabilny do dnia. Baz danych 2-5 zmieniają się w przedziale od 0 do 80 eDTUs. W ciągu dnia możesz dodać pięciu innych baz danych, które używają różnych eDTUs w ciągu dnia. Dzień 2 jest cały dzień wystawiona na 200 eDTU. 
- W dniu 3, 5 o godzinie Dodawanie innego 15 baz danych. Użycie bazy danych zwiększa w ciągu dnia do punktu zadecydujesz, aby zwiększyć eDTUs puli od 200 do 400 8 o godzinie: 05 Opłaty na poziomie 200 eDTU były obowiązywać do 20: 00 i zwiększa do 400 eDTUs pozostałe cztery godziny. 

## <a name="how-does-the-use-of-active-geo-replication-in-an-elastic-database-pool-show-up-on-my-bill"></a>Jak korzystanie z aktywnego Geo replikacji w puli elastyczne bazy danych są wyświetlane na rachunku?
W przeciwieństwie do jednej bazy danych [Active replikacji Geo](sql-database-geo-replication-overview.md) za pomocą elastyczne bazy danych nie ma bezpośredni wpływ rozliczeń.  Tylko naliczanego za usługę eDTUs obsługi administracyjnej dla każdej z nich pul (puli podstawowy i pomocniczy puli)

## <a name="how-does-the-use-of-the-auditing-feature-impact-my-bill"></a>Jak korzystanie z funkcji inspekcji wpłynie na rachunku? 
Inspekcja jest wbudowana usługa bazy danych SQL bez dodatkowych kosztów i jest dostępny dla Basic, Standard i Premium baz danych. Jednak aby przechowywać dzienniki inspekcji, inspekcji zastosowania funkcji, konto Azure miejsca do magazynowania i stawek dla tabel i kolejek w magazynie Azure opłata na podstawie rozmiaru dziennika inspekcji.

## <a name="how-do-i-find-the-right-service-tier-and-performance-level-for-single-databases-and-elastic-database-pools"></a>Jak znaleźć poziomie warstwa i wydajności usługi prawo dla pojedynczego baz danych i pule elastyczne bazy danych? 
Istnieje kilka narzędzi dla Ciebie dostępne. 

- Dla lokalnej bazy danych zalecamy baz danych i DTUs wymagane za pomocą [DTU advisor zmiany rozmiaru](http://dtucalculator.azurewebsites.net/) , a ocenianie wiele baz danych dla pul elastyczne bazy danych.
- Jeśli w jednej bazie danych będzie korzystać z w puli, aparat inteligentne firmy Azure zaleca puli elastyczne bazy danych Jeśli wzorzec zastosowania historycznych, która go. Zobacz [Monitor i zarządzanie nimi puli elastyczne bazy danych za pomocą portalu Azure](sql-database-elastic-pool-manage-portal.md). Aby uzyskać szczegółowe informacje o sobie wykonywanie obliczeń, zobacz [Wydajność i cena zagadnienia związane z puli elastyczne bazy danych](sql-database-elastic-pool-guidance.md)
- Aby sprawdzić, czy chcesz wybrać jednej bazie danych w górę lub w dół, zobacz [wskazówki na temat wydajności dla pojedynczego baz danych](sql-database-performance-guidance.md).

## <a name="how-often-can-i-change-the-service-tier-or-performance-level-of-a-single-database"></a>Jak często można zmienić poziom warstwa lub wydajności usługi jednej bazie danych 
Z bazy danych w wersji 12 możesz zmienić warstwie usługi (między Basic, Standard i Premium) lub wydajności poziomu Warstwa usługi (na przykład S1, s2) tak często, jak chcesz. Dla starszych wersji baz danych można zmienić poziom warstwa lub wydajności usługi łącznie cztery razy w okresie 24-godzinnym.

##<a name="how-often-can-i-adjust-the-edtus-per-pool"></a>Jak często można dostosować eDTUs na pulę? 
Tak często, jak chcesz.

## <a name="how-long-does-it-take-to-change-the-service-tier-or-performance-level-of-a-single-database-or-move-a-database-in-and-out-of-an-elastic-database-pool"></a>Jak długo trwa Aby zmienić poziom warstwa lub wydajności usługi jednej bazie danych lub przenieść bazę danych i puli elastyczne bazy danych? 
Zmienianie poziomu usług bazy danych i przenoszenie i puli wymaga bazy danych do skopiowania na platformie jako operacja tła. Zmienianie poziomu usług może potrwać od kilku minut do kilku godzin, w zależności od rozmiaru bazy danych. W obu przypadkach baz danych pozostaną online i dostępne podczas przenoszenia. Aby uzyskać szczegółowe informacje na temat zmieniania jednego baz danych zobacz [Zmienianie warstwa usług bazy danych](sql-database-scale-up.md). 

## <a name="when-should-i-use-a-single-database-vs-elastic-databases"></a>Kiedy używać jednej bazie danych, a elastyczne baz danych? 
Na ogół pul elastyczne bazy danych są przeznaczone dla typowego [wzorzec aplikacji (władz akredytacji bezpieczeństwa) oprogramowania jako usług](sql-database-design-patterns-multi-tenancy-saas-applications.md), w przypadku jednej bazie danych dla dzierżawy lub klienta. Zakup pojedyncze bazy danych i overprovisioning spotkanie zmiennej i szczytowy żądanie dla każdej bazy danych jest często nie koszty. Zarządzanie ankiety wydajności pula pule i baz danych i rozwijaniu skalować automatycznie. 

Aparat inteligentne firmy Azure zaleca puli dla baz danych podczas deseń zastosowania na to pozwala. Aby uzyskać szczegółowe informacje zobacz [zalecenia poziomu ceny bazy danych SQL](sql-database-service-tier-advisor.md). Szczegółowe wytyczne dotyczące Wybieranie między jedno- i elastycznym baz danych zobacz [Wydajność i cena zagadnienia związane z pul elastyczne bazy danych](sql-database-elastic-pool-guidance.md).

## <a name="what-does-it-mean-to-have-up-to-200-of-your-maximum-provisioned-database-storage-for-backup-storage"></a>Co to znaczy mieć maksymalnie 200% magazynie maksymalna ustanawianie bazy danych do przechowywania kopii zapasowej 
Miejsce do magazynowania kopii zapasowej jest magazynowania skojarzone z kopii zapasowych automatycznego bazy danych, używane dla [punkt-w-czasu-Przywracanie](sql-database-recovery-using-backups.md#-point-in-time-restore) i ich [Przywracanie Geo](sql-database-recovery-using-backups.md#geo-restore). Microsoft Azure SQL Database zawiera maksymalnie 200% magazynie maksymalna ustanawianie bazy danych kopii zapasowej miejsca do magazynowania bez dodatkowych opłat. Na przykład jeśli masz wystąpienie standardowej bazy danych o rozmiarze DB ustanawianie 250 GB, mają do dyspozycji 500 GB miejsca do magazynowania kopii zapasowej bez dodatkowych opłat. Jeśli bazy danych przekracza dostarczonych magazynu kopii zapasowej, możesz zmniejszyć okres przechowywania, kontaktując się z pomocy technicznej Azure lub zapłacić do przechowywania dodatkowych kopii zapasowej wystawiona stawki standardowej odczytu geograficznie zbędne miejsca do magazynowania (Pomoc Zdalna GRS). Aby uzyskać więcej informacji na GRS pomoc Zdalna rozliczenia Zobacz szczegóły ceny miejsca do magazynowania.

## <a name="im-moving-from-webbusiness-to-the-new-service-tiers-what-do-i-need-to-know"></a>Korzystam I przenoszenie z sieci Web i firmy do nowej warstwy usług, co należy wiedzieć?
Azure baz danych sieci Web SQL i małych firm są teraz wycofana. Poziomy Basic, standardowe Premium i elastyczne Zamień Ustępujący baz danych sieci Web i małych firm. Mamy dodatkowe — często zadawane pytania, które powinny być pomocne w tym okresie przejścia. [Witryny sieci Web i Business Edition zachód słońca — często zadawane pytania](sql-database-web-business-sunset-faq.md)

## <a name="what-is-an-expected-replication-lag-when-geo-replicating-a-database-between-two-regions-within-the-same-azure-geography"></a>Co to jest zwłoka replikacji oczekiwanych podczas replikacji geo bazy danych między dwoma regionami w tym samym geograficzne Azure?  
Pracujemy obecnie są obsługiwane RPO pięć sekund i opóźnienie replikacji została mniejszą niż czy po pomocniczej geo jest obsługiwany w Azure zaleca iloczynów region i w tej samej warstwie usługi.

## <a name="what-is-an-expected-replication-lag-when-geo-secondary-is-created-in-the-same-region-as-the-primary-database"></a>Co to jest zwłoka replikacji oczekiwane po utworzeniu geo pomocniczego w tym samym regionie jako podstawowej bazy danych?  
Na podstawie empiryczne danych, nie jest zbyt dużo różnica między wewnątrz region i czasu zwłoki między region replikacji po Azure zalecamy iloczynów region zostanie użyty. 

## <a name="if-there-is-a-network-failure-between-two-regions-how-does-the-retry-logic-work-when-geo-replication-is-set-up"></a>Jeśli występuje błąd sieci między dwoma regionami, jak Logika ponawiania działa po skonfigurowaniu replikacji Geo?  
W przypadku rozłączenia, możemy ponownie co 10 sekund do przywrócenia połączenia.

## <a name="what-can-i-do-to-guarantee-that-a-critical-change-on-the-primary-database-is-replicated"></a>Co można zrobić, aby mieć pewność, że są replikowane krytycznej zmiany na głównej bazy danych?
Pomocniczej geo jest replice asynchroniczna i firma Microsoft nie należy przechowywać w pełnej synchronizacji z podstawowych. Jednak firma Microsoft udostępnia metody wymuszające synchronizacji zapewnienie replikacji krytyczne zmian (na przykład aktualizacji hasła). Synchronizacja wymuszonego wpływa na wydajność, ponieważ blokuje wywołujący wątku aż wszystkie przekazane transakcje są replikowane. Aby uzyskać szczegółowe informacje zobacz [sp_wait_for_database_copy_sync](https://msdn.microsoft.com/library/dn467644.aspx). 

## <a name="what-tools-are-available-to-monitor-the-replication-lag-between-the-primary-database-and-geo-secondary"></a>Z jakich narzędzi są dostępne monitorowanie zwłoka replikacji między podstawowej bazy danych i pomocniczego geo?
Firma Microsoft uwidaczniają zwłoki w czasie rzeczywistym replikacji między podstawowej bazy danych i monitora pomocniczego geo za pośrednictwem DMV. Aby uzyskać szczegółowe informacje zobacz [sys.dm_geo_replication_link_status](https://msdn.microsoft.com/library/mt575504.aspx).
