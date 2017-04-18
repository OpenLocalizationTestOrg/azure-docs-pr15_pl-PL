<properties
   pageTitle="Tworzenie strefy DNS przy użyciu interfejsu wiersza polecenia | Microsoft Azure"
   description="Dowiedz się, jak utworzyć strefy DNS dla usługi Azure DNS krok po kroku, aby rozpocząć hostingu DNS domeny przy użyciu interfejsu wiersza polecenia"
   services="dns"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/16/2016"
   ms.author="sewhee"/>

# <a name="create-an-azure-dns-zone-using-cli"></a>Tworzenie strefy Azure DNS za pomocą interfejsu wiersza polecenia


> [AZURE.SELECTOR]
- [Azure Portal](dns-getstarted-create-dnszone-portal.md)
- [Programu PowerShell](dns-getstarted-create-dnszone.md)
- [Polecenie Azure](dns-getstarted-create-dnszone-cli.md)


W tym artykule będzie szczegółową procedurę tworzenia strefy DNS za pomocą interfejsu wiersza polecenia. Możesz również utworzyć przy użyciu programu PowerShell lub Azure portal strefy DNS.

[AZURE.INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]


## <a name="before-you-begin"></a>Przed rozpoczęciem

Te instrukcje za pomocą interfejsu wiersza polecenia programu Microsoft Azure. Pamiętaj zaktualizować do najnowszej polecenie Azure (0.9.8 lub nowsza) korzystania z poleceń Azure DNS. Typ `azure -v` sprawdzić wersję polecenie Azure, która jest zainstalowana na komputerze.

## <a name="step-1---set-up-azure-cli"></a>Krok 1 — Konfigurowanie polecenie Azure

### <a name="1-install-azure-cli"></a>1. Instalowanie polecenie Azure

Możesz zainstalować Azure interfejsu wiersza polecenia dla systemu Windows, Linux lub MAC. Poniższe czynności muszą zostać ukończone, zanim będzie możliwe zarządzanie DNS Azure za pomocą interfejsu wiersza polecenia Azure. Więcej informacji znajduje się na [Instalowanie polecenie Azure](../xplat-cli-install.md). Polecenia DNS wymagają polecenie Azure wersji 0.9.8 lub nowszej.

Wszystkie polecenia dostawcy sieci na polecenie można znaleźć przy użyciu następującego polecenia:

    azure network

### <a name="2-switch-cli-mode"></a>2. tryb polecenie Przełącz

Azure DNS używa Azure Menedżera zasobów. Upewnij się, że Przełączanie trybu polecenie używać polecenia ARM.

    azure config mode arm

### <a name="3-sign-in-to-your-azure-account"></a>3. Zaloguj się do konta Azure

Wyświetli monit o poświadczenia uwierzytelniania. Należy pamiętać, że można używać tylko kont ZBĘDNA.

    azure login -u "username"

### <a name="4-select-the-subscription"></a>4. Wybierz subskrypcję

Wybranie Azure subskrypcji korzystać.

    azure account set "subscription name"

### <a name="5-create-a-resource-group"></a>5. utworzyć grupę zasobów

Azure Menedżera zasobów wymaga, aby wszystkie grupy zasobów określ lokalizację. To jest używany jako domyślnej lokalizacji dla zasobów w danej grupy zasobów. Jednak ponieważ wszystkie zasoby DNS są globalnego, nie regionalne, wybór lokalizacji grupa zasobów nie ma wpływu na Azure DNS.

Możesz pominąć ten krok, jeśli korzystasz z istniejącej grupy zasobów.

    azure group create -n myresourcegroup --location "West US"


### <a name="6-register"></a>6. Rejestruj

Usługa Azure DNS odbywa się przez dostawcę Microsoft.Network zasobów. Twoja subskrypcja Azure musi zostać zarejestrowany używać tego dostawcy zasobów, zanim będzie można używać Azure DNS. Jest to operacja jednorazowa dla każdej subskrypcji.

    azure provider register --namespace Microsoft.Network


## <a name="step-2---create-a-dns-zone"></a>Krok 2 — Tworzenie strefy DNS

Strefy DNS zostanie utworzona za pomocą `azure network dns zone create` polecenia. Opcjonalnie możesz utworzyć strefy DNS wraz z znaczniki. Znaczniki są Lista par wartości z pola Nazwa i są używane przez Menedżera zasobów Azure do zasobów etykiety na potrzeby rozliczeń lub grupowania. Aby uzyskać więcej informacji na temat znaczników zobacz [Używanie znaczników w celu organizowania Azure zasobów](../resource-group-using-tags.md).

W systemie DNS Azure, należy określić nazwy stref bez przerywania **".'**. Na przykład jako "**contoso.com**" zamiast "**contoso.com.**".


### <a name="to-create-a-dns-zone"></a>Aby utworzyć strefy DNS

W poniższym przykładzie tworzy strefy DNS o nazwie *contoso.com* w grupie zasobów o nazwie *MyResourceGroup*.

Przykład umożliwia utworzenie strefy DNS podstawianie wartości do własnego.

    azure network dns zone create myresourcegroup contoso.com

### <a name="to-create-a-dns-zone-and-tags"></a>Aby utworzyć strefy DNS i znaczników.

