<properties
    pageTitle="Wprowadzenie do Linux platformy Azure | Microsoft Azure"
    description="Informacje o używaniu maszyn wirtualnych Linux Azure."
    services="virtual-machines-linux"
    documentationCenter="python"
    authors="szarkos"
    manager="timlt"
    editor=""
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/24/2016"
    ms.author="szark"/>

#<a name="introduction-to-linux-on-azure"></a>Wprowadzenie do Linux Azure

W tym temacie omówiono niektóre aspekty w chmurze Azure za pomocą maszyn wirtualnych Linux. Wdrażanie maszyny wirtualnej Linux jest proste proces przy użyciu obrazu z galerii.


## <a name="authentication-usernames-passwords-and-ssh-keys"></a>Uwierzytelnianie: Nazwy użytkowników, hasła i klucze SSH

Podczas tworzenia maszyny wirtualnej Linux przy użyciu portalu klasyczny Azure, zostanie wyświetlony monit o podanie nazwy użytkownika, hasło lub kluczem publicznym SSH. Wybór nazwy użytkownika do wdrażania maszyny wirtualnej Linux Azure podlega następujące ograniczenie: nazwy konta systemowe (UID < 100) jeszcze zainstalowane na komputerze wirtualnych nie są dozwolone, "root" na przykład.


 - Zobacz [Tworzenie maszyny wirtualnej systemem Linux](virtual-machines-linux-quick-create-cli.md)
 - Dowiedz się, [jak używać SSH z systemem Linux Azure](virtual-machines-linux-mac-create-ssh-keys.md)


## <a name="obtaining-superuser-privileges-using-sudo"></a>Uzyskiwanie za pomocą uprawnień administratora`sudo`

Konto użytkownika, które zostało określone podczas wdrażania wystąpienie maszyn wirtualnych Azure jest kontem uprzywilejowanych. To konto jest konfigurowany przez agenta Linux Azure, aby można było podniesienia uprawnień do korzystania z głównego (konto administratora) `sudo` narzędzia. Po zalogowaniu się przy użyciu tego konta, będzie można uruchomić polecenia jako główny przy użyciu składni polecenia

    # sudo <COMMAND>

Opcjonalnie można uzyskać powłokę głównego za pomocą **sudo s**.

- Zobacz [Używanie głównego uprawnienia w przypadku maszyn wirtualnych Linux platformy Azure](virtual-machines-linux-use-root-privileges.md)


## <a name="firewall-configuration"></a>Konfiguracja zapory

Azure zawiera filtr pakietów przychodzących ograniczeniem łączności do porty określone w portalu klasyczny Azure. Domyślnie tylko dozwolone port jest SSH. Możesz otworzyć dostęp do dodatkowych portów na komputerze wirtualnych Linux przez skonfigurowanie punkty końcowe w portalu klasyczny Azure:

 - Zobacz: [Jak skonfigurować punkty końcowe maszyn wirtualnych](virtual-machines-windows-classic-setup-endpoints.md)

Obrazy Linux w galerii Azure nie należy włączać zapory *iptables* domyślnie. W razie potrzeby zapory może być skonfigurowany do zapewniają dodatkowe funkcje filtrowania.


## <a name="hostname-changes"></a>Nazwa hosta zmian

Po początkowym wdrożeniu wystąpienie obraz Linux, musisz podać nazwę hosta dla maszyny wirtualnej. Po uruchomieniu maszyny wirtualnej tej nazwy hosta jest opublikowany do serwerów DNS platformy, dzięki czemu wiele maszyn wirtualnych połączone ze sobą mogą wykonywać wyszukiwania adres IP przy użyciu nazwy hostów.

W razie potrzeby zmiany hostname po wdrożeniu maszyny wirtualnej użyj polecenia

    # sudo hostname <newname>

