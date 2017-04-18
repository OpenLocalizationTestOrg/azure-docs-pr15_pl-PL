 <properties
   pageTitle="Azure i Linux | Microsoft Azure"
   description="W tym artykule opisano usługi Azure obliczenia, miejsca do magazynowania i sieci przy użyciu maszyn wirtualnych Linux."
   services="virtual-machines-linux"
   documentationCenter="virtual-machines-linux"
   authors="vlivech"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="09/14/2016"
   ms.author="v-livech"/>

# <a name="azure-and-linux"></a>Azure i Linux

Microsoft Azure to zbiór rosnących zintegrowane publicznej usług, w tym z baz danych analizy, maszyn wirtualnych, urządzeń przenośnych, sieci, miejscem do magazynowania, w chmurze oraz w sieci web — idealne rozwiązanie w przypadku hostingu rozwiązanie.  Microsoft Azure zawiera skalowalna platforma przetwarzania, która umożliwia tylko zapłacić za możesz używać, gdy ma — bez konieczności inwestycji w lokalnym sprzętu.  Azure jest gotowa, gdy rozbudowy rozwiązanie się niezależnie od skali jest wymagane do obsługi potrzeb klientów.

Jeśli znasz różne funkcje firmy Amazon AWS, można sprawdzić AWS Azure w porównaniu z [definicji mapowania dokumentu](https://azure.microsoft.com/campaigns/azure-vs-aws/mapping/).


## <a name="regions"></a>Obszary
Zasoby Microsoft Azure są rozmieszczane wielu regionach geograficznych na świecie.  "Obszar" oznacza wielu centrach danych w jednym obszarze geograficznym.  Od 1 stycznia 2016 obejmuje: 8 w Ameryce, 2 w Europie, 6 w części Azji, 2 w Chinach kontynent i 3 Indie.  Jeśli chcesz, aby uzyskać pełną listę wszystkich regionów Azure, możemy Obsługa listę istniejących oraz nowo ogłaszane regionów.

- [Regiony Azure](https://azure.microsoft.com/regions/)

## <a name="availability"></a>Dostępność
Aby wdrożenia kwalifikować się do naszych 99.95 maszyn wirtualnych umowy o poziomie usług należy zainstalować dwie lub ustaw więcej maszyny wirtualne działa z pracą umieszczona dostępności. Dzięki pośrednictwem usługi SMS są rozmieszczane wiele domen błędów w naszym centrach danych, a także rozmieszczane hosts z różnych konserwacja systemu windows. Pełna [Umowa dotycząca poziomu usług Azure](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_0/) wyjaśniono gwarantowanej dostępność Azure jako całość.

## <a name="azure-virtual-machines--instances"></a>Azure maszyn wirtualnych i wystąpienia
Microsoft Azure obsługuje wiele popularnych dystrybucji Linux dostarczane i obsługiwane przez liczbę partnerów.  Znajdziesz dystrybucji, takich jak Enterprise funkcję czerwony, CentOS Debian, Ubuntu, CoreOS, RancherOS, FreeBSD i innych funkcji w Azure Marketplace. Firma Microsoft aktywnie pracować z różnych społeczności Linux, aby dodać jeszcze więcej podtypy do listy [Azure zatwierdzone Linux Distros](virtual-machines-linux-endorsed-distros.md) .

Jeśli swojej preferowanej distro Linux wybranym nie ma obecnie w galerii, możesz "zabrać ze sobą własne Linux" maszyn wirtualnych przez [Tworzenie i przekazywanie Linux wirtualnego dysku twardego platformy Azure](virtual-machines-linux-create-upload-generic.md).

Azure maszyn wirtualnych umożliwiają wdrażanie szeroką gamę obliczeniowych rozwiązań w sposób adaptacyjne. Możesz wdrożyć praktycznie dowolnych obciążenie pracą i dowolny język na niemal dowolnym systemu operacyjnego — systemu Windows, Linux, albo niestandardowy utworzony z jednego z naszych rosnących listę partnerów. Nadal nie widać, czego szukasz?  Nie martw się — możesz też zabrać ze sobą własnych obrazów ze źródeł lokalnych.

## <a name="vm-sizes"></a>Rozmiary maszyn wirtualnych
Podczas wdrażania maszyn wirtualnych w Azure zamierzasz wybierz rozmiar pamięci Wirtualnej w jednej z naszych szereg rozmiarów nadaje się do usługi Obciążenie pracą. Rozmiar wpływa również przetwarzanie power, pamięci i przechowywania możliwości maszyny wirtualnej. Konta oparte na czas maszyn wirtualnych jest uruchomiony i używające jej przydzielone zasoby. Pełną listę [rozmiarów maszyn wirtualnych](virtual-machines-linux-sizes.md).

Poniżej przedstawiono podstawowe wskazówki dotyczące wybierając jeden z naszych serii (A, D, DS, G i GS) rozmiar pamięci Wirtualnej.

* Maszyny wirtualne A serii są naszych wartość cenach podstawowej maszyny wirtualne uproszczonej obciążenia i scenariuszy deweloperów/testowania. Są powszechnie dostępny we wszystkich regionach i mogą połączyć i używać wszystkich standardowych zasobów dostępnych dla maszyn wirtualnych.
* A serii (A8 - A11) o rozmiarach obliczeń specjalnych intensywnie konfiguracji odpowiednie dla aplikacji klaster komputerowe wysokiej wydajności.
* Maszyny wirtualne serii są przeznaczone do uruchamiania aplikacji, które wymagają dysku tymczasowym wydajność i nowszy power obliczeń. Maszyny wirtualne serii D zapewniają szybsze procesory, wyższy stosunek pamięci do podstawowych i dyskiem (SSD) na dysku tymczasowym.
* Dv2 serii jest najnowsza wersja naszych serii D, Procesora bardziej zaawansowanych funkcji. Procesor Dv2 serii jest około 35% krócej niż Procesora serii. Jest oparty na najnowszej generacji 2,4 GHz Intel Xeon® E5-2673 v3 procesor (Haskell) i 2.0 technologii zwiększenie wydajności Turbo firmy Intel, można przejść do 3,2 GHz. Seria Dv2 ma sam konfiguracji pamięci i na dysku jako serii D.
* Maszyny wirtualne serii G oferują najwięcej pamięci i uruchamiać na hostów, które mają rodziny procesorów firmy Intel Xeon E5 V3.

Uwaga: Zasadami serii i maszyny wirtualne serii GS mają dostęp do magazynowania Premium — naszych SSD kopii wysokiej wydajności i małych opóźnień miejsca do magazynowania dla intensywnie obciążenia We/Wy. Magazyn Premium jest dostępna w niektórych regionach. Aby uzyskać szczegółowe informacje Zobacz:

- [Przechowywanie Premium: Wysokiej wydajności miejsca do magazynowania dla obciążenia Azure maszyn wirtualnych](../storage/storage-premium-storage.md)

## <a name="automation"></a>Automatyzacji
Aby osiągnąć kulturą DevOps pisane z wielkiej litery, wszystkie infrastruktury musi być kodem.  Gdy wszystkie infrastruktury znajduje się w kodzie, które mogą być łatwo odtworzyć (serwery Phoenix).  Azure działa z głównych automatyzacji, narzędzie, takich jak Ansible, Kuchmistrz, SaltStack i Puppet.  Azure też ma własny narzędzia automatyzacji:

- [Szablony Azure](virtual-machines-linux-create-ssh-secured-vm-from-template.md)

- [Azure VMAccess](virtual-machines-linux-using-vmaccess-extension.md)

Azure jest wdrażania obsługę [chmury inicjowania](http://cloud-init.io/) przez większość Linux Distros, która obsługuje go.  Obecnie maszyny wirtualne Ubuntu Canonical osoby są wdrażane z chmury inicjowania domyślnie włączone.  RedHats RHEL, CentOS i Fedora obsługi chmury inicjowania, jednak obrazy Azure obsługiwany przez RedHat nie jest zainstalowany inicjowania chmury.  Aby użyć inicjowania chmury rodziny RedHat systemu operacyjnego, musisz utworzyć obraz niestandardowy ze zainstalowany inicjowania chmury.

- [Za pomocą inicjowania chmury na maszyny wirtualne Linux Azure](virtual-machines-linux-using-cloud-init.md)

## <a name="quotas"></a>Przydziałów
Każdej subskrypcji Azure ma domyślnych ograniczeń zasobów w miejsce, w którym mogą mieć wpływ wdrażania dużej liczby maszyny wirtualne projektu. Limit bieżącego na podstawie subskrypcji na to 20 maszyny wirtualne w rozbiciu na regiony.  Można powstałe limitów przydziału miejsca przechowywania żąda Zwiększ limit bilet pomocy technicznej.  Aby uzyskać więcej informacji na temat limitów przydziału:

- [Limity dotyczące usługi Azure subskrypcji](../azure-subscription-service-limits.md)


## <a name="partners"></a>Partnerzy

Firma Microsoft ściśle współpracuje z partnerami zapewnienie obrazów dostępnych zostały zaktualizowane i zoptymalizowany pod kątem obsługi Azure.  Aby uzyskać więcej informacji na temat partnerów Sprawdź stronach witryny marketplace poniżej.

- [Linux na zatwierdzone Azure dystrybucji](virtual-machines-linux-endorsed-distros.md)

RedHat - [Azure Marketplace - RedHat Enterprise Linux 7.2](https://azure.microsoft.com/marketplace/partners/redhat/redhatenterpriselinux72/)

Kanoniczny - [Azure Marketplace - Ubuntu serwera 16.04 KÓW](https://azure.microsoft.com/marketplace/partners/canonical/ubuntuserver1604lts/)

Debian - [Azure Marketplace - 8 Debian "Joasia"](https://azure.microsoft.com/marketplace/partners/credativ/debian8/)

FreeBSD - [Azure Marketplace - FreeBSD 10.3](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd103/)

CoreOS - [Azure Marketplace - CoreOS (stały)](https://azure.microsoft.com/marketplace/partners/coreos/coreosstable/)

RancherOS - [Azure Marketplace - RancherOS](https://azure.microsoft.com/marketplace/partners/rancher/rancheros/)

Bitnami - [Bitnami biblioteki Azure](https://azure.bitnami.com/)

Mezosfery - [Azure Marketplace - mezosfery kontrolera domeny i OS Azure](https://azure.microsoft.com/marketplace/partners/mesosphere/dcosdcos/)

Docker - [Azure Marketplace — usługa Azure kontenera z rój Docker](https://azure.microsoft.com/marketplace/partners/microsoft/acsswarms/)

Jenkins - [Azure Marketplace - platformy CloudBees Jenkins](https://azure.microsoft.com/marketplace/partners/cloudbees/jenkins-platformjenkins-platform/)


## <a name="getting-setup-on-azure"></a>Uzyskiwanie konfiguracji Azure
Aby rozpocząć korzystanie z platformy Azure należy konto Azure, polecenie Azure zainstalowany i para kluczy publicznych i prywatnych SSH.

## <a name="sign-up-for-an-account"></a>Załóż konto
Pierwszym krokiem w korzystaniu z chmurą Azure jest Załóż konto Azure.  Przejdź do strony [Tworzenia konta konto Azure](https://azure.microsoft.com/pricing/free-trial/) , aby rozpocząć pracę.

## <a name="install-the-cli"></a>Instalowanie interfejsu wiersza polecenia
Przy użyciu nowego konta Azure można rozpocząć natychmiast za pomocą portalu Azure, czyli panelu administrator oparte na sieci web.  Aby zarządzać chmura Azure za pomocą wiersza polecenia, należy zainstalować `azure-cli`.  Zainstaluj [Polecenie Azure ](../xplat-cli-install.md)z systemem Mac lub Linux.

## <a name="create-an-ssh-key-pair"></a>Tworzenie parę kluczy SSH
Teraz masz konto Azure, Azure portalu i polecenie Azure.  Następnym krokiem jest utworzenie pary kluczy SSH, który jest używany do SSH do Linux bez użycia hasła.  [Tworzenie SSH klawiszy Linux i Mac](virtual-machines-linux-mac-create-ssh-keys.md) umożliwiające mniej hasło logowania i większe bezpieczeństwo.

## <a name="getting-started-with-linux-on-microsoft-azure"></a>Wprowadzenie do programu Linux na platformy Microsoft Azure
Przy użyciu ustawień konto Azure, polecenie Azure zainstalowany i kluczy SSH utworzonych zechcesz teraz rozpocząć tworzenie infrastruktury w chmurze Azure.  Pierwsze zadanie jest utworzenie kilka maszyny wirtualne.

## <a name="create-a-vm-using-the-cli"></a>Tworzenie maszyn wirtualnych za pomocą interfejsu wiersza polecenia
Tworzenie maszyny Linux za pomocą interfejsu wiersza polecenia jest szybkim sposobem wdrażanie maszyny bez opuszczania terminal pracujesz w.  Wszystkie obiekty, które można określić w portalu sieci web jest dostępna za pośrednictwem wiersza polecenia flagi lub przełącznika.  

- [Tworzenie maszyny Linux za pomocą interfejsu wiersza polecenia](virtual-machines-linux-quick-create-cli.md)

## <a name="create-a-vm-in-the-portal"></a>Tworzenie maszyn wirtualnych w portalu
Tworzenie maszyny Linux w portalu Azure sieci web jest umożliwia łatwe punktu i klikać różne opcje, aby przejść do wdrożenia.  Zamiast używać wiersza polecenia flag lub przełączników, jest możliwość wyświetlania układu sieci web i różnych opcji i ustawień.  Wszystkie dostępne za pośrednictwem interfejsu wiersza polecenia jest również dostępne w portalu.

- [Tworzenie maszyny Linux za pomocą portalu](virtual-machines-linux-quick-create-portal.md)

## <a name="login-using-ssh-without-a-password"></a>Zaloguj się przy użyciu SSH bez użycia hasła
Maszyn wirtualnych działa teraz Azure i możesz przystąpić do logowania.  Zaloguj się przy użyciu SSH za pomocą hasła jest niebezpiecznej i czasochłonne.  Używanie klawiszy SSH jest najbardziej bezpieczny sposób, a także najszybszym sposobem logowania.  Po utworzeniu możesz Linux maszyn wirtualnych za pośrednictwem portalu lub polecenie, masz dwie opcje uwierzytelniania.  Jeśli wybierzesz hasła dla SSH Azure konfiguruje maszyn wirtualnych, aby umożliwić logowania przy użyciu hasła.  Jeśli zdecydujesz się korzystać z kluczem publicznym SSH, Azure konfiguruje maszyn wirtualnych umożliwia tylko logowania przy użyciu klawiszy SSH i wyłącza hasło logowania. Aby zabezpieczyć maszyn wirtualnych usługi Linux, umożliwiając tylko SSH klucza logowania, opcja SSH publicznej klucza podczas tworzenia maszyn wirtualnych w portalu lub interfejsu wiersza polecenia.

- [Wyłączanie hasła SSH maszyn wirtualnych usługi Linux przez skonfigurowanie SSHD](virtual-machines-linux-mac-disable-ssh-password-usage.md)

## <a name="related-azure-components"></a>Składniki Azure pokrewne

## <a name="storage"></a>Miejsca do magazynowania

- [Wprowadzenie do magazynu platformy Microsoft Azure](../storage/storage-introduction.md)

- [Dodanie dysku do maszyny Linux przy użyciu polecenie azure](virtual-machines-linux-add-disk.md)

- [Dołączanie dysku danych do maszyny Linux w portalu Azure](virtual-machines-linux-attach-disk-portal.md)

## <a name="networking"></a>Sieci

- [Omówienie wirtualnych sieci](../virtual-network/virtual-networks-overview.md)

- [Adresy IP platformy Azure](../virtual-network/virtual-network-ip-addresses-overview-arm.md)

- [Otwieranie portów do maszyny Linux platformy Azure](virtual-machines-linux-nsg-quickstart.md)

- [Tworzenie w pełni kwalifikowaną nazwę domeny w Azure portal](virtual-machines-linux-portal-create-fqdn.md)


## <a name="containers"></a>Kontenery

- [Maszyn wirtualnych i kontenerów platformy Azure](virtual-machines-linux-containers.md)

- [Wprowadzenie kontenera usługi Azure](../container-service/container-service-intro.md)

- [Wdrażanie klastrze Usługa Azure kontenera](../container-service/container-service-deployment.md)

## <a name="next-steps"></a>Następne kroki

Teraz masz omówienie Linux Azure.  Następnym krokiem jest z usługi utworzyć kilka maszyny wirtualne.

- [Tworzenie maszyny Linux Azure za pomocą portalu](virtual-machines-linux-quick-create-portal.md)

- [Tworzenie maszyny Linux Azure za pomocą interfejsu wiersza polecenia](virtual-machines-linux-quick-create-cli.md)
