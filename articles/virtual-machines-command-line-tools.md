<properties
    pageTitle="Azure poleceń interfejsu wiersza polecenia w trybie Zarządzanie usługą | Microsoft Azure"
    description="Polecenia Azure interfejs wiersza polecenia (polecenie) w trybie Zarządzanie usługą Zarządzanie wdrożeń w modelu Klasyczny wdrażania"
    services="virtual-machines-linux,virtual-machines-windows,mobile-services, cloud-services"
    documentationCenter=""
    authors="dlepow"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="multiple"
    ms.workload="multiple"
    ms.tgt_pltfrm="vm-multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/22/2016"
    ms.author="danlep"/>

# <a name="azure-cli-commands-in-azure-service-management-asm-mode"></a>Azure poleceń interfejsu wiersza polecenia w trybie Zarządzanie usługą Azure (asm)

[AZURE.INCLUDE [learn-about-deployment-models](../includes/learn-about-deployment-models-classic-include.md)]Możesz także [Przeczytaj o wszystkich poleceń modelu Menedżera zasobów](virtual-machines/azure-cli-arm-commands.md)oraz od klasycznych do modelu Menedżera zasobów za pomocą interfejsu wiersza polecenia można [migrować zasobów](virtual-machines/virtual-machines-linux-cli-migration-classic-resource-manager.md) .

Ten artykuł zawiera opis składni i opcje poleceń polecenie Azure, której często używasz do tworzenia i zarządzania Azure zasobów w modelu Klasyczny wdrożenia. Masz dostęp do tych poleceń, uruchamiając polecenie w trybie Zarządzanie usługą Azure (asm). Nie jest to odwołanie ukończone, a wersji polecenie może wyświetlić nieco inną poleceń i parametrów. 

Aby rozpocząć korzystanie, [Zainstaluj polecenie Azure](xplat-cli-install.md) i [łączność z subskrypcji usługi Azure](xplat-cli-connect.md).

Bieżący składni polecenia i opcje w wierszu polecenia wpisz `azure help` lub, aby wyświetlić Pomoc dotyczącą określonego polecenia `azure help [command]`. Także znaleźć polecenie przykładów w dokumentacji do tworzenia i zarządzania określonych usług Azure.

Parametry opcjonalne są wyświetlane w nawiasach kwadratowych (na przykład `[parameter]`). Wszystkie inne parametry są wymagane.

Oprócz polecenia określonych parametrów opisanych tutaj istnieją trzy opcjonalne parametry, których można używać, aby wyświetlić szczegółowe dane wyjściowe, takich jak opcje żądania i kody stanu. `-v` Parametr zawiera pełne dane wyjściowe i `-vv` parametru znajdują się jeszcze bardziej szczegółowe pełne dane wyjściowe. `--json` Opcja wyświetla wynik w formacie nieprzetworzonych json.

## <a name="setting-asm-mode"></a>Ustawianie trybu asm

Użyj następującego polecenia umożliwiające zarządzanie usługą Azure polecenie Tryb poleceń.

    azure config mode asm

>[AZURE.NOTE] Tryb Menedżera zasobów Azure i zarządzanie usługą Azure tryb interfejsu wiersza polecenia są wzajemnie wykluczających się. Oznacza to, że zasoby utworzonych w jednym trybie nie można zarządzać z innego trybu.

## <a name="manage-your-account-information-and-publish-settings"></a>Zarządzanie informacjami o kontach i ustawienia publikowania
Jest jednym ze sposobów polecenie nawiązać połączenie z kontem przy użyciu informacji Azure subskrypcji. (Aby użyć innych opcji, zobacz [Nawiązywanie połączenia z Azure subskrypcję polecenie Azure](xplat-cli-connect.md) ). Te informacje można uzyskać z portalu klasyczny Azure w pliku ustawień publikowania zgodnie z opisem w tym miejscu. Możesz zaimportować plik ustawień Publikuj jako trwałych lokalnej konfiguracji ustawienie polecenie używa dla kolejnych operacji. Należy zaimportować do publikowania ustawienia jeden raz.

**Pobieranie konta [opcje]**

To polecenie otwiera przeglądarki, aby pobrać plik .publishsettings z portalu klasyczny Azure.

    ~$ azure account download
    info:   Executing command account download
    info:   Launching browser to https://windows.azure.com/download/publishprofile.aspx
    help:   Save the downloaded file, then execute the command
    help:   account import <file>
    info:   account download command OK

**konto importu [opcje] &lt;pliku >**


To polecenie importuje plik publishsettings lub certyfikatu, aby go mogą być używane przez narzędzie Sesje w przyszłości.

    ~$ azure account import publishsettings.publishsettings
    info:   Importing publish settings file publishsettings.publishsettings
    info:   Found subscription: 3-Month Free Trial
    info:   Found subscription: Pay-As-You-Go
    info:   Setting default subscription to: 3-Month Free Trial
    warn:   The 'publishsettings.publishsettings' file contains sensitive information.
    warn:   Remember to delete it now that it has been imported.
    info:   Account publish settings imported successfully

> [AZURE.NOTE] Plik publishsettings może zawierać (oznacza to, że nazwa subskrypcji i identyfikator) szczegółowe informacje o więcej niż jedną subskrypcję. Podczas importowania pliku publishsettings pierwszej subskrypcji jest używany jako domyślny opis. Aby użyć innej subskrypcji, uruchom następujące polecenie:
<code>~$ azure config set subscription &lt;other-subscription-id&gt;</code>

**Konto wyczyść pola wyboru]**

To polecenie usuwa przechowywane publishsettings, które zostały zaimportowane. Użyj tego polecenia po zakończeniu na tym komputerze za pomocą narzędzia i chcesz zagwarantować, że narzędzie nie można używać z kontem w przyszłych sesjach.

    ~$ azure account clear
    Clearing account info.
    info:   OK

**listy kont [opcje]**

Lista zaimportowanych subskrypcji

    ~$ azure account list
    info:    Executing command account list
    data:    Name                                    Id
           Current
    data:    --------------------------------------  -------------------------------
    -----  -------
    data:    Forums Subscription                     8679c8be-3b05-49d9-b8fb  true
    data:    Evangelism Team Subscription            9e672699-1055-41ae-9c36  false
    data:    MSOpenTech-Prod                         c13e6a92-706e-4cf5-94b6  false

**Ustawianie konta [opcje] &lt;subskrypcji&gt;**

Ustawianie bieżącej subskrypcji

###<a name="commands-to-manage-your-affinity-groups"></a>Polecenia do zarządzania koligacji grup

**Lista koligacji grupy kont [opcje]**

To polecenie wyświetla Azure grup koligacji.

Jeśli grupy maszyn wirtualnych obejmuje wiele fizycznych komputerów można ustawić koligacji grup. Grupa koligacji Określa, że fizycznych komputerów powinny być jak blisko siebie, jak to możliwe, w celu zmniejszenia czasu oczekiwania w sieci.

    ~$ azure account affinity-group list
    + Fetching affinity groups
    data:   Name                                  Label   Location
    data:   ------------------------------------  ------  --------
    data:   535EBAED-BF8B-4B18-A2E9-8755FB9D733F  opentec  West US
    info:   account affinity-group list command OK

**Tworzenie grupy koligacji kont [opcje] &lt;nazwy&gt;**

To polecenie tworzy grupę koligacji

    ~$ azure account affinity-group create opentec -l "West US"
    info:    Executing command account affinity-group create
    + Creating affinity group
    info:    account affinity-group create command OK

**Grupa koligacji kont Pokaż [opcje] &lt;nazwy&gt;**

To polecenie pokazuje szczegóły grupy koligacji

    ~$ azure account affinity-group show opentec
    info:    Executing command account affinity-group show
    + Getting affinity groups
    data:    $ xmlns "http://schemas.microsoft.com/windowsazure"
    data:    $ xmlns:i "http://www.w3.org/2001/XMLSchema-instance"
    data:    Name "opentec"
    data:    Label "b3BlbnRlYw=="
    data:    Description $ i:nil "true"
    data:    Location "West US"
    data:    HostedServices ""
    data:    StorageServices ""
    data:    Capabilities Capability 0 "PersistentVMRole"
    data:    Capabilities Capability 1 "HighMemory"
    info:    account affinity-group show command OK

**konto grupy koligacji usuwanie [opcje] &lt;nazwy&gt;**

To polecenie usuwa określonej grupy koligacji

    ~$ azure account affinity-group delete opentec
    info:    Executing command account affinity-group delete
    Delete affinity group opentec? [y/n] y
    + Deleting affinity group
    info:    account affinity-group delete command OK

###<a name="commands-to-manage-your-account-environment"></a>Polecenia do zarządzania środowiska konta

**Lista Koperta kont [opcje]**

Lista środowiskach konta

    C:\windows\system32>azure account env list
    info:    Executing command account env list
    data:    Name
    data:    ---------------
    data:    AzureCloud
    data:    AzureChinaCloud
    info:    account env list command OK

**Koperta konta Pokaż [opcje] [środowiska]**

Pokaż szczegółowe informacje o koncie środowiska

    ~$ azure account env show
    info:    Executing command account env show
    Environment name: AzureCloud
    data:    Environment publishingProfile  http://go.microsoft.com/fwlink/?LinkId=2544
    data:    Environment portal  http://go.microsoft.com/fwlink/?LinkId=2544
    info:    account env show command OK

**Koperta konta add [opcje] [środowiska]**

To polecenie dodaje środowisku do konta

**Koperta konta opcji [] [środowiska]**

To polecenie ustawia środowisko konta

**Konto Koperta usuwanie [opcje] [środowiska]**

To polecenie usuwa określonego środowiska z konta

## <a name="commands-to-manage-your-classic-virtual-machines"></a>Polecenia do zarządzania klasyczny maszyn wirtualnych
Na poniższym diagramie przedstawiono sposób klasyczny Azure maszyn wirtualnych obsługiwanych w środowisku produkcyjnym wdrażania usługi Azure chmury.

![Diagram techniczne Azure](./media/virtual-machines-command-line-tools/architecturediagram.jpg)

**Utwórz nowy** tworzy dysk w magazynie obiektów blob (czyli e:\ na diagramie); **Dołączanie** dołącza dysk już utworzony, ale niedołączonej maszyn wirtualnych.

**Tworzenie maszyn wirtualnych [opcje] &lt;nazwy dns > &lt;obraz > &lt;nazwa_użytkownika > [hasło]**

To polecenie tworzy Azure maszyn wirtualnych. Domyślnie każda maszyna wirtualna (maszyn wirtualnych) zostanie utworzona w osobnym usługi w chmurze. Można określić, że maszyny wirtualnej należy dodać do istniejącego usługi w chmurze za pomocą opcji - c. opisane poniżej.

Maszyn wirtualnych tworzenie poleceń, takich jak Azure portal klasyczny, tylko tworzy maszyn wirtualnych w środowisku produkcyjnym wdrożenia. Istnieje możliwość utworzenia maszyny wirtualnej tymczasowy środowiska wdrażania usługi w chmurze. Jeśli Twoja subskrypcja nie ma istniejące konto Azure miejsca do magazynowania, polecenie tworzy jedną.

Można określić lokalizację za pośrednictwem — parametr lokalizacja, lub można wybrać grupę koligacji przez parametr — koligacji grupy. Jeśli nie jest dostępne, zostanie wyświetlony monit o podanie jedną z listy prawidłowych lokalizacji.

Podane hasło musi być 8 123 znaków i spełnia wymagań dotyczących złożoności hasła używanego dla tego komputera wirtualnych systemu operacyjnego.

Jeśli planujesz używać SSH Zarządzanie wdrożonym maszyny wirtualnej Linux (tak jak zwykle w przypadku), należy włączyć SSH za pomocą opcji -e, podczas tworzenia maszyny wirtualnej. Nie jest możliwe włączenie SSH po utworzeniu maszyny wirtualnej.

Maszyn wirtualnych systemu Windows można włączyć RDP później, dodając portu 3389 jako punkt końcowy.

Dla tego polecenia są obsługiwane następujące parametry opcjonalne:

**- c, — łączenie** tworzenie maszyny wirtualnej wewnątrz wdrożeniem utworzone w usłudze hostingu. Jeśli ta opcja nie jest używany — vmname, nazwę nowej maszyny wirtualnej jest generowany automatycznie.<br />
**-n, nazwa — maszyn wirtualnych** Określ nazwę maszyny wirtualnej. Ten parametr ma nazwę usługi hostingu domyślnie. Jeśli nie określono - vmname, nazwę dla nowej maszyny wirtualnej jest generowane jako &lt;nazwa usługi >&lt;identyfikator >, gdzie &lt;identyfikator > liczbę istniejących maszyn wirtualnych usługi oraz 1. Na przykład jeśli to polecenie umożliwia dodawanie maszyny wirtualnej z usługą hostingu Moja_usługa zawierającej istniejących maszyn wirtualnych nowa maszyna wirtualna ma nazwę MyService2.<br />
**-u, — adres url obiektów blob** Określ adres URL magazynu obiektów blob docelowej podczas tworzenia dysku systemowym maszyn wirtualnych. <br />
**-z, rozmiar — pamięci wirtualnej** Określ wielkość maszyny wirtualnej. Prawidłowe wartości: "ExtraSmall", "Małe", "Średni", "Duże", "ExtraLarge", "A5", "A6", "A7", "A8", "A9", "A10", "A11", "Basic_A0", "Basic_A1", "Basic_A2", "Basic_A3", "Basic_A4", "Standard_D1", "Standard_D2", "Standard_D3", "Standard_D4", "Standard_D11", "Standard_D12", "Standard_D13", "Standard_D14", "Standard_DS1", "Standard_DS2", "Standard_DS3", "Standard_DS4", "Standard_DS11", "Standard_DS12", "Standard_DS13", "Standard_DS14", "Standard_G1", "Standard_G2" , "Standard_G3", "Standard_G4", "Standard_G55". Wartość domyślna to "Małe". <br />
**-r** Dodaje łączności RDP maszyn wirtualnych systemu Windows. <br />
**-e, — ssh** Dodaje łączności SSH maszyn wirtualnych systemu Windows. <br />
**-t, — ssh certyfikatu** Określa certyfikat SSH. <br />
**-s** Subskrypcji <br />
**-o, — społeczności** Określony obraz jest obrazem społeczności <br />
**-w** Nazwa wirtualnej sieci <br/>
**-l, — lokalizacja** Określa lokalizację (na przykład "Ameryka Północna centralnej My"). <br />
**, — grupa koligacji** Określa grupę koligacji.<br />
**-w, — wirtualnej Nazwa sieciowa** Określ, wirtualną sieć, według którego chcesz dodać nowy maszyny wirtualnej. Wirtualnych sieci można skonfigurować i zarządzać za pomocą portalu klasyczny Azure.<br />
**-b, — nazwy podsieci** Umożliwia określenie nazwy podsieci, aby przypisać maszyny wirtualnej.

