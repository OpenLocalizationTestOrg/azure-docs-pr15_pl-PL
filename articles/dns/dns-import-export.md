<properties
   pageTitle="Importowanie i eksportowanie pliku strefy domeny DNS Azure za pomocą interfejsu wiersza polecenia | Microsoft Azure"
   description="Dowiedz się, jak importowanie i eksportowanie pliku strefy DNS Azure DNS za pomocą interfejsu wiersza polecenia Azure"
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

# <a name="import-and-export-a-dns-zone-file-using-the-azure-cli"></a>Importowanie i eksportowanie plik strefy DNS za pomocą interfejsu wiersza polecenia Azure


W tym artykule przeprowadzi Cię przez proces importowania i eksportowania plików strefy DNS Azure w systemie DNS za pomocą interfejsu wiersza polecenia Azure.

## <a name="introduction-to-dns-zone-migration"></a>Wprowadzenie do migracji strefy DNS

Plik strefy DNS jest plik tekstowy, który zawiera szczegóły każdego rekordu systemu nazw domen (DNS) w tej strefie. Wynika standardowy format, dzięki czemu odpowiednie do przenoszenia rekordy DNS między systemów DNS. Za pomocą pliku strefy jest szybkie, niezawodne i wygodny sposób przenieść strefy DNS do lub poza Azure DNS.

