<properties
   pageTitle="Wymagania systemowe StorSimple wirtualnych tablicy"
   description="Więcej informacji na temat oprogramowania i sieci wymagania dotyczące macierzy wirtualnych StorSimple"
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
   ms.workload="NA"
   ms.date="10/17/2016"
   ms.author="alkohli"/>

# <a name="storsimple-virtual-array-system-requirements"></a>Wymagania systemowe StorSimple wirtualnych tablicy

## <a name="overview"></a>Omówienie

W tym artykule opisano wymagania systemowe dla usługi Microsoft Azure StorSimple wirtualnych tablicy (nazywane także StorSimple lokalnej wirtualną lub urządzenia wirtualnego StorSimple) oraz dla klientów magazyn, dostęp do tablicy. Zalecamy zapoznanie się informacje starannie przed wdrożenie systemu StorSimple, a następnie odnoszą się do niego stosownie do potrzeb podczas wdrażania i kolejnych operacji.

Wymagania systemowe obejmują:

-   **Wymagania dotyczące oprogramowania dla klientów miejsca do magazynowania** — w tym artykule opisano platformy obsługiwane wirtualizacji, przeglądarki sieci web inicjatorach, SMB klientów, wymagania minimalne wirtualne urządzenie i wszelkie dodatkowe wymagania dotyczące tych systemów operacyjnych.

-   **Sieć wymagania dotyczące urządzenia StorSimple** - zawiera informacje dotyczące portów, które muszą być otwarte w zaporze, aby umożliwić iSCSI, chmury lub danych związanych z zarządzaniem.

StorSimple informacje dotyczące wymagań systemu opublikowanych w tym artykule dotyczy tylko programu tablic wirtualnych StorSimple.

- W przypadku urządzeń z serii 8000 przejdź do [wymagania systemowe dla swojego urządzenia serii StorSimple 8000](storsimple-system-requirements.md).
 
