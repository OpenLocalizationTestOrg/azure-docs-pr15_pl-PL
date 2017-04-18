<properties
   pageTitle="Zapewnianie ciągłości w chmurze — bazy danych odzyskiwania — baza danych SQL | Microsoft Azure"
   description="Dowiedz się, jak bazy danych SQL Azure obsługuje chmury ciągłości i odzyskiwanie bazy danych i pomaga zachować uruchomione aplikacje w chmurze krytyczne."
   keywords="Zapewnianie ciągłości, chmury ciągłości, odzyskiwanie bazy danych, odzyskiwanie bazy danych"
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
   ms.workload="NA"
   ms.date="10/13/2016"
   ms.author="carlrab"/>

# <a name="overview-of-business-continuity-with-azure-sql-database"></a>Omówienie ciągłości z bazy danych SQL Azure

To omówienie opisano możliwości, jakie zapewnia bazy danych SQL Azure ciągłości i awarii. Udostępnia opcje, zalecenia i samouczki dotyczące odzyskanie kłopotliwe środki zdarzenia, które mogą spowodować utratę danych lub powodować bazy danych i aplikacji stają się niedostępne. Zawiera dyskusji, co zrobić, gdy użytkownik integralności danych wpływa błąd aplikacji, Azure region zawiera awarii lub aplikacja wymaga konserwacji. 

## <a name="sql-database-features-that-you-can-use-to-provide-business-continuity"></a>Funkcje bazy danych SQL, które umożliwia podanie ciągłości

Baza danych SQL udostępnia kilka funkcji ciągłości firm, w tym kopie zapasowe automatycznego i replikacja bazy danych opcjonalne. Każdy innych cechach odzyskiwania szacowany czas (Wstaw) i potencjalną utratą danych ostatnich transakcji. Po opis tych opcji, możesz wybrać jeden z nich — i, w większości przypadków używane razem w różnych scenariuszach. Podczas opracowywania planu ciągłości firm, należy zrozumieć maksymalny dopuszczalne czas oczekiwania aplikacji pełni odzyskuje po zdarzeniu kłopotliwe środki — jest to celem czasu do odzyskiwania (RTO). Trzeba również zrozumienie maksymalnej ilości najnowsze aktualizacje danych (interwał) przeszkadzają aplikacji utraty po odtworzeniu po zdarzeniu kłopotliwe środki - wskaźniku punktu odzyskiwania (RPO). 

W poniższej tabeli porównano Wstaw i RPO dla trzech najbardziej typowych scenariuszy.

| Funkcja |  Podstawowe warstwy | Standardowy warstwy  | Poziom Premium |
|---|---|---|---|
| Wskaż w czasu Przywracanie z kopii zapasowej | Dowolny punkt, w ciągu 7 dni   | Dowolny punkt ciągu 35 dni  | Dowolny punkt ciągu 35 dni |
Geo-Przywracanie z kopii zapasowych replikowane geo | Wstaw < 12 godzin, RPO < 1h   | Wstaw < 12 godzin, RPO < 1h   | Wstaw < 12 godzin, RPO < 1h |
|Replikacja Geo Active | Wstaw < 30s, RPO < 5s   | Wstaw < 30s, RPO < 5s | Wstaw < 30s, RPO < 5s |


### <a name="use-database-backups-to-recover-a-database"></a>Odzyskiwanie bazy danych za pomocą kopie zapasowe bazy danych

Baza danych SQL wykonuje automatycznie kombinacji pełnej bazy danych kopii zapasowych co tydzień, różnicy kopie zapasowe co godzinę i kopie zapasowe dziennika transakcji co pięć minut bazy danych do ochrony przed utratą danych w firmie. Te kopie zapasowe są przechowywane w magazynie lokalnie zbędne 35 dni dla baz danych w warstwy usługi Standard i Premium i siedem dni dla baz danych w warstwie podstawowej usługi — zobacz [warstwy usługi](sql-database-service-tiers.md) , aby uzyskać więcej informacji na warstwy usługi. Jeśli okres przechowywania z poziomu usługi nie spełnia wymagań dotyczących firm, można zwiększyć okres przechowywania, [zmieniając warstwa usług](sql-database-scale-up.md). Pełny i zróżnicowany bazy danych kopii zapasowych są replikowane [Centrum danych iloczynów](../best-practices-availability-paired-regions.md) ochrony przed awaria centrum danych — zobacz [automatycznych kopii zapasowych](sql-database-automated-backups.md) , aby uzyskać więcej informacji.