Azure DNS obsługuje importowanie i eksportowanie plików strefy przy użyciu interfejsu wiersza polecenia Azure (polecenie). Polecenie Azure to narzędzie wiersza polecenia i platform używana do zarządzania usług Azure. Jest dostępny dla systemu Windows, Mac i Linux oraz platform ze [strony pobierania Azure](https://azure.microsoft.com/downloads/). Obsługa wielu platform jest szczególnie istotne w przypadku importowania i eksportowania plików stref, ponieważ najczęściej używane oprogramowanie serwera nazw, [POWIĄZAĆ](https://www.isc.org/downloads/bind/), zwykle wykonuje w Linux.

## <a name="obtain-your-existing-dns-zone-file"></a>Uzyskiwanie istniejący plik strefy DNS

Przed zaimportowaniem pliku strefy DNS do Azure DNS, musisz uzyskać kopię pliku strefy. Źródło tego pliku zależy miejsce, w którym jest obecnie hostowana strefy DNS.

- Jeśli do strefy DNS jest obsługiwana przez usługę partnera (na przykład rejestratora domen, dedykowane dostawcy hostingu DNS lub dostawcy alternatywny chmurze), tej usługi powinien zawierać możliwość pobierania pliku strefy DNS.

- Jeśli do strefy DNS jest obsługiwana w systemie Windows DNS, domyślny folder dla plików strefy jest **%systemroot%\system32\dns**. Na karcie **Ogólne** konsoli zarządzania usługi DNS zawiera także pełną ścieżkę do każdego pliku strefy.

- Jeśli swoją strefę DNS znajduje się za pomocą powiązania, lokalizację pliku strefy dla każdej strefy określono w pliku konfiguracji powiązania **named.conf**.

**Praca z plikami strefy w witrynie GoDaddy**<BR>
Strefa pliki pobrane z GoDaddy mają nieco niestandardowy format. Należy rozwiązać ten problem, przed zaimportowaniem te pliki strefy do Azure DNS. Nazw DNS w Dane_rekordu każdego rekordu DNS zdefiniowanego są w pełni kwalifikowanych nazw, ale nie masz zakończenia "." Oznacza to, że są interpretowane przez inne systemy DNS jako względne nazwy. Musisz edytować plik strefy, aby dołączyć zakończenia "." do ich nazwy przed zaimportowaniem ich do Azure DNS.

## <a name="import-a-dns-zone-file-into-azure-dns"></a>Importowanie pliku strefy DNS do Azure DNS


Importowanie pliku strefy spowoduje utworzenie nowej strefy w systemie DNS Azure Jeśli jeszcze nie istnieje. Jeśli strefy już istnieje, zestawy rekordów w pliku strefy musi zostać połączony z istniejących zestawów rekordów.

### <a name="merge-behavior"></a>Scalanie zachowanie

- Domyślnie są scalane istniejących, jak i nowych zestawach rekordów. Identyczne rekordy w zestawie rekordów scalonych są Anuluj zduplikowane.

- Alternatywnie, określając `--force` opcji importowania zastąpią istniejące zestawy rekordów przy użyciu nowych zestawów rekordów. Istniejące zestawy rekordów, które nie mają odpowiedni rekord, ustaw w pliku strefy zaimportowanych nie zostaną usunięte.

- W przypadku scalania zestawy rekordów time to live (TTL) wcześniej istniejących zestawów rekord jest używany. Gdy `--force` jest używane, jest używana wartość TTL nowego zestawu rekordów.

- Rozpocznij parametrów uwierzytelniania (SOA) (z wyjątkiem `host`) są zawsze pobierane z pliku zaimportowanych strefy, niezależnie od tego, czy `--force` jest używana. Podobnie dla ustawioną wierzchołka strefy rekord serwera nazw wartość TTL zawsze pochodzi z pliku zaimportowanego strefy.

- Importowany rekord CNAME nie spowoduje zastąpienie istniejącego rekordu CNAME o takiej samej nazwie chyba że `--force` określono parametru.

- Gdy wystąpi konflikt między rekord CNAME i innym rekordem takiej samej nazwie, ale innego typu (niezależnie od tego, który jest istniejące lub nowe), jest zachowywana istniejący rekord. To zależy od stosowania `--force`.

### <a name="additional-information-about-importing"></a>Dodatkowe informacje na temat importowania danych

Poniższe uwagi zapewnia dodatkowe informacje techniczne dotyczące strefy procesu importowania.

- `$TTL` Dyrektywy jest opcjonalne, a jest obsługiwana. Gdy nie `$TTL` znajduje się dyrektywy, rekordy bez jawne TTL będzie zaimportowanych Ustaw domyślne TTL 3600 sekund. Po dwa rekordy w ten sam zestaw rekordów określić różnych wartości czasu TTL, niższą wartość jest używany.

- `$ORIGIN` Dyrektywy jest opcjonalne, a jest obsługiwana. Gdy nie `$ORIGIN` jest ustawiona, zostanie użyta wartość domyślna nazwa strefy określonego w wierszu polecenia (plus zakończenia ".").

- `$INCLUDE` i `$GENERATE` dyrektywy nie są obsługiwane.

- Obsługiwane są następujące typy rekordów: A, AAAA, CNAME, MX, NS, SOA, SRV i TXT.

- Rekord SOA zostanie utworzona automatycznie przez serwer DNS Azure po utworzeniu strefy. Podczas importowania pliku strefy wszystkie parametry SOA są pobierane z strefy pliku *z wyjątkiem* `host` parametru. Ten parametr używa wartości dostarczony przez Azure DNS. Jest tak, ponieważ ten parametr muszą odwoływać się do podstawową nazwę serwera podaną przez Azure DNS.

- Rekord serwera nazw ustawioną wierzchołka strefy jest również tworzone automatycznie przez Azure DNS po utworzeniu strefy. Jest importowana tylko TTL tego zestawu rekordów. Te rekordy zawierają nazwy serwerów nazw dostarczony przez Azure DNS. Dane rekordu nie jest zastępowana przez wartości zawarte w pliku strefy zaimportowane.

- Podczas publicznej podglądu Azure DNS obsługuje tylko jednego ciągu rekordy TXT. Rekordy TXT wielociągu będą łączone i obcinane do 255 znaków.

### <a name="cli-format-and-values"></a>Polecenie format i wartości


Format polecenie polecenie Azure, aby zaimportować strefy DNS jest:<BR>`azure network dns zone import [options] <resource group> <zone name> <zone file name>`

Wartości:

- `<resource group>`jest nazwą grupy zasobów dla strefy Azure DNS.
- `<zone name>`jest nazwą strefy.
- `<zone file name>`to ścieżka i nazwa pliku strefy do zaimportowania.

Jeśli strefa o tej nazwie nie istnieje w grupie zasobów, zostanie on utworzony dla Ciebie. Jeśli strefy już istnieje, zaimportowany zestawy rekordów zostaną scalone z istniejących zestawów rekordów. Aby zastąpić istniejące zestawy rekordów, należy użyć `--force` opcji.

Aby sprawdzić format pliku strefy bez rzeczywistego importowania danych, użyj `--parse-only` opcji.

### <a name="step-1-import-a-zone-file"></a>Krok 1. Importowanie pliku strefy

Aby zaimportować plik strefy dla strefy **contoso.com**.

1. Zaloguj się do subskrypcji usługi Azure za pomocą interfejsu wiersza polecenia Azure.

        azure login

2. Wybierz subskrypcję, do której chcesz utworzyć do nowej strefy DNS.

        azure account set <subscription name>

3. Azure usługa DNS jest Azure tylko do Menedżera zasobów, aby polecenie Azure należy przełączyć do trybu Menedżera zasobów.

        azure config mode arm

4. Aby korzystać z usługi Azure DNS, musisz zarejestrować subskrypcji, aby korzystać z dostawcy zasobów Microsoft.Network. (Jest to operacja jednorazowa dla każdej subskrypcji).

        azure provider register Microsoft.Network

5. Jeśli nie masz już, musisz też utworzyć grupę zasobów Menedżera zasobów.

        azure group create myresourcegroup westeurope

6. Aby zaimportować strefie **contoso.com** z pliku **contoso.com.txt** do nowej strefy DNS w grupie zasobów **myresourcegroup**, uruchom polecenie `azure network dns zone import`.<BR>To polecenie Załaduj plik strefy i analizować je. Polecenie zostanie wykonane serię poleceń w usłudze Azure DNS do utworzenia strefy i wszystkie rekordu zestawy w tej strefie. Polecenie będzie również raporty dotyczące postępu w oknie konsoli wraz z występują błędy lub ostrzeżenia. Ponieważ zestawy rekordów są tworzone w serii, może potrwać kilka minut, aby zaimportować plik dużej strefy.

        azure network dns zone import myresourcegroup contoso.com contoso.com.txt



### <a name="step-2-verify-the-zone"></a>Krok 2. Sprawdź strefy

Aby sprawdzić strefy DNS po zaimportowaniu pliku, można użyć jednej z następujących metod:

- Rekordy można wyświetlić za pomocą następującego polecenia polecenie Azure.

        azure network dns record-set list myresourcegroup contoso.com

- Rekordy można wyświetlić za pomocą polecenia cmdlet programu PowerShell `Get-AzureRmDnsRecordSet`.

- Możesz użyć `nslookup` do weryfikacji rozpoznawania nazw dla rekordów. Ponieważ strefa nie jest jeszcze delegować, będzie konieczne jawnie określ poprawne serwery nazw Azure DNS. Przykładowe poniżej pokazano, jak pobrać nazwy serwerów nazw przypisane do strefy. IT również pokazano, jak kwerendy rekord "www" przy użyciu `nslookup`.

        C:\>azure network dns record-set show myresourcegroup contoso.com @ NS
        info:Executing command network dns record-set show
        + Looking up the DNS Record Set "@" of type "NS"
        data:Id: /subscriptions/…/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/NS/@
        data:Name: @
        data:Type: Microsoft.Network/dnszones/NS
        data:Location: global
        data:TTL : 3600
        data:NS records
        data:Name server domain name : ns1-01.azure-dns.com
        data:Name server domain name : ns2-01.azure-dns.net
        data:Name server domain name : ns3-01.azure-dns.org
        data:Name server domain name : ns4-01.azure-dns.info
        data:
        info:network dns record-set show command OK

        C:\> nslookup www.contoso.com ns1-01.azure-dns.com

        Server: ns1-01.azure-dns.com
        Address:  40.90.4.1

        Name:www.contoso.com
        Addresses:  134.170.185.46
        134.170.188.221

### <a name="step-3-update-dns-delegation"></a>Krok 3. Aktualizowanie delegowania DNS

Po upewnieniu się, że strefy został prawidłowo zaimportowany, będzie konieczne zaktualizowanie delegowania usługi DNS, aby wskazywały serwery nazw Azure DNS. Aby uzyskać więcej informacji zobacz artykuł [aktualizowanie delegowania usługi DNS](dns-domain-delegation.md).

## <a name="export-a-dns-zone-file-from-azure-dns"></a>Eksportowanie pliku strefy DNS z Azure DNS

Format polecenie polecenie Azure, aby zaimportować strefy DNS jest:<BR>`azure network dns zone export [options] <resource group> <zone name> <zone file name>`

Wartości:

- `<resource group>`jest nazwą grupy zasobów dla strefy Azure DNS.
- `<zone name>`jest nazwą strefy.
- `<zone file name>`to ścieżka i nazwa pliku strefy do wyeksportowania.

Jako importu strefy najpierw muszą się zalogować, wybraniu subskrypcji i skonfigurować polecenie Azure do korzystania z trybu Menedżera zasobów.

### <a name="to-export-a-zone-file"></a>Aby wyeksportować plik strefy


1. Zaloguj się do subskrypcji usługi Azure za pomocą interfejsu wiersza polecenia Azure.

        azure login

2. Wybierz subskrypcję, do której chcesz utworzyć do nowej strefy DNS.

        azure account set <subscription name>

3. Azure DNS jest usługą Azure tylko do Menedżera zasobów. Polecenie Azure musi być przełączane do trybu Menedżera zasobów.

        azure config mode arm

4. Aby wyeksportować istniejących Azure DNS zone **contoso.com** , w grupie zasobów **myresourcegroup** do pliku **contoso.com.txt** (w bieżącym folderze), uruchom `azure network dns zone export`. To polecenie nawiąże połączenie usługi Azure DNS wyliczanie zestawy rekordów w strefie i eksportowanie wyników do pliku strefy zgodnego z programem powiązania.

        azure network dns zone export myresourcegroup contoso.com contoso.com.txt

