<properties
   pageTitle="Zarządzanie zestawy rekordów DNS i rekordów przy użyciu Azure portal | Microsoft Azure"
   description="Zarządzanie zestawy rekordów DNS i rekordy Azure DNS hostingu własną domeną w usłudze Azure DNS. Wszystkie polecenia programu PowerShell dla operacji na zestawach rekordów i rekordów."
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

# <a name="manage-dns-records-and-record-sets-by-using-powershell"></a>Zarządzanie rekordami DNS i zestawy rekordów przy użyciu programu PowerShell



> [AZURE.SELECTOR]
- [Azure Portal](dns-operations-recordsets-portal.md)
- [Polecenie Azure](dns-operations-recordsets-cli.md)
- [Programu PowerShell](dns-operations-recordsets.md)



W tym artykule pokazano, jak zarządzać zestawy rekordów i rekordy strefy DNS przy użyciu programu Windows PowerShell.

Należy opis różnic między zestawy rekordów DNS i poszczególnych rekordów DNS. Zestaw rekordów jest zbiór rekordów w strefie, które mają taką samą nazwę i tego samego typu. Aby uzyskać więcej informacji zobacz [zestawy rekordów DNS Utwórz i rekordów przy użyciu Azure portal](dns-getstarted-create-recordset-portal.md).

Zarządzanie zestawy rekordów i rekordów, wymaga najnowszej wersji pakietu poleceń cmdlet programu PowerShell Menedżera zasobów Azure. Aby uzyskać więcej informacji zobacz [jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md). Aby uzyskać więcej informacji na temat pracy z programem PowerShell zobacz [Za pomocą programu Azure przy użyciu Menedżera zasobów Azure](../powershell-azure-resource-manager.md).



## <a name="create-a-new-record-set-and-a-record"></a>Tworzenie nowego zestawu rekordów i rekordu

Aby utworzyć rekord ustawić przy użyciu programu PowerShell, zobacz [zestawy rekordów DNS Utwórz i rekordów przy użyciu programu PowerShell](dns-getstarted-create-recordset.md).

## <a name="get-a-record-set"></a>Pobierz zestaw rekordów

Aby pobrać istniejący zestaw rekordów, należy użyć `Get-AzureRmDnsRecordSet`. Określ zestawu rekord względną nazwę, typ rekordu i strefę czasową.

    $rs = Get-AzureRmDnsRecordSet –Name www –RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup

W przypadku `New-AzureRmDnsRecordSet`, Nazwa rekordu musi być względną nazwę, co oznacza trzeba wykluczyć nazwę strefy.

Można określić strefę czasową, przy użyciu nazwy strefy i nazwa grupy zasobów lub za pomocą obiektu strefy:

    $zone = Get-AzureRmDnsZone -Name contoso.com -ResouceGroupName MyAzureResourceGroup
    $rs = Get-AzureRmDnsRecordSet -Name www –RecordType A -Zone $zone

`Get-AzureRmDnsRecordSet`Zwraca obiekt, który reprezentuje zestaw rekordów, który został utworzony w systemie DNS Azure.

## <a name="list-record-sets"></a>Zestawy rekordów listy

Można również użyć`Get-AzureRmDnsRecordSet` do listy zestawów rekordów w przypadku pominięcia *— nazwę* i/lub *RecordType —* parametry.

### <a name="to-list-all-record-sets"></a>Aby wyświetlić listę wszystkich zestawach rekordów

W tym przykładzie zwraca wszystkie zestawy rekordów, niezależnie od tego, czy nazwy lub typu rekordu:

    $list = Get-AzureRmDnsRecordSet -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup

### <a name="to-list-record-sets-of-a-given-record-type"></a>Do listy zestawów rekordów danego typu rekordu

W tym przykładzie zwraca wszystkie zestawy rekordów, które są zgodne z typem rekordu danego. W tym przypadku rekord ustawić to jest zwracany jest rekordy "A":

    $list = Get-AzureRmDnsRecordSet –RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup

