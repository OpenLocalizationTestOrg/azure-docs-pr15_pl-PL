<properties 
   pageTitle="Urządzenia wirtualnego StorSimple aktualizacji 2 | Microsoft Azure"
   description="Dowiedz się, jak tworzyć, wdrażania i zarządzać urządzenie wirtualne StorSimple w wirtualnej sieci Microsoft Azure. (Dotyczy aktualizacja StorSimple 2)."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/23/2016"
   ms.author="alkohli" />

# <a name="deploy-and-manage-a-storsimple-virtual-device-in-azure"></a>Wdrażanie i zarządzanie nią urządzenie wirtualne StorSimple platformy Azure


##<a name="overview"></a>Omówienie
Urządzenie wirtualne serii StorSimple 8000 jest dodatkowe możliwości, które zawiera rozwiązanie Microsoft Azure StorSimple. Urządzenia wirtualnego StorSimple działa na komputerze wirtualnych w wirtualnej sieci Microsoft Azure i służy do tworzenia kopii zapasowych i klonowanie danych z hosty. Ten samouczek opisano, jak wdrażać i zarządzanie urządzenie wirtualne platformy Azure i jest stosowane do wszystkich urządzeń wirtualnych wersja oprogramowania aktualizacji 2 i dolnym.


#### <a name="virtual-device-model-comparison"></a>Porównanie modeli urządzenia wirtualnego

Urządzenia wirtualnego StorSimple jest dostępne w dwóch modeli, 8010 standardowy (wcześniej nazywanego 1100) i premium 8020 (wprowadzona w aktualizacji nr 2). W celu porównania dwóch modeli są oznaczane znakami tabulacji poniżej.


