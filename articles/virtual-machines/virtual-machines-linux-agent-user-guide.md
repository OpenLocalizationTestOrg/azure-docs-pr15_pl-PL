<properties 
    pageTitle="Przewodnik użytkownika agenta Linux | Microsoft Azure" 
    description="Dowiedz się, jak zainstalować i skonfigurować agenta Linux (waagent) do zarządzania komputera wirtualnych interakcji z kontroler tkaninie Azure." 
    services="virtual-machines-linux" 
    documentationCenter="" 
    authors="szarkos" 
    manager="timlt" 
    editor=""
    tags="azure-service-management,azure-resource-manager" />

<tags 
    ms.service="virtual-machines-linux" 
    ms.workload="infrastructure-services" 
    ms.tgt_pltfrm="vm-linux" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/17/2016" 
    ms.author="szark"/>



# <a name="azure-linux-agent-user-guide"></a>Przewodnik użytkownika Azure agenta Linux

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="introduction"></a>Wprowadzenie

Program Microsoft Azure Linux Agent (waagent) zarządza Linux i FreeBSD inicjowania obsługi administracyjnej i interakcji maszyn wirtualnych za pomocą sterownika tkaninie Azure. Udostępnia następujące funkcje do Linux i FreeBSD IaaS wdrożenia:

