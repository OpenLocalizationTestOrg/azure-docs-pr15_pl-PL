<properties
    pageTitle="Migrowanie IaaS zasoby z klasycznym do Menedżera zasobów Azure za pomocą interfejsu wiersza polecenia Azure | Microsoft Azure"
    description="W tym artykule opisano za pośrednictwem obsługiwane platformy migracji zasobów, od klasycznych do Menedżera zasobów Azure za pomocą interfejsu wiersza polecenia Azure"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/19/2016"
    ms.author="cynthn"/>

# <a name="migrate-iaas-resources-from-classic-to-azure-resource-manager-by-using-azure-cli"></a>Migrowanie IaaS zasoby z klasycznym do Menedżera zasobów Azure za pomocą interfejsu wiersza polecenia Azure

Te kroki pokazano, jak przeprowadzić migrację infrastruktury jako zasoby usługi (IaaS) z modelu Klasyczny wdrożenia do modelu wdrożenia Menedżera zasobów Azure przy użyciu poleceń Azure interfejs wiersza polecenia (polecenie). Artykuł wymaga [Polecenie Azure](../xplat-cli-install.md).

>[AZURE.NOTE] Wszystkie operacje opisane w tym miejscu są idempotent. Jeśli masz problem z inną niż funkcji nieobsługiwanych lub błąd konfiguracji zalecamy ponów próbę Przygotuj, przerwania lub przekazania operacji. Platformy spróbuj ją ponownie.

## <a name="step-1-prepare-for-migration"></a>Krok 1: Przygotowywanie do migracji

Poniżej przedstawiono kilka najważniejsze wskazówki, które zaleca się podczas oceny Migrowanie IaaS zasoby z klasycznej do Menedżera zasobów:

- Zapoznaj się z [listy nieobsługiwaną konfiguracją lub funkcji](virtual-machines-windows-migration-classic-resource-manager.md). Jeśli masz maszyn wirtualnych, które funkcji nieobsługiwanych konfiguracji i funkcji, zalecamy poczekać możliwość konfiguracji i funkcji ogłaszane. Można także usunąć tę funkcję lub przenieść z tej konfiguracji włączenie migracji wymaganiom użytkownika.
-   Jeśli masz automatycznego skrypty wdrażanie usługi infrastruktury i aplikacji dzisiaj, spróbuj utworzyć podobne konfiguracji testu przy użyciu tych skryptów migracji. Alternatywnie możesz skonfigurować środowiskach próbki za pomocą portalu Azure.

## <a name="step-2-set-your-subscription-and-register-the-provider"></a>Krok 2: Ustawianie subskrypcji i zarejestrowania dostawcy

W przypadku scenariuszy migracji należy skonfigurować środowiska dla obu klasyczny i Menedżera zasobów. [Instalowanie polecenie Azure](../xplat-cli-install.md) i [Wybierz subskrypcję](../xplat-cli-connect.md).

Zaloguj się do swojego konta.
    
    azure login

Wybierz subskrypcję, Azure za pomocą następującego polecenia.

    azure account set "<azure-subscription-name>"

>[AZURE.NOTE] Rejestracja jest krok czasu, ale musi zostać wykonane raz przed podjęciem migracji. Bez rejestrowania pojawi się następujący komunikat o błędzie 

>   *BadRequest: Subskrypcja nie jest zarejestrowany migracji.* 

Zarejestruj się z dostawcą zasobów migracji przy użyciu następującego polecenia. Należy zauważyć, że w niektórych przypadkach to polecenie przekroczenie limitu czasu. Jednak rejestracji zakończy się pomyślnie.

    azure provider register Microsoft.ClassicInfrastructureMigrate

Poczekaj pięć minut rejestracji zakończyć. Możesz sprawdzić stan zatwierdzenia przy użyciu następującego polecenia. Upewnij się, że jest RegistrationState `Registered` przed kontynuowaniem.

    azure provider show Microsoft.ClassicInfrastructureMigrate

Teraz przełączać się polecenie do `asm` tryb.

    azure config mode asm

## <a name="step-3-make-sure-you-have-enough-azure-resource-manager-virtual-machine-cores-in-the-azure-region-of-your-current-deployment-or-vnet"></a>Krok 3: Upewnij się, że masz za mało rdzenie maszyn wirtualnych Menedżera zasobów Azure w regionie Azure bieżącego wdrożenia lub VNET

W tym kroku musisz przełączać się do `arm` tryb. To zrobić przy użyciu następującego polecenia.

```
azure config mode arm
```