Polecenie Azure DNS obsługuje znaczniki strefy DNS określonego przy użyciu opcjonalne *-znacznika* parametru. W poniższym przykładzie pokazano, jak utworzyć strefy DNS z dwóch znaczników, projekt = pokaz i Koperta = test.

W poniższym przykładzie umożliwia utworzenie strefy DNS i znaczniki, zastępując własne wartości.

    azure network dns zone create myresourcegroup contoso.com -t "project=demo";"env=test"

## <a name="view-records"></a>Wyświetl rekordy

Tworzenie strefy DNS tworzy również poniższych rekordów DNS:

- "Uruchom z" rekordu SOA (). Występuje w katalogu głównym każdej strefy DNS.

- Autorytatywne rekordów serwera nazw (SN). Pokaż tych, które serwerów nazw znajdują się strefy. Azure DNS używa puli serwerów nazw, a więc różnych serwerów nazw można przypisywać do różnych stref w systemie DNS Azure. Aby uzyskać więcej informacji, zobacz [pełnomocnika domeny DNS Azure](dns-domain-delegation.md) .

Służy do wyświetlania tych rekordów `azure network dns-record-set show`.<BR>
*Sposób użycia: show dns sieci zestaw rekordów < — grupa zasobów >< dns zone nazwa-> <name><type>*


W poniższym przykładzie, jeśli uruchomienie polecenia z grupy zasobów *myresourcegroup*, rekord skonfiguruj nazwę *"@"* (w przypadku rekord główny) i wpisz *SOA*, jego dadzą następujący wynik:


    azure network dns record-set show myresourcegroup "contoso.com" "@" SOA
    info:    Executing command network dns-record-set show
    + Looking up the DNS record set "@"
    data:    Id                              : /subscriptions/#######################/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/SOA/@
    data:    Name                            : @
    data:    Type                            : Microsoft.Network/dnszones/SOA
    data:    Location                        : global
    data:    TTL                             : 3600
    data:    SOA record:
    data:      Email                         : msnhst.microsoft.com
    data:      Expire time                   : 604800
    data:      Host                          : edge1.azuredns-cloud.net
    data:      Minimum TTL                   : 300
    data:      Refresh time                  : 900
    data:      Retry time                    : 300
    data:                                    :
<BR>
Aby wyświetlić rekordy serwera nazw utworzone za pomocą strefę czasową, użyj następującego polecenia:

    azure network dns record-set show myresourcegroup "contoso.com" "@" NS
    info:    Executing command network dns-record-set show
    + Looking up the DNS record set "@"
    data:    Id                              : /subscriptions/#######################/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/NS/@
    data:    Name                            : @
    data:    Type                            : Microsoft.Network/dnszones/NS
    data:    Location                        : global
    data:    TTL                             : 3600
    data:    NS records
    data:        Name server domain name     : ns1-05.azure-dns.com
    data:        Name server domain name     : ns2-05.azure-dns.net
    data:        Name server domain name     : ns3-05.azure-dns.org
    data:        Name server domain name     : ns4-05.azure-dns.info
    data:
    info:    network dns-record-set show command OK

>[AZURE.NOTE] Rekord zestawów w głównym (lub *wierzchołka*) Użyj strefy DNS **@** jako nazwa zestawu rekordów.

## <a name="test"></a>Test

Istnieje możliwość przetestowania swoją strefę DNS przy użyciu narzędzia systemu DNS, takie jak nslookup, WYKOPANIE, lub `Resolve-DnsName` polecenia cmdlet programu PowerShell.

Jeśli nie zostało jeszcze delegowane domeny do nowej strefy w systemie DNS Azure za pomocą, należy bezpośredni kwerendy DNS bezpośrednio do jednego z serwerów nazw strefy. Serwery nazw strefy podano w rekordów serwera nazw, zgodnie z treścią "Pokaż zestaw rekordów dns sieci azure" powyżej. Upewnij się, że zastępczych poprawne wartości dla swoją strefę w poleceniu poniżej.

W poniższym przykładzie użyto WYKOPANIE kwerendy przy użyciu serwerów nazw przypisane do strefy DNS domeny contoso.com. Kwerenda zawiera wskazywały serwer nazw, dla którego użyliśmy * @ * i nazwę strefy za pomocą WYKOPANIE.

     <<>> DiG 9.10.2-P2 <<>> @ns1-05.azure-dns.com contoso.com
    (1 server found)
    global options: +cmd
    Got answer:
    ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 60963
    flags: qr aa rd; QUERY: 1, ANSWER: 0, AUTHORITY: 1, ADDITIONAL: 1
    WARNING: recursion requested but not available

    OPT PSEUDOSECTION:
    EDNS: version: 0, flags:; udp: 4000
    QUESTION SECTION:
    contoso.com.                        IN      A

    AUTHORITY SECTION:
    contoso.com.         300     IN      SOA     edge1.azuredns-cloud.net.
    msnhst.microsoft.com. 6 900 300 604800 300

    Query time: 93 msec
    SERVER: 208.76.47.5#53(208.76.47.5)
    WHEN: Tue Jul 21 16:04:51 Pacific Daylight Time 2015
    MSG SIZE  rcvd: 120

## <a name="next-steps"></a>Następne kroki

Po utworzeniu strefy DNS, utworzyć [zestawy rekordów i rekordy](dns-getstarted-create-recordset-cli.md) uruchomić rozpoznawanie nazw dla domeny internetowej.