W tym przykładzie MSFT__Win2K8R2SP1-120514-1520-141205-01-en-us-30GB to obraz dostarczone przez platformę. Aby uzyskać więcej informacji na temat obrazów systemu operacyjnego Zobacz listę obraz maszyn wirtualnych.

    ~$ azure vm create my-vm-name MSFT__Windows-Server-2008-R2-SP1.11-29-2011 username --location "West US" -r
    info:   Executing command vm create
    Enter VM 'my-vm-name' password: ************
    info:   vm create command OK

**Tworzenie z maszyn wirtualnych &lt;nazwy dns > &lt;roli pliku >**

To polecenie tworzy Azure maszyn wirtualnych z pliku roli JSON.

    ~$ azure vm create-from my-vm example.json
    info:   OK

**Lista maszyn wirtualnych [opcje]**

To polecenie wyświetla Azure maszyn wirtualnych. Opcja — json Określa, że wyniki są zwracane w formacie JSON nieprzetworzonych.

    ~$ azure vm list
    info:   Executing command vm list
    data:   DNS Name                          VM Name      Status
    data:   --------------------------------  -----------  ---------
    data:   my-vm-name.cloudapp-preview.net        my-vm        ReadyRole
    info:   vm list command OK

**Lista lokalizacji maszyn wirtualnych [opcje]**

To polecenie wyświetla wszystkie lokalizacje dostępne konto Azure.

    ~$ azure vm location list
    info:   Executing command vm location list
    data:   Name                   Display Name
    data:   ---------------------  ------------
    data:   Azure Preview  West US
    info:   account location list command OK

**Pokazywanie maszyn wirtualnych [opcje] &lt;nazwa >**

To polecenie zawiera szczegółowe informacje o Azure maszyn wirtualnych. Opcja — json Określa, że wyniki są zwracane w formacie JSON nieprzetworzonych.

    ~$ azure vm show my-vm
    info:   Executing command vm show
    data:   {
    data:       InstanceSize: 'Small',
    data:       InstanceStatus: 'ReadyRole',
    data:       DataDisks: [],
    data:       IPAddress: '10.26.192.206',
    data:       DNSName: 'my-vm.cloudapp.net',
    data:       InstanceStateDetails: {},
    data:       VMName: 'my-vm',
    data:       Network: {
    data:           Endpoints: [
    data:               {
    data:                   Protocol: 'tcp',
    data:                   Vip: '65.52.250.250',
    data:                   Port: '63238' ,
    data:                   LocalPort: '3389',
    data:                   Name: 'RemoteDesktop'
    data:               }
    data:           ]
    data:       },
    data:       Image: 'MSFT__Windows-Server-2008-R2-SP1.11-29-2011',
    data:       OSVersion: 'WA-GUEST-OS-1.18_201203-01'
    data:   }
    info:   vm show command OK

**Usuwanie maszyn wirtualnych [opcje] &lt;nazwa >**

To polecenie usuwa Azure maszyn wirtualnych. Domyślnie to polecenie nie powoduje usunięcia obiektów blob platformy Azure, z której są tworzone dysku systemu operacyjnego i danych. Aby usunąć to i maszyny wirtualnej, na którym jest oparty, określić opcję -b.

    ~$ azure vm delete my-vm
    info:   Executing command vm delete
    info:   vm delete command OK

**maszyn wirtualnych start [opcje] &lt;nazwa >**

To polecenie uruchamia Azure maszyn wirtualnych.

    ~$ azure vm start my-vm
    info:   Executing command vm start
    info:   vm start command OK

**[opcje] ponownego uruchamiania maszyn wirtualnych &lt;nazwa >**

To polecenie ponowne uruchomienie Azure maszyn wirtualnych.

    ~$ azure vm restart my-vm
    info:   Executing command vm restart
    info:   vm restart command OK

**Zamknięcie maszyn wirtualnych [opcje] &lt;nazwa >**

To polecenie zamyka Azure maszyn wirtualnych. Opcja -p może umożliwia określenie, zamknięcia nie są wydane zasobów obliczeń.

```
~$ azure vm shutdown my-vm
info:   Executing command vm shutdown
info:   vm shutdown command OK  
```

**Przechwytywanie maszyn wirtualnych &lt;nazwę maszyn wirtualnych > &lt;docelowej obraz nazw >**

To polecenie przechwycenie Azure maszyn wirtualnych.

Obraz maszyn wirtualnych można zarejestrować tylko, jeśli stan maszyn wirtualnych jest **zatrzymania**. Przed kontynuowaniem zamknij maszyny wirtualnej.

    ~$ azure.cmd vm capture my-vm mycaptureimagename --delete
    info:   Executing command vm capture
    + Fetching VMs
    + Capturing VM
    info:   vm capture command OK

**Eksportowanie maszyn wirtualnych [opcje] &lt;nazwę maszyn wirtualnych > &lt;ścieżka pliku >**

To polecenie eksportuje obrazu Azure maszyn wirtualnych do pliku

    ~$ azure vm export "myvm" "C:\"
    info:    Executing command vm export
    + Getting virtual machines
    + Exporting the VM
    info:   vm export command OK

##  <a name="commands-to-manage-your-azure-virtual-machine-endpoints"></a>Polecenia do zarządzania punktów końcowych Azure maszyn wirtualnych
Na poniższym diagramie przedstawiono architektura Typowe rozmieszczanie wielu wystąpień klasycznego maszyny wirtualnej. W tym przykładzie port 3389 jest otwarty na każdym komputerze wirtualnych (dla dostępu RDP). Na każdym komputerze wirtualnych używaną przez usługę równoważenia obciążenia do rozsyłania ruchu maszyn wirtualnych również jest wewnętrzny adres IP (na przykład 168.55.11.1). Można także ten wewnętrzny adres IP dla komunikacji między maszyn wirtualnych.

![azurenetworkdiagram](./media/virtual-machines-command-line-tools/networkdiagram.jpg)

Zewnętrzne żądania maszyn wirtualnych przejdź za pośrednictwem usługi równoważenia obciążenia. Z tego powodu żądania nie można określić przed określoną maszyn wirtualnych w wdrożeń wiele maszyn wirtualnych. W przypadku wdrożeń z wielu maszyn wirtualnych należy skonfigurować mapowanie portu między maszyn wirtualnych (maszyn wirtualnych port) i równoważenia obciążenia (kg port).

**Tworzenie punktu końcowego maszyn wirtualnych &lt;nazwę maszyn wirtualnych > &lt;kg port > [maszyn wirtualnych port]**

To polecenie tworzy punkt końcowy maszyn wirtualnych. Można także użyć -u lub--serwera zwrotny, Włącz bezpośrednio w-Określ, czy umożliwiający bezpośredni serwera zwrot z tego punktu końcowego, domyślnie wyłączone.

    ~$ azure vm endpoint create my-vm 8888 8888
    azure vm endpoint create my-vm 8888 8888
    info:   Executing command vm endpoint create
    + Fetching VM
    + Reading network configuration
    + Updating network configuration
    info:   vm endpoint create command OK

