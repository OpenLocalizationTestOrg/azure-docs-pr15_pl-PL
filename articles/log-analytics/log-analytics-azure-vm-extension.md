<properties
    pageTitle="Łączenie Azure maszyn wirtualnych z dziennika analizy | Microsoft Azure"
    description="Dla systemu Windows i Linux oraz maszyn wirtualnych działa Azure zalecany sposób zebrane dzienniki i metryki jest instalując rozszerzenia maszyn wirtualnych Azure analizy dziennika. Aby zainstalować rozszerzenie maszyn wirtualnych analizy dziennika na maszyny wirtualne Azure, możesz używać Azure portal lub programu PowerShell."
    services="log-analytics"
    documentationCenter=""
    authors="richrundmsft"
    manager="jochan"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="richrund"/>

# <a name="connect-azure-virtual-machines-to-log-analytics"></a>Łączenie Azure maszyn wirtualnych z analizy dziennika

W przypadku komputerów systemu Windows i Linux oraz zbierania dzienników i metryk zaleca instalując agenta analizy dziennika.

Najłatwiejszym sposobem zainstalować agenta analizy dziennika na Azure maszyn wirtualnych jest za pośrednictwem rozszerzenia maszyn wirtualnych analizy dziennika.  Przy użyciu rozszerzenia upraszcza proces instalacji i automatycznie konfiguruje agenta wysyłanie danych do obszaru roboczego analizy dziennika, określonym przez użytkownika. Agent jest również automatycznie uaktualniane, zapewnienie, że masz najnowsze funkcje i poprawki.

Dla maszyn wirtualnych systemu Windows należy włączyć rozszerzenia maszyn wirtualnych *Monitorowania agenta firmy Microsoft* .
Dla maszyn wirtualnych systemu Linux należy włączyć rozszerzenia maszyn wirtualnych *Usługi OMS agenta dla Linux* .

Dowiedz się więcej o [Linux agent] i [rozszerzenia Azure maszyn wirtualnych](../virtual-machines/virtual-machines-windows-extensions-features.md) (.. / virtual-machines/virtual-machines-linux-agent-user-guide.md).

Użycie oparte na agenta zbioru danych dziennika, musisz skonfigurować [źródła danych w dzienniku analizy](log-analytics-data-sources.md) Aby określić dzienniki i miar, które chcesz zebrać.

>[AZURE.IMPORTANT] Po skonfigurowaniu analizy dziennika zindeksować danych dziennika przy użyciu [zestawu narzędzi Diagnostyka Azure](log-analytics-azure-storage.md)i Konfigurowanie agenta zbieranie dzienników samej, dzienniki są zbierane dwa razy. Zajmujesz dla obu źródeł danych. Jeśli masz agent zainstalowany, a następnie należy zbieranie danych dziennika przy użyciu agenta tylko — nie jest skonfigurowana analizy dziennika do zbierania danych dziennika od diagnostyki Azure.

Istnieją trzy sposoby włączyć rozszerzenie maszyn wirtualnych analizy dziennika:

+ Za pomocą portalu Azure
+ Przy użyciu programu PowerShell Azure
+ Za pomocą szablonu Azure Menedżera zasobów

## <a name="enable-the-vm-extension-in-the-azure-portal"></a>Włączanie rozszerzenia maszyn wirtualnych w portalu Azure

