<properties 
   pageTitle="Rozdzielczość maszyny wirtualne i wystąpienia roli"
   description="Nazwa scenariusze rozdzielczość dla IaaS Azure, rozwiązania hybrydowe między usługami w chmurze różnych, usługi Active Directory i przy użyciu serwera DNS "
   services="virtual-network"
   documentationCenter="na"
   authors="GarethBradshawMSFT"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/31/2016"
   ms.author="telmos" />

# <a name="name-resolution-for-vms-and-role-instances"></a>Rozpoznawanie nazw maszyny wirtualne i wystąpień roli

W zależności od tego, jak używać Azure do obsługi IaaS, PaaS i rozwiązania hybrydowe może być konieczne Zezwalaj maszyny wirtualne i wystąpienia roli, które zostało utworzone w celu komunikowania się między sobą. Ten komunikat jest możliwe przy użyciu adresów IP, ale jest znacznie upraszcza użyć nazwy, które można łatwo zapamiętać nie będzie miało wpływu. 

Jeśli wystąpienia roli i maszyny wirtualne obsługiwane platformy Azure rozpoznawanie nazw domen do adresów IP, można użyć jednej z dwóch metod:

- [Rozpoznawanie nazw dostarczonego przez Azure](#azure-provided-name-resolution)

- [Rozpoznawanie nazw przy użyciu serwera DNS](#name-resolution-using-your-own-dns-server) (co może powodować przekazywanie kwerendy do serwerów DNS firmy Azure) 

Typ rozpoznawania nazw, którego używasz, zależy od tego, jak maszyny wirtualne i roli wystąpienia usługi konieczne komunikowania się między sobą.

**W poniższej tabeli przedstawiono scenariusze i odpowiednie rozwiązania rozpoznawanie nazwy:**

| **Scenariusz** | **Rozwiązanie** | **Sufiks** |
|--------------|--------------|----------|
| Rozpoznawanie nazw między wystąpienia roli lub maszyny wirtualne znajduje się w tym samym usługi w chmurze lub wirtualnej sieci | [Rozpoznawanie nazw dostarczonego przez Azure](#azure-provided-name-resolution)| Nazwa hosta lub nazwy FQDN |
| Rozpoznawanie nazw między wystąpienia roli lub maszyny wirtualne znajduje się w różnych wirtualnych sieci | Zarządzane przez klienta serwerów DNS przesyłania dalej kwerend między vnets rozpoznawania przez Azure (serwer proxy DNS).  zobacz [Rozpoznawanie nazw przy użyciu serwera DNS](#name-resolution-using-your-own-dns-server)| Tylko FQDN |
| Rozpoznawanie nazw komputera i Usługa lokalnego z roli wystąpienia lub maszyny wirtualne platformy Azure | Zarządzane przez klienta serwerów DNS (kontrolera domeny np lokalnymi, kontrolera domeny lokalnej tylko do odczytu lub synchronizowane za pomocą transferu strefy DNS pomocniczej).  Zobacz [Rozpoznawanie nazw przy użyciu serwera DNS](#name-resolution-using-your-own-dns-server)|Tylko FQDN |
| Na stronie Azure nazwy hostów z lokalnego komputerów | Przesyłanie dalej zapytań do serwera proxy DNS zarządzane przez klienta w odpowiednich vnet serwer proxy przekazuje kwerendy Azure rozpoznawania. Zobacz [Rozpoznawanie nazw przy użyciu serwera DNS](#name-resolution-using-your-own-dns-server)| Tylko FQDN |
| Odwracanie DNS dla wewnętrznych adresy IP | [Rozpoznawanie nazw przy użyciu serwera DNS](#name-resolution-using-your-own-dns-server) | n/d! |
| Rozpoznawanie nazw między maszyny wirtualne lub wystąpień roli znajduje się w chmurze różnych usług, nie znajdują się w wirtualnej sieci| Nie dotyczy. Łączność między maszyny wirtualne i wystąpienia ról w chmurze różnych usługach nie jest obsługiwane poza wirtualnej sieci.| n/d! |



## <a name="azure-provided-name-resolution"></a>Rozpoznawanie nazw dostarczonego przez Azure

Wraz z rozpoznawania nazw DNS publicznej Azure udostępnia rozpoznawanie nazw wewnętrznych maszyny wirtualne i wystąpień roli, które znajdują się w tej samej sieci wirtualnej lub usługa w chmurze.  Maszyny wirtualne/wystąpień w usłudze w chmurze udostępnić samej sufiks DNS (wystarczy tylko nazwa hosta), ale w chmurze różnych klasyczny wirtualnych sieci usług mają różnych sufiksów DNS więc nazwy FQDN jest potrzebny do rozpoznawania nazw między usługami w chmurze różnych.  W sieci wirtualne w modelu wdrożenia Menedżera zasobów sufiks DNS jest zgodna sieci wirtualnych (aby Kwalifikowaną nie jest potrzebna) i nazw DNS można przypisywać do nic i maszyny wirtualne. Mimo że rozpoznawanie nazw dostarczonego przez Azure wymaga konfiguracji, nie jest odpowiednim wyborem dla wszystkich scenariuszy wdrażania, jak pokazano w powyższej tabeli.

> [AZURE.NOTE] W przypadku ról w sieci web i pracownika można także przejść adresów IP wystąpień roli na podstawie liczby i wystąpienie roli za pomocą interfejsu API usługi REST Azure usługi zarządzania. Aby uzyskać więcej informacji zobacz [Odwołanie interfejsu API usługi zarządzania pozostałych](https://msdn.microsoft.com/library/azure/ee460799.aspx).

### <a name="features-and-considerations"></a>Funkcje i zagadnienia

**Funkcje:**

- Łatwość użytkowania: Aby można było używać rozpoznawania nazw dostarczonego przez Azure jest wymagana żadna konfiguracja.

- Usługa rozpoznawania nazw dostarczonego przez Azure jest wysoce dostępna, zapisywania, możesz konieczności tworzyć i zarządzać nimi klastrów serwerów DNS.

- Można w połączeniu z serwerów DNS rozwiązywać zarówno w wersji lokalnej i Azure nazwy hostów.

- Rozpoznawanie nazw znajduje się między roli wystąpienia-pośrednictwem w tym samym usługi w chmurze bez potrzeby FQDN SMS.

- Rozpoznawanie nazw znajduje się między maszyny wirtualne w wirtualnej sieci, które korzystają z modelu wdrożenia Menedżera zasobów, bez potrzeby nazwy FQDN. Wirtualnych sieci w modelu Klasyczny wdrożenia wymagają nazwy FQDN podczas rozpoznawania nazw w chmurze różnych usługach. 

- Możesz użyć nazwy hostów, które najlepiej opisują wdrożeń, zamiast pracy z nazwami generowane automatycznie.

**Zagadnienia:**

- Nie można zmodyfikować sufiks DNS utworzone Azure.

- Nie można ręcznie zarejestrować swoje rekordy.

- WINS i NetBIOS nie są obsługiwane. (Nie widać usługi maszyny wirtualne w Eksploratorze Windows).

- Nazwy hostów musi być zgodny z DNS (muszą korzystać tylko 0-9, a-z i "-", a nie rozpoczęcie lub zakończenie z "-". Zobacz RFC 3696 sekcji 2).

- Dla każdego maszyn wirtualnych jest ograniczenie ruchu kwerend DNS. To nie powinny mieć wpływ na większości aplikacji.  Jeżeli obserwuje się ograniczanie żądań, upewnij się, że włączone jest buforowanie po stronie klienta.  Aby uzyskać więcej informacji zobacz [wprowadzenie najlepiej rozpoznawania nazw dostarczonego przez Azure](#Getting-the-most-from-Azure-provided-name-resolution).

- Tylko maszyny wirtualne w pierwszym usług w chmurze 180 są rejestrowane dla każdej wirtualnej sieci w modelu Klasyczny wdrożenia. To nie dotyczy wirtualnych sieci w modelach wdrożenia Menedżera zasobów.


### <a name="getting-the-most-from-azure-provided-name-resolution"></a>Uzyskiwanie najbardziej z rozpoznawanie nazw dostarczonego przez Azure
**Buforowanie po stronie klienta:**

Nie każda kwerenda DNS musi być przesyłane przez sieć.  Po stronie klienta pomaga ograniczyć liczbę opóźnienie i zwiększyć odporność na blips sieci rozwiązywanie cykliczne kwerendy DNS z lokalnej pamięci podręcznej.  Rekordy DNS zawierają Time To Live (TTL) umożliwiającym pamięci podręcznej do przechowywania rekordu tak długo bez wpływania aktualność rekord, aby po stronie klienta jest odpowiedni dla większości sytuacji.

Domyślny klient DNS systemu Windows ma wbudowane pamięci podręcznej DNS.  Niektóre Linux distros nie powinno obejmować pamięci podręcznej domyślnie, zaleca się jeden dodania do każdego maszyn wirtualnych Linux (po sprawdzeniu, że nie istnieje lokalnej pamięci podręcznej już).

Istnieje wiele różnych DNS buforowania pakiety, np. dnsmasq, zapoznaj się z instrukcjami, aby zainstalować dnsmasq na najbardziej typowych distros:

- **Ubuntu (używa resolvconf)**:
    - wystarczy zainstalować pakiet dnsmasq ("get stanie instalacji dnsmasq sudo").
- **SUSE (używa netconf)**:
    - Zainstaluj pakiet dnsmasq ("sudo zypper instalacji dnsmasq") 
    - Włączanie usługi dnsmasq ("Włącz dnsmasq.service systemctl") 
    - Uruchom usługę dnsmasq ("systemctl start dnsmasq.service") 
    - Edytowanie "/ itp sysconfig sieci i konfiguracji" i zmień NETCONFIG_DNS_FORWARDER = "" do "dnsmasq"
    - Aktualizowanie resolv.conf ("Aktualizacja netconfig"), aby ustawić pamięci podręcznej jako lokalnego programu rozpoznawania nazw DNS
- **OpenLogic (używa NetworkManager)**:
    - Zainstaluj pakiet dnsmasq ("sudo yum instalacji dnsmasq")
    - Włączanie usługi dnsmasq ("Włącz dnsmasq.service systemctl")
    - Uruchom usługę dnsmasq ("systemctl start dnsmasq.service")
    - Dodawanie "dołączy domain name servers 127.0.0.1;" do "/etc/dhclient-eth0.conf"
    - Uruchom usługę sieci ("usługi sieci ponownego uruchomienia"), aby ustawić pamięci podręcznej jako lokalnego programu rozpoznawania nazw DNS

> [AZURE.NOTE] Pakiet "dnsmasq" jest tylko jeden z wielu pamięć podręczną DNS, dostępne Linux.  Przed użyciem, sprawdź, czy jej przydatności do określonych potrzeb, a nie pamięci podręcznej jest zainstalowany.

**Ponowne próby po stronie klienta:**

System DNS jest przede wszystkim protokołu UDP.  Jak protokół UDP nie gwarantuje dostarczenia wiadomości, ponów próbę logicznych jest obsługiwany w samego protokołu DNS.  Każdy komputer kliencki DNS (systemu operacyjnego) może powodować wystąpienie Logika ponawiania różne w zależności od preferencji twórcami:

 - Systemy operacyjne Windows ponów próbę po 1 drugi, a następnie ponownie po innym 2, 4 i innego 4 sekundy. 
 - Domyślne prób konfiguracji Linux po 5 sekundach.  Zalecane jest to zmienić, tak aby ponowić próbę 5 razy w drugim 1 w określonych odstępach.  

Aby sprawdzić bieżące ustawienia na Głosowa Linux, "kot /etc/resolv.conf" i sprawdź dane w wierszu "Opcje", np.:

    options timeout:1 attempts:5

Plik resolv.conf jest zazwyczaj generowane automatycznie i nie powinny być edytowane.  Określone instrukcje dotyczące dodawania wiersza "Opcje" zależy od distro:

- **Ubuntu** (korzysta z resolvconf):
    - Dodawanie linii opcji "/ etc/resolveconf/resolv.conf.d/head" 
    - Uruchamianie "resolvconf -u" aktualizacji
- **SUSE** (korzysta z netconf):
    - Dodawanie "timeout:1 prób: 5" do NETCONFIG_DNS_RESOLVER_OPTIONS = "" parametr w "/ itp sysconfig sieci i konfiguracji" 
    - Uruchom "netconfig aktualizację" aktualizacji
- **OpenLogic** (korzysta z NetworkManager):
    - Dodawanie "echo"Opcje timeout:1 prób: 5"" do "-etc/NetworkManager/dispatcher.d/11-dhclient" 
    - Uruchamianie "sieć ponowne uruchomienie usługi" aktualizacji

## <a name="name-resolution-using-your-own-dns-server"></a>Rozpoznawanie nazw przy użyciu serwera DNS
Istnieje wiele sytuacji, w przypadku, gdy potrzeb rozpoznawanie nazw może Przejdź oprócz funkcji dostępnych na Azure, na przykład gdy używasz domen usługi Active Directory lub gdy jest wymagane rozpoznawania nazw DNS między wirtualnych sieci (vnets).  Dotyczyć scenariuszom Azure umożliwia przy użyciu serwerów DNS.  

Serwery DNS w sieci wirtualnej może przekazywać kwerendy DNS do rozpoznawania nazw rekurencyjne Azure i rozwiązywania nazwy hostów w wirtualnej sieci.  Na przykład domeny kontrolera (kontrolera domeny) w Azure można odpowiedzieć na pytania DNS dla swojej domeny i przesyłanie dalej wszystkie inne kwerendy Azure.  Dzięki temu maszyny wirtualne wyświetlić zasoby lokalna (za pośrednictwem kontrolera domeny) oraz dostarczonego przez Azure nazwy hostów (za pośrednictwem usługi przesyłania dalej).  Dostęp do rozpoznawania nazw cykliczne w Azure jest udostępniana za pośrednictwem wirtualnych adresów IP 168.63.129.16.

Przesyłanie dalej DNS również umożliwia rozpoznawania nazw DNS między vnet oraz umożliwia komputery lokalnego rozwiązania dostarczonego przez Azure nazwy hostów.  Aby rozwiązać hostname maszyn wirtualnych, serwera DNS maszyn wirtualnych musi znajdować się w tej samej sieci wirtualnej i można skonfigurować do przodu hostname kwerend Azure.  Sufiks DNS różni się w każdej vnet, program reguły warunkowe przesyłanie dalej można wysyłać kwerendy DNS do poprawne vnet rozdzielczość.  Poniższa ilustracja przedstawia dwa vnets i sieć lokalna, wykonując rozpoznawania nazw DNS vnet między tą metodą.  Przykład przesyłania jest dostępne w [galerii szablonów Szybki Start Azure](https://azure.microsoft.com/documentation/templates/301-dns-forwarder/) i [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/301-dns-forwarder).

![Między vnet DNS](./media/virtual-networks-name-resolution-for-vms-and-role-instances/inter-vnet-dns.png)

Przy użyciu rozpoznawania nazwy podanej Azure, sufiks DNS wewnętrzny (*. internal.cloudapp.net) jest udostępniana w poszczególnych maszyn wirtualnych za pomocą protokołu DHCP.  Umożliwia rozpoznawanie nazwy hosta, jak nazwa hosta rekordy znajdują się w strefie internal.cloudapp.net.  Gdy używasz rozwiązanie rozdzielczość własną nazwę, sufiks IDN nie jest podany do maszyny wirtualne, ponieważ zakłócać innych architekturach DNS (na przykład scenariusze domeny).  Zamiast tego udostępniamy symbolu zastępczego przerwy w działaniu (reddog.microsoft.com).  

W razie potrzeby można określić sufiks DNS wewnętrznych, używając programu PowerShell lub interfejsu API:

-  Wirtualnych sieci w modelach wdrożenia Menedżera zasobów sufiks jest dostępna za pośrednictwem zasobów [karty sieciowej](https://msdn.microsoft.com/library/azure/mt163668.aspx) lub za pośrednictwem polecenia cmdlet [Get-AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt619434.aspx) .    
-  W modelach klasyczny wdrożenia sufiks jest dostępne za pośrednictwem połączenia [Uzyskiwanie interfejsu API wdrożenia](https://msdn.microsoft.com/library/azure/ee460804.aspx) lub za pośrednictwem [Get AzureVM-debugowanie](https://msdn.microsoft.com/library/azure/dn495236.aspx) polecenia cmdlet.


Przekazywanie zapytań Azure nie potrzeb, będzie konieczne podanie rozwiązanie DNS.  Musisz rozwiązania DNS:

-  Umożliwiają rozpoznawanie odpowiednie hostname, np. przez [DDNS](virtual-networks-name-resolution-ddns.md).  Uwaga: Jeśli przy użyciu DDNS może być konieczne wyłączenie dzierżaw DHCP Azure osoby są bardzo długo i oczyszczania oczyszczania rekordu DNS może usunąć rekordy DNS przed czasem. 
-  Podaj rozpoznawanie cykliczne odpowiednie umożliwiająca rozpoznawanie nazw domen zewnętrznych.
-  Być dostępna (TCP i UDP na porcie 53) z klientów, z którymi służy i będą mogli uzyskać dostęp do Internetu.
-  Zabezpieczone przed dostępem z Internetu, ograniczanie zagrożeń powodowanych czynników zewnętrznych.

> [AZURE.NOTE] Celu uzyskania najlepszej wydajności używając maszyny wirtualne Azure jako serwery DNS, należy wyłączyć protokołu IPv6 i [Poziomu wystąpienia publiczny adres IP](virtual-networks-instance-level-public-ip.md) powinny być przypisane do każdego serwera DNS maszyn wirtualnych.  Jeśli chcesz używać jako swojego serwera DNS systemu Windows Server, [w tym artykule](http://blogs.technet.com/b/networking/archive/2015/08/19/name-resolution-performance-of-a-recursive-windows-dns-server-2012-r2.aspx) znajdują się dodatkowe analizy i optymalizacje.


### <a name="specifying-dns-servers"></a>Określanie serwerów DNS

Podczas korzystania z serwerów DNS Azure umożliwia określenie wiele serwerów DNS na wirtualnej sieci lub na sieciowej (Menedżer zasobów) / (klasyczny) usługa w chmurze.  Serwery DNS określone dla chmury usługi i sieciowej uzyskać pierwszeństwa przez określone dla wirtualnej sieci.

> [AZURE.NOTE] Właściwości połączenia sieciowego, takie jak serwera DNS adresy IP, nie powinny być edytowane bezpośrednio z poziomu maszyny wirtualne systemu Windows, jak mogą uzyskać usunięte podczas pracy usługi poprawianego po otrzymuje zastępowana wirtualnych sieciowej. 


Podczas używania modelu wdrożenia Menedżera zasobów, serwerów DNS można określić w portalu interfejsu API/szablony ([vnet](https://msdn.microsoft.com/library/azure/mt163661.aspx), [nic](https://msdn.microsoft.com/library/azure/mt163668.aspx)) lub programu PowerShell ([vnet](https://msdn.microsoft.com/library/mt603657.aspx), [nic](https://msdn.microsoft.com/library/mt619370.aspx)).

Podczas używania modelu Klasyczny wdrożenia, serwerów DNS sieci wirtualnej można określić w portalu lub [pliku *Konfiguracji sieci* ](https://msdn.microsoft.com/library/azure/jj157100).  Dla usług w chmurze do serwerów DNS określono za pośrednictwem [pliku *Konfiguracji usługi* ](https://msdn.microsoft.com/library/azure/ee758710) lub w programie PowerShell ([AzureVM nowy](https://msdn.microsoft.com/library/azure/dn495254.aspx)).

> [AZURE.NOTE] Jeśli zmienisz ustawienia DNS dla wirtualnych sieci i wirtualnych komputera, na którym jest już zainstalowany, należy ponownie uruchomić każdego dotyczy maszyn wirtualnych zmiany zostały wprowadzone.


## <a name="next-steps"></a>Następne kroki

Model wdrażania Menedżera zasobów:

- [Tworzenie lub aktualizowanie wirtualnej sieci](https://msdn.microsoft.com/library/azure/mt163661.aspx)
- [Tworzenie lub aktualizowanie karty sieciowej](https://msdn.microsoft.com/library/azure/mt163668.aspx)
- [Nowy AzureRmVirtualNetwork](https://msdn.microsoft.com/library/mt603657.aspx)
- [Nowy AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt619370.aspx)

 
Model klasyczny wdrażania:

- [Schemat konfiguracji usługi Azure](https://msdn.microsoft.com/library/azure/ee758710)
- [Schemat konfiguracji wirtualnej sieci](https://msdn.microsoft.com/library/azure/jj157100)
- [Konfigurowanie wirtualnej sieci przy użyciu pliku konfiguracji sieci](virtual-networks-using-network-configuration-file.md) 