**maszyn wirtualnych punktu końcowego tworzenie zakres [opcje] &lt;nazwę maszyn wirtualnych > &lt;kg port > [:&lt;port maszyn wirtualnych > [:&lt;Protocol (protokół) > [:&lt;serwera zwrotny, Włącz bezpośrednio w-> [:&lt;kg zestaw nazw > [:&lt;sondy protokołu > [:&lt;port sondy > [:&lt;sondy ścieżka > [:&lt;nazwa wewnętrzna kg >]]] {1 -*}**

Tworzenie wielu maszyn wirtualnych punktów końcowych.

**Usuwanie punktu końcowego maszyn wirtualnych [opcje] &lt;nazwę maszyn wirtualnych > &lt;Nazwa punktu końcowego >**

To polecenie usuwa punkt końcowy maszyn wirtualnych.

    ~$ azure vm endpoint delete my-vm http
    azure vm endpoint delete my-vm http
    info:   Executing command vm endpoint delete
    + Fetching VM
    + Reading network configuration
    + Updating network configuration
    info:   vm endpoint delete command OK

**Lista punktów końcowych maszyn wirtualnych &lt;nazwę maszyn wirtualnych >**

To polecenie wyświetla wszystkich maszyn wirtualnych punktów końcowych. Opcja — json Określa, że wyniki są zwracane w formacie JSON nieprzetworzonych.

    ~$ azure vm endpoint list my-linux-vm
    data:   Name  External Port  Local Port
    data:   ----  -------------  ----------
    data:   ssh   22             22

**Aktualizacja punktu końcowego maszyn wirtualnych [opcje] &lt;nazwę maszyn wirtualnych > &lt;Nazwa punktu końcowego >**

To polecenie aktualizuje punkt końcowy maszyn wirtualnych do nowych wartości przy użyciu następujących opcji.

    -n, --endpoint-name <name>          the new endpoint name
    -lo, --lb-port <port>                the new load balancer port
    -t, --vm-port <port>                the new local port
    -o, --endpoint-protocol <protocol>  the new transport layer protocol for port (tcp or udp)

**punkt końcowy maszyn wirtualnych Pokaż [opcje] &lt;nazwę maszyn wirtualnych >**

To polecenie zawiera szczegóły punktów końcowych maszyny

    ~$ azure vm endpoint show "mycouchvm"
    info:    Executing command vm endpoint show
    + Getting virtual machines
    data:    Network Endpoints 0 LoadBalancedEndpointSetName "CouchDB_EP-5984"
    data:    Network Endpoints 0 LocalPort "5984"
    data:    Network Endpoints 0 Name "CouchDB_EP"
    data:    Network Endpoints 0 Port "5984"
    data:    Network Endpoints 0 Protocol "tcp"
    data:    Network Endpoints 0 Vip "168.61.9.97"
    data:    Network Endpoints 1 LoadBalancedEndpointSetName "CouchEP_2-2020"
    data:    Network Endpoints 1 LocalPort "2020"
    data:    Network Endpoints 1 Name "CouchEP_2"
    data:    Network Endpoints 1 Port "2020"
    data:    Network Endpoints 1 Protocol "tcp"
    data:    Network Endpoints 1 Vip "168.61.9.97"
    data:    Network Endpoints 2 LocalPort "3389"
    data:    Network Endpoints 2 Name "RemoteDesktop"
    data:    Network Endpoints 2 Port "3389"
    data:    Network Endpoints 2 Protocol "tcp"
    data:    Network Endpoints 2 Vip "168.61.9.97"
    info:    vm endpoint show command OK

## <a name="commands-to-manage-your-azure-virtual-machine-images"></a>Polecenia do zarządzania obrazy Azure maszyn wirtualnych

Obrazy maszyn wirtualnych są przechwycone obrazy już skonfigurowaną maszyn wirtualnych, które mogą być replikowane stosownie do potrzeb.

**Lista obrazów maszyn wirtualnych [opcje]**

To polecenie otrzymuje listę maszyn wirtualnych obrazów. Istnieją trzy typy obrazów: obrazy utworzoną przez firmę Microsoft, które są prefiksem "MSFT", obrazy utworzone przez inne firmy, które są prefiksem Nazwa dostawcy i obrazy, można utworzyć. Do tworzenia obrazów, możesz przechwycić istniejących maszyn wirtualnych lub utworzyć obraz z niestandardowej VHD przekazane do magazyn obiektów blob. Aby uzyskać więcej informacji o korzystaniu z niestandardowej VHD zobaczyć obraz maszyn wirtualnych tworzenie.
Opcja — json Określa, że wyniki są zwracane w formacie JSON nieprzetworzonych.

    ~$ azure vm image list
    data:   Name                                                                   Category   OS
    data:   ---------------------------------------------------------------------  ---------  -------
    data:   CANONICAL__Canonical-Ubuntu-12-04-20120519-2012-05-19-en-us-30GB.vhd   Canonical  Linux
    data:   MSFT__Windows-Server-2008-R2-SP1.11-29-2011                            Microsoft  Windows
    data:   MSFT__Windows-Server-2008-R2-SP1-with-SQL-Server-2012-Eval.11-29-2011  Microsoft  Windows
    data:   MSFT__Windows-Server-8-Beta.en-us.30GB.2012-03-22                      Microsoft  Windows
    data:   MSFT__Windows-Server-8-Beta.2-17-2012                                  Microsoft  Windows
    data:   MSFT__Windows-Server-2008-R2-SP1.en-us.30GB.2012-3-22                  Microsoft  Windows
    data:   OpenLogic__OpenLogic-CentOS-62-20120509-en-us-30GB.vhd                 OpenLogic  Linux
    data:   SUSE__SUSE-Linux-Enterprise-Server-11SP2-20120521-en-us-30GB.vhd       SUSE       Linux
    data:   SUSE__OpenSUSE64121-03192012-en-us-15GB.vhd                            SUSE       Linux
    data:   WIN2K8-R2-WINRM                                                        User       Windows
    info:   vm image list command OK

**Pokaż obraz maszyn wirtualnych [opcje] &lt;nazwa >**

To polecenie pokazuje szczegóły obrazu maszyn wirtualnych.

    ~$ azure vm image show MSFT__Windows-Server-2008-R2-SP1.11-29-2011
    + Fetching VM image
    info:   Executing command vm image show
    data:   {
    data:       Label: 'Windows Server 2008 R2 SP1, Nov 2011',
    data:       Name: 'MSFT__Windows-Server-2008-R2-SP1.11-29-2011',
    data:       Description: 'Microsoft Windows Server 2008 R2 SP1',
    data:       @: { xmlns: 'http://schemas.microsoft.com/windowsazure', xmlns:i: 'http://www.w3.org/2001/XMLSchema-instance' },
    data:       Category: 'Microsoft',
    data:       OS: 'Windows',
    data:       Eula: 'http://www.microsoft.com',
    data:       LogicalSizeInGB: '30'
    data:   }
    info:   vm image show command OK

**Usuwanie obrazu maszyn wirtualnych [opcje] &lt;nazwa >**

To polecenie usuwa obraz maszyn wirtualnych.

    ~$ azure vm image delete my-vm-image
    info:   Executing command vm image delete
    info:   VM image deleted: my-vm-image
    info:   vm image delete command OK

**Tworzenie maszyn wirtualnych obraz &lt;nazwa > [ścieżka źródłowa]**

To polecenie tworzy obraz maszyn wirtualnych. Plików niestandardowych VHD są przekazywane do blob miejsca do magazynowania, a następnie obraz maszyn wirtualnych jest tworzony stamtąd. Ten obraz maszyn wirtualnych jest następnie wykorzystać do tworzenia maszyny wirtualnej. Parametry lokalizacji i OS są wymagane.

>[AZURE.NOTE]To polecenie obsługuje obecnie tylko plików VHD stały. Aby przekazać plik VHD dynamiczne, za pomocą [Narzędzia Azure wirtualnego dysku twardego dla Przejdź](https://github.com/Microsoft/azure-vhd-utils-for-go).

Niektóre systemy nałożyć limity deskryptora plików na proces. Jeśli ten limit zostanie przekroczony, narzędzie wyświetli błąd limit deskryptora pliku. Można uruchomić polecenie ponownie, używając -p &lt;numer > parametr, aby zmniejszyć maksymalną liczbę równoległych przekazywania. Domyślna maksymalna liczba równoległych przekazywania jest 96.

    ~$ azure vm image create mytestimage ./Sample.vhd -o windows -l "West US"
    info:   Executing command vm image create
    + Retrieving storage accounts
    info:   VHD size : 13 MB
    info:   Uploading 13312.5 KB
    Requested:100.0% Completed:100.0% Running: 105 Time:    8s Speed:  1721 KB/s
    info:   http://myaccount.blob.core.azure.com/vm-images/Sample.vhd is uploaded successfully
    info:   vm image create command OK

## <a name="commands-to-manage-your-azure-virtual-machine-data-disks"></a>Polecenia do zarządzania dysków danych Azure maszyn wirtualnych

Dyski danych to pliki VHD w magazynie obiektów blob, które mogą być używane przez maszyny wirtualnej. Aby uzyskać więcej informacji na temat dyski danych są wdrażane blob miejsca do magazynowania, zobacz Azure diagram techniczne przedstawionym wcześniej.

Polecenia służące do dołączania dyski danych (dołączyć dysku azure maszyn wirtualnych i azure maszyn wirtualnych dysku Dołącz nowe) przypisać numer jednostki logicznej (LUN) na dysku załączonych danych zgodnie z wymaganiami protokołu SCSI. Pierwszy dysk danych dołączone do maszyny wirtualnej przypisano LUN 0, następnej przydzielonych LUN 1 i tak dalej.

Po odłączeniu dyskiem danych dyskiem azure maszyn wirtualnych odłączanie polecenia, za pomocą &lt;lun&gt; parametr, aby wskazać, które dysku, aby odłączyć.

>[AZURE.NOTE] Należy zawsze odłączeniu dyski danych w odwrotnej kolejności, zaczynając od najwyższym numerze LUN, która została przypisana. Warstwy Linux SCSI nie obsługuje odłączenia niższych numerach LUN podczas wyższym numerze LUN nadal jest dołączony. Nie należy na przykład odłączanie LUN 0, jeśli LUN 1 nadal jest dołączony.

**Pokaż dysku maszyn wirtualnych [opcje] &lt;nazwa >**

To polecenie zawiera szczegółowe informacje o Azure dysku.

    ~$ azure vm disk show anucentos-anucentos-0-20120524070008
    info:   Executing command vm disk show
    data:   AttachedTo DeploymentName "mycentos"
    data:   AttachedTo HostedServiceName "myanucentos"
    data:   AttachedTo RoleName "myanucentos"
    data:   OS "Linux"
    data:   Location "Azure Preview"
    data:   LogicalDiskSizeInGB "30"
    data:   MediaLink "http://mystorageaccount.blob.core.azure-preview.com/vhd-store/mycentos-cb39b8223b01f95c.vhd"
    data:   Name "mycentos-mycentos-0-20120524070008"
    data:   SourceImageName "OpenLogic__OpenLogic-CentOS-62-20120509-en-us-30GB.vhd"
    info:   vm disk show command OK

**Lista dysków maszyn wirtualnych [opcje] [maszyn wirtualnych nazwa]**

To polecenie list Azure dysków lub dysków dołączone do określonej maszyny wirtualnej. Jeśli jest uruchomiony przy użyciu parametru Nazwa maszyn wirtualnych, zwraca wszystkie dyski dołączone do maszyny wirtualnej. LUN 1 jest tworzone maszyny wirtualnej, a inne dyski wymienionych dołączonych oddzielnie.

    ~$ azure vm disk list mycentos
    info:   Executing command vm disk list
    data:   Lun  Size(GB)  Blob-Name
    data:   ---  --------  --------------------------------
    data:   1    30        mycentos-cb39b8223b01f95c.vhd
    data:   2    10        mycentos-e3f0d717950bb78d.vhd
    info:   vm disk list command OK

Wykonywanie tego polecenia bez parametru Nazwa maszyn wirtualnych zwraca wszystkich dysków.

    ~$ azure vm disk list
    data:   Name                                        OS
    data:   ------------------------------------------  -------
    data:   mycentos-mycentos-0-20120524070008          Linux
    data:   mycentos-mycentos-2-20120525055052
    data:   mywindows-winvm-20120522223119              Windows
    info:   vm disk list command OK

**Usuwanie dysku maszyn wirtualnych [opcje] &lt;nazwa >**

To polecenie usuwa Azure dysku z osobistym repozytorium. Dysk musi być odłączony od maszyny wirtualnej, przed usunięciem.

    ~$ azure vm disk delete mycentos-mycentos-2-20120525055052
    info:   Executing command vm disk delete
    info:   Disk deleted: mycentos-mycentos-2-20120525055052
    info:   vm disk delete command OK

**Tworzenie maszyn wirtualnych dysku &lt;nazwa > [ścieżka źródłowa]**

To polecenie przekazywanie i rejestruje Azure dysku. — adres url obiektów blob, — lokalizację lub — należy określić grupę koligacji. Użycie tego polecenia z [ścieżka źródłowa] przekazaniu określonego pliku VHD i utworzeniu obrazu. Ten obraz można następnie dołączyć maszyn wirtualnych, za pomocą dysku maszyn wirtualnych Dołącz.

Niektóre systemy nałożyć limity deskryptora plików na proces. Jeśli ten limit zostanie przekroczony, narzędzie wyświetli błąd limit deskryptora pliku. Można uruchomić polecenie ponownie, używając -p &lt;numer > parametr, aby zmniejszyć maksymalną liczbę równoległych przekazywania. Domyślna maksymalna liczba równoległych przekazywania jest 96.

    ~$ azure vm disk create my-data-disk ~/test.vhd --location "West US"
    info:   Executing command vm disk create
    info:   VHD size : 10 MB
    info:   Uploading 10240.5 KB
    Requested:100.0% Completed:100.0% Running:  81 Time:   11s Speed:   952 KB/s
    info:   http://account.blob.core.azure.com/disks/test.vhd is uploaded successfully
    info:   vm disk create command OK

**Przekaż dysku maszyn wirtualnych [opcje] &lt;ścieżki źródłowej > &lt;adres url obiektów blob > &lt;miejsca do magazynowania konta klucza >**

To polecenie umożliwia przekazywanie dysku maszyn wirtualnych

    ~$ azure vm disk upload "http://sourcestorage.blob.core.windows.net/vhds/sample.vhd" "http://destinationstorage.blob.core.windows.net/vhds/sample.vhd" "DESTINATIONSTORAGEACCOUNTKEY"
    info:   Executing command vm disk upload
    info:   Uploading 12351.5 KB
    info:   vm disk upload command OK

**Dołączanie dysku maszyn wirtualnych &lt;nazwę maszyn wirtualnych > &lt;nazwa dysku obraz >**

To polecenie dołącza istniejący dysk w magazynie obiektów blob istniejących maszyn wirtualnych wdrożony w usłudze w chmurze.

    ~$ azure vm disk attach my-vm my-vm-my-vm-2-201242418259
    info:   Executing command vm disk attach
    info:   vm disk attach command OK

**maszyn wirtualnych dysku Dołącz nowe &lt;nazwę maszyn wirtualnych > &lt;rozmiar w gb > [adres url obiektów blob]**

To polecenie dołącza dysku danych Azure maszyn wirtualnych. W tym przykładzie 20 jest wielkością nowego dysku, w gigabajtów załączyć. Można opcjonalnie jawnie określić obiektów blob docelowej, aby utworzyć za pomocą adresu URL obiektów blob jako ostatni argument. Jeśli nie określisz adres URL obiektów blob, obiektów blob jest generowany automatycznie.

    ~$ azure vm disk attach-new nick-test36 20 http://nghinazz.blob.core.azure-preview.com/vhds/vmdisk1.vhd
    info:   Executing command vm disk attach-new
    info:   vm disk attach-new command OK  

**Odłączanie dysku maszyn wirtualnych &lt;nazwę maszyn wirtualnych > &lt;lun >**

To polecenie odłącza dysku danych Azure maszyn wirtualnych. &lt;LUN > określa dysk, który ma zostać odłączony. Aby uzyskać listę dysków związanych z dyskiem, zanim zostanie odłączona, użyj listy dysków maszyn wirtualnych &lt;nazwę maszyn wirtualnych >.

    ~$ azure vm disk detach my-vm 2
    info:   Executing command vm disk detach
    info:   vm disk detach command OK

## <a name="commands-to-manage-your-azure-cloud-services"></a>Polecenia do zarządzania usługami chmura Azure

Usług w chmurze Azure są aplikacji i usług dostępnych w sieci web ról i pracownika. Następujące polecenia umożliwia zarządzanie usługami w chmurze Azure.

**[opcje] tworzenia usługi &lt;NazwaUsługi >**

To polecenie tworzy usługi w chmurze

    ~$ azure service create newservicemsopentech
    info:    Executing command service create
    + Getting locations
    help:    Location:
      1) East Asia
      2) Southeast Asia
      3) North Europe
      4) West Europe
      5) East US
      6) West US
      : 6
    + Creating cloud service
    data:    Cloud service name newservicemsopentech
    info:    service create command OK

**Usługa Pokaż [opcje] &lt;NazwaUsługi >**

To polecenie zawiera szczegółowe informacje o usłudze w chmurze Azure

    ~$ azure service show newservicemsopentech
    info:    Executing command service show
    + Getting cloud service
    data:    Name newservicemsopentech
    data:    Url https://management.core.windows.net/9e672699-1055-41ae-9c36-e85152f2e352/services/hostedservices/newservicemsopentech
    data:    Properties location West US
    data:    Properties label newservicemsopentech
    data:    Properties status Created
    data:    Properties dateCreated
    data:    Properties dateLastModified
    info:    service show command OK

**Lista usług [opcje]**

To polecenie wyświetla usług w chmurze Azure.

    ~$ azure service list
    info:   Executing command service list
    data:   Name         Status
    data:   -----------  -------
    data:   service1     Created
    data:   service2     Created
    info:   service list command OK

**Usługa usunąć [opcje] &lt;nazwa >**

To polecenie usuwa usługi w chmurze Azure.

    ~$ azure service delete myservice
    info:   Executing command service delete myservice
    info:   cloud-service delete command OK

Aby wymusić usunięcie, użyj `-q` parametru.


## <a name="commands-to-manage-your-azure-certificates"></a>Polecenia do zarządzania Azure certyfikatów

Usługa Azure certyfikaty są certyfikatów SSL powiązany z Twoim kontem Azure. Aby uzyskać więcej informacji na temat certyfikatów Azure zobacz [Zarządzanie certyfikaty](http://msdn.microsoft.com/library/azure/gg981929.aspx).

**Lista certyfikatu usługi [opcje]**

To polecenie wyświetla Azure certyfikaty.

    ~$ azure service cert list
    info:   Executing command service cert list
    + Fetching cloud services
    + Fetching certificates
    data:   Service   Thumbprint                                Algorithm
    data:   --------  ----------------------------------------  ---------
    data:   myservice  262DBF95B5E61375FA27F1E74AC7D9EAE842916C  sha1
    info:   service cert list command OK

**Tworzenie certyfikatu usługi &lt;prefiks dns > &lt;pliku > [hasło]**

To polecenie przekazywanie certyfikatu. Puste monitu o hasło certyfikatów, które nie są chronione hasłem.

    ~$ azure service cert create nghinazz ~/publishSet.pfx
    info:   Executing command service cert create
    Cert password:
    + Creating certificate
    info:   service cert create command OK

**Usuwanie certyfikatu usługi [opcje] &lt;odcisku palca >**

To polecenie usuwa certyfikatu.

    ~$ azure service cert delete 262DBF95B5E61375FA27F1E74AC7D9EAE842916C
    info:   Executing command service cert delete
    + Deleting certificate
    info:   nghinazz : cert deleted
    info:   service cert delete command OK

## <a name="commands-to-manage-your-web-apps"></a>Poleceń, aby zarządzać swoimi aplikacjami sieci web

Konfiguracja sieci web dostępne przez identyfikator URI jest Azure w przeglądarce. Aplikacje sieci Web są obsługiwane w środowisku maszyn wirtualnych, ale nie należy wziąć pod uwagę szczegóły tworzenie i wdrażanie maszyny wirtualnej samodzielnie. Szczegóły te są obsługiwane dla Ciebie przez Azure.

**Lista witryn [opcje]**

To polecenie wyświetla aplikacji sieci web.

    ~$ azure site list
    info:   Executing command site list
    data:   Name            State    Host names
    data:   --------------  -------  --------------------------------------------------
    data:   mongosite       Running  mongosite.antdf0.antares.windows.net
    data:   myphpsite       Running  myphpsite.antdf0.antares.windows.net
    data:   mydrupalsite36  Running  mydrupalsite36.antdf0.antares.windows.net
    info:   site list command OK

**Konfigurowanie witryny [opcje] [nazwa]**

To polecenie ustawia opcje konfiguracji dla aplikacji sieci web [nazwa]

    ~$ azure site set
    info:    Executing command site set
    Web site name: mydemosite
    + Getting sites
    + Updating site config information
    info:    site set command OK

**Witryna deploymentscript [opcje]**

To polecenie generuje skryptu niestandardowego wdrażania

    ~$ azure site deploymentscript --node
    info:    Executing command site deploymentscript
    info:    Generating deployment script for node.js Web Site
    info:    Generated deployment script files
    info:    site deploymentscript command OK

**Tworzenie witryny [opcje] [nazwa]**

To polecenie tworzy aplikacji sieci web i katalogu lokalnego.

    ~$ azure site create mysite
    info:   Executing command site create
    info:   Using location northeuropewebspace
    info:   Creating a new web site
    info:   Created web site at  mysite.antdf0.antares.windows.net
    info:   Initializing repository
    info:   Repository initialized
    info:   site create command OK

> [AZURE.NOTE] Nazwa witryny musi być unikatowa. Nie można tworzyć witryny o takiej samej nazwie DNS jako istniejącej witryny.

**Przeglądanie witryny [opcje] [nazwa]**

To polecenie otwiera aplikacji sieci web w przeglądarce.

    ~$ azure site browse mysite
    info:   Executing command site browse
    info:   Launching browser to http://mysite.antdf0.antares-test.windows-int.net
    info:   site browse command OK

**Pokaż witryny [opcje] [nazwa]**

To polecenie zawiera szczegóły dotyczące aplikacji sieci web.

    ~$ azure site show mysite
    info:   Executing command site show
    info:   Showing details for site
    data:   Site AdminEnabled true
    data:   Site HostNames mysite.antdf0.antares-test.windows-int.net
    data:   Site Name mysite
    data:   Site Owner 00060000814EDDEE
    data:   Site RepositorySiteName mysite
    data:   Site SelfLink https://s1.api.antdf0.antares.windows.net:454/subscriptions/444e62ff-4c5f-4116-a695-5c803ed584a5/webspaces/northeuropewebspace/sites/mysite
    data:   Site State Running
    data:   Site UsageState Normal
    data:   Site WebSpace northeuropewebspace
    data:   Config AppSettings
    data:   Config ConnectionStrings
    data:   Config DefaultDocuments 0=Default.htm, 1=Default.asp, 2=index.htm, 3=index.html, 4=iisstart.htm, 5=default.aspx, 6=index.php, 7=hostingstart.aspx
    data:   Config DetailedErrorLoggingEnabled false
    data:   Config HttpLoggingEnabled false
    data:   Config Metadata
    data:   Config NetFrameworkVersion v4.0
    data:   Config NumberOfWorkers 1
    data:   Config PhpVersion 5.3
    data:   Config PublishingPassword rJ}[Er2v[Y]q16B6vTD]n$[C2z}Z.pvgLfRcLnAp%ax]xstiLny};o@vmMAote@d
    data:   Config RequestTracingEnabled false
    data:   Repository https://mysite.scm.antdf0.antares-test.windows-int.net/
    info:   site show command OK