Można zainstalować agenta analiz dziennika i połączyć Azure maszyny wirtualnej uruchamianej na za pomocą [Azure portal](https://portal.azure.com).

### <a name="to-install-the-log-analytics-agent-and-connect-the-virtual-machine-to-a-log-analytics-workspace"></a>Zainstalować agenta analizy dziennika i połączyć maszyny wirtualnej do obszaru roboczego analizy dziennika

1.  Zaloguj się do [portalu Azure](http://portal.azure.com).
2.  Wybierz pozycję **Przeglądaj** po lewej stronie portalu i przejdź do **Usługi dziennika analizy (OMS)** , a następnie zaznacz go.
3.  Na liście obszarów roboczych analizy dziennika wybierz ten, który ma być używany z maszyn wirtualnych Azure.  
    ![Obszary robocze usługi OMS](./media/log-analytics-azure-vm-extension/oms-connect-azure-01.png)
4.  W obszarze **Zarządzanie analizy dziennikiem**wybierz **maszyn wirtualnych**.  
    ![Maszyn wirtualnych](./media/log-analytics-azure-vm-extension/oms-connect-azure-02.png)
5.  Na liście maszyn **wirtualnych**wybierz maszyny wirtualnej, na którym chcesz zainstalować agenta. **Stan połączenia usługi OMS** maszyn wirtualnych wskazuje, że jest **braku połączenia**.  
    ![Nie masz połączenia maszyn wirtualnych](./media/log-analytics-azure-vm-extension/oms-connect-azure-03.png)
6.  W sekcji szczegółów komputera wirtualnych zaznacz pole wyboru **Połącz**. Agent jest automatycznie zainstalowany i skonfigurowany do obszaru roboczego analizy dziennika. Ten proces może potrwać kilka minut, w tym czasie stan połączenia usługi OMS jest *Nawiązywanie połączenia...*  
    ![Łączenie maszyn wirtualnych](./media/log-analytics-azure-vm-extension/oms-connect-azure-04.png)
7.  Po zainstalowaniu i łączenie agent stan **połączenia usługi OMS** zostaną zaktualizowane do wyświetlenie **tego obszaru roboczego**.  
    ![Połączone](./media/log-analytics-azure-vm-extension/oms-connect-azure-05.png)


## <a name="enable-the-vm-extension-using-powershell"></a>Włączanie rozszerzenia maszyn wirtualnych przy użyciu programu PowerShell

Istnieją różne polecenia Azure klasyczny maszyn wirtualnych i maszyn wirtualnych Menedżera zasobów. Oto przykłady zarówno klasyczny i maszyn wirtualnych Menedżera zasobów.

W przypadku klasycznej maszyn wirtualnych Użyj w poniższym przykładzie programu PowerShell:

```
Add-AzureAccount

$workspaceId = "enter workspace ID here"
$workspaceKey = "enter workspace key here"
$hostedService = "enter hosted service here"

$vm = Get-AzureVM –ServiceName $hostedService

# For Windows VM uncomment the following line
# Set-AzureVMExtension -VM $vm -Publisher 'Microsoft.EnterpriseCloud.Monitoring' -ExtensionName 'MicrosoftMonitoringAgent' -Version '1.*' -PublicConfiguration "{'workspaceId': '$workspaceId'}" -PrivateConfiguration "{'workspaceKey': '$workspaceKey' }" | Update-AzureVM -Verbose

# For Linux VM uncomment the following line
# Set-AzureVMExtension -VM $vm -Publisher 'Microsoft.EnterpriseCloud.Monitoring' -ExtensionName 'OmsAgentForLinux' -Version '1.*' -PublicConfiguration "{'workspaceId': '$workspaceId'}" -PrivateConfiguration "{'workspaceKey': '$workspaceKey' }" | Update-AzureVM -Verbose
```

W przypadku maszyn wirtualnych Menedżera zasobów Użyj w poniższym przykładzie programu PowerShell:

```
Login-AzureRMAccount
Select-AzureSubscription -SubscriptionId "**"

$workspaceName = "your workspace name"
$VMresourcegroup = "**"
$VMresourcename = "**"

$workspace = (Get-AzureRmOperationalInsightsWorkspace).Where({$_.Name -eq $workspaceName})

if ($workspace.Name -ne $workspaceName)
{
    Write-Error "Unable to find OMS Workspace $workspaceName. Do you need to run Select-AzureRMSubscription?"
}

$workspaceId = $workspace.CustomerId
$workspaceKey = (Get-AzureRmOperationalInsightsWorkspaceSharedKeys -ResourceGroupName $workspace.ResourceGroupName -Name $workspace.Name).PrimarySharedKey

$vm = Get-AzureRmVM -ResourceGroupName $VMresourcegroup -Name $VMresourcename
$location = $vm.Location

# For Windows VM uncomment the following line
# Set-AzureRmVMExtension -ResourceGroupName $VMresourcegroup -VMName $VMresourcename -Name 'MicrosoftMonitoringAgent' -Publisher 'Microsoft.EnterpriseCloud.Monitoring' -ExtensionType 'MicrosoftMonitoringAgent' -TypeHandlerVersion '1.0' -Location $location -SettingString "{'workspaceId': '$workspaceId'}" -ProtectedSettingString "{'workspaceKey': '$workspaceKey'}"

# For Linux VM uncomment the following line
# Set-AzureRmVMExtension -ResourceGroupName $VMresourcegroup -VMName $VMresourcename -Name 'OmsAgentForLinux' -Publisher 'Microsoft.EnterpriseCloud.Monitoring' -ExtensionType 'OmsAgentForLinux' -TypeHandlerVersion '1.0' -Location $location -SettingString "{'workspaceId': '$workspaceId'}" -ProtectedSettingString "{'workspaceKey': '$workspaceKey'}"


```
Po skonfigurowaniu komputera wirtualnych przy użyciu programu PowerShell, należy podać **Identyfikator obszaru roboczego** i **Klucza podstawowego**. Identyfikator i klucz można znaleźć na stronie **Ustawienia** w portalu usługi OMS lub przy użyciu programu PowerShell, jak pokazano w poprzednim przykładzie.

![Identyfikator obszaru roboczego i klucza podstawowego](./media/log-analytics-azure-vm-extension/oms-analyze-azure-sources.png)

## <a name="deploy-the-vm-extension-using-a-template"></a>Wdrażanie z rozszerzeniem maszyn wirtualnych przy użyciu szablonu

Za pomocą Menedżera zasobów Azure można utworzyć szablon prostych (w formacie JSON), który definiuje wdrażania i konfigurowania aplikacji. Ten szablon jest znany jako szablonu Menedżera zasobów i umożliwia deklaracyjnych Definiowanie wdrożenia. Za pomocą szablonu, można wielokrotnie wdrażania aplikacji w całym cyklu życia aplikacji i mieć pewność, że zasoby wdrożonych jest spójna.

Łącznie z agentem analizy dziennika jako części szablonu Menedżera zasobów, można zapewnić, że każda maszyna wirtualna jest wstępnie skonfigurowana do raportu do obszaru roboczego analizy dziennika.

Aby uzyskać więcej informacji na temat szablonów Menedżera zasobów zobacz [Szablony do tworzenia Azure Menedżera zasobów](../resource-group-authoring-templates.md).

Oto przykład szablonu Menedżera zasobów, który służy do wdrażania maszyny wirtualnej z systemem Windows z rozszerzeniem monitorowania agenta firmy Microsoft zainstalowanego. Ten szablon jest szablonem typowe maszyn wirtualnych, zawierają następujące rozszerzenia:

+ Parametry workspaceId i workspaceName
+ Microsoft.EnterpriseCloud.Monitoring sekcji rozszerzenia zasobów
+ Wyjściowe, aby wyszukać workspaceId i workspaceSharedKey


```
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "adminUsername": {
      "type": "string",
      "metadata": {
        "description": "Username for the Virtual Machine."
      }
    },
    "adminPassword": {
      "type": "securestring",
      "metadata": {
        "description": "Password for the Virtual Machine."
      }
    },
    "dnsLabelPrefix": {
       "type": "string",
       "metadata": {
          "description": "DNS Label for the Public IP. Must be lowercase. It should match with the following regular expression: ^[a-z][a-z0-9-]{1,61}[a-z0-9]$ or it will raise an error."
       }
    },
    "workspaceId": {
      "type": "string",
      "metadata": {
        "description": "OMS workspace ID"
      }
    },
    "workspaceName": {
      "type": "string",
      "metadata": {
         "description": "OMD workspace name"
      }
    },
    "windowsOSVersion": {
      "type": "string",
      "defaultValue": "2012-R2-Datacenter",
      "allowedValues": [
        "2008-R2-SP1",
        "2012-Datacenter",
        "2012-R2-Datacenter",
        "Windows-Server-Technical-Preview"
      ],
      "metadata": {
        "description": "The Windows version for the VM. This will pick a fully patched image of this given Windows version. Allowed values: 2008-R2-SP1, 2012-Datacenter, 2012-R2-Datacenter, Windows-Server-Technical-Preview."
      }
    }
  },
  "variables": {
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]",
    "apiVersion": "2015-06-15",
    "imagePublisher": "MicrosoftWindowsServer",
    "imageOffer": "WindowsServer",
    "OSDiskName": "osdiskforwindowssimple",
    "nicName": "myVMNic",
    "addressPrefix": "10.0.0.0/16",
    "subnetName": "Subnet",
    "subnetPrefix": "10.0.0.0/24",
    "storageAccountType": "Standard_LRS",
    "publicIPAddressName": "myPublicIP",
    "publicIPAddressType": "Dynamic",
    "vmStorageAccountContainerName": "vhds",
    "vmName": "MyWindowsVM",
    "vmSize": "Standard_DS1",
    "virtualNetworkName": "MyVNET",
    "resourceId": "[resourceGroup().id]",
    "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',variables('virtualNetworkName'))]",
    "subnetRef": "[concat(variables('vnetID'),'/subnets/',variables('subnetName'))]"
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[variables('storageAccountName')]",
      "apiVersion": "[variables('apiVersion')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "accountType": "[variables('storageAccountType')]"
      }
    },
    {
      "apiVersion": "[variables('apiVersion')]",
      "type": "Microsoft.Network/publicIPAddresses",
      "name": "[variables('publicIPAddressName')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "publicIPAllocationMethod": "[variables('publicIPAddressType')]",
        "dnsSettings": {
          "domainNameLabel": "[parameters('dnsLabelPrefix')]"
        }
      }
    },
    {
      "apiVersion": "[variables('apiVersion')]",
      "type": "Microsoft.Network/virtualNetworks",
      "name": "[variables('virtualNetworkName')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "addressSpace": {
          "addressPrefixes": [
            "[variables('addressPrefix')]"
          ]
        },
        "subnets": [
          {
            "name": "[variables('subnetName')]",
            "properties": {
              "addressPrefix": "[variables('subnetPrefix')]"
            }
          }
        ]
      }
    },
    {
      "apiVersion": "[variables('apiVersion')]",
      "type": "Microsoft.Network/networkInterfaces",
      "name": "[variables('nicName')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'))]",
        "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
      ],
      "properties": {
        "ipConfigurations": [
          {
            "name": "ipconfig1",
            "properties": {
              "privateIPAllocationMethod": "Dynamic",
              "publicIPAddress": {
                "id": "[resourceId('Microsoft.Network/publicIPAddresses',variables('publicIPAddressName'))]"
              },
              "subnet": {
                "id": "[variables('subnetRef')]"
              }
            }
          }
        ]
      }
    },
    {
      "apiVersion": "2015-06-15",
      "type": "Microsoft.Compute/virtualMachines",
      "name": "[variables('vmName')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))]",
        "[concat('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
      ],
      "properties": {
        "hardwareProfile": {
          "vmSize": "[variables('vmSize')]"
        },
        "osProfile": {
          "computername": "[variables('vmName')]",
          "adminUsername": "[parameters('adminUsername')]",
          "adminPassword": "[parameters('adminPassword')]"
        },
        "storageProfile": {
          "imageReference": {
            "publisher": "[variables('imagePublisher')]",
            "offer": "[variables('imageOffer')]",
            "sku": "[parameters('windowsOSVersion')]",
            "version": "latest"
          },
          "osDisk": {
            "name": "osdisk",
            "vhd": {
              "uri": "[concat('http://',variables('storageAccountName'),'.blob.core.windows.net/',variables('vmStorageAccountContainerName'),'/',variables('OSDiskName'),'.vhd')]"
            },
            "caching": "ReadWrite",
            "createOption": "FromImage"
          }
        },
        "networkProfile": {
          "networkInterfaces": [
            {
              "id": "[resourceId('Microsoft.Network/networkInterfaces',variables('nicName'))]"
            }
          ]
        },
        "diagnosticsProfile": {
          "bootDiagnostics": {
             "enabled": "true",
             "storageUri": "[concat('http://',variables('storageAccountName'),'.blob.core.windows.net')]"
          }
        }
      },
      "resources": [
        {
          "type": "extensions",
          "name": "Microsoft.EnterpriseCloud.Monitoring",
          "apiVersion": "[variables('apiVersion')]",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
          ],
          "properties": {
            "publisher": "Microsoft.EnterpriseCloud.Monitoring",
            "type": "MicrosoftMonitoringAgent",
            "typeHandlerVersion": "1.0",
            "autoUpgradeMinorVersion": true,
            "settings": {
              "workspaceId": "[parameters('workspaceId')]"
            },
            "protectedSettings": {
              "workspaceKey": "[listKeys(resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspaceName')), '2015-03-20').primarySharedKey]"
            }
          }
        }
      ]
    }
  ],
  "outputs": {
      "sharedKeyOutput": {
         "value": "[listKeys(resourceId('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName')), '2015-03-20').primarySharedKey]",
         "type": "string"
      },
      "workspaceIdOutput": {
         "value": "[reference(concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName')), '2015-03-20').customerId]",
        "type" : "string"
      }
  }
}
```

Szablon można wdrożyć przy użyciu następującego polecenia programu PowerShell:

```
New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateFile $templateFilePath
```

## <a name="troubleshooting-windows-virtual-machines"></a>Rozwiązywanie problemów z maszyn wirtualnych systemu Windows

Jeśli nie instalowania lub raportowania rozszerzenie agenta maszyn wirtualnych *Monitorowania agenta firmy Microsoft* można wykonywać następujące czynności, aby rozwiązać problem.

1. Sprawdzić, czy jest zainstalowany agent maszyn wirtualnych Azure i działać poprawnie, wykonując kroki opisane w [KB 2965986](https://support.microsoft.com/kb/2965986#mt1).
  + Można również sprawdzić plik dziennika agenta maszyn wirtualnych`C:\WindowsAzure\logs\WaAppAgent.log`
  + Jeśli dziennik nie istnieje, nie jest zainstalowany agent maszyn wirtualnych.
    - [Instalowanie agenta maszyn wirtualnych Azure na klasyczny maszyny wirtualne](../virtual-machines/virtual-machines-windows-classic-agents-and-extensions.md)
2. Upewnij się, że zadanie weryfikowania rozszerzenie monitorowania agenta firmy Microsoft jest uruchomiony, wykonaj następujące czynności:
  + Zaloguj się do maszyny wirtualnej
  + Otwórz Harmonogram zadań i Znajdź `update_azureoperationalinsight_agent_heartbeat` zadania
  + Upewnij się, zadanie jest włączony i działa co minutę
  + Ewidencjonowanie pliku dziennika weryfikowania`C:\WindowsAzure\Logs\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\heartbeat.log`
3. Przeglądanie plików dziennika rozszerzenie maszyn wirtualnych agenta firmy Microsoft monitorowania w`C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent`
3. Upewnij się, że maszyna wirtualna może zostać uruchomiony skryptów programu PowerShell
4. Upewnij się, że uprawnienia C:\Windows\temp nie zmieniły
5. Wyświetlanie stanu agenta firmy Microsoft monitorowania, wpisując następujące polecenie w oknie programu PowerShell podwyższonym poziomem uprawnień na komputerze wirtualnych`  (New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg').GetCloudWorkspaces() | Format-List`
6. Przeglądanie plików dziennika konfiguracji monitorowania agenta firmy Microsoft w`C:\Windows\System32\config\systemprofile\AppData\Local\SCOM\Logs`

Aby uzyskać więcej informacji zapoznaj się z [Rozwiązywanie problemów z rozszerzenia systemu Windows](../virtual-machines/virtual-machines-windows-extensions-troubleshoot.md).

## <a name="troubleshooting-linux-virtual-machines"></a>Rozwiązywanie problemów z maszyn wirtualnych Linux

Jeśli rozszerzenie agenta maszyn wirtualnych *Usługi OMS agenta Linux* nie instaluje lub raportowania można wykonywać następujące czynności, aby rozwiązać problem.

1. Jeśli stan rozszerzenia jest *Nieznany* wyboru, jeśli zainstalowano agenta maszyn wirtualnych Azure i działać poprawnie, przeglądanie pliku dziennika agenta maszyn wirtualnych`/var/log/waagent.log`
  + Jeśli dziennik nie istnieje, nie jest zainstalowany agent maszyn wirtualnych.
  - [Instalowanie agenta Azure maszyn wirtualnych na maszyny wirtualne Linux](../virtual-machines/virtual-machines-linux-agent-user-guide.md)
2. Dla innych nieprawidłowe statusy Przejrzyj agenta usługi OMS dla maszyn wirtualnych Linux rozszerzenia plików dziennika `/var/log/azure/Microsoft.EnterpriseCloud.Monitoring.OmsAgentForLinux/*/extension.log` i`/var/log/azure/Microsoft.EnterpriseCloud.Monitoring.OmsAgentForLinux/*/CommandExecution.log`
3. Jeśli stan rozszerzenia jest prawidłowy, ale nie można przekazać danych Przejrzyj agenta usługi OMS Linux pliki dziennika`/var/opt/microsoft/omsagent/log/omsagent.log`

Aby uzyskać więcej informacji zapoznaj się z [Rozwiązywanie problemów z rozszerzeniem Linux](../virtual-machines/virtual-machines-linux-extensions-troubleshoot.md).


## <a name="next-steps"></a>Następne kroki

+ Konfigurowanie [źródeł danych w dzienniku analizy](log-analytics-data-sources.md) Aby określić dzienniki i metryki na potrzeby zbierania.
+ Do zbierania danych z wirtualnej maszyny [rozwiązań Dodawanie analizy dziennika z galerii rozwiązań](log-analytics-add-solutions.md).
+ [Zbieranie danych przy użyciu zestawu narzędzi Diagnostyka Azure](log-analytics-azure-storage.md) dla innych zasobów, które są uruchomione w Azure.

Dla komputerów, które nie są w Azure możesz zainstalować agenta analizy dziennika przy użyciu metod opisanych w następujących artykułach:

+ [Nawiązywanie połączenia komputerów z systemem Windows z analizy dziennika](log-analytics-windows-agents.md)
+ [Łączenie komputerów Linux do analizy dziennika](log-analytics-linux-agents.md)