Za pomocą tych automatycznych kopii zapasowych umożliwiająca przywrócenie bazy danych z różnych zdarzeń kłopotliwe środki, zarówno w centrum danych, jak i do innego centrum danych. Szacowany czas odzyskiwania przy użyciu automatycznych kopii zapasowych, zależy od kilku czynników, takich jak liczba baz danych Odzyskiwanie w tym samym regionie, w tym samym czasie, rozmiar bazy danych, rozmiar dziennika transakcji i przepustowość sieci. W większości przypadków czas odzyskiwania jest mniej niż 12 godzin. Jeśli odzyskiwanie do innego obszaru danych, utracie danych jest ograniczona do 1 godzina zbędne geo magazyn godzinowe kopii zapasowych różnicy bazy danych. 

> [AZURE.IMPORTANT] Aby odzyskać przy użyciu kopie zapasowe automatycznego, użytkownik musi należeć do roli współautora serwera SQL lub właścicielem subskrypcji — zobacz [RBAC: wbudowane role](../active-directory/role-based-access-built-in-roles.md). Można odzyskać przy użyciu Azure portal, programu PowerShell lub interfejsu API usługi REST. Nie można używać języku Transact-SQL.

Użyj kopie zapasowe automatycznego jako mechanizm ciągłości i odzyskiwania firm, jeśli aplikacji:

- Nie są uwzględniane misji krytyczne.
- Nie ma SLA powiązanie, dlatego przestoje 24 godziny lub dłużej nie spowoduje odpowiedzialności finansowej.
- Ma niski stopień zmiany danych (niski transakcje / h) i utraty do godziny zmiany jest utracie danych. 
- Koszt jest wielkość liter. 

Szybsze odzyskiwania, należy użyć [Aktywnej replikacji Geo](sql-database-geo-replication-overview.md) (omówione dalej). Musisz mieć możliwość odzyskiwanie danych z kropką starsze niż 35 dni, warto rozważyć archiwizacji regularnie do pliku PLECAK bazy danych (plik skompresowany zawierający schemat bazy danych i skojarzonych z nim danych usługi) przechowywane w magazynie obiektów blob platformy Azure lub w innej lokalizacji. Aby uzyskać więcej informacji na temat tworzenia archiwum transakcyjnie spójne bazy danych zobacz [Tworzenie kopii bazy danych](sql-database-copy.md) i [Eksportowanie kopii bazy danych](sql-database-export.md). 

### <a name="use-active-geo-replication-to-reduce-recovery-time-and-limit-data-loss-associated-with-a-recovery"></a>Zmniejszanie czasu i ograniczanie utraty danych odzyskiwania za pomocą aktywnej replikacji Geo

Oprócz korzystania kopie zapasowe bazy danych dla odzyskiwanie bazy danych w przypadku przerwania firm, [Aktywnej replikacji Geo](sql-database-geo-replication-overview.md) umożliwia konfigurowanie bazy danych ma maksymalnie cztery czytelne pomocniczej bazy danych w regionach wybranych przez użytkownika. Te pomocniczej bazy danych są zachowywane zsynchronizowane z podstawową bazą danych przy użyciu mechanizmu replikacji asynchroniczne. Ta funkcja służy do ochrony przed firm zakłócenia w przypadku awarii centrum danych lub podczas uaktualniania aplikacji. Aby zapewnić lepszą wydajność kwerendy dla kwerendami tylko do odczytu dla użytkowników geograficznie można także aktywnej replikacji Geo.

