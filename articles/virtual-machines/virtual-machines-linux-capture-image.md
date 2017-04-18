<properties
    pageTitle="Przechwytywanie maszyny Linux ma zostać użyte jako szablonu | Microsoft Azure"
    description="Dowiedz się, jak przechwytywanie i obraz systemem Linux Azure maszyny wirtualnej (maszyn wirtualnych) utworzone za pomocą Menedżera zasobów Azure model wdrożenia."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="iainfou"/>


# <a name="capture-a-linux-virtual-machine-running-on-azure"></a>Przechwytywanie maszyny wirtualnej Linux uruchomionych Azure

Wykonaj kroki opisane w tym artykule, aby przechwycić komputera wirtualnych Azure Linux (maszyn wirtualnych) w modelu wdrożenia Menedżera zasobów, a. Uogólnienie maszyn wirtualnych Usuń informacje osobiste konto i przygotować maszyn wirtualnych może być używany jako obraz. Następnie przechwycić obraz uogólniony wirtualnego dysku twardego (VHD) systemu operacyjnego, wirtualnych dysków twardych dysków załączonych danych i [szablon Menedżera zasobów](../azure-resource-manager/resource-group-overview.md) dla nowych wdrożeń maszyn wirtualnych. 

Aby utworzyć maszyny wirtualne przy użyciu obrazu, konfigurowanie zasobów dla każdego nowego maszyn wirtualnych i wdrożyć go z przechwycone obrazy wirtualnego dysku twardego za pomocą szablonu (plik JavaScript Object Notation lub JSON,). W ten sposób można replikować maszyn wirtualnych z jego bieżącym konfiguracji oprogramowania, podobnie jak w przypadku używania obrazów w Azure Marketplace.

>[AZURE.TIP]Jeśli chcesz utworzyć kopię do istniejących maszyn wirtualnych Linux oraz jej specjalistyczne stanem dla kopii zapasowej lub debugowania, zobacz [Tworzenie kopii uruchomionych Azure maszyny wirtualnej Linux](virtual-machines-linux-copy-vm.md). I jeśli chcesz przekazać Linux wirtualny dysk twardy z lokalnego maszyn wirtualnych, zobacz [przekazywanie i tworzenie maszyny Linux z obrazu dysku niestandardowe](virtual-machines-linux-upload-vhd.md).  

## <a name="before-you-begin"></a>Przed rozpoczęciem

Upewnij się, że są spełnione następujące wymagania:

* **Azure maszyn wirtualnych utworzone w modelu wdrożenia Menedżera zasobów** — Jeśli nie utworzono maszyny Linux, możesz używać [portal](virtual-machines-linux-quick-create-portal.md), [Polecenie Azure](virtual-machines-linux-quick-create-cli.md)lub [Szablony Menedżera zasobów](virtual-machines-linux-cli-deploy-templates.md). 

    Konfigurowanie maszyn wirtualnych, stosownie do potrzeb. Na przykład [dodać dyski danych](virtual-machines-linux-add-disk.md), zastosować aktualizacje i zainstalować aplikacje. 
* **Polecenie azure** — Zainstaluj [Azure interfejsu wiersza polecenia](../xplat-cli-install.md) na komputerze lokalnym.

## <a name="step-1-remove-the-azure-linux-agent"></a>Krok 1: Usuwanie agenta Azure Linux

Najpierw uruchom polecenie **waagent** z parametrem **deprovision** na maszyn wirtualnych Linux. To polecenie usuwa pliki i dane, aby przygotować maszyn wirtualnych do uogólnianie. Aby uzyskać szczegółowe informacje zobacz [Podręcznik użytkownika Azure agenta Linux](virtual-machines-linux-agent-user-guide.md).

1. Połącz ze swojego maszyn wirtualnych Linux za pomocą klienta SSH.

2. W oknie SSH wpisz następujące polecenie:

    ```
    sudo waagent -deprovision+user
    ```
    >[AZURE.NOTE] To polecenie można uruchamiać tylko na maszyn wirtualnych, który chcesz przechwycić jako obraz. Nie gwarantuje, że obraz jest wyczyszczone wszystkich informacji poufnych lub nadaje się do rozpowszechniania.