| Modelu urządzenia          | 8010<sup>1</sup>                                                                     | 8020                                                                                                                               |
|-----------------------|---------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------|
| **Maksymalna pojemność**      | 30 TB                                                                     | 64 TB                                                                                                                                |
| **Azure maszyn wirtualnych**              | Standard_A3 (4 rdzenie, 7 GB pamięci RAM)                                                                      | Standard_DS3 (4 rdzenie, 14 GB pamięci RAM)                                                                                                                          |
| **Zgodność wersji** | Wersje uruchomiony wstępnie Aktualizacja 2 lub nowszy                                             | Wersje z aktualizacji, 2 lub nowszym                                                                                                  |
| **Dostępność regionu**   | Wszystkie regiony Azure                                                         | Azure regionów, które obsługują Premium miejsca do magazynowania<br></br>Aby uzyskać listę regionów zobacz [obsługiwane regionów dla 8020](#supported-regions-for-8020) |
| **Typ magazynu**          | Używa magazynu standardowego Azure dla lokalnych dyskach<br></br> Dowiedz się, jak [utworzyć konto standardowego magazynu]() | Używa magazynu Premium Azure dla lokalnych dyskach<sup>2</sup> <br></br>Dowiedz się, jak [utworzyć konto Premium miejsca do magazynowania](storage-premium-storage.md#create-and-use-a-premium-storage-account-for-a-virtual-machine-data-disk)                                                               |
| **Wskazówki obciążenie pracą**     | Element poziomu pobieranie plików z kopii zapasowych                                              | Deweloperów w chmurze i przetestować scenariusze, krótki czas oczekiwania, wyższa obciążenia wydajności <br></br>Urządzenie pomocnicze awarii                                                                                            |
 
<sup>1</sup> *wcześniej znana pod nazwą 1100*.

<sup>2</sup> *zarówno 8010, jak i 8020 za pomocą standardowego magazynu Azure dla poziomu chmury. Różnica istnieje tylko w warstwie lokalnych w urządzeniu*.

#### <a name="supported-regions-for-8020"></a>Obsługiwanych regionów dla 8020

Regiony Premium miejsca do magazynowania, które są obecnie obsługiwane dla 8020 są oznaczane znakami tabulacji poniżej. Ta lista jest uaktualniany jako miejsca do magazynowania Premium stał się dostępny w regionach więcej. 

| S. Brak.                                                  | Obecnie obsługiwane w regionach |
|---------------------------------------------------------|--------------------------------|
| 1                                                       | Centralny Stany Zjednoczone                     |
| 2                                                       |  Wschodniej Stany Zjednoczone                       |
| 3                                                       |  Wschodniej USA 2                     |
| 4                                                       | Zachód Stany Zjednoczone                        |
| 5                                                       | Europa Północna                   |
| 6                                                       | Europa Zachodnia                    |
| 7                                                       | Kraje Azji Południowo                 |
| 8                                                       | Wschód Japonii                     |
| 9                                                       | Zachód Japonii                     |
| 10                                                      | Wschód Australia                 |
| 11                                                      | Australia kopiec *           |
| 12                                                      | Wschodnioazjatycki *                     |
| 13                                                      | Południowej centralnej USA *              |

* Premium magazynowania został uruchomiony ostatnio w tych geos.

W tym artykule opisano krok po kroku procesu wdrażania urządzenie wirtualne StorSimple platformy Azure. Po zapoznaniu się w tym artykule, konieczne będzie:

- Zrozumieć, jak urządzenia wirtualnego różni się od urządzenia fizycznego.

- Umożliwia tworzenie i konfigurowanie urządzenia wirtualnego.

- Nawiązywanie połączenia urządzeń wirtualnych.

- Dowiedz się, jak pracować z urządzenia wirtualnego.

Ten samouczek dotyczy wszystkich StorSimple urządzenia wirtualne uruchomiony aktualizacji, 2 lub nowszy. 

## <a name="how-the-virtual-device-differs-from-the-physical-device"></a>Jak urządzenia wirtualnego różni się od urządzenia fizycznego

Urządzenia wirtualnego StorSimple jest tylko do oprogramowania wersji StorSimple, na którym jest uruchomiony w jednym węźle maszyny wirtualnej w programie Microsoft Azure. Urządzenie wirtualne obsługuje scenariuszy odzyskiwania danych, w których nie jest dostępna fizycznie urządzenia, a jest odpowiedni do użycia w poziomie elementu pobierania z kopii zapasowych, odzyskiwanie lokalnego i scenariusze deweloperów i przetestuj chmurze.

#### <a name="differences-from-the-physical-device"></a>Różnice w stosunku do urządzenia fizycznego

W poniższej tabeli pokazano kilka kluczowych różnic między urządzenia wirtualnego StorSimple i urządzenia fizycznego StorSimple.

|                             | Urządzenia fizycznego                                          | Urządzenie wirtualne                                                                            |
|-----------------------------|----------------------------------------------------------|-------------------------------------------------------------------------------------------|
| **Lokalizacja**                    | Znajduje się w obrębie centrum danych.                               | Działa w Azure.                                                                            |
| **Interfejsy**          | Zawiera sześć interfejsy: danych od 0 do 5 danych.                  | Zawiera tylko jedną sieciowej: danych 0.                                        |
| **Rejestracja**                | Zarejestrowane w kroku konfiguracji.                | Rejestracja jest oddzielnego zadania.                                                          |
| **Klucza szyfrowania danych usługi** | Ponownie wygenerować na tym urządzeniu fizycznym, a następnie zaktualizuj urządzenia wirtualnego przy użyciu nowego klucza.           | Nie można odtworzyć z urządzenia wirtualnego. |


## <a name="prerequisites-for-the-virtual-device"></a>Wymagania wstępne dotyczące urządzenia wirtualnego

W poniższych sekcjach opisano wymagania wstępne dotyczące konfiguracji dla swojego urządzenia wirtualnego StorSimple. Przed wdrożeniem urządzenie wirtualne, przejrzyj [Zagadnienia dotyczące zabezpieczeń dotyczące korzystania z urządzenia wirtualnego](storsimple-security.md#storsimple-virtual-device-security).

#### <a name="azure-requirements"></a>Wymagania Azure

Przed postanowień urządzenia wirtualnego, należy wprowadzić następujące wymagania w środowisku usługi Azure:

- Wirtualna urządzenia, [Konfigurowanie wirtualną sieć Azure](../virtual-network/virtual-networks-create-vnet-classic-portal.md). Jeśli przy użyciu magazynu Premium, należy utworzyć wirtualną sieć w Azure region, który pozwala na przechowywanie Premium. Więcej informacji na temat [regiony, które są obecnie obsługiwane dla 8020](#supported-regions-for-8020).
- Zaleca się, aby użyć domyślnego serwera DNS dostarczony przez Azure zamiast określenie własnej nazwy serwera DNS. Jeśli nie jest prawidłową nazwę serwera DNS lub serwera DNS nie będzie mógł poprawnie przekształcania adresów IP, tworzenie urządzenia wirtualnego nie powiedzie się.
- Punkt do witryny i witryny do witryny są opcjonalne, ale nie są wymagane. Jeśli chcesz, możesz skonfigurować te opcje dla bardziej zaawansowanych scenariuszy. 
- [Azure maszyn wirtualnych](../virtual-machines/virtual-machines-linux-about.md) (Serwery hosta) można tworzyć w wirtualnej sieci, która służy wielkość ujawnionego przez urządzenia wirtualnego. Tych serwerach musi spełniać następujące wymagania:                            
    - Być systemu Windows i Linux oraz maszyny wirtualne iSCSI inicjator zainstalowanego oprogramowania.
    - Działać w tej samej sieci wirtualnej wirtualne urządzenie.
    - Można połączyć w miejscu docelowym iSCSI urządzenia wirtualnego za pośrednictwem wewnętrzny adres IP urządzenia wirtualnego.

- Upewnij się, że skonfigurowano obsługę ruchu iSCSI i chmura w tej samej sieci wirtualnej.


#### <a name="storsimple-requirements"></a>Wymagania dotyczące StorSimple

Wprowadź następujące aktualizacje usługi Azure StorSimple przed utworzeniem urządzenie wirtualne:


- Dodawanie [rekordów kontrolki programu access](storsimple-manage-acrs.md) dla maszyny wirtualne, które mają serwerów hosta dla wirtualnego urządzenia.

- Użyj [konta miejsca do magazynowania](storsimple-manage-storage-accounts.md#add-a-storage-account) w tym samym regionie jako urządzenia wirtualnego. Wydajność może spowodować kont przestrzeni dyskowej w różnych regionów. Za pomocą konta standardowe lub miejsce do magazynowania Premium z urządzenia wirtualnego. Więcej informacji na temat tworzenia [standardowego magazynu konto]() lub [konto Premium miejsca do magazynowania](storage-premium-storage.md#create-and-use-a-premium-storage-account-for-a-virtual-machine-data-disk)

- Użyj konta innego miejsca do magazynowania dla Tworzenie wirtualnych urządzenia z używaną dla określonego typu danych. Używanie tego samego konta miejsca do magazynowania może spowodować wydajność.

Upewnij się, że przed rozpoczęciem mają następujące informacje:

- Azure klasyczny portalu konta przy użyciu poświadczeń programu access.

- Kopia klucza szyfrowania danych usługi fizycznie na urządzeniu z systemem.


## <a name="create-and-configure-the-virtual-device"></a>Tworzenie i konfigurowanie urządzenia wirtualnego

Przed wykonaniem tej procedury upewnij się, że komputer spełnia [wymagania wstępne dotyczące urządzenia wirtualnego](#prerequisites-for-the-virtual-device). 

Po utworzeniu wirtualnej sieci, skonfigurowana usługa Menedżera StorSimple i zarejestrowana fizycznie urządzenia StorSimple z usługą, możesz utworzyć i skonfigurować urządzenie wirtualne StorSimple za pomocą następujące czynności. 

### <a name="step-1-create-a-virtual-device"></a>Krok 1: Tworzenie urządzenie wirtualne

Wykonaj poniższe czynności, aby utworzyć urządzenia wirtualnego StorSimple.

[AZURE.INCLUDE [Create a virtual device](../../includes/storsimple-create-virtual-device-u2.md)]

Jeśli tworzenie urządzenia wirtualnego nie powiedzie się, w tym kroku, być może nie ma łączności z Internetem. Aby uzyskać więcej informacji, przejdź do [Rozwiązywanie błędów połączenia z Internetem](#troubleshoot-internet-connectivity-errors) po creatig urządzenie wirtualne.


### <a name="step-2-configure-and-register-the-virtual-device"></a>Krok 2: Konfigurowanie i zarejestrować urządzenie wirtualne

Przed rozpoczęciem tej procedury upewnij się, że masz kopię klucza szyfrowania danych usługi. Klucza szyfrowania danych usługi został utworzony, gdy skonfigurowany pierwszego urządzenia StorSimple i zostały instrukcjami, aby zapisać go w bezpiecznym miejscu. Jeśli nie masz kopię klucza szyfrowania danych usługi, aby uzyskać pomoc należy skontaktować się Support firmy Microsoft.

Wykonaj poniższe czynności, aby skonfigurować i zarejestrować urządzenia wirtualnego StorSimple.
[AZURE.INCLUDE [Configure and register a virtual device](../../includes/storsimple-configure-register-virtual-device.md)]

### <a name="step-3-optional-modify-the-device-configuration-settings"></a>Krok 3: (Opcjonalnie) Modyfikuj ustawienia konfiguracji urządzenia

W poniższej sekcji opisano ustawienia konfiguracji urządzenia wymagane dla StorSimple urządzenia wirtualnego, jeśli chcesz używać CHAP, Menedżer migawkę StorSimple lub zmień hasło administratora urządzenia.

#### <a name="configure-the-chap-initiator"></a>Konfigurowanie inicjator CHAP

Ten parametr zawiera poświadczeń, które urządzenia wirtualnego (docelowa) oczekuje z Inicjator (serwery), które próbujesz uzyskać dostęp do wielkość. Inicjator zapewni CHAP nazwy użytkownika i hasło CHAP identyfikują na urządzeniu podczas tego uwierzytelniania. Aby uzyskać szczegółowe instrukcje przejdź do [Skonfigurowania CHAP dla danego urządzenia](storsimple-configure-chap.md#unidirectional-or-one-way-authentication).

#### <a name="configure-the-chap-target"></a>Konfigurowanie docelowej CHAP

Ten parametr zawiera poświadczeń, których urządzenia wirtualnego używa z włączoną obsługą CHAP inicjator żądania uwierzytelniania wzajemnego lub dwukierunkowym. Wirtualna urządzenia użyje odwrócić CHAP nazwy użytkownika i hasła odwrócić CHAP do identyfikacji na inicjator w trakcie tego procesu uwierzytelniania. Należy zauważyć, że ustawienia wartości docelowej CHAP ustawień globalnych. Po zastosowaniu tych wszystkich wielkość połączony z urządzenia wirtualnego miejsca do magazynowania użyje uwierzytelniania CHAP. Aby uzyskać szczegółowe instrukcje przejdź do [Skonfigurowania CHAP dla danego urządzenia](storsimple-configure-chap.md#bidirectional-or-mutual-authentication).

#### <a name="configure-the-storsimple-snapshot-manager-password"></a>Konfigurowanie hasła Menedżer migawkę StorSimple

Oprogramowanie StorSimple migawkę Menedżera znajduje się na hoście systemu Windows i umożliwia administratorom zarządzanie kopie zapasowe urządzenia StorSimple w formularzu lokalnych i w chmurze migawek.

>[AZURE.NOTE] W przypadku urządzenia wirtualnego hosta systemu Windows jest Azure maszyn wirtualnych.

Podczas konfigurowania urządzenia w Menedżerze migawkę StorSimple, wyświetli się monit o Podaj adres IP urządzenia StorSimple i hasło do uwierzytelnienia urządzenia miejsca do magazynowania. Aby uzyskać szczegółowe instrukcje przejdź do [Konfigurowanie Menedżera migawkę StorSimple hasła](storsimple-change-passwords.md#change-the-storsimple-snapshot-manager-password).

#### <a name="change-the-device-administrator-password"></a>Zmienianie hasła administratora urządzenia 

Podczas uzyskiwania dostępu do urządzenia wirtualnego, korzystając z interfejsu programu Windows PowerShell, trzeba będzie wprowadź hasło administratora urządzenia. Zabezpieczania danych jest wymagane, aby zmienić hasło, przed użyciem urządzenia wirtualnego. Aby uzyskać szczegółowe instrukcje przejdź do [hasło administratora urządzenia Konfiguruj](storsimple-change-passwords.md#change-the-device-administrator-password).

## <a name="connect-remotely-to-the-virtual-device"></a>Nawiązanie połączenia zdalnego urządzenia wirtualnego
Domyślnie nie włączono dostęp zdalny do wirtualnego urządzenia za pomocą interfejsu programu Windows PowerShell. Musisz najpierw włączyć zarządzania zdalnego na urządzenie wirtualne, a następnie włączyć klienta, który będzie używany do wirtualnego urządzenia.

Proces dwuetapowy nawiązywanie połączeń zdalnych jest wyszczególnione poniżej.

### <a name="step-1-configure-remote-management"></a>Krok 1: Konfigurowanie zdalnego zarządzania

Wykonaj poniższe kroki, aby skonfigurować zarządzania zdalnego dla swojego urządzenia wirtualnego StorSimple.

[AZURE.INCLUDE [Configure remote management via HTTP for virtual device](../../includes/storsimple-configure-remote-management-http-device.md)]

### <a name="step-2-remotely-access-the-virtual-device"></a>Krok 2: Zdalny dostęp do urządzenia wirtualnego

Po włączeniu zarządzania zdalnego na stronie Konfiguracja urządzenia StorSimple umożliwia programu Windows PowerShell zdalnych nawiązywanie połączenia z urządzenia wirtualnego z innej maszyn wirtualnych w tej samej sieci wirtualnych; na przykład można połączyć z hosta maszyn wirtualnych już skonfigurowane i używane do łączenia iSCSI. W przypadku większości wdrożeń będzie otwartego publicznej punktu końcowego, aby uzyskać dostęp do hosta maszyn wirtualnych, używanego do uzyskiwania dostępu do urządzenia wirtualnego.

>[AZURE.WARNING] **Aby zwiększyć bezpieczeństwo zalecamy użycie protokołu HTTPS, podczas łączenia się punkty końcowe, a następnie usuń punkty końcowe po zakończeniu usługi zdalnej sesji programu PowerShell.**

Należy wykonać procedury opisane w [Łączenie zdalnie na urządzeniu StorSimple](storsimple-remote-connect.md) konfigurowania zdalnych dla swojego urządzenia wirtualnego.

## <a name="connect-directly-to-the-virtual-device"></a>Podłącz bezpośrednio do urządzenia wirtualnego

W przypadku nawiązywania połączenia bezpośrednio do urządzenia wirtualnego. Jeśli chcesz połączyć bezpośrednio do urządzenia wirtualnego z innego komputera, poza wirtualnej sieci organizacji lub poza środowisko Microsoft Azure, należy utworzyć dodatkowe punkty końcowe, zgodnie z opisem w poniższej procedurze. 

Wykonaj następujące czynności, aby utworzyć punkt końcowy publicznej na tym urządzeniu wirtualną.

[AZURE.INCLUDE [Create public endpoints on a virtual device](../../includes/storsimple-create-public-endpoints-virtual-device.md)]

Firma Microsoft zaleca nawiązywanie połączenia z innym maszyn wirtualnych w tej samej sieci wirtualnej ponieważ tej wskazówki minimalizuje liczbę publicznej punkty końcowe w wirtualnej sieci. Korzystając z tej metody, po prostu połączyć się z komputerem wirtualnych za pośrednictwem sesji pulpitu zdalnego i skonfiguruj tego maszyn wirtualnych do użytku, jak w przypadku innych klienta systemu Windows w sieci lokalnej. Nie musisz Dołącz numer portu publicznej, ponieważ port będzie już znany.

## <a name="work-with-the-storsimple-virtual-device"></a>Praca z urządzenia wirtualnego StorSimple

Teraz, gdy zostanie utworzony i skonfigurowany urządzenia wirtualnego StorSimple, możesz przystąpić do rozpocząć pracę z nim. Pracy z kontenerów głośność, wielkości i zasady kopii zapasowej na urządzeniu wirtualną tak samo jak na urządzeniu fizycznie StorSimple; tylko różnica polega na tym, że należy upewnić się, wybierz wirtualne urządzenie z listy urządzeń. Zapoznaj się z [za pomocą Menedżera StorSimple usługi Zarządzanie urządzenie wirtualne](storsimple-manager-service-administration.md) zapoznać się z procedurami krok po kroku różnych zadań zarządzania wirtualne urządzenie.

W poniższych sekcjach opisano niektóre różnice, które można napotkać podczas pracy z urządzenia wirtualnego.

### <a name="maintain-a-storsimple-virtual-device"></a>Obsługa urządzenia wirtualnego StorSimple

Ponieważ jest to urządzenie tylko do oprogramowania, obsługa dla urządzenia wirtualnego jest minimalne, po porównaniu ich obsługa dla urządzenia fizycznego. Dostępne są następujące opcje:

- **Aktualizacje oprogramowania** — możesz wyświetlić Data ostatniej aktualizacji oprogramowania, wraz z wszelkimi aktualizowanie komunikaty o stanie. Przycisk **skanowanie aktualizacji** w dolnej części strony umożliwia wykonać skanowanie ręczne, jeśli chcesz sprawdzać dostępność nowych aktualizacji. Jeśli są dostępne aktualizacje, kliknij pozycję **Zainstaluj aktualizacje** do zainstalowania. Wirtualne urządzenie jest tylko jeden interfejs, oznacza to, że będzie przerwaniu połączenia z usługą drobne aktualizacje są stosowane. Urządzenia wirtualnego będzie Zamknij i uruchom ponownie (Jeśli to konieczne), aby zastosować wszystkie aktualizacje, które zostały wydane. Szczegółową procedurę przejdź do [zaktualizować urządzenie](storsimple-update-device.md#install-regular-updates-via-the-azure-classic-portal).
- **Pomocy technicznej pakietu** — możesz tworzyć i przekazywanie pakietu pomocy technicznej, aby ułatwić rozwiązywanie problemów z urządzeniem wirtualnych Support firmy Microsoft. Szczegółową procedurę przejdź do [tworzenia i zarządzania nimi pakiet pomocy technicznej](storsimple-create-manage-support-package.md).

### <a name="storage-accounts-for-a-virtual-device"></a>Kont miejsca do magazynowania dla urządzenie wirtualne

Konta przestrzeni dyskowej są tworzone do użycia przez usługę Menedżera StorSimple, urządzenia wirtualnego i urządzenia fizycznego. Po utworzeniu konta miejsca do magazynowania, zaleca się użycie identyfikator regionu w polu przyjazna nazwa, aby upewnić się, że regionu jest spójne wszystkie składniki systemu. Wirtualna urządzenia ważne jest, że wszystkie składniki znajdować się w tym samym regionie, aby zapobiec występują problemy z wydajnością.

Szczegółową procedurę Przejdź w celu [dodania miejsca do magazynowania](storsimple-manage-storage-accounts.md#add-a-storage-account).

### <a name="deactivate-a-storsimple-virtual-device"></a>Dezaktywowanie urządzenia wirtualnego StorSimple

Dezaktywowanie urządzenie wirtualne usuwa maszyn wirtualnych i zasoby utworzenia został zainicjowany. Po zdezaktywowaniu urządzenia wirtualnego nie można przywrócić do poprzedniego stanu. Przed dezaktywowaniu urządzenia wirtualnego upewnij się zatrzymać lub usunąć klientów i hostów zależne od siebie.

Dezaktywowanie urządzenia wirtualnego powoduje następujące działania:

- Urządzenie wirtualne zostanie usunięty.

- Dysk systemu operacyjnego i dyski danych utworzona dla urządzenia wirtualnego zostaną usunięte.

- Usług hostingowych i wirtualną sieć utworzone podczas inicjowania obsługi administracyjnej są zachowywane. Jeśli nie są używane, należy usunąć je ręcznie.

- Migawki chmury utworzoną dla urządzenia wirtualnego są zachowywane.

Aby uzyskać procedury krok po kroku, przejdź do [Dezaktywuj i usuń urządzenie StorSimple](storsimple-deactivate-and-delete-device.md).

Po zainicjowaniu urządzenia wirtualnego są wyświetlane jako nieaktywne na stronie usługi StorSimple menedżera, możesz usunąć wirtualne urządzenie z listy urządzeń na stronie **urządzenia** .


### <a name="start-stop-and-restart-a-virtual-device"></a>Rozpocznij, Zatrzymaj i uruchom ponownie urządzenie wirtualne
W przeciwieństwie do urządzenia fizycznego StorSimple istnieje Brak zasilania na lub power wyłączanie przycisku do umieszczenia na urządzeniu virtual StorSimple. Jednak czasami może występować okazji której chcesz zatrzymać, a następnie ponownie uruchom urządzenie wirtualne. Na przykład niektóre aktualizacje mogą wymagać ponownego maszyn wirtualnych aby ukończyć proces aktualizacji. Najprostszym sposobem uruchamiania, Zatrzymaj i uruchom ponownie urządzenie wirtualne jest użycie konsoli zarządzania maszyn wirtualnych.

**Po wyświetleniu konsoli zarządzania stanu wirtualne urządzenie działa, ponieważ jest uruchomiony domyślnie po jego utworzeniu.** Można uruchomić, zatrzymać i ponownie uruchomić maszyny wirtualnej w dowolnym momencie.

[AZURE.INCLUDE [Stop and restart virtual device](../../includes/storsimple-stop-restart-virtual-device.md)]

### <a name="reset-to-factory-defaults"></a>Przywracanie domyślnych ustawień fabrycznych

Jeśli zdecydujesz się po prostu chcesz rozpoczęcie z urządzeniem wirtualnych, po prostu Dezaktywuj i usuń go, a następnie utwórz nową. Tak jak podczas zresetować jest fizycznie urządzenia nowym urządzeniu wirtualnych nie będą mieć aktualizacje zainstalowane; w związku z tym upewnij się sprawdzić dostępność aktualizacji przed użyciem.


## <a name="fail-over-to-the-virtual-device"></a>Nie na urządzenie wirtualne

Odzyskiwanie (DR) jest jednym z najważniejszych scenariusze urządzenia wirtualnego StorSimple została zaprojektowana dla. W tym scenariuszu urządzenia StorSimple fizycznego lub całego centrum danych może nie być dostępna. Na szczęście urządzenia wirtualnego umożliwia przywracanie operacje w innej lokalizacji. Podczas DR kontenerów głośność za pomocą urządzenia źródła zmienić własność i są przenoszone do urządzenia wirtualnego. Wymagania wstępne dotyczące DR to że urządzenia wirtualnego została utworzona i skonfigurowana, wszystkie wielkość w kontenerze głośność jest w trybie offline i pojemnika zbiorowego ma skojarzony migawkę chmury.

>[AZURE.NOTE] 
>
> - Używając urządzenia wirtualnego jako urządzenie pomocnicze dla DR, pamiętać, że 8010 ma 30 TB miejsca w standardowej i 8020 ma 64 TB miejsca Premium. Wyższa urządzenia wirtualnego 8020 wydajność może być bardziej odpowiednie dla scenariusza DR.
> - Nie można pracy awaryjnej lub klonowanie z urządzeniem z systemem aktualizacji 2 do urządzenia oprogramowanie 1 sprzed aktualizacji. Jednak może zakończyć niepowodzeniem na urządzeniu działa aktualizacji 2 na urządzeniu z systemem aktualizacji 1 (1.1 lub 1.2)

Szczegółową procedurę przejdź do [przełączanie awaryjne do urządzenia wirtualnego](storsimple-device-failover-disaster-recovery.md#fail-over-to-a-storsimple-virtual-device).
 

## <a name="shut-down-or-delete-the-virtual-device"></a>Zamykanie lub usunąć urządzenie wirtualne

Jeśli wcześniej skonfigurowane i używane StorSimple urządzenia wirtualnego ale teraz chcesz zatrzymać naliczania opłat obliczeń dla tej funkcji, można zamknąć wirtualne urządzenie. Zamykanie urządzenia wirtualnego nie powoduje usunięcia jego systemu operacyjnego lub dyski danych w magazynie. Zatrzymywanie opłaty naliczane od Twojej subskrypcji, ale będzie nadal opłaty miejsca do magazynowania dla dysków systemu operacyjnego i danych.

Jeśli usuniesz lub zamknięcia wirtualnego urządzenia, pojawią się **Offline** na stronie urządzeń usługę Menedżer StorSimple. Możesz dezaktywować lub usunąć urządzenie, jeśli chcesz także usunąć kopie zapasowe utworzone przez urządzenia wirtualnego. Aby uzyskać więcej informacji zobacz [Dezaktywuj i usuń urządzenie StorSimple](storsimple-deactivate-and-delete-device.md).

[AZURE.INCLUDE [Shut down a virtual device](../../includes/storsimple-shutdown-virtual-device.md)]

[AZURE.INCLUDE [Delete a virtual device](../../includes/storsimple-delete-virtual-device.md)]

   
## <a name="troubleshoot-internet-connectivity-errors"></a>Rozwiązywanie problemów z błędami połączeń internetowych 

Podczas tworzenia urządzenia wirtualnego Jeśli istnieje nie łączność z Internetem, zakończy się niepowodzeniem kroku tworzenia. Aby rozwiązać, jeśli błąd jest ze względu na łączność z Internetem, wykonaj następujące czynności w portalu klasyczny Azure:

1. Tworzenie maszyny wirtualnej systemu Windows server 2012 w Azure. Tej maszyny wirtualnej należy używać tego samego konta miejsca do magazynowania, VNet i podsieci używane przez urządzenie wirtualną. Jeśli już masz istniejącego hosta Windows Server Azure za pomocą tego samego konta miejsca do magazynowania, Vnet i podsieci, możesz go użyć rozwiązywać łączność z Internetem.
2. Zdalnego do dzienników do maszyny wirtualnej utworzony w poprzednim kroku. 
3. Otwórz okno polecenia wewnątrz maszyny wirtualnej (Win + R, a następnie wpisz `cmd`).
4. Uruchom następujące polecenia cmd w wierszu.

    `nslookup windows.net`

5. Jeśli `nslookup` ulegnie błąd połączenia internetowego uniemożliwia rejestrowanie z usługą Menedżera StorSimple wirtualnego urządzenia. 
6. Wprowadź wymagane zmiany wirtualnej sieci w celu zapewnienia, że urządzenie wirtualne mieli dostęp do witryn Azure, takie jak "windows.net".

## <a name="next-steps"></a>Następne kroki

- Dowiedz się, jak [używać usługi Menedżera StorSimple Zarządzanie urządzenie wirtualne](storsimple-manager-service-administration.md).
 
- Opis sposobu [przywracania woluminu StorSimple z zestawu kopii zapasowych](storsimple-restore-from-backup-set.md). 

