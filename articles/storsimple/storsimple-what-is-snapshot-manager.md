<properties 
   pageTitle="Co to jest Menedżer migawkę StorSimple? | Microsoft Azure"
   description="W tym artykule opisano Menedżera migawkę StorSimple, jego architektury i funkcji."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="05/24/2016"
   ms.author="v-sharos" />

# <a name="what-is-storsimple-snapshot-manager"></a>Co to jest Menedżer migawkę StorSimple?

## <a name="overview"></a>Omówienie

Menedżer migawkę StorSimple jest przystawką programu Microsoft Management Console (MMC), która upraszcza zarządzanie kopii zapasowej w środowisku programu Microsoft Azure StorSimple i ochrony danych. StorSimple migawkę Menedżera można zarządzać danych Microsoft Azure StorSimple w centrum danych i w chmurze rozwiązanie problemu z jednego miejsca do magazynowania zintegrowane, a więc upraszczanie procesów wykonywania kopii zapasowych i zmniejszenie kosztów.

To omówienie wprowadza Menedżer migawkę StorSimple, w tym artykule opisano jego funkcji i wyjaśniono, jego rolę w programie Microsoft Azure StorSimple. 

Zawiera omówienie dotyczące całego systemu Microsoft Azure StorSimple, w tym urządzenia StorSimple, usługa Menedżera StorSimple Menedżera migawkę StorSimple i karta StorSimple dla programu SharePoint, zobacz [serii StorSimple 8000: masowej chmury hybrydowych](storsimple-overview.md). 
 
>[AZURE.NOTE] 
>
>- Nie można używać Menedżera migawkę StorSimple do zarządzania Microsoft Azure StorSimple wirtualnych tablic (nazywane także StorSimple lokalnej wirtualną urządzenia).
>
>- Jeśli planujesz zainstalować na urządzeniu StorSimple StorSimple aktualizacji 2, pamiętaj pobrać najnowszą wersję programu StorSimple migawkę Manager i zainstaluj ją, **przed zainstalowaniem StorSimple aktualizacji 2**. Najnowszą wersję programu StorSimple migawkę Manager jest zgodny z poprzednimi wersjami i działa we wszystkich wersjach wydaną pakietu Microsoft Azure StorSimple. Jeśli korzystasz z poprzedniej wersji StorSimple migawkę menedżera, należy zaktualizować (nie musisz odinstalować poprzednią wersję przed zainstalowaniem nowej wersji).

## <a name="storsimple-snapshot-manager-purpose-and-architecture"></a>Menedżer migawkę StorSimple przeznaczeniu, architektura

Menedżer migawkę StorSimple zapewnia centralną konsolę zarządzania używaną w celu utworzenia spójnych, w chwili kopii lokalnej i danych w chmurze. Na przykład możesz użyć konsoli:

- Konfigurowanie, wykonywanie kopii zapasowej i usuwanie wielkości.
- Konfigurowanie grup głośność, aby upewnić się, którego kopię zapasową danych jest spójna aplikacji.
- Zarządzanie zasadami kopii zapasowej, dlatego danych kopii zapasowej interwałem serii rozłożonych w czasie.
- Tworzenie lokalnej i w chmurze migawki, które mogą być przechowywane w chmurze, a na potrzeby awarii.

Menedżer migawkę StorSimple pobiera na liście aplikacji zarejestrowana u dostawcy VSS na hoście. Następnie do tworzenia kopii zapasowych spójną z aplikacją, sprawdza wielkość używaną przez aplikację i proponuje głośność grupy, aby skonfigurować. Menedżer migawkę StorSimple używa tych grup głośność wygenerować kopie zapasowe, które są zgodne z aplikacji. (Spójności aplikacji istnieje po wszystkie powiązane z nim pliki i baz danych są synchronizowane i reprezentują rzeczywistego stanu aplikacji w określonym punkcie w czasie). 