Jeśli podstawową bazą danych przechodzi do trybu offline nieoczekiwanie lub należy przejść do trybu offline dla działania związane z obsługą, można szybko awansować pomocniczym podstawowych (zwanych również trybie awaryjnym) i skonfigurować aplikacje nawiązywania połączenia z nowo utworzonego podstawowa. Planowane trybie awaryjnym jest bez utraty danych. Z nieplanowanego przełączania awaryjnego może być niektórych niewielkiej ilości utratą danych bardzo ostatnich transakcji ze względu na charakter asynchroniczne replikacji. Po przełączeniu możesz później awarii - zgodnie z planem lub gdy centrum danych przechodzi do trybu online. We wszystkich przypadkach użytkownicy wystąpić niewielki przestoje i musisz ponownie nawiązać połączenie. 

> [AZURE.IMPORTANT] Aby użyć aktywnego replikacji Geo, możesz być właścicielem subskrypcji lub mieć uprawnienia administracyjne w programie SQL Server. Można skonfigurować i pracy awaryjnej przy użyciu Azure portal, programu PowerShell lub interfejsu API usługi REST przy użyciu uprawnień dla tej subskrypcji lub języku Transact-SQL za pomocą uprawnień w ramach programu SQL Server.

Replikacja Geo Active za pomocą aplikacji, która spełnia dowolne z poniższych kryteriów:

- Niezwykle ważne jest misji.
- Zawiera Umowa dotycząca poziomu usług (SLA), który nie zezwala na najmniej 24 godziny przestoje.
- Przestoje spowoduje odpowiedzialności finansowej.
- Ma duże wartości danych zmiana jest duży i utraty godziny danych nie jest akceptowane.
- Dodatkowe koszty aktywnej replikacji geo jest mniejsza niż potencjalne odpowiedzialności finansowej i skojarzone utraty firm.

## <a name="recover-a-database-after-a-user-or-application-error"></a>Odzyskiwanie bazy danych po błędu użytkownika lub aplikacji

* Nikt nie będzie doskonale! Użytkownik może przypadkowego usunięcia danych, przypadkowo usunąć tabelę ważne lub nawet upuść całej bazy danych. Lub aplikacji mogą przypadkowo zastąpić danych nieprawidłowe dane z powodu wady aplikacji. 

W tym scenariuszu są opcje odzyskiwania.

### <a name="perform-a-point-in-time-restore"></a>Wykonywanie przywracania w chwili

Umożliwia automatyczne kopie zapasowe odzyskiwanie kopii bazy danych do punktu dobre znane w czasie, pod warunkiem, że czas znajduje się w obrębie okres przechowywania bazy danych. Po przywróceniu bazy danych, możesz zastąpić oryginalnej bazy danych przywrócenie bazy danych lub skopiuj dane potrzebne do oryginalnej bazy danych z przywracane dane. Jeśli bazy danych korzysta z aktywnej replikacji Geo, zalecamy kopiowany wymagane dane przywrócona kopia do oryginalnej bazy danych. Przywrócenie bazy danych możesz zastąpić oryginalnej bazy danych, konieczne będzie skonfigurowanie i ponownie zsynchronizować aktywnej replikacji Geo (który może wymagać czasu dużej bazy danych). 

