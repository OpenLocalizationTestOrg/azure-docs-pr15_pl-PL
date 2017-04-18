<properties
   pageTitle="Testowanie SAP NetWeaver na pośrednictwem platformy Microsoft Azure SUSE Linux SMS | Microsoft Azure"
   description="Testowanie SAP NetWeaver na maszyny wirtualne Linux SUSE platformy Microsoft Azure"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="hermanndms"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"
   keywords=""/>
<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="09/15/2016"
   ms.author="hermannd"/>

# <a name="running-sap-netweaver-on-microsoft-azure-suse-linux-vms"></a>Bieżąca SAP NetWeaver na maszyny wirtualne Linux SUSE platformy Microsoft Azure


W tym artykule opisano różne kwestie do rozważenia podczas używasz SAP NetWeaver w programie Microsoft Azure SUSE Linux wirtualnych maszyn. Od 2016 19 maja SAP NetWeaver jest oficjalnym wsparciem na pośrednictwem SUSE Linux SMS Azure. Wszystkie szczegóły dotyczące Linux wersji, wersje jądra SAP i tak dalej można znaleźć w 1928533 Uwaga systemu SAP "aplikacji SAP Azure: obsługiwane produkty i typy Azure maszyn wirtualnych".
Dalsze dokumentację dotyczącą SAP na Linux maszyny wirtualne można znaleźć tutaj: [Za pomocą SAP na Linux wirtualnych maszyn](virtual-machines-linux-sap-get-started.md).


Poniższe informacje powinny pomóc w uniknięciu niektórych potencjalnych problemów.

## <a name="suse-images-on-azure-for-running-sap"></a>Obrazy SUSE Azure uruchamiania systemu SAP