Menedżer migawkę StorSimple kopie zapasowe formę migawki przyrostowe, które umożliwiają przechwytywanie tylko zmiany od czasu ostatniej kopii zapasowej. W wyniku wykonywania kopii zapasowych wymaga mniej miejsca do magazynowania i można tworzyć i szybko przywrócić. Menedżer migawki StorSimple używa Windows głośność cień kopiowania usługi (VSS), aby upewnić się, że migawki przechwytywać aplikacji spójnych danych. (Aby uzyskać więcej informacji, przejdź do integracji z sekcją usługi kopiowania w tle woluminu systemu Windows.) StorSimple migawkę Menedżera można utworzyć kopii zapasowej harmonogramów lub wykonać natychmiastowej kopie zapasowe, stosownie do potrzeb. Jeśli chcesz przywrócić dane z kopii zapasowej, umożliwia StorSimple migawkę menedżera, możesz wybrać z katalogiem lokalnie lub migawki w chmurze. Azure StorSimple przywraca tylko dane, które są potrzebne w razie potrzeby go uniemożliwiające opóźnienia w udostępnianiu danych podczas operacji przywracania).

![Architektura StorSimple migawkę Manager](./media/storsimple-what-is-snapshot-manager/HCS_SSM_Overview.png)

**Architektura StorSimple migawkę Manager** 

## <a name="support-for-multiple-volume-types"></a>Obsługę wielu typów głośności

Menedżer migawkę StorSimple umożliwia konfigurowanie i z powrotem w następujących typów wielkości: 

- **Podstawowe** — woluminu podstawowego jest jedną partycją na dysku podstawowym. 

- **Prosta wielkości** — woluminu prostego jest dynamicznego woluminu, który zawiera miejsca na jednym dysku dynamicznym. Woluminu prostego składa się z jednego obszaru dysku lub wielu regiony, które są połączone ze sobą na tym samym dysku. (Tylko na dyskach dynamicznych można utworzyć prosty wielkości). Nie są odporność na uszkodzenia.

- **Wielkości dynamiczne** — dynamiczne głośność jest utworzony na dysku dynamiczne. Dyski dynamiczne umożliwia śledzenie informacji dotyczących ilości znajdujących się na dyskach dynamicznych w komputerze bazy danych. 

- **Dynamiczne wielkości z odzwierciedlające** — dynamiczne wielkości z odzwierciedlające utworzono w architekturze RAID 1. Z RAID 1 identyczne dane są zapisywane na dysku dwóch lub więcej produkcji zestaw lustrzane. Żądanie odczytu następnie są obsługiwane przez każdego dysku, który zawiera żądane dane.

- **Udostępnione klaster wielkości** — przy użyciu udostępnionych klaster ilości (CSVs), wiele węzłów w klastrze pracy awaryjnej można jednocześnie odczytu lub zapisu na tym samym dysku. Przełączanie awaryjne z jednego węzła do innego węzła mogą występować szybko, bez konieczności zmiany w własność dysku lub instalowanie, odinstalowywanie i usunięcie woluminu. 

>[AZURE.IMPORTANT] Nie mieszać CSVs i innych niż CSVs w tym samym migawkę. Łączenie CSVs i innych niż CSVs migawki nie jest obsługiwane. 
 
Menedżer migawkę StorSimple umożliwia Przywracanie całego woluminu grupy lub klonowanie poszczególnych ilości i odzyskiwanie pojedyncze pliki.

