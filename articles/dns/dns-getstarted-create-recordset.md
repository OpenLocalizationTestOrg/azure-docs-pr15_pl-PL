<properties
   pageTitle="Tworzenie zestawu rekordów i rekordy strefy DNS przy użyciu programu PowerShell | Microsoft Azure"
   description="Jak utworzyć rekordy hosta DNS Azure. Konfigurowanie rekordu ustawia i rekordów przy użyciu programu PowerShell"
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



# <a name="create-dns-record-sets-and-records-by-using-powershell"></a>Tworzenie zestawów rekordów DNS i rekordów przy użyciu programu PowerShell


> [AZURE.SELECTOR]
- [Azure Portal](dns-getstarted-create-recordset-portal.md)
- [Programu PowerShell](dns-getstarted-create-recordset.md)
- [Polecenie Azure](dns-getstarted-create-recordset-cli.md)

W tym artykule przeprowadzi Cię przez proces tworzenia rekordów i zestawy rekordów przy użyciu programu Windows PowerShell. Po utworzeniu swoją strefę DNS, możesz dodać rekordy DNS dla swojej domeny. Aby to zrobić, najpierw należy zrozumieć rekordów DNS i zestawy rekordów.

[AZURE.INCLUDE [dns-about-records-include](../../includes/dns-about-records-include.md)]

## <a name="verify-that-you-have-the-latest-version-of-powershell"></a>Sprawdź, czy masz najnowszą wersję programu PowerShell

Upewnij się, że zainstalowano najnowszą wersję pakietu poleceń cmdlet programu PowerShell Menedżera zasobów Azure. Aby uzyskać więcej informacji o instalowaniu poleceń cmdlet programu PowerShell, zobacz [jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md) .

## <a name="create-a-record-set-and-record"></a>Tworzenie zestawu rekordów i rekordu

W tej sekcji opisano, jak utworzyć zestaw rekordów i rekordu.


### <a name="1-connect-to-your-subscription"></a>1. nawiązywanie połączenia z subskrypcji

Otwórz konsoli programu PowerShell i łączenia się z kontem. Użyj Poniższy przykładowy umożliwiające nawiązywanie połączeń:

    Login-AzureRmAccount

Sprawdzanie subskrypcji dla tego konta.

    Get-AzureRmSubscription

Określ subskrypcję, do której chcesz użyć.

    Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"

Aby uzyskać więcej informacji na temat pracy z programem PowerShell zobacz [Przy użyciu programu Windows PowerShell przy użyciu Menedżera zasobów](../powershell-azure-resource-manager.md).


### <a name="2-create-a-record-set"></a>2. Utwórz zestaw rekordów

Tworzenie zestawów rekordów za pomocą `New-AzureRmDnsRecordSet` polecenia cmdlet. Podczas tworzenia zestawu rekordów, musisz określić zestawu rekord nazwy, strefę czasową, time to live (TTL) i typ rekordu.

Do utworzenia rekordu w wierzchołka strefy (w tym przypadku "contoso.com"), użyj nazwy rekordu "@", wraz ze znakami cudzysłowu. Jest to typowe Konwencja DNS.

Poniższy przykład tworzy rekord Ustawianie o nazwie względnych "www" w strefie DNS "contoso.com". W pełni kwalifikowaną nazwę zestawu rekordów jest "www.contoso.com". Typ rekordu jest "A", a wartość TTL jest 60 sekund. Po zakończeniu tych czynności należy zestaw rekordów pustego ciągu "www" przypisany do zmiennej *$rs*.

    $rs = New-AzureRmDnsRecordSet -Name "www" -RecordType "A" -ZoneName "contoso.com" -ResourceGroupName "MyAzureResourceGroup" -Ttl 60

#### <a name="if-a-record-set-already-exists"></a>Jeśli rekord już istnieje

