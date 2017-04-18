<properties
   pageTitle="Zapewnianie ciągłości w chmurze — przywracanie usuniętych bazy danych — baza danych SQL | Microsoft Azure"
   description="Informacje na temat przywracania w chwili, umożliwiająca przywrócenie bazy danych SQL Azure do poprzedniego punktu w czasie (maksymalnie 35 dni)."
   services="sql-database"
   documentationCenter=""
   authors="stevestein"
   manager="jhubbard"
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/01/2016"
   ms.author="sstein"/>

# <a name="recover-an-azure-sql-database-using-automated-database-backups"></a>Odzyskiwanie bazy danych programu Azure SQL za pomocą kopie zapasowe automatycznego bazy danych

Baza danych SQL zawiera trzy opcje odzyskiwania bazy danych za pomocą [bazy danych SQL automatycznego wykonywania kopii zapasowych](sql-database-automated-backups.md). Przywracanie z bazy danych z kopii zapasowych inicjowanych przez usługę, w ich [okres przechowywania](sql-database-service-tiers.md) do:

- Nowej bazy danych na tym samym serwerze logiczne odzyskane do określonego punktu w czasie w okresie zachowywania. 
- Baza danych na tym samym serwerze logiczne odtworzyć podczas usuwania usunięte bazy danych.
- Nowej bazy danych na serwerze logicznych w dowolnym regionie odtworzyć ostatnio codzienne wykonywanie kopii zapasowych w magazynie obiektów blob replikowane geo (Pomoc Zdalna GRS).

Umożliwia także [bazy danych SQL automatycznego wykonywania kopii zapasowych](sql-database-automated-backups.md) utworzyć [Kopiowanie bazy danych](sql-database-copy.md) na serwerze logicznych w dowolnym regionie transakcyjnie zgodny z bieżącej bazy danych SQL. Możesz użyć kopii bazy danych i [Eksportowanie PLECAK](sql-database-export.md) archiwizowania transakcyjnie spójne kopii bazy danych w celu długotrwałego przechowywania poza usługi okres przechowywania lub przesyłanie lokalnej kopii bazy danych lub maszyn wirtualnych Azure wystąpienie programu SQL Server.

## <a name="recovery-time"></a>Czasu

