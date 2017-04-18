<properties
    pageTitle="Projektowanie infrastruktury sieciowej awarii | Microsoft Azure"
    description="W tym artykule omówiono zagadnienia projektowe sieci odzyskiwania witryny Azure"
    services="site-recovery"
    documentationCenter=""
    authors="prateek9us"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="09/19/2016"
    ms.author="pratshar"/>

#  <a name="designing-your-network-infrastructure-for-disaster-recovery"></a>Projektowanie infrastruktury sieciowej awarii

W tym artykule są kierowane do informatyków, którzy są odpowiedzialne za Projektując, wykonania i pomocniczych ciągłości i infrastruktury odzyskiwania (BCDR) danych i chcą je wykorzystać Microsoft Azure witryny odzyskiwania (automatycznego odzyskiwania systemu) do pomocy technicznej i rozszerzenie ich usług BCDR. W tym dokumencie omówiono praktyczne zagadnienia związane z Menedżera maszyn wirtualnych systemu Centrum wdrażania serwera, ich wad i zalet rozciągnięty podsieci porównaniu podsieci pracy awaryjnej i struktury odzyskiwanie do wirtualnego witryn w programie Microsoft Azure.

## <a name="overview"></a>Omówienie

[Odzyskiwanie witryny Azure (ASR)](https://azure.microsoft.com/services/site-recovery/) to usługa Microsoft Azure, która orchestrates ochrony i odzyskiwania aplikacji wirtualnych firm ciągłości operacji odzyskiwania (BCDR). Ten dokument jest przeznaczony do przeprowadzenia czytelnika przez proces projektowania sieci, skoncentrowanie się na Projektując zakresów adresów IP i podsieci w witrynie odzyskiwania po awarii podczas replikacji wirtualnych maszyn przy użyciu witryny odzyskiwania.

Ponadto w tym artykule przedstawiono, jak Odzyskiwanie witryny umożliwia Projektując i implementowania wielooddziałowości wirtualnego centrum danych do obsługi usług BCDR w czasie badania lub awarii.

W świecie, w którym wszyscy oczekuje łączności 24-7 jest bardziej ważne niż kiedykolwiek przechowywać infrastruktury i aplikacji usługi i uruchomione. Zapewnianie ciągłości i odzyskiwanie danych (BCDR) jest przywrócenie elementów nie powiodło się, więc organizacji można szybko wznowić normalnego działania. Opracowywania strategii odzyskiwania danych na zajęcie się prawdopodobne, niszczące zdarzenia jest bardzo trudne. Jest to z powodu trudności związane z przewidywania w przyszłości, szczególnie dotyczy mało prawdopodobne wydarzeń i wysokich kosztów zapewnienie odpowiednich środków ochrony przed dalekosiężną catastrophes. 

Kluczowe znaczenie dla BCDR planowania, celem czasu odzyskiwania (RTO) i odzyskiwania punktów cel (RPO) należy zdefiniować jako część planu odzyskiwania danych. Po awarii trafiał klienta centrum danych, za pomocą Odzyskiwanie witryny Azure, klienci mogą szybko (RTO niski) przełączyć do trybu online ich zreplikowanej maszyn wirtualnych znajdujących się w centrum danych pomocniczą lub Microsoft Azure z utratą danych minimalne (RPO niski). 

Pracy awaryjnej jest możliwe dzięki automatycznego odzyskiwania systemu, która początkowo kopiuje wyznaczonych maszyn wirtualnych z poziomu Centrum danych podstawowych centrum danych pomocniczej lub Azure (w zależności od tego scenariusza) i okresowo odświeża repliki. Podczas planowania infrastruktury należy rozważyć jako potencjalne gardło zapobiec możesz firmy spotkania RTO i RPO celów projektu sieci.  

Planując wdrożenie rozwiązania odzyskiwania po awarii Administratorzy jedną z najważniejszych pytania w radosne jest jak maszyna wirtualna może być niedostępne po tym przełączeniu. Automatycznego odzyskiwania systemu umożliwia administratorom wybierz sieć, do której maszyna wirtualna może być połączony z po przełączeniu. Jeśli podstawowej witryny jest zarządzane przez serwer VMM następnie można to osiągnąć przy użyciu mapowania sieci. Aby uzyskać więcej informacji, zobacz [przygotować się do sieci mapowania](site-recovery-network-mapping.md) .

Podczas projektowania sieci witryny odzyskiwania, administrator musi dwie opcje:

- Użyj innego obszaru adresów IP dla sieci w witrynie odzyskiwania. W tym scenariuszu maszyny wirtualnej po przełączeniu zostanie wyświetlony nowy adres IP i administrator musi wykonać aktualizacji w systemie DNS. Przeczytaj więcej na temat sposobów wykonywania DNS aktualizowanie [tutaj](site-recovery-vmm-to-vmm.md#test-your-deployment) 
- Za pomocą samego zakresu adresów IP dla sieci w witrynie odzyskiwania. W niektórych scenariuszach administratorów woli zachować adresy IP, które mają w witrynie podstawowego nawet po tym przełączeniu. W scenariuszu normalny administrator musi zaktualizować trasy wskaż nową lokalizację adresów IP. Ale w scenariuszu miejsce, w którym rozciągnięty VLAN zostanie wdrożony między podstawowych i witrynami odzyskiwania zachowywania adresów IP dla maszyn wirtualnych staje się atrakcyjną opcję. Zachowanie samej IP adresy upraszcza proces odzyskiwania, nie podejmując z dala od komputera dowolna sieć powiązanych kroków wpis w przypadku awarii.


Planując wdrożenie rozwiązania odzyskiwania danych Administratorzy jedną z najważniejszych pytań ich pamiętać jest jak aplikacje będą dostępne po zakończeniu tym przełączeniu. Nowoczesne aplikacje prawie zawsze są zależne od sieci w pewnym stopniu, dlatego fizycznie przeniesienie reprezentuje usługę z jednej witryny do innej sieci wezwanie. Istnieją dwa główne sposoby, które ten problem wynika z rozwiązania odzyskiwania po awarii. Pierwszą metodę ma obsługiwać stały adresów IP. Pomimo usług, przenoszenie i serwerami hostingu w różnych miejscach aplikacje potrwać konfiguracji adresów IP z ich do nowej lokalizacji. Druga metoda polega na całkowite zmiana adresu IP podczas przejścia do witryny odzyskane. Każde podejście zawiera kilka zmian implementacji, które przedstawiono poniżej.

Podczas projektowania sieci witryny odzyskiwania, administrator musi dwie opcje:

## <a name="option-1-retain-ip-addresses"></a>Opcja 1: Zachowanie adresów IP 

Z perspektywy proces odzyskiwania po awarii przy użyciu stały IP adresy wydaje się Najprostszym sposobem zaimplementowania, ale został liczbę potencjalnych problemach, które w praktyce był ujęciu najmniej popularne. Odzyskiwanie witryny Azure umożliwia zachowanie adresów IP we wszystkich scenariuszach. Przed jedną zdecyduje się zachować IP, odpowiednie należy zwrócić uwagę na ograniczenia, które nakłada na możliwości przeniesienia awaryjnego. Pozwól nam Przyjrzyj się czynniki, które mogą ułatwić podjęcie decyzji, aby zachować adresy IP, czy nie. Można to osiągnąć na dwa sposoby, za pomocą rozciągnięty podsieci lub przez wykonanie pełnego podsieci trybie awaryjnym.

### <a name="stretched-subnet"></a>Rozciągnięty podsieci

W tym miejscu podsieci jest udostępniana jednocześnie zarówno w podstawowych i DR lokalizacji. W prostych słowach oznacza to, możesz przenieść serwer i konfiguracji IP (Layer 3) do drugiego miejsca i sieci będzie skierować ruch do nowej lokalizacji automatycznie. Jest to zbyt trudne zajęcie się z perspektywy serwera, ale ma wiele problemów:

- Z perspektywy warstwy 2 (warstwa łącza danych) będzie wymagała urządzeń sieciowych, który może zarządzać rozciągnięty VLAN, ale to stał się mniejszy problem jest teraz powszechnie dostępny. Problem z drugą i trudniejsze jest rozciągając VLAN potencjalne domeny błędów jest używane w obu witrynach zasadniczo staje się pojedynczego punktu awarii. W trakcie prawdopodobne wystąpienie ma się stało, że emisji Burza pracę, ale może okazać się odizolowane. Możemy znaleźć mieszanych opinie o tym problemie ostatniej i umieścić wiele implementacji pomyślnego również "Firma Microsoft nie wykona poniżej tej technologii".
- Podsieć rozciągnięty nie jest możliwe, jeśli korzystasz z programu Microsoft Azure jako witryny DR.


### <a name="subnet-failover"></a>Podsieć pracy awaryjnej

Istnieje możliwość zaimplementowania przełączanie awaryjne podsieci do uzyskania korzyści rozwiązania rozciągnięty podsieci opisanych powyżej bez rozciągając podsieci w wielu witrynach. W tym miejscu dowolnej danej podsieci może uczestniczyć w witryna 1 lub 2 witryny, ale nie w obu witrynach jednocześnie. Aby zachować obszaru adresów IP w przypadku trybie awaryjnym, prawdopodobnie programowy rozmieszczanie infrastruktury router przenieść podsieci z jednej witryny. Scenariusz pracy awaryjnej, który podsieci spowoduje przeniesienie z skojarzonego z chronionych maszyny wirtualne. Główną wadą tego rozwiązania jest w przypadku awarii należy przenieść całą podsieci, które mogą okazać się przycisk OK, ale mogą wpływać na zagadnienia szczegółowości pracy awaryjnej. 

Przeanalizujmy, jak fikcyjnej przedsiębiorstwa, o nazwie Contoso będzie mógł odtworzyć jej maszyny wirtualne w lokalizacji odzyskiwania podczas awarii na całą podsieć. Firma Microsoft będzie najpierw Przyjrzyj się jak Contoso jest możliwość zarządzania ich podsieci podczas replikacji maszyny wirtualne między dwoma lokalnego lokalizacji, a następnie omówimy będzie, jak pracy awaryjnej podsieci działa, gdy [Azure jest używany jako witryny odzyskiwania po awarii](#failover-to-azure).

#### <a name="failover-to-a-secondary-on-premises-site"></a>Przełączanie awaryjne do pomocniczym lokalnej witryny

Pozwól nam Przyjrzyj się scenariusz miejsce, w którym chcemy zachować IP każdego z pośrednictwem SMS i przełączaniem pełną podsieci razem. Podstawowy witryna zawiera aplikacje w 192.168.1.0/24 podsieci. Sytuacji tym przełączeniu wszystkich maszyn wirtualnych, które są częścią tej podsieci nie powiedzie się w witrynie odzyskiwania i zachować ich adresy IP. Trasy trzeba odpowiednio można modyfikować tak, aby odzwierciedlała fakt, że maszyn wirtualnych należących do podsieci 192.168.1.0/24 teraz przejścia do witryny odzyskiwania. 

Na poniższej ilustracji marszrut między podstawowej witryny i odzyskiwania witryny, trzecia witryny i podstawowej witryny i trzecia witryny i odzyskiwania witryny będą musiały odpowiednio można modyfikować. 

Poniższych obrazach przedstawiono podsieci przed tym przełączeniu. 192.168.0.1/24 podsieci jest aktywny w witrynie podstawowego przed tym przełączeniu i aktywnym witryny odzyskiwania po przełączeniu 

![Przed pracy awaryjnej](./media/site-recovery-network-design/network-design2.png)

Przed pracy awaryjnej


Na poniższym obrazie przedstawiono sieci i podsieci po przełączeniu.
    
![Po przełączeniu](./media/site-recovery-network-design/network-design3.png)

Po przełączeniu

W witrynie pomocniczej jest lokalnego i korzystania z serwera VMM do zarządzania, a następnie po włączeniu ochrony dla określonej maszyny wirtualnej, automatycznego odzyskiwania systemu będą przydzielane zasoby sieciowe według następujących przepływu pracy:

- Automatycznego odzyskiwania systemu przydziela adres IP dla każdego interfejsu sieciowego na komputerze wirtualnych z statycznej puli adresów IP, zdefiniowane sieci odpowiednie dla każdego wystąpienia VMM Centrum systemu.
- Jeśli administrator definiuje w tym samym puli adresów IP dla sieci w witrynie odzyskiwania co puli adresów IP sieci w witrynie podstawowego podczas przydzielania adres IP maszyn wirtualnych replice automatycznego odzyskiwania systemu chcesz przydzielić tego samego adresu IP, jak podstawowy maszyny wirtualnej.  IP zarezerwowanych VMM, ale nie ustawiono jako IP pracy awaryjnej. IP pracy awaryjnej jest ustawiony tuż przed tym przełączeniu.

![Zachowanie adresu IP](./media/site-recovery-network-design/network-design4.png)
    
Rysunek 5

Rysunek 5 pokazuje ustawienia TCP/IP pracy awaryjnej maszyny wirtualnej replice (na konsoli funkcji Hyper-V). Te ustawienia będą wypełnione, po prostu, przed uruchomieniem maszyny wirtualnej po przełączeniu

Jeśli samej adresów IP nie jest dostępna, automatycznego odzyskiwania systemu chcesz przydzielić kilka innych dostępny adres IP z zdefiniowanej puli adresów IP. 

Po włączeniu maszyn wirtualnych ochrony służy po przykładowy skrypt do weryfikacji IP, która została przydzielona maszyn wirtualnych. Tym samym IP czy Ustaw jako IP pracy awaryjnej i przypisane do maszyn wirtualnych w czasie pracy awaryjnej:

        $vm = Get-SCVirtualMachine -Name <VM_NAME>
        $na = $vm[0].VirtualNetworkAdapters>
        $ip = Get-SCIPAddress -GrantToObjectID $na[0].id
        $ip.address  

>[AZURE.NOTE] W sytuacji, gdy maszyn wirtualnych za pomocą DHCP Zarządzanie adresów IP jest całkowicie niezależnych automatycznego. Administrator musi upewnij się, że serwerze serwowania adresów IP w witrynie odzyskiwania może służyć z tego samego zakresu co podstawowej witryny.

#### <a name="failover-to-azure"></a>Przełączanie awaryjne do Azure

Odzyskiwanie witryny Azure (ASR) umożliwia Microsoft Azure do pełnić rolę witryny odzyskiwania po awarii w środowisku maszyn wirtualnych systemu. W tym przypadku należy postępować z jedną więcej ograniczeń. 

Przeanalizujmy scenariusza, w których fikcyjnej firmy o nazwie banku Woodgrove nie infrastruktury lokalnego hostingu ich linią aplikacji biznesowych i są one hostingu ich aplikacji dla urządzeń przenośnych Azure. Łączność między maszyny wirtualne banku Woodgrove w Azure i lokalnych serwerów są dostarczane przez witryny do witryny (S2S) wirtualnej sieci prywatnej (VPN). S2S VPN umożliwia banku Woodgrove wirtualną sieć Azure wyświetlane jako rozszerzenie sieci lokalnej banku Woodgrove. Ten komunikat jest włączona przez sieć VPN S2S między krawędziami banku Woodgrove i Azure wirtualnej sieci. Teraz Woodgrove chce replikować jego obciążenia działa w jego centrum danych Azure za pomocą automatycznego odzyskiwania systemu. Ta opcja spełnia wymagania Woodgrove, chce ekonomicznych opcję DR i umożliwia przechowywanie danych w chmurze publicznej środowiskach. Woodgrove ma zajmować z aplikacjami i konfiguracji, które zależą od ustalonej adresy IP, w związku z tym mają wymagane do przechowywania adresów IP dla swoich aplikacji po awarii Azure.

Woodgrove postanowił do przypisywania adresów IP z zakresu adresów IP (172.16.1.0/24, 172.16.2.0/24) zasoby uruchomione w Azure.

Woodgrove można było odtworzyć jej maszyn wirtualnych Azure, zachowując adresów IP wirtualną sieć Azure musi być utworzony. Rozszerzenie sieci lokalnej powinno być tak, aby aplikacje mogą bezproblemowo awaryjne z lokalnej witryny Azure. Azure umożliwia dodawanie witryny do witryny, a także punkt do witryny połączenia VPN do wirtualnych sieci utworzone w Azure. Podczas konfigurowania połączenia witryny do witryny, sieci Azure umożliwia skierować ruch do lokalizacji lokalnego (Azure dzwoni go sieci lokalnej) tylko wtedy, gdy zakres adresów IP różni się od lokalnego zakres adresów IP, ponieważ Azure nie obsługuje rozciągając podsieci.  Oznacza to, że jeśli masz 192.168.1.0/24 podsieci lokalnego, nie można dodać 192.168.1.0/24 sieci lokalnej sieci Azure. Oczekiwany jest Azure nie może ustalić, czy nie aktywnego maszyny wirtualne w podsieci i że podsieci jest tworzony tylko do celów DR. Aby można było poprawnie skierować ruch sieciowy poza siecią Azure podsieci w sieci i sieci lokalnej nie może powodować konfliktu. 

![Przed przejęciem awaryjnym podsieci](./media/site-recovery-network-design/network-design7.png)

Przed pracy awaryjnej

Aby pomóc Woodgrove spełnienie wymagań ich firm, trzeba zaimplementować następujące kolejki czynności:

- Tworzenie dodatkowych sieci, Pozwól nam nadaj jej odzyskiwania sieci, gdzie chcesz utworzone maszyn wirtualnych przez nie powiodło się.
- W celu zapewnienia, że adresów IP dla maszyny jest zachowywana po przełączeniu, przejdź na kartę Konfiguruj w obszarze właściwości maszyn wirtualnych Określ samej IP, czy maszyn wirtualnych ma lokalnego, a następnie kliknij przycisk Zapisz. Gdy maszyn wirtualnych to nie powiodło się nad, odzyskiwanie witryny Azure przypisze dostarczonych IP maszyn wirtualnych. 

![Właściwości sieci](./media/site-recovery-network-design/network-design8.png)

Po tym przełączeniu zostanie wywołana i maszyn wirtualnych są tworzone w sieci odzyskiwania z odpowiednim IP, można ustalić łączności z tą siecią za pomocą [Vnet Vnet połączenia](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md). W razie potrzeby tej akcji można tworzone.  Jak wspomniano w poprzedniej sekcji o podsieci powrotu, nawet przełączanie awaryjne do Azure, przekierowuje będą musiały odpowiednio można modyfikować tak, aby odzwierciedlała tego 192.168.1.0/24 ma teraz przeniesione do Azure. 

![Po przełączeniu podsieci](./media/site-recovery-network-design/network-design9.png)

Po przełączeniu

Jeśli nie masz sieci Azure, jak pokazano na powyższym obrazie. Po przełączeniu, możesz utworzyć połączenie vpn witryny między "Podstawowej witryny" i "Sieć odzyskiwania".  


## <a name="option-2-changing-ip-addresses"></a>Opcja 2: Zmienianie adresów IP

Ta metoda prawdopodobnie najbardziej rozpowszechnione według co możemy umieścić. Ma formę zmieniania adresu IP każdej maszyn wirtualnych biorących udział w tym przełączeniu. Zwrot ta metoda wymaga sieci przychodzącego do "więcej" aplikacji, które były w IPx jest teraz na IPy. Nawet jeśli IPx i IPy są nazwy logiczne, wpisy DNS zazwyczaj trzeba zmienić lub opróżniany w całej sieci, zbuforowane wpisy w tabelach sieci muszą zostać zaktualizowane lub opróżniany, przestoje można uznać zależności od tego, jak infrastruktura DNS została skonfigurowana. Tych problemów może być ograniczona przy użyciu najniższe wartości TTL w przypadku aplikacji sieci intranet i [Azure Menedżer ruchu z automatycznego odzyskiwania systemu](http://azure.microsoft.com/blog/2015/03/03/reduce-rto-by-using-azure-traffic-manager-with-azure-site-recovery/) dla aplikacji internetowych zależności

### <a name="changing-the-ip-addresses---illustration"></a>Zmienianie adresów IP — ilustracja

Pozwól nam Przyjrzyj się tego scenariusza miejsce, w którym zamierzasz używać różnych adresy IP przez podstawowy i witryn odzyskiwania. W poniższym przykładzie, możemy również mają innej witryny z tam, gdzie aplikacje hostowana na głównego lub odzyskiwania witryny jest możliwy.

![Różne IP - przed pracy awaryjnej](./media/site-recovery-network-design/network-design10.png)

Rysunek 11

W rysunek 11 istnieją niektóre aplikacje obsługiwany w podsieci 192.168.1.0/24 podsieci w witrynie podstawowego i zostały skonfigurowane do pojawiły się w witrynie odzyskiwania w podsieci 172.16.1.0/24 po przełączeniu. Trasy połączeń sieci VPN zostały skonfigurowane prawidłowo tak, aby wszystkie trzy lokalizacje dostęp do siebie.
 
Jak znaleźć 12 wyświetlane po przełączeniu na jedną lub więcej aplikacji, zostaną przywrócone w podsieci odzyskiwania. W tym przypadku firma Microsoft nie są ograniczone do pracy awaryjnej całą podsieć w tym samym czasie. Żadne zmiany są wymagane o skonfigurowanie trasy VPN lub siecią. Awaryjnego i niektóre aktualizacje DNS będzie upewnij się, że aplikacje są nadal dostępne. Jeśli DNS jest skonfigurowany do zezwalania na aktualizacje dynamiczne maszyn wirtualnych chcesz zarejestrować się za pomocą nowego od po rozpoczęciu po przełączeniu. 

![Różne IP - po przełączeniu](./media/site-recovery-network-design/network-design11.png)

Rysunek 12

Po przełączeniu na replice maszyna wirtualna może być adres IP, który nie jest taki sam, jak adres IP podstawowego maszyny wirtualnej. Maszyn wirtualnych zaktualizuje serwera DNS, które używają po rozpoczęciu. Wpisy DNS zazwyczaj należy zmieniono lub opróżniany w całej sieci, a pamięci podręcznej w tabelach sieci zapisów należy zaktualizować lub opróżniony, aby nie była rzadko wypadek z przestoje, gdy te zmiany stanu miejsce. Ten problem może być ograniczona przez:

- Przy użyciu najniższe wartości TTL dla aplikacji sieci intranet.
- Za pomocą Menedżera ruch Azure automatycznego odzyskiwania systemu dla internet zależności aplikacji.
- Za pomocą tego skryptu w ramach planu odzyskiwania zaktualizować serwer DNS, aby zapewnić czas aktualizacji (skrypt jest wymagane, jeśli skonfigurowano rejestracji dynamicznego DNS)

        string]$Zone,
        [string]$name,
        [string]$IP
        )
        $Record = Get-DnsServerResourceRecord -ZoneName $zone -Name $name
        $newrecord = $record.clone()
        $newrecord.RecordData[0].IPv4Address  =  $IP
        Set-DnsServerResourceRecord -zonename $zone -OldInputObject $record -NewInputObject $Newrecord


### <a name="changing-the-ip-addresses--dr-to-azure"></a>Zmienianie adresów IP — DR Azure

Wpis w blogu [Sieć infrastruktury Instalatora systemu Microsoft Azure witryn odzyskiwania danych](http://azure.microsoft.com/blog/2014/09/04/networking-infrastructure-setup-for-microsoft-azure-as-a-disaster-recovery-site/) wyjaśniono, jak skonfigurować wymagane Azure infrastrukturę sieci przy zachowaniu adresów IP nie jest wymagane. Jego zaczyna się od opisu aplikacji, a następnie w konfiguracji sieci w lokalnej i w Azure, a następnie kończąc sposobów wykonywania awaryjnego test i planowana trybie awaryjnym.



## <a name="next-steps"></a>Następne kroki

[Dowiedz się](site-recovery-network-mapping.md) , jak Odzyskiwanie witryny mapy źródłowej i docelowej sieci gdy serwer VMM jest używany do zarządzania podstawowej witryny. 