3. Wpisz **y** , aby kontynuować. Możesz dodać **-wymusić** parametr, aby uniknąć tego kroku potwierdzenia.

4. Po zakończeniu polecenia wpisz **zamknąć**. W tym kroku zamyka klienta SSH.

    
## <a name="step-2-capture-the-vm"></a>Krok 2: Przechwytywanie maszyn wirtualnych

Aby przechwycić maszyn wirtualnych, a za pomocą interfejsu wiersza polecenia Azure. W poniższych przykładach należy zastąpić przykładowe nazwy parametrów własne wartości. Nazwy parametrów przykład obejmują **myResourceGroup**, **myVnet**i **myVM**.

5. Na komputerze lokalnym otwórz polecenie Azure i [Logowanie do subskrypcji usługi Azure](../xplat-cli-connect.md). 

6. Upewnij się, że jesteś w trybie Menedżera zasobów.

    ```
    azure config mode arm
    ```

7. Zamknij maszyn wirtualnych, które można już wstrzymano obsługę administracyjną przy użyciu następującego polecenia:

    ```
    azure vm deallocate -g MyResourceGroup -n myVM
    ```

8. Generalize maszyn wirtualnych przy użyciu następującego polecenia:

    ```
    azure vm generalize -g MyResourceGroup -n myVM
    ```

9. Teraz uruchom polecenie **Przechwytywanie azure maszyn wirtualnych** , które znajdują się maszyn wirtualnych. W poniższym przykładzie obraz wirtualnych dysków twardych są przechwytywane o nazwach rozpoczynających się od **MyVHDNamePrefix**, a opcja **-t** Określa ścieżkę do szablonu **MyTemplate.json**. 

    ```
    azure vm capture MyResourceGroup MyResourceGroup MyVHDNamePrefix -t MyTemplate.json
    ```

    >[AZURE.IMPORTANT]Domyślnie z tego samego konta miejsca do magazynowania, używany przez oryginalny maszyn wirtualnych tworzone pobieranie plików obrazów wirtualnego dysku twardego. Użyj *tego samego konta miejsca do magazynowania* do przechowywania wirtualnych dysków twardych wszelkie nowe maszyny wirtualne tworzenie z obrazu. 

6. Aby znaleźć lokalizację przechwycony obraz, otwórz szablon JSON w edytorze tekstów. W **storageProfile**Znajdź **uri** **obrazu** znajdującego się w kontenerze **system** . Na przykład identyfikator URI obrazu dysku systemu operacyjnego jest podobne do`https://xxxxxxxxxxxxxx.blob.core.windows.net/system/Microsoft.Compute/Images/vhds/MyVHDNamePrefix-osDisk.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`

## <a name="step-3-create-a-vm-from-the-captured-image"></a>Krok 3: Tworzenie maszyn wirtualnych z przechwycony obraz
Teraz użyć obrazu za pomocą szablonu do utworzenia maszyny Linux. Te kroki pokazano, jak używać polecenie Azure i szablonu pliku JSON, które zostały przechwycone, Tworzenie maszyn wirtualnych w nowej, wirtualną sieć.

### <a name="create-network-resources"></a>Tworzenie zasobów

Aby użyć szablonu, należy najpierw skonfiguruj wirtualnej sieci i NIC dla Twojej nowej maszyn wirtualnych. Zalecamy zapoznanie utworzyć grupę zasobów dla tych zasobów w lokalizacji, w której jest przechowywany obraz maszyn wirtualnych. Uruchamianie polecenia podobne do następujących, zastępując nazw dla zasobów i odpowiedniej lokalizacji Azure ("centralus" w tych poleceń):

    azure group create MyResourceGroup1 -l "centralus"

    azure network vnet create MyResourceGroup1 myVnet -l "centralus"

    azure network vnet subnet create MyResourceGroup1 myVnet mySubnet

    azure network public-ip create MyResourceGroup1 myIP -l "centralus"

    azure network nic create MyResourceGroup1 myNIC -k mySubnet -m myVnet -p myIP -l "centralus"

### <a name="get-the-id-of-the-nic"></a>Pobierz identyfikator karty sieciowej