Z programem SAP NetWeaver Azure, należy użyć tylko SUSE Linux Enterprise Server SLES 12 (SPx) — patrz również Uwaga systemu SAP 1928533. Specjalne obrazu SUSE jest Azure Marketplace ("SLES 11 z dodatkiem SP3 dla SAP CAL"), ale nie są one przeznaczone do użycia ogólne. Nie należy używać tego obrazu, ponieważ jest ona zarezerwowana rozwiązania [SAP w chmurze urządzenia biblioteki](https://cal.sap.com/) .  

Menedżer zasobów Azure należy używać dla wszystkich nowych badań i instalacji Azure. Aby wyszukać obrazy SUSE SLES i wersje przy użyciu programu PowerShell Azure lub Azure interfejs wiersza polecenia (polecenie), należy użyć następujących poleceń. Następnie służy dane wyjściowe, na przykład, aby zdefiniować obraz systemu operacyjnego w szablonie JSON do wdrażania nowych maszyny SUSE Linux.
Te polecenia programu PowerShell są prawidłowe programu Azure PowerShell 1.0.1 lub nowszy.

* Należy poszukać istniejących wydawców, w tym SUSE:

   ```
   PS  : Get-AzureRmVMImagePublisher -Location "West Europe"  | where-object { $_.publishername -like "*US*"  }
   CLI : azure vm image list-publishers westeurope | grep "US"
   ```

* Należy poszukać istniejących produktów SUSE:

   ```
   PS  : Get-AzureRmVMImageOffer -Location "West Europe" -Publisher "SUSE"
   CLI : azure vm image list-offers westeurope SUSE
   ```

* Należy poszukać SUSE SLES ofertą:

   ```
   PS  : Get-AzureRmVMImageSku -Location "West Europe" -Publisher "SUSE" -Offer "SLES"
   CLI : azure vm image list-skus westeurope SUSE SLES
   ```

* Należy poszukać określonej wersji SLES SKU:

   ```
   PS  : Get-AzureRmVMImage -Location "West Europe" -Publisher "SUSE" -Offer "SLES" -skus "12"
   CLI : azure vm image list westeurope SUSE SLES 12
   ```

## <a name="installing-walinuxagent-in-a-suse-vm"></a>Instalowanie WALinuxAgent w maszyny SUSE

Agent o nazwie WALinuxAgent jest częścią obrazy SLES Azure Marketplace. Aby uzyskać informacji na temat instalowania go ręcznie (na przykład podczas przekazywania SLES OS wirtualnego dysku twardego (wirtualny dysk twardy) z lokalnego) zobacz:

- [OpenSUSE] (http://software.opensuse.org/package/WALinuxAgent)

- [Azure] (wirtualną maszyn — linux — zatwierdzone distros.md)

- [SUSE] (https://www.suse.com/communities/blog/suse-linux-enterprise-server-configuration-for-windows-azure/)

## <a name="sap-enhanced-monitoring"></a>SAP — "monitorowaniu"

SAP "rozszerzony monitorowania" jest obowiązkowe niezbędne do uruchomienia SAP Azure. Sprawdź, czy szczegóły w SAP Uwaga 2191498 "SAP na Linux z Azure: rozszerzony monitorowania".

## <a name="attaching-azure-data-disks-to-an-azure-linux-vm"></a>Dołączanie dyski Azure danych do maszyn wirtualnych Linux Azure

Należy nigdy nie Azure instalacji dyski danych do maszyn wirtualnych Linux Azure za pomocą identyfikatora urządzenia. Zamiast tego użyj unikatowym identyfikatorem (UUID). Za pomocą narzędzi graficznego do instalacji Azure dyski danych, na przykład, należy zachować ostrożność. Sprawdź dokładnie wpisy w /etc/fstab.

Problem z identyfikator urządzenia jest, że mogą ulec zmianie, a następnie maszyn wirtualnych Azure mogą Odłóż podczas uruchamiania. Aby zmniejszyć ten problem, można dodać parametr nofail w /etc/fstab. Ale Uważaj z nofail ponieważ aplikacje mogą używać punktu instalacji jako przed i może zapisać w katalogu głównego systemu plików w przypadku, gdy dysk Azure danych zewnętrznych nie został zainstalowany przy uruchomieniu.

Jedynym wyjątkiem za pomocą UUID jest dołączanie dysk z systemem operacyjnym w celu rozwiązywania problemów, zgodnie z opisem w poniższej sekcji.

## <a name="troubleshooting-a-suse-vm-that-isnt-accessible-anymore"></a>Rozwiązywanie problemów z SUSE maszyn wirtualnych, która nie jest już dostępna

Może być sytuacjach maszyny SUSE Azure zawieszaniem się podczas uruchamiania (na przykład ze względu na błąd związany instalowanie dysków). Ten problem można sprawdzić za pomocą funkcji Diagnostyka uruchamiania dla maszyn wirtualnych Azure w wersji 2 w portalu Azure. Aby uzyskać więcej informacji, zobacz [uruchamiania diagnostyki] (https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/).

Jednym ze sposobów rozwiązania tego problemu jest dysk systemu operacyjnego z uszkodzonego maszyn wirtualnych dołączać do innego SUSE maszyn wirtualnych Azure. Następnie wprowadź odpowiednie zmiany, takie jak edytowanie /etc/fstab lub usuwanie zasad udev sieci, zgodnie z opisem w następnej sekcji.

Istnieje jeden najważniejsze brać pod uwagę. Wdrażanie kilka SUSE maszyny wirtualne z tego samego obrazu Azure Marketplace (na przykład SLES 11 SP4) powoduje dysku systemu operacyjnego, aby zawsze można zainstalować samego UUID. W związku z tym za pomocą UUID dołączyć dysk systemu operacyjnego, z różnych maszyn wirtualnych, który został wdrożony przy użyciu tego samego obrazu Azure Marketplace spowoduje dwóch UUID identyczne. Powoduje problemy, a może oznaczać, że maszyn wirtualnych, przeznaczone do rozwiązywania problemów w rzeczywistości uruchomi się z dysku OS dołączone i uszkodzony, zamiast oryginalnej.

Istnieją dwa sposoby Aby tego uniknąć:

* Używanie innego obrazu Azure Marketplace dla rozwiązywania problemów maszyn wirtualnych (na przykład SLES 11 SPx zamiast SLES 12).
* Nie należy dołączyć uszkodzonego dysku OS z innego maszyn wirtualnych przy użyciu UUID — Użyj coś jeszcze.

## <a name="uploading-a-suse-vm-from-on-premises-to-azure"></a>Przekazywanie maszyny SUSE ze źródeł lokalnych Azure

Aby uzyskać opis czynności, aby przekazać maszyny SUSE ze źródeł lokalnych Azure, zobacz [przygotowywanie maszyny wirtualnej SLES lub openSUSE do Azure] (virtual-machines-linux-suse-create-upload-vhd.md).

Jeśli chcesz przekazać maszyn wirtualnych bez kroku deprovision na końcu (na przykład do Zachowaj istniejącej instalacji SAP, a także nazwa hosta), sprawdź następujące elementy:

* Upewnij się, że dysk systemu operacyjnego jest instalowany za pomocą UUID i nie identyfikator urządzenia. Zmiana UUID tylko w /etc/fstab nie zostanie zwrócony dysku systemu operacyjnego. Ponadto nie zapomnij dostosowania moduł ładujący uruchamiania za pośrednictwem YaST lub edytując /boot/grub/menu.lst.
* Jeśli Użyj formatu VHDX dysku SUSE OS i przekonwertuj ją wirtualnego dysku twardego do przekazywania Azure, jest bardzo prawdopodobne, że urządzenie sieciowe zmieni się z eth0 do eth1. Aby uniknąć problemów, gdy jest uruchamiany Azure później, zmienić z powrotem eth0 zgodnie z opisem w [rozwiązywania problemów eth0 sklonowanym VMware 11 SLES] (https://dartron.wordpress.com/2013/09/27/fixing-eth1-in-cloned-sles-11-vmware/).

Oprócz co opisano w artykule zaleca się, Usuń to:

   /lib/udev/rules.d/75-persistent-NET-Generator.Rules

Można również zainstalować agenta Linux Azure (waagent) pomóc uniknąć potencjalnych problemów, ile są kart.

## <a name="deploying-a-suse-vm-on-azure"></a>Wdrażanie maszyny SUSE Azure

Należy utworzyć nowy SUSE maszyny wirtualne za pomocą plików szablonów JSON w nowy model Azure Menedżera zasobów. Po utworzeniu pliku szablonu JSON można wdrożyć maszyn wirtualnych przy użyciu następującego polecenia interfejsu wiersza polecenia jako alternatywa dla środowiska PowerShell:

   ```
   azure group deployment create "<deployment name>" -g "<resource group name>" --template-file "<../../filename.json>"

   ```
Aby uzyskać więcej informacji na temat plików szablonów JSON Zobacz [szablonów do tworzenia Menedżera zasobów Azure] (zasobów — Grupa tworzenia templates.md) i [Azure Szybki Start szablony] (https://azure.microsoft.com/documentation/templates/).

Aby uzyskać więcej informacji na temat interfejsu wiersza polecenia i Menedżera zasobów Azure Zobacz [używanie polecenie Azure dla komputerów Mac, Linux i systemu Windows przy użyciu Menedżera zasobów Azure] (xplat polecenie azure zasobów manager.md).

## <a name="sap-license-and-hardware-key"></a>Klucz licencji i sprzęt SAP

Oficjalne kwalifikacji SAP Azure nowego mechanizmu została wprowadzona do obliczania klucz sprzętu SAP używaną licencja SAP. Jądra SAP musiał dostosowywane, aby użyć tego. Poprzednie wersje jądra SAP Linux nie zawiera ta zmiana kodu. W związku z tym, w niektórych przypadkach (na przykład maszyn wirtualnych Azure rozmiaru), zmienić klucz sprzętu SAP i licencji SAP został już być nieprawidłowe. To jest obliczany w najnowszej jądra SAP Linux. Aby uzyskać szczegółowe informacje, sprawdź Uwaga systemu SAP 1928533.

## <a name="suse-sapconf-package--tuned-adm"></a>Pakiet sapconf SUSE-dostosowanych adm

SUSE udostępnia pakiet o nazwie "sapconf" zarządzającą zestawu ustawienia specyficzne dla SAP. Aby uzyskać więcej szczegółowych informacji dotyczących działanie tego pakietu oraz jak instalować i używać go, zobacz [za pomocą sapconf przygotowywanie SUSE Linux Enterprise Server do uruchomienia systemów SAP] (https://www.suse.com/communities/blog/using-sapconf-to-prepare-suse-linux-enterprise-server-to-run-sap-systems/) i [co to jest sapconf lub przygotowywania SUSE Linux Enterprise Server do uruchamiania systemów SAP?] (http://scn.sap.com/community/linux/blog/2014/03/31/what-is-sapconf-or-how-to-prepare-a-suse-linux-enterprise-server-for-running-sap-systems).

W międzyczasie jest nowe narzędzie, która zastępuje sapconf - dostosowanych adm. Jeden można znaleźć więcej informacji na temat tego narzędzia po dwóch poniższych łączy.

SLES dokumentację dotyczącą profilu dostosowanych adm sap — hana można znaleźć [w tym miejscu](https://www.suse.com/documentation/sles-for-sap-12/book_s4s/data/sec_s4s_configure_sapconf.html) 

Określania systemów SAP obciążenie pracą z dostosowanych adm — można znaleźć [tutaj](https://www.suse.com/documentation/sles-for-sap-12/pdfdoc/book_s4s/book_s4s.pdf) w rozdziału 6.2


## <a name="nfs-share-in-distributed-sap-installations"></a>Udostępnianie NFS w instalacjach rozłożone SAP

Jeśli masz instalacji rozproszonej — na przykład miejsce, w którym chcesz zainstalować bazę danych i serwery aplikacji SAP w osobnym maszyny wirtualne — możesz udostępnić katalogu /sapmnt za pośrednictwem sieciowego systemu plików (NFS). Jeśli masz problemy z instrukcjami instalacji po utworzeniu udziału NFS dla /sapmnt, sprawdź, czy "no_root_squash" jest ustawiona dla udziału.

## <a name="logical-volumes"></a>Wielkości logicznych.

W przeszłości ile wymagane dużej logiczne na wielu dyskach Azure danych (na przykład w przypadku systemu SAP bazy danych), została zalecane lvm w pełni nie został jeszcze zatwierdzonych Azure za pomocą mdadm. Aby dowiedzieć się, jak skonfigurować RAID Linux Azure za pomocą mdadm, zobacz [Konfigurowanie oprogramowania RAID w systemie Linux](virtual-machines-linux-configure-raid.md). W tym samym czasie od początku maja 2016 również lvm jest w pełni obsługiwany Azure i może być używany jako alternatywa dla mdadm. Aby uzyskać dodatkowe informacje dotyczące lvm Azure, zobacz [Konfigurowanie LVM na maszyny Linux platformy Azure](virtual-machines-linux-configure-lvm.md).


## <a name="azure-suse-repository"></a>Azure repozytorium SUSE

Jeśli masz problem z dostępem do standardowego repozytorium Azure SUSE umożliwia simple polecenie je zresetować. Może się to zdarzyć, jeśli tworzenie prywatnych obraz systemu operacyjnego w obszarze Azure, a następnie skopiować obraz do innego obszaru, w którym chcesz wdrożyć nowe maszyny wirtualne opartych na tym prywatne obrazem systemu operacyjnego. Po prostu uruchom następujące polecenie wewnątrz maszyn wirtualnych:

   ```
   service guestregister restart
   ```

## <a name="gnome-desktop"></a>Gnome pulpitu

Jeśli chcesz korzystać z pulpitu Gnome, aby zainstalować pełną system pokaz SAP w jednym maszyn wirtualnych — w tym Graficznym SAP przeglądarki i systemu SAP management console — umożliwia wskazówka zainstalować go na obrazy Azure SLES:

   Aby uzyskać SLES 11:

   ```
   zypper in -t pattern gnome
   ```

   Aby uzyskać SLES 12:

   ```
   zypper in -t pattern gnome-basic
   ```

## <a name="sap-support-for-oracle-on-linux-in-the-cloud"></a>SAP — Obsługa Oracle w systemie Linux w chmurze

Istnieje ograniczenie pomocy technicznej z programu Oracle w systemie Linux w środowisku wirtualizowanym. Mimo że nie jest temat specyficzne dla Azure, należy zapoznać się z. SAP — nie obsługuje Oracle na SUSE lub czerwony funkcję w chmurze publiczne, takie jak Azure. Aby omówić w tym temacie, skontaktuj się bezpośrednio z programu Oracle.
