<properties
    pageTitle="Migrowanie do Menedżera zasobów przy użyciu programu PowerShell | Microsoft Azure"
    description="W tym artykule opisano za pośrednictwem obsługiwane platformy migracji zasobów IaaS od klasycznych do Menedżera zasobów Azure za pomocą polecenia programu PowerShell Azure"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="cynthn"/>

# <a name="migrate-iaas-resources-from-classic-to-azure-resource-manager-by-using-azure-powershell"></a>Migrowanie IaaS zasoby z klasycznym do Menedżera zasobów Azure przy użyciu programu PowerShell Azure

Te kroki pokazano, jak przeprowadzić migrację infrastruktury jako zasoby usługi (IaaS) z modelu Klasyczny wdrożenia do modelu wdrożenia Menedżera zasobów Azure za pomocą polecenia programu PowerShell Azure. 

Jeśli chcesz, możesz także migrować zasobów przy użyciu [interfejsu wiersza polecenia Azure (polecenie Azure)](virtual-machines-linux-cli-migration-classic-resource-manager.md).

- Obsługiwane scenariusze migracji, zobacz [Obsługiwane platformy migracji zasobów IaaS z klasycznej do Menedżera zasobów Azure](virtual-machines-windows-migration-classic-resource-manager.md). 
- Szczegółowe wskazówki i skorzystać z migracji zobacz [techniczne dive głębokości obsługiwane platformy migracji z klasycznej do Menedżera zasobów Azure](virtual-machines-windows-migration-classic-resource-manager-deep-dive.md).

## <a name="step-1-plan-for-migration"></a>Krok 1: Planowanie migracji

Poniżej przedstawiono kilka najważniejsze wskazówki, które zaleca się podczas oceny Migrowanie IaaS zasoby z klasycznej do Menedżera zasobów:

- Zapoznaj się z [obsługiwanych i nieobsługiwanych funkcji i konfiguracji](virtual-machines-windows-migration-classic-resource-manager.md). Jeśli masz maszyn wirtualnych, które funkcji nieobsługiwanych konfiguracji i funkcji, zalecamy poczekać obsługę konfiguracji i funkcji do ogłaszane. Jeśli wydaje się odpowiedni, Usuń tę funkcję lub przenieść z tej konfiguracji w celu umożliwienia migracji.
-   Jeśli masz automatycznego skrypty wdrażanie usługi infrastruktury i aplikacji dzisiaj, spróbuj utworzyć podobne konfiguracji testu przy użyciu tych skryptów migracji. Alternatywnie możesz skonfigurować środowiskach próbki za pomocą portalu Azure.

>[AZURE.IMPORTANT]Wirtualna sieć bramy (-— witryny, Azure ExpressRoute, bramy aplikacji, punktu do witryny) nie są obecnie obsługiwane migracji od klasycznych do Menedżera zasobów. Aby przeprowadzić migrację klasyczny wirtualnej sieci z bramą, należy usunąć bramę przed uruchomieniem czas trwania operacji przekazywania do przenieść sieć (bez usuwania bramy można uruchamiać kroku Przygotuj). Po zakończeniu migracji ponownie nawiązać połączenie bramy w Menedżerze zasobów Azure.

## <a name="step-2-install-the-latest-version-of-azure-powershell"></a>Krok 2: Zainstaluj najnowszą wersję programu Azure PowerShell

