<properties
   pageTitle="Zarządzanie strefy DNS przy użyciu programu PowerShell | Microsoft Azure"
   description="Możesz zarządzać strefy DNS przy użyciu programu Powershell Azure. Jak zaktualizować, usuwanie i tworzenie strefy DNS na Azure DNS"
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

# <a name="how-to-manage-dns-zones-using-powershell"></a>Jak zarządzać strefy DNS przy użyciu programu PowerShell

> [AZURE.SELECTOR]
- [Polecenie Azure](dns-operations-dnszones-cli.md)
- [Programu PowerShell](dns-operations-dnszones.md)



W tym artykule procedurach pokazano, jak zarządzać swoją strefę DNS przy użyciu programu PowerShell. Aby wykonać te kroki, musisz zainstalować najnowszą wersję programu PowerShell Menedżera zasobów Azure poleceń cmdlet (1.0 lub nowsza). Aby uzyskać więcej informacji o instalowaniu poleceń cmdlet programu PowerShell, zobacz [jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md) .


## <a name="create-a-new-dns-zone"></a>Tworzenie nowej strefy DNS

Aby utworzyć strefy DNS, zobacz [Tworzenie strefy DNS przy użyciu programu PowerShell](dns-getstarted-create-dnszone.md).

## <a name="get-a-dns-zone"></a>Uzyskiwanie strefy DNS

Aby pobrać strefy DNS, za pomocą `Get-AzureRmDnsZone` polecenia cmdlet. Operacja zwraca obiekt strefy DNS odpowiadającą istniejącej strefy w systemie DNS Azure. Obiekt zawiera dane o strefie (na przykład liczba zestawy rekordów), ale nie zawiera zestawy rekordów, się.

    $zone = Get-AzureRmDnsZone -Name contoso.com –ResourceGroupName MyAzureResourceGroup

## <a name="list-dns-zones"></a>Strefy DNS listy

Usuwając nazwę strefy z `Get-AzureRmDnsZone`, można wyliczyć wszystkich stref w grupie zasobów. Operacja zwraca tablicę obiektów strefy.

    $zoneList = Get-AzureRmDnsZone -ResourceGroupName MyAzureResourceGroup

## <a name="update-a-dns-zone"></a>Aktualizacja strefy DNS

Zmiany do zasobu strefy DNS mogą zostać wprowadzone przy użyciu `Set-AzureRmDnsZone`. To nie aktualizacji zestawy rekordów DNS w strefie (zobacz, [jak rekordy Zarządzaj systemem DNS](dns-operations-recordsets.md)). Tylko służy do aktualizowania właściwości samego zasobu strefy. To jest obecnie ograniczone do Menedżera zasobów Azure "znaczników" dla zasobu strefy. Aby uzyskać więcej informacji, zobacz [Etags i znaczników](dns-getstarted-create-dnszone.md#Etags-and-tags) .

Użyj jednej z następujących czynności dwa sposoby aktualizacji strefy DNS:

### <a name="specify-the-zone-using-the-zone-name-and-resource-group"></a>Określanie strefy za pomocą grupie strefy nazwy i zasobów

    Set-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup [-Tag $tags]

### <a name="specify-the-zone-using-a-zone-object"></a>Określanie strefy przy użyciu obiektu $zone

Określanie strefy za pomocą obiektu $zone z `Get-AzureRmDnsZone`. Podczas korzystania z `Set-AzureRmDnsZone` z obiektem $zone testy Etag są używane do zapewnienia równoległe zmiany nie są zastępowane. Możesz użyć opcjonalne *-zastąpić* Przełącz pomijany kontrole. Aby uzyskać więcej informacji, zobacz [Etags i znaczników](dns-getstarted-create-dnszone.md#Etags-and-tags) .


    $zone = Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
    <..modify $zone.Tags here...>
    Set-AzureRmDnsZone -Zone $zone [-Overwrite]


## <a name="delete-a-dns-zone"></a>Usuwanie strefy DNS

Strefy DNS można usunąć za pomocą polecenia cmdlet AzureRmDnsZone Usuń.

Przed usunięciem strefy DNS w systemie DNS Azure, należy usunąć wszystkie zestawy rekordów, z wyjątkiem NS i SOA rekordy w katalogu głównym strefy, które zostały utworzone automatycznie podczas tworzenia strefy.

Użyj jednej z następujących dwie metody usuwania strefy DNS:

### <a name="specify-the-zone-using-the-zone-name-and-resource-group-name"></a>Określenie strefy przy użyciu nazwy strefy i nazwa grupy zasobów

Operacja ma opcjonalny *-życie* przełącznik, który powoduje pominięcie monitu, aby potwierdzić zamiar usunięcia strefy DNS.

    Remove-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup [-Force]

### <a name="specify-the-zone-using-a-zone-object"></a>Określanie strefy przy użyciu obiektu $zone

Określanie strefy za pomocą obiektu $zone z `Get-AzureRmDnsZone`. Operacja ma opcjonalny *-życie* przełącznik, który powoduje pominięcie monitu, aby potwierdzić zamiar usunięcia strefy DNS. W przypadku `Set-AzureRmDnsZone`, określając strefy za pomocą obiektu $zone umożliwia kontroli Etag w celu zapewnienia równoległe zmiany nie zostaną usunięte. <BR>
Opcjonalne *-zastąpić* flagi pomija kontrole. Aby uzyskać więcej informacji, zobacz [Etags i znaczników](dns-getstarted-create-dnszone.md#Etags-and-tags) .

    $zone = Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsZone -Zone $zone [-Force] [-Overwrite]



Obiekt strefy może również być przekazywane w potoku zamiast jako parametr przekazano jest:

    Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup | Remove-AzureRmDnsZone [-Force] [-Overwrite]

## <a name="next-steps"></a>Następne kroki

Po utworzeniu strefy DNS, utworzyć [zestawy rekordów i rekordy](dns-getstarted-create-recordset.md) uruchomić rozpoznawanie nazw dla domeny internetowej.