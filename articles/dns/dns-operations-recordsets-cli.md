<properties
   pageTitle="Zarządzanie zestawy rekordów DNS i rekordy DNS Azure za pomocą interfejsu wiersza polecenia Azure | Microsoft Azure"
   description="Zarządzanie zestawy rekordów DNS i rekordy Azure DNS hostingu własną domeną w usłudze Azure DNS. Polecenie Wszystkie polecenia umożliwiające obsługę operacji na zestawach rekordów i rekordów."
   services="dns"
   documentationCenter="na"
   authors="jtuliani"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/22/2016"
   ms.author="jtuliani"/>

# <a name="manage-dns-records-and-record-sets-by-using-cli"></a>Zarządzanie rekordami DNS i zestawy rekordów przy użyciu interfejsu wiersza polecenia


> [AZURE.SELECTOR]
- [Azure Portal](dns-operations-recordsets-portal.md)
- [Polecenie Azure](dns-operations-recordsets-cli.md)
- [Programu PowerShell](dns-operations-recordsets.md)


W tym artykule pokazano, jak zarządzać zestawy rekordów i rekordy strefy DNS przy użyciu interfejsu wiersza polecenia Azure i platform (polecenie).

Należy opis różnic między zestawy rekordów DNS i poszczególnych rekordów DNS. Zestaw rekordów to zbiór rekordów w strefie, które mają taką samą nazwę i tego samego typu. Aby uzyskać więcej informacji zobacz [Opis zestawy rekordów i rekordów](dns-getstarted-create-recordset-cli.md).


## <a name="configure-the-cross-platform-azure-cli"></a>Konfigurowanie polecenie Azure i platform

Azure DNS jest usługą Azure tylko do Menedżera zasobów. Nie ma interfejs API Azure usługi zarządzania. Upewnij się, że polecenie Azure jest skonfigurowany do używania trybu Menedżer zasobów za pomocą `azure config mode arm` polecenia.

Jeśli zobaczysz **o błędzie: "dns" nie jest to polecenie azure**, najprawdopodobniej ponieważ używasz polecenie Azure w trybie Zarządzanie usługą Azure, a nie w trybie Menedżera zasobów.

## <a name="create-a-new-record-set-and-record"></a>Tworzenie nowego zestawu rekordów i rekordu

Aby utworzyć nowy rekord w portalu Azure, zobacz [Tworzenie zestawu rekordów i rekordów](dns-getstarted-create-recordset-cli.md).


## <a name="retrieve-a-record-set"></a>Pobieranie zestawu rekordów

Aby pobrać istniejący zestaw rekordów, należy użyć `azure network dns record-set show`. Określanie grupy zasobów, nazwa strefy rekordu Ustaw względna nazwa i typ rekordu. Za pomocą przykładzie poniżej, zamieniając wartości na własny.

    azure network dns record-set show myresourcegroup contoso.com www A


## <a name="list-record-sets"></a>Zestawy rekordów listy

Można wyświetlić listę wszystkich rekordów DNS w strefie za pomocą `azure network dns record-set list` polecenia. Należy określić nazwę grupy zasobów i nazwa strefy.

### <a name="to-list-all-record-sets"></a>Aby wyświetlić listę wszystkich zestawach rekordów

W tym przykładzie zwraca wszystkie zestawy rekordów, niezależnie od tego, czy nazwy lub typu rekordu:

    azure network dns record-set list myresourcegroup contoso.com

### <a name="to-list-record-sets-of-a-given-type"></a>Do listy zestawów rekordów danego typu

W tym przykładzie zwraca wszystkie zestawy rekordów, które są zgodne z danym typ rekordu (w tym przypadku rekordy "A"):

    azure network dns record-set list myresourcegroup contoso.com A


## <a name="add-a-record-to-a-record-set"></a>Dodawanie rekordu do zestawu rekordów

Dodawanie rekordów do zestawów rekordów przy użyciu `azure network dns record-set add-record`polecenia. Parametry dodawania rekordów do zestawu rekordów się różnić w zależności od typu rekordu, który jest ustawiany. Na przykład, gdy używasz zestaw rekordów typu "A", można określić tylko rekordów przy użyciu parametru `-a <IPv4 address>`.

Aby utworzyć zestaw rekordów, należy użyć `azure network dns record-set create`polecenia. Określanie grupy zasobów, nazwa strefy rekordu ustawić względną nazwę, typ rekordu i czasu wygaśnięcia (TTL). Jeśli `--ttl` nie zdefiniowano parametru, przyjmowana jest wartość (w sekundach) domyślna cztery.

    azure network dns record-set create myresourcegroup  contoso.com "test-a"  A --ttl 300


Po utworzeniu "" zestawu rekordów, Dodaj adres IP protokołu IPv4 przy użyciu `azure network dns record-set add-record`polecenia.

    azure network dns record-set add-record myresourcegroup contoso.com "test-a" A -a 192.168.1.1


W poniższych przykładach pokazano, jak utworzyć zestaw rekordów każdego typu rekordu. Każdy zestaw rekordów zawiera jeden rekord.

[AZURE.INCLUDE [dns-add-record-cli-include](../../includes/dns-add-record-cli-include.md)]


## <a name="update-a-record-in-a-record-set"></a>Zaktualizuj rekord w zestawie rekordów

### <a name="to-add-another-ip-address-1234-to-an-existing-a-record-set-www"></a>Aby dodać innego adresu IP (1.2.3.4) do istniejącego rekord "A" Ustaw ("www"):

    azure network dns record-set add-record  myresourcegroup contoso.com  A
    -a 1.2.3.4
    info:    Executing command network dns record-set add-record
    Record set name: www
    + Looking up the dns zone "contoso.com"
    + Looking up the DNS record set "www"
    + Updating DNS record set "www"
    data:    Id                              : /subscriptions/################################/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/a/www
    data:    Name                            : www
    data:    Type                            : Microsoft.Network/dnszones/a
    data:    Location                        : global
    data:    TTL                             : 4
    data:    A records:
    data:        IPv4 address                : 192.168.1.1
    data:        IPv4 address                : 1.2.3.4
    data:
    info:    network dns record-set add-record command OK

### <a name="to-remove-an-existing-value-from-a-record-set"></a>Aby usunąć istniejącą wartość z zestawu rekordów
Używanie `azure network dns record-set delete-record`.

    azure network dns record-set delete-record myresourcegroup contoso.com www A -a 1.2.3.4
    info:    Executing command network dns record-set delete-record
    + Looking up the DNS record set "www"
    Delete DNS record? [y/n] y
    + Updating DNS record set "www"
    data:    Id                              : /subscriptions/################################/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/A/www
    data:    Name                            : www
    data:    Type                            : Microsoft.Network/dnszones/A
    data:    Location                        : global
    data:    TTL                             : 4
    data:    A records:
    data:    IPv4 address                    : 192.168.1.1
    data:
    info:    network dns record-set delete-record command OK



## <a name="remove-a-record-from-a-record-set"></a>Usuwanie rekordu z zestawu rekordów

Rekordy można usunąć z rekordu ustawić przy użyciu `azure network dns record-set delete-record`. Rekord, który jest usuwany musi być dokładne dopasowanie z istniejącego rekordu we wszystkich parametrów.

Usunięcie ostatni rekord w zestawie rekordów nie powoduje usunięcia zestawu rekordów. Aby uzyskać więcej informacji zobacz sekcję tego artykułu o nazwie [Usuwanie zestawu rekordów](#delete).

    azure network dns record-set delete-record myresourcegroup contoso.com www A -a 192.168.1.1

    azure network dns record-set delete myresourcegroup contoso.com www A

### <a name="remove-an-aaaa-record-from-a-record-set"></a>Usuwanie rekordu AAAA z zestawu rekordów

    azure network dns record-set delete-record myresourcegroup contoso.com test-aaaa  AAAA -b "2607:f8b0:4009:1803::1005"

### <a name="remove-a-cname-record-from-a-record-set"></a>Usuwanie rekordu CNAME z zestawu rekordów

    azure network dns record-set delete-record myresourcegroup contoso.com test-cname CNAME -c www.contoso.com


### <a name="remove-an-mx-record-from-a-record-set"></a>Usuwanie rekordu MX z zestawu rekordów

    azure network dns record-set delete-record myresourcegroup contoso.com "@" MX -e "mail.contoso.com" -f 5

### <a name="remove-an-ns-record-from-record-set"></a>Usuwanie rekordu NS z zestawu rekordów

    azure network dns record-set delete-record myresourcegroup contoso.com  "test-ns" NS -d "ns1.contoso.com"

### <a name="remove-a-ptr-record-from-a-record-set"></a>Usuń rekord PTR w zestawie rekordów
W tym przypadku "Moje-arpa-zone.com" oznacza strefę ARPA reprezentujące zakres adresów IP.  Każdy rekord PTR w tej strefie odpowiada adres IP z tego zakresu adresów IP.

    azure network dns record-set delete-record myresourcegroup my-arpa-zone.com "10" PTR -P "myservice.contoso.com"

### <a name="remove-an-srv-record-from-a-record-set"></a>Usuwanie rekordu SRV z zestawu rekordów

    azure network dns record-set delete-record myresourcegroup contoso.com  "_sip._tls" SRV -p 0 -w 5 -o 8080 -u "sip.contoso.com"

### <a name="remove-a-txt-record-from-a-record-set"></a>Usuwanie rekordu TXT w zestawie rekordów

    azure network dns record-set delete-record myresourcegroup contoso.com  "test-TXT" TXT -x "this is a TXT record"

## <a name="delete"></a>Usuwanie zestawu rekordów

Zestawy rekordów można usunąć za pomocą `Remove-AzureRmDnsRecordSet` polecenia cmdlet. Nie można usunąć SOA i rekord NS zestawy u wierzchołki strefy (nazwa = "@") tworzonych automatycznie podczas tworzenia strefy. Zostaną usunięte automatycznie po usunięciu strefy.

W poniższym przykładzie zostanie usunięty rekord "A" Ustawianie "test-a" z "contoso.com" strefy DNS:

    azure network dns record-set delete myresourcegroup contoso.com  "test-a" A

Przełączanie opcjonalne *-q* można pominąć monit o potwierdzenie.


## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji o usłudze DNS Azure zobacz [Omówienie Azure DNS](dns-overview.md). Informacje dotyczące automatycznych DNS Zobacz [Tworzenie stref i rekord zestawów przy użyciu zestawu SDK .NET](dns-sdk.md).

Jeśli chcesz pracować z odwrotnej rekordy DNS, zobacz [jak zarządzać odwrotnej rekordy DNS dla usług za pomocą interfejsu wiersza polecenia Azure](dns-reverse-dns-record-operations-cli.md).