Aby wdrożyć maszyn wirtualnych z obrazu przy użyciu JSON zapisany podczas przechwytywania, potrzebny jest identyfikator karty sieciowej. Uzyskaj ją, uruchamiając następujące polecenie:

    azure network nic show MyResourceGroup1 myNIC

**Identyfikator** w wyniku kwerendy jest podobne do`/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/MyResourceGroup1/providers/Microsoft.Network/networkInterfaces/myNic`



### <a name="create-a-vm"></a>Tworzenie maszyn wirtualnych
Teraz uruchom następujące polecenie, aby utworzyć do maszyn wirtualnych z przechwycony obraz maszyn wirtualnych. Aby określić ścieżkę do pliku JSON szablonu, który został zapisany, należy użyć parametru **-f** .

    azure group deployment create MyResourceGroup1 MyDeployment -f MyTemplate.json

W wynikach polecenia zostanie wyświetlony monit o podanie pod nową nazwą maszyn wirtualnych, nazwa użytkownika administratora i hasło i identyfikator NIC wcześniej utworzone.

    info:    Executing command group deployment create
    info:    Supply values for the following parameters
    vmName: myNewVM
    adminUserName: myAdminuser
    adminPassword: ********
    networkInterfaceId: /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resource Groups/MyResourceGroup1/providers/Microsoft.Network/networkInterfaces/myNic

Poniższy przykład pokazuje, co widać na pomyślne wdrożenie:

    + Inicjowanie konfiguracji szablonu i parametrów
    + Tworzenie informacji o wdrożeniu: utworzony szablon wdrożenia xxxxxxx
    + Oczekiwanie na wdrożenia ukończyć danych: DeploymentName: MyDeployment danych: ResourceGroupName: MyResourceGroup1 danych: ProvisioningState: zakończyła się pomyślnie danych: sygnatury czasowej: xxxxxxx danych: tryb: danych pierwotnych: typ wartości z pola Nazwa


    data:    ------------------  ------------  -------------------------------------

    dane: myNewVM ciąg vmName


    dane: vmSize Standard_D1 ciąg


    dane: myAdminuser ciąg adminUserName


    dane: adminPassword SecureString zdefiniowane


    dane: networkInterfaceId ciąg /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/MyResourceGroup1/providers/Microsoft.Network/networkInterfaces/myNic informacje: wdrożenie grupy Utwórz polecenie OK

### <a name="verify-the-deployment"></a>Sprawdź wdrażania

Teraz SSH maszyn wirtualnych, utworzonego Sprawdź wdrożenia i Rozpoczynanie korzystania z nowych maszyn wirtualnych. Aby połączyć za pośrednictwem SSH, Znajdź adres IP maszyn wirtualnych, został utworzony, uruchamiając następujące polecenie:

    azure network public-ip show MyResourceGroup1 myIP

Publiczny adres IP znajduje się w wynikach polecenia. Domyślnie łączysz się maszyn wirtualnych Linux przez SSH na porcie 22.

## <a name="create-additional-vms"></a>Tworzenie dodatkowych maszyny wirtualne
Za pomocą przechwycony obraz i szablonu do wdrażania dodatkowe maszyny wirtualne, wykonując kroki w poprzedniej sekcji. Inne opcje, aby utworzyć maszyny wirtualne z obrazu zawierać przy użyciu szablonu Szybki Start lub uruchomienie polecenia **Utwórz azure maszyn wirtualnych** .

### <a name="use-the-captured-template"></a>Korzystanie z szablonu przechwycony

Aby użyć przechwycony obraz szablonu, wykonaj następujące czynności (dane szczegółowe w poprzedniej sekcji):

* Upewnij się, to obraz maszyn wirtualnych z tego samego konta miejsca do magazynowania, obsługującego usługi maszyn wirtualnych wirtualnego dysku twardego.
* Skopiuj plik JSON szablonu i określ unikatową nazwę dla dysku systemu operacyjnego nowa maszyna wirtualna wirtualny dysk twardy (lub wirtualnych dysków twardych). Na przykład w **storageProfile**, w obszarze **wirtualnego dysku twardego**w **uri**Określ unikatową nazwę **osDisk** wirtualny dysk twardy, podobnie jak`https://xxxxxxxxxxxxxx.blob.core.windows.net/vhds/MyNewVHDNamePrefix-osDisk.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`
* Tworzenie NIC w tym samym lub innym wirtualnej sieci.
* Przy użyciu zmodyfikowany JSON plik szablonu, można tworzyć wdrożeniu w grupie zasobów, w którym możesz skonfigurować wirtualnej sieci.