Strefy można określić przy użyciu albo *— Nazwa_strefy* i *ResourceGroupName —* parametry (jak pokazano) lub przez określenie obiektu strefy:

    $zone = Get-AzureRmDnsZone -Name contoso.com -ResouceGroupName MyAzureResourceGroup
    $list = Get-AzureRmDnsRecordSet -Zone $zone

## <a name="add-a-record-to-a-record-set"></a>Dodawanie rekordu do zestawu rekordów

Dodawanie rekordów do zestawów rekordów przy użyciu `Add-AzureRmDnsRecordConfig` polecenia cmdlet. To jest operacji w trybie offline. Tylko lokalny obiekt reprezentujący zestawu rekordów został zmieniony.

Parametry dodawania rekordów do zestawu rekordów się różnić w zależności od typu zestawu rekordów. Na przykład, gdy używasz zestaw rekordów typu "A", można określić tylko rekordów przy użyciu parametru *-Adres_ipv4*.

Dodatkowe rekordy można dodawać do poszczególnych rekordów określonych przez dodatkowe połączenia z `Add-AzureRmDnsRecordConfig`. Możesz dodać maksymalnie 20 rekordów do dowolnego zestawu rekordów. Jednak zestawy rekordów typu "CNAME" może zawierać najwyżej jeden rekord i zestaw rekordów nie może zawierać dwa rekordy identyczne. Puste zestawy rekordów (z zero rekordów) można utworzyć, ale nie są wyświetlane na serwery nazw Azure DNS.

Po zestawu rekordów zawiera odpowiednie zbiór rekordów, należy przekazać go za pomocą `Set-AzureRmDnsRecordSet` polecenia cmdlet. Po zestaw rekordów zatwierdzony, zastępuje istniejący rekord, ustaw w systemie DNS Azure.

### <a name="to-create-an-a-record-set-with-a-single-record"></a>Aby utworzyć rekord A zestaw z pojedynczego rekordu

    $rs = New-AzureRmDnsRecordSet -Name "test-a" -RecordType A -Ttl 60 -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Add-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address "1.2.3.4"
    Set-AzureRmDnsRecordSet -RecordSet $rs

Kolejność operacji do utworzenia rekordu może również być *przekazywane w potoku*, co oznacza, że należy przekazać obiekt zestawu rekordów przy użyciu potoku zamiast przekazując je jako parametr. Na przykład:

    New-AzureRmDnsRecordSet -Name "test-a" -RecordType A -Ttl 60 -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup | Add-AzureRmDnsRecordConfig -Ipv4Address "1.2.3.4" | Set-AzureRmDnsRecordSet

### <a name="additional-record-type-examples"></a>Przykłady dodatkowe typ rekordu

[AZURE.INCLUDE [dns-add-record-ps-include](../../includes/dns-add-record-ps-include.md)]

## <a name="modify-existing-record-sets"></a>Modyfikowanie istniejących zestawów rekordów

Instrukcje dotyczące modyfikowania istniejącego zestawu rekordów są podobne do czynności, które należy wykonać podczas tworzenia rekordów. Kolejność operacji jest następująca:

1.  Pobieranie istniejący rekord ustawić przy użyciu `Get-AzureRmDnsRecordSet`.

2.  Modyfikowanie rekordu określonych przez dodawanie rekordów, usuwanie rekordów, zmiana parametrów rekord, lub zmiana rekordu Ustaw time to live (TTL). To jest operacji w trybie offline. Tylko lokalny obiekt reprezentujący zestawu rekordów został zmieniony.

3.  Zatwierdź zmiany za pomocą `Set-AzureRmDnsRecordSet` polecenia cmdlet. Zastępuje istniejący rekord w Azure DNS.


### <a name="to-update-a-record-in-an-existing-record-set"></a>Aby zaktualizować rekord w istniejącym zestawie rekordów

