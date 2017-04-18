<properties
   pageTitle="Rozpoczynanie pracy z usługą DNS Azure | Microsoft Azure"
   description="Dowiedz się, jak utworzyć strefy DNS dla usługi Azure DNS. To jest krok po kroku, aby przejść do pierwszej strefy DNS na utworzony w celu uruchamiania hostingu DNS domeny przy użyciu programu PowerShell."
   services="dns"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/16/2016"
   ms.author="sewhee"/>

# <a name="create-a-dns-zone-using-powershell"></a>Tworzenie strefy DNS przy użyciu programu Powershell

> [AZURE.SELECTOR]
- [Azure Portal](dns-getstarted-create-dnszone-portal.md)
- [Programu PowerShell](dns-getstarted-create-dnszone.md)
- [Polecenie Azure](dns-getstarted-create-dnszone-cli.md)

W tym artykule będzie szczegółową procedurę tworzenia strefy DNS przy użyciu programu PowerShell. Można także tworzyć strefy DNS przy użyciu interfejsu wiersza polecenia lub Azure portal.

[AZURE.INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]

## <a name="tagetag"></a>Informacje o Etags i znaczników

### <a name="etags"></a>Etags

Załóżmy, że dwie osoby lub procesów dwie próby modyfikowania rekordu DNS w tym samym czasie. Który wins? I będzie pierwszym miejscu wiadomo, że został właśnie zastąpione zmiany utworzone przez inną osobę?

Azure DNS używa Etags bezpiecznie obsługiwać równoległe zmiany do tego samego zasobu. Każdemu zasobowi DNS (strefa lub zestawu rekordów) ma Etag skojarzone z nim. Po pobraniu zasobu jego Etag również zostanie pobrana. Podczas aktualizowania zasobu, masz opcję, aby przesłać Etag więc sprawdzić Azure DNS Etag na dopasowania serwera. Ponieważ każdej aktualizacji do zasobu powoduje Etag są generowane, niezgodności Etag wskazuje, że zmiana równoczesne. Etags są również używane podczas tworzenia nowego zasobu aby upewnić się, że zasób jeszcze nie istnieje.

Domyślnie Azure programu PowerShell DNS używa Etags zablokować równoległych zmian do stref i zestawy rekordów. Opcjonalne *-zastąpić* Przełącz mogą być używane pomijany Etag testy, w tym przypadku równoległych zmian, które wystąpiły zostaną zastąpione.

Na poziomie interfejsu API usługi REST DNS Azure Etags określono za pomocą nagłówków HTTP.  W poniższej tabeli znajduje się działają tak samo jak:

|Nagłówek|Zachowanie|
|------|--------|
|Brak|POŁOŻENIE zawsze powodzeniem (nie kontrole Etag)|
|Jeżeli dopasowania<etag>|POŁOŻENIE powiedzie się tylko, jeśli istnieje zasobów i odpowiada Etag|
|Jeżeli dopasowanie *     | POŁOŻENIE powiedzie się tylko, jeśli istnieje zasobów|
|Jeżeli żaden dopasowania * |  POŁOŻENIE powiedzie się tylko, jeśli nie istnieje zasobów|

### <a name="tags"></a>Znaczniki

Znaczniki różnią się od Etags. Znaczniki są Lista par wartości z pola Nazwa i są używane przez Menedżera zasobów Azure do zasobów etykiety na potrzeby rozliczeń lub grupowania. Aby uzyskać więcej informacji na temat znaczników zobacz [Używanie znaczników w celu organizowania Azure zasobów](../resource-group-using-tags.md).

Azure DNS PowerShell obsługuje znaczniki zarówno na strefy i zestawy rekordów, określona przy użyciu opcji `-Tag` parametru.


## <a name="before-you-begin"></a>Przed rozpoczęciem

Sprawdź, czy masz następujące elementy przed rozpoczęciem konfiguracji.