> [AZURE.NOTE] Zobacz agent Azure Linux [pliku README](https://github.com/Azure/WALinuxAgent/blob/master/README.md) , aby uzyskać dodatkowe informacje.

* **Obraz inicjowania obsługi administracyjnej.**
  - Tworzenie konta użytkownika
  - Konfigurowanie typów uwierzytelniania SSH
  - Wdrożenie programu SSH klucze publiczne i pary kluczy
  - Ustawienie Nazwa hosta
  - Publikowanie nazwa hosta DNS platformy
  - Raportowanie odcisku klucza hosta SSH platformy
  - Zarządzanie zasobami dysku
  - Formatowanie i instalowania dysku zasobu
  - Konfigurowanie obszar wymiany

* **Sieci**
  - Zarządza trasy do poprawianie zgodności z serwerami DHCP platformy
  - Zapewnia stabilność Nazwa interfejsu sieci

* **Jądra**
  - Konfiguruje NUMA wirtualnych (wyłączanie dla jądra < 2.6.37)
  - Korzystanie z funkcji Hyper-V entropii dla /dev/random
  - Konfiguruje SCSI przekroczenia limitu czasu dla urządzenia katalogu głównego (który może być zdalnego)

* **Narzędzia diagnostyczne**
  - Przekierowywanie konsoli do portu szeregowego

* **SCVMM wdrożenia**
    - Wykrywa i używa do ładowania agenta VMM Linux podczas uruchamiania w środowisku systemu Centrum Menedżer maszyn wirtualnych 2012 R2

* **Rozszerzenie maszyn wirtualnych**
    - Wprowadzić składnik utworzony przez firmę Microsoft i partnerów do maszyn wirtualnych Linux (IaaS) o ponownym włączeniu oprogramowania i konfiguracji automatyzacji
    - Rozszerzenie maszyn wirtualnych implementacji na [https://github.com/Azure/azure-linux-extensions](https://github.com/Azure/azure-linux-extensions)


## <a name="communication"></a>Komunikacji
Przepływu informacji z platformy do agenta odbywa się na dwóch kanałów:

* Czas uruchamiania dołączone DVD w przypadku wdrożeń IaaS. Ten dysk DVD zawiera zgodne z OVF pliku konfiguracji, który zawiera wszystkie informacje obsługi administracyjnej niż rzeczywisty keypairs SSH.
* Punkt końcowy TCP Uwidacznianie interfejsu API usługi REST używana do uzyskiwania wdrażanie i Konfiguracja topologii.


## <a name="requirements"></a>Wymagania dotyczące
Poniższe systemy zostały przetestowane i są znane do pracy z agentem Linux Azure:

> [AZURE.NOTE] Przedstawione w tym tej listy mogą różnić się od oficjalnym Lista obsługiwanych systemów na platformie Azure firmy Microsoft: [http://support.microsoft.com/kb/2805216](http://support.microsoft.com/kb/2805216)

* CoreOS
* CentOS 6.3 +
* Czerwone funkcję Enterprise Linux 6,7 +
* Debian 7.0 +
* Ubuntu 12.04 +
* openSUSE 12.3 +
* SLES 11 Z DODATKIEM SP3 +
* Oracle Linux 6.4 +

Inne obsługiwane systemy:

* FreeBSD 10 + (Azure agenta Linux v2.0.10 +)

Linux agent zależy od niektóre pakiety systemu w celu poprawnego:

* Python 2.6 +
* OpenSSL 1.0 +
* OpenSSH 5.3 +
* Narzędzia plików: ulec sfdisk fdisk, mkfs,
* Narzędzia hasło: chpasswd, sudo
* Narzędzia do edycji tekstu: sed, grep
* Sieci narzędzia: trasę ip
* Jądra obsługę instalowanie UDF filesystems.

## <a name="installation"></a>Instalacji
Instalacja za pomocą obrotów na MINUTĘ lub pakiet DEB z repozytorium pakietu do dystrybucji jest preferowana metoda Instalowanie i uaktualnianie agenta Linux Azure. Wszystkie [zatwierdzone dostawców rozkładu](virtual-machines-linux-endorsed-distros.md) integrowanie pakietu agenta Azure Linux ich obrazów i repozytoria.

Zapoznaj się z dokumentacją w [repo Azure Linux agenta na Github](https://github.com/Azure/WALinuxAgent) dla zaawansowane opcje instalacji, takich jak instalowanie ze źródła lub niestandardowych lokalizacjach lub prefiksów.


## <a name="command-line-options"></a>Opcje wiersza polecenia

### <a name="flags"></a>Flagi

- pełne: Zwiększanie szczegółowości określonego polecenia
- Wymuszanie: Pomiń interakcyjnych potwierdzenie dla niektórych poleceń

### <a name="commands"></a>Polecenia

- Pomoc: Lista obsługiwanych poleceń i flagi.

- deprovision: próbować oczyścić system, a następnie wprowadź odpowiednie do ponownego inicjowania obsługi administracyjnej. Operacja usunięte następujące czynności:
 * Wszystkie klucze hosta SSH (jeśli Provisioning.RegenerateSshHostKeyPair jest "y" w pliku konfiguracji)
 * Konfiguracja serwerów nazw w /etc/resolv.conf
 * Hasła root z /etc/shadow (jeśli Provisioning.DeleteRootPassword jest "y" w pliku konfiguracji)
 * Tryb buforowanej dzierżawy klientów DHCP
 * Nazwa hosta resetuje do localhost.localdomain


> [AZURE.WARNING] Cofanie ubezpieczeń nie gwarantuje, że obraz jest wyczyszczone wszystkich informacji poufnych i odpowiednie w celu ponownej dystrybucji.


- deprovision + użytkownika: wykonuje wszystko w obszarze - deprovision (powyżej) i również usuwa starego konta użytkownika ustanawianie (uzyskanego z /var/lib/waagent) i skojarzone danych. Ten parametr jest podczas usuwania inicjowania obsługi administracyjnej obrazu, który został wcześniej inicjowania obsługi administracyjnej Azure, może być przechwycone i ponownego użycia.

- Wersja: Wyświetla wersję waagent

- serialconsole: konfiguruje CHODNIKÓW, aby oznaczyć ttyS0 (pierwszy portu szeregowego) jako konsolę uruchamiania. Dzięki temu dzienniki uruchamiania jądra są wysyłane do portu szeregowego, a udostępnione do debugowania.

- demon: uruchamianie waagent jako demon Zarządzanie interakcji z platformy. Ten argument jest określony do waagent w obszarze skrypt inicjowania waagent.

- Rozpoczynanie: uruchamianie waagent w tle


## <a name="configuration"></a>Konfiguracja

Plik konfiguracyjny (-etc/waagent.conf) Określa akcje waagent. Poniżej przedstawiono przykładowy plik konfiguracyjny:

    Provisioning.Enabled=y
    Provisioning.DeleteRootPassword=n
    Provisioning.RegenerateSshHostKeyPair=y
    Provisioning.SshHostKeyPairType=rsa
    Provisioning.MonitorHostName=y
    Provisioning.DecodeCustomData=n
    Provisioning.ExecuteCustomData=n
    Provisioning.PasswordCryptId=6
    Provisioning.PasswordCryptSaltLength=10
    ResourceDisk.Format=y
    ResourceDisk.Filesystem=ext4
    ResourceDisk.MountPoint=/mnt/resource
    ResourceDisk.MountOptions=None
    ResourceDisk.EnableSwap=n
    ResourceDisk.SwapSizeMB=0
    LBProbeResponder=y
    Logs.Verbose=n
    OS.RootDeviceScsiTimeout=300
    OS.OpensslPath=None
    HttpProxy.Host=None
    HttpProxy.Port=None

Szczegóły poniżej opisano różne opcje konfiguracji. Opcje konfiguracji są trzy typy; Wartość logiczna ciąg lub liczba całkowita. Opcje konfiguracji logicznych można określić jako "y" lub "n". Specjalne słowo kluczowe "Brak" mogą być używane do niektórych ciąg typu wpisy konfiguracji wyszczególnione poniżej.

**Provisioning.Enabled:**  
Typ: logiczna  
Domyślne: y

Umożliwia użytkownikowi włączyć lub wyłączyć funkcję obsługi administracyjnej w agenta. Prawidłowe wartości to "y" lub "n". Jeśli obsługa administracyjna zostanie wyłączona, SSH klawiszy hosta i użytkownika na obrazie są zachowywane, a żadnej konfiguracji określonego w Azure inicjowania obsługi administracyjnej interfejsu API jest ignorowana.

> [AZURE.NOTE] `Provisioning.Enabled` Jest domyślnie "n" w przypadku obrazów chmury Ubuntu przez chmury inicjowania obsługi administracyjnej.

  
**Provisioning.DeleteRootPassword:**  
Typ: logiczna  
Domyślne: n

Jeśli zestaw, hasła głównego w pliku cienia/itp/jest usunięte podczas inicjowania obsługi administracyjnej.


**Provisioning.RegenerateSshHostKeyPair:**  
Typ: logiczna  
Domyślne: y

Jeśli podczas inicjowania obsługi administracyjnej z/etc/ssh/zostaną usunięte zestaw, wszystkie SSH hosta pary kluczy (ecdsa, dsa i rsa). I generowania pojedynczy świeży pary kluczy.

Typ szyfrowania dla świeży pary kluczy jest konfigurowany przez wpis Provisioning.SshHostKeyPairType. Pamiętaj, że niektóre dystrybucji ponownie utworzy SSH pary kluczy dla wszystkich typów brak szyfrowania po uruchomieniu demona SSH (na przykład po ponownym uruchomieniu).


**Provisioning.SshHostKeyPairType:**  
Typ: ciąg  
Domyślne: rsa

Może być równa typu algorytmu szyfrowania, obsługiwany przez demona SSH na komputerze wirtualnych. Zazwyczaj obsługiwane wartości to "rsa", "dsa" i "ecdsa". Należy zauważyć, że "putty.exe" w systemie Windows nie obsługuje "ecdsa". Tak Jeśli zamierzasz użyć putty.exe w systemie Windows, aby nawiązać połączenie z wdrożeniem Linux, użyj "rsa" lub "dsa".


**Provisioning.MonitorHostName:**  
Typ: logiczna  
Domyślne: y

Jeśli zestawu waagent będzie monitorować maszyna wirtualna Linux oraz dla zmiany hostname (zwracane przez polecenie "hostname") i automatycznie aktualizuje sieciową konfiguracji obrazu w celu odzwierciedlenia zmian. Aby przekazać zmiany nazwy do serwerów DNS, sieci zostanie uruchomiony ponownie maszyny wirtualnej. Spowoduje to w skrócie utratę łączność z Internetem.


**Provisioning.DecodeCustomData**  
Typ: logiczna  
Domyślne: n

Jeśli zostanie ustawiona, waagent dekodowanie CustomData z Base64.


**Provisioning.ExecuteCustomData**  
Typ: logiczna  
Domyślne: n

Jeśli skonfigurowano, waagent będzie wykonywać CustomData po zainicjowaniu obsługi administracyjnej.


**Provisioning.PasswordCryptId**  
Typ: String  
Domyślne: 6

Algorytm używane przez kryptograficznego podczas generowania skrótu hasła.  
 1 - MD5  
 2a - Blowfish  
 5 - AGENT KONDYCJI SYSTEMU 256  
 6 - AGENT KONDYCJI SYSTEMU 512  


**Provisioning.PasswordCryptSaltLength**  
Typ: String  
Domyślne: 10

Długość losowe salt używana podczas generowania skrótu hasła.


**ResourceDisk.Format:**  
Typ: logiczna  
Domyślne: y

Jeśli skonfigurowano, dysk zasobu dostarczone przez platformę jest formatowany i zainstalowanych przez waagent, jeśli typ systemu plików, zlecone przez użytkownika w "ResourceDisk.Filesystem" jest inny niż "ntfs". Jedną partycją typu Linux (83) będą dostępne na dysku. Należy zauważyć, że ten partition nie mają zostać sformatowane, jeśli będzie można ją zainstalować pomyślnie.


**ResourceDisk.Filesystem:**  
Typ: ciąg  
Domyślne: ext4

Określa typ systemu plików dla dysku zasobu. Obsługiwane wartości zależy od rozkładu Linux. Jeśli ciąg jest X, a następnie mkfs. X powinny znajdować się na obrazie Linux. Obrazy SLES 11 zwykle należy użyć "ext3". Obrazy FreeBSD należy używać "ufs2" poniżej.


**ResourceDisk.MountPoint:**  
Typ: ciąg  
Wartość domyślna: / mnt/zasobów 

Określa ścieżkę, w którym jest zainstalowany na dysku zasobu. Uwaga dysk zasobu jest dyskiem *tymczasowe* i może zostać opróżniony, gdy wstrzymano obsługę administracyjną maszyn wirtualnych.


**ResourceDisk.MountOptions**  
Typ: ciąg  
Domyślne: Brak

Określa opcje instalacji dysku, które zostaną przekazane do polecenia -o instalacji. To jest lista rozdzielanych przecinkami wartości. "nodev, nosuid". Zobacz mount(8), aby uzyskać szczegółowe informacje.


**ResourceDisk.EnableSwap:**  
Typ: logiczna  
Domyślne: n

Jeśli skonfigurowano, plik wymiany (-swapfile) jest tworzony na dysku zasobów i dodawany do obszaru wymiany systemu.


**ResourceDisk.SwapSizeMB:**  
Typ: liczba całkowita  
Domyślny: 0

Rozmiar pliku wymiany w megabajtach.


**Logs.Verbose:**  
Typ: logiczna  
Domyślne: n

Jeśli zestaw, szczegółowości dziennika jest zwiększane. Waagent loguje się /var/log/waagent.log i wpływa na funkcje logrotate system, aby obrócić dzienników.


**SYSTEM OPERACYJNY. EnableRDMA**  
Typ: logiczna  
Domyślne: n

Jeśli skonfigurowano, agent spróbuje zainstalować, a następnie załadować sterownik jądra RDMA, zgodnej z wersją oprogramowania układowego od używanego sprzętu.


**SYSTEM OPERACYJNY. RootDeviceScsiTimeout:**  
Typ: liczba całkowita  
Domyślne: 300

Limit czasu SCSI to konfiguruje w sekundach na dyskach systemu operacyjnego dysku i danych. Jeśli nie skonfigurowano, systemu, w którym są używane ustawienia domyślne.


**SYSTEM OPERACYJNY. OpensslPath:**  
Typ: ciąg  
Domyślne: Brak

To można określić alternatywną ścieżkę do openssl binarne dla operacji szyfrowania.


**HttpProxy.Host, HttpProxy.Port**  
Typ: ciąg  
Domyślne: Brak

Jeśli skonfigurowano, agent użyje ten serwer proxy dostęp do Internetu. 


## <a name="ubuntu-cloud-images"></a>Obrazy Ubuntu chmury

Należy zauważyć, że Ubuntu chmury obrazy są używane [inicjowania chmury](https://launchpad.net/ubuntu/+source/cloud-init) do wykonywania wielu zadań konfiguracji, które w przeciwnym razie byłyby zarządzane przez agenta Linux Azure.  Uwaga występują następujące różnice:


- **Provisioning.Enabled** domyślnie "n" na obrazów chmury Ubuntu obsługi administracyjnej zadań za pomocą inicjowania chmury.

- Poniższe parametry konfiguracji nie mają wpływu na Ubuntu chmury obrazów Zarządzanie dysk zasobu i Zamień miejsca za pomocą inicjowania chmurze:

 - **ResourceDisk.Format**
 - **ResourceDisk.Filesystem**
 - **ResourceDisk.MountPoint**
 - **ResourceDisk.EnableSwap**
 - **ResourceDisk.SwapSizeMB**

- Zobacz następujące zasoby, aby skonfigurować punkt instalacji dysku zasobów i Zamień miejsca na obrazy chmury Ubuntu podczas inicjowania obsługi administracyjnej:

 - [Typu Ubuntu Wiki: Konfigurowanie partycje wymiany](http://go.microsoft.com/fwlink/?LinkID=532955&clcid=0x409)
 - [Wstrzykiwania niestandardowe dane do Azure maszyn wirtualnych](virtual-machines-windows-classic-inject-custom-data.md)

