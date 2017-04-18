<properties 
   pageTitle="Opcje rozpoznawania nazw DNS dla maszyny wirtualne Linux platformy Azure"
   description="Nazwa usługi DNS scenariusze maszyny wirtualne Linux w IaaS Azure, w tym dostępne rozdzielczości, hybrydowych zewnętrznych serwerów DNS i wyświetlić swój własny DNS."
   services="virtual-machines"
   documentationCenter="na"
   authors="RicksterCDN"
   manager="timlt"
   editor="tysonn" />
<tags 
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/19/2016"
   ms.author="rclaus" />

# <a name="dns-name-resolution-options-for-linux-vms-in-azure"></a>Opcje rozpoznawania nazw DNS dla maszyny wirtualne Linux platformy Azure

Azure przewiduje rozpoznawania nazw DNS domyślnie wszystkie maszyny wirtualne znajdujących się pojedynczego wirtualnej sieci. Jesteś w stanie stosować własne rozwiązanie rozpoznawanie nazwy DNS konfigurując usługi DNS dla usługi Azure hostowanej maszyny wirtualne. Następujące scenariusze należy pomagają w określeniu, która sprawdza się lepiej dla określonej sytuacji.

- [Rozpoznawanie nazw dostarczonego przez Azure](#azure-provided-name-resolution)

- [Rozpoznawanie nazw przy użyciu serwera DNS](#name-resolution-using-your-own-dns-server) 

Typ rozpoznawania nazw, którego używasz, zależy od tego, jak maszyny wirtualne i roli wystąpienia usługi konieczne komunikowania się między sobą.

**W poniższej tabeli przedstawiono scenariusze i odpowiednie rozwiązania rozpoznawanie nazwy:**

| **Scenariusz** | **Rozwiązanie** | **Sufiks** |
|--------------|--------------|----------|
| Rozpoznawanie nazw między wystąpienia roli lub znajdującego się w tej samej sieci wirtualnej maszyny wirtualne | [Rozpoznawanie nazw dostarczonego przez Azure](#azure-provided-name-resolution)| Nazwa hosta lub nazwy FQDN |
| Rozpoznawanie nazw między wystąpienia roli lub maszyny wirtualne znajduje się w różnych wirtualnych sieci | Zarządzane przez klienta serwerów DNS przesyłania dalej kwerend między vnets rozpoznawania przez Azure (serwer proxy DNS).  zobacz [Rozpoznawanie nazw przy użyciu serwera DNS](#name-resolution-using-your-own-dns-server)| Tylko FQDN |
| Rozpoznawanie nazw komputera i Usługa lokalnego z roli wystąpienia lub maszyny wirtualne platformy Azure | Zarządzane przez klienta serwerów DNS (na przykład kontrolera domeny lokalnego, kontrolera domeny lokalnej tylko do odczytu lub synchronizowane za pomocą transferu strefy DNS pomocniczej).  Zobacz [Rozpoznawanie nazw przy użyciu serwera DNS](#name-resolution-using-your-own-dns-server)|Tylko FQDN |
| Na stronie Azure nazwy hostów z lokalnego komputerów | Przesyłanie dalej zapytań do serwera proxy DNS zarządzane przez klienta w odpowiednich vnet serwer proxy przekazuje kwerendy Azure rozpoznawania. Zobacz [Rozpoznawanie nazw przy użyciu serwera DNS](#name-resolution-using-your-own-dns-server)| Tylko FQDN |
| Odwracanie DNS dla wewnętrznych adresy IP | [Rozpoznawanie nazw przy użyciu serwera DNS](#name-resolution-using-your-own-dns-server) | n/d! |

## <a name="azure-provided-name-resolution"></a>Rozpoznawanie nazw dostarczonego przez Azure

Wraz z rozpoznawania nazw DNS publicznej Azure udostępnia rozpoznawanie nazw wewnętrznych dla maszyny wirtualne i wystąpienia roli, które znajdują się w tej samej sieci wirtualnej.  W oparty na architekturze ARM wirtualnych sieci sufiks DNS jest spójna w wirtualnej sieci (aby Kwalifikowaną nie jest potrzebna) i nazw DNS można przypisywać do nic i maszyny wirtualne. Mimo że rozpoznawanie nazw dostarczonego przez Azure wymaga konfiguracji, nie jest odpowiednim wyborem dla wszystkich scenariuszy wdrażania, jak pokazano w powyższej tabeli.

### <a name="features-and-considerations"></a>Funkcje i zagadnienia

**Funkcje:**

- Łatwość użytkowania: konfiguracja nie jest wymagane do nazwy podanej Azure rozdzielczość.

- Usługa rozpoznawania nazw dostarczonego przez Azure jest wysoce dostępna, zapisywania, możesz konieczności tworzyć i zarządzać nimi klastrów serwerów DNS.

- Można wraz z serwerów DNS rozwiązywać zarówno w wersji lokalnej i Azure nazwy hostów.

- Rozpoznawanie nazw znajduje się między maszyny wirtualne w wirtualnych sieci bez potrzeby nazwy FQDN. 

- Możesz użyć nazwy hostów, które najlepiej opisują wdrożeń, zamiast pracy z nazwami generowane automatycznie.

**Zagadnienia:**

- Nie można zmodyfikować sufiks DNS utworzone Azure.

- Nie można ręcznie zarejestrować swoje rekordy.

- WINS i NetBIOS nie są obsługiwane.

- Nazwy hostów musi być zgodny z DNS (muszą korzystać tylko 0-9, a-z i "-", a nie rozpoczęcie lub zakończenie z "-". Zobacz RFC 3696 sekcji 2).

- Dla każdego maszyn wirtualnych jest ograniczenie ruchu kwerend DNS. To nie powinny mieć wpływ na większości aplikacji.  Jeżeli obserwuje się ograniczanie żądań, upewnij się, że włączone jest buforowanie po stronie klienta.  Aby uzyskać więcej informacji zobacz [wprowadzenie najlepiej rozpoznawanie nazw dostarczonego przez Azure](#Getting-the-most-from-Azure-provided-name-resolution).


### <a name="getting-the-most-from-azure-provided-name-resolution"></a>Uzyskiwanie najbardziej z rozpoznawanie nazw dostarczonego przez Azure
**Buforowanie po stronie klienta:**

Nie każda kwerenda DNS są wysyłane przez sieć.  Po stronie klienta pomaga ograniczyć liczbę opóźnienie i zwiększyć odporność na blips sieci rozwiązywanie cykliczne kwerendy DNS z lokalnej pamięci podręcznej.  Rekordy DNS zawierają Time To Live (TTL) umożliwiającym pamięci podręcznej do przechowywania rekordu tak długo bez wpływania aktualność rekordu.  Z tego powodu po stronie klienta jest odpowiedni dla większości sytuacji.

Niektóre distros Linux nie powinno obejmować pamięci podręcznej domyślnie.  Zaleca się, że możesz dodać je do każdego maszyn wirtualnych Linux (po sprawdzeniu, że nie istnieje lokalnej pamięci podręcznej już).

Istnieje kilka różnych DNS buforowania pakiety, na przykład dnsmasq, zapoznaj się z instrukcjami, aby zainstalować dnsmasq na najbardziej typowych distros:

- **Ubuntu (używa resolvconf)**:
    - Zainstaluj pakiet dnsmasq ("get stanie instalacji dnsmasq sudo").
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

> [AZURE.NOTE]: Pakiet "dnsmasq" jest tylko jeden z wielu pamięć podręczną DNS, dostępne Linux.  Przed użyciem, sprawdzanie ich przydatności do określonych potrzeb, a nie pamięci podręcznej jest zainstalowany.

**Ponowne próby po stronie klienta:**

System DNS jest przede wszystkim protokołu UDP.  Jak protokół UDP nie gwarantuje dostarczenia wiadomości, ponów próbę logicznych jest obsługiwany w samego protokołu DNS.  Każdy komputer kliencki DNS (systemu operacyjnego) może powodować wystąpienie Logika ponawiania różne w zależności od preferencji twórcami:

 - Systemy operacyjne Windows ponów próbę po jednokrotnym drugi, a następnie ponownie po innym dwóch, czterech i innego cztery sekundy. 
 - Domyślne prób konfiguracji Linux po pięciu sekundach.  Należy zmienić tę opcję, aby ponowić próbę pięć razy na sekundę.  

Aby sprawdzić bieżące ustawienia na Głosowa Linux, "kot /etc/resolv.conf" i sprawdź dane w wierszu "Opcje", na przykład:

    options timeout:1 attempts:5

Plik resolv.conf jest generowane automatycznie i nie powinny być edytowane.  Określone instrukcje dotyczące dodawania wiersza "Opcje" zależy od distro:

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
Istnieje kilka sytuacji, w której potrzeb rozpoznawanie nazw może wykracza poza funkcji, pod warunkiem, Azure, na przykład gdy jest wymagane rozpoznawania nazw DNS między wirtualnych sieci (vnets).  Dotyczyć w tym scenariuszu Azure umożliwia przy użyciu serwerów DNS.  

Serwery DNS w sieci wirtualnej może przekazywać kwerendy DNS do rozpoznawania nazw rekurencyjne Azure i rozwiązywania nazwy hostów w wirtualnej sieci.  Na przykład serwera DNS platformy Azure można odpowiedzieć na pytania DNS dla własne pliki strefy DNS i przesyłanie dalej wszystkie inne kwerendy Azure.  Dzięki temu maszyny wirtualne wyświetlić obie pozycje w plikach strefy i dostarczonego przez Azure nazwy hostów (za pośrednictwem usługi przesyłania dalej).  Dostęp do rozpoznawania nazw cykliczne w Azure jest udostępniana za pośrednictwem wirtualnych adresów IP 168.63.129.16.

Przesyłanie dalej DNS również umożliwia rozpoznawania nazw DNS między vnet oraz umożliwia komputery lokalnego rozwiązania dostarczonego przez Azure nazwy hostów.  Aby rozwiązać hostname maszyn wirtualnych, serwera DNS maszyn wirtualnych musi znajdować się w tej samej sieci wirtualnej i można skonfigurować do przodu hostname kwerend Azure.  Sufiks DNS różni się w każdej vnet, program reguły przesyłania dalej warunkowego można wysyłać kwerendy DNS do poprawne vnet rozdzielczość.  Poniższa ilustracja przedstawia dwa vnets i sieć lokalna, wykonując rozpoznawania nazw DNS między vnet przy użyciu tej metody:

![Między vnet DNS](./media/virtual-machines-linux-azure-dns/inter-vnet-dns.png)

Podczas rozpoznawania nazwy podanej Azure sufiks DNS wewnętrzny jest udostępniana w poszczególnych maszyn wirtualnych za pomocą protokołu DHCP.  Gdy używasz rozwiązanie rozdzielczość własną nazwę, ponieważ zakłócać innych architekturach DNS ten sufiks nie podano do maszyny wirtualne.  Aby odwołać się do urządzenia za nazwy FQDN lub skonfigurować sufiks na pośrednictwem usługi SMS, sufiks może zostać określony przy użyciu programu PowerShell lub interfejsu API:

-  Zarządzanie zasobami Azure zarządzane vnets, sufiks jest dostępna za pośrednictwem zasobów [karty sieciowej](https://msdn.microsoft.com/library/azure/mt163668.aspx) lub zostanie uruchomione polecenie `azure network public-ip show <resource group> <pip name>` Aby wyświetlić szczegóły swojego publiczny adres IP, łącznie z nazwy FQDN karty sieciowej.    


Przekazywanie zapytań Azure nie spełnia potrzeb użytkownika, musisz podać rozwiązanie DNS.  Rozwiązanie DNS musi:

-  Podaj odpowiednie hostname rozdzielczości, na przykład za pośrednictwem [DDNS](../virtual-network/virtual-networks-name-resolution-ddns.md).  Uwaga: Jeśli przy użyciu DDNS może być konieczne wyłączenie dzierżaw DHCP Azure osoby są bardzo długo i oczyszczania oczyszczania rekordu DNS może usunąć rekordy DNS przed czasem. 
-  Podaj rozpoznawanie cykliczne odpowiednie umożliwiająca rozpoznawanie nazw domen zewnętrznych.
-  Być dostępna (TCP i UDP na porcie 53) z klientów, z którymi służy i będą mogli uzyskać dostęp do Internetu.
-  Zabezpieczone przed dostępem z Internetu, ograniczanie zagrożeń powodowanych czynników zewnętrznych.

> [AZURE.NOTE] Celu uzyskania najlepszej wydajności używając maszyny wirtualne Azure jako serwery DNS, należy wyłączyć protokołu IPv6 i [Poziomu wystąpienia publiczny adres IP](../virtual-network/virtual-networks-instance-level-public-ip.md) powinny być przypisane do każdego serwera DNS maszyn wirtualnych.  