- Subskrypcję usługi Azure. Jeśli nie masz jeszcze subskrypcji usługi Azure, można aktywować swoje [korzyści subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) lub Utwórz konto [bezpłatne konto](https://azure.microsoft.com/pricing/free-trial/).

- Musisz zainstalować najnowszą wersję programu PowerShell Menedżera zasobów Azure poleceń cmdlet (1.0 lub nowsza). Aby uzyskać więcej informacji o instalowaniu poleceń cmdlet programu PowerShell, zobacz [jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md) .

## <a name="step-1---sign-in"></a>Krok 1 — Logowanie

Otwórz konsoli programu PowerShell i łączenia się z kontem. Aby uzyskać więcej informacji zobacz [Przy użyciu programu Windows PowerShell przy użyciu Menedżera zasobów](../powershell-azure-resource-manager.md).

Użyj Poniższy przykładowy umożliwiające nawiązywanie połączeń:

    Login-AzureRmAccount

Sprawdzanie subskrypcji dla tego konta.

    Get-AzureRmSubscription

Określ subskrypcję, do której chcesz użyć.

    Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"

## <a name="step-2---create-a-resource-group"></a>Krok 2 — utworzyć grupę zasobów

Azure Menedżera zasobów wymaga, aby wszystkie grupy zasobów określ lokalizację. To jest używany jako domyślnej lokalizacji dla zasobów w danej grupy zasobów. Jednak ponieważ wszystkie zasoby DNS są globalnego, nie regionalne, wybór lokalizacji grupa zasobów nie ma wpływu na Azure DNS.

Możesz pominąć ten krok, jeśli korzystasz z istniejącej grupy zasobów.

    New-AzureRmResourceGroup -Name MyAzureResourceGroup -location "West US"


## <a name="step-3---register"></a>Krok 3 — Rejestruj

Usługa Azure DNS odbywa się przez dostawcę Microsoft.Network zasobów. Twoja subskrypcja Azure musi zostać zarejestrowany używać tego dostawcy zasobów, zanim będzie można używać Azure DNS. Jest to operacja jednorazowa dla każdej subskrypcji.

    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network


## <a name="step-4----create-a-dns-zone"></a>Krok 4 — tworzenie strefy DNS

Strefy DNS zostanie utworzona za pomocą `New-AzureRmDnsZone` polecenia cmdlet. Istnieją przykłady poniżej tworzenia strefy DNS z lub bez znaczników. Aby uzyskać więcej informacji na temat znaczników zobacz sekcję na [znaczników](#tags) w tym artykule.

>[AZURE.NOTE] W systemie DNS Azure, należy określić nazwy stref bez przerywania **".'**. Na przykład jako "**contoso.com**" zamiast "**contoso.com.**".

### <a name="to-create-a-dns-zone"></a>Aby utworzyć strefy DNS

W poniższym przykładzie tworzy strefy DNS o nazwie *contoso.com* w grupie zasobów o nazwie *MyResourceGroup*. Przykład umożliwia utworzenie strefy DNS podstawianie własne wartości.

    New-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup

### <a name="to-create-a-dns-zone-with-tags"></a>Aby utworzyć strefy DNS ze znacznikami

W poniższym przykładzie pokazano sposób tworzenia strefy DNS z dwóch znaczników *projektu = pokaz* i *Koperta = test*. Przykład umożliwia utworzenie strefy DNS podstawianie własne wartości.

    New-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup -Tag @( @{ Name="project"; Value="demo" }, @{ Name="env"; Value="test" } )

## <a name="view-records"></a>Wyświetl rekordy

Tworzenie strefy DNS tworzy również poniższych rekordów DNS:

- Rekord *Uruchom urząd* (SOA). Występuje w katalogu głównym każdej strefy DNS.

- Autorytatywne rekordów serwera nazw (SN). Pokaż tych, które serwerów nazw znajdują się strefy. Azure DNS używa puli serwerów nazw, a więc różnych serwerów nazw może być przypisana do innej strefy w systemie DNS Azure. Zobacz [Delegowanie domeny DNS Azure](dns-domain-delegation.md) , aby uzyskać więcej informacji.

Służy do wyświetlania tych rekordów `Get-AzureRmDnsRecordSet`:

    Get-AzureRmDnsRecordSet -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup

    Name              : @
    ZoneName          : contoso.com
    ResourceGroupName : MyResourceGroup
    Ttl               : 3600
    Etag              : 2b855de1-5c7e-4038-bfff-3a9e55b49caf
    RecordType        : SOA
    Records           : {[ns1-01.azure-dns.com,msnhst.microsoft.com,900,300,604800,300]}
    Tags              : {}

    Name              : @
    ZoneName          : contoso.com
    ResourceGroupName : MyResourceGroup
    Ttl               : 3600
    Etag              : 5fe92e48-cc76-4912-a78c-7652d362ca18
    RecordType        : NS
    Records           : {ns1-01.azure-dns.com, ns2-01.azure-dns.net, ns3-01.azure-dns.org,
                  ns4-01.azure-dns.info}
    Tags              : {}


Rekord zestawów w głównym (lub *wierzchołka*) Użyj strefy DNS **@** jako nazwa zestawu rekordów.


## <a name="test"></a>Test

Za pomocą narzędzia systemu DNS, takich jak nslookup, wykopanie lub [polecenia cmdlet programu PowerShell Nazwa_dns rozwiązania problemu](https://technet.microsoft.com/library/jj590781.aspx), możesz przetestować swoją strefę DNS.

Jeśli jeszcze nie delegowane domeny do nowej strefy w systemie DNS Azure za pomocą, będzie konieczne bezpośredni kwerendy DNS bezpośrednio do jednego z serwerów nazw strefy. Serwery nazw strefy podano w rekordów serwera nazw, zgodnie z treścią `Get-AzureRmDnsRecordSet` powyżej. Upewnij się, że zastępczych poprawne wartości dla Twojej strefy na poniższe polecenie.

    nslookup
    > set type=SOA
    > server ns1-01.azure-dns.com
    > contoso.com

    Server: ns1-01.azure-dns.com
    Address:  208.76.47.1

    contoso.com
            primary name server = ns1-01.azure-dns.com
            responsible mail addr = msnhst.microsoft.com
            serial  = 1
            refresh = 900 (15 mins)
            retry   = 300 (5 mins)
            expire  = 604800 (7 days)
            default TTL = 300 (5 mins)


## <a name="next-steps"></a>Następne kroki

Po utworzeniu strefy DNS, utworzyć [zestawy rekordów i rekordy](dns-getstarted-create-recordset.md) uruchomić rozpoznawanie nazw dla domeny internetowej.

