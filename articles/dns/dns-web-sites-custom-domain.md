<properties
   pageTitle="Tworzenie niestandardowych rekordów DNS dla aplikacji sieci web | Microsoft Azure  "
   description="Jak utworzyć rekordy DNS dla aplikacji sieci web za pomocą Azure DNS domeny niestandardowej."
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

# <a name="create-dns-records-for-a-web-app-in-a-custom-domain"></a>Tworzenie rekordów DNS dla aplikacji sieci web w domenie niestandardowej

Azure DNS służy do przechowywania niestandardowej domeny dla aplikacji sieci web. Na przykład tworzysz aplikację sieci web Azure i chcesz, aby użytkownicy do nich dostęp przez jeden przy użyciu postaci nazwy FQDN contoso.com lub www.contoso.com.

Aby to zrobić, należy utworzyć dwa rekordy:

- Rekord główny "A" wskazująca contoso.com
- Rekord "CNAME" Nazwa "www", wskazującego na rekord

Należy pamiętać, że po utworzeniu rekordu A dla aplikacji sieci web w Azure rekord należy ręcznie aktualizowane, jeśli adres IP źródłowych zmiany aplikacji sieci web.

## <a name="before-you-begin"></a>Przed rozpoczęciem

Przed rozpoczęciem należy najpierw utworzyć strefy DNS w systemie DNS Azure i przekazaniu strefy rejestratora domen DNS Azure.

1. Aby utworzyć strefy DNS, wykonaj czynności podane w sekcji [Tworzenie strefy DNS](dns-getstarted-create-dnszone.md).
2. Aby delegować DNS Azure DNS, wykonaj czynności w [Delegowanie domeny DNS](dns-domain-delegation.md).

Po utworzeniu strefy i delegowanie go do Azure DNS, następnie można utworzyć rekordy dla domeny niestandardowej.


## <a name="1-create-an-a-record-for-your-custom-domain"></a>1. Utwórz rekord A dla domeny niestandardowej

Rekord jest używany do mapowania nazwy na adres IP. W poniższym przykładzie możemy przypisze @ jako rekord adresu IP protokołu IPv4:

### <a name="step-1"></a>Krok 1

Utwórz rekord A i przypisać do zmiennej $rs

    $rs= New-AzureRMDnsRecordSet -Name "@" -RecordType "A" -ZoneName "contoso.com" -ResourceGroupName "MyAzureResourceGroup" -Ttl 600

### <a name="step-2"></a>Krok 2

Dodaj wartość IPv4 do poprzednio utworzone zestawu rekordów "@" przy użyciu zmiennej $rs przydzielone. Przypisana wartość IPv4 będzie adres IP dla aplikacji sieci web.