Azure Linux Agent zawiera funkcje, aby automatycznie Wykryj ta zmiana nazwy i odpowiednio skonfigurować maszyny wirtualnej utrzymują tę zmianę i publikowanie tej zmiany do serwerów DNS platformy.

 - [Przewodnik użytkownika Azure agenta Linux](virtual-machines-linux-agent-user-guide.md)

### <a name="cloud-init"></a>Chmura inicjowania
Obrazy **Ubuntu** i **CoreOS** używają inicjowania chmura Azure, który zapewnia dodatkowe funkcje inicjowanie maszyny wirtualnej.

 - [Jak do dodania niestandardowych danych](virtual-machines-windows-classic-inject-custom-data.md)
 - [Niestandardowe dane i inicjowania chmury firmy Microsoft Azure](https://azure.microsoft.com/blog/2014/04/21/custom-data-and-cloud-init-on-windows-azure/)
 - [Tworzenie partycje wymiany Azure za pomocą inicjowania chmury](https://wiki.ubuntu.com/AzureSwapPartitions)
 - [Jak używać CoreOS Azure](https://coreos.com/os/docs/latest/booting-on-azure.html)


## <a name="virtual-machine-image-capture"></a>Przechwytywanie obrazu maszyn wirtualnych

Azure umożliwia przechwytywanie stanu istniejących maszyn wirtualnych do obrazu, który następnie może być używany do wdrożenia wystąpienia dodatkowe maszyn wirtualnych. Azure Linux Agent mogą być używane do przywrócenia niektóre dostosowania, które zostało wykonane podczas inicjowania obsługi administracyjnej. Może być wykonaj poniższe czynności, aby przechwycić maszyny wirtualnej jako obraz:

1. Uruchamianie **waagent-deprovision** Aby cofnąć Dostosowywanie obsługi administracyjnej. Lub **waagent-deprovision + użytkownika** możesz również usunąć konto użytkownika określone podczas inicjowania obsługi administracyjnej i wszystkich skojarzonych z nimi dane.

2. Zamykanie w dół i Wyłącz maszyny wirtualnej.

3. Kliknij pozycję *Rejestrowanie* w portalu klasyczny Azure lub przechwytywanie maszyny wirtualnej jako obraz za pomocą narzędzia programu Powershell lub interfejsu wiersza polecenia.

 - Zobacz: [Jak przechwytywać maszyny wirtualnej Linux ma zostać użyte jako szablonu](virtual-machines-linux-classic-capture-image.md)


## <a name="attaching-disks"></a>Dołączanie dysków

Każda maszyna wirtualna występują tymczasowe, lokalny *dysk zasobu* . Ponieważ dane na dysku zasobów mogą nie być trwałe przez ponownego uruchamiania, często jest używany przez aplikacje i procesów uruchomionych na komputerze wirtualnych dla zmiennych i **tymczasowe** przechowywanie danych. Służy także do przechowywania strony lub Zamień pliki systemu operacyjnego.

W systemie Linux dysk zasobu jest zwykle zarządzane przez agenta Linux Azure i instalowane automatycznie do **/mnt/resource** (lub **katalogu/mnt** na obrazach Ubuntu).


>[AZURE.NOTE] Należy zauważyć, że dysk zasobu jest to dysk **tymczasowe** i może to być usunięte i sformatowany po uruchomieniu maszyn wirtualnych.

W systemie Linux dysku danych może nosić nazwę przez jądro jako `/dev/sdc`, a użytkownicy będą musieli partycje, formatowanie i zainstalować tego zasobu. To jest objęta krok po kroku w samouczku: [sposoby dołączania danych dysku maszyn wirtualnych](virtual-machines-linux-classic-attach-disk.md).

 - **Zobacz też:** [Konfigurowanie oprogramowania RAID w systemie Linux](virtual-machines-linux-configure-raid.md)  &  [Skonfigurować LVM na maszyny Linux platformy Azure](virtual-machines-linux-configure-lvm.md)