Przywracanie bazy danych przy użyciu kopie zapasowe automatycznego bazy danych w czasie odzyskiwania wpływa wiele czynników: 
 - Rozmiar bazy danych
 - Poziom wydajności bazy danych
 - Liczba dzienników transakcji związane
 - Ilość aktywności, który ma być odtwarzane do odzyskania punktu przywracania
 - W przypadku przywracania do innego obszaru przepustowość sieci 
 - Liczba żądań równoczesne Przywróć przetwarzanych w regionie docelowej. 
 
 Bardzo duże i/lub aktywnej bazy danych przywracania może potrwać kilka godzin. Jeśli istnieje długotrwałego awarii w regionie, jest możliwe, że będzie dużej liczby żądań Przywróć Geo przetwarzanych przez pozostałe regiony. W przypadku dużej liczby żądań może to zwiększyć czas odzyskiwania dla baz danych w danym regionie. Większość bazy danych przywraca wykonane w ciągu 12 godzin.

 Istnieje żadne wbudowane funkcje, aby zbiorczo przywracania. [Bazy danych SQL Azure: pełny odzyskiwania serwera](https://gallery.technet.microsoft.com/Azure-SQL-Database-Full-82941666) skrypt jest przykładem jednym ze sposobów wykonasz to zadanie.

> [AZURE.IMPORTANT] Aby odzyskać przy użyciu kopie zapasowe automatycznego, musi należeć do roli współautora serwera SQL w subskrypcji lub być właścicielem subskrypcji. Można odzyskać przy użyciu portalu Azure programu PowerShell lub interfejsu API usługi REST. Nie można używać języku Transact-SQL. 

## <a name="point-in-time-restore"></a>Przywracanie w chwili

W chwili przywrócić umożliwia przywracanie istniejącej bazy danych jako nowej bazy danych do wcześniejszego punktu w czasie na tym samym serwerze logiczne za pomocą [bazy danych SQL automatycznego wykonywania kopii zapasowych](sql-database-automated-backups.md). Nie można zastąpić istniejącej bazy danych. Można przywrócić do wcześniejszego punktu w czasie przy użyciu [Azure portal](sql-database-point-in-time-restore-portal.md), [programu PowerShell](sql-database-point-in-time-restore-powershell.md) lub [Interfejsu API usługi REST](https://msdn.microsoft.com/library/azure/mt163685.aspx).

> [AZURE.SELECTOR]
- [W chwili przywracania: Portal Azure](sql-database-point-in-time-restore-portal.md)
- [Przywracanie w chwili: programu PowerShell](sql-database-point-in-time-restore-powershell.md)

Bazy danych można przywrócić w dowolnej poziom wydajności lub elastyczne puli. Należy upewnić się, że masz wystarczających przydziałów DTU na serwerze logiczne lub elastycznych puli. Należy pamiętać przywracania umożliwia utworzenie nowej bazy danych, a poziom warstwa i wydajności usługi przywrócenie bazy danych mogą być inne niż bieżący stan live bazy danych. Po zakończeniu przywrócenie bazy danych jest normalny pełni dostępny online bazy danych rozliczana według stawki normalny na podstawie poziomu warstwa i wydajności usługi. Nie spowodować opłaty, aż do przywracania bazy danych.

Zazwyczaj Przywracanie bazy danych z punktem earler na potrzeby odzyskiwania. Gdy ten sposób można Traktuj przywrócenie bazy danych jako zamiennik oryginalnej bazy danych lub go używać do pobierania danych z, a następnie zaktualizuj oryginalnej bazy danych. 

- ***Bazy danych, który chcesz zamienić wyszukiwany:*** Jeśli przywrócenie bazy danych jest przeznaczona jako zamiennik oryginalnej bazy danych, należy sprawdzić poziom wydajności i/lub warstwa usług są odpowiednie i skalowanie bazy danych w razie potrzeby. Można zmienić nazwy oryginalnej bazy danych, a następnie nadaj przywrócenie bazy danych oryginalną nazwą za pomocą polecenia ZMIEŃ bazę danych w T-SQL. 
- ***Odzyskiwanie danych:*** Jeśli planujesz do pobierania danych z przywrócenie bazy danych do odzyskania z błędem użytkownik lub aplikacja będzie oddzielnie konieczne zapis i wykonywanie skryptów odzyskiwania niezależnie od danych, należy wyodrębnić dane z przywrócenie bazy danych do oryginalnej bazy danych. Mimo że operacja przywracania może zająć sporo czasu, przywracanie bazy danych będą widoczne na liście baz danych w całej. Po usunięciu bazy danych podczas przywracania spowoduje anulowanie operacji, a nie zostanie naliczona dla bazy danych, która nie została ukończona przywracania. 

Aby uzyskać szczegółowe informacje dotyczące odzyskiwanie z błędy aplikacji i użytkowników za pomocą przywracania w chwili, zobacz Przywracanie [punktu w czasie](sql-database-recovery-using-backups.md#point-in-time-restore)

## <a name="deleted-database-restore"></a>Przywracanie usuniętego bazy danych

Przywracanie usuniętego bazy danych umożliwia przywracanie usuniętych bazy danych do czasu usunięcia usunięte bazy danych na tym samym serwerze logiczne za pomocą [bazy danych SQL automatycznego wykonywania kopii zapasowych](sql-database-automated-backups.md). 

> [AZURE.IMPORTANT] Po usunięciu wystąpienie serwera bazy danych SQL Azure usuwane są również wszystkie swoje bazy danych i nie można odzyskać. Istnieje Przywracanie usuniętego serwera w tej chwili nie są obsługiwane.

Za pomocą tej samej lub nazwę nowej bazy danych dla przywrócenie bazy danych. Można użyć [Azure portal](sql-database-restore-deleted-database-portal.md), [programu PowerShell](sql-database-restore-deleted-database-powershell.md) lub [pozostałych (createMode = Przywróć)](https://msdn.microsoft.com/library/azure/mt163685.aspx). 

> [AZURE.SELECTOR]
- [Przywracanie bazy danych usunięte: Azure portal](sql-database-restore-deleted-database-portal.md)
- [Przywracanie bazy danych usunięte: programu PowerShell](sql-database-restore-deleted-database-powershell.md)

## <a name="geo-restore"></a>Przywracanie Geo

Przywracanie Geo umożliwia przywracanie bazy danych SQL na serwerze w dowolnym regionie Azure z ostatnich replikowane geo [automatyczną codziennego wykonywania kopii zapasowej](sql-database-automated-backups.md). Przywracanie Geo zbędne geo kopii zapasowej została użyta jako źródło i można odzyskać bazy danych, nawet jeśli bazy danych lub centrum danych jest niedostępne z powodu awarii. Można użyć [Azure portal](sql-database-geo-restore-portal.md), [programu PowerShell](sql-database-geo-restore-powershell.md)lub [pozostałych (createMode = odzyskiwanie)](https://msdn.microsoft.com/library/azure/mt163685.aspx) 

> [AZURE.SELECTOR]
- [Przywracanie Geo: Portal Azure](sql-database-geo-restore-portal.md)
- [Przywracanie Geo: programu PowerShell](sql-database-geo-restore-powershell.md)

Przywracanie Geo jest domyślna opcja odzyskiwania, gdy bazy danych jest niedostępny z powodu zdarzenia w regionie, gdzie jest hostowana bazy danych. Jeśli przypadek dużą skalę w regionie skutkuje niedostępności aplikacji bazy danych umożliwia przywracanie Geo Przywracanie bazy danych z najnowszą kopię zapasową na serwerze w innym regionie. Wszystkie kopie zapasowe są replikowane geo i nie może mieć opóźnienia między, gdy kopia zapasowa jest podjętych i replikować geo Azure obiektów blob w innym regionie. To opóźnienie może być aż do godziny, w przypadku awarii mogą występować w górę do 1 godzina utracie danych, to znaczy RPO maksymalnie 1 godzina. Poniżej przedstawiono przywracania bazy danych z ostatniego codziennego wykonywania kopii zapasowej.

![Przywracanie Geo](./media/sql-database-geo-restore/geo-restore-2.png)

Aby uzyskać szczegółowe informacje dotyczące odzyskiwanie z awarii przy użyciu Geo przywracania zobacz [Odzyskiwanie z awarii](sql-database-disaster-recovery.md)

> [AZURE.IMPORTANT] Przywracanie Geo są dostępne z wszystkich poziomów usługi, jest najbardziej podstawowe rozwiązań odzyskiwania danych dostępne w bazie danych SQL z najdłuższej RPO i oszacowanie czasu odzyskiwania (Wstaw). Podstawowe baz danych z maksymalnego rozmiaru 2 GB Geo-Przywracanie rozwiązaniem jest rozsądne DR z Wstaw 12 godzin. Większe Standard lub Premium baz danych jeśli znacznie krótsze czasy odzyskiwania są konieczne lub w celu zmniejszenia prawdopodobieństwa utracie danych należy rozważyć użycie aktywnej replikacji Geo. Replikacja Geo Active oferuje wiele niższe RPO i Wstaw jako wystarczy zainicjować awarię na stale zreplikowanej pomocniczym. Aby uzyskać szczegółowe informacje zobacz [Replikacja Geo Active](sql-database-geo-replication-overview.md).

## <a name="programmatically-performing-recovery-using-automated-backups"></a>Wykonywanie programowo przy użyciu kopie zapasowe automatycznego odzyskiwania

Opisane powyżej, w addiition do portalu Azure odzyskiwanie bazy danych mogą być wykonywane przy użyciu programu PowerShell Azure i interfejsu API usługi REST. W poniższej tabeli opisano zestaw poleceń dostępnych.

### <a name="powershell"></a>Programu PowerShell

|Polecenie cmdlet|Opis|
|------|-----------|
|[Get-AzureRmSqlDatabase](https://msdn.microsoft.com/en-us/library/azure/mt603648.aspx)|Otrzymuje jeden lub więcej baz danych.|
|[Get-AzureRMSqlDeletedDatabaseBackup](https://msdn.microsoft.com/en-us/library/azure/mt693387.aspx)|Otrzymuje usunięte bazę danych, którą można przywrócić.|
|[Get-AzureRmSqlDatabaseGeoBackup](https://msdn.microsoft.com/library/azure/mt693388.aspx)|Otrzymuje zbędne geo kopii zapasowej bazy danych.|
|[Przywracanie AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt693390.aspx)|Przywraca z bazą danych SQL.|
||||

### <a name="rest-api"></a>INTERFEJSU API USŁUGI REST

|INTERFEJS API|Opis|
|---|-----------|
|[POZOSTAŁE (createMode = odzyskiwanie)](https://msdn.microsoft.com/library/azure/mt163685.aspx)|Przywraca bazę danych|
|[Uzyskiwanie Tworzenie lub aktualizowanie stan bazy danych](https://msdn.microsoft.com/library/azure/mt643934.aspx)|Zwraca stan podczas operacji przywracania|
||||



## <a name="summary"></a>Podsumowanie

Automatycznych kopii zapasowych chronić baz danych z użytkowników i błędów aplikacji, usunięcie przypadkowe bazy danych i długotrwałego dostawie. To rozwiązanie administratorami zero koszt zero jest dostępne w przypadku wszystkich baz danych programu SQL. 

## <a name="next-steps"></a>Następne kroki

- Przegląd ciągłości działalności i scenariuszy zobacz [Omówienie ciągłości firm](sql-database-business-continuity.md)
- Aby uzyskać informacje o bazy danych SQL Azure automatycznego wykonywania kopii zapasowych, zobacz [Baza danych SQL automatycznego wykonywania kopii zapasowych](sql-database-automated-backups.md)
- Aby poznać opcje odzyskiwania szybciej, zobacz [Aktywne Geo replikacji](sql-database-geo-replication-overview.md)  
- Aby dowiedzieć się o używaniu kopie zapasowe automatycznego archiwizowania, zobacz [Kopiowanie bazy danych](sql-database-copy.md)
