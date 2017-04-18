<properties
    pageTitle="Planowanie uaktualnienia do bazy danych SQL w wersji 12 | Microsoft Azure"
    description="W tym artykule opisano przygotowań oraz ograniczenia związane podczas uaktualniania do wersji w wersji 12 z bazy danych SQL Azure."
    services="sql-database"
    documentationCenter=""
    authors="MightyPen"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="genemi"/>


# <a name="plan-and-prepare-to-upgrade-to-sql-database-v12"></a>Planowanie i przygotowywanie przeprowadzić uaktualnienie do wersji 12 bazy danych SQL


W tym temacie opisano planowanie i przygotowania należy wykonać, aby uaktualnić baz danych Azure SQL z wersji V11 do wersji 12.


Nowy [Azure Portal](https://portal.azure.com/) jest dostępna do obsługi uaktualnienia do wersji 12.


W poniższej tabeli przedstawiono inne tematy pomocy dla wersji 12.


| Tytuł i łącza | Opis zawartości |
| :--- | :--- |
| [Co nowego w wersji 12 bazy danych SQL](sql-database-v12-whats-new.md) | W tym artykule opisano, jak w wersji 12 przesunąć bazy danych SQL Azure bliżej pełnej zgodności z programu Microsoft SQL Server szczegółów. |
| [Tworzenie bazy danych w bazie danych SQL w wersji 12](sql-database-get-started.md) | W tym artykule opisano, jak utworzyć nową bazę danych Azure SQL w wersji wersji 12. Opisuje różne opcje poza tylko pustą bazę danych. |


## <a name="plan-ahead"></a>Planowanie


Podane niżej podsekcje opisują szkoleń i podejmowania decyzji, które musisz zająć się zająć akcje kierunku uaktualniania bazy danych Azure SQL do wersji 12.

### <a name="version-clarification"></a>Wyjaśnienie wersję


Tego dokumentu dotyczy uaktualniania pakietu Microsoft Azure SQL Database wersji V11 do wersji 12. Bardziej formalnego numery wersji są zbliżony dwóch wartości, według instrukcji Transact-SQL **Wybierz @@version; ** :


- 12.0.2000.8 *(lub bit nowszej wersji 12)*
- 11.0.9228.18 *(V11)*
 - V11 została także nazywane SAWA w wersji 2.

### <a name="service-tier-planning"></a>Usługa planowania warstwy


Począwszy od wersji 12, bazy danych SQL Azure będzie obsługuje tylko warstwy usługi o nazwie Basic, Standard i Premium. Warstwa usług sieci Web i małych firm nie jest obsługiwana w wersji 12. W związku z tym jeśli planujesz uaktualnić z bazą danych Azure SQL obecnie znajduje się warstwa usług sieci Web i firm, należy zdecydować, co należy jego nowego poziomu usług.


Aby uzyskać szczegółowe informacje dotyczące poziomów usługi podstawowe, Standard i Premium zobacz:

- [Warstwy usługi bazy danych SQL](sql-database-service-tiers.md)
- [Uaktualnianie baz danych sieci Web i firm bazy danych SQL do nowej warstwy usług](sql-database-upgrade-server-portal.md)



### <a name="review-the-geo-replication-configuration"></a>Przeglądanie konfiguracji Geo replikacji


Bazy danych Azure SQL skonfigurowano Geo replikacji, należy jego bieżącą konfigurację i zatrzymywanie Geo replikacji przed rozpoczęciem akcje przygotowanie do uaktualnienia. Po zakończeniu uaktualniania Geo replikacji należy zmienić konfigurację bazy danych.


Strategii należy pozostawić bez zmian źródła i przetestować na kopii bazy danych.


## <a name="preparation-actions"></a>Przygotowanie akcje


Po zakończeniu planowania można wykonywać czynności opisanych w podane niżej podsekcje, aby przygotować się do ostatniej fazy uaktualnienia.


Szczegółowe opisy ostatniej fazy uaktualnienia są dostępne w tematach pomocy, które są połączone z górnej części tego tematu Pomocy.


### <a name="service-tier-actions"></a>Akcje poziomu usług


Usługa sieci Web i małych firm ceny warstwa nie jest obsługiwana w wersji 12.


Jeśli bazy danych V11 Azure SQL jest baza danych sieci Web i firm, proces uaktualniania oferuje przełączania bazy danych do poziomu obsługiwane. Uaktualnianie zaleca warstwa pasującą Historia obciążenie pracą bazy danych. Jednak można wybrać dowolny obsługiwanych warstwy, którą chcesz.


Kroki niezbędne podczas uaktualniania można zmniejszyć, przełączając V11 bazy danych od poziomu sieci Web i firm przed rozpoczęciem uaktualnienia. Możesz to zrobić przy użyciu nowego [Azure Portal](https://portal.azure.com/).


Jeśli wiadomo, które warstwa usług, aby przełączyć się do, S2 stopień warstwie standardowy może być za pośrednictwem początkowy wybór. Wszelkie niższego poziomu ma mniej zasobów niż warstwie sieci Web i małych firm sprzedał.


### <a name="suspend-geo-replication-during-upgrade"></a>Zawieszenia replikacji Geo podczas uaktualniania


Uaktualnianie do wersji 12 nie można uruchomić, jeśli Geo replikacji jest aktywny w bazie danych. Najpierw skonfiguruj bazę danych, aby zakończyć korzystanie Geo replikacji.


Po zakończeniu uaktualniania można skonfigurować bazę danych, aby ponownie użyć Geo replikacji.


### <a name="open-ports-on-an-azure-vm-for-client-connectivity"></a>Otwarte porty na maszyn wirtualnych Azure dla łączność z klientami


Jeśli używany program klient nawiązuje połączenie bazy danych SQL w wersji 12 Klient uruchomiony na Azure maszyn wirtualnych (maszyn wirtualnych), należy otworzyć z następujących zakresów portów na maszyn wirtualnych:

- 11000 11999
- 14000 14999


Kliknij [tutaj](sql-database-develop-direct-route-ports-adonet-v12.md) szczegółowe informacje na temat porty dla wersji 12 bazy danych SQL. Porty są wymagane przez ulepszenia wydajności w wersji 12 bazy danych SQL.


##<a id="limitations"></a>Ograniczenia w czasie i po uaktualnieniu do wersji 12


### <a name="portals-for-v12"></a>Portali dla wersji 12


Istnieją trzy portali dla Azure i każda ma różne możliwości dotyczące wersji 12 bazy danych SQL.


- [http://Portal.Azure.com/](https://portal.azure.com/)<br/>Ten Portal Azure nowego a jest nadal w stanie Podgląd. Ten portal nie jest jeszcze na pełny (GA, General Availability). Ten portal:
 - Można zarządzać w wersji 12 serwera i bazy danych.
 - Można uaktualnić V11 bazy danych do wersji 12.


- [http://Manage.windowsazure.com/](http://manage.windowsazure.com/)<br/>Ten klasyczny Portal Azure mogą ostatecznie wycofane. Ten portal:
 - Można zarządzać w wersji 12 serwera i bazy danych.
 - Mogą *nie* uaktualnienia usługi V11 bazy danych do wersji 12.


- (http://*yourservername*. database.windows.net)<br/>Portal klasyczny bazy danych Azure SQL:
 - Czy*nie* zarządzać serwerami wersji 12.


Zachęcamy do nawiązywanie połączenia z baz danych Azure SQL z programu Visual Studio 2015 (VS2015). VS2015 może być używany do następujących zadań:


- Aby uruchomić instrukcji Transact-SQL.
- Aby zaprojektować schemat.
- Aby utworzyć bazę danych, trybu online lub offline.


Lub zamiast tego bezpłatnie możesz pobrać [Visual Studio społeczności 2015](https://www.visualstudio.com/vs/community/), który jest w pełni funkcjonalnej wersji VS2015.


W starszych Azure klasyczny portalu na stronie bazy danych możesz kliknąć **Otwórz w programie Visual Studio** Uruchom VS2015 na komputerze połączenie z bazą danych SQL Azure.


Dla alternatywnego umożliwia SQL Server Management Studio (SSMS) 2014 z [CU6](http://support.microsoft.com/kb/3031047/) nawiązywanie połączenia z bazą danych SQL Azure. Szczegółowe informacje znajdują się na ten wpis w blogu:<br/>[Aktualizacje z bazy danych SQL Azure narzędzie klienta](https://azure.microsoft.com/blog/2014/12/22/client-tooling-updates-for-azure-sql-database/).


> [AZURE.IMPORTANT] Zalecane jest zawsze używać najnowszej wersji programu Management Studio do pozostawać aktualizacje Microsoft Azure i baza danych SQL. [Aktualizowanie programu SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).


### <a name="limitation-during-upgrade-to-v12"></a>Ograniczenie *podczas* uaktualniania do wersji 12


Baza danych V11 pozostaje dostępny uzyskać dostęp do danych podczas uaktualniania do wersji 12. Istnieje jeszcze kilka ograniczeń brać pod uwagę.


| Ograniczenia | Opis |
| :--- | :--- |
| Czas trwania uaktualnienia | Czas trwania uaktualnienia zależy od rozmiaru, edition i liczba baz danych na serwerze. Proces uaktualniania może zostać uruchomiony dla godzin do dni dla serwerów, szczególnie dla serwerów, które ma baz danych:<br/><br/>*Większych niż 50 GB lub<br/> * Na warstwie usług innych niż premium<br/><br/>Tworzenie nowych baz danych na serwerze podczas uaktualniania można również zwiększyć uaktualnienia czas trwania. |
| Replikacja nie Geo | Geo replikacja nie jest obsługiwane na serwerze w wersji 12 jest obecnie biorących udział w uaktualnienie V11. |
| Baza danych jest przez chwilę niedostępny w ostatniej fazy uaktualnienia do wersji 12 | Bazy danych, należącą do swojego serwera V11 pozostają dostępne podczas procesu uaktualniania. Jednak połączenia z serwerem i baz danych jest przez chwilę niedostępny w ostatniej fazy, podczas pracy z wersją na zaczyna się od V11 do wersji 12 jest już gotowy.<br/><br/>Przełączanie w okresie zakresu od 40 sekund do 5 minut. W przypadku większości serwerów Przełącz na jest planowane do wykonania w ciągu 90 sekund. Przełączanie z czasem zwiększa dla serwerów o dużej liczby baz danych, lub gdy baz danych używających obciążenia intensywnie zapisu. |


### <a name="limitation-after-upgrade-to-v12"></a>Ograniczenie *po* uaktualnieniu do wersji 12


| Ograniczenia | Opis |
| :--- | :--- |
| Nie można przywrócić V11 | Po uaktualnienia w miejscu wynik nie można cofnąć lub cofnąć. |
| Warstwa sieci Web lub firm | Po uaktualnienia rozpoczyna się z serwerem dla nowej bazy danych w wersji 12 można już nie rozpoznaje lub zaakceptować warstwa usług sieci Web i firm. |



### <a name="export-and-import-after-upgrade-to-v12"></a>Eksportowanie i importowanie *po* uaktualnieniu do wersji 12


Można eksportować lub importowanie bazy danych w wersji 12 przy użyciu [Azure Portal](https://portal.azure.com/). Lub można eksportować lub importować za pomocą dowolnej z następujących narzędzi:


- SQL Server Management Studio (SSMS)
- Visual Studio 2015 r.
- AIF warstwy danych (DacFx)


Jednak za pomocą narzędzi, trzeba najpierw mieć lub instalowanie ich najnowsze aktualizacje, aby upewnić się, że obsługują nowe funkcje w wersji 12:


- [Zbiorcza aktualizacja 6 programu SQL Server Management Studio 2014 r.](http://support2.microsoft.com/kb/3031047)
- [Aktualizacja luty 2015 dla bazy danych SQL Server narzędzie w Visual Studio 2013](https://msdn.microsoft.com/data/hh297027)
- [Warstwy danych luty 2015 aplikacji Framework (DacFx) dla wersji 12 bazy danych Azure SQL](http://www.microsoft.com/download/details.aspx?id=45886)


> [AZURE.NOTE] Poprzedni łączy do narzędzi zostały zaktualizowane lub później 2 marca 2015 r. Firma Microsoft zaleca używanie tych nowsze aktualizacje te narzędzia.


#### <a name="automated-export"></a>Automatyczne eksportowanie


Automatyczne eksportowanie nadal będzie dostępna jako podgląd.  Podczas korzystania z automatycznego eksportu, są wymagane żadne aktualizacje narzędzia do klienta.  Jeśli chcesz wykonać utworzony plik i zaimportować do serwera lokalnego, aktualizacje narzędzia wymienione powyżej są wymagane do pomyślnie zaimportować.


### <a name="restore-to-v12-of-a-deleted-v11-database"></a>Przywracanie usuniętego V11 bazy danych w wersji 12

W poniższym scenariuszu wyjaśniono, że usunięte bazą danych V11 Azure SQL mogą być przywracane na serwerze bazy danych SQL Azure w wersji 12.

1. Załóżmy, że serwer bazy danych SQL Azure V11. <br/> Na serwerze bazy danych dysponować przestarzałych warstwa usług sieci Web i firm.
2. Usuwanie bazy danych.
3. Uaktualnij serwer do wersji 12.
4. Następnie przywróć bazę danych na serwerze. <br/> Baza danych związku z tym jest uaktualniany do wersji 12 na poziomie S0 warstwie standardowa usługa.
5. Jeśli S0 nie jest preferencji, możesz przełączać bazy danych do dowolnego poziomu obsługiwanej usługi.


### <a name="powershell-cmdlets"></a>Polecenia cmdlet programu PowerShell


Polecenia cmdlet programu PowerShell są dostępne uruchomić, zatrzymać lub monitorowanie uaktualnienie do wersji 12 bazy danych SQL Azure z V11 lub dowolną inną wersją wersji sprzed 12.

- [Uaktualnianie do wersji 12 bazy danych SQL przy użyciu programu PowerShell](sql-database-upgrade-server-powershell.md)

Aby dokumentacji o tych poleceń cmdlet zobacz:


- [Get-AzureRMSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt603582.aspx)
- [Rozpocznij AzureRMSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt619403.aspx)
- [Zatrzymaj AzureRMSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt603589.aspx)


Polecenia cmdlet Zatrzymaj oznacza, że anulować, nie Zatrzymaj wskaźnik myszy. Istnieje sposobem wznów uaktualnianie innych niż ponownie uruchomić od początku. Polecenia cmdlet Stop wyczyści i udostępnia wszystkie odpowiednie zasoby.


## <a name="failure-resolution"></a>Błąd rozdzielczości


Jeśli uaktualnienie nie powiedzie się z dowolnego powodu na stronach parzystych, V11 bazy danych pozostaje aktywny i dostępny w zwykły sposób.


## <a name="related-links"></a>Łącza pokrewne


- Microsoft Azure [Funkcje Preview](https://azure.microsoft.com/services/preview/)


<!--Anchors-->
[Subheading 1]: #subheading-1
