<properties
   pageTitle="Wdrażanie maszyny wirtualne NIC wielu przy użyciu szablonu w Menedżerze zasobów | Microsoft Azure"
   description="Dowiedz się, jak wdrożyć maszyny wirtualne NIC wielu przy użyciu szablonu w Menedżerze zasobów"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/02/2016"
   ms.author="jdial" />

# <a name="deploy-multi-nic-vms-using-a-template"></a>Wdrażanie maszyny wirtualne NIC wielu przy użyciu szablonu

[AZURE.INCLUDE [virtual-network-deploy-multinic-arm-selectors-include.md](../../includes/virtual-network-deploy-multinic-arm-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-deploy-multinic-intro-include.md](../../includes/virtual-network-deploy-multinic-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][model klasyczny wdrożenia](virtual-network-deploy-multinic-classic-ps.md).

[AZURE.INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

Ponieważ w tym momencie nie są maszyny wirtualne z jednym NIC i maszyny wirtualne z kart w tej samej grupy zasobów, wykona serwerem wewnętrznym w grupę zasobów i wszystkie inne składniki w innej grupie zabezpieczeń. Grupa zasobów o nazwie *IaaSStory* dla grupy zasobów głównym i *Wewnętrznej bazy danych IaaSStory* dla serwerów wewnętrznej za pomocą poniższych kroków.

## <a name="prerequisites"></a>Wymagania wstępne

Przed wdrożeniem serwerem wewnętrznym, należy wdrożyć grupy zasobów głównym z wszystkie niezbędne zasoby dla tego scenariusza. Aby wdrożyć te zasoby, wykonaj poniższe czynności.

1. Przejdź do [strony szablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC).
2. Na stronie szablonu z prawej strony **nadrzędnej, grupa zasobów**kliknij pozycję **Deploy Azure**.
3. W razie potrzeby zmień wartości parametrów, a następnie postępuj zgodnie z instrukcjami portal Azure Podgląd, aby wdrożyć grupa zasobów.

> [AZURE.IMPORTANT] Upewnij się, że nazwy konta przestrzeni dyskowej są unikatowe. Nie może zawierać zduplikowane magazynowania nazwy kont Azure.

## <a name="understand-the-deployment-template"></a>Opis szablonu rozmieszczenia

Przed wdrożeniem szablonu dostarczanego z tej dokumentacji, upewnij się, że wiesz, co oznacza. Poniższe kroki zapewniają przystępny Przegląd danego szablonu.

1. Przejdź do [strony szablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC).
2. Kliknij przycisk **azuredeploy.json** , aby otworzyć plik szablonu.
3. Zwróć uwagę, parametrów *osType* wymienionych poniżej. Ten parametr jest używany do wybierania obrazów maszyn wirtualnych dla serwera bazy danych, wraz z wielu systemów operacyjnych pozostałe ustawienia.

        "osType": {
          "type": "string",
          "defaultValue": "Windows",
          "allowedValues": [
            "Windows",
            "Ubuntu"
          ],
          "metadata": {
            "description": "Type of OS to use for VMs: Windows or Ubuntu."
          }
        },

4. Przewiń w dół do listy zmiennych i sprawdzanie definicji dla zmiennych **dbVMSetting** , wymienione poniżej. Otrzymuje jeden z elementów tablicy zawarte w zmiennej **dbVMSettings** . Jeśli znasz terminologię opracowywania oprogramowania, możesz wyświetlić zmienną **dbVMSettings** jako tablicę skrótów lub dictionay.

        "dbVMSetting": "[variables('dbVMSettings')[parameters('osType')]]"

5. Załóżmy, że użytkownik chce wdrażanie maszyny wirtualne systemu Windows działa SQL wewnętrzna. Następnie wartość **osType** będzie *systemu Windows*, a zmienna **dbVMSetting** zawiera wymienione poniżej, elementu, który odpowiada pierwszej wartości w zmiennej **dbVMSettings** .

          "Windows": {
            "vmSize": "Standard_DS3",
            "publisher": "MicrosoftSQLServer",
            "offer": "SQL2014SP1-WS2012R2",
            "sku": "Standard",
            "version": "latest",
            "vmName": "DB",
            "osdisk": "osdiskdb",
            "datadisk": "datadiskdb",
            "nicName": "NICDB",
            "ipAddress": "192.168.2.",
            "extensionDeployment": "",
            "avsetName": "ASDB",
            "remotePort": 3389,
            "dbPort": 1433
          },

6. Zwróć uwagę, że **vmSize** zawiera wartość *Standard_DS3*. Określone rozmiary maszyn wirtualnych umożliwia korzystanie z kart. Można sprawdzić, które maszyn wirtualnych o rozmiarach wielokrotne NIC włączone, odwiedź witrynę [Omówienie NIC wielokrotne](virtual-networks-multiple-nics.md).
7. Przewiń w dół do **zasobów** i zwróć uwagę pierwszy element. Opisuje konta miejsca do magazynowania. To konto miejsca do magazynowania będzie używana do obsługi dysków danych każdej bazy danych maszyn wirtualnych. W tym scenariuszu maszyn wirtualnych każdej bazy danych ma dysk OS przechowywane w magazynie zwykła i dwa dyski dane przechowywane w magazynie SSD (premium).

        {
          "apiVersion": "2015-05-01-preview",
          "type": "Microsoft.Storage/storageAccounts",
          "name": "[parameters('prmStorageName')]",
          "location": "[variables('location')]",
          "tags": {
            "displayName": "Storage Account - Premium"
          },
          "properties": {
            "accountType": "[parameters('prmStorageType')]"
          }
        },

8. Przewiń w dół do następnego zasobu wymienione poniżej. Ten zasób reprezentuje NIC na potrzeby dostępu do bazy danych w każdym maszyn wirtualnych w bazie danych. Zwróć uwagę, użyj funkcji **kopiowania** w tego zasobu. Szablon pozwala na wdrażanie tyle maszyny wirtualne wymaganą, na podstawie parametru **dbCount** . W związku z tym potrzebne do utworzenia takiej samej ilości miejsca sieciowe dla bazy danych programu access, jedną dla każdego maszyn wirtualnych.

        {
          "apiVersion": "2015-06-15",
          "type": "Microsoft.Network/networkInterfaces",
          "name": "[concat(variables('dbVMSetting').nicName,'-DA-', copyindex(1))]",
          "location": "[variables('location')]",
          "tags": {
            "displayName": "NetworkInterfaces - DB DA"
          },
          "copy": {
            "name": "dbniccount",
            "count": "[parameters('dbCount')]"
          },
          "properties": {
            "ipConfigurations": [
              {
                "name": "ipconfig1",
                "properties": {
                  "privateIPAllocationMethod": "Static",
                  "privateIPAddress": "[concat(variables('dbVMSetting').ipAddress,copyindex(4))]",
                  "subnet": {
                    "id": "[variables('backEndSubnetRef')]"
                  }
                }
              }
            ]
          }
        },

9. Przewiń w dół do następnego zasobu wymienione poniżej. Ten zasób reprezentuje NIC na potrzeby zarządzania w każdej maszyn wirtualnych bazy danych. Ponownie potrzebujesz jednego z tych nic dla każdej bazy danych maszyn wirtualnych. Zwróć uwagę, element **networkSecurityGroup** , łączenie NSG, która zezwala na dostęp do RDP-SSH, aby tylko tej karty Sieciowej.

        {
          "apiVersion": "2015-06-15",
          "type": "Microsoft.Network/networkInterfaces",
          "name": "[concat(variables('dbVMSetting').nicName, '-RA-',copyindex(1))]",
          "location": "[variables('location')]",
          "tags": {
            "displayName": "NetworkInterfaces - DB RA"
          },
          "copy": {
            "name": "dbniccount",
            "count": "[parameters('dbCount')]"
          },
          "properties": {
            "ipConfigurations": [
              {
                "name": "ipconfig1",
                "properties": {
                  "networkSecurityGroup": {
                    "id": "[resourceId('Microsoft.Network/networkSecurityGroups', parameters('remoteAccessNSGName'))]"
                  },
                  "privateIPAllocationMethod": "Static",
                  "privateIPAddress": "[concat(variables('dbVMSetting').ipAddress,copyindex(54))]",
                  "subnet": {
                    "id": "[variables('backEndSubnetRef')]"
                  }
                }
              }
            ]
          }
        },

10. Przewiń w dół do następnego zasobu wymienione poniżej. Ten zasób reprezentuje dostępność Ustawianie zostać udostępniona przez wszystkie maszyny wirtualne bazy danych. W ten sposób można zagwarantować, że będzie zawsze jeden maszyn wirtualnych w zestawie uruchomione podczas konserwacji.

        {
          "apiVersion": "2015-06-15",
          "type": "Microsoft.Compute/availabilitySets",
          "name": "[variables('dbVMSetting').avsetName]",
          "location": "[variables('location')]",
          "tags": {
            "displayName": "AvailabilitySet - DB"
          }
        },

11. Przewiń w dół do następnego zasobu. Tego zasobu reprezentuje bazy danych maszyny wirtualne, jak pokazano w pierwszym kilka wierszy wymienione poniżej. Zwróć uwagę, użyj funkcji **kopiowania** ponownie, zapewnienie, że wielu maszyny wirtualne są tworzone w oparciu o parametr **dbCount** . Ponadto w zbiorze **dependsOn** . Wyświetla listę dwóch nic konieczności można utworzyć przed wdrożeniem maszyn wirtualnych, wraz z zestawu dostępność i konto miejsca do magazynowania.

          "apiVersion": "2015-06-15",
          "type": "Microsoft.Compute/virtualMachines",
          "name": "[concat(variables('dbVMSetting').vmName,copyindex(1))]",
          "location": "[variables('location')]",
          "dependsOn": [
            "[concat('Microsoft.Network/networkInterfaces/', variables('dbVMSetting').nicName,'-DA-', copyindex(1))]",
            "[concat('Microsoft.Network/networkInterfaces/', variables('dbVMSetting').nicName,'-RA-', copyindex(1))]",
            "[concat('Microsoft.Compute/availabilitySets/', variables('dbVMSetting').avsetName)]",
            "[concat('Microsoft.Storage/storageAccounts/', parameters('prmStorageName'))]"
          ],
          "tags": {
            "displayName": "VMs - DB"
          },
          "copy": {
            "name": "dbvmcount",
            "count": "[parameters('dbCount')]"
          },

12. Przewiń w dół na elemencie **networkProfile** zasobu maszyn wirtualnych wymienione poniżej. Zwróć uwagę, że istnieją dwa sieciowe są odniesienia dla każdego maszyn wirtualnych. Podczas tworzenia kart dla maszyn wirtualnych, należy ustawić właściwość **podstawowego** jednego z sieciowe na *wartość PRAWDA*, a pozostałe *FAŁSZ*.

        "networkProfile": {
          "networkInterfaces": [
            {
              "id": "[resourceId('Microsoft.Network/networkInterfaces', concat(variables('dbVMSetting').nicName,'-DA-',copyindex(1)))]",
              "properties": { "primary": true }
            },
            {
              "id": "[resourceId('Microsoft.Network/networkInterfaces', concat(variables('dbVMSetting').nicName,'-RA-',copyindex(1)))]",
              "properties": { "primary": false }
            }
          ]
        }
      }

## <a name="deploy-the-arm-template-by-using-click-to-deploy"></a>Wdrażanie szablonu ARM za pomocą kliknięcia do wdrożenia

> [AZURE.IMPORTANT] Upewnij się, że postępuj zgodnie z instrukcjami [wymagania wstępne](#Pre-requisites) przed zgodnie z poniższymi instrukcjami.

Poniższy przykładowy szablon dostępny w publicznej repozytorium używa parametru plik zawierający domyślne wartości umożliwia generowanie scenariuszy opisanych powyżej. Aby wdrożyć ten szablon przy użyciu kliknij wdrażania, zrób [to łącze](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC), po prawej stronie **grupy zasobów wewnętrznej bazy danych (zobacz dokumentację)** kliknij **Wdrażanie Azure**, Zamień wartości domyślne parametrów w razie potrzeby i postępuj zgodnie z instrukcjami w portalu.

Na poniższym rysunku przedstawiono zawartość nowej grupy zasobów po wdrożeniu.

![Grupa zasobów wewnętrznej](./media/virtual-network-deploy-multinic-arm-template/Figure2.png)

## <a name="deploy-the-template-by-using-powershell"></a>Wdrażanie szablon przy użyciu programu PowerShell

Aby wdrożyć szablon, który został pobrany przy użyciu programu PowerShell, wykonaj poniższe czynności.

[AZURE.INCLUDE [powershell-preview-include.md](../../includes/powershell-preview-include.md)]

3. Uruchamianie **`New-AzureRmResourceGroup`** polecenia cmdlet, aby utworzyć grupę zasobów za pomocą tego szablonu.

        New-AzureRmResourceGroup -Name IaaSStory-Backend -Location uswest `
            -TemplateFile 'https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.json' `
            -TemplateParameterFile 'https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.parameters.json'

    Oczekiwany wynik:

        ResourceGroupName : IaaSStory-Backend
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
                            Actions  NotActions
                            =======  ==========
                            *                  

        Resources         :
                            Name                 Type                                 Location
                            ===================  ===================================  ========
                            ASDB                 Microsoft.Compute/availabilitySets   westus  
                            DB1                  Microsoft.Compute/virtualMachines    westus  
                            DB2                  Microsoft.Compute/virtualMachines    westus  
                            NICDB-DA-1           Microsoft.Network/networkInterfaces  westus  
                            NICDB-DA-2           Microsoft.Network/networkInterfaces  westus  
                            NICDB-RA-1           Microsoft.Network/networkInterfaces  westus  
                            NICDB-RA-2           Microsoft.Network/networkInterfaces  westus  
                            wtestvnetstorageprm  Microsoft.Storage/storageAccounts    westus  

        ResourceId        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory-Backend

## <a name="deploy-the-template-by-using-the-azure-cli"></a>Wdrażanie szablonu za pomocą interfejsu wiersza polecenia Azure

Aby wdrożyć szablon przy użyciu interfejsu wiersza polecenia Azure, wykonaj poniższe czynności.

1. Jeśli po raz pierwszy używasz polecenie Azure, zobacz [Instalowanie i konfigurowanie polecenie Azure](../xplat-cli-install.md) i postępuj zgodnie z instrukcjami do miejsca, w którym wybierz swoje konto Azure i subskrypcji.
2. Uruchamianie **`azure config mode`** polecenie, aby przełączyć się do trybu Menedżera zasobów, jak pokazano poniżej.

        azure config mode arm

    Oto oczekiwany wynik polecenia powyżej:

        info:    New mode is arm

3. Otwórz [plik parametr](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.parameters.json), zaznacz jego zawartość i zapisanie go w pliku na komputerze. Na przykład możemy zapisano plik parametrów *parameters.json*.

4. Uruchamianie **`azure group deployment create`** polecenia cmdlet wdrażania nowych VNet przy użyciu szablonu i parametr pliki, możesz pobrać i modyfikować powyżej. Lista wyświetlana po wynik wyjaśniono parametry używane.

        azure group create -n IaaSStory-Backend -l westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.json -e parameters.json

    Oczekiwany wynik:

        info:    Executing command group create
        + Getting resource group IaaSStory-Backend
        + Creating resource group IaaSStory-Backend
        info:    Created resource group IaaSStory-Backend
        + Initializing template configurations and parameters
        + Creating a deployment
        info:    Created template deployment "azuredeploy"
        data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory-Backend
        data:    Name:                IaaSStory-Backend
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:
        info:    group create command OK
