<properties 
   pageTitle="O urządzeniach VPN brama VPN witryny do witryny połączeń wirtualnych sieci Azure | Microsoft Azure"
   description="W tym artykule omówiono urządzenia VPN i parametry protokołu IP dla połączeń Brama VPN S2S i zawiera łącza do instrukcje dotyczące konfigurowania i próbki."
   services="vpn-gateway"
   documentationCenter="na"
   authors="yushwang"
   manager="rossort"
   editor=""
  tags="azure-resource-manager, azure-service-management"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/13/2016"
   ms.author="yushwang;cherylmc" />

# <a name="about-vpn-devices-for-site-to-site-vpn-gateway-connections"></a>O urządzeniach VPN połączeń Brama VPN witryny do witryny

Aby skonfigurować połączenie VPN witryny do witryny (S2S) jest wymagane urządzenie VPN. Połączenia witryny do witryny można utworzyć rozwiązanie hybrydowego lub w dowolnym momencie bezpiecznego połączenia między sieci lokalnej i wirtualnych sieci. W tym artykule omówiono zgodnych urządzeń sieci VPN oraz parametrów konfiguracji. 

>[AZURE.NOTE] Konfigurując połączenia witryny do witryny, wymagane dla swojego urządzenia VPN jest adres IP protokołu IPv4 publicznej.                                                                                                                                                                               