Następujące polecenie interfejsu wiersza polecenia służy do Sprawdź bieżącą ilość rdzenie, które masz w Menedżerze zasobów Azure. Aby dowiedzieć się więcej na temat podstawowych przydziałów, zobacz [limity i Menedżer zasobów Azure](../articles/azure-subscription-service-limits.md#limits-and-the-azure-resource-manager)

```
azure vm list-usage -l "<Your VNET or Deployment's Azure region"
```

Po zakończeniu sprawdzania ten krok, możesz przełączać się do `asm` tryb.

    azure config mode asm


## <a name="step-4-option-1---migrate-virtual-machines-in-a-cloud-service"></a>Krok 4: Opcja 1 - Migrowanie maszyn wirtualnych w usłudze w chmurze 

Uzyskiwanie listy usług w chmurze przy użyciu następującego polecenia, a następnie wybierz usługi w chmurze, który chcesz przeprowadzić migrację. Należy zauważyć, że jeśli maszyny wirtualne w usłudze w chmurze w wirtualnej sieci lub jeśli mają ról w sieci web i pracownika, zostanie wyświetlony komunikat o błędzie.

    azure service list

Uruchom następujące polecenie, aby uzyskać nazwę wdrożenia usługi w chmurze z pełne dane wyjściowe. W większości przypadków nazwa wdrożenia jest taka sama jak nazwa usługi w chmurze.

    azure service show <serviceName> -vv

Przygotowywanie maszyn wirtualnych w usłudze w chmurze migracji. Istnieją dwie opcje do wyboru.

Jeśli chcesz Migrowanie maszyn wirtualnych do utworzonych platformy wirtualnej sieci, użyj następującego polecenia.

    azure service deployment prepare-migration <serviceName> <deploymentName> new "" "" ""

Jeśli chcesz przeprowadzić migrację do istniejącej sieci wirtualnej w modelu wdrożenia Menedżera zasobów, użyj następującego polecenia.

    azure service deployment prepare-migration <serviceName> <deploymentName> existing <destinationVNETResourceGroupName> subnetName <vnetName>

Po tej operacji Przygotuj się pomyślnie, możesz przejrzeć pełne dane wyjściowe do uzyskania stanu migracji maszyny wirtualne i upewnij się, że są one w `Prepared` stan.

    azure vm show <vmName> -vv

Sprawdź konfigurację przygotowany zasobów za pomocą interfejsu wiersza polecenia lub Azure portal. Jeśli nie jest jeszcze gotowa do migracji i chcesz wrócić do stanu stare, użyj następującego polecenia.

    azure service deployment abort-migration <serviceName> <deploymentName>

Jeśli konfiguracja przygotowany odpowiada Twoim potrzebom, możesz Przechodzenie do przodu i zatwierdź zasobów przy użyciu następującego polecenia.

    azure service deployment commit-migration <serviceName> <deploymentName>


    
## <a name="step-4-option-2----migrate-virtual-machines-in-a-virtual-network"></a>Krok 4: Opcja 2 - Migrowanie maszyn wirtualnych w wirtualnej sieci

Wybierz wirtualnej sieci, który chcesz przeprowadzić migrację. Należy zauważyć, że jeśli wirtualną sieć zawiera ról w sieci web i pracownik lub maszyny wirtualne z nieobsługiwane konfiguracji, zostanie wyświetlony komunikat o błędzie sprawdzania poprawności.

Uzyskaj wirtualnych sieci w subskrypcji przy użyciu następującego polecenia.

    azure network vnet list
    
Wynik będzie wyglądał następująco:

![Zrzut ekranu przedstawiający wiersza polecenia o nazwie całego wirtualną sieć wyróżnione.](./media/virtual-machines-linux-cli-migration-classic-resource-manager/vnet.png)

W powyższym przykładzie **virtualNetworkName** jest całą nazwę **"Grupy classicubuntu16 classicubuntu16"**.

Przygotowywanie wirtualna sieć wyboru migracji przy użyciu następującego polecenia.

    azure network vnet prepare-migration <virtualNetworkName>

Sprawdź konfigurację dla przygotowany maszyn wirtualnych za pomocą interfejsu wiersza polecenia lub Azure portal. Jeśli nie jest jeszcze gotowa do migracji i chcesz wrócić do stanu stare, użyj następującego polecenia.

    azure network vnet abort-migration <virtualNetworkName>

Jeśli konfiguracja przygotowany odpowiada Twoim potrzebom, możesz Przechodzenie do przodu i zatwierdź zasobów przy użyciu następującego polecenia.

    azure network vnet commit-migration <virtualNetworkName>

## <a name="step-5-migrate-a-storage-account"></a>Krok 5: Migrowanie konta miejsca do magazynowania

Po zakończeniu migracji maszyn wirtualnych, zaleca się Migrowanie konta miejsca do magazynowania.

Przygotowywanie konta magazynu migracji przy użyciu następującego polecenia

    azure storage account prepare-migration <storageAccountName>

Sprawdź konfigurację konta przygotowany miejsca do magazynowania przy użyciu interfejsu wiersza polecenia lub Azure portal. Jeśli nie jest jeszcze gotowa do migracji i chcesz wrócić do stanu stare, użyj następującego polecenia.

    azure storage account abort-migration <storageAccountName>

Jeśli konfiguracja przygotowany odpowiada Twoim potrzebom, możesz Przechodzenie do przodu i zatwierdź zasobów przy użyciu następującego polecenia.

    azure storage account commit-migration <storageAccountName>

## <a name="next-steps"></a>Następne kroki

- [Obsługiwane platformy migracji zasobów IaaS od klasycznych do Menedżera zasobów](virtual-machines-windows-migration-classic-resource-manager.md)
- [Techniczne głębokości dive obsługiwane platformy migracji od klasycznych do Menedżera zasobów](virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)
