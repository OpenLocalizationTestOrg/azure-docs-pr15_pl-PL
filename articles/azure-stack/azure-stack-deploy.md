<properties
    pageTitle="Przed wdrożeniem Zapewnić stos Azure | Microsoft Azure"
    description="Wyświetlanie wymagań środowiska i sprzętu do zatwierdzenia Koncepcji stos Azure (administrator usługi)."
    services="azure-stack"
    documentationCenter=""
    authors="ErikjeMS"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/12/2016"
    ms.author="erikje"/>

# <a name="azure-stack-deployment-prerequisites"></a>Wymagania wstępne dotyczące Azure stos wdrażania

Przed wdrożeniem Azure stos Zapewnić ([Koncepcji](azure-stack-poc.md)), upewnij się, że Twój komputer spełnia następujące wymagania.
Wymagania dotyczące rozmieszczania Technical Preview 2 do zatwierdzenia Koncepcji są takie same jak wymagane dla Technical Preview 1. W związku z tym możesz użyć tego samego sprzętu, używanym do poprzedniego podglądu jedno pole.

## <a name="hardware"></a>Sprzętowe

| Składnik | Minimalna  | Zalecane |
|---|---|---|
| Dysków: System operacyjny | 1 dysk systemu operacyjnego z co najmniej 200 GB dostępne dla partycją systemową (SSD lub dysk twardy) | 1 dysk systemu operacyjnego z co najmniej 200 GB dostępne dla partycją systemową (SSD lub dysk twardy) |
| Dysków: ogólne dane Zapewnić stos Azure | 4 dysków. Każdy dysk zawiera co najmniej 140 GB wydajność (SSD lub dysk twardy). Wszystkie dostępne dyski będą używane. | 4 dyski. Każdy dysk zawiera co najmniej 250 GB wydajność (SSD lub dysk twardy). Wszystkie dostępne dyski będą używane.|
| Obliczanie: Procesor | Podwójny procesorem: 12 rdzenie fizycznym (razem)  | Podwójny procesorem: 16 rdzenie fizycznym (razem) |
| Obliczanie: pamięci | 96 GB PAMIĘCI RAM  | 128 GB PAMIĘCI RAM |
| Obliczanie: BIOS | Włączono funkcji Hyper-V (z obsługą DESKI)  | Włączono funkcji Hyper-V (z obsługą DESKI) |
| Sieć: NIC | Windows Server 2012 R2 certyfikacji wymagane dla NIC; nie specjalizowanych funkcji wymagane | Windows Server 2012 R2 certyfikacji wymagane dla NIC; nie specjalizowanych funkcji wymagane |
| Sprzętowego logo certyfikatu | [Certyfikowane dla systemu Windows Server 2012 R2](http://windowsservercatalog.com/results.aspx?&chtext=&cstext=&csttext=&chbtext=&bCatID=1333&cpID=0&avc=79&ava=0&avq=0&OR=1&PGS=25&ready=0) |[Certyfikowane dla systemu Windows Server 2012 R2](http://windowsservercatalog.com/results.aspx?&chtext=&cstext=&csttext=&chbtext=&bCatID=1333&cpID=0&avc=79&ava=0&avq=0&OR=1&PGS=25&ready=0)|

**Konfiguracja dysku danych:** Wszystkie dyski danych musi być tego samego typu (wszystkie skojarzenia zabezpieczeń lub wszystkich SATA) i pojemności. Użycie dysków skojarzeń zabezpieczeń dysków musi być dołączone za pośrednictwem pojedynczej ścieżki (podano nie MPIO, obsługa wiele ścieżek).

**Opcje konfiguracji HBA**
 
- (Preferowane) Prosta HBA
- RAID HBA — karta musi być skonfigurowany w trybie "przechodzą przez".
- HBA RAID — dysków należy skonfigurować jako pojedynczy dysk, RAID 0

**Obsługiwane bus i multimediów wpisz kombinacje**

-   DYSK TWARDY

-   DYSK TWARDY SKOJARZENIA ZABEZPIECZEŃ

-   DYSK TWARDY RAID

-   RAID SSD (Jeśli typ multimediów jest nieokreślony nieznany\*)

-   SATA SSD + DYSK TWARDY

-   SKOJARZENIA ZABEZPIECZEŃ SSD + DYSK TWARDY SKOJARZEŃ ZABEZPIECZEŃ

\*Kontrolery RAID bez możliwości przekazująca nie może rozpoznać typ multimediów. Takie kontrolery oznaczy zarówno dysk twardy, jak i SSD nieokreślone. W takim przypadku SSD zostanie użyty jako wygenerowanie zamiast pamięci podręcznej urządzenia. W związku z tym należy wdrożyć Zapewnić Microsoft Azure stos na tych twarde SSD.

**Przykład HBA**: LSI 9207 8i, LSI-9300-8i lub LSI 9265 8i w trybie przekazujące

Dostępne są konfiguracje OEM próbki.

## <a name="operating-system"></a>System operacyjny

| | **Wymagania dotyczące**  |
|---|---|
| **Wersja systemu operacyjnego** | Windows Server 2012 R2 lub nowszy. Wersja systemu operacyjnego nie jest krytyczne przed rozpoczęciem wdrażania, jak będzie uruchomić komputer hosta w wirtualny dysk twardy, który znajduje się w stos Azure instalacji zip. System operacyjny i wszystkie poprawki wymagane są już zintegrowany z obrazu. Nie używaj klucze aktywować wszystkie wystąpienia Windows Server używane w Zapewnić.|

## <a name="deployment-requirements-check-tool"></a>Narzędzie Sprawdzanie wymagań wdrażania

Po zainstalowaniu systemu operacyjnego, umożliwia [Sprawdzanie wdrażania Azure stos Technical Preview 2](https://gallery.technet.microsoft.com/Deployment-Checker-for-50e0f51b) upewnij się, że sprzęt spełnia wszystkie wymagania.



## <a name="microsoft-azure-active-directory-accounts"></a>Konta usługi Microsoft Azure Active Directory

Wdrożenie programu Microsoft Azure stos Zapewnić musi być połączony z Azure. W związku z tym należy przygotować konta usługi Microsoft Azure Active Directory przed uruchomieniem wdrożenia skrypt programu PowerShell. To konto staje się administratora globalnego dla dzierżawy usługi Azure Active Directory. Będzie można używać do obsługi administracyjnej i delegowanie aplikacji oraz usług główne dla wszystkich usług Azure stos współdziałające z usługi Azure Active Directory i interfejsu API grafiki. Również posłuży go jako właściciel subskrypcji dostawcy domyślnego (które później możesz zmienić). Możesz Zaloguj się do portalu administracyjnego systemu Azure stos, przy użyciu tego konta.

1. Utwórz konto Azure AD administratora katalogu dla co najmniej jednej usługi Azure Active Directory. Jeśli już istnieje, możesz go użyć. W przeciwnym razie możesz utworzyć je bezpłatnie w [http://azure.microsoft.com/en-us/pricing/free-trial/](http://azure.microsoft.com/pricing/free-trial/) (w Chinach, odwiedź stronę <http://go.microsoft.com/fwlink/?LinkID=717821> zamiast.)

    Zapisz te poświadczenia do użycia w kroku 6 [uruchomić skrypt wdrożenia programu PowerShell](azure-stack-run-powershell-script.md#run-the-powershell-deployment-script). To konto *administratora usługi* można konfigurować i zarządzać chmury zasobów, konta użytkowników, plany dzierżawy, przydziałów i cennik. W portalu ich tworzenie witryny sieci Web chmury, chmury prywatne maszyn wirtualnych, tworzenie planów i zarządzanie subskrypcjami użytkownika.

2. [Utwórz](azure-stack-add-new-user-aad.md) co najmniej jeden konta, aby zalogować się do zatwierdzenia Koncepcji stos Azure jako dzierżawy.

  	| **Konto usługi Azure Active Directory**  | **Obsługiwane?** |
  	|---|---| 
  	| Konto służbowe z prawidłową publicznej subskrypcji Azure  | Tak |
  	| Account Microsoft przy użyciu prawidłowych publicznej subskrypcji Azure  | Brak |
  	| Konto służbowe z subskrypcją Azure Chinach prawidłowe  | Tak |
  	| Konto służbowe z prawidłową nam dla instytucji rządowych Azure subskrypcji  | Tak |


## <a name="network"></a>Sieci

### <a name="switch"></a>Przełączanie

Jeden dostępny port przełącznika dla komputera, aby Zapewnić.  

Na komputerze Zapewnić stos Azure obsługuje nawiązywanie połączeń z portu dostępu przełącznika lub kanału. Nie specjalizowanych funkcji są wymagane przełącznika. Jeśli korzystasz z portem kanału lub jeśli musisz skonfigurować identyfikator VLAN, musisz podać Identyfikatora sieci jako parametr wdrożenia. Można zobaczyć przykłady [listę parametrów wdrożenia](azure-stack-run-powershell-script.md).

### <a name="subnet"></a>Podsieć

Nie można nawiązać komputera Zapewnić podsieci następujące czynności:
- 192.168.200.0/24
- 192.168.100.0/27
- 192.168.101.0/26
- 192.168.102.0/24
- 192.168.103.0/25
- 192.168.104.0/25

Te podsieci są zarezerwowane dla sieci wewnętrznych w środowisku Microsoft Azure stos Zapewnić.

### <a name="ipv4ipv6"></a>IPv4 i IPv6

Tylko protokołu IPv4 jest obsługiwana. Nie można utworzyć sieci IPv6.

### <a name="dhcp"></a>DHCP

Upewnij się, że NIC połączony z sieci jest dostępna serwerze. Jeśli DHCP nie jest dostępny, należy przygotować dodatkowe statyczne sieć IPv4 poza używaną przez hosta. Należy podać adres IP i bramą jako parametr wdrożenia. Można zobaczyć przykłady [listę parametrów wdrożenia](azure-stack-run-powershell-script.md).

### <a name="internet-access"></a>Dostęp do Internetu

Stos Azure wymaga dostęp do Internetu, bezpośrednio lub za pośrednictwem przezroczysty serwera proxy. Stos Azure nie obsługuje konfiguracji serwera proxy sieci web, aby umożliwić dostęp do Internetu. Zarówno adresu IP hosta, jak i nowych adresów IP przydzielonych do MAS BGPNAT01 (przez DHCP lub statyczny adres IP) muszą być mieli dostęp do Internetu. Porty 80 i 443 są używane w obszarze domeny graph.windows.net i login.windows.net.

### <a name="telemetry"></a>Telemetrycznego

Do obsługi przepływu danych telemetrycznych, na porcie 443 (HTTPS) musi być otwarty w Twojej sieci. Punkt końcowy klienta jest https://vortex-win.data.microsoft.com.


## <a name="next-steps"></a>Następne kroki

[Pobieranie pakietu wdrażania Zapewnić stos Azure](https://azure.microsoft.com/overview/azure-stack/try/?v=try)

[Wdrażanie Azure stos zatwierdzania Koncepcji](azure-stack-run-powershell-script.md)