Jeśli istnieje już zestawie rekordów, polecenie kończy się niepowodzeniem, chyba że *-zastąpić* przełącznik jest używany. *-Zastąpić* opcję wyzwalane monit o potwierdzenie, który można pominąć przy użyciu *-życie* przełączanie.


    $rs = New-AzureRmDnsRecordSet -Name www -RecordType A -Ttl 300 -ZoneName contoso.com -ResouceGroupName MyAzureResouceGroup [-Tag $tags] [-Overwrite] [-Force]


W tym przykładzie Określ strefę przy użyciu nazwy strefy i nazwa grupy zasobów. Alternatywnie możesz określić obiekt strefy zwracane przez `Get-AzureRmDnsZone` lub `New-AzureRmDnsZone`.

    $zone = Get-AzureRmDnsZone -Name contoso.com –ResourceGroupName MyAzureResourceGroup
    $rs = New-AzureRmDnsRecordSet -Name www -RecordType A -Ttl 300 –Zone $zone [-Tag $tags] [-Overwrite] [-Force]

`New-AzureRmDnsRecordSet`Zwraca obiekt, który reprezentuje zestaw rekordów, który został utworzony w systemie DNS Azure.

### <a name="3-add-a-record"></a>3. Dodawanie rekordu

Aby użyć zestawu rekordów nowo utworzonego "www", należy dodać rekordy do niego. Możesz dodać IPv4 rekordy *A* , aby rekord "www" ustawić przy użyciu w poniższym przykładzie. W tym przykładzie zależy od zmiennych *$rs* ustawiany w poprzednim kroku.

Dodawanie rekordów z rekordem ustawić przy użyciu `Add-AzureRmDnsRecordConfig` jest operacji w trybie offline. Tylko lokalne zmiennych *$rs* jest aktualizowana.


    Add-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address 134.170.185.46
    Add-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address 134.170.188.221

### <a name="4-commit-the-changes"></a>4. Zatwierdź zmiany

Zatwierdź zmiany w zestawie rekordów. Używanie `Set-AzureRmDnsRecordSet` Aby przekazać zmiany do zestawu rekordów do Azure DNS.

    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="5-retrieve-the-record-set"></a>5. pobrać zestawu rekordów

Można podjąć zestawie rekordów z Azure DNS za pomocą `Get-AzureRmDnsRecordSet` jak pokazano w poniższym przykładzie.


    Get-AzureRmDnsRecordSet –Name www –RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup


    Name              : www
    ZoneName          : contoso.com
    ResourceGroupName : MyAzureResourceGroup
    Ttl               : 3600
    Etag              : 68e78da2-4d74-413e-8c3d-331ca48246d9
    RecordType        : A
    Records           : {134.170.185.46, 134.170.188.221}
    Tags              : {}


Aby wykonać kwerendę dotyczącą nowego zestawu rekordów, można użyć narzędzia nslookup lub innego narzędzia systemu DNS.

Jeśli jeszcze nie delegowane domeny serwery nazw Azure DNS, należy jawnie określić nazwę serwera i adres strefy.


    nslookup www.contoso.com ns1-01.azure-dns.com

    Server: ns1-01.azure-dns.com
    Address:  208.76.47.1

    Name:    www.contoso.com
    Addresses:  134.170.185.46
                134.170.188.221

## <a name="create-a-record-set-of-each-type-with-a-single-record"></a>Tworzenie zestawu rekordów każdego typu z jednym rekordem


W poniższych przykładach pokazano, jak utworzyć zestaw rekordów każdego typu rekordu. Każdy zestaw rekordów zawiera jeden rekord.

[AZURE.INCLUDE [dns-add-record-ps-include](../../includes/dns-add-record-ps-include.md)]


## <a name="next-steps"></a>Następne kroki

[Jak zarządzać strefy DNS przy użyciu programu PowerShell](dns-operations-dnszones.md)

[Zarządzanie rekordami DNS i zestawy rekordów przy użyciu programu PowerShell](dns-operations-recordsets.md)

[Automatyzowanie Azure operacji .NET SDK](dns-sdk.md)
