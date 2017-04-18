<properties
    pageTitle="Konfigurowanie połączenia VPN w zarządzaniu interfejsu API platformy Azure"
    description="Dowiedz się, jak skonfigurować połączenie VPN w zarządzaniu interfejsu API Azure i usług sieci web programu access przez niego."
    services="api-management"
    documentationCenter=""
    authors="antonba"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="antonba"/>

# <a name="how-to-setup-vpn-connections-in-azure-api-management"></a>Konfigurowanie połączenia VPN w zarządzaniu interfejsu API platformy Azure

Obsługa sieci VPN interfejsu API zarządzania zezwala na łączenie interfejsu API zarządzania uzyskanie wirtualną sieć Azure (klasyczny). Dzięki temu klienci interfejsu API zarządzania zapewnić bezpieczne połączenie do ich wewnętrznej bazy danych usługi sieci web, które są lokalnego lub są niedostępne do publicznego Internetu, w przeciwnym razie.

>[AZURE.NOTE] Azure zarządzania interfejsu API współdziała z VNETs klasycznego. Aby uzyskać informacje o tworzeniu klasyczny VNET zobacz [Tworzenie wirtualnej sieci (klasyczny) przy użyciu Azure Portal](../virtual-network/virtual-networks-create-vnet-classic-pportal.md). Aby uzyskać informacje dotyczące łączenia klasycznego VNETs się ARM VNETS zobacz [Łączenie klasycznego VNets do nowych VNets](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md).

## <a name="enable-vpn"> </a>Połączenia VPN Włączanie

>Połączenie VPN jest dostępna tylko w poziomów **Premium** i **Deweloper** . Aby przełączyć się do niego, otwórz usługi zarządzania interfejsu API w [Klasycznym Portal Azure][] , a następnie otwórz kartę **Skala** . W sekcji **Ogólne** wybierz poziom Premium, a następnie kliknij przycisk Zapisz.

Aby włączyć połączenia VPN, otwórz usługi zarządzania interfejsu API w [Klasycznym Portal Azure][] i przejdź na kartę **Konfiguruj** . 

W sekcji VPN przełączanie **połączenia VPN** **w**.

![Konfigurowanie karty wystąpienia interfejsu API zarządzania][api-management-setup-vpn-configure]

Będzie teraz wyświetlić listę wszystkich regionów, gdzie jest obsługi administracyjnej usługi zarządzania interfejsu API.

Wybierz opcję VPN i podsieć dla wszystkich regionów. Lista sieci VPN zostanie wypełniona, oparte na sieci wirtualnych dostępnych w ramach subskrypcji Azure skonfigurowanymi w regionie, którą konfigurujesz.

![Zaznacz opcję sieć VPN][api-management-setup-vpn-select]

Kliknij pozycję **Zapisz** u dołu ekranu. Nie można wykonywać innych operacji zarządzania interfejsu API usługi z portalu klasyczny Azure, podczas aktualizacji. Brama usługi pozostanie dostępny i nie narusza obsługi połączeń.

Zauważ, że adres VIP bramy będzie zmieniać zawsze VPN jest włączone lub wyłączone.

## <a name="connect-vpn"> </a>Nawiązywanie połączenia z usługi sieci web za VPN

Po połączeniu usługi zarządzania interfejsu API sieci VPN, uzyskiwanie dostępu do usług sieci web w sieci wirtualnej nie różni się od uzyskiwania dostępu do usług publicznych. Po prostu wpisz lokalny adres lub nazwa hosta (Jeśli serwer DNS został skonfigurowany dla wirtualnej sieci Azure) usługi sieci web w polu **adres URL usługi sieci Web** podczas tworzenia nowego interfejsu API usługi lub edytowania istniejącego wzornika.

![Dodawanie interfejsu API z sieci VPN][api-management-setup-vpn-add-api]

## <a name="required-ports-for-api-management-vpn-support"></a>Wymagane porty obsługę sieci VPN zarządzania interfejsu API

Kiedy wystąpienie usługi zarządzania interfejsu API znajduje się w VNET, są używane porty w poniższej tabeli. Te porty są zablokowane, usługa może nie działać prawidłowo. Co najmniej jedną z tych portów zablokowane jest najczęściej problem konfiguracji podczas korzystania z VNET interfejsu API zarządzania.

| Porty                      | Kierunek        | Protokół transportowy | Cel                                                          | Źródło i miejsce docelowe              |
|------------------------------|------------------|--------------------|------------------------------------------------------------------|-----------------------------------|
| porty 80, 443                      | Ruch przychodzący          | PORT TCP                | Komunikacja z klientem do zarządzania interfejsu API                           | INTERNET-VIRTUAL_NETWORK        |
| 80,443                       | Wychodzące         | PORT TCP                | Interfejs API zarządzania zależność Bus usługi Azure i magazynowania Azure | VIRTUAL_NETWORK / INTERNET        |
| 1433                         | Wychodzące         | PORT TCP                | Interfejs API zarządzania współzależnościami SQL                               | VIRTUAL_NETWORK / INTERNET        |
| 9350, 9351, 9352, 9353, 9354 | Wychodzące         | PORT TCP                | Interfejs API zarządzania współzależnościami Bus usługi                       | VIRTUAL_NETWORK / INTERNET        |
| 5671                         | Wychodzące         | AMQP               | Zależność od interfejsu API zarządzania dziennika zdarzeń Centrum zasad            | VIRTUAL_NETWORK / INTERNET        |
| 6381, 6382, 6383             | Ruch przychodzący i wychodzący | UDP                | Interfejs API zarządzania współzależnościami Redis pamięci podręcznej                       | VIRTUAL_NETWORK-VIRTUAL_NETWORK |
| 445                          | Wychodzące         | PORT TCP                | Interfejs API zarządzania zależności w udziale plików Azure dla CYFRA            | VIRTUAL_NETWORK / INTERNET        |

## <a name="custom-dns"> </a>Konfiguracji serwera DNS niestandardowe

Interfejs API zarządzania zależy od liczby usług Azure. Kiedy wystąpienie usługi zarządzania interfejsu API znajduje się w VNET używania niestandardowych serwera DNS, musi mieć możliwość rozpoznawania nazwy hostów tych usług Azure. Wykonaj [te](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server) wskazówki na niestandardowe ustawienia DNS.  

## <a name="related-content"> </a>Dotyczące zawartości


* [Tworzenie wirtualnych sieci przy użyciu połączenie VPN witryny do witryny za pomocą portalu klasyczny Azure][]
* [Jak za pomocą Inspektora interfejsu API do śledzenia połączeń w zarządzaniu interfejsu API platformy Azure][]

[api-management-setup-vpn-configure]: ./media/api-management-howto-setup-vpn/api-management-setup-vpn-configure.png
[api-management-setup-vpn-select]: ./media/api-management-howto-setup-vpn/api-management-setup-vpn-select.png
[api-management-setup-vpn-add-api]: ./media/api-management-howto-setup-vpn/api-management-setup-vpn-add-api.png

[Enable VPN connections]: #enable-vpn
[Connect to a web service behind VPN]: #connect-vpn
[Related content]: #related-content

[Portal Azure klasyczny]: https://manage.windowsazure.com/

[Tworzenie wirtualnych sieci przy użyciu połączenie VPN witryny do witryny za pomocą portalu klasyczny Azure]: ../vpn-gateway/vpn-gateway-site-to-site-create.md
[Jak za pomocą Inspektora interfejsu API do śledzenia połączeń w zarządzaniu interfejsu API platformy Azure]: api-management-howto-api-inspector.md