Aby uzyskać więcej informacji i szczegółowe informacje na temat przywracania bazy danych do punktu w czasie za pomocą portalu Azure lub przy użyciu programu PowerShell, zobacz [Przywracanie w chwili](sql-database-recovery-using-backups.md#point-in-time-restore). Nie można odzyskać przy użyciu języka Transact-SQL.

### <a name="restore-a-deleted-database"></a>Przywracanie usuniętego bazy danych

Jeśli baza danych zostanie usunięty, ale logiczne serwera nie została usunięta, można przywrócić usunięty bazy danych do punktu, w którym został usunięty. Przywrócenie kopii zapasowej bazy danych do tego samego logiczne serwera SQL z której został usunięty. Można przywrócić pierwotną nazwę lub podaj nową nazwę lub przywrócenie bazy danych.

Aby uzyskać więcej informacji i uzyskać szczegółowe instrukcje dotyczące przywracanie usuniętych bazy danych za pomocą portalu Azure lub przy użyciu programu PowerShell, zobacz [Przywracanie usuniętych bazy danych](sql-database-recovery-using-backups.md#deleted-database-restore). Nie można przywrócić przy użyciu języka Transact-SQL.

> [AZURE.IMPORTANT] Usunięcie logiczne server nie można odzyskać usunięte bazy danych. 

### <a name="import-from-a-database-archive"></a>Importowanie z archiwum bazy danych

Jeśli utratą danych miejsce poza bieżący okres przechowywania automatycznego wykonywania kopii zapasowych i zostały archiwizacji bazy danych, możesz [importowania zarchiwizowane pliku PLECAK](sql-database-import.md) do nowej bazy danych. Na tym etapie można zamienić oryginalnej bazy danych w bazie danych zaimportowanych lub skopiuj dane potrzebne do oryginalnej bazy danych z zaimportowanych danych. 

## <a name="recover-a-database-to-another-region-from-an-azure-regional-data-center-outage"></a>Przywrócenie bazy danych do innego obszaru z Azure dane regionalne centrum awarii

<!-- Explain this scenario -->

Mimo że rzadkie Centrum Azure danych może zawierać awarii. W przypadku wystąpienia awarii powoduje zakłócenia firm tylko może trwać kilka minut lub może trwać godzin. 

- Jedną z opcji jest czekać na bazę danych, aby powrocie do trybu online po awarii centrum danych. Działa to aplikacje, których można możesz mieć bazy danych w trybie offline. Na przykład w rozwoju projektu lub bezpłatną wersję próbną nie musisz pracować nad stale. Centrum danych po awarii nie wiesz, jak długo trwa awarii, więc ta opcja tylko wtedy, gdy nie potrzebujesz bazy danych przez pewien czas.
- Innym rozwiązaniem jest albo przełączanie awaryjne do innego obszaru danych, jeśli korzystasz z aktywnej replikacji Geo lub odzyskiwanie przy użyciu kopie zapasowe zbędnych geo bazy danych (Geo przywracanie). Pracy awaryjnej wystarczy tylko kilka sekund podczas odzyskiwania z kopii zapasowych trwa godzin.

Po wykonaniu akcji, jak długo trwa odzyskanie i ile utratą danych ponoszenia wypadku awarii centrum danych zależy od tego, jak mają być używane funkcje ciągłości biznesowych opisany w aplikacji. W rzeczywistości można użyć kombinacji aktywnej replikacji Geo zależnie od potrzeb aplikacji i kopie zapasowe bazy danych. Zapoznać się z omówieniem aplikacji zagadnienia projektowe dla autonomicznego baz danych oraz elastycznych puli korzystania z tych funkcji ciągłości firm zobacz [Projektowanie wniosek o odzyskiwanie w chmurze](sql-database-designing-cloud-solutions-for-disaster-recovery.md) i [elastycznym puli strategii odzyskiwania danych](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).

W poniższych sekcjach omówienie czynności, aby odzyskać przy użyciu kopie zapasowe bazy danych lub aktywnej replikacji Geo. Aby uzyskać szczegółowe instrukcje, w tym planowanie wymagania, kroki odzyskiwania wpis i informacji na temat celu zasymulowania awarii do wykonywania rozwijania odzyskiwanie danych zobacz [odzyskiwanie bazy danych SQL z awarii](sql-database-disaster-recovery.md).

### <a name="prepare-for-an-outage"></a>Przygotowanie do awarii

Niezależnie od tego, funkcja ciągłości biznesowej używanego należy:

- Identyfikowanie i przygotowanie serwera docelowej, w tym reguły zapory na poziomie serwera logowania i uprawnienia na poziomie głównym bazy danych.
- Dowiedzieć się, jak przekierowywać klientów i aplikacje klienckie na nowy serwer
- Dokument inne zależności, takich jak inspekcja ustawienia i alertów 
 
Jeśli nie planowanie i przygotowywanie poprawnie, korzystanie z aplikacji online po trybie awaryjnym lub odzyskiwania wymaga więcej czasu, które mogą również wymagać rozwiązywania problemów w czasie obciążenia — nieprawidłowe połączenie.

### <a name="failover-to-a-geo-replicated-secondary-database"></a>Przełączanie awaryjne do replikowane geo pomocniczego bazy danych 

Jeśli korzystasz z aktywnej replikacji Geo jako mechanizmu odzyskiwania [wymusić awarię na pomocniczym replikowane geo](sql-database-disaster-recovery.md#failover-to-geo-replicated-secondary-database). W ciągu kilku sekund pomocniczej jest podwyższany do są zamieniane na nowy podstawowy i jest gotowa do wysłania do rejestrowania nowych transakcji i odpowiedzieć na wszelkie pytania — z zaledwie kilku sekund utraty danych dane, które nie zostały jeszcze replikowane. Aby uzyskać informacje na temat automatyzacji awaryjnego procesu zobacz [Projektowanie aplikacji chmury awarii](sql-database-designing-cloud-solutions-for-disaster-recovery.md).

> [AZURE.NOTE] Gdy centrum danych przechodzi do trybu online, możesz to zrobić aby oryginalny podstawowy (lub nie).

### <a name="perform-a-geo-restore"></a>Wykonywanie przywracania Geo 

Jeśli korzystasz z automatycznych kopii zapasowych z replikacją zbędne geo miejsca do magazynowania jako mechanizmu odzyskiwania [zainicjować odzyskiwanie bazy danych za pomocą funkcji Odzyskiwanie Geo](sql-database-disaster-recovery.md#recover-using-geo-restore). Odzyskiwania zazwyczaj odbywa się w ciągu 12 godzin — utraty danych do godziny określona przez podczas ostatniego godzinowe różnicy tworzenia kopii zapasowej z podjętych i zreplikowanej. Zakończenia odzyskiwania bazy danych nie może zarejestrować wszystkie transakcje lub odpowiedzieć na wszelkie pytania. 

> [AZURE.NOTE] Jeśli w centrum danych jest powrocie do trybu online, przed przejściem aplikacji odzyskanego bazy danych, możesz po prostu anulować odzyskiwania.  

### <a name="perform-post-failover--recovery-tasks"></a>Publikowanie wykonywanie pracy awaryjnej / zadań Odzyskiwanie 

Po ich odzyskaniu z obu mechanizmu odtwarzania należy wykonać następujące zadania dodatkowe przed użytkowników i aplikacje Wstecz i rozpocząć pracę:

- Przekieruj klientów i aplikacje klienckie na serwerze nowych i przywrócenie bazy danych
- Upewnij się, że reguły zapory poziomie serwera odpowiednie obowiązują użytkownikom nawiązywanie połączenia (lub użyj [zapory poziomie bazy danych](sql-database-firewall-configure.md#creating-database-level-firewall-rules))
- Upewnij się, że odpowiednie logowania i uprawnienia na poziomie wzorca bazy danych (lub użyj [zawarte użytkowników](https://msdn.microsoft.com/library/ff929188.aspx))
- Konfigurowanie inspekcji, zależnie od potrzeb
- Konfigurowanie alertów, zależnie od potrzeb

## <a name="upgrade-an-application-with-minimal-downtime"></a>Uaktualnianie aplikacji z minimalnymi przestoje

Czasami aplikacja musi być trybu offline ze względu na planowanej konserwacji, takich jak uaktualnienie aplikacji. [Uaktualnienie aplikacji Zarządzanie](sql-database-manage-application-rolling-upgrade.md) informacje dotyczące używania aktywnej replikacji Geo umożliwiające uaktualnianie przewijane aplikację chmurze, aby zminimalizować przestoje podczas uaktualniania i podaj ścieżka odzyskiwania, w przypadku wystąpią problemy. W tym artykule wygląda na dwóch różnych metod orchestrating proces uaktualniania i omówiono zalety oraz możliwości poszczególnych opcji.

## <a name="next-steps"></a>Następne kroki

Zapoznać się z omówieniem aplikacji zagadnienia projektowe dla autonomicznego baz danych oraz elastycznych puli zobacz [Projektowanie wniosek o odzyskiwanie w chmurze](sql-database-designing-cloud-solutions-for-disaster-recovery.md) i [strategii odzyskiwania danych elastyczne puli](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).