- [Wielkości i grup głośności](#volumes-and-volume-groups) 
- [Typy kopii zapasowych i zasady kopii zapasowej](#backup-types-and-backup-policies) 

Aby uzyskać więcej informacji na temat funkcji Menedżera migawkę StorSimple i sposobach ich używania zobacz [Menedżer migawkę StorSimple interfejsu użytkownika](storsimple-use-snapshot-manager.md).

## <a name="volumes-and-volume-groups"></a>Wielkości i grup głośności

StorSimple migawkę Menedżer tworzenie wielkości i skonfigurować je w grupach głośność. 

Menedżer migawkę StorSimple używa grupy głośność tworzenie kopii zapasowych, które są zgodne z aplikacji. Zgodność aplikacji istnieje, gdy wszystkie powiązane z nim pliki i baz danych są synchronizowane i reprezentują rzeczywistego stanu aplikacji w określonym punkcie w czasie. Grupy głośność, (które są nazywane także *grupy spójności*) podstawą kopii zapasowej lub przywracanie zadania.

Głośność grupy nie są takie same jako kontenery głośność. Kontener głośność zawiera co najmniej jeden wielkości, które współużytkują konto miejsca do magazynowania chmury i inne atrybuty, takie jak zużycie szyfrowania i przepustowość. Kontenera pojedynczy głośność może zawierać maksymalnie 256 znacznie ustanawianie ilości StorSimple. Aby uzyskać więcej informacji na temat kontenerów głośność przejdź do [zarządzania kontenerów głośność](storsimple-manage-volume-containers.md). Grupy głośność to kolekcje wielkości, które konfigurować w celu ułatwienia kopii zapasowych. Po wybraniu dwóch wielkości, które należą do różnych głośność kontenerów, umieść je w grupie pojedynczy głośności, a następnie utwórz zasady tworzenia kopii zapasowych dla tej grupy głośność każdego woluminu będzie kopii zapasowej w kontenerze odpowiedniej wielkości przy użyciu konta odpowiednie miejsca do magazynowania.

>[AZURE.NOTE] Wszystkie ilości w grupie głośność musi pochodzić z chmury jednego usługodawcę.

## <a name="integration-with-windows-volume-shadow-copy-service"></a>Integracja z usługą kopiowania w tle głośność systemu Windows

Menedżer migawkę StorSimple Windows głośność cień kopiowania usługi (VSS) jest używany do rejestrowania aplikacji spójnych danych. VSS ułatwia spójności aplikacji przez komunikowania się z aplikacji obsługującej VSS koordynowanie tworzenie migawek przyrostowe. VSS gwarantuje, że aplikacje są tymczasowo nieaktywne lub spoczynku, gdy są wykonywane migawki. 

Menedżer migawkę StorSimple stosowania VSS współpracuje z programu SQL Server i ogólnego wielkości NTFS. Proces jest w następujący sposób: 

1. Żądającego, zwykle jest zarządzanie danymi i rozwiązania ochrony (takie jak StorSimple migawkę Menedżer) lub aplikacji wykonywania kopii zapasowej, wywołuje VSS i z pytaniem, aby zbieranie informacji z programu Edytor w aplikacji docelowej.

2. VSS kontaktuje się składnika Edytor do pobierania opis danych. Edytor zwraca opis danych do wykonania kopii zapasowej. 

3. VSS sygnalizuje Autor powinien przygotowanie do tworzenia kopii zapasowych. Edytor Przygotowanie danych na potrzeby kopii zapasowej, wykonując otwartych transakcji aktualizowanie dzienników transakcji i tak dalej, a następnie powiadamia VSS.

4. VSS powoduje, że autor powinien tymczasowo zatrzymać magazynów danych aplikacji i upewnij się, że dane nie są zapisywane wielkość podczas tworzenia kopii w tle. W tym kroku zapewnia spójność danych i przejście nie więcej niż 60 sekund.

5. VSS powoduje, że dostawcę w celu utworzenia kopii w tle. Dostawców, które mogą być oprogramowania lub sprzętu opartymi, zarządzanie wielkość, które są obecnie uruchomione i tworzyć ich kopie w tle na żądanie. Dostawca tworzy kopię w tle i powiadamia VSS po jego zakończeniu.

6. VSS kontaktów writer powiadomić aplikację, którą można wznowić We/Wy i upewnij się, że We/Wy została wstrzymana pomyślnie podczas tworzenia kopii w tle. 

7. Jeśli kopia zakończyło się pomyślnie, VSS zwraca lokalizacji kopii żądającego. 

8. Jeśli dane zostały zapisane podczas utworzenia kopii w tle, wykonywanie kopii zapasowej zostaną niespójne. VSS usuwa kopii w tle i powiadamia żądającego. Żądającego można automatycznie Powtórz proces tworzenia kopii zapasowej lub Powiadom administratora próbować w późniejszym czasie.

Widać na ilustracji.

![Proces VSS](./media/storsimple-what-is-snapshot-manager/HCS_SSM_VSS_process.png)

**Proces usługi kopiowania w tle głośność systemu Windows** 

## <a name="backup-types-and-backup-policies"></a>Typy kopii zapasowych i zasady kopii zapasowej

StorSimple migawkę Menedżera można utworzyć kopię zapasową danych i zapisać go lokalnie, jak i w chmurze. Menedżer migawkę StorSimple służy do tworzenia kopii zapasowych danych od razu lub zasady tworzenia kopii zapasowych umożliwia utworzenie harmonogramu automatycznego pobierania kopie zapasowe. Zasady kopii zapasowej umożliwiają także określić, ile migawek zostaną zachowane. 

### <a name="backup-types"></a>Typy kopii zapasowych

Menedżer migawkę StorSimple umożliwia tworzenie kopii zapasowych następujących typów:

- **Lokalne migawek** — lokalne migawki są w chwili kopii danych woluminu, które są przechowywane na tym urządzeniu StorSimple. Zazwyczaj ten typ kopii zapasowej można tworzyć i szybko przywrócić. Za pomocą migawkę lokalne jak lokalnej kopii zapasowej.

- **Migawki chmury** — chmury migawki są w chwili kopii danych woluminu, które są przechowywane w chmurze. Migawki chmury odpowiada migawkę replikować na komputerze z inną, poza przestrzeni dyskowej. Migawki chmurze są szczególnie przydatne w sytuacjach odzyskiwania po awarii.

### <a name="on-demand-and-scheduled-backups"></a>Na żądanie i zaplanowanych kopii zapasowych

Za pomocą Menedżera migawkę StorSimple rozpoczęciu jednorazowej kopii zapasowej być tworzone natychmiast lub zasady tworzenia kopii zapasowych umożliwia planowanie cyklicznych kopii zapasowych.

Zasady tworzenia kopii zapasowych ustawiono automatyczne reguły, które umożliwia Planowanie regularnego wykonywania kopii zapasowych. Zasady tworzenia kopii zapasowych pozwala określić częstotliwość i parametrów do robienia migawek grupy określonego woluminu. Zasady umożliwia określenie daty rozpoczęcia i wygaśnięcia, godziny, częstotliwości i wymagań przechowywania lokalnego i migawki w chmurze. Zasady są stosowane natychmiast po zdefiniowaniu. 

Menedżer migawkę StorSimple umożliwia konfigurowanie lub skonfigurowanie zasady kopii zapasowej, gdy jest to konieczne. 

Możesz skonfigurować dla każdej kopii zapasowej zasady, które są tworzone następujące informacje:

- **Nazwa** — unikatową nazwę zaznaczonej zasady kopii zapasowej.

- **Typ** — typ kopii zapasowej zasad; lokalne migawki lub migawki w chmurze.

- **Grupa zbiorczymi** — grupy głośność, której przypisano wybranych zasad kopii zapasowej.

- **Przechowywanie** — liczba kopii zapasowych do zachowania. Jeśli zaznaczysz pole **Wszystkie** wszystkich kopii zapasowych są zachowywane do momentu osiągnięta maksymalna liczba kopii zapasowych objętości, po czym zasady zostaną kończą się niepowodzeniem i generuje komunikat o błędzie. Ponadto można określić liczbę kopii zapasowych, aby zachować (od 1 do 64).

- **Data** — Data utworzenia kopii zapasowej zasad.

Aby dowiedzieć się, jak skonfigurować zasady kopii zapasowej przejdź do [Menedżer migawkę StorSimple służy do tworzenia i zarządzanie zasadami kopii zapasowej](storsimple-snapshot-manager-manage-backup-policies.md).

### <a name="backup-job-monitoring-and-management"></a>Zadania wykonywania kopii zapasowej monitorowania i zarządzania

Za pomocą Menedżera migawkę StorSimple do monitorowania i zarządzania nadchodzące zaplanowanych i wykonanych zadań kopii zapasowej. Ponadto Menedżer migawkę StorSimple udostępnia katalog maksymalnie 64 wykonanych kopii zapasowych. Wykaz umożliwia znajdowanie i przywracanie wielkości lub pojedyncze pliki. 

Informacje dotyczące monitorowania zadania kopii zapasowej przejdź do [Menedżer migawkę StorSimple służy do wyświetlania i zarządzanie zadaniami kopii zapasowej](storsimple-snapshot-manager-manage-backup-jobs.md).


## <a name="next-steps"></a>Następne kroki

- Dowiedz się więcej o [korzystaniu z Menedżera migawkę StorSimple administrowania rozwiązania StorSimple](storsimple-snapshot-manager-admin.md).

- [Menedżer migawkę StorSimple](https://www.microsoft.com/download/details.aspx?id=44220)pobierania.
