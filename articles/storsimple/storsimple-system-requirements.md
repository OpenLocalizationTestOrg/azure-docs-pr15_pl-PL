<properties
   pageTitle="Wymagania systemowe StorSimple | Microsoft Azure"
   description="W tym artykule opisano oprogramowania, sieci i wymagań dotyczących wysokiej dostępności oraz Najważniejsze wskazówki dotyczące rozwiązania Microsoft Azure StorSimple."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="08/31/2016"
   ms.author="alkohli"/>

# <a name="storsimple-software-high-availability-and-networking-requirements"></a>Oprogramowanie StorSimple, wysokiej dostępności i wymagania sieciowe

## <a name="overview"></a>Omówienie

Witamy w usłudze Microsoft Azure StorSimple. W tym artykule opisano wymagania systemowe i najważniejsze wskazówki dotyczące urządzenia StorSimple oraz dla klientów miejsca do magazynowania uzyskiwania dostępu do urządzenia. Zalecamy zapoznanie się informacje starannie przed wdrożenie systemu StorSimple, a następnie odnoszą się do niego stosownie do potrzeb podczas wdrażania i kolejnych operacji.

Wymagania systemowe obejmują:

- **Wymagania dotyczące oprogramowania dla klientów miejsca do magazynowania** — w tym artykule opisano obsługiwane systemy operacyjne oraz wszelkie dodatkowe wymagania tych systemów operacyjnych.
- **Sieć wymagania dotyczące urządzenia StorSimple** - zawiera informacje dotyczące portów, które muszą być otwarte w zaporze, aby umożliwić iSCSI, chmury lub danych związanych z zarządzaniem.
- **Wysoce wymagania dotyczące StorSimple** — opis wymagań wysokiej dostępności i najważniejsze wskazówki dotyczące komputera StorSimple urządzenia i hosta. 


## <a name="software-requirements-for-storage-clients"></a>Wymagania dotyczące oprogramowania dla klientów miejsca do magazynowania

Są następujące wymagania dotyczące oprogramowania dla klientów miejsca do magazynowania, które urządzenia StorSimple.

