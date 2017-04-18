<properties 
   pageTitle="Najważniejsze wskazówki dotyczące tablicy Virtual StorSimple | Microsoft Azure"
   description="W tym artykule opisano najważniejsze wskazówki dotyczące rozmieszczania i zarządzania tablicy Virtual StorSimple."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/18/2016"
   ms.author="alkohli" />

# <a name="storsimple-virtual-array-best-practices"></a>Tablica Virtual StorSimple najważniejsze wskazówki

## <a name="overview"></a>Omówienie

Microsoft Azure StorSimple wirtualnych tablicy jest zintegrowane masowej, zarządzającą zadań magazynu między lokalnego wirtualne urządzenie działa monitor maszyny wirtualnej i Microsoft Azure magazynu w chmurze. Tablica Virtual StorSimple jest alternatywy wydajne, efektywne pod względem kosztów w tabeli fizycznie 8000 serii. Wirtualna tablicy można uruchamiać na istniejącą infrastrukturę monitor maszyny wirtualnej, obsługuje iSCSI protokołów SMB i dobrze nadaje się do scenariusze dotyczące usługi office remote/oddział. Aby uzyskać więcej informacji o rozwiązaniach StorSimple przejdź do [Programu Microsoft Azure StorSimple Przegląd](https://www.microsoft.com/en-us/server-cloud/products/storsimple/overview.aspx).

W tym artykule opisano najważniejsze wskazówki wykonane w trakcie początkowej konfiguracji, wdrażania i zarządzania tablicy Virtual StorSimple. Poniższe najważniejsze wskazówki kierować sprawdzanej ustawień i zarządzania wirtualną macierzy. Ten artykuł jest dobrany Administratorzy IT, którzy wdrażanie i zarządzanie nią tablic wirtualnych w centrach.

Zalecamy okresowy przegląd wskazówek dotyczących pomocy, upewnij się, że urządzenie jest nadal w zgodności po wprowadzeniu zmian przepływu konfiguracji lub operacji. Należy w przypadku wystąpienia problemów podczas implementowania tymi najlepszymi rozwiązaniami w macierzy wirtualnych, [skontaktuj się z pomocą techniczną firmy Microsoft](storsimple-contact-microsoft-support.md) w celu uzyskania pomocy.

## <a name="configuration-best-practices"></a>Najważniejsze wskazówki dotyczące konfiguracji 

Poniższe najważniejsze wskazówki obejmuje wskazówek, które muszą występować podczas początkowej konfiguracji i wdrażania tablic wirtualnych. Poniższe najważniejsze wskazówki zawierać związane ze inicjowania obsługi administracyjnej maszyny wirtualnej ustawienia zasad grupy, zmiany rozmiaru, aby skonfigurować sieć, konfigurowanie kont miejsca do magazynowania i tworzenia akcji i wielkości wirtualnych tablicy. 

### <a name="provisioning"></a>Inicjowanie obsługi administracyjnej 

Tablica Virtual StorSimple jest maszyny wirtualnej (maszyn wirtualnych) obsługi administracyjnej na monitor maszyny wirtualnej (funkcji Hyper-V lub VMware) serwera hosta. Podczas inicjowania obsługi administracyjnej maszyny wirtualnej, upewnij się, że hosta jest w stanie przeznaczoną wystarczających zasobów. Aby uzyskać więcej informacji przejdź do [minimalnych wymagań dotyczących zasobów](storsimple-ova-deploy2-provision-hyperv.md#step-1-ensure-that-the-host-system-meets-minimum-virtual-device-requirements) , aby zapewnić tablicy. 

Wykonania poniższych najlepszych rozwiązań podczas inicjowania obsługi administracyjnej wirtualnych tablicy:


|                        | Funkcji Hyper-V                                                                                                                                        | VMware                                                                                                               |
|------------------------|------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|
| **Typ maszyn wirtualnych**   | **Generowanie 2** Maszyn wirtualnych do użytku z systemem Windows Server 2012 lub nowszym, obraz *.vhdx* . <br></br> **Generowanie 1** Maszyn wirtualnych obrazu *VHD* i korzystanie z systemu Windows Server 2008 lub nowszym.                                                                                                              | Za pomocą maszyn wirtualnych w wersji 8-11 przy użyciu *.vmdk* obrazu.                                                                      |
| **Typ pamięci**            | Konfigurowanie jako **statyczne pamięci**. <br></br> Nie należy używać opcji **pamięci dynamicznej** .            |                                                    |
| **Typ danych dysku**         | Inicjowanie obsługi jako **dynamiczne rozwijanie**.<br></br> **Stały rozmiar** zajmuje dużo czasu. <br></br> Nie należy używać opcji **różnicowy** .                                                                                                                   | Za pomocą opcji **cienkie świadczenia** .                                                                                      |
| **Modyfikowanie dysku danych** | Rozszerzenie lub zmniejszanie nie jest dozwolone. Próba powoduje utratę wszystkich danych lokalnych na urządzeniu.                       | Rozszerzenie lub zmniejszanie nie jest dozwolone. Próba powoduje utratę wszystkich danych lokalnych na urządzeniu. |

### <a name="sizing"></a>Zmiana rozmiaru

Podczas zmiany rozmiaru macierzy Virtual StorSimple, należy rozważyć następujące kwestie:

- Lokalne zastrzeżenie wielkości lub akcji. Około 12% obszaru jest zastrzeżona w warstwie lokalne dla każdego ustanawianie woluminu warstwowych lub udostępnianie. Około 10% obszaru jest również zastrzeżona dla lokalnie przypięty woluminu systemu plików.
- Obciążenie migawkę. Około 15% miejsca na warstwie lokalne jest zarezerwowana do migawki.
- Potrzeba przywracania. Jeśli wykonanie przywracania jako nowa operacja, zmiany rozmiaru należy uwzględniać przestrzeń potrzebna do przywracania. Przywracanie jest gotowe do udziału lub wielkość ten sam rozmiar lub większej.
- Niektóre buforu powinna zostać przydzielona dla dowolnego nieoczekiwane wzrostu.

W zależności od czynników poprzedniego, wymagania dotyczące zmiany rozmiaru może być reprezentowany przez następujące równanie:

`Total usable local disk size = (Total provisioned locally pinned volume/share size including space for file system) + (Max (local reservation for a volume/share) for all tiered volumes/share) + (Local reservation for all tiered volumes/shares)`

`Data disk size = Total usable local disk size + Snapshot overhead + buffer for unexpected growth or new share or volume`


W poniższych przykładach przedstawiono sposób zmieniania rozmiaru tablicę wirtualnych zgodnie z własnymi potrzebami.

#### <a name="example-1"></a>Przykład 1:
Na wirtualnej macierzy ma mieć możliwość 

- Inicjowanie obsługi 2 TB warstwowych głośność lub udostępnianie.
- Inicjowanie obsługi 1 TB warstwowych głośność lub udostępnianie.
- Inicjowanie obsługi 300 GB wielkości lokalnie przypięty lub udostępnianie.


Dla poprzednich wielkości lub akcji Pozwól nam obliczyć wymagania dotyczące miejsca na warstwie lokalny. 

Najpierw dla każdego warstwowych głośności i udostępnianie lokalnego rezerwacji są takie same do 12% rozmiaru głośności i Udostępnij. Lokalnie przypięty głośność/share rezerwacji lokalne w przypadku 10% wielkości lokalnie przypięty głośność/share (oprócz ustanawianie rozmiar). W tym przykładzie potrzebne

- Lokalne rezerwacji 240 GB (do 2 TB tiered głośność/share)
- Lokalne zastrzeżenie 120 GB (do 1 TB tiered głośność/share)
- 330 GB lokalnie przypięty głośność lub udostępnianie (Dodawanie 10% rezerwacji lokalne do rozmiaru 300 GB obsługi administracyjnej)

Jest wymagana w warstwie lokalne pory całkowita ilość miejsca: 240 GB + 120 GB + 330 GB = 690 GB.

Drugi potrzebujemy co najmniej ilość miejsca w warstwie lokalne jako największa rezerwacji pojedynczy. Ten dodatkowy kwota jest używana w przypadku, gdy chcesz przywrócić z migawki chmury. W tym przykładzie największa Rezerwacja lokalne jest 330 GB (w tym rezerwacji systemu plików), więc należy dodać do 690 GB: 690 GB + 330 GB = 1020 GB.
W przypadku możemy kolejnych przywraca dodatkowe możemy zawsze zwolnić miejsce z poprzedniej operacji przywracania.

Trzecia potrzebujemy 15% sumy lokalnej przestrzeni pory do przechowywania lokalne migawki, tak, aby tylko 85% jest dostępna. W tym przykładzie, który będzie wokół 1020 GB = 0.85&ast;danych ustanawianie dysku TB. Tak, będzie dysku ustanawianie danych (1020&ast;(1-0.85)) = 1200 GB = 1,20 TB ~ 1,25 TB (zaokrąglania do najbliższej kwartyl)

Factoring w nieoczekiwane wzrostu i przywraca nowy, należy zapewnić dysku lokalnym wokół 1.25-1,5 TB.

> [AZURE.NOTE] Zaleca się również, że dysk lokalny jest znacznie obsługi administracyjnej. Zalecenie dotyczy, ponieważ miejsca przywracania jest potrzebny tylko wtedy, gdy chcesz przywrócić dane, które są starsze niż pięć dni. Odzyskiwanie na poziomie elementu umożliwia przywracanie danych dla ostatnich pięciu dni bez konieczności dodatkowego miejsca do przywrócenia.

#### <a name="example-2"></a>Przykład 2: 
Na wirtualnej macierzy ma mieć możliwość 

- Inicjowanie obsługi 2 TB tiered głośności
- Obsługa administracyjna lokalnie 300 GB przypięte głośności

W zależności od 12% rezerwacji lokalnej przestrzeni warstwowych wielkości-akcji i 10% lokalnie przypięty wielkości-akcji, potrzebujemy

- Lokalne zastrzeżenie 240 GB (do 2 TB tiered głośność/share)
- 330 GB lokalnie przypięty głośność lub udostępnianie (Dodawanie 10% rezerwacji lokalne do obszaru 300 GB obsługi administracyjnej)

Całkowita ilość miejsca na warstwie lokalnym wymagane są: 240 GB + 330 GB = 570 GB

Minimalny odstęp lokalne potrzebna do przywracania jest 330 GB. 

15% sumy dysku służy do przechowywania migawek, tak aby tylko 0.85 jest dostępna. Tak, jest rozmiar dysku (900&ast;(1-0.85)) = 1.06 TB ~ 1,25 TB (zaokrąglania do najbliższej kwartyl) 

Factoring w dowolnym nieoczekiwane wzrostu, umożliwia obsługę dysku lokalnym 1.25-1,5 TB.


### <a name="group-policy"></a>Zasady grupy

Zasady grupy jest infrastrukturę, która umożliwia wykonania określonych konfiguracji dla użytkowników i komputerów. Ustawienia zasad grupy są zawarte w obiektach zasad grupy (GPO), które są połączone z następujących kontenerów w usługach domenowych Active Directory (AD DS): witryny, domeny i jednostki organizacyjne (OU). 

W przypadku macierzy wirtualnych domeny, obiekty zasad grupy można stosować do niego. Te obiekty zasad grupy można zainstalować aplikacji, takich jak oprogramowanie antywirusowe, które mogą negatywnie wpłynąć na działanie macierzy Virtual StorSimple.

Dlatego zaleca się, że możesz:

-   Upewnij się, że wirtualnych macierzy znajduje się w osobnym jednostka organizacyjna (OU) usługi Active Directory. 

-   Upewnij się, że nie obiekty zasad grupy (GPO) są stosowane do wirtualnego macierzy. Można zablokować dziedziczenie, aby upewnić się, że tablica wirtualnych (węzła podrzędnego) automatycznie dziedziczą wszystkie obiekty zasad grupy z witryny nadrzędnej. Aby uzyskać więcej informacji przejdź do [Blokowanie dziedziczenia](https://technet.microsoft.com/library/cc731076.aspx).


### <a name="networking"></a>Sieci

Konfiguracja sieciowa dla wirtualnych macierzy odbywa się za pośrednictwem lokalnych sieci web interfejsu użytkownika. Interfejs wirtualną sieć jest włączona przez monitor maszyny wirtualnej, w której tabeli wirtualnej obsługi administracyjnej. Na stronie [Ustawienia sieci](storsimple-ova-deploy3-fs-setup.md) umożliwia skonfigurowanie adresu IP interfejsu wirtualnej sieci, podsieci i bramy.  Można również skonfigurować głównego i pomocniczego serwera DNS, ustawienia czasu i ustawienia serwera proxy opcjonalne dla danego urządzenia. W większości konfiguracji sieci jest jednorazowej konfiguracji. Sprawdź [wymagania sieciowe StorSimple](storsimple-ova-system-requirements.md#networking-requirements) przed wdrożeniem wirtualnych tablicy.

Wdrażając wirtualnych macierzy, zaleca się, należy wykonać poniższe najważniejsze wskazówki:

-   Upewnij się, że sieci, w którym wirtualnych tablica zostanie wdrożony zawsze ma pojemność przeznaczoną 5 przepustowości internetowej MB/s (lub więcej). 

    -   Potrzeba przepustowości internetowej zależnie od właściwości z pracą i szybkość zmian danych.

    -   Zmiana danych, które może być obsłużone jest bezpośrednio proporcjonalna do przepustowość Internetu. Na przykład podczas robienia kopii zapasowej 5 przepustowości MB/s umożliwia rozmieszczanie zmiany danych około 18 GB w 8 godzin. Dzięki cztery razy większej przepustowości (20 MB/s) można obsługiwać cztery razy więcej zmiany danych (72 GB). 

-   Upewnij się, że łączność z Internetem jest zawsze dostępna. Sporadycznie lub mało wiarygodnych połączenia z Internetem do urządzeń może spowodować utratę dostępu do danych w chmurze i może spowodować w nieobsługiwanej konfiguracji.

-   Jeśli zamierzasz wdrożyć urządzenia jako serwerem: 
    -   Zalecamy, aby wyłączyć opcję **automatycznie adres IP uzyskiwanie** (DHCP). 
    -   Konfigurowanie adresów IP. Skonfiguruj podstawowy i pomocniczy serwer DNS.

    -   Jeśli definiujące wiele interfejsów sieciowych usługi wirtualnych tablicy, tylko pierwszy interfejs sieci (domyślnie ten interfejs jest **Ethernet**) mogą skorzystać z chmury. Aby określić typ ruchu, można utworzyć wiele interfejsów wirtualną sieć w macierzy wirtualnych (skonfigurowanego jako serwerem) i łączenie tych interfejsów do różnych podsieci.

-   Przepustowości chmury tylko (używane przez wirtualną tablica), konfigurowanie ograniczania na routera lub zapory. Jeśli zdefiniujesz ograniczania w swojej monitor maszyny wirtualnej, będzie wartość ograniczenia wszystkie protokoły tym iSCSI i SMB zamiast tylko przepustowości chmury. 

-   Upewnij się, że synchronizacja czasu monitorami jest włączona. Jeśli przy użyciu funkcji Hyper-V, wybierz pozycję wirtualnych macierzy w Menedżerze funkcji Hyper-V, przejdź do **Ustawienia &gt; Integration Services**i upewnij się, że **Synchronizacja** jest zaznaczone.

### <a name="storage-accounts"></a>Konta miejsca do magazynowania

Tablica Virtual StorSimple może być skojarzone z kontem jednego miejsca do magazynowania. To konto miejsca do magazynowania może być konta automatycznie wygenerowanego miejsca do magazynowania, konta w tej samej subskrypcji usługi lub konta miejsca do magazynowania związane z innej subskrypcji. Aby uzyskać więcej informacji, zobacz jak [zarządzać kontami miejsca do magazynowania dla wirtualnych macierzy](storsimple-ova-manage-storage-accounts.md).

Za pomocą następujących zaleceniach w przypadku kont miejsca do magazynowania skojarzone z wirtualną macierzy.

-   Podczas łączenia wielu tablic wirtualnych przy użyciu konta z jednego miejsca do magazynowania, współczynnik maksymalnej pojemności (64 TB) tablicę wirtualnych i maksymalny rozmiar (500 TB) dla konta miejsca do magazynowania. To ogranicza liczbę pełnym rozmiarze tablic wirtualnych, które mogą być skojarzone z tym kontem miejsca do magazynowania do około 7.

-   Podczas tworzenia nowego konta miejsca do magazynowania
    -   Zaleca się utworzenie go w regionie najbliżej pakietu office remote/oddział miejsce, w którym wdrożenie macierzy Virtual StorSimple aby zminimalizować opóźnienia.

    -   Mieć na uwadze, że nie można przenieść konto miejsca do magazynowania w różnych regionów. Nie można również przesuwać usługi w subskrypcjach.

    -   Użyj konta miejsca do magazynowania, który wykonuje nadmiarowości między centrami danych. Nadmiarowe Geo przestrzeni dyskowej (GRS), strefy zbędne przestrzeni dyskowej (ZRS) i lokalnie zbędne przestrzeni dyskowej (LRS) są obsługiwane do użytku z wirtualną macierzy. Aby uzyskać więcej informacji na temat różnych typów kont miejsca do magazynowania przejdź do [replikacji Azure miejsca do magazynowania](../storage/storage-redundancy.md).


### <a name="shares-and-volumes"></a>Akcje i ilości

Do tablicy Virtual StorSimple umożliwia obsługę akcji, gdy jest skonfigurowana jako serwer plików i ilości, gdy skonfigurowane jako serwerem. Najważniejsze wskazówki dotyczące tworzenia akcji i ilości odnoszą się do rozmiaru i typu skonfigurowane.

#### <a name="volumeshare-size"></a>Rozmiar woluminu i udostępnianie

Na wirtualnej macierzy umożliwia obsługę akcji, gdy jest skonfigurowana jako serwer plików i ilości, gdy skonfigurowane jako serwerem. Najważniejsze wskazówki dotyczące tworzenia akcji i ilości odnoszą się do rozmiaru i skonfigurowany typ. 

Należy pamiętać poniższych najlepszych rozwiązań podczas inicjowania obsługi administracyjnej akcji lub wielkości na urządzeniu wirtualną.

-   Rozmiarów plików ustanawianie rozmiaru warstwowych udostępnianie mogą mieć wpływ na wydajność tiering. Praca z dużych plików może spowodować powolne warstwa się. Podczas pracy z dużych plików, zaleca się, że największa plik jest mniejszy niż 3% wielkości Udostępnij.

-   Maksymalnej ilości 16-akcji można utworzyć wirtualny tablicy. Jeśli przypięte lokalnie, wielkości i akcji może być między 50 GB do 2 TB. Jeśli tiered, wielkości akcje muszą być między 500 GB do 20 TB. 

-   Podczas tworzenia woluminu, współczynnik zużycie oczekiwanych danych, a także przyszłego zwiększenia ilości. Gdy wielkość nie można rozwinąć później, można zawsze przywrócić do woluminu większe.

-   Po utworzeniu wielkość nie można zmniejszyć rozmiar woluminu na StorSimple.
   
-   Podczas zapisywania warstwowych głośność na StorSimple, gdy dane woluminu osiągnie określony próg (względem lokalnej przestrzeni zarezerwowane dla głośność), to ograniczenie Jo. Kontynuowanie pisanie do tego woluminu wolniej Jo znacznie. Chociaż można napisać do woluminu warstwowych poza możliwości ustanawianie (Firma Microsoft nie aktywnie stop użytkownikowi pisania spoza ustanawianie), pojawi się powiadomienie o alercie, że masz oversubscribed. Gdy zostanie wyświetlony alert, konieczne jest środki naprawcze takich jak Usuń dane woluminu, lub przywrócić wielkość większa objętość (rozszerzeń głośność nie jest obecnie obsługiwane).

-   W przypadku użycia odzyskiwanie danych dozwolony akcji/objętości 16 i maksymalną liczbę akcji i ilości, które mogą być przetwarzane równolegle jest również 16, numer, akcji i wielkości nie ma wpływ na RPO i RTOs. 

#### <a name="volumeshare-type"></a>Typ głośności i udostępnianie

StorSimple obsługuje dwa typy głośności i Udostępnij na podstawie zużycia: lokalnie przypięte i tiered. Lokalnie przypięty wielkości akcje grubo zainicjowano obsługę administracyjną należy znacznie zainicjowano warstwowych wielkości i akcji. 

Zaleca się wykonania poniższych najlepszych rozwiązań, konfigurując StorSimple ilości/akcje:

-   Określ typ głośność według obciążenia, które mają zostać wdrażanie przed utworzeniem woluminu. Używana lokalnie przypięta wielkości obciążenia lokalne gwarancje danych, które wymagają (nawet podczas awarii chmury) i wymagają opóźnienia niskim chmury. Po utworzeniu woluminu na wirtualnej macierzy, nie można zmienić typu woluminu z lokalnie przypięta do warstwowych lub *odwrotnie*. Na przykład utworzyć lokalnie przypięty wielkości, wdrażając obciążenia SQL lub obciążenia hostingu wirtualnych maszyn; Używanie warstwowych wielkości dla obciążenia udostępnianie plików.

-   Zaznacz opcję mniej często używanych danych archiwizacji kontaktach ze pliki dużych rozmiarów. Rozmiar fragmentu deduplication 512 k jest używany po włączeniu tę opcję, aby przyspieszyć transfer danych do chmury.

#### <a name="volume-format"></a>Format głośności

Po utworzeniu wielkości StorSimple na z serwerem, musisz zainicjować, zainstalować i sformatować wielkość. Operacja odbywa się na hoście połączony z urządzenia StorSimple. Podczas instalowania i formatowanie wielkości na hoście StorSimple, zaleca się zgodnie ze wskazówkami.

-   Wykonaj szybkie formatowanie wszystkie ilości StorSimple.

-   Podczas formatowania woluminu StorSimple, użyj rozmiaru jednostki alokacji (Australia) 64 KB (wartość domyślna to 4 KB). 64 KB Australia jest na podstawie testów zrobiona we własnym zakresie typowych obciążenia StorSimple i inne obciążenia.

-   Korzystając z tablicy Virtual StorSimple skonfigurowanego jako serwerem, nie należy używać łączone wielkości lub dyski dynamiczne jako te lub dysków nie są obsługiwane przez StorSimple.

#### <a name="share-access"></a>Udostępnianie programu access

Podczas tworzenia akcji na serwerze plików wirtualnych tablicy, należy przestrzegać następujących zasad:

-   Podczas tworzenia udziału, Przypisz grupy użytkowników jako administrator Udostępnij zamiast pojedynczego użytkownika.

-   Po utworzeniu udziału, edytując akcji za pomocą Eksploratora Windows, możesz zarządzać uprawnienia NTFS.

#### <a name="volume-access"></a>Dostęp do woluminu

Podczas konfigurowania wielkość iSCSI macierzy Virtual StorSimple, należy do kontrolowania dostępu, w razie konieczności. Aby określić, które serwery hosta można uzyskać dostęp do wielkości, Utwórz, a skojarzyć wielkości StorSimple rekordów kontrolki programu access (ACRs).

Podczas konfigurowania ACRs dla ilości StorSimple, należy użyć poniższych najlepszych rozwiązań:

-   Zawsze należy skojarzyć awaryjnego co najmniej jeden z woluminu.

-   Definiowanie wielu ACRs tylko w środowisku grupowany.

-   Przydzielając więcej niż jeden awaryjnego do woluminu, upewnij się, głośność nie jest dostępna w sposób miejsce, w którym będą one jednocześnie dostępne według więcej niż jednego hosta bez klastrów. Jeśli wiele ACRs zostały przypisane do woluminu, komunikat ostrzegawczy dzwoni umożliwiają przeglądanie konfiguracji.

### <a name="data-security-and-encryption"></a>Bezpieczeństwo danych i szyfrowania

Wirtualna macierzy StorSimple zawiera funkcje zabezpieczeń i szyfrowania danych, które zapewniają poufności i integralności danych. Podczas korzystania z tych funkcji, zaleca się, należy wykonać następujące wskazówki: 

-   Definiowanie klucza szyfrowania miejsca do magazynowania w chmurze do generowania szyfrowania AES-256 przed wysłaniem danych z wirtualnego macierzy w chmurze. Ten klucz nie jest wymagane, jeśli dane są szyfrowane będzie zaczynać się. Klucz można wygenerowane i były bezpieczne, przy użyciu systemu zarządzania kluczami, takich jak [Azure klucza magazynu](../key-vault/key-vault-whatis.md).

-   Podczas konfigurowania konta miejsca do magazynowania za pośrednictwem usługi Menedżera StorSimple, upewnij się, że możesz włączyć tryb SSL, aby utworzyć bezpiecznego kanału komunikacji sieci między urządzeniem przenośnym StorSimple a chmury.

-   Ponownie wygenerować klucze dla kont usługi miejsca do magazynowania (po zalogowaniu się do usługi Magazyn Azure) okresowo koncie wszelkie zmiany uzyskać dostęp do na podstawie zmienione listy administratorów.

-   Dane w macierzy wirtualnych są kompresowane i deduplicated przed wysłaniem Azure. Nie zaleca się przy użyciu usługi roli Deduplication danych na hoście systemu Windows Server.


## <a name="operational-best-practices"></a>Operacyjne najważniejsze wskazówki

Operacyjne najważniejsze wskazówki są wskazówki, które należy stosować w operacji wirtualnych tablicy lub codziennego zarządzania. Tych wskazówek obejmują zarządzania określonych zadań, takich jak sporządzanie kopie zapasowe, przywracanie z kopii zapasowej zestawu wykonywania w trybie awaryjnym, dezaktywowanie i usuwanie tablicy, monitorowanie użycia systemu i zdrowia i uruchamianie wirusami skanowania wirtualnych macierzy.

### <a name="backups"></a>Wykonywanie kopii zapasowych

Dane dotyczące wirtualnych macierzy kopii zapasowej w chmurze na dwa sposoby, domyślnie automatycznego codziennego wykonywania kopii zapasowej całego urządzenia rozpoczynając na 22:30 lub za pośrednictwem utworzyć ręczną kopię zapasową na żądanie. Domyślnie urządzenie automatycznie tworzy dzienne migawki chmury wszystkich danych znajdujących się na nim. Aby uzyskać więcej informacji przejdź do [tworzenia kopii zapasowych macierzy Virtual StorSimple](storsimple-ova-backup.md).

Nie można zmienić częstotliwość i przechowywaniem dokumentów skojarzonych z kopii zapasowych domyślnej, ale można skonfigurować godziny, jaką codzienne wykonywanie kopii zapasowych są inicjowane każdego dnia. Konfigurując automatyczne kopie zapasowe czas rozpoczęcia, zalecamy:

-   Planowanie kopii zapasowych dla mniejszego. Czas rozpoczęcia kopii zapasowej należy nie pokrywają się z wieloma hosta Jo.

-   Inicjowanie utworzyć ręczną kopię zapasową na żądanie podczas planowania do wykonywania pracy urządzenie awaryjnej lub przed do okna konserwacji ochrony danych na wirtualnej macierzy.

### <a name="restore"></a>Przywracanie

Można przywrócić z kopii zapasowej ustawić na dwa sposoby: Przywróć do innego woluminu lub Udostępnij lub odzyskania poziomie elementu (dostępne tylko w macierzy wirtualnych pełnienia roli serwera plików). Odzyskiwanie na poziomie elementu umożliwia szczegółowego odzyskiwanie plików i folderów z kopii zapasowej chmury wszystkich akcji na tym urządzeniu StorSimple. Aby uzyskać więcej informacji przejdź do [Przywracanie z kopii zapasowej](storsimple-ova-restore.md).

Podczas wykonywania przywracania, pamiętać o następujących wytycznych:

-   Wirtualna macierzy StorSimple nie obsługuje przywracania w miejscu. Jednak to można łatwo osiągnąć procesem dwuetapowym: zwiększyć ilość miejsca na wirtualnego tablicy, a następnie przywróć do innego woluminu udziału.

-   Przywracanie elementów z woluminu lokalnego należy pamiętać przywracania będzie długotrwałych operacji. Chociaż wielkość może szybko przejść w tryb online, dane w dalszym ciągu można uwodniony w tle.

-   Typ woluminu zmienia się podczas procesu przywracania. Głośność warstwowych zostanie przywrócony do innego woluminu warstwowych i lokalnie przypięty głośności do innego lokalnie przypięte głośność.

-   Podczas próby Przywracanie z kopii zapasowej woluminu lub w udziale ustawić, jeśli zadanie przywracania kończy się niepowodzeniem, głośność docelowej lub nadal można utworzyć udziału w portalu. Należy pamiętać, możesz usunąć tego woluminu nieużywane docelowej lub udostępnianie w portalu w celu zminimalizowania problemów w przyszłości wynikających z tego elementu.

### <a name="failover-and-disaster-recovery"></a>Odzyskiwanie awaryjnego i danych

Trybie awaryjnym urządzenia umożliwia migrację danych z urządzenia *źródła* w obrębie centrum danych na *inne urządzenie znajduje się w tej samej lub innej lokalizacji geograficznej* . Tym przełączeniu urządzenie jest przeznaczony dla całego urządzenia. Podczas awaryjnym przeniesieniu własności zmiany danych chmury urządzenia źródła do tego urządzenia.

Do tablicy Virtual StorSimple można tylko się niepowodzeniem przez innego wirtualną tablicy zarządzane przez tę samą usługę Menedżer StorSimple. Awarię na urządzeniu z systemem 8000 serii lub tablicę zarządzane przez inną usługę Menedżer StorSimple (niż urządzenia źródła) nie jest dozwolone. Aby dowiedzieć się więcej o zagadnieniach pracy awaryjnej, przejdź do [wymagania wstępne do przełączania awaryjnego urządzenia](storsimple-ova-failover-dr.md).

Podczas wykonywania błędu nad programu virtual tablicy, pamiętać o następujących czynności:

-   Dla planowanej trybie awaryjnym jest zalecane najlepiej podjęcie wszystkich wielkości akcji w trybie offline przed podjęciem tym przełączeniu. Postępuj zgodnie z instrukcjami konkretnych systemów operacyjnych wykonać wielkości/akcje w trybie offline na hoście pierwszego, a następnie sporządzanie te offline na urządzeniu wirtualną.

-   Plik serwera odzyskiwanie (DR) zalecamy Połącz urządzenie do tej samej domeny jako źródła, tak aby uprawnienia udostępniania są automatycznie. Tylko przełączanie awaryjne do urządzenia w tej samej domeny jest obsługiwana w tej wersji.

-   Po pomyślnym zakończeniu DR urządzenia jest automatycznie usuwane. Jeśli urządzenie nie jest już dostępna, maszyny wirtualnej, który zainicjowany w systemie hosta jest nadal przez inne zasoby. Zalecamy, możesz usunąć ten maszyn wirtualnych systemu hosta, aby uniemożliwić naliczania opłat.

-   Należy zauważyć, że nawet jeśli tym przełączeniu kończy się niepowodzeniem, **dane są zawsze bezpieczne w chmurze**. Należy rozważyć następujące trzy scenariusze, w których tym przełączeniu nie powiodło się:

    -   Na wczesnym etapie tym przełączeniu, na przykład gdy są wykonywane testy przed DR wystąpił błąd. W takiej sytuacji urządzenie docelowej jest nadal można używać. Ponownie spróbować tym przełączeniu na tym samym urządzeniu docelowej.

    -   Wystąpił błąd podczas rzeczywisty awaryjnego procesu. W tym przypadku urządzenie jest oznaczony jako nie będzie można używać. Należy obsługi administracyjnej i konfigurowanie innego wirtualną tablicy docelowej i użyć do przełączania awaryjnego.

    -   Tym przełączeniu zostało wykonane, po czym urządzenia został usunięty, ale urządzenia występują problemy, a nie masz dostępu do jakichkolwiek danych. Dane są nadal bezpiecznych w chmurze i mogą być pobierane łatwe tworzenie innego wirtualną tablicy, a następnie użyć go jako urządzenia dla DR.

### <a name="deactivate"></a>Dezaktywowanie

Po zdezaktywowaniu tablicę Virtual StorSimple Server połączenia między urządzeniem i odpowiadające im usługi Menedżera StorSimple. Dezaktywacja jest operacji na **stałe** i nie można cofnąć. Wyłączone urządzenia nie można zarejestrować w usłudze Menedżer StorSimple ponownie. Aby uzyskać więcej informacji przejdź do [Dezaktywowanie i usuwanie macierzy Virtual StorSimple](storsimple-deactivate-and-delete-device.md).

W przypadku dezaktywowania wirtualnych macierzy, należy pamiętać o następujących najważniejszych wskazówek:

-   Zrób migawkę chmury wszystkich danych przed dezaktywowanie urządzenie wirtualne. Po dezaktywowaniu tablicę wirtualnych wszystkie dane z urządzenia lokalne zostaną utracone. Migawki chmury umożliwia odzyskiwanie danych w późniejszym terminie.

-   Przed dezaktywowaniu tablicę Virtual StorSimple upewnij się zatrzymać lub usunąć klientów i hostów, które są zależne od urządzenia.

-   Usuwanie urządzenia został dezaktywowany, jeśli już używasz tak, aby nie naliczania opłat.

### <a name="monitoring"></a>Monitorowanie

W celu zapewnienia, że macierzy Virtual StorSimple znajduje się w Państwie prawidłowy ciągły, musisz monitorowanie tablicy i upewnij się, otrzymywania informacji o tym alerty systemu. Monitorowanie ogólnej kondycji wirtualnych tablicy, zaimplementować następujące wskazówki:

- Konfigurowanie monitorowania, aby śledzić użycie miejsca na dysku wirtualnego tablicy danych, a także dysku systemu operacyjnego. Jeśli z funkcji Hyper-V, umożliwia połączenie Menedżera maszyn wirtualnych systemu w Centrum (SCVMM) i System Center Operations Manager (SCOM) monitorowanie hosty wirtualizacji.   

- Skonfiguruj powiadomienia e-mail na wirtualnej macierzy do wysyłania alertów na niektórych poziomach zastosowania.                                                                                                                                                                                                

### <a name="index-search-and-virus-scan-applications"></a>Indeks wyszukiwania i wirusami skanowanie aplikacji

Macierz Virtual StorSimple można automatycznie poziomu danych z warstwy lokalnych z chmurą Microsoft Azure. Użycie aplikacji, takich jak indeks wyszukiwania lub skanowanie w poszukiwaniu wirusów przeglądania danych przechowywanych na StorSimple należy zajmować się, że dane chmury nie uzyskać dostęp do i pobierane powrót do poziomu lokalnego.

Zaleca się wykonania następujących najważniejszych wskazówek, podczas konfigurowania skanowanie indeksu wyszukiwania lub wirusa wirtualnych macierzy:

-   Wyłącz wszystkie operacje automatycznie skonfigurowany pełne skanowanie.

-   Do przedstawienia wielkości warstwowych skonfigurować indeks wyszukiwania lub wirusa skanowania aplikację, aby wykonać skanowanie przyrostowe. To samo skanowanie tylko nowych danych może znajdujących się na warstwie lokalne. Dane, które jest warstwowa w chmurze nie są dostępne podczas operacji przyrostowe.

-   Upewnij się, filtry wyszukiwania poprawne i ustawienia są skonfigurowane tak, aby uzyskać zeskanowany zamierzonego typy plików. Na przykład obraz plików (JPEG, GIF i TIFF) i rysunek techniczny rysunków, nie należy zeskanowane podczas odbudowywania indeksu przyrostowe lub pełny.

Korzystając z systemu Windows indeksowanie proces, przestrzegaj następujących wytycznych:

-   Nie należy używać indeksowania systemu Windows do przedstawienia wielkości warstwowych, jak ją przypomina dużych ilości danych (TBs) z chmury Jeśli jest konieczne często odbudowany indeks. Odbudowanie indeksu chcesz pobrać wszystkie typy plików do indeksowania ich zawartości.

-   Korzystanie z systemu Windows, indeksowania proces lokalnie przypięty wielkości, jak to będzie tylko dostęp do danych na lokalnym warstw do tworzenia indeksu (dane chmur nie będzie dostępna).

### <a name="byte-range-locking"></a>Blokowanie zakresu bajtów

Aplikacje można zablokować określony zakres bajtów w plikach. Jeśli bajtu blokowanie zakresu został włączony w aplikacji, które będą zapisywane do Twojej StorSimple, następnie obsługi nie działa w wirtualnej macierzy. Do obsługi pracy wszystkich obszarach uzyskać dostępu do plików powinny być odblokowana. Blokowanie zakresu bajtów nie jest obsługiwane w warstwowych wielkości na wirtualnej macierzy.

Zalecane sposoby zmniejszenia bajtu blokowanie zakres obejmują:

-   Wyłączanie blokowania w logiki aplikacji zakres bajtów.

-   Używana lokalnie przypięte wielkości (zamiast tiered) dla danych skojarzonych z tej aplikacji. Lokalnie przypięty wielkości nie warstwa w chmurze.

-   Gdy lokalnie przy użyciu przypięte wielkości z zakresu bajtu blokowanie włączone, głośność mogą pochodzić online, przed zakończeniem przywracania. W takich przypadkach trzeba poczekać Przywróć, aby zakończyć się.

## <a name="multiple-arrays"></a>Wielu tablicach

Wiele tablic wirtualnych może być konieczne należy wdrożyć na koncie rosnących zestaw roboczy danych, który może rozsypywać na chmurze, co wpływa na działanie urządzenia. W takich przypadkach najlepiej urządzeń skali w miarę zestaw roboczy. Wymaga to co najmniej jedno urządzenie, które mają zostać dodane w centrum danych lokalnych. Podczas dodawania urządzenia, możesz:

-   Dzielenie bieżącego zestawu danych.
-   Wdrażanie nowego obciążenia nowych urządzeń.
-   Wdrażanie wielu tablic wirtualnych, zalecane z równoważenia obciążenia perspektywy rozłożenie tablicy w różnych monitor maszyny wirtualnej hosts.

-  Wiele tablic wirtualnych (w przypadku pełnienia roli serwera plików lub serwerem) mogą być rozmieszczone w Distributed Namespace systemu plików. Aby uzyskać szczegółowe instrukcje przejdź do [Distributed pliku System Namespace rozwiązanie z Podręcznik wdrażania miejsca do magazynowania hybrydowych w chmurze](https://www.microsoft.com/download/details.aspx?id=45507). Rozłożone replikacją systemu plików nie jest obecnie zalecane do użytku z wirtualną tablicy. 


## <a name="see-also"></a>Zobacz też
Dowiedz się, jak [Administrowanie macierzy Virtual StorSimple](storsimple-ova-manager-service-administration.md) za pośrednictwem usługi Menedżera StorSimple.
