<properties 
   pageTitle="Ćwiczenia odzyskiwania danych bazy danych SQL | Microsoft Azure" 
   description="Dowiedz się, orientacji i najważniejsze wskazówki dotyczące korzystania z bazy danych SQL Azure przeprowadzić ćwiczenia odzyskiwanie danych, które mogą pomóc zachować Twoja misja krytycznych aplikacji biznesowych mechanizm błędów i awarii." 
   services="sql-database" 
   documentationCenter="" 
   authors="anosov1960" 
   manager="jhubbard" 
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-management" 
   ms.date="07/31/2016"
   ms.author="sstein; sashan"/>

#<a name="performing-disaster-recovery-drill"></a>Wykonywanie awarii odzyskiwania zwijania i rozwijania szczegółów

Zalecane jest, jest okresowo przeprowadzana weryfikacja przygotowania aplikacji odzyskiwania przepływu pracy. Weryfikowanie zachowanie aplikacji i skutki utraty danych i/lub zakłóceń polega na tym pracy awaryjnej jest zalecane odtwarzania. Również jest wymagane przez większość standardami jako część certyfikacji ciągłości firm.

Wykonywanie rozwijania odzyskiwania po awarii składa się z:

- Imitującym awarii warstwy danych
- Odzyskiwanie 
- Sprawdź poprawność odzyskiwanie wpis integralności aplikacji

W zależności od jak, [przeznaczona aplikacji dla ciągłości](sql-database-business-continuity.md), wykonywanie Drąż w przepływie pracy mogą się różnić. Poniżej opisano najważniejsze wskazówki przeprowadzenie awarii odzyskiwania zwijania i rozwijania szczegółów w kontekście bazy danych SQL Azure. 

##<a name="geo-restore"></a>Przywracanie Geo

Aby zapobiec utracie danych podczas przeprowadzania rozwijania odzyskiwanie danych, zaleca się wykonanie rozwijania za pomocą środowisku testowym, tworząc kopię środowisku produkcyjnym i go używać, aby sprawdzić, przepływu pracy awaryjnej aplikacji.
 
####<a name="outage-simulation"></a>Symulacja awarii

Symulacja awaria można usunąć lub zmienić nazwę źródłowej bazy danych. Spowoduje to błąd łączność aplikacji. 

####<a name="recovery"></a>Odzyskiwanie

- Pełnienie Geo przywracania bazy danych na innym serwerze opisany [poniżej](sql-database-disaster-recovery.md). 
- Zmień konfigurację aplikacji, aby nawiązać połączenie odzyskanego baz danych i prowadnicy [Konfigurowanie bazy danych po ich odzyskaniu](sql-database-disaster-recovery.md) , aby wykonać odzyskiwanie.

####<a name="validation"></a>Sprawdzanie poprawności

- Wykonywanie Drąż sprawdzając odzyskiwanie wpis integralności aplikacji (to znaczy parametry połączenia, logowania, podstawowe funkcje testy lub drugiej reguły sprawdzania poprawności procedur signoffs aplikacji standardowej).

##<a name="geo-replication"></a>Geo replikacji

Dla bazy danych, które są chronione za pomocą replikacji Geo wykonywania zwijania i rozwijania szczegółów obejmuje planowane przełączanie awaryjne do pomocniczego bazy danych. Tym planowane przełączeniu gwarantuje, że podstawowy i pomocniczy baz danych pozostają zsynchronizowane przy role. W przeciwieństwie do nieplanowanego przełączania awaryjnego tej operacji nie spowoduje utraty danych, więc Drąż mogą być wykonywane w środowisku produkcyjnym. 

####<a name="outage-simulation"></a>Symulacja awarii

Aby symulować awarii, możesz wyłączyć aplikację sieci web lub maszyn wirtualnych połączony z bazą danych. Spowoduje to błędów połączenia dla klientów w sieci web.

####<a name="recovery"></a>Odzyskiwanie

- Upewnij się, Konfiguracja aplikacji w regionie DR wskazuje byłego pomocniczego, które staną się pełni dostępny nowy podstawowy. 
- Wykonywanie [planowanych pracy awaryjnej](sql-database-geo-replication-powershell.md#initiate-a-planned-failover) nawiązać nowe podstawowa pomocniczego bazy danych
- Wykonaj przewodnik [konfiguracji bazy danych po odzyskiwania](sql-database-disaster-recovery.md) do wykonania odzyskiwania.

####<a name="validation"></a>Sprawdzanie poprawności

- Wykonywanie Drąż sprawdzając odzyskiwanie wpis integralności aplikacji (to znaczy parametry połączenia, logowania, podstawowe funkcje testy lub drugiej reguły sprawdzania poprawności procedur signoffs aplikacji standardowej).


## <a name="next-steps"></a>Następne kroki

- Aby uzyskać informacje o ciągłości scenariusze, zobacz [ciągłości scenariusze](sql-database-business-continuity.md)
- Aby uzyskać informacje o bazy danych SQL Azure automatycznego wykonywania kopii zapasowych, zobacz [Baza danych SQL automatycznego wykonywania kopii zapasowych](sql-database-automated-backups.md)
- Aby dowiedzieć się o używaniu kopie zapasowe automatycznego odzyskiwania, zobacz [Przywracanie bazy danych z kopii zapasowych zainicjowane usługi](sql-database-recovery-using-backups.md)
- Aby poznać opcje odzyskiwania szybciej, zobacz [Aktywne Geo replikacji](sql-database-geo-replication-overview.md)  