### <a name="use-a-quickstart-template"></a>Korzystanie z szablonu Szybki Start

Jeśli chcesz skonfigurować automatycznie po utworzeniu maszyny z obrazu sieci, można określić tych zasobów w szablonie. Na przykład zobacz temat [101 maszyn wirtualnych użytkownika obraz z szablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image) z GitHub. Ten szablon umożliwia utworzenie maszyn wirtualnych z obraz niestandardowy i niezbędne wirtualnej sieci, publiczny adres IP i zasoby sieciowa. Aby skorzystać z za pomocą tego szablonu w Azure portal zobacz [Tworzenie maszyny wirtualnej z niestandardowego obrazu przy użyciu szablonu Menedżera zasobów](http://codeisahighway.com/how-to-create-a-virtual-machine-from-a-custom-image-using-an-arm-template/).

### <a name="use-the-azure-vm-create-command"></a>Polecenia do utworzenia Użyj azure maszyn wirtualnych

Zazwyczaj najłatwiej jest je szablon Menedżera zasobów umożliwia tworzenie maszyn wirtualnych z obrazu. Jednak można utworzyć maszyn wirtualnych _imperatively_ przy użyciu polecenia **Utwórz azure maszyn wirtualnych** **-Q** (**— obraz urn**) parametru. Jeśli ta metoda również przekazać parametru **-d** (**--os-dysku-wirtualnego dysku twardego**) określ lokalizację pliku VHD OS nowych maszyn wirtualnych. Ten plik musi znajdować się w kontenerze wirtualnych dysków twardych konta miejsca do magazynowania, której jest przechowywany plik obrazu wirtualnego dysku twardego. Polecenie kopiuje wirtualnego dysku twardego dla nowych maszyn wirtualnych automatycznie w kontenerze **wirtualnych dysków twardych** .

Przed uruchomieniem **Tworzenie azure maszyn wirtualnych** z obrazem, wykonaj następujące czynności:

1.  Tworzenie grupy zasobów lub Zidentyfikuj istniejącej grupy zasobów do wdrożenia.

2.  Tworzenie publicznej zasoby Adres IP i NIC dla nowych maszyn wirtualnych. Aby uzyskać procedury Tworzenie wirtualnych sieci, publiczny adres IP i NIC za pomocą interfejsu wiersza polecenia Zobacz wcześniej w tym artykule. (**Tworzenie azure maszyn wirtualnych** można także tworzyć NIC, ale musisz przekazać dodatkowe parametry określające wirtualnej sieci i podsieci).


Następnie uruchom polecenie, które przekazuje identyfikatory URI zarówno nowego pliku wirtualnego dysku twardego systemu operacyjnego, jak i istniejącego obrazu. W tym przykładzie rozmiarze maszyn wirtualnych Standard_A1 zostanie utworzona w regionu wschodniego USA.

    azure vm create MyResourceGroup1 myNewVM eastus Linux -d "https://xxxxxxxxxxxxxx.blob.core.windows.net/vhds/MyNewVHDNamePrefix.vhd" -Q "https://xxxxxxxxxxxxxx.blob.core.windows.net/system/Microsoft.Compute/Images/vhds/MyVHDNamePrefix-osDisk.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd" -z Standard_A1 -u myAdminname -p myPassword -f myNIC

Opcje dodatkowe polecenie Uruchom `azure help vm create`.

## <a name="next-steps"></a>Następne kroki

Aby zarządzać pośrednictwem usługi SMS przy użyciu interfejsu wiersza polecenia, zobacz zadania w [rozmieszczanie i zarządzanie nimi maszyn wirtualnych przy użyciu szablonów Menedżera zasobów Azure i polecenie Azure](virtual-machines-linux-cli-deploy-templates.md).