- Dla urządzeń z serii 7000 przejdź do [wymagania systemowe dla swojego urządzenia serii 5000 7000 StorSimple](http://onlinehelp.storsimple.com/1_StorSimple_System_Requirements).


## <a name="software-requirements"></a>Wymagania dotyczące oprogramowania

Wymagania dotyczące oprogramowania zawiera informacje o obsługiwanych przeglądarek, wersje SMB, platformy wirtualizacji i wymagania minimalne wirtualne urządzenie.

### <a name="supported-virtualization-platforms"></a>Platformy obsługiwane wirtualizacji


| **Monitor maszyny wirtualnej** | **Wersja**                          |
|----------------|--------------------------------------|
| Funkcji Hyper-V        | Windows Server 2008 R2 z dodatkiem SP1 lub nowszym |
| VMware ESXi    | 5,5 lub nowszy                        |

### <a name="virtual-device-requirements"></a>Wymagania dotyczące urządzeń wirtualnych

| **Składnik**                                | **Wymaganie**            |
|----------------------------------------------|----------------------------|
| Minimalna liczba procesorów wirtualnych (rdzenie) | 4                          |
| Minimalne pamięci RAM                         | 8 GB                       |
| Miejsca na dysku<sup>1</sup>                       | Dysk systemu operacyjnego - 80 GB <br></br>Dane dysku - 500 GB do 8 TB|
| Minimalna liczba interfejsy sieci       | 1                          |
| Minimalne Internet przepustowości<sup>2</sup>       | 5 MB/s                     |

<sup>1</sup> - cienki obsługi administracyjnej

<sup>2</sup> — wymagania sieci mogą się różnić w zależności od dziennego kursu zmiany danych. Na przykład jeśli urządzenie musi utworzyć kopię zapasową 10 GB lub więcej zmian w ciągu dnia, następnie codziennego wykonywania kopii zapasowej przez połączenie 5 MB/s może potrwać do 4,25 godzin (Jeśli dane nie można kompresowane lub deduplicated).

### <a name="supported-web-browsers"></a>Obsługiwane przeglądarki sieci web

| **Składnik**     | **Wersja** | **Dodatkowe wymagania i notatek** |
|-------------------|-----------------|-----------------------------------|
| Microsoft Edge    | Najnowsza wersja  |                                   |
| Program Internet Explorer | Najnowsza wersja  | Przetestowano z programem Internet Explorer 11  |
| Google Chrome     | Najnowsza wersja  | Przetestowano przy użyciu przeglądarki Chrome 46             |

### <a name="supported-storage-clients"></a>Klienci obsługiwane miejsca do magazynowania 

Są następujące wymagania dotyczące oprogramowania dla inicjatorach, dostępne StorSimple macierzy wirtualnych (skonfigurowanego jako serwerem).

| **Obsługiwane systemy operacyjne** | **Wersja wymagana** | **Dodatkowe wymagania i notatek** |
| --------------------------- | ---------------- | ------------- |
| System Windows Server              | 2008R2 Z DODATKIEM SP1, 2012, 2012R2 |StorSimple można tworzyć znacznie ustanawianie i pełni ustanawianie wielkości. Nie można go utworzyć częściowo ustanawianie wielkości. Dla ilości iSCSI StorSimple są obsługiwane tylko: <ul><li>Prosta wielkości na dyskach podstawowych systemu Windows.</li><li>Windows NTFS formatowania woluminu.</li>|


Są następujące wymagania dotyczące oprogramowania dla klientów SMB dostępne StorSimple macierzy wirtualnych (skonfigurowane jako serwera plików).

| **Wersja SMB** |
|-------------|
| SMB 2.x     |
| SMB 3.0     |
| SMB 3.02    |

> [AZURE.IMPORTANT] Kopiowanie lub nie przechowywać pliki chronione przez system Windows System (szyfrowania plików) na serwerze plików StorSimple wirtualnych tablica; Spowoduje to nieobsługiwanej konfiguracji. 

## <a name="networking-requirements"></a>Wymagania dotyczące sieci 

W poniższej tabeli wymieniono porty, które muszą być otwarte w zaporze umożliwiające iSCSI, SMB, chmury lub danych związanych z zarządzaniem. W tej tabeli *w* lub *ruchu przychodzącego* odwołuje się do kierunku, w którym przychodzących żądań dostępu do urządzenia. Odwołuje *się* lub *ruchu wychodzącego* w kierunku, w którym urządzenia StorSimple wysyła poza rozmieszczania danych zewnętrznych,: na przykład ruchu wychodzącego z Internetem.

| **Port nr<sup>1</sup>** | **Lub** | **Zakres portu** | **Wymagane**              | **Notatki**                                                                                                            |
|--------------------------|---------------|----------------|---------------------------|----------------------------------------------------------------------------------------------------------------------|
| PORT TCP 80 (HTTP)            | W nowym oknie           | WAN            | Brak                        | Port wyjściowy umożliwia dostęp do Internetu pobrać aktualizacje. <br></br>Serwer proxy ruchu wychodzącego sieci web jest użytkownikiem można konfigurować. |
| PORT TCP 443 (HTTPS)          | W nowym oknie           | WAN            | Tak                       | Port wyjściowy jest używana do uzyskiwania dostępu do danych w chmurze. <br></br>Serwer proxy ruchu wychodzącego sieci web jest użytkownikiem można konfigurować. |
| UDP 53 (DNS)             | W nowym oknie           | WAN            | W niektórych przypadkach; Zobacz uwagi. | Ten port jest wymagany tylko wtedy, gdy korzystasz z serwera DNS internetowych. <br></br> **Uwaga**: Jeśli wdrażanie serwera plików, zaleca się używanie lokalnego serwera DNS.|
| UDP 123 (NTP)            | W nowym oknie           | WAN            | W niektórych przypadkach; Zobacz uwagi. | Ten port jest wymagany tylko wtedy, gdy korzystasz z serwera NTP internetowych.<br></br> **Uwaga:** Wdrażanie serwera plików, zaleca się synchronizowanie czasu kontrolerami domeny usługi Active Directory.  |
| PORT TCP 80 (HTTP)           | W            | SIECI LAN            | Tak                       | Jest to numer portu ruchu przychodzącego lokalne interfejsu użytkownika na tym urządzeniu StorSimple do zarządzania lokalnego. <br></br> **Uwaga**: uzyskiwanie dostępu do lokalnych interfejsu użytkownika za pomocą protokołu HTTP spowoduje automatyczne przekierowanie na HTTPS.|
| PORT TCP 443 (HTTPS)          | W            | SIECI LAN            | Tak                       | Jest to numer portu ruchu przychodzącego lokalne interfejsu użytkownika na tym urządzeniu StorSimple do zarządzania lokalnego.|
| Port TCP 3260 (iSCSI)         | W            | SIECI LAN            | Brak                        | Ten port jest używany przez iSCSI dostępu do danych.|

<sup>1</sup> nie porty przychodzące muszą być otwarte na publicznego Internetu.

> [AZURE.IMPORTANT] Upewnij się, zapory nie modyfikować ani odszyfrowywanie ruch SSL między urządzeniem StorSimple i Azure.


### <a name="url-patterns-for-firewall-rules"></a>Adres URL deseni dla reguły zapory 

Administratorzy sieci często można skonfigurować reguły zapory zaawansowane na podstawie wzorców adresów URL, aby odfiltrować przychodzącego i ruchu wychodzącego. Wirtualna macierzy i usługa Menedżer StorSimple zależą od innych aplikacji firmy Microsoft, takich jak Bus usługi Azure Azure Active Directory kontrola dostępu, kont miejsca do magazynowania i serwery Microsoft Update. Desenie adresu URL skojarzone z nich może służyć do skonfiguruje reguły zapory. Należy zrozumieć, że wzorców adresów URL skojarzone z nich można zmienić. Wymaga to co z kolei administratorem sieci, aby monitorować i aktualizować reguły zapory dla programu StorSimple jako i w razie potrzeby. 

Zaleca się, że możesz ustawić reguły zapory dla ruchu wychodzącego, oparte na StorSimple liberally stałe adresy IP, w większości przypadków. Jednak może zawierać poniższe informacje, aby ustawić reguły zapory zaawansowanej, które są potrzebne do utworzenia bezpiecznych środowiskach.

> [AZURE.NOTE] 
> 
> - Urządzenia (źródło) adresy IP powinno być zawsze ustawione dla wszystkich interfejsów sieci korzystających z chmury. 
> - Miejsce docelowe adresy IP powinna być równa [zakresów adresów IP centrum danych Azure](https://www.microsoft.com/en-us/download/confirmation.aspx?id=41653).


| Wzór adresu URL                                                      | Składnik i funkcji                                           |
|------------------------------------------------------------------|---------------------------------------------------------------|
| `https://*.storsimple.windowsazure.com/*`<br>`https://*.accesscontrol.windows.net/*`<br>`https://*.servicebus.windows.net/*`   | Usługa Menedżera StorSimple<br>Usługa kontroli dostępu<br>Usługa Azure Bus|
|`http://*.backup.windowsazure.com`|Rejestracja urządzenia|
|`http://crl.microsoft.com/pki/*`<br>`http://www.microsoft.com/pki/*`|Odwołania certyfikatu |
| `https://*.core.windows.net/*`<br>`https://*.data.microsoft.com`<br>`http://*.msftncsi.com` | Konta magazynu platformy Azure i monitorowania |
| `http://*.windowsupdate.microsoft.com`<br>`https://*.windowsupdate.microsoft.com`<br>`http://*.update.microsoft.com`<br> `https://*.update.microsoft.com`<br>`http://*.windowsupdate.com`<br>`http://download.microsoft.com`<br>`http://wustat.windows.com`<br>`http://ntservicepack.microsoft.com`| Serwery Microsoft Update<br>                        |
| `http://*.deploy.akamaitechnologies.com`                         |Sieci CDN Akamai |
| `https://*.partners.extranet.microsoft.com/*`                    | Pakiet pomocy technicznej                                                  |
| `http://*.data.microsoft.com `                   | Usługa telemetrycznego w systemie Windows, zobacz [Aktualizacja dla klienta i telemetrycznego diagnostyczne](https://support.microsoft.com/en-us/kb/3068708)                                                  |

## <a name="next-step"></a>Następny krok

-   [Przygotowywanie portalu wdrożenia macierzy wirtualnych StorSimple](storsimple-ova-deploy1-portal-prep.md)