**Usuwanie witryny [opcje] [nazwa]**

To polecenie usuwa aplikacji sieci web.

    ~$ azure site delete mysite
    info:   Executing command site delete
    info:   Deleting site mysite
    info:   Site mysite has been deleted
    info:   site delete command OK

 **zamienianie witryny [opcje] [nazwa]**

To polecenie zamienia dwa gniazda aplikacji sieci web.

To polecenie obsługuje następujących dodatkowych opcji:

**- q lub **— cichy **: nie Monituj o potwierdzenie. Użyj tej opcji w skrypty automatyczne.


**Rozpocznij witryny [opcje] [nazwa]**

To polecenie uruchamia aplikacji sieci web.

    ~$ azure site start mysite
    info:   Executing command site start
    info:   Starting site mysite
    info:   Site mysite has been started
    info:   site start command OK

**zatrzymanie witryny [opcje] [nazwa]**

To polecenie zatrzymuje aplikacji sieci web.

    ~$ azure site stop mysite
    info:   Executing command site stop
    info:   Stopping site mysite
    info:   Site mysite has been stopped
    info:   site stop command OK

**Uruchom ponownie witryny [opcje] [nazwa]**

To polecenie przestaje, a następnie uruchomienie aplikacji określona witryna sieci web.

To polecenie obsługuje następujących dodatkowych opcji:

**— przedział** &lt;przedział >: Nazwa przedział go ponownie.


**Lista lokalizacji witryny [opcje]**

To polecenie wyświetla lokalizacje aplikacji sieci web.

    ~$ azure site location list
    info:    Executing command site location list
    + Getting locations
    data:    Name
    data:    ----------------
    data:    West Europe
    data:    West US
    data:    North Central US
    data:    North Europe
    data:    East Asia
    data:    East US
    info:    site location list command OK

###<a name="commands-to-manage-your-web-app-application-settings"></a>Polecenia do zarządzania ustawieniami aplikacji aplikacji sieci web

**Lista appsetting witryny [opcje] [nazwa]**

To polecenie wyświetla ustawienie aplikacji dodany do aplikacji sieci web.

    ~$ azure site appsetting list
    info:    Executing command site appsetting list
    Web site name: mydemosite
    + Getting sites
    + Getting site config information
    data:    Name  Value
    data:    ----  -----
    data:    test  value
    info:    site appsetting list command OK

**Dodawanie appsetting witryny [opcje] &lt;keyvaluepair > [nazwa]**

To polecenie dodaje ustawienie aplikacji do aplikacji sieci web jako parę wartości klucza.

    ~$ azure site appsetting add test=value
    info:    Executing command site appsetting add
    Web site name: mydemosite
    + Getting sites
    + Getting site config information
    + Updating site config information
    info:    site appsetting add command OK

**Usuwanie witryny appsetting [opcje] &lt;klucz > [nazwa]**

To polecenie usuwa ustawienie określonej aplikacji z aplikacji sieci web.

    ~$ azure site appsetting delete test
    info:    Executing command site appsetting delete
    Web site name: mydemosite
    + Getting sites
    + Getting site config information
    Delete application setting test? [y/n] y
    + Updating site config information
    info:    site appsetting delete command OK

**Pokazywanie appsetting witryny [opcje] &lt;klucz > [nazwa]**

To polecenie wyświetla szczegóły ustawienie określonej aplikacji

    ~$ azure site appsetting show test
    info:    Executing command site appsetting show
    Web site name: mydemosite
    + Getting sites
    + Getting site config information
    data:    Value:  value
    info:    site appsetting show command OK

###<a name="commands-to-manage-your-web-app-certificates"></a>Polecenia do zarządzania własnych certyfikatów aplikacji sieci web

**Lista certyfikatu witryny [opcje] [nazwa]**

To polecenie wyświetla listę certyfikaty aplikacji sieci web.

    ~$ azure site cert list
    info:    Executing command site cert list
    Web site name: mydemosite
    + Getting sites
    + Getting site information
    data:    Subject                       Expiration Date                    Thumbprint
    data:    ----------------------------  -----------------------------------------
    ----------------  ----------------------------------------
    data:    *.msopentech.com              Fri Nov 28 2014 09:49:57 GMT-0800 (Pacific Standard Time)  A40E82D3DC0286D1F58650E570ECF8224F69A148
    data:    msopentech.azurewebsites.net  Fri Jun 19 2015 11:57:32 GMT-0700 (Pacific Daylight Time)  CE1CD6538852BF7A5DC32001C2E26A29B541F0E8
    info:    site cert list command OK

**Dodawanie certyfikatu witryny [opcje] &lt;ścieżka certyfikatu > [nazwa]**

**Usuwanie certyfikatu witryny [opcje] &lt;odcisku palca > [nazwa]**

**witryny certyfikatu, Pokaż [opcje] &lt;odcisku palca > [nazwa]**

To polecenie pokazuje szczegóły certyfikatu

    ~$ azure site cert show CE1CD65852B38DC32001C2E0E8F7A526A29B541F
    info:    Executing command site cert show
    Web site name: mydemosite
    + Getting sites
    + Getting site information
    data:    Certificate hostNames 0=msopentech.azurewebsites.net
    data:    Certificate expirationDate
    data:    Certificate friendlyName msopentech.azurewebsites.net
    data:    Certificate issueDate
    data:    Certificate issuer CN=MSIT Machine Auth CA 2, DC=redmond, DC=corp, DC=microsoft, DC=com
    data:    Certificate subjectName msopentech.azurewebsites.net
    data:    Certificate thumbprint CE1CD65852B38DC32001C2E0E8F7A526A29B541F
    info:    site cert show command OK

###<a name="commands-to-manage-your-web-app-connection-strings"></a>Polecenia do zarządzania parametry połączenia aplikacji sieci web

**Lista connectionstring witryny [opcje] [nazwa]**

**Dodawanie connectionstring witryny [opcje] &lt;connectionname > &lt;wartość > &lt;typu > [nazwa]**

**Usuwanie witryny connectionstring [opcje] &lt;connectionname > [nazwa]**

**Pokazywanie connectionstring witryny [opcje] &lt;connectionname > [nazwa]**

###<a name="commands-to-manage-your-web-app-default-documents"></a>Polecenia do zarządzania dokumentach domyślnej aplikacji sieci web

**Lista defaultdocument witryny [opcje] [nazwa]**

**Witryna defaultdocument add [opcje] &lt;dokumentu > [nazwa]**

**Usuwanie witryny defaultdocument [opcje] &lt;dokumentu > [nazwa]**

###<a name="commands-to-manage-your-web-app-deployments"></a>Polecenia do zarządzania wdrażanie aplikacji sieci web

**Lista wdrażania witryn [opcje] [nazwa]**

**Wdrażanie witryny Pokaż [opcje] &lt;commitId > [nazwa]**

**ponownego wdrażania wdrażania witryny [opcje] &lt;commitId > [nazwa]**

**Witryna wdrażania github [opcje] [nazwa]**

**Użytkownik wdrażania witryny opcji [] [nazwa_użytkownika] [hasło]**

###<a name="commands-to-manage-your-web-app-domains"></a>Polecenia do zarządzania domenami aplikacji sieci web

**Lista domen witryny [opcje] [nazwa]**

**Dodawanie domeny witryny [opcje] &lt;nazwy domeny > [nazwa]**

**Usuwanie domeny witryny [opcje] &lt;nazwy domeny > [nazwa]**

###<a name="commands-to-manage-your-web-app-handler-mappings"></a>Polecenia do zarządzania mapowania obsługi aplikacji sieci web

**Lista obsługi witryn [opcje] [nazwa]**

**Obsługa witryny add [opcje] &lt;rozszerzenie > &lt;procesor > [nazwa]**

**usuwanie obsługi witryny [opcje] &lt;rozszerzenie > [nazwa]**

###<a name="commands-to-manage-your-web-jobs"></a>Polecenia do zarządzania zadaniami usługi sieci Web

**Lista zadań witryny [opcje] [nazwa]**

To polecenie listę zadań w sieci web w obszarze aplikacji sieci web.

To polecenie obsługuje następujące dodatkowe opcje:

+ **— Typ zadania** &lt;typu zadania >: opcjonalne. Typ webjob. Prawidłową wartością jest "wyzwalane" lub "ciągła". Domyślnie zwracają webjobs wszystkich typów.
+ **— przedział** &lt;przedział >: Nazwa przedział go ponownie.

**witryny zadania, Pokaż [opcje] &lt;jobName > &lt;jobType > [nazwa]**

To polecenie pokazuje szczegóły zadania określone w sieci web.

To polecenie obsługuje następujące dodatkowe opcje:

+ **— Nazwa zadania** &lt;Nazwa zadania >: wymagane. Nazwa webjob.
+ **— Typ zadania** &lt;typu zadania >: wymagane. Typ webjob. Prawidłową wartością jest "wyzwalane" lub "ciągłe".
+ **— przedział** &lt;przedział >: Nazwa przedział go ponownie.

**Usuwanie zadania witryny [opcje] &lt;jobName > &lt;jobType > [nazwa]**

To polecenie usuwa zadanie określona witryna sieci web.

To polecenie obsługuje następujące dodatkowe opcje:

+ **— Nazwa zadania** &lt;Nazwa zadania > wymagane. Nazwa webjob.
+ **— Typ zadania** &lt;typu zadania > wymagane. Typ webjob. Prawidłową wartością jest "wyzwalane" lub "ciągłe".
+ **-q** lub **--ciche**: nie Monituj o potwierdzenie. Użyj tej opcji w skrypty automatyczne.
+ **— przedział** &lt;przedział >: Nazwa przedział go ponownie.

**Przekaż zadania witryny [opcje] &lt;jobName > &lt;jobType > <jobFile> [nazwa]**

To polecenie usuwa zadanie określona witryna sieci web.

To polecenie obsługuje następujące dodatkowe opcje:

+ **— Nazwa zadania** &lt;Nazwa zadania >: wymagane. Nazwa webjob.
+ **— Typ zadania** &lt;typu zadania >: wymagane. Typ webjob. Prawidłową wartością jest "wyzwalane" lub "ciągła".
+ **— Plik zadania** &lt;plik zadania >: wymagane. Plik zadania.
+ **— przedział** &lt;przedział >: Nazwa przedział go ponownie.

**witryny rozpoczęcia zadania [opcje] &lt;jobName > &lt;jobType > [nazwa]**

To polecenie uruchamia zadanie określona witryna sieci web.

To polecenie obsługuje następujące dodatkowe opcje:

+ **— Nazwa zadania** &lt;Nazwa zadania >: wymagane. Nazwa webjob.
+ **— Typ zadania** &lt;typu zadania >: wymagane. Typ webjob. Prawidłową wartością jest "wyzwalane" lub "ciągłe".
+ **— przedział** &lt;przedział >: Nazwa przedział go ponownie.

**Zatrzymaj zadanie witryny [opcje] &lt;jobName > &lt;jobType > [nazwa]**

To polecenie zatrzymuje zadania określona witryna sieci web. Tylko ciągłe zadania można je zatrzymać.

To polecenie obsługuje następujące dodatkowe opcje:

+ **— Nazwa zadania** &lt;Nazwa zadania >: wymagane. Nazwa webjob.
+ **— przedział** &lt;przedział >: Nazwa przedział go ponownie.

###<a name="commands-to-manage-your-web-jobs-history"></a>Polecenia do zarządzania historii zadań w sieci Web

**Lista historii zadań witryny [opcje] [nazwa_zadania] [nazwa]**

To polecenie wyświetla historię uruchomień zadania określona witryna sieci web.

To polecenie obsługuje następujące dodatkowe opcje:

+ **— Nazwa zadania** &lt;Nazwa zadania >: wymagane. Nazwa webjob.
+ **— przedział** &lt;przedział >: Nazwa przedział go ponownie.

**Wyświetlanie historii zadań witryny [opcje] [nazwa_zadania] [runId] [nazwa]**

To polecenie pobiera szczegółowe informacje o zadaniu uruchamianie zadania określona witryna sieci web.

To polecenie obsługuje następujące dodatkowe opcje:

+ **— Nazwa zadania** &lt;Nazwa zadania >: wymagane. Nazwa webjob.
+ **— Uruchom identyfikator** &lt;identyfikator Uruchom >: opcjonalne. Identyfikator historii Uruchom. Jeśli nie zostanie określony, wyświetlane najnowsze uruchamianie.
+ **— przedział** &lt;przedział >: Nazwa przedział go ponownie.

###<a name="commands-to-manage-your-web-app-diagnostics"></a>Polecenia do zarządzania diagnostyki aplikacji sieci web

**Dziennik witryny Pobieranie [opcje] [nazwa]**

Pobierz plik zip zawierający diagnostyki aplikacji sieci web.

    ~$ azure site log download
    info:    Executing command site log download
    Web site name: mydemosite
    + Getting sites
    + Getting site information
    + Downloading diagnostic log to diagnostics.zip
    info:    site log download command OK

**Zakończenie dziennika witryny [opcje] [nazwa]**

To polecenie łączy usługi terminalowe usługa strumieniowego przesyłania dziennika.

    ~$ azure site log tail
    info:    Executing command site log tail
    Web site name: mydemosite
    + Getting sites
    + Getting site information
    2013-11-19T17:24:17  Welcome, you are now connected to log-streaming service.

**Dziennik witryny opcji [] [nazwa]**

To polecenie konfiguruje diagnostyczne opcje dla aplikacji sieci web.

    ~$ azure site log set -a
    info:    Executing command site log set
    + Getting output options
    help:    Output:
      1) file
      2) storage
      : 1
    Web site name: mydemosite
    + Getting locations
    + Getting sites
    + Getting site information
    + Getting diagnostic settings
    + Updating diagnostic settings
    info:    site log set command OK

###<a name="commands-to-manage-your-web-app-repositories"></a>Polecenia do zarządzania repozytoria aplikacji sieci web

**gałąź repozytorium witryny [opcje] &lt;gałąź > [nazwa]**

**Usuwanie witryny repozytorium [opcje] [nazwa]**

**synchronizowanie witryny repozytorium [opcje] [nazwa]**

###<a name="commands-to-manage-your-web-app-scaling"></a>Polecenia do zarządzania skalowania aplikacji sieci web

**tryb Skala witryn [opcje] &lt;Tryb > [nazwa]**

**Skala witryny wystąpienia [opcje] &lt;wystąpienia > [nazwa]**


## <a name="commands-to-manage-azure-mobile-services"></a>Polecenia do zarządzania usługi Azure Mobile