Aby znaleźć adres IP dla aplikacji sieci web, wykonaj czynności podane w [artykule Konfigurowanie niestandardowej nazwy domeny w usłudze Azure w aplikacji](../web-sites-custom-domain-name.md#Find-the-virtual-IP-address).

    Add-AzureRMDnsRecordConfig -RecordSet $rs -Ipv4Address <your web app IP address>

### <a name="step-3"></a>Krok 3

Zatwierdź zmiany w zestawie rekordów. Używanie `Set-AzureRMDnsRecordSet` Aby przekazać zmiany do zestawu rekordów do Azure DNS:

    Set-AzureRMDnsRecordSet -RecordSet $rs

## <a name="2-create-a-cname-record-for-your-custom-domain"></a>2. Utwórz rekord CNAME dla domeny niestandardowej

Jeśli domena jest już zarządzany przez system DNS Azure (zobacz [Delegowanie domeny DNS](dns-domain-delegation.md), można użyć następujących przykład, aby utworzyć rekord CNAME na potrzeby contoso.azurewebsites.net.

### <a name="step-1"></a>Krok 1

Otwieranie programu PowerShell i Utwórz nowy zestaw rekordów CNAME i przypisać do zmiennej $rs. W tym przykładzie spowoduje utworzenie typu zestawu rekordów CNAME "czasu na żywo" 600 sekund w strefy DNS o nazwie "contoso.com".

    $rs = New-AzureRMDnsRecordSet -ZoneName contoso.com -ResourceGroupName myresourcegroup -Name "www" -RecordType "CNAME" -Ttl 600

    Name              : www
    ZoneName          : contoso.com
    ResourceGroupName : myresourcegroup
    Ttl               : 600
    Etag              : 8baceeb9-4c2c-4608-a22c-229923ee1856
    RecordType        : CNAME
    Records           : {}
    Tags              : {}


### <a name="step-2"></a>Krok 2

Po utworzeniu zestawu rekordów CNAME, należy utworzyć alias wartość, która będzie wskazywać aplikacji sieci web.

Przy użyciu wcześniej przydzielonych zmiennej "$rs" umożliwia polecenia programu PowerShell poniżej Tworzenie aliasu dla contoso.azurewebsites.net aplikacji sieci web.

    Add-AzureRMDnsRecordConfig -RecordSet $rs -Cname "contoso.azurewebsites.net"

    Name              : www
    ZoneName          : contoso.com
    ResourceGroupName : myresourcegroup
    Ttl               : 600
    Etag              : 8baceeb9-4c2c-4608-a22c-229923ee185
    RecordType        : CNAME
    Records           : {contoso.azurewebsites.net}
    Tags              : {}

### <a name="step-3"></a>Krok 3

Zatwierdź zmiany za pomocą `Set-AzureRMDnsRecordSet` polecenia cmdlet:

    Set-AzureRMDnsRecordSet -RecordSet $rs

Można sprawdzić poprawność rekord został utworzony poprawnie przez badanie "www.contoso.com" przy użyciu narzędzia nslookup, tak jak pokazano poniżej:

    PS C:\> nslookup
    Default Server:  Default
    Address:  192.168.0.1

    > www.contoso.com
    Server:  default server
    Address:  192.168.0.1

    Non-authoritative answer:
    Name:    <instance of web app service>.cloudapp.net
    Address:  <ip of web app service>
    Aliases:  www.contoso.com
    contoso.azurewebsites.net
    <instance of web app service>.vip.azurewebsites.windows.net

## <a name="create-an-awverify-record-for-web-apps"></a>Utwórz rekord "awverify" dla aplikacji sieci web


Jeśli zdecydujesz się na używanie rekordu A dla aplikacji sieci web, musi przejść przez proces weryfikacji, aby upewnić się, że jesteś właścicielem domeny niestandardowej. Proces weryfikacji należy utworzyć specjalny rekord CNAME o nazwie "awverify". W tej sekcji dotyczą tylko rekordy.


### <a name="step-1"></a>Krok 1

Utwórz rekord "awverify". W poniższym przykładzie zostanie utworzony rekord "aweverify" contoso.com w celu zweryfikowania własności do domeny niestandardowej.

    $rs = New-AzureRMDnsRecordSet -ZoneName contoso.com -ResourceGroupName myresourcegroup -Name "awverify" -RecordType "CNAME" -Ttl 600

    Name              : awverify
    ZoneName          : contoso.com
    ResourceGroupName : myresourcegroup
    Ttl               : 600
    Etag              : 8baceeb9-4c2c-4608-a22c-229923ee1856
    RecordType        : CNAME
    Records           : {}
    Tags              : {}


### <a name="step-2"></a>Krok 2

Po utworzeniu zestawu rekordów "awverify" przypisywanie rekordu CNAME, ustaw alias. W poniższym przykładzie możemy przypisze rekordu CNAMe alias równa awverify.contoso.azurewebsites.net.

    Add-AzureRMDnsRecordConfig -RecordSet $rs -Cname "awverify.contoso.azurewebsites.net"

    Name              : awverify
    ZoneName          : contoso.com
    ResourceGroupName : myresourcegroup
    Ttl               : 600
    Etag              : 8baceeb9-4c2c-4608-a22c-229923ee185
    RecordType        : CNAME
    Records           : {awverify.contoso.azurewebsites.net}
    Tags              : {}

### <a name="step-3"></a>Krok 3

Zatwierdź zmiany za pomocą `Set-AzureRMDnsRecordSet cmdlet`w sposób pokazany poniżej polecenia.

    Set-AzureRMDnsRecordSet -RecordSet $rs



## <a name="next-steps"></a>Następne kroki

Postępuj zgodnie z instrukcjami [Konfigurowanie niestandardowej nazwy domeny dla usługi aplikacji](../app-service-web/web-sites-custom-domain-name.md) do konfigurowania aplikacji sieci web, aby używać domeny niestandardowej.