Istnieją dwa główne opcje instalacji Azure programu PowerShell: [Galerii programu PowerShell](https://www.powershellgallery.com/profiles/azure-sdk/) lub [Instalatora platformy sieci Web (WebPI)](http://aka.ms/webpi-azps). WebPI otrzymuje comiesięczna aktualizacja. Galeria programu PowerShell otrzymywania aktualizacji w sposób ciągły. Ten artykuł jest oparty na Azure programu PowerShell wersji 2.1.0.

Instrukcje dotyczące instalacji zobacz [jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md).

<br>
## <a name="step-3-set-your-subscription-and-sign-up-for-migration"></a>Krok 3: Ustawianie subskrypcji i zaloguj do migracji

Najpierw uruchom wiersz programu PowerShell. Migracji, musisz skonfigurować środowiska dla obu klasyczny i Menedżera zasobów.

Zaloguj się do konta z modelu Menedżera zasobów.

```powershell
    Login-AzureRmAccount
```

Dostępne subskrypcje można uzyskać, używając następującego polecenia:

```powershell
    Get-AzureRMSubscription | Sort SubscriptionName | Select SubscriptionName
```

Ustawianie subskrypcji Azure dla bieżącej sesji. W tym przykładzie ustawiono **Moją subskrypcję Azure**domyślną nazwę subskrypcji. Zamień nazwę subskrypcji przykład własne. 

```powershell
    Select-AzureRmSubscription –SubscriptionName "My Azure Subscription"
```

>[AZURE.NOTE] Rejestracja jest jednorazowego kroku, ale należy je raz przed podjęciem migracji. Bez rejestrowania, pojawi się następujący komunikat o błędzie: 

>   *BadRequest: Subskrypcja nie jest zarejestrowany migracji.* 

Rejestrować dostawcy zasobów migracji przy użyciu następującego polecenia:

```powershell
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate
```

Poczekaj pięć minut rejestracji zakończyć. Możesz sprawdzić stan zatwierdzenia przy użyciu następującego polecenia:

```powershell
    Get-AzureRmResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate
```

Upewnij się, że jest RegistrationState `Registered` przed kontynuowaniem. 

Teraz Zaloguj się do swojego konta dla modelu Klasyczny.

```powershell
    Add-AzureAccount
```

Dostępne subskrypcje można uzyskać, używając następującego polecenia:

```powershell
    Get-AzureSubscription | Sort SubscriptionName | Select SubscriptionName
```

Ustawianie subskrypcji Azure dla bieżącej sesji. Ten przykład przedstawia Domyślna subskrypcja **Moją subskrypcję Azure**. Zamień nazwę subskrypcji przykład własne. 

```powershell
    Select-AzureSubscription –SubscriptionName "My Azure Subscription"
```

<br>

## <a name="step-4-make-sure-you-have-enough-azure-resource-manager-virtual-machine-cores-in-the-azure-region-of-your-current-deployment-or-vnet"></a>Krok 4: Upewnij się, że masz za mało rdzenie maszyn wirtualnych Menedżera zasobów Azure w regionie Azure bieżącego wdrożenia lub VNET

Za pomocą następującego polecenia programu PowerShell sprawdzania numeru bieżącej rdzeni, które masz w Menedżerze zasobów Azure. Aby dowiedzieć się więcej na temat podstawowych przydziałów, zobacz [limity i Azure Menedżera zasobów](../azure-subscription-service-limits.md#limits-and-the-azure-resource-manager). 

W tym przykładzie sprawdza dostępność w regionie **Zachód USA** . Zamień nazwę regionu przykładowy na własny. 

```powershell
Get-AzureRmVMUsage -Location "West US"
```

## <a name="step-5-run-commands-to-migrate-your-iaas-resources"></a>Krok 5: Uruchomienie poleceń, aby przeprowadzić migrację zasobów IaaS

>[AZURE.NOTE] Wszystkie operacje opisane w tym miejscu są idempotent. Jeśli masz problem z inną niż funkcji nieobsługiwanych lub błąd konfiguracji zalecamy ponów próbę Przygotuj, przerwania lub przekazania operacji. Platformy próbuje ją ponownie.

### <a name="migrate-virtual-machines-in-a-cloud-service-not-in-a-virtual-network"></a>Migrowanie maszyn wirtualnych w usłudze w chmurze (nie w wirtualnej sieci)

Uzyskiwanie listy usług w chmurze przy użyciu następującego polecenia, a następnie wybierz usługi w chmurze, który chcesz przeprowadzić migrację. W przypadku maszyny wirtualne w usłudze w chmurze w wirtualnej sieci lub mają ról w sieci web lub pracownika, polecenie zwraca komunikat o błędzie.

```powershell
    Get-AzureService | ft Servicename
```

Uzyskaj nazwę wdrożenia usługi w chmurze. W tym przykładzie nazwa usługi jest **Moja usługa**. Zamień nazwę usługi przykład nazwę usługi. 

```powershell
    $serviceName = "My Service"
    $deployment = Get-AzureDeployment -ServiceName $serviceName
    $deploymentName = $deployment.DeploymentName
```

Przygotowywanie maszyn wirtualnych w usłudze w chmurze migracji. Istnieją dwie opcje do wyboru.

* **Opcja 1. Migrowanie maszyn wirtualnych do wirtualnej sieci utworzone platformy**

    Najpierw sprawdź poprawność w przypadku migrowania usług w chmurze za pomocą następujących poleceń:

    ```powershell
    $validate = Move-AzureService -Validate -ServiceName $serviceName `
        -DeploymentName $deploymentName -CreateNewVirtualNetwork
    $validate.ValidationMessages
    ```

    Poprzednie polecenie wyświetla wszelkie ostrzeżenia i błędy, które blokują migracji. Jeśli sprawdzanie poprawności zakończyło się pomyślnie, następnie na można przenosić do kroku **Przygotuj** :

    ```powershell
    Move-AzureService -Prepare -ServiceName $serviceName `
        -DeploymentName $deploymentName -CreateNewVirtualNetwork
    ```

* **Opcja 2. Migrowanie do istniejącej sieci wirtualnej w modelu wdrożenia Menedżera zasobów**

    W tym przykładzie nazwa grupy zasobów **myResourceGroup**, wirtualną sieć nazwę, aby **myVirtualNetwork** i nazwę podsieci **mySubNet**. Zamień imiona i nazwiska w tym przykładzie nazw własnych zasobów.

    ```powershell
    $existingVnetRGName = "myResourceGroup"
    $vnetName = "myVirtualNetwork"
    $subnetName = "mySubNet"
    ```

    Najpierw sprawdź poprawność w przypadku migrowania usług w chmurze przy użyciu następującego polecenia:

    ```powershell
    $validate = Move-AzureService -Validate -ServiceName $serviceName `
        -DeploymentName $deploymentName -UseExistingVirtualNetwork -VirtualNetworkResourceGroupName $existingVnetRGName -VirtualNetworkName $vnetName -SubnetName $subnetName
    $validate.ValidationMessages
    ```

    Poprzednie polecenie wyświetla wszelkie ostrzeżenia i błędy, które blokują migracji. Jeśli sprawdzanie poprawności zakończyło się pomyślnie, można przystąpić do Przygotuj następujące czynności:

    ```powershell
    Move-AzureService -Prepare -ServiceName $serviceName -DeploymentName $deploymentName `
        -UseExistingVirtualNetwork -VirtualNetworkResourceGroupName $existingVnetRGName `
        -VirtualNetworkName $vnetName -SubnetName $subnetName
    ```
    

Po pomyślnym Operacja przygotowywania przy użyciu jednej z powyższych opcji kwerendy stan migracji maszyny wirtualne. Upewnij się, że są one w `Prepared` stan.

W tym przykładzie ustawia nazwę maszyn wirtualnych **myVM**. Zamień nazwę przykład z nazwą maszyn wirtualnych.

    ```powershell
    $vmName = "myVM"
    $vm = Get-AzureVM -ServiceName $serviceName -Name $vmName
    $vm.VM.MigrationState
    ```

Sprawdź konfigurację przygotowany zasobów przy użyciu programu PowerShell lub Azure portal. Jeśli nie jest jeszcze gotowa do migracji i chcesz wrócić do stanu stare, użyj następującego polecenia:

```powershell
    Move-AzureService -Abort -ServiceName $serviceName -DeploymentName $deploymentName
```

Jeśli przygotowany konfiguracji odpowiada Twoim potrzebom, możesz Przechodzenie do przodu i zatwierdzanie zasobów przy użyciu następującego polecenia:

```powershell
    Move-AzureService -Commit -ServiceName $serviceName -DeploymentName $deploymentName
```

### <a name="migrate-virtual-machines-in-a-virtual-network"></a>Migrowanie maszyn wirtualnych w wirtualnej sieci

Aby przeprowadzić migrację maszyn wirtualnych w wirtualnej sieci, możesz przeprowadzić migrację sieci. Maszyn wirtualnych automatycznie przeprowadzić migrację z siecią. Wybierz wirtualnej sieci, który chcesz przeprowadzić migrację. 

W tym przykładzie ustawia nazwę wirtualnej sieci **myVnet**. Zamień nazwę wirtualną sieć przykładowy na własny. 

```powershell
    $vnetName = "myVnet"
```

>[AZURE.NOTE] Jeśli wirtualną sieć zawiera sieci web lub role pracownik lub maszyny wirtualne z nieobsługiwane konfiguracji, zostanie wyświetlony komunikat o błędzie sprawdzania poprawności.

Najpierw sprawdź poprawność w przypadku migrowania wirtualnej sieci przy użyciu następującego polecenia:

```powershell
    Move-AzureVirtualNetwork -Validate -VirtualNetworkName $vnetName
```

Poprzednie polecenie wyświetla wszelkie ostrzeżenia i błędy, które blokują migracji. Jeśli sprawdzanie poprawności zakończyło się pomyślnie, można przystąpić do Przygotuj następujące czynności:

```powershell
    Move-AzureVirtualNetwork -Prepare -VirtualNetworkName $vnetName
```

Sprawdź konfigurację dla przygotowany maszyn wirtualnych przy użyciu programu PowerShell Azure lub Azure portal. Jeśli nie jest jeszcze gotowa do migracji i chcesz wrócić do stanu stare, użyj następującego polecenia:

```powershell
    Move-AzureVirtualNetwork -Abort -VirtualNetworkName $vnetName
```

Jeśli przygotowany konfiguracji odpowiada Twoim potrzebom, możesz Przechodzenie do przodu i zatwierdzanie zasobów przy użyciu następującego polecenia:

```powershell
    Move-AzureVirtualNetwork -Commit -VirtualNetworkName $vnetName
```

### <a name="migrate-a-storage-account"></a>Migrowanie konta miejsca do magazynowania

Po zakończeniu migracji maszyn wirtualnych, zaleca się Migrowanie kont miejsca do magazynowania.

Przygotowywanie każdego konta magazynu migracji przy użyciu następującego polecenia. W tym przykładzie nazwę konta magazynu jest **myStorageAccount**. Zamień nazwę przykład nazwę konta magazynu. 

```powershell
    $storageAccountName = "myStorageAccount"
    Move-AzureStorageAccount -Prepare -StorageAccountName $storageAccountName
```

Sprawdź konfigurację konta przygotowany miejsca do magazynowania przy użyciu programu PowerShell Azure lub Azure portal. Jeśli nie jest jeszcze gotowa do migracji i chcesz wrócić do stanu stare, użyj następującego polecenia:

```powershell
    Move-AzureStorageAccount -Abort -StorageAccountName $storageAccountName
```

Jeśli przygotowany konfiguracji odpowiada Twoim potrzebom, możesz Przechodzenie do przodu i zatwierdzanie zasobów przy użyciu następującego polecenia:

```powershell
    Move-AzureStorageAccount -Commit -StorageAccountName $storageAccountName
```

## <a name="next-steps"></a>Następne kroki

- Aby uzyskać więcej informacji o migracji zobacz [Obsługiwane platformy migracji zasobów IaaS z klasycznej do Menedżera zasobów Azure](virtual-machines-windows-migration-classic-resource-manager.md).
- Aby przeprowadzić migrację dodatkowych zasobów sieciowych do Menedżera zasobów przy użyciu programu PowerShell Azure, za pomocą podobne kroki [Przenieś AzureNetworkSecurityGroup](https://msdn.microsoft.com/library/mt786729.aspx), [Przenieś AzureReservedIP](https://msdn.microsoft.com/library/mt786752.aspx)i [Przenieś AzureRouteTable](https://msdn.microsoft.com/library/mt786718.aspx).
- Otwórz źródło skryptów, których możesz przeprowadzić migrację Azure zasoby z klasycznej do Menedżera zasobów zobacz [społeczności narzędzia do migracji do migracji Menedżera zasobów Azure](virtual-machines-windows-migration-scripts.md)