W tym przykładzie zmienimy adres IP istniejący rekord "A":

    $rs = Get-AzureRmDnsRecordSet -name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    $rs.Records[0].Ipv4Address = "134.170.185.46"
    Set-AzureRmDnsRecordSet -RecordSet $rs

`Set-AzureRmDnsRecordSet` Polecenia cmdlet używa etag testy, aby upewnić się, że równoległe zmiany nie są zastępowane. Używanie *-zastąpić* flagę, aby pominąć kontrole. Aby uzyskać więcej informacji zobacz [temat etags i znaczników](dns-getstarted-create-dnszone.md#tagetag).

### <a name="to-modify-an-soa-record"></a>Aby zmodyfikować rekord SOA

Nie można dodawać i usuwać rekordy z ustawioną wierzchołka strefy rekordu SOA tworzone automatycznie (nazwa = "@"). Jednak można modyfikować dowolne parametrów w rekordzie SOA (z wyjątkiem "Host") i TTL zestaw rekordów.

W poniższym przykładzie pokazano, jak zmienić właściwość *E-mail* rekordu SOA:

    $rs = Get-AzureRmDnsRecordSet -Name "@" -RecordType SOA -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    $rs.Records[0].Email = "admin.contoso.com"
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="to-modify-ns-records-at-the-zone-apex"></a>Aby zmodyfikować rekordom u wierzchołka strefy

Nie można dodawać, usuwać lub modyfikować rekordy w tworzone automatycznie NS zestawu rekordów u wierzchołka strefy (nazwa = "@"). Tylko zmiany, które są dozwolone, jest zmodyfikowanie zestawu rekordów TTL.

W poniższym przykładzie pokazano, jak zmienić właściwość TTL zestawu rekordów serwera nazw:

    $rs = Get-AzureRmDnsRecordSet -Name "@" -RecordType NS -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    $rs.Ttl = 300
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="to-add-records-to-an-existing-record-set"></a>Aby dodać rekordy do istniejącego zestawu rekordów

W tym przykładzie dodajemy dwa dodatkowe rekordy MX do istniejącego zestawu rekordów:

    $rs = Get-AzureRmDnsRecordSet -name "test-mx" -RecordType MX -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Add-AzureRmDnsRecordConfig -RecordSet $rs -Exchange "mail2.contoso.com" -Preference 10
    Add-AzureRmDnsRecordConfig -RecordSet $rs -Exchange "mail3.contoso.com" -Preference 20
    Set-AzureRmDnsRecordSet -RecordSet $rs

## <a name="remove-a-record-from-an-existing-record-set"></a>Usuwanie rekordu z istniejącego zestawu rekordów

Rekordy można usunąć z rekordu ustawić przy użyciu `Remove-AzureRmDnsRecordConfig`. Rekord, który jest usuwany musi być dokładne dopasowanie z istniejącego rekordu we wszystkich parametrów. Zmiany muszą być wprowadzone przy użyciu `Set-AzureRmDnsRecordSet`.

Usunięcie ostatni rekord w zestawie rekordów nie powoduje usunięcia zestawu rekordów. Aby uzyskać więcej informacji, zobacz [Usuwanie zestawu rekordów](#delete-a-record-set) .


    $rs = Get-AzureRmDnsRecordSet -Name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address "1.2.3.4"
    Set-AzureRmDnsRecordSet -RecordSet $rs

Kolejność operacji, aby usunąć rekord z zestawu rekordów może również być przekazywane w potoku, co oznacza, że należy przekazać obiekt zestawu rekordów przy użyciu potoku zamiast przekazując je jako parametr. Na przykład:

    Get-AzureRmDnsRecordSet -Name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup | Remove-AzureRmDnsRecordConfig -Ipv4Address "1.2.3.4" | Set-AzureRmDnsRecordSet

### <a name="remove-an-aaaa-record-from-a-record-set"></a>Usuwanie rekordu AAAA z zestawu rekordów

    $rs = Get-AzureRmDnsRecordSet -Name "test-aaaa" -RecordType AAAA -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Ipv6Address "2607:f8b0:4009:1803::1005"
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="remove-a-cname-record-from-a-record-set"></a>Usuwanie rekordu CNAME z zestawu rekordów

Zestaw rekordów CNAME może zawierać najwyżej jeden rekord, usunięcie tego rekordu pozostawia puste zestawu rekordów.

    $rs =  Get-AzureRmDnsRecordSet -name "test-cname" -RecordType CNAME -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Cname "www.contoso.com"
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="remove-an-mx-record-from-a-record-set"></a>Usuwanie rekordu MX z zestawu rekordów

    $rs = Get-AzureRmDnsRecordSet -name "test-mx" -RecordType MX -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Exchange "mail.contoso.com" -Preference 5
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="remove-an-ns-record-from-record-set"></a>Usuwanie rekordu NS z zestawu rekordów

    $rs = Get-AzureRmDnsRecordSet -Name "test-ns" -RecordType NS -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Nsdname "ns1.contoso.com"
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="remove-an-srv-record-from-a-record-set"></a>Usuwanie rekordu SRV z zestawu rekordów

    $rs = Get-AzureRmDnsRecordSet -Name "_sip._tls" -RecordType SRV -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs –Priority 0 –Weight 5 –Port 8080 –Target "sip.contoso.com"
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="remove-a-txt-record-from-a-record-set"></a>Usuwanie rekordu TXT w zestawie rekordów

    $rs = Get-AzureRmDnsRecordSet -Name "test-txt" -RecordType TXT -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Value "This is a TXT record"
    Set-AzureRmDnsRecordSet -RecordSet $rs

## <a name="delete-a-record-set"></a>Usuwanie zestawu rekordów

Zestawy rekordów można usunąć za pomocą `Remove-AzureRmDnsRecordSet` polecenia cmdlet. Nie można usunąć SOA i rekord NS zestawy u wierzchołka strefy (nazwa = "@") tworzonych automatycznie podczas tworzenia strefy. Zostaną usunięte automatycznie po usunięciu strefy.

Aby usunąć zestaw rekordów, użyj jednej z następujących trzech metod:

### <a name="specify-all-the-parameters-by-name"></a>Określ wszystkie parametry według nazwy

Opcjonalne *-życie* przełącznika można pominąć monit o potwierdzenie.

    Remove-AzureRmDnsRecordSet -Name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup [-Force]


### <a name="specify-the-record-set-by-name-and-type-and-specify-the-zone-by-object"></a>Określanie zestawu rekordów, nazwa i typ, a następnie określ strefę czasową przez obiekt

    $zone = Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordSet -Name "test-a" -RecordType A -Zone $zone [-Force]

### <a name="specify-the-record-set-by-object"></a>Określanie zestawu przez obiekt rekordów

    $rs = Get-AzureRmDnsRecordSet -Name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordSet –RecordSet $rs [-Overwrite] [-Force]

Po określeniu rekordu ustawić za pomocą obiektu umożliwia etag testy upewnić się, że równoległe zmiany nie zostaną usunięte. Opcjonalne *-zastąpić* flagi pomija kontrole. Aby uzyskać więcej informacji, zobacz [Etags i znaczników](dns-getstarted-create-dnszone.md#tagetag) .

Obiekt zestawu rekordów może również być przekazywane w potoku zamiast jako parametr przekazano jest:

    Get-AzureRmDnsRecordSet -Name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup | Remove-AzureRmDnsRecordSet [-Overwrite] [-Force]

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji o usłudze DNS Azure zobacz [Omówienie Azure DNS](dns-overview.md). Informacje dotyczące automatycznych DNS Zobacz [Tworzenie stref i rekord zestawów przy użyciu zestawu SDK .NET](dns-sdk.md).

Aby uzyskać więcej informacji na temat odwrotnej rekordy DNS Zobacz [jak zarządzać odwrotnej rekordy DNS dla usług przy użyciu programu PowerShell](dns-reverse-dns-record-operations-ps.md).