Usług Azure Mobile zgromadzono zbiór usług Azure, które pozwalają możliwości wewnętrznej bazy danych dla aplikacji. Polecenia usług Mobile są podzielone na następujące kategorie:

+ [Polecenia do zarządzania wystąpieniami usługi mobilnej](#Mobile_Services)
+ [Polecenia do zarządzania konfiguracją usługi mobilnej](#Mobile_Configuration)
+ [Polecenia do zarządzania tabel usługi mobilnej](#Mobile_Tables)
+ [Polecenia do zarządzania skryptów usługi mobilnej](#Mobile_Scripts)
+ [Polecenia do zarządzania zaplanowanych zadań](#Mobile_Jobs)
+ [Polecenia do skalowania usług mobilnych](#Mobile_Scale)

Poniższe opcje mają zastosowanie do większości poleceń usług Mobile:

+ **-h** lub **— Pomoc**: dane wyjściowe informacje dotyczące użycia.
+ **-s `<id>` ** lub **— subskrypcji `<id>` **: używanie określoną subskrypcję, określony jako `<id>`.
+ **-v** lub **— pełne**: pisanie pełne dane wyjściowe.
+ **— json**: pisanie JSON dane wyjściowe.

### <a name="Mobile_Services"></a>Polecenia do zarządzania wystąpieniami usługi mobilnej

**lokalizacje przenośnych [opcje]**

To polecenie wyświetla lokalizacje geograficzne obsługiwane przez usługi Mobile.

    ~$ azure mobile locations
    info:    Executing command mobile locations
    info:    East US (default)
    info:    West US
    info:    North Europe

**Telefon komórkowy tworzenie [opcje] [nazwa_usługi] [sqlAdminUsername] [sqlAdminPassword]**

To polecenie tworzy usług mobilnych wraz z serwera i bazy danych SQL.

    ~$ azure mobile create todolist your_login_name Secure$Password
    info:    Executing command mobile create
    + Creating mobile service
    info:    Overall application state: Healthy
    info:    Mobile service (todolist) state: ProvisionConfigured
    info:    SQL database (todolist_db) state: Provisioned
    info:    SQL server (e96ean1c6v) state: ProvisionConfigured
    info:    mobile create command OK

To polecenie obsługuje następujące dodatkowe opcje:

+ **- r `<sqlServer>` ** lub **— SQL `<sqlServer>` **: Użyj istniejącego serwera bazy danych SQL, określony jako `<sqlServer>`.
+ **-d `<sqlDb>` ** lub **— sqlDb `<sqlDb>` **: Użyj istniejącej bazy danych SQL, określony jako `<sqlDb>`.
+ **-l `<location>` ** lub **— lokalizacji `<location>` **: Tworzenie usługi w konkretnym miejscu, określony jako `<location>`. Uruchom azure przenośnych lokalizacje, aby uzyskać dostępne lokalizacje.
+ **— sqlLocation `<location>` **: tworzenie programu SQL server w określonym `<location>`; Domyślnie do lokalizacji usługi mobilnej.

**usuwanie urządzeń przenośnych [opcje] [nazwa_usługi]**

To polecenie usuwa usług mobilnych wraz z serwera i bazy danych SQL.

    ~$ azure mobile delete todolist -a -q
    info:    Executing command mobile delete
    data:    Mobile service todolist
    data:    SQL database todolistAwrhcL60azo1C401
    data:    SQL server fh1kvbc7la
    + Deleting mobile service
    info:    Deleted mobile service
    + Deleting SQL server
    info:    Deleted SQL server
    + Deleting mobile application
    info:    Deleted mobile application
    info:    mobile delete command OK

To polecenie obsługuje następujące dodatkowe opcje:

+ **-d** lub **- deleteData**: usuwanie wszystkich danych z tej usługi mobilnej z bazy danych.
+ **** lub **— deleteAll**: usuwanie bazy danych SQL i serwera.
+ **-q** lub **--ciche**: nie Monituj o potwierdzenie. Użyj tej opcji w skrypty automatyczne.

**listy dla urządzeń przenośnych [opcje]**

To polecenie wyświetla usługi urządzeń przenośnych.

    ~$ azure mobile list
    info:    Executing command mobile list
    data:    Name          State  URL
    data:    ------------  -----  --------------------------------------
    data:    todolist      Ready  https://todolist.azure-mobile.net/
    data:    mymobileapp   Ready  https://mymobileapp.azure-mobile.net/
    info:    mobile list command OK

**Pokaż przenośnych [opcje] [nazwa_usługi]**

To polecenie zawiera szczegóły dotyczące usług mobilnych.

    ~$ azure mobile show todolist
    info:    Executing command mobile show
    + Getting information
    info:    Mobile application
    data:    status Healthy
    data:    Mobile service name todolist
    data:    Mobile service status ProvisionConfigured
    data:    SQL database name todolistAwrhcL60azo1C401
    data:    SQL database status Linked
    data:    SQL server name fh1kvbc7la
    data:    SQL server status Linked
    info:    Mobile service
    data:    name todolist
    data:    state Ready
    data:    applicationUrl https://todolist.azure-mobile.net/
    data:    applicationKey XXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
    data:    masterKey XXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
    data:    webspace WESTUSWEBSPACE
    data:    region West US
    data:    tables TodoItem
    info:    mobile show command OK

**ponowne uruchomienie komputera przenośnego [opcje] [nazwa_usługi]**

To polecenie ponowne uruchomienie wystąpienia usług mobilnych.

    ~$ azure mobile restart todolist
    info:    Executing command mobile restart
    + Restarting mobile service
    info:    Service was restarted.
    info:    mobile restart command OK

**Dziennik przenośnych [opcje] [nazwa_usługi]**

To polecenie zwraca dzienniki usług mobilnych, odfiltrowywać wszystkie typy dziennika, ale `error`.

    ~$ azure mobile log todolist -t error
    info:    Executing command mobile log
    data:
    data:    timeCreated 2013-01-07T16:04:43.351Z
    data:    type error
    data:    source /scheduler/TestingLogs.js
    data:    message This is an error.
    data:
    info:    mobile log command OK

To polecenie obsługuje następujące dodatkowe opcje:

+ **- r `<query>` ** lub **— kwerendy `<query>` **: wykonuje kwerendę określonego dziennika.
+ **-t `<type>` ** lub **— typ `<type>` **: filtrowanie dziennikach zwracane przez wpis `<type>`, które mogą być `information`, `warning`, lub `error`.
+ **-k `<skip>` ** lub **— pominąć `<skip>` **: pomija liczbę wierszy określony przez `<skip>`.
+ **-p `<top>` ** lub **— góry `<top>` **: zwraca określoną liczbę wierszy, określone przez `<top>`.

> [AZURE.NOTE] **— Kwerendy** parametr ma pierwszeństwo przed **— Typ**, **— pominąć**, i **— góry**.

**odzyskiwanie urządzeń przenośnych [opcje] [unhealthyservicename] [healthyservicename]**

To polecenie odzyskuje nieprawidłowe usług mobilnych przez przeniesienie go do prawidłowy usługi mobilnej w innym regionie.

To polecenie obsługuje następujących dodatkowych opcji:

**-q** lub **--ciche**: Pomiń Monituj o potwierdzenie odzyskiwania.

**[opcje] [nazwa_usługi] [typ] ponownie wygenerować klucz urządzeń przenośnych**

To polecenie generuje klawisz aplikacji usług mobilnych.

    ~$ azure mobile key regenerate todolist application
    info:    Executing command mobile key regenerate
    info:    New application key is SmLorAWVfslMcOKWSsuJvuzdJkfUpt40
    info:    mobile key regenerate command OK

Główne typy są `master` i `application`.

> [AZURE.NOTE] Podczas generowania klawiszy, klienci, którzy używają starego klucza może być nie może uzyskać dostępu do usług mobilnych. Podczas ponownego generowania klawisz aplikacji, należy zaktualizować aplikacji przy użyciu nowej wartości klucza.

**Ustawianie klucza przenośnych [opcje] [nazwa_usługi] [typ] [wartość]**

To polecenie ustawia klucz usług mobilnych na określoną wartość.


### <a name="Mobile_Configuration"></a>Polecenia do zarządzania konfiguracją usługi mobilnej

**Lista konfiguracji urządzeń przenośnych [opcje] [nazwa_usługi]**

To polecenie wyświetla opcje konfiguracji dla usług mobilnych.

    ~$ azure mobile config list todolist
    info:    Executing command mobile config list
    + Getting mobile service configuration
    data:    dynamicSchemaEnabled true
    data:    microsoftAccountClientSecret Not configured
    data:    microsoftAccountClientId Not configured
    data:    microsoftAccountPackageSID Not configured
    data:    facebookClientId Not configured
    data:    facebookClientSecret Not configured
    data:    twitterClientId Not configured
    data:    twitterClientSecret Not configured
    data:    googleClientId Not configured
    data:    googleClientSecret Not configured
    data:    apnsMode none
    data:    apnsPassword Not configured
    data:    apnsCertifcate Not configured
    info:    mobile config list command OK

**Uzyskiwanie konfiguracji urządzeń przenośnych [opcje] [nazwa_usługi] [klawisz]**

To polecenie pobiera z określonej konfiguracji opcji dla urządzeń przenośnych usługi, w tym przypadku dynamiczne schematu.

    ~$ azure mobile config get todolist dynamicSchemaEnabled
    info:    Executing command mobile config get
    data:    dynamicSchemaEnabled true
    info:    mobile config get command OK

**Ustawianie konfiguracji urządzeń przenośnych [opcje] [nazwa_usługi] [klawisz] [wartość]**

To polecenie ustawia opcję określonej konfiguracji dla urządzeń przenośnych usługi, w tym przypadku dynamiczne schematu.

    ~$ azure mobile config set todolist dynamicSchemaEnabled false
    info:    Executing command mobile config set
    info:    mobile config set command OK


### <a name="Mobile_Tables"></a>Polecenia do zarządzania tabel usługi mobilnej

**Lista urządzeń przenośnych tabeli [opcje] [nazwa_usługi]**

To polecenie wyświetla wszystkie tabele w sieci komórkowej.

    ~$azure mobile table list todolist
    info:    Executing command mobile table list
    data:    Name      Indexes  Rows
    data:    --------  -------  ----
    data:    Channel   1        0
    data:    TodoItem  1        0
    info:    mobile table list command OK

**Pokaż tabelę przenośnych [opcje] [nazwa_usługi] [nazwa_tabeli]**

To polecenie pokazuje Zwraca szczegóły dotyczące określonej tabeli.

    ~$azure mobile table show todolist
    info:    Executing command mobile table show
    + Getting table information
    info:    Table statistics:
    data:    Number of records 5
    info:    Table operations:
    data:    Operation  Script       Permissions
    data:    ---------  -----------  -----------
    data:    insert     1900 bytes   user
    data:    read       Not defined  user
    data:    update     Not defined  user
    data:    delete     Not defined  user
    info:    Table columns:
    data:    Name  Type           Indexed
    data:    ----  -------------  -------
    data:    id    bigint(MSSQL)  Yes
    data:    text      string
    data:    complete  boolean
    info:    mobile table show command OK

**Tworzenie tabeli przenośnych [opcje] [nazwa_usługi] [nazwa_tabeli]**

To polecenie tworzy tabelę.

    ~$azure mobile table create todolist Channels
    info:    Executing command mobile table create
    + Creating table
    info:    mobile table create command OK

To polecenie obsługuje następujących dodatkowych opcji:

+ **-p `&lt;permissions>` ** lub **— uprawnienia `&lt;permissions>` **: Lista rozdzielany przecinkami `<operation>` = `<permission>` pary, gdzie `<operation>` jest `insert`, `read`, `update`, lub `delete` i `&lt;permissions>` jest `public`, `application` (ustawienie domyślne), `user`, lub `admin`.

**Odczytywanie danych mobilnych [opcje] [nazwa_usługi] [nazwa_tabeli] [query]**

To polecenie odczytuje dane z tabeli.

    ~$azure mobile data read todolist TodoItem
    info:    Executing command mobile data read
    data:    id  text     complete
    data:    --  -------  --------
    data:    1   item #1  false
    data:    2   item #2  true
    data:    3   item #3  false
    data:    4   item #4  true
    info:    mobile data read command OK

To polecenie obsługuje następujące dodatkowe opcje:

+ **-k `<skip>` ** lub **— pominąć `<skip>` **: pomija liczbę wierszy określony przez `<skip>`.
+ **-t `<top>` ** lub **— góry `<top>` **: zwraca określoną liczbę wierszy, określone przez `<top>`.
+ **-l** lub **— Lista**: zwraca dane w formie listy.

**Aktualizacja tabeli przenośnych [opcje] [nazwa_usługi] [nazwa_tabeli]**

To polecenie zmienia uprawnienia do usuwania w tabeli, aby tylko Administratorzy.

    ~$azure mobile table update todolist Channels -p delete=admin
    info:    Executing command mobile table update
    + Updating permissions
    info:    Updated permissions
    info:    mobile table update command OK

To polecenie obsługuje następujące dodatkowe opcje:

+ **-p `&lt;permissions>` ** lub **— uprawnienia `&lt;permissions>` **: Lista rozdzielany przecinkami `<operation>` = `<permission>` pary, gdzie `<operation>` jest `insert`, `read`, `update`, lub `delete` i `&lt;permissions>` jest `public`, `application` (ustawienie domyślne), `user`, lub `admin`.
+ **— deleteColumn `<columns>` **: plik rozdzielany przecinkami listy kolumn, aby usunąć, jako `<columns>`.
+ **-q** lub **--ciche**: usuwa kolumny bez monitowania o potwierdzenie.
+ **— addIndex `<columns>` **: plik rozdzielany przecinkami listy kolumn do uwzględnienia w indeksie.
+ **— deleteIndex `<columns>` **: plik rozdzielany przecinkami listy kolumn, aby wykluczyć z indeksu.

**Usuwanie tabeli przenośnych [opcje] [nazwa_usługi] [nazwa_tabeli]**

To polecenie usuwa tabeli.

    ~$azure mobile table delete todolist Channels
    info:    Executing command mobile table delete
    Do you really want to delete the table (yes/no): yes
    + Deleting table
    info:    mobile table delete command OK

Określ parametr - q, aby usunąć tabelę bez potwierdzenia. W tym celu zapobieżenia blokowania skryptów automatyzacji.

**danych mobilnych obciąć [opcje] [nazwa_usługi] [nazwa_tabeli]**

Tego polecenia spowoduje usunięcie wszystkich wierszy danych z tabeli.

    ~$azure mobile data truncate todolist TodoItem
    info:    Executing command mobile data truncate
    info:    There are 7 data rows in the table.
    Do you really want to delete all data from the table? (y/n): y
    info:    Deleted 7 rows.
    info:    mobile data truncate command OK


### <a name="Mobile_Scripts"></a>Polecenia do zarządzania skryptów

Polecenia w tej sekcji są używane do zarządzania skrypty serwera, które należą do usługi mobilnej. Aby uzyskać więcej informacji zobacz [Praca z serwera skryptów w usługach Mobile](https://github.com/Azure/azure-mobile-services/blob/master/docs/mobile-services-how-to-use-server-scripts.md).

**Lista urządzeń przenośnych skrypt [opcje] [nazwa_usługi]**

To polecenie wyświetla zarejestrowanych skrypty, w tym zarówno w tabeli i harmonogram skryptów.

    ~$azure mobile script list todolist
    info:    Executing command mobile script list
    + Getting script information
    info:    Table scripts
    data:    Name                   Size
    data:    ---------------------  ----
    data:    table/TodoItem.delete  256
    data:    table/Devices.insert   1660
    error:   Unable to get shared scripts
    info:    Scheduler scripts
    data:    Name                 Status     Interval   Last run   Next run
    data:    -------------------  ---------  ---------  ---------  ---------
    data:    scheduler/undefined  undefined  undefined  undefined  undefined
    data:    scheduler/undefined  undefined  undefined  undefined  undefined
    info:    mobile script list command OK

**Pobieranie skryptu przenośnych [opcje] [nazwa_usługi] [NazwaSkryptu]**

To polecenie do pobrania Wstaw skrypt z tabeli TodoItem w pliku o nazwie `todoitem.insert.js` w `table` podfolderu.

    ~$azure mobile script download todolist table/todoitem.insert.js
    info:    Executing command mobile script download
    info:    Saved script to ./table/todoitem.insert.js
    info:    mobile script download command OK

To polecenie obsługuje następujące dodatkowe opcje:

+ **-p `<path>` ** lub **— ścieżka `<path>` **: lokalizacji w pliku, w którym chcesz zapisać skrypt, miejsce, w którym bieżącego katalogu roboczego to ustawienie domyślne.
+ **-f `<file>` ** lub **— plik `<file>` **: nazwę pliku, w którym chcesz zapisać skrypt.
+ **-o** lub **— zastępuje**: Zastąp istniejące pliki.
+ **-c** lub **— konsoli**: napisać skrypt do konsoli zamiast do pliku.

**Przekaż skrypt przenośnych [opcje] [nazwa_usługi] [NazwaSkryptu]**

To polecenie wysyła skrypt o nazwie `todoitem.insert.js` z `table` podfolderu.

    ~$azure mobile script upload todolist table/todoitem.insert.js
    info:    Executing command mobile script upload
    info:    mobile script upload command OK

Nazwa pliku musi się składać z nazw tabel i operacji. Musi znajdować się w podfolderze tabeli względem lokalizacji, w którym polecenie jest wykonywane. Można również użyć **-f `<file>` ** lub **— plik `<file>` ** parametr, aby określić inną nazwę pliku i ścieżkę do pliku, który zawiera skrypt do rejestrowania.


**usuwanie urządzeń przenośnych skrypt [opcje] [nazwa_usługi] [NazwaSkryptu]**

To polecenie usuwa istniejący skrypt wstawianie z tabeli TodoItem.

    ~$azure mobile script delete todolist table/todoitem.insert.js
    info:    Executing command mobile script delete
    info:    mobile script delete command OK

### <a name="Mobile_Jobs"></a>Polecenia do zarządzania zaplanowanych zadań

Polecenia w tej sekcji są używane do zarządzania zaplanowanych zadań, które należą do usługi mobilnej. Aby uzyskać więcej informacji zobacz [Planowanie zadań](http://msdn.microsoft.com/library/windowsazure/jj860528.aspx).

**Lista zadań przenośnych [opcje] [nazwa_usługi]**

To polecenie wyświetla zaplanowanych zadań.

    ~$azure mobile job list todolist
    info:    Executing command mobile job list
    info:    Scheduled jobs
    data:    Job name    Script name           Status    Interval     Last run              Next run
    data:    ----------  --------------------  --------  -----------  --------------------  --------------------
    data:    getUpdates  scheduler/getUpdates  enabled   15 [minute]  2013-01-14T16:15:00Z  2013-01-14T16:30:00Z
    info:    You can manipulate scheduled job scripts using the 'azure mobile script' command.
    info:    mobile job list command OK

**Tworzenie zadania przenośnych [opcje] [nazwa_usługi] [nazwa_zadania]**

To polecenie tworzy zadanie o nazwie `getUpdates` który zaplanowano uruchamianie co godzinę.

    ~$azure mobile job create -i 1 -u hour todolist getUpdates
    info:    Executing command mobile job create
    info:    Job was created in disabled state. You can enable the job using the 'azure mobile job update' command.
    info:    You can manipulate the scheduled job script using the 'azure mobile script' command.
    info:    mobile job create command OK

To polecenie obsługuje następujące dodatkowe opcje:

+ **-i `<number>` ** lub **— interwał `<number>` **: interwału zadania, w postaci liczby całkowitej. Wartość domyślna to `15`.
+ **-u `<unit>` ** lub **— intervalUnit `<unit>` **: jednostkę _interwału_, który może mieć jedną z następujących wartości:
    + **minuta** (ustawienie domyślne)
    + **Godzina**
    + **dzień**
    + **miesiąc**
    + **Brak** (na żądanie zadania)
+ **-t `<time>`** **— czas rozpoczęcia `<time>` ** Godzina rozpoczęcia pierwszego uruchomienia skryptu w formacie ISO. Wartość domyślna to `now`.

> [AZURE.NOTE] Nowe zadania są tworzone w stanu wyłączenia, ponieważ nadal należy przekazać skrypt. Przekazywanie skryptu oraz polecenie **Aktualizuj zadania urządzeń przenośnych** , aby włączyć zadania za pomocą polecenia **Przekaż skrypt urządzeń przenośnych** .

**Aktualizacja zadania przenośnych [opcje] [nazwa_usługi] [nazwa_zadania]**

Następujące polecenie włącza osób niepełnosprawnych `getUpdates` zadania.

    ~$azure mobile job update -a enabled todolist getUpdates
    info:    Executing command mobile job update
    info:    mobile job update command OK

To polecenie obsługuje następujące dodatkowe opcje:

+ **-i `<number>` ** lub **— interwał `<number>` **: interwału zadania, w postaci liczby całkowitej. Wartość domyślna to `15`.
+ **-u `<unit>` ** lub **— intervalUnit `<unit>` **: jednostkę _interwału_, który może mieć jedną z następujących wartości:
    + **minuta** (ustawienie domyślne)
    + **Godzina**
    + **dzień**
    + **miesiąc**
    + **Brak** (na żądanie zadania)
+ **-t `<time>`** **— czas rozpoczęcia `<time>` ** Godzina rozpoczęcia pierwszego uruchomienia skryptu w formacie ISO. Wartość domyślna to `now`.
+ ** `<status>` ** lub **— Stan `<status>` **: stan zadania, które mogą być `enabled` lub `disabled`.

**Usuń zadanie przenośnych [opcje] [nazwa_usługi] [nazwa_zadania]**

To polecenie usuwa zaplanowane zadanie getUpdates z serwera listy zadań.

    ~$azure mobile job delete todolist getUpdates
    info:    Executing command mobile job delete
    info:    mobile job delete command OK

> [AZURE.NOTE] Usunięcie zadania powoduje usunięcie przekazany skrypt.

### <a name="Mobile_Scale"></a>Polecenia do skalowania usług mobilnych

Polecenia w tej sekcji są używane do skalowania usług mobilnych. Aby uzyskać więcej informacji zobacz [Skalowanie usług mobilnych](http://msdn.microsoft.com/library/windowsazure/jj193178.aspx).

**Skala przenośnych Pokaż [opcje] [nazwa_usługi]**

To polecenie wyświetla informacje o skali, w tym bieżący tryb obliczeń i liczbę wystąpień.

    ~$azure mobile scale show todolist
    info:    Executing command mobile scale show
    data:    webspace WESTUSWEBSPACE
    data:    computeMode Free
    data:    numberOfInstances 1
    info:    mobile scale show command OK

**Zmienianie skali przenośnych [opcje] [nazwa_usługi]**

To polecenie zmienia skali usług mobilnych z bezpłatnej do trybu premium.

    ~$azure mobile scale change -c Reserved -i 1 todolist
    info:    Executing command mobile scale change
    + Rescaling the mobile service
    info:    mobile scale change command OK

To polecenie obsługuje następujące dodatkowe opcje:

+ **- c `<mode>` ** lub **— computeMode `<mode>` **: tryb obliczeń musi być `Free` lub `Reserved`.
+ **-i `<count>` ** lub **— numberOfInstances `<count>` **: liczba wystąpień używane podczas pracy w trybie zastrzeżone.

> [AZURE.NOTE] Gdy tryb obliczeń należy ustawić na `Reserved`, wszystkich usług mobilnych w tym samym regionie Uruchom w trybie premium.


###<a name="commands-to-enable-preview-features-for-your-mobile-service"></a>Polecenia do włączania funkcji podglądu tej usługi Mobile

**listy urządzeń przenośnych podglądu [opcje] [nazwa_usługi]**

To polecenie wyświetla dostępne funkcje podglądu na określonej usługi i czy są włączone.

    ~$ azure mobile preview list mysite
    info:    Executing command mobile preview list
    + Getting preview features
    data:    Preview feature  Enabled
    data:    ---------------  -------
    data:    SourceControl    No
    data:    Users            No
    info:    You can enable preview features using the 'azure mobile preview enable' command.
    info:    mobile preview list command OK

**[opcje] [nazwa_usługi] [featurename] Włącz podgląd urządzeń przenośnych**

To polecenie włącza funkcję podglądu określonego dla urządzeń przenośnych usługi. Po włączeniu funkcji preview nie można wyłączyć dla urządzeń przenośnych usługi.

###<a name="commands-to-manage-your-mobile-service-apis"></a>Polecenia do zarządzania interfejsów API usługi urządzeń przenośnych

**listy dla urządzeń przenośnych interfejsu api [opcje] [nazwa_usługi]**

To polecenie wyświetla listę urządzeń przenośnych usługi niestandardowe interfejsów API, które zostały utworzone dla urządzeń przenośnych usługi.

    ~$ azure mobile api list mysite
    info:    Executing command mobile api list
    + Retrieving list of APIs
    info:    APIs
    data:    Name                  Get          Put          Post         Patch        Delete
    data:    --------------------  -----------  -----------  -----------  -----------  -----------
    data:    myCustomRetrieveAPI   application  application  application  application  application
    info:    You can manipulate API scripts using the 'azure mobile script' command.
    info:    mobile api list command OK

**Interfejs api przenośnych tworzenie [opcje] [nazwa_usługi] [NAZWA_FUNKCJI_API]**

Tworzy interfejs API niestandardowej usługi mobilnej

    ~$ azure mobile api create mysite myCustomRetrieveAPI
    info:    Executing command mobile api create
    + Creating custom API: 'myCustomRetrieveAPI'
    info:    API was created successfully. You can modify the API using the 'azure mobile script' command.
    info:    mobile api create command OK

To polecenie obsługuje następujących dodatkowych opcji:

**-p** lub **— uprawnienia** &lt;uprawnienia >: plik rozdzielany przecinkami listę &lt;metody > =&lt;uprawnień > pary.

**Aktualizacja interfejsu api przenośnych [opcje] [nazwa_usługi] [NAZWA_FUNKCJI_API]**

To polecenie aktualizuje niestandardowego interfejsu API określonych usług mobilnych.

To polecenie obsługuje następujących dodatkowych opcji:

To polecenie obsługuje następujące dodatkowe opcje:

+ **-p** lub **— uprawnienia** &lt;uprawnienia >: plik rozdzielany przecinkami listę &lt;metody > =&lt;uprawnień > pary.
+ **-f** lub **— Wymuszanie**: zastępują wszelkie niestandardowe zmiany w pliku metadanych uprawnienia.

**usuwanie urządzeń przenośnych interfejsu api [opcje] [nazwa_usługi] [NAZWA_FUNKCJI_API]**

    ~$ azure mobile api delete mysite myCustomRetrieveAPI
    info:    Executing command mobile api delete
    + Deleting API: 'myCustomRetrieveAPI'
    info:    mobile api delete command OK

To polecenie usuwa niestandardowego interfejsu API określonych usług mobilnych.

###<a name="commands-to-manage-your-mobile-application-app-settings"></a>Polecenia do zarządzania ustawieniami aplikacji aplikacji mobilnej

**Lista urządzeń przenośnych appsetting [opcje] [nazwa_usługi]**

To polecenie wyświetla ustawienia aplikacji aplikacji mobilnej dla określonej usługi.

    ~$ azure mobile appsetting list mysite
    info:    Executing command mobile appsetting list
    + Retrieving app settings
    data:    Name               Value
    data:    -----------------  -----
    data:    enablebetacontent  true
    info:    mobile appsetting list command OK

**Dodawanie urządzeń przenośnych appsetting [opcje] [nazwa_usługi] [nazwa] [wartość]**

To polecenie dodaje ustawienie niestandardowej aplikacji dla urządzeń przenośnych usługi.

    ~$ azure mobile appsetting add mysite enablebetacontent true
    info:    Executing command mobile appsetting add
    + Retrieving app settings
    + Adding app setting
    info:    mobile appsetting add command OK

**usuwanie urządzeń przenośnych appsetting [opcje] [nazwa_usługi] [nazwa]**

To polecenie usuwa ustawienie określonej aplikacji dla urządzeń przenośnych usługi.

    ~$ azure mobile appsetting delete mysite enablebetacontent
    info:    Executing command mobile appsetting delete
    + Retrieving app settings
    + Removing app setting 'enablebetacontent'
    info:    mobile appsetting delete command OK

**Pokaż przenośnych appsetting [opcje] [nazwa_usługi] [nazwa]**

To polecenie usuwa ustawienie określonej aplikacji dla urządzeń przenośnych usługi.

    ~$ azure mobile appsetting show mysite enablebetacontent
    info:    Executing command mobile appsetting show
    + Retrieving app settings
    info:    enablebetacontent: true
    info:    mobile appsetting show command OK

## <a name="manage-tool-local-settings"></a>Zarządzanie ustawieniami lokalne narzędzie

Ustawienia lokalne są identyfikator subskrypcji i domyślne nazwę konta magazynu usługi.

**Lista konfiguracji [opcje]**

To polecenie wyświetla ustawienia konfiguracji.

    ~$ azure config list
    info:   Displaying config settings
    data:   Setting                Value
    data:   ---------------------  ------------------------------------
    data:   subscription           32-digit-subscription-key
    data:   defaultStorageAccount  name

**Ustawianie konfiguracji [opcje] &lt;nazwa&gt;,&lt;wartości&gt;**

To polecenie zmienia ustawienia konfiguracji.

    ~$ azure config set defaultStorageAccount myname
    info:   Setting 'defaultStorageAccount' to value 'myname'
    info:   Changes saved.

## <a name="commands-to-manage-service-bus"></a>Polecenia do zarządzania Bus usługi

Zarządzać swoim kontem usługi Bus za pomocą tych poleceń

**Sprawdź nazw SB [opcje] &lt;nazwa >**

Sprawdź, czy nazw bus usługi jest prawnych i dostępne.

**Tworzenie nazw SB &lt;nazwa > &lt;lokalizacji >**

Tworzy nazw Bus usługi.

    ~$ azure sb namespace create mysbnamespacea-test "West US"
    info:    Executing command sb namespace create
    + Creating namespace mysbnamespacea-test in region West US
    data:    Name: mysbnamespacea-test
    data:    Region: West US
    data:    DefaultKey: fBu8nQ9svPIesFfMFVhCFD+/sY0rRbifWMoRpYy0Ynk=
    data:    Status: Activating
    data:    CreatedAt: 2013-11-14T16:23:29.32Z
    data:    AcsManagementEndpoint: https://mysbnamespacea-test-sb.accesscontrol.windows.net/
    data:    ServiceBusEndpoint: https://mysbnamespacea-test.servicebus.windows.net/

    data:    ConnectionString: Endpoint=sb://mysbnamespacea-test.servicebus.windows.
    net/;SharedSecretIssuer=owner;SharedSecretValue=fBu8nQ9svPIesFfMFVhCFD+/sY0rRbif
    WMoRpYy0Ynk=
    data:    SubscriptionId: 8679c8be3b0549d9b8fb4bd232a48931
    data:    Enabled: true
    data:    _: [object Object]
    info:    sb namespace create command OK


**Usuwanie nazw SB &lt;nazwa >**

Usuwanie nazw.

    ~$ azure sb namespace delete mysbnamespacea-test
    info:    Executing command sb namespace delete
    Delete namespace mysbnamespacea-test? [y/n] y
    + Deleting namespace mysbnamespacea-test
    info:    sb namespace delete command OK

**Lista nazw SB**

Wyświetl wszystkie przestrzenie nazw utworzone dla Twojego konta.

    ~$ azure sb namespace list
    info:    Executing command sb namespace list
    + Getting namespaces
    data:    Name                 Region   Status
    data:    -------------------  -------  ------
    data:    mysbnamespacea-test  West US  Active
    info:    sb namespace list command OK


**Lista lokalizacji nazw SB**

Wyświetlić listę wszystkich nazw dostępne lokalizacje.

    ~$ azure sb namespace location list
    info:    Executing command sb namespace location list
    + Getting locations
    data:    Name              Code
    data:    ----------------  ----------------
    data:    East Asia         East Asia
    data:    West Europe       West Europe
    data:    North Europe      North Europe
    data:    East US           East US
    data:    Southeast Asia    Southeast Asia
    data:    North Central US  North Central US
    data:    West US           West US
    data:    South Central US  South Central US
    info:    sb namespace location list command OK

**Pokazywanie nazw SB &lt;nazwa >**

Wyświetlanie szczegółów dotyczących określonej przestrzeń nazw.

    ~$ azure sb namespace show mysbnamespacea-test
    info:    Executing command sb namespace show
    + Getting namespace
    data:    Name: mysbnamespacea-test
    data:    Region: West US
    data:    DefaultKey: fBu8nQ9svPIesFfMFVhCFD+/sY0rRbifWMoRpYy0Ynk=
    data:    Status: Active
    data:    CreatedAt: 2013-11-14T16:23:29.32Z
    data:    AcsManagementEndpoint: https://mysbnamespacea-test-sb.accesscontrol.windows.net/
    data:    ServiceBusEndpoint: https://mysbnamespacea-test.servicebus.windows.net/

    data:    ConnectionString: Endpoint=sb://mysbnamespacea-test.servicebus.windows.
    net/;SharedSecretIssuer=owner;SharedSecretValue=fBu8nQ9svPIesFfMFVhCFD+/sY0rRbif
    WMoRpYy0Ynk=
    data:    SubscriptionId: 8679c8be3b0549d9b8fb4bd232a48931
    data:    Enabled: true
    data:    UpdatedAt: 2013-11-14T16:25:37.85Z
    info:    sb namespace show command OK

**Sprawdź nazw SB &lt;nazwa >**

Sprawdź, czy obszar nazw jest dostępna.

## <a name="commands-to-manage-your-storage-objects"></a>Polecenia do zarządzania obiekty miejsca do magazynowania

###<a name="commands-to-manage-your-storage-accounts"></a>Polecenia do zarządzania konta miejsca do magazynowania

**listy kont miejsca do magazynowania [opcje]**

To polecenie wyświetla konta miejsca do magazynowania w subskrypcji usługi.

    ~$ azure storage account list
    info:    Executing command storage account list
    + Getting storage accounts
    data:    Name             Label  Location
    data:    ---------------  -----  --------
    data:    mybasestorage           West US
    info:    storage account list command OK

**Pokaż konta miejsca do magazynowania [opcje]<name>**

To polecenie wyświetla informacje o koncie określonego miejsca do magazynowania, łącznie z właściwości identyfikator URI i konta.

**Tworzenie konta miejsca do magazynowania [opcje]<name>**

To polecenie tworzy konto miejsca do magazynowania na podstawie podanej opcji.

    ~$ azure storage account create mybasestorage --label PrimaryStorage --location "West US"
    info:    Executing command storage account create
    + Creating storage account
    info:    storage account create command OK

To polecenie obsługuje następujące dodatkowe opcje:

+ **-e** lub **— Etykieta** &lt;etykiety >: etykietę dla konta miejsca do magazynowania.
+ **-d** lub **— Opis** &lt;opis >: opis konta miejsca do magazynowania.
+ **-l** lub **— lokalizacji** &lt;nazwy >: regionu geograficznego, w której chcesz utworzyć konto miejsca do magazynowania.
+ **** lub **— Grupa koligacji** &lt;nazwy >: Grupa koligacji, z którą chcesz skojarzyć konta miejsca do magazynowania. 
+ **— Typ**: wskazuje typ konta, aby utworzyć: albo standardowy magazyn opcję nadmiarowości (LRS-ZRS-GRS-RAGRS) lub Premium przestrzeni dyskowej (PLRS).

**Konfigurowanie konta miejsca do magazynowania [opcje]<name>**

To polecenie aktualizuje konto określonego miejsca do magazynowania.

    ~$ azure storage account set mybasestorage --kind Storage --sku-name GRS
    info:    Executing command storage account set
    + Updating storage account
    info:    storage account set command OK

To polecenie obsługuje następujące dodatkowe opcje:

+ **-e** lub **— Etykieta** &lt;etykiety >: etykietę dla konta miejsca do magazynowania.
+ **-d** lub **— Opis** &lt;opis >: opis konta miejsca do magazynowania.
+ **-l** lub **— lokalizacji** &lt;nazwy >: regionu geograficznego, w której chcesz utworzyć konto miejsca do magazynowania.
+ **— Typ**: wskazuje nowy typ konta: albo standardowy magazyn opcję nadmiarowości (LRS-ZRS-GRS-RAGRS) lub Premium przestrzeni dyskowej (PLRS).

**Usuń konto miejsca do magazynowania [opcje]<name>**

To polecenie usuwa konto określonego miejsca do magazynowania.

To polecenie obsługuje następujących dodatkowych opcji:

**-q** lub **--ciche**: nie Monituj o potwierdzenie. Użyj tej opcji w skrypty automatyczne.

###<a name="commands-to-manage-your-storage-account-keys"></a>Polecenia do zarządzania kluczy konta miejsca do magazynowania

**[opcje] listy klawiszy konta miejsca do magazynowania<name>**

To polecenie wyświetla klucze głównego i pomocniczego dla konta określonego miejsca do magazynowania.

**klucze konta miejsca do magazynowania odnowić [opcje]<name>**

###<a name="commands-to-manage-your-storage-container"></a>Polecenia do zarządzania do kontenera magazynu

**listy kontenera miejsca do magazynowania [opcje] [prefiks]**

To polecenie wyświetla listę kontenera miejsca do magazynowania dla konta określonego miejsca do magazynowania. Konto miejsca do magazynowania jest określony przez ciąg połączenia lub klucz konta i nazwę konta miejsca do magazynowania.

To polecenie obsługuje następujące dodatkowe opcje:

+ **-p** lub **-Prefiks** &lt;prefiks >: prefiks nazwy kontenera miejsca do magazynowania.
+ **** lub **— nazwę konta** &lt;nazwa konta >: nazwę konta magazynu.
+ **-k** lub **— klucz konta** &lt;accountKey >: klucz konta miejsca do magazynowania.
+ **-c** lub **— Parametry połączenia** &lt;connectionString >: parametry połączenia miejsca do magazynowania.
+ **— Debugowanie**: wykonuje polecenie miejsca do magazynowania w trybie debugowania.

**Pokaż kontenera miejsca do magazynowania [opcje] [kontenera]**
**kontenera magazynu tworzenie [opcje] [kontenera]**

To polecenie tworzy kontenera miejsca do magazynowania dla konta określonego miejsca do magazynowania. Konto miejsca do magazynowania jest określony przez ciąg połączenia lub klucz konta i nazwę konta miejsca do magazynowania.

To polecenie obsługuje następujące dodatkowe opcje:

+ **— kontenera** &lt;kontenera >: nazwa kontenera magazynu, aby utworzyć.
+ **-p** lub **-Prefiks** &lt;prefiks >: prefiks nazwy kontenera miejsca do magazynowania.
+ **** lub **— nazwę konta** &lt;nazwa konta >: nazwę konta magazynu
+ **-k** lub **— klucz konta** &lt;accountKey >: klucz konta miejsca do magazynowania
+ **-c** lub **— Parametry połączenia** &lt;connectionString >: parametry połączenia miejsca do magazynowania
+ **— Debugowanie**: wykonuje polecenie miejsca do magazynowania w trybie debugowania.

**Usuń kontener miejsca do magazynowania [opcje] [kontenera]**

To polecenie usuwa kontenerze określonego miejsca do magazynowania. Konto miejsca do magazynowania jest określony przez ciąg połączenia lub klucz konta i nazwę konta miejsca do magazynowania.

To polecenie obsługuje następujące dodatkowe opcje:

+ **— kontenera** &lt;kontenera >: nazwa kontenera magazynu, aby utworzyć.
+ **-p** lub **-Prefiks** &lt;prefiks >: prefiks nazwy kontenera miejsca do magazynowania.
+ **** lub **— nazwę konta** &lt;nazwa konta >: nazwę konta magazynu.
+ **-k** lub **— klucz konta** &lt;accountKey >: klucz konta miejsca do magazynowania.
+ **-c** lub **— Parametry połączenia** &lt;connectionString >: parametry połączenia miejsca do magazynowania.
+ **— Debugowanie**: wykonuje polecenie miejsca do magazynowania w trybie debugowania.

**kontener miejsca do magazynowania w ustawieniach [] [kontenera]**

To polecenie ustawia listy kontroli dostępu kontenera magazynu. Konto miejsca do magazynowania jest określony przez ciąg połączenia lub klucz konta i nazwę konta miejsca do magazynowania.

To polecenie obsługuje następujące dodatkowe opcje:

+ **— kontenera** &lt;kontenera >: nazwa kontenera magazynu, aby utworzyć.
+ **-p** lub **-Prefiks** &lt;prefiks >: prefiks nazwy kontenera miejsca do magazynowania.
+ **** lub **— nazwę konta** &lt;nazwa konta >: nazwę konta magazynu.
+ **-k** lub **— klucz konta** &lt;accountKey >: klucz konta miejsca do magazynowania.
+ **-c** lub **— Parametry połączenia** &lt;connectionString >: parametry połączenia miejsca do magazynowania.
+ **— Debugowanie**: wykonuje polecenie miejsca do magazynowania w trybie debugowania.

###<a name="commands-to-manage-your-storage-blob"></a>Polecenia do zarządzania usługi Magazyn obiektów blob

**Magazyn obiektów blob listy [opcje] [kontenera] [prefiks]**

To polecenie zwraca listę obiektów blob miejsca do magazynowania w kontenerze określonego miejsca do magazynowania.

To polecenie obsługuje następujące dodatkowe opcje:

+ **— kontenera** &lt;kontenera >: nazwa kontenera magazynu, aby utworzyć.
+ **-p** lub **-Prefiks** &lt;prefiks >: prefiks nazwy kontenera miejsca do magazynowania.
+ **** lub **— nazwę konta** &lt;nazwa konta >: nazwę konta magazynu.
+ **-k** lub **— klucz konta** &lt;accountKey >: klucz konta miejsca do magazynowania.
+ **-c** lub **— Parametry połączenia** &lt;connectionString >: parametry połączenia miejsca do magazynowania.
+ **— Debugowanie**: wykonuje polecenie miejsca do magazynowania w trybie debugowania.

**Magazyn obiektów blob Pokaż [opcje] [kontenera] [obiektów blob]**

To polecenie wyświetla szczegółowe informacje o określony magazyn obiektów blob.

To polecenie obsługuje następujące dodatkowe opcje:

+ **— kontenera** &lt;kontenera >: nazwa kontenera magazynu, aby utworzyć.
+ **-p** lub **-Prefiks** &lt;prefiks >: prefiks nazwy kontenera miejsca do magazynowania.
+ **** lub **— nazwę konta** &lt;nazwa konta >: nazwę konta magazynu.
+ **-k** lub **— klucz konta** &lt;accountKey >: klucz konta miejsca do magazynowania.
+ **-c** lub **— Parametry połączenia** &lt;connectionString >: parametry połączenia miejsca do magazynowania.
+ **— Debugowanie**: wykonuje polecenie miejsca do magazynowania w debugowania.

**Magazyn obiektów blob usuwanie [opcje] [kontenera] [obiektów blob]**

To polecenie obsługuje następujące dodatkowe opcje:

+ **— kontenera** &lt;kontenera >: nazwa kontenera magazynu, aby utworzyć.
+ **-b** lub **— obiektów blob** &lt;blobName >: nazwy obiektów blob przestrzeni dyskowej, aby usunąć.
+ **-q** lub **--ciche**: usuwanie określonych obiektów blob miejsca do magazynowania bez potwierdzenia.
+ **** lub **— nazwę konta** &lt;nazwa konta >: nazwę konta magazynu.
+ **-k** lub **— klucz konta** &lt;accountKey >: klucz konta miejsca do magazynowania.
+ **-c** lub **— Parametry połączenia** &lt;connectionString >: parametry połączenia miejsca do magazynowania.
+ **— Debugowanie**: wykonuje polecenie miejsca do magazynowania w debugowania.

**Magazyn obiektów blob przekazywania [opcje] [plik] [kontenera] [obiektów blob]**

To polecenie przekazywanie pliku do obiektów blob miejsca do magazynowania specified\.

To polecenie obsługuje następujące dodatkowe opcje:

+ **— kontenera** &lt;kontenera >: nazwa kontenera magazynu, aby utworzyć.
+ **-b** lub **— obiektów blob** &lt;blobName >: nazwy obiektów blob przestrzeni dyskowej, aby przekazać.
+ **-t** lub **- blobtype** &lt;blobtype >: typ magazyn obiektów blob: strony lub bloku.
+ **-p** lub **— Właściwości** &lt;właściwości >: właściwości magazyn obiektów blob przekazywanego pliku. Właściwości są klucz = wartość pary s i rozdzielone semicolon(;). Właściwości dostępne są typ zawartości, contentEncoding, contentLanguage i cacheControl.
+ **-m** lub **metadane —** &lt;metadanych >: metadanych obiektów blob miejsca do magazynowania dla przekazywanego pliku. Metadane są klucza = pary wartości d rozdzielone średnikami (;).
+ **— concurrenttaskcount** &lt;concurrenttaskcount >: Maksymalna liczba przekazywania równoczesne żądania.
+ **-q** lub **--ciche**: zastępowanie określonych obiektów blob miejsca do magazynowania bez potwierdzenia.
+ **** lub **— nazwę konta** &lt;nazwa konta >: nazwę konta magazynu.
+ **-k** lub **— klucz konta** &lt;accountKey >: klucz konta miejsca do magazynowania.
+ **-c** lub **— Parametry połączenia** &lt;connectionString >: parametry połączenia miejsca do magazynowania.
+ **— Debugowanie**: wykonuje polecenie miejsca do magazynowania w debugowania.

**Magazyn obiektów blob pobierania [opcje] [kontenera] [obiektów blob] [cel]**

To polecenie pobiera określony magazyn obiektów blob.

To polecenie obsługuje następujące dodatkowe opcje:

+ **— kontenera** &lt;kontenera >: nazwa kontenera magazynu, aby utworzyć.
+ **-b** lub **— obiektów blob** &lt;blobName >: nazwy obiektów blob miejsca do magazynowania.
+ **-d** lub **— miejsce docelowe** [cel]: ścieżkę pliku lub katalogu docelowego pobierania.
+ **-m** lub **— checkmd5**: md5sum wyboru dla pobrany plik.
+ **— concurrenttaskcount** &lt;concurrenttaskcount > maksymalna liczba przekazywania równoczesne żądania
+ **-q** lub **--ciche**: plik docelowy bez potwierdzenia.
+ **** lub **— nazwę konta** &lt;nazwa konta >: nazwę konta magazynu.
+ **-k** lub **— klucz konta** &lt;accountKey >: klucz konta miejsca do magazynowania.
+ **-c** lub **— Parametry połączenia** &lt;connectionString >: parametry połączenia miejsca do magazynowania.
+ **— Debugowanie**: wykonuje polecenie miejsca do magazynowania w debugowania.

## <a name="commands-to-manage-sql-databases"></a>Polecenia do zarządzania bazy danych SQL

Zarządzanie baz danych SQL Azure za pomocą tych poleceń

###<a name="commands-to-manage-sql-servers"></a>Polecenia do zarządzania serwerami SQL.

Te polecenia umożliwia zarządzanie serwery SQL

**Tworzenie programu SQL server &lt;administratorLogin > &lt;administratorPassword > &lt;lokalizacji >**

Tworzenie serwera bazy danych

    ~$ azure sql server create test T3stte$t "West US"
    info:    Executing command sql server create
    + Creating SQL Server
    data:    Server Name i1qwc540ts
    info:    sql server create command OK

**Program SQL server Pokaż &lt;nazwa >**

Wyświetlanie szczegółów serwera.

    ~$ azure sql server show xclfgcndfg
    info:    Executing command sql server show
    + Getting SQL server
    data:    SQL Server Name xclfgcndfg
    data:    SQL Server AdministratorLogin msopentechforums
    data:    SQL Server Location West US
    data:    SQL Server FullyQualifiedDomainName xclfgcndfg.database.windows.net
    info:    sql server show command OK

**Program SQL server listy**

Uzyskiwanie listy serwerów.

    ~$ azure sql server list
    info:    Executing command sql server list
    + Getting SQL server
    data:    Name        Location
    data:    ----------  --------
    data:    xclfgcndfg  West US
    info:    sql server list command OK

**Program SQL server Usuń &lt;nazwa >**

Usuwa serwer

    ~$ azure sql server delete i1qwc540ts
    info:    Executing command sql server delete
    Delete server i1qwc540ts? [y/n] y
    + Removing SQL Server
    info:    sql server delete command OK

###<a name="commands-to-manage-sql-databases"></a>Polecenia do zarządzania bazy danych SQL

Zarządzanie bazami danych programu SQL za pomocą tych poleceń.

**Tworzenie bazy danych SQL [opcje] &lt;nazwa_serwera > &lt;nazwa_bazy_danych > &lt;administratorPassword >**

Tworzy wystąpienie bazy danych

    ~$ azure sql db create fr8aelne00 newdb test
    info:    Executing command sql db create
    Administrator password: ********
    + Creating SQL Server Database
    info:    sql db create command OK

**bazy danych SQL, Pokaż [opcje] &lt;nazwa_serwera > &lt;nazwa_bazy_danych > &lt;administratorPassword >**

Wyświetlanie szczegółów bazy danych.

    C:\windows\system32>azure sql db show fr8aelne00 newdb test
    info:    Executing command sql db show
    Administrator password: ********
    + Getting SQL server databases
    data:    Database _ ContentRootElement=m:properties, id=https://fr8aelne00.datab
    ase.windows.net/v1/ManagementService.svc/Server2('fr8aelne00')/Databases(4), ter
    m=Microsoft.SqlServer.Management.Server.Domain.Database, scheme=http://schemas.m
    icrosoft.com/ado/2007/08/dataservices/scheme, link=[rel=edit, title=Database, hr
    ef=Databases(4), rel=http://schemas.microsoft.com/ado/2007/08/dataservices/relat
    ed/Server, type=application/atom+xml;type=entry, title=Server, href=Databases(4)
    /Server, rel=http://schemas.microsoft.com/ado/2007/08/dataservices/related/Servi
    ceObjective, type=application/atom+xml;type=entry, title=ServiceObjective, href=
    Databases(4)/ServiceObjective, rel=http://schemas.microsoft.com/ado/2007/08/data
    services/related/DatabaseMetrics, type=application/atom+xml;type=entry, title=Da
    tabaseMetrics, href=Databases(4)/DatabaseMetrics, rel=http://schemas.microsoft.c
    om/ado/2007/08/dataservices/related/DatabaseCopies, type=application/atom+xml;ty
    pe=feed, title=DatabaseCopies, href=Databases(4)/DatabaseCopies], title=, update
    d=2013-11-18T19:48:27Z, name=
    data:    Database Id 4
    data:    Database Name newdb
    data:    Database ServiceObjectiveId 910b4fcb-8a29-4c3e-958f-f7ba794388b2
    data:    Database AssignedServiceObjectiveId 910b4fcb-8a29-4c3e-958f-f7ba794388b2
    data:    Database ServiceObjectiveAssignmentState 1
    data:    Database ServiceObjectiveAssignmentStateDescription Complete
    data:    Database ServiceObjectiveAssignmentErrorCode
    data:    Database ServiceObjectiveAssignmentErrorDescription
    data:    Database ServiceObjectiveAssignmentSuccessDate
    data:    Database Edition Web
    data:    Database MaxSizeGB 1
    data:    Database MaxSizeBytes 1073741824
    data:    Database CollationName SQL_Latin1_General_CP1_CI_AS
    data:    Database CreationDate
    data:    Database RecoveryPeriodStartDate
    data:    Database IsSystemObject
    data:    Database Status 1
    data:    Database IsFederationRoot
    data:    Database SizeMB -1
    data:    Database IsRecursiveTriggersOn
    data:    Database IsReadOnly
    data:    Database IsFederationMember
    data:    Database IsQueryStoreOn
    data:    Database IsQueryStoreReadOnly
    data:    Database QueryStoreMaxSizeMB
    data:    Database QueryStoreFlushPeriodSeconds
    data:    Database QueryStoreIntervalLengthMinutes
    data:    Database QueryStoreClearAll
    data:    Database QueryStoreStaleQueryThresholdDays
    info:    sql db show command OK

**Lista bazy danych SQL [opcje] &lt;nazwa_serwera > &lt;administratorPassword >**

Lista baz danych.

    ~$ azure sql db list fr8aelne00 test
    info:    Executing command sql db list
    Administrator password: ********
    + Getting SQL server databases
    data:    Name    Edition  Collation                     MaxSizeInGB
    data:    ------  -------  ----------------------------  -----------
    data:    master  Web      SQL_Latin1_General_CP1_CI_AS  5
    info:    sql db list command OK

**Usuwanie bazy danych SQL [opcje] &lt;nazwa_serwera > &lt;nazwa_bazy_danych > &lt;administratorPassword >**

Usunięcie bazy danych.

    ~$ azure sql db delete fr8aelne00 newdb test
    info:    Executing command sql db delete
    Administrator password: ********
    Delete database newdb? [y/n] y
    + Getting SQL server databases
    + Removing database
    info:    sql db delete command OK

###<a name="commands-to-manage-your-sql-server-firewall-rules"></a>Polecenia do zarządzania regułami zapory programu SQL Server

Zarządzanie regułami zapory programu SQL Server przy użyciu tych poleceń

**Tworzenie SQL firewallrule [opcje] &lt;nazwa_serwera > &lt;Nazwa_reguły > &lt;startIPAddress > &lt;endIPAddress >**

Tworzenie reguły zapory dla programu SQL Server.

    ~$ azure sql firewallrule create fr8aelne00 allowed 131.107.0.0 131.107.255.255
    info:    Executing command sql firewallrule create
    + Creating Firewall Rule
    info:    sql firewallrule create command OK

**Pokazywanie SQL firewallrule [opcje] &lt;nazwa_serwera > &lt;Nazwa_reguły >**

Pokaż szczegóły reguły zapory.

    ~$ azure sql firewallrule show fr8aelne00 allowed
    info:    Executing command sql firewallrule show
    + Getting firewall rule
    data:    Firewall rule Name allowed
    data:    Firewall rule Type Microsoft.SqlAzure.FirewallRule
    data:    Firewall rule State Normal
    data:    Firewall rule SelfLink https://management.core.windows.net/9e672699-105
    5-41ae-9c36-e85152f2e352/services/sqlservers/servers/fr8aelne00/firewallrules/allowed
    data:    Firewall rule ParentLink https://management.core.windows.net/9e672699-1
    055-41ae-9c36-e85152f2e352/services/sqlservers/servers/fr8aelne00
    data:    Firewall rule StartIPAddress 131.107.0.0
    data:    Firewall rule EndIPAddress 131.107.255.255
    info:    sql firewallrule show command OK

**Lista firewallrule SQL [opcje] &lt;nazwa_serwera >**

Lista reguły zapory.

    ~$ azure sql firewallrule list fr8aelne00
    info:    Executing command sql firewallrule list
    \data:    Name     Start IP address  End IP address
    data:    -------  ----------------  ---------------
    data:    allowed  131.107.0.0       131.107.255.255
    +
    info:    sql firewallrule list command OK

**Usuwanie SQL firewallrule [opcje] &lt;nazwa_serwera > &lt;Nazwa_reguły >**

To polecenie usuwa reguły zapory.

    ~$ azure sql firewallrule delete fr8aelne00 allowed
    info:    Executing command sql firewallrule delete
    Delete rule allowed? [y/n] y
    + Removing firewall rule
    info:    sql firewallrule delete command OK

## <a name="commands-to-manage-your-virtual-networks"></a>Polecenia do zarządzania sieci wirtualnej

Zarządzanie wirtualnej sieci przy użyciu tych poleceń

**Tworzenie vnet sieci [opcje] &lt;lokalizacji >**

Tworzenie wirtualnych sieci.

    ~$ azure network vnet create vnet1 --location "West US" -v
    info:    Executing command network vnet create
    info:    Using default address space start IP: 10.0.0.0
    info:    Using default address space cidr: 8
    info:    Using default subnet start IP: 10.0.0.0
    info:    Using default subnet cidr: 11
    verbose: Address Space [Starting IP/CIDR (Max VM Count)]: 10.0.0.0/8 (16777216)
    verbose: Subnet [Starting IP/CIDR (Max VM Count)]: 10.0.0.0/11 (2097152)
    verbose: Fetching Network Configuration
    verbose: Fetching or creating affinity group
    verbose: Fetching Affinity Groups
    verbose: Fetching Locations
    verbose: Creating new affinity group AG1
    info:    Using affinity group AG1
    verbose: Updating Network Configuration
    info:    network vnet create command OK

**Pokaż vnet sieci &lt;nazwa >**

Pokaż szczegóły wirtualnej sieci.

    ~$ azure network vnet show vnet1
    info:    Executing command network vnet show
    + Fetching Virtual Networks
    data:    Name "vnet1"
    data:    Id "25786fbe-08e8-4e7e-b1de-b98b7e586c7a"
    data:    AffinityGroup "AG1"
    data:    State "Created"
    data:    AddressSpace AddressPrefixes 0 "10.0.0.0/8"
    data:    Subnets 0 Name "subnet-1"
    data:    Subnets 0 AddressPrefix "10.0.0.0/11"
    info:    network vnet show command OK

**listy vnet sieci**

Lista wszystkich istniejących wirtualnych sieci.

    ~$ azure network vnet list
    info:    Executing command network vnet list
    + Fetching Virtual Networks
    data:    Name        Status   AffinityGroup
    data:    ----------  -------  -------------
    data:    vnet1      Created  AG1
    data:    vnet2      Created  AG1
    data:    vnet3      Created  AG1
    data:    vnet4      Created  AG1
    info:    network vnet list command OK


**Usuwanie sieci vnet &lt;nazwa >**

Usuwa określoną wirtualnej sieci.

    ~$ azure network vnet delete opentechvn1
    info:    Executing command network vnet delete
    + Fetching Network Configuration
    Delete the virtual network opentechvn1 ?  (y/n) y
    + Deleting the virtual network opentechvn1
    info:    network vnet delete command OK

**Eksportowanie sieci [ścieżka]**

Konfiguracji zaawansowanej sieciowej można wyeksportować lokalnie konfiguracji sieci. Konfiguracja sieci wyeksportowanego obejmuje ustawienia serwera DNS, wirtualną sieć ustawienia, ustawienia sieci lokalnej witryny i inne ustawienia.

**import Network [ścieżka]**

Importowanie konfiguracji sieci lokalnej.

**sieć serwer_DNS zarejestrować [opcje] &lt;dnsIP >**

Zarejestruj się serwera DNS, który ma zostać użyta do rozpoznawania nazw w konfiguracji sieci.

    ~$ azure network dnsserver register 98.138.253.109 --dns-id FrontEndDnsServer
    info:    Executing command network dnsserver register
    + Fetching Network Configuration
    + Updating Network Configuration
    info:    network dnsserver register command OK

**listy serwer_DNS sieci**

Listy wszystkich serwerów DNS w konfiguracji sieci.

    ~$ azure network dnsserver list
    info:    Executing command network dnsserver list
    + Fetching Network Configuration
    data:    DNS Server ID         DNS Server IP
    data:    --------------------  --------------
    data:    DNS-bb39b4ac34d66a86  44.55.22.11
    data:    FrontEndDnsServer     98.138.253.109
    info:    network dnsserver list command OK

**sieć serwer_DNS unregister [opcje] &lt;dnsIP >**

Usuwa wpis serwera DNS z konfiguracji sieci.

    ~$ azure network dnsserver unregister 77.88.99.11
    info:    Executing command network dnsserver unregister
    + Fetching Network Configuration
    Delete the DNS server entry dns-4 ( 77.88.99.11 ) %s ? (y/n) y
    + Deleting the DNS server entry dns-4 ( 77.88.99.11 )
    info:    network dnsserver unregister command OK