Jeśli urządzenia nie ma w tabeli [sprawdzana poprawność VPN urządzeń](#devicetable) , zobacz sekcję [urządzeń sprawdzania poprawności nie VPN](#additionaldevices) tego artykułu. Istnieje możliwość, że urządzenie może pracować z Azure. Obsługa urządzenia VPN skontaktuj się z dostawcą urządzenia.

**Elementy do należy pamiętać podczas wyświetlania tabel:**

- Został zmiany terminologia statycznego i dynamicznego routingu. Prawdopodobnie napotkasz oba warunki. Nie ulega zmianie funkcjonalności, zmieniana tylko nazwy.
    - Routing statyczny = PolicyBased
    - Routing dynamiczny = RouteBased
- Specyfikacje dla bramy wysokiej wydajności sieci VPN i brama RouteBased VPN są takie same, chyba że inaczej. Na przykład sprawdzanej urządzeń sieci VPN, które są zgodne z bram RouteBased VPN również są zgodne z bramy sieci VPN Azure wysokiej wydajności. 


## <a name="devicetable"></a>Zatwierdzone urządzenia VPN 

Sprawdzeniu poprawności zestawu standardowych urządzeń VPN we współpracy z dostawcami urządzenia. Wszystkich urządzeń w rodziny urządzenia zawartych na poniższej liście powinny Praca z bram Azure VPN. Zobacz [Temat Brama VPN](vpn-gateway-about-vpngateways.md) Aby sprawdzić typ bramy, którą trzeba utworzyć rozwiązanie, które chcesz skonfigurować. 

Aby skonfigurować urządzenia VPN, zapoznaj się z łącza, które odpowiadają rodziny odpowiedniego urządzenia. Obsługa urządzenia VPN skontaktuj się z dostawcą urządzenia.



| **Dostawcy**                      | **Rodzina urządzenia**                                        | **Minimalna wersja systemu operacyjnego**                             | **PolicyBased**                                                                                                                                                                                                             | **RouteBased**                                                                                                                                                                    |
|---------------------------------|----------------------------------------------------------|----------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Telesis pokrewnych                  | Wyczyść Routery VPN serii                                    | 2.9.2                                              | Wkrótce                                                                                                                                                                                                                                          | Niezgodne                                                                                                                                                                                               |
| Barracuda sieci, Inc.        | Zapory NextGen barracuda F-serii             | PolicyBased: 5.4.3, RouteBased: 6.2.0  | [Instrukcje dotyczące konfiguracji](https://techlib.barracuda.com/NGF/AzurePolicyBasedVPNGW) | [Instrukcje dotyczące konfiguracji](https://techlib.barracuda.com/NGF/AzureRouteBasedVPNGW)                                                                                                                                                                                              |
| Barracuda sieci, Inc.        |  Barracuda NextGen zapory X serii                 | Barracuda zapory 6.5 | [Zapora barracuda](https://techlib.barracuda.com/BFW/ConfigAzureVPNGateway) | Niezgodne                                                                                                                                                                                               |
| Brokatem                         | VRouter Vyatta 5400                                      | Wirtualna Router 6.6R3 GA                            | [Instrukcje dotyczące konfiguracji](http://www1.brocade.com/downloads/documents/html_product_manuals/vyatta/vyatta_5400_manual/wwhelp/wwhimpl/js/html/wwhelp.htm#href=VPN_Site-to-Site%20IPsec%20VPN/Preface.1.1.html)                                       | Niezgodne                                                                                                                                                                                               |
| Punkt wyboru                     | Brama zabezpieczeń                                         | R75.40, R75.40VS                                     | [Instrukcje dotyczące konfiguracji](https://supportcenter.checkpoint.com/supportcenter/portal?eventSubmit_doGoviewsolutiondetails=&solutionid=sk101275)                                         | [Instrukcje dotyczące konfiguracji](https://supportcenter.checkpoint.com/supportcenter/portal?eventSubmit_doGoviewsolutiondetails=&solutionid=sk101275) |
| Cisco                           | ASA                                                      | 8.3                                                | [Przykłady firmy Cisco](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ASA)                                                                                                                                                                        | Niezgodne                                                                                                                                                                                               |
| Cisco                           | AUTOMATYCZNEGO ODZYSKIWANIA SYSTEMU                                                      | IOS 15,1 (PolicyBased), IOS 15,2 (RouteBased)                | [Przykłady firmy Cisco](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ASR)                                                                                                                                                                        | [Przykłady firmy Cisco](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ASR)                                                                                                                                 |
| Cisco                           | ISR                                                      | IOS 15.0 (PolicyBased), IOS 15,1 (RouteBased *)               | [Przykłady firmy Cisco](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ISR)                                                                                                                                                                        | [Przykłady Cisco *](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ISR)                                                                                                                                |
| Citrix                          | VPX MPX NetScaler, SDX,      |10.1 i powyżej                                           | [Instrukcje dotyczące integracji](https://docs.citrix.com/en-us/netscaler/11-1/system/cloudbridge-connector-introduction/cloudbridge-connector-azure.html)                                                                                                                                                                            | Niezgodne                                                                                                                                                                                               |
| Dell SonicWALL                  | Seria SC, NSA serii, SuperMassive serii E-klasy NSA serii | SonicOS 5.8.x, [SonicOS 5.9.x](http://documents.software.dell.com/sonicos/5.9/microsoft-azure-configuration-guide/supported-platforms?ParentProduct=850), [SonicOS 6.x](http://documents.software.dell.com/sonicos/6.2/microsoft-azure-configuration-guide/supported-platforms?ParentProduct=646 )          | [Instrukcje dotyczące - SonicOS 6.2](http://documents.software.dell.com/sonicos/6.2/microsoft-azure-configuration-guide?ParentProduct=646) [Instrukcje dotyczące - SonicOS 5,9](http://documents.software.dell.com/sonicos/5.9/microsoft-azure-configuration-guide?ParentProduct=850)                                                                                                                                   | [Instrukcje dotyczące - SonicOS 6.2](http://documents.software.dell.com/sonicos/6.2/microsoft-azure-configuration-guide?ParentProduct=646) [Instrukcje dotyczące - SonicOS 5,9](http://documents.software.dell.com/sonicos/5.9/microsoft-azure-configuration-guide?ParentProduct=850)                                                                                                                                                                                      |
| F5                              | Seria BIG IP                                 |           12.0                                            | [Instrukcje dotyczące konfiguracji](https://devcentral.f5.com/articles/connecting-to-windows-azure-with-the-big-ip)                                                                                                                                                                          | [Instrukcje dotyczące konfiguracji](https://devcentral.f5.com/articles/big-ip-to-azure-dynamic-ipsec-tunneling)                                                                                                                                                                                         |
| Fortinet                        | FortiGate                                                | FortiOS ppkt 5.2.7                                      | [Instrukcje dotyczące konfiguracji](http://docs.fortinet.com/d/fortigate-configuring-ipsec-vpn-between-a-fortigate-and-microsoft-azure)                                                                                                                                                                          | [Instrukcje dotyczące konfiguracji](http://docs.fortinet.com/d/fortigate-configuring-ipsec-vpn-between-a-fortigate-and-microsoft-azure)                                                                                                                                  |
| Japonia inicjatywy Internet (IIJ) | Seria SEIL                                              | SEIL / X 4.60 4.60 SEIL-B1 SEIL-x86 3,20            | [Instrukcje dotyczące konfiguracji](http://www.iij.ad.jp/biz/seil/ConfigAzureSEILVPN.pdf)                                                                                                                                                                   | Niezgodne                                                                                                                                                                                               |
| Juniper                         | SRX                                                      | JunOS 10.2 (PolicyBased), JunOS 11,4 (RouteBased)            | [Przykłady juniper](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SRX)                                                                                                                                                                      | [Przykłady juniper](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SRX)                                                                                                                              |
| Juniper                         | Seria J                                                 | JunOS 10.4r9 (PolicyBased), JunOS 11,4 (RouteBased)          | [Przykłady juniper](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/JSeries)                                                                                                                                                                 | [Przykłady juniper](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/JSeries)                                                                                                                         |
| Juniper                         | ISG                                                      | ScreenOS 6.3 (PolicyBased i RouteBased)                  | [Przykłady juniper](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/ISG)                                                                                                                                                                      | [Przykłady juniper](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/ISG)                                                                                                                              |
| Juniper                         | SSG                                                      | ScreenOS 6.2 (PolicyBased i RouteBased)                  | [Przykłady juniper](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SSG)                                                                                                                                                                      | [Przykłady juniper](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SSG)                                                                                                                              |
| Microsoft                       | Usługa Routing i zdalny dostęp                        | System Windows Server 2012                                | Niezgodne                                                                                                                                                                                                                                       | [Przykłady firmy Microsoft](http://go.microsoft.com/fwlink/p/?LinkId=717761)                                                                                           |
| Otwieranie AG systemów | Brama zabezpieczeń misji sterowania | N/D! | [Przewodnik po instalacji](https://www.open.ch/_pdf/Azure/AzureVPNSetup_Installation_Guide.pdf) | [Przewodnik po instalacji](https://www.open.ch/_pdf/Azure/AzureVPNSetup_Installation_Guide.pdf) |
| Openswan                        | Openswan                                                 | 2.6.32                                             | (Już wkrótce)                                                                                                                                                                                                                                        | Niezgodne                                                                                                                                                                                               |
| Palo Alto Networks              | Wszystkie urządzenia operacyjnego KADROWANIE     | PRZESUŃ — OS 6.1.5 lub nowszy (PolicyBased), PRZESUŃ systemu operacyjnego 7.0.5 lub nowszym (RouteBased)       | [Instrukcje dotyczące konfiguracji]( https://live.paloaltonetworks.com/t5/Configuration-Articles/How-to-Configure-VPN-Tunnel-Between-a-Palo-Alto-Networks/ta-p/59065)                                                                                                                                                                                          | [Instrukcje dotyczące konfiguracji](https://live.paloaltonetworks.com/t5/Integration-Articles/Configuring-IKEv2-VPN-for-Microsoft-Azure-Environment/ta-p/60340)                                                                                                                                                                                    |
| WatchGuard                      | Wszystkie                                                      | V11.x Fireware XTM                                 | [Instrukcje dotyczące konfiguracji](http://customers.watchguard.com/articles/Article/Configure-a-VPN-connection-to-a-Windows-Azure-virtual-network/)                                                                                                                                                                          | Niezgodne                                                                                                                                                                                               |

(*) Routery serii 7200 ISR obsługuje tylko PolicyBased sieci VPN.

## <a name="additionaldevices"></a>Sprawdzania poprawności nie urządzeń sieci VPN

Jeśli nie widzisz urządzenia wymienione w tabeli urządzeń sprawdzania poprawności VPN, nadal może on działać z połączeniem witryny do witryny. Sprawdź, czy urządzenia VPN spełnia minimalne wymagania opisane w sekcji wymagania dotyczące bramy artykułu [O bram sieci VPN](vpn-gateway-about-vpngateways.md#gateway-requirements) . Urządzenia spełniające minimalne wymagania powinno również działać z sieci VPN bramy. Aby uzyskać dodatkowe instrukcje pomocy technicznej i konfiguracji, skontaktuj się z producentem urządzenia.


## <a name="editing-device-configuration-samples"></a>Przykładowe edytowanie Konfiguracja urządzenia

Po pobraniu przedstawionych przykładowych konfiguracji urządzenia VPN należy zamienić niektóre z wartości tak, aby odzwierciedlała ustawień środowiska. 

**Aby edytować próbki:**

1. Otwórz przykładowy za pomocą Notatnika. 
1. Wyszukaj i Zastąp wszystkie ciągi <*tekst*> wartości, które dotyczą środowiska. Pamiętaj uwzględnić < i >. Po określeniu nazwy nazwy, które można wybrać powinny być unikatowe. Jeśli polecenie nie działa, zapoznaj się z dokumentacją producenta urządzenia.

| **Przykładowy tekst**                | **Zmień na**                                                                                                        |
|----------------------------------|----------------------------------------------------------------------------------------------------------------------|
| &lt;RP_OnPremisesNetwork&gt;           | Nazwy wybrany dla tego obiektu. Przykład: myOnPremisesNetwork                                                       |
| &lt;RP_AzureNetwork&gt;                | Nazwy wybrany dla tego obiektu. Przykład: myAzureNetwork                                                            |
| &lt;RP_AccessList&gt;                  | Nazwy wybrany dla tego obiektu. Przykład: myAzureAccessList                                                         |
| &lt;RP_IPSecTransformSet&gt;           | Nazwy wybrany dla tego obiektu. Przykład: myIPSecTransformSet                                                       |
| &lt;RP_IPSecCryptoMap&gt;              | Nazwy wybrany dla tego obiektu. Przykład: myIPSecCryptoMap                                                          |
| &lt;SP_AzureNetworkIpRange&gt;         | Określ zakres. Przykład: 192.168.0.0                                                                                  |
| &lt;SP_AzureNetworkSubnetMask&gt;      | Określ maskę podsieci. Przykład: 255.255.0.0                                                                            |
| &lt;SP_OnPremisesNetworkIpRange&gt;    | Określ zakres lokalnego. Przykład: 10.2.1.0                                                                         |
| &lt;SP_OnPremisesNetworkSubnetMask&gt; | Określ maskę podsieci lokalnego. Przykład: 255.255.255.0                                                              |
| &lt;SP_AzureGatewayIpAddress&gt;       | Te informacje dotyczą wirtualnej sieci i znajduje się w portalu zarządzania jako **adresu IP bramy**. |
| &lt;SP_PresharedKey&gt;                | Te informacje dotyczą wirtualnej sieci i znajduje się w portalu zarządzania jako klucz Zarządzanie.          |



## <a name="ipsec-parameters"></a>Parametry protokołu IP

>[AZURE.NOTE] Wartości wymienione w poniższej tabeli są obsługiwane przez bramę VPN Azure, jednak nie jest obecnie można określić lub wybierz określone połączenie z Brama VPN Azure. Należy określić ograniczeń z urządzenia VPN lokalnego. Ponadto trzeba kleszcze MSS u 1350.

### <a name="ike-phase-1-setup"></a>Ustawienia IKE etap 1

| **Właściwość**                                       | **PolicyBased** | **Brama RouteBased i standardowy lub wysokiej wydajności sieci VPN** |
|----------------------------------------------------|--------------------------------|------------------------------------------------------------------|
| Wersja IKE                                        | IKEv1                          | IKEv2                                                            |
| Grupa Diffie-Hellman                               | Grupa 2 (1024-bitowa)             | Grupa 2 (1024-bitowa)                                               |
| Metody uwierzytelniania                              | Klucz wstępny                 | Klucz wstępny                                                   |
| Algorytmy szyfrowania                              | AES256 AES128 3DES             | AES256 3DES                                                      |
| Algorytm mieszania                                  | SHA1(SHA128)                   | SHA1(SHA128), SHA2(SHA256)                                                     |
| Etap 1 zabezpieczeń skojarzenia istnienia (czas) | 28 800 sekund                 | 10,800 sekund                                                   |


### <a name="ike-phase-2-setup"></a>Ustawienia IKE etap 2

| **Właściwość**                                                             | **PolicyBased**                 | **Brama RouteBased i standardowy lub wysokiej wydajności sieci VPN**   |
|--------------------------------------------------------------------------|------------------------------------------------|--------------------------------------------------------------------|
| Wersja IKE                                                              | IKEv1                                          | IKEv2                                                              |
| Algorytm mieszania                                                        | SHA1(SHA128)                                   | SHA1(SHA128)                                                       |
| Istnienia skojarzenia zabezpieczeń fazy 2 (czas)                        | 3600 sekund                                  | 3600 sekund                                                                  |
| Istnienia skojarzenia zabezpieczeń fazy 2 (przepustowość)                  | 102,400,000 KB                                 | -                                                                  |
| Szyfrowanie SA IPsec i oferty uwierzytelniania (w kolejności) | 1. ESP-AES256 2. ESP AES128 3. ESP 3DES 4. N/D! | Zobacz *oferuje bramy RouteBased IPsec skojarzenia ()* (poniżej). |
| Doskonałe utajnienie (przekazywania PFS)                                            | Brak                                             | Nie (*)|
| Martwe równorzędnych wykrywania                                                      | Brak obsługi                                  | Obsługiwane                                                          |

(*) Azure bramy jako odpowiadającego IKE może zaakceptować PFS DH grupy 1, 2, 5, 14, 24.

### <a name="routebased-gateway-ipsec-security-association-sa-offers"></a>Udostępnia bramy RouteBased IPsec skojarzenia)

W poniższej tabeli wymieniono szyfrowanie SA IPsec i oferuje uwierzytelniania. Oferty są wyświetlane kolejności preferencji że oferty są prezentowane lub zaakceptowane.

| **Szyfrowanie IPsec skojarzenia zabezpieczeń i uwierzytelniania ofert** | **Azure bramy jako inicjator**                               | **Azure bramy jako odpowiadającego**                                   |
|---------------------------------------------------|--------------------------------------------------------------|--------------------------------------------------------------|
| 1                                                 | AGENT KONDYCJI SYSTEMU ESP AES_256                                              | AGENT KONDYCJI SYSTEMU ESP AES_128                                              |
| 2                                                 | AGENT KONDYCJI SYSTEMU ESP AES_128                                              | ESP 3_DES MD5                                                |
| 3                                                 | ESP 3_DES MD5                                                | AGENT KONDYCJI SYSTEMU ESP 3_DES                                                |
| 4                                                 | AGENT KONDYCJI SYSTEMU ESP 3_DES                                                | SHA1 AH z ESP AES_128 z HMAC zawiera wartości null                      |
| 5                                                 | SHA1 AH z ESP AES_256 z HMAC zawiera wartości null                      | SHA1 AH z ESP 3_DES z null HMAC                        |
| 6                                                 | SHA1 AH z ESP AES_128 z HMAC zawiera wartości null                      | AH MD5 z ESP 3_DES z HMAC null, nie istnienia proponowany |
| 7                                                 | SHA1 AH z ESP 3_DES z null HMAC                        | SHA1 AH z ESP 3_DES SHA1, nie istnienia                    |
| 8                                                 | AH MD5 z ESP 3_DES z HMAC null, nie istnienia proponowany | AH MD5 z ESP 3_DES MD5, nie istnienia                     |
| 9                                                 | SHA1 AH z ESP 3_DES SHA1, nie istnienia                    | ESP DES MD5                                                  |
| 10                                                | AH MD5 z ESP 3_DES MD5, nie istnienia                     | ESP DES SHA1, nie istnienia                                   |
| 11                                                | ESP DES MD5                                                  | Tak jak SHA1 z ESP DES null HMAC, nie istnienia proponowany        |
| 12                                                | ESP DES SHA1, nie istnienia                                   | Tak jak MD5 z ESP DES null HMAC, nie istnienia proponowany        |
| 13                                                | Tak jak SHA1 z ESP DES null HMAC, nie istnienia proponowany        | SHA1 AH z ESP DES SHA1, nie istnienia                      |
| 14                                                | Tak jak MD5 z ESP DES null HMAC, nie istnienia proponowany        | AH MD5 z ESP DES MD5, nie istnienia                       |
| 15                                                | SHA1 AH z ESP DES SHA1, nie istnienia                      | Agent kondycji systemu ESP, nie istnienia                                        |
| 16                                                | AH MD5 z ESP DES MD5, nie istnienia                       | ESP MD5, nie istnienia                                        |
| 17                                                | -                                                            | Tak jak agent kondycji systemu, nie istnienia                                         |
| 18                                                | -                                                            | AH MD5 nie istnienia                                        |


- Szyfrowanie IPsec ESP NULL można określić za pomocą bramy RouteBased i wysokiej wydajności sieci VPN. Zależności szyfrowania nie zapewnia ochrony danych w drodze i tylko jeśli należy użyć maksymalna przepustowość i minimalna opóźnienie jest wymagane.  Klienci mogą wybrać to scenariusze komunikacji VNet do VNet lub dla szyfrowania stosowana jest gdzie indziej w rozwiązaniu.

- Łączność między lokalnej za pośrednictwem Internetu za pomocą bramy Azure VPN domyślne ustawienia szyfrowania i algorytmów mieszania wymienionych w tabelach powyżej w celu zabezpieczenia komunikacji krytyczne.