| Obsługiwane systemy operacyjne | Wersja wymagana | Dodatkowe wymagania i notatek |
| --------------------------- | ---------------- | ------------- |
| System Windows Server              | 2008R2 Z DODATKIEM SP1, 2012, 2012R2 |StorSimple iSCSI wielkości są obsługiwane do użytku na tylko następujące typy dysku systemu Windows:<ul><li>Woluminu prostego na dysku podstawowym</li><li>Proste i lustrzane woluminu na dysku dynamicznym</li></ul>Funkcje ODX i systemu Windows Server 2012 cienkie-inicjowania obsługi administracyjnej są obsługiwane, jeśli używasz głośność iSCSI StorSimple.<br><br>StorSimple można tworzyć znacznie ustanawianie i pełni ustanawianie wielkości. Nie można go utworzyć częściowo ustanawianie wielkości.<br><br>Ponowne formatowanie woluminu znacznie ustanawianie może zająć dużo czasu. Zalecamy usunięcie głośności, a następnie utworzyć nową zamiast formatowania. Jeśli nadal woli jednak zmienić głośność:<ul><li>Uruchom następujące polecenie przed sformatowanie w celu uniknięcia opóźnień odzyskiwanie miejsca: <br>`fsutil behavior set disabledeletenotify 1`</br></li><li>Po ukończeniu formatowania, użyj następującego polecenia, aby ponownie włączyć odzyskania miejsca:<br>`fsutil behavior set disabledeletenotify 0`</br></li><li>Zgodnie z opisem w [KB 2878635](https://support.microsoft.com/kb/2870270) na komputerze z systemem Windows Server, należy zastosować poprawkę systemu Windows Server 2012.</li></ul></li></ul></ul> Jeśli konfigurujesz Menedżera migawkę StorSimple lub karty StorSimple dla programu SharePoint, przejdź do [wymagania dotyczące oprogramowania dla składników opcjonalnych](#software-requirements-for-optional-components).|
| VMWare ESX | 5.5 i 6.0 | Obsługiwane za pomocą VMWare vSphere jako klient. Funkcja VAAI blok jest obsługiwana z VMware vSphere na urządzeniach StorSimple.
| Linux RHEL-CentOS | 5, 6 i 7 | Obsługa klientów iSCSI Linux Otwórz iSCSI inicjator wersji 5, 6 i 7. |
| Linux | SUSE Linux 11 | |
 > [AZURE.NOTE] IBM AIX nie jest obecnie obsługiwana z StorSimple.

## <a name="software-requirements-for-optional-components"></a>Wymagania dotyczące oprogramowania dla składników opcjonalnych

Są następujące wymagania dotyczące oprogramowania dla opcjonalne składniki StorSimple (Menedżer migawkę StorSimple i karta StorSimple dla programu SharePoint).

| Składnik | Platformy hosta | Dodatkowe wymagania i notatek |
| --------------------------- | ---------------- | ------------- |
| Menedżer migawkę StorSimple | Windows Server 2008R2 z dodatkiem SP1, 2012, 2012R2 | Użyj StorSimple migawkę menedżera w systemie Windows Server jest wymagana, tworzenie kopii zapasowych i przywracanie lustrzanych dysków dynamicznych oraz wszelkie spójną z aplikacją kopie zapasowe.<br> Menedżer migawkę StorSimple jest obsługiwana tylko w systemie Windows Server 2008 R2 z dodatkiem SP1 (wersja 64-bitowa), Windows 2012 R2 i Windows Server 2012.<ul><li>Jeśli korzystasz z okna Server 2012, należy zainstalować .NET 3.5 — 4,5, przed zainstalowaniem Menedżera migawkę StorSimple.</li><li>Jeśli korzystasz z systemu Windows Server 2008 R2 z dodatkiem SP1, należy zainstalować Windows Management Framework 3.0, przed zainstalowaniem Menedżera migawkę StorSimple.</li></ul> |
| Karta StorSimple dla programu SharePoint | Windows Server 2008R2 z dodatkiem SP1, 2012, 2012R2 |<ul><li>Karta StorSimple dla programu SharePoint jest obsługiwana tylko w programie SharePoint 2010 i SharePoint 2013.</li><li>SPZ wymaga programu SQL Server Enterprise Edition, w wersji 2008 R2 lub 2012.</li></ul>|

## <a name="networking-requirements-for-your-storsimple-device"></a>Wymagania dotyczące urządzenia StorSimple sieci

Urządzenie StorSimple to urządzenie zablokowany. Jednak porty muszą być otwarte w zaporze, aby umożliwić iSCSI, chmura i danych związanych z zarządzaniem. W poniższej tabeli wymieniono porty, które muszą być otwarte w zaporze. W tej tabeli *w* lub *ruchu przychodzącego* odwołuje się do kierunku, w którym przychodzących żądań dostępu do urządzenia. Odwołuje *się* lub *ruchu wychodzącego* w kierunku, w którym urządzenia StorSimple wysyła poza rozmieszczania danych zewnętrznych,: na przykład ruchu wychodzącego z Internetem.

| Port nr<sup>1,2</sup> | Lub | Zakres portu | Wymagane | Notatki |
|------------------------|-----------|------------|----------|-------|
|Port TCP 80 (HTTP)<sup>3</sup>|  W nowym oknie |  WAN | Brak |<ul><li>Port wyjściowy umożliwia dostęp do Internetu pobrać aktualizacje.</li><li>Serwer proxy ruchu wychodzącego sieci web jest użytkownikiem można konfigurować.</li><li>Aby zezwolić na aktualizacje systemu, ten port także należy otworzyć kontrolera stałe adresy IP.</li></ul> |
|Port TCP 443 (HTTPS)<sup>3</sup>| W nowym oknie | WAN | Tak |<ul><li>Port wyjściowy jest używana do uzyskiwania dostępu do danych w chmurze.</li><li>Serwer proxy ruchu wychodzącego sieci web jest użytkownikiem można konfigurować.</li><li>Aby zezwolić na aktualizacje systemu, ten port także należy otworzyć kontrolera stałe adresy IP.</li><li>Ten port jest też używany na obu kontrolerach dla śmieci.</li></ul>|
|UDP 53 (DNS) | W nowym oknie | WAN | W niektórych przypadkach; Zobacz uwagi. |Ten port jest wymagany tylko wtedy, gdy korzystasz z serwera DNS internetowych. |
| UDP 123 (NTP) | W nowym oknie | WAN | W niektórych przypadkach; Zobacz uwagi. |Ten port jest wymagany tylko wtedy, gdy korzystasz z serwera NTP internetowych. |
| PORT TCP 9354 | W nowym oknie | WAN | Tak |Port wyjściowy jest używany przez urządzenie StorSimple można komunikować się z usługą Menedżera StorSimple. |
| 3260 (iSCSI) | W | SIECI LAN | Brak | Ten port jest używany przez iSCSI dostępu do danych.|
| 5985 | W | SIECI LAN | Brak | Ruch przychodzący port jest używany przez Menedżera migawkę StorSimple komunikowanie się z urządzeniem StorSimple.<br>Ten port jest także używany podczas zdalnego łączenia się programu Windows PowerShell dla StorSimple za pomocą protokołu HTTP. |
| 5986 | W | SIECI LAN | Brak | Ten port jest używany podczas zdalnego łączenia się programu Windows PowerShell dla StorSimple za pomocą protokołu HTTPS. |

<sup>1</sup> nie porty przychodzące muszą być otwarte na publicznego Internetu.

<sup>2</sup> Jeśli wiele portów wykonać konfiguracji bramy, kolejności routingu ruchu wychodzącego będzie można ustalić na podstawie wysyłkowej port opisane w [routingu portu](#routing-metric), poniżej.

<sup>3</sup> kontroler stałe adresy IP na urządzeniu StorSimple musi być routingu i mógł połączyć się z Internetem. Stałe adresy IP są używane do obsługi aktualizacji do urządzenia. Jeśli kontrolerów urządzeń nie można połączyć się z Internetem za pomocą stałych adresy IP, nie można zaktualizować urządzenie StorSimple.

> [AZURE.IMPORTANT] Upewnij się, zapory nie modyfikować ani odszyfrowywanie ruch SSL między urządzeniem StorSimple i Azure.

### <a name="url-patterns-for-firewall-rules"></a>Adres URL deseni dla reguły zapory

Administratorzy sieci często można skonfigurować reguły zapory zaawansowane na podstawie wzorców adresów URL, aby odfiltrować przychodzącego i ruchu wychodzącego. Urządzenia StorSimple i usługa Menedżer StorSimple zależą od innych aplikacji firmy Microsoft, takich jak Bus usługi Azure Azure Active Directory kontrola dostępu, kont miejsca do magazynowania i serwery Microsoft Update. Desenie adresu URL skojarzone z nich może służyć do skonfiguruje reguły zapory. Należy zrozumieć, że wzorców adresów URL skojarzone z nich można zmienić. Wymaga to co z kolei administratorem sieci, aby monitorować i aktualizować reguły zapory dla programu StorSimple jako i w razie potrzeby.

Zaleca się, że możesz ustawić reguły zapory dla ruchu wychodzącego, oparte na StorSimple liberally stałe adresy IP, w większości przypadków. Jednak może zawierać poniższe informacje, aby ustawić reguły zapory zaawansowanej, które są potrzebne do utworzenia bezpiecznych środowiskach.

> [AZURE.NOTE] Urządzenia (źródło) adresy IP zawsze powinna być równa wszystkich interfejsów sieci włączone. Miejsce docelowe adresy IP powinna być równa [zakresów adresów IP centrum danych Azure](https://www.microsoft.com/en-us/download/confirmation.aspx?id=41653).

#### <a name="url-patterns-for-azure-portal"></a>Adres URL deseni dla Azure portal
| Wzór adresu URL                                                      | Składnik i funkcji                                           | Adresy IP urządzenia                           |
|------------------------------------------------------------------|---------------------------------------------------------------|-----------------------------------------|
| `https://*.storsimple.windowsazure.com/*`<br>`https://*.accesscontrol.windows.net/*`<br>`https://*.servicebus.windows.net/*`   | Usługa Menedżera StorSimple<br>Usługa kontroli dostępu<br>Usługa Azure Bus| Interfejsy sieci korzystających z chmury        |
|`https://*.backup.windowsazure.com`|Rejestracja urządzenia| Tylko dane 0|
|`http://crl.microsoft.com/pki/*`<br>`http://www.microsoft.com/pki/*`|Odwołania certyfikatu |Interfejsy sieci korzystających z chmury |
| `https://*.core.windows.net/*` <br>`https://*.data.microsoft.com`<br>`http://*.msftncsi.com` | Konta magazynu platformy Azure i monitorowania | Interfejsy sieci korzystających z chmury        |
| `http://*.windowsupdate.microsoft.com`<br>`https://*.windowsupdate.microsoft.com`<br>`http://*.update.microsoft.com`<br> `https://*.update.microsoft.com`<br>`http://*.windowsupdate.com`<br>`http://download.microsoft.com`<br>`http://wustat.windows.com`<br>`http://ntservicepack.microsoft.com`| Serwery Microsoft Update<br>                             | Kontroler tylko stałe adresy IP               |
| `http://*.deploy.akamaitechnologies.com`                         |Sieci CDN Akamai |Kontroler tylko stałe adresy IP   |
| `https://*.partners.extranet.microsoft.com/*`                    | Pakiet pomocy technicznej                                                  | Interfejsy sieci korzystających z chmury        |

#### <a name="url-patterns-for-azure-government-portal"></a>Adres URL deseni dla portal Azure dla instytucji rządowych
| Wzór adresu URL                                                      | Składnik i funkcji                                           | Adresy IP urządzenia                           |
|------------------------------------------------------------------|---------------------------------------------------------------|-----------------------------------------|
| `https://*.storsimple.windowsazure.us/*`<br>`https://*.accesscontrol.usgovcloudapi.net/*`<br>`https://*.servicebus.usgovcloudapi.net/*`   | Usługa Menedżera StorSimple<br>Usługa kontroli dostępu<br>Usługa Azure Bus| Interfejsy sieci korzystających z chmury        |
|`https://*.backup.windowsazure.us`|Rejestracja urządzenia| Tylko dane 0|
|`http://crl.microsoft.com/pki/*`<br>`http://www.microsoft.com/pki/*`|Odwołania certyfikatu |Interfejsy sieci korzystających z chmury |
| `https://*.core.usgovcloudapi.net/*` <br>`https://*.data.microsoft.com`<br>`http://*.msftncsi.com` | Konta magazynu platformy Azure i monitorowania | Interfejsy sieci korzystających z chmury        |
| `http://*.windowsupdate.microsoft.com`<br>`https://*.windowsupdate.microsoft.com`<br>`http://*.update.microsoft.com`<br> `https://*.update.microsoft.com`<br>`http://*.windowsupdate.com`<br>`http://download.microsoft.com`<br>`http://wustat.windows.com`<br>`http://ntservicepack.microsoft.com`| Serwery Microsoft Update<br>                             | Kontroler tylko stałe adresy IP               |
| `http://*.deploy.akamaitechnologies.com`                         |Sieci CDN Akamai |Kontroler tylko stałe adresy IP   |
| `https://*.partners.extranet.microsoft.com/*`                    | Pakiet pomocy technicznej                                                  | Interfejsy sieci korzystających z chmury        |

### <a name="routing-metric"></a>Metryki routingu

Metryki routingu jest skojarzony z interfejsów i bramy, która kierowania danych do określonej sieci. Metrykę routingu jest używana przez protokół routingu do obliczania najlepsze ścieżkę do podanej lokalizacji docelowej, jeśli dowie się, że istnieje wiele ścieżek do tego samego miejsca docelowego. Niższa metryki routingu, wyższa preferencji.

W kontekście StorSimple Jeśli wiele interfejsów sieciowych i bram są skonfigurowane do ruchu w kanale metryka routingu wejścia Odtwórz, aby określić względną kolejność, w jakiej interfejsy uzyskiwanie używane. Metryka routingu nie można zmienić przez użytkownika. Można jednak `Get-HcsRoutingTable` polecenia cmdlet, aby wydrukować routingu tabeli (i metryk) na urządzeniu StorSimple. Więcej informacji na temat polecenia cmdlet Get-HcsRoutingTable w [wdrożenia StorSimple rozwiązywania problemów](storsimple-troubleshoot-deployment.md).

Algorytmy metryki routingu są różne w zależności od wersji oprogramowania uruchomionym na urządzeniu StorSimple.

**Wersjach przed aktualizacji 1**

Dotyczy to również wersji oprogramowania przed Update 1, takich jak GA, 0,1, 0,2 lub 0,3 wersji. Kolejność na podstawie metryk routingu jest następująca:

   *Ostatnia skonfigurowane 10 GbE sieciowej > inne 10 GbE sieciowej > Ostatnia skonfigurowane 1 GbE sieciowej > sieciowej inne GbE 1*


**Wersjach począwszy od 1 aktualizacji i przed aktualizacji 2**

Dotyczy to również wersji oprogramowania, takie jak 1, 1.1 lub 1.2. Kolejność na podstawie routingu metryk zależy od w następujący sposób:

   *DANYCH 0 > Ostatnia skonfigurowane 10 GbE sieciowej > inne 10 GbE sieciowej > Ostatnia skonfigurowane 1 GbE sieciowej > sieciowej inne GbE 1*

   Update 1 jest przekształcana w metryki routingu danych 0 najniższe; w związku z tym wszystkie chmury — ruch odbywa się za pośrednictwem danych 0. Zanotuj to jeśli istnieje więcej niż jeden interfejs sieci chmury na urządzeniu StorSimple.


**Wersje, rozpoczynając od aktualizacji 2**

Aktualizacja 2 zawiera kilka ulepszeń związanych z sieci i metryka routingu uległa zmianie. Zachowanie można następująco wyjaśnić.

- Zestaw wstępnie wartości zostały przypisane do interfejsów.   

- Należy rozważyć Przykładowa tabela poniżej wartości przypisane do różnych interfejsów sieciowych, gdy są one chmury — włączone lub wyłączone chmury, ale przy użyciu skonfigurowanej bramy. Należy zauważyć, że wartości w tym miejscu są przykładowe wartości.


  	| Interfejs sieciowy | Obsługiwanych w chmurze | Chmura wyłączonymi bramy |
  	|-----|---------------|---------------------------|
  	| Dane 0  | 1            | -                        |
  	| Dane 1  | 2            | 20                       |
  	| Dane 2  | 3            | 30                       |
  	| Dane 3  | 4            | 40                       |
  	| Dane 4  | 5            | 50                       |
  	| Dane 5  | 6            | 60                       |


- Kolejność, w którym ruch chmury będą kierowane za pośrednictwem interfejsów sieci jest:

    *Dane 0 > dane 1 > Data 2 > Dane 3 > danych 4 > danych 5*

    Można to wyjaśnić poniższym przykładzie.

    Należy rozważyć, czy urządzenia StorSimple z dwóch obsługiwanych w chmurze interfejsów, danych 0 i 5 danych. Dane 1 do 4 danych są wyłączone chmury, ale masz skonfigurowany bramy. Kolejność, w którym ruch będą kierowane w przypadku tego urządzenia będą:

    *Dane 0 (1) > danych 5 (6) > dane 1 (20) > dane 2 (30) > Dane 3 (40) > danych 4 (50)*

    *Jeżeli liczby w nawiasach oznaczają odpowiednich metryki routingu.*

    Jeśli dane 0 nie powiedzie się, ruch chmury będzie uzyskiwanie kierowane do 5 danych. Zakładając, że brama jest skonfigurowany w innych sieciach, gdyby zarówno danych 0 i 5 danych kończy się niepowodzeniem, ruch chmury przejdzie do danych 1.


- Jeśli sieciowej obsługiwanych w chmurze nie powiedzie się, to 3 ponowne próby z 30 opóźnieniem drugi, aby nawiązać połączenie z interfejsem. Jeśli nie wszystkie próby połączenia, ruch jest kierowane do następnego dostępne obsługiwanych w chmurze interfejsu określone w tabeli routingu. Jeśli wszystkie sieci obsługiwanych w chmurze interfejsy błędów, następnie urządzenie nie powiedzie się nad do innego kontrolera (nie Uruchom ponownie komputer w tym przypadku).

- W przypadku awarii VIP dla interfejsu sieci iSCSI, będzie 3 ponowne próby z opóźnieniem dwie sekundy. To zachowanie jest przebywała zgodne z poprzednimi wersjami. Jeśli nie wszystkich interfejsów sieciowych iSCSI, trybie awaryjnym kontroler zostanie przeprowadzone (wraz z Uruchom ponownie komputer).


- Alert jest również wywoływane na urządzeniu StorSimple po awarii VIP. Aby uzyskać więcej informacji przejdź do [alertu podręczna karta informacyjna](storsimple-manage-alerts.md).

- Pod względem ponowne próby iSCSI ma pierwszeństwo przed chmury.

    Poniżej podano przykład: StorSimple urządzenie ma dwa interfejsy włączona, dane 0 i 1 danych. Dane 0 jest chmury obsługą dane 1 jest zarówno chmury i obsługą iSCSI. Żadne interfejsy sieciowe na tym urządzeniu są włączone dla chmury lub iSCSI.

    Jeśli 1 danych kończy się niepowodzeniem, podane go jest ostatnim interfejsu iSCSI w sieci, spowoduje w trybie awaryjnym kontroler danych 1 na innym kontrolerze.


### <a name="networking-best-practices"></a>Najlepsze rozwiązania sieci

Oprócz powyższych wymagań sieci, aby uzyskać optymalną wydajność rozwiązania StorSimple należy stosować następujące wskazówki:

- Upewnij się, że urządzenie StorSimple ma dedykowane 40 przepustowości MB/s (lub więcej) dostępne przez cały czas. Nie mogą być udostępniane przepustowość (lub alokacji należy zagwarantować za pomocą zasad QoS) z innymi aplikacjami.

- Upewnij się, że łączności z Internetem jest dostępna w każdej chwili. Sporadycznie lub mało wiarygodnych połączenia z Internetem do urządzeń, takich jak nie połączenia z Internetem, spowoduje nieobsługiwanej konfiguracji.

- Wyodrębnianie ruch iSCSI i chmura przez o dedykowane interfejsy na urządzeniu iSCSI i chmury dostępu. Aby uzyskać więcej informacji, zobacz jak [zmodyfikować interfejsy](storsimple-modify-device-config.md#modify-network-interfaces) na urządzeniu StorSimple.

- Nie należy używać konfiguracji łącze agregacji Control Protocol (LACP) interfejsów sieciowych. Jest to nieobsługiwanej konfiguracji.


## <a name="high-availability-requirements-for-storsimple"></a>Wymagania dotyczące wysokiej dostępności dla StorSimple

Platformy sprzętowej, która jest dołączona do rozwiązania StorSimple zawiera funkcje dostępności i niezawodności, które zapewniają podstawę infrastruktury wysokiej dostępności, odporność na uszkodzenia przestrzeni dyskowej w centrum danych. Istnieją jednak, wymagania i najlepsze rozwiązania, które powinny być zgodne z aby zapewnić dostępność rozwiązania StorSimple. Przed wdrożeniem StorSimple dokładnie przejrzyj następujące wymagania i najważniejsze wskazówki dotyczące urządzenia StorSimple i połączonego hostów.

Aby uzyskać więcej informacji na temat monitorowania i konserwowanie sprzętowej urządzenia StorSimple przejdź do [użycia usługę Menedżer StorSimple monitorowanie sprzętowej i stan](storsimple-monitor-hardware-status.md) i [Wymiana składników sprzętu StorSimple](storsimple-hardware-component-replacement.md).

### <a name="high-availability-requirements-and-procedures-for-your-storsimple-device"></a>Wymagania wysokiej dostępności i procedury dotyczące urządzenia StorSimple

Przejrzyj poniższe informacje dokładnie w celu zapewnienia wysokiej dostępności urządzenia StorSimple.

#### <a name="pcms"></a>PCMs

StorSimple urządzeń należą zbędne, wyłączania zasilania i chłodzenia modułów (PCMs). Każdy PCM ma za mało możliwości obsługi dla całego podwozia. W celu zapewnienia wysokiej dostępności, musi być zainstalowany zarówno PCMs.

- Usługi PCMs łączenie się z różnych power źródłami zapewnienie dostępności, jeśli źródłem zasilania nie powiedzie się.
- Jeśli PCM kończy się niepowodzeniem, poproś o wymianę natychmiast.
- Usuwanie nie powiodło się PCM tylko wtedy, gdy masz zastąpienie i są gotowe do zainstalowania go.
- Nie usuwaj obu PCMs jednocześnie. Moduł PCM zawiera moduł baterii kopii zapasowej. Usuwanie oba PCMs spowoduje zamknięcie bez ochrony baterii i stan urządzenia nie zostaną zapisane. Aby uzyskać więcej informacji o baterii przejdź do [zarządzania modułu akumulatora kopii zapasowej](storsimple-battery-replacement.md#maintain-the-backup-battery-module).

#### <a name="controller-modules"></a>Kontroler modułów

StorSimple urządzeń należą moduły zbędne, wyłączania kontrolerze. Moduły kontrolera działają w sposób aktywne/pasywne. W dowolnym momencie jeden moduł kontrolera jest aktywna, a udostępnia usługi, gdy moduł kontroler jest pasywne. Moduł kontrolera pasywne jest włączony i jest aktywna, jeśli moduł kontrolera active kończy się niepowodzeniem lub usunięte. Każdy moduł kontroler ma za mało możliwości obsługi dla całego podwozia. Musi być zainstalowany zarówno moduły kontroler zapewnienie wysokiej dostępności.

- Upewnij się, że oba moduły kontroler są zainstalowane przez cały czas.

- Jeśli moduł kontrolera kończy się niepowodzeniem, poproś o wymianę natychmiast.

- Usuń moduł uszkodzony kontroler tylko wtedy, gdy masz zastąpić i są gotowe do zainstalowania go. Usuwanie moduł na dłuższy czas wpłynie na przepływ powietrza i w związku z tym chłodzenia systemu.

- Upewnij się, że połączeń sieciowych do obu modułów kontroler są identyczne i interfejsy sieciowe połączonego mają konfiguracji sieci identyczne.

- Jeśli moduł kontrolera kończy się niepowodzeniem lub konieczność wymiany, upewnij się, jest moduł kontroler do stanu aktywnego przed zamianą moduł kontrolera nie powiodło się. Aby sprawdzić, czy kontroler jest aktywne, przejdź do [Identyfikowanie active kontrolera na urządzeniu](storsimple-controller-replacement.md#identify-the-active-controller-on-your-device).

- Nie usuwaj obu moduły kontroler w tym samym czasie. Jeśli trybie awaryjnym kontroler jest w toku, Zamknij moduł kontrolera wstrzymania lub nie usuwać obudowy.

- Po przełączeniu kontrolera Poczekaj co najmniej pięć minut, zanim usuniesz albo moduł kontrolera.

#### <a name="network-interfaces"></a>Interfejsy

StorSimple urządzenia kontrolera moduły każdego mają cztery 1 Gigabit i 10 dwóch interfejsów sieciowych Gigabit Ethernet.

- Upewnij się, że połączeń sieciowych do obu modułów kontroler są identyczne i sieci interfejsy o połączeniu interfejsów modułu kontroler konfiguracji sieci identyczne.

- Jeśli to możliwe, należy wdrożyć połączeń sieciowych w innych przełączników zapewniające dostępność usługi w wypadku awarii urządzenia sieciowego.

- Odłączanie wyłącznie lub ostatniego pozostała obsługą iSCSI interfejsu (z przypisane adresy IP), najpierw wyłączyć interfejs, a następnie odłącz kable. Jeśli interfejs jest odłączony najpierw, a następnie spowoduje active kontrolera kończy się niepowodzeniem w stronie biernej kontroler. Jeśli kontroler pasywne zawiera również jego odpowiednie interfejsy odłączony, oba kontrolery zostanie uruchomiony kilka razy przed rozpoczęciem na jednym kontrolerze.

- Co najmniej dwa interfejsy danych należy połączyć się z siecią w każdym module kontrolerze.

- Po włączeniu dwóch 10 GbE interfejsów, wdrażanie tych przez inne przełączniki.

- Jeśli to możliwe, użyj MPIO na serwerach, aby upewnić się, że serwery przeszkadzają łącza, siecią lub awaria interfejsu.

Aby uzyskać więcej informacji dotyczących urządzenia wysokiej dostępności i wydajności, przejdź do [zainstalować urządzenia StorSimple 8100](storsimple-8100-hardware-installation.md#cable-your-storsimple-8100-device) lub [urządzeniu StorSimple 8600](storsimple-8600-hardware-installation.md#cable-your-storsimple-8600-device).

#### <a name="ssds-and-hdds"></a>Twarde SSD i dysków twardych

StorSimple urządzeń należą stanie stałym dyski (twarde SSD) i stacji dysków twardych (dysków twardych), które są chronione za pomocą zdublowany spacje. Użycie spacji lustrzane sprawia, że urządzenie jest możliwość tolerować błąd twarde SSD lub dysków twardych.

- Upewnij się, że są zainstalowane wszystkie moduły SSD i dysk twardy.

- Jeśli SSD lub dysk twardy kończy się niepowodzeniem, poproś o wymianę natychmiast.

- Jeśli SSD lub dysk twardy kończy się niepowodzeniem lub wymagana jest wymiana, upewnij się, możesz usunąć tylko SSD lub dysk twardy, która wymaga wymiany.

- Nie usuwaj więcej niż jeden SSD lub dysk twardy systemu w dowolnym momencie w czasie.
Błąd 2 lub więcej dysków określonego typu (dysk twardy, SSD) lub następujących po sobie błędach określonego przedziału czasu krótkiej może spowodować utratę system nieprawidłowego działania i możliwości danych. W takim przypadku [skontaktuj się z pomocą techniczną firmy Microsoft](storsimple-contact-microsoft-support.md) w celu uzyskania pomocy.

- Podczas zastąpienie monitoruje **Stan sprzętu** na stronie **konserwacji** dyski twarde SSD i dysków twardych. Stan zielony znacznik wyboru wskazuje, czy dyski są prawidłowy lub OK, wskaż czerwony wykrzyknik wskazuje, nie powiodło się SSD lub dysk twardy.

- Zalecamy skonfigurowanie chmury migawek dla wszystkich wielkości, potrzebne do ochrony w przypadku awarii.

#### <a name="ebod-enclosure"></a>Załącznik EBOD

Model urządzenia StorSimple 8600 zawiera załącznik rozszerzonego wiele z dysków (EBOD), oprócz podstawowego załącznik. EBOD zawiera kontrolery EBOD i stacji dysków twardych (dysków twardych), które są chronione za pomocą zdublowany spacje. Użycie spacji lustrzane sprawia, że urządzenie jest możliwość tolerować awarii jeden lub więcej dysków twardych. Załącznik EBOD jest połączony z podstawowego załącznik za pośrednictwem zbędne kable skojarzeń zabezpieczeń.

- Upewnij się, że oba moduły kontroler EBOD załącznik zarówno kable skojarzenia zabezpieczeń, a także wszystkie dysków twardych są instalowane przez cały czas.

- Jeśli moduł kontrolera załącznik EBOD kończy się niepowodzeniem, poproś o wymianę natychmiast.

- Jeśli moduł kontrolera załącznik EBOD nie powiedzie się, upewnij się, moduł kontroler jest aktywna, przed zastąpienie modułu nie powiodło się. Aby sprawdzić, czy kontroler jest aktywne, przejdź do [Identyfikowanie active kontrolera na urządzeniu](storsimple-controller-replacement.md#identify-the-active-controller-on-your-device).

- Podczas odpowiednikiem przyklejanych modułu EBOD kontrolera stale monitorować stan składnika w usłudze Menedżer StorSimple po zalogowaniu się do **utrzymania** > **stanu sprzętu**.

- Jeśli kabel skojarzenia zabezpieczeń kończy się niepowodzeniem lub wymaga wymiana (Microsoft Support powinna zostać uwzględniona do dokonania takiego określenia), upewnij się, usunięcie kabla skojarzeń zabezpieczeń, która wymaga wymiany.

- Nie jednocześnie usunąć zarówno kable skojarzeń zabezpieczeń systemu w dowolnym momencie w czasie.

### <a name="high-availability-recommendations-for-your-host-computers"></a>Zalecenia wysoką dostępność dla komputerów hosta

Dokładnie przejrzyj poniższe najważniejsze wskazówki, aby zapewnić wysoki poziom dostępności hostów połączonych na urządzeniu StorSimple.

- Konfigurowanie StorSimple z [dwóch węzłach pliku konfiguracji klaster serwera][1]. Usuwając pojedynczych punktów awarii i tworzenia w nadmiarowości po stronie hosta, całe rozwiązanie staje się wysokiej dostępności.

- Za pomocą dostępne przez cały czas dostępne akcje (CA) systemu Windows Server 2012 (SMB 3.0) wysokiej dostępności podczas pracy awaryjnej kontroler miejsca do magazynowania. Aby uzyskać dodatkowe informacje dotyczące konfigurowania klastrów serwerów plików i udziałów przez cały czas dostępne z systemem Windows Server 2012 zapoznaj się z tym [pokaz wideo](http://channel9.msdn.com/Events/IT-Camps/IT-Camps-On-Demand-Windows-Server-2012/DEMO-Continuously-Available-File-Shares).

## <a name="next-steps"></a>Następne kroki

- [Więcej informacji na temat ograniczeń systemowych StorSimple](storsimple-limits.md).
- [Dowiedz się, jak wdrożyć rozwiązanie StorSimple](storsimple-deployment-walkthrough-u2.md).

<!--Reference links-->
[1]: https://technet.microsoft.com/library/cc731844(v=WS.10).aspx
