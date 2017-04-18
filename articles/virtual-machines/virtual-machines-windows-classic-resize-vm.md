<properties
    pageTitle="Zmienianie rozmiaru klasyczny maszyn wirtualnych systemu Windows | Microsoft Azure"
    description="Zmienianie rozmiaru utworzone w modelu Klasyczny wdrożenia, przy użyciu programu Powershell Azure maszyny wirtualnej systemu Windows."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="Drewm3"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="drewm"/>


# <a name="resize-a-windows-vm-created-in-the-classic-deployment-model"></a>Zmienianie rozmiaru maszyn wirtualnych systemu Windows utworzone w modelu Klasyczny wdrażania

W tym artykule przedstawiono sposób zmieniania rozmiaru maszyn wirtualnych systemu Windows, utworzone w modelu Klasyczny wdrażania przy użyciu programu Powershell Azure.

Zastanawiając zmienianie rozmiarów maszyny, istnieją dwa pojęcia, które kontrolę zakresu rozmiary dostępne w celu zmiany rozmiaru maszyny wirtualnej. Koncepcja pierwszy jest region, w którym jest używany do maszyn wirtualnych. Na liście dostępne formaty maszyn wirtualnych w regionie znajduje się w obszarze karty usługi Azure regionów strony sieci web. Koncepcja druga jest fizycznie sprzęt obecnie hostingu swojej maszyn wirtualnych. Serwerów fizycznych hostingu maszyny wirtualne są pogrupowane w klastrów typowych sprzętu. Metoda zmieniania rozmiaru maszyn wirtualnych różni się w zależności od odpowiedniej nowy rozmiar pamięci Wirtualnej jest obsługiwana przez klaster sprzętu aktualnie obsługujący maszyn wirtualnych.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Można też [zmienić rozmiar maszyny utworzone w modelu wdrożenia Menedżera zasobów](virtual-machines-windows-resize-vm.md).


## <a name="add-your-account"></a>Dodawanie konta

Musisz skonfigurować Azure programu PowerShell do pracy z zasobami Azure klasyczny. Wykonaj poniższe czynności, aby skonfigurować Azure programu PowerShell do zarządzania zasobami klasyczny.

1. W wierszu polecenia programu PowerShell wpisz `Add-AzureAccount` i naciśnij klawisz **Enter**. 
2. Wpisz adres e-mail skojarzonego z subskrypcją usługi Azure i kliknij przycisk **Kontynuuj**. 
3. Wpisz hasło do konta. 
4. Kliknij przycisk **Zaloguj**. 


## <a name="resize-in-the-same-hardware-cluster"></a>Zmienianie rozmiaru w tym samym klastrze sprzętu

Aby zmienić rozmiar maszyn wirtualnych rozmiar dostępnych w grupie sprzętu hostingu maszyn wirtualnych, wykonaj następujące czynności.

1. Uruchom następujące polecenie programu PowerShell, aby wyświetlić listę dostępnych w grupie sprzętu hostingu usługi w chmurze, który zawiera maszyn wirtualnych rozmiarów maszyn wirtualnych.

    ```powershell
    Get-AzureService | where {$_.ServiceName -eq "<cloudServiceName>"}
    ```

2. Uruchom następujące polecenia w celu zmiany rozmiaru maszyn wirtualnych.

    ```powershell
    Get-AzureVM -ServiceName <cloudServiceName> -Name <vmName> | Set-AzureVMSize -InstanceSize <newVMSize> | Update-AzureVM
    ```

## <a name="resize-on-a-new-hardware-cluster"></a>Zmienianie rozmiaru w klastrze nowego sprzętu

Aby zmienić rozmiar maszyn wirtualnych o rozmiarze nie są dostępne w grupie sprzętu hostingu Głosowa, chmury można odtworzyć usługi i wszystkie maszyny wirtualne w usłudze w chmurze. Każdej usługi w chmurze jest hostowana w klastrze pojedynczej sprzętu, więc wszystkie maszyny wirtualne w usłudze w chmurze muszą być rozmiar, który jest obsługiwany w klastrze sprzętu. W poniższej procedurze zostanie opisano sposób zmieniania rozmiaru maszyny przez usunięcie i ponowne utworzenie usług w chmurze.

1. Uruchom następujące polecenie programu PowerShell, aby wyświetlić listę dostępnych rozmiarów maszyn wirtualnych w regionie. 

    ```powershell
    Get-AzureLocation | where {$_.Name -eq "<locationName>"}
    ```

2. Zanotuj wszystkie ustawienia konfiguracji dla każdego maszyn wirtualnych w usłudze w chmurze, który zawiera maszyn wirtualnych można zmienić. 
3. Usuwanie wszystkich maszyny wirtualne w wybranie opcji przechowywania dysków dla każdego maszyn wirtualnych usługi w chmurze.
4. Ponownie utwórz maszyn wirtualnych można zmienić przy użyciu odpowiedniej rozmiar pamięci Wirtualnej.
5. Ponownie utwórz wszystkie inne maszyny wirtualne będące w usłudze w chmurze za pomocą rozmiar pamięci Wirtualnej dostępnych w grupie sprzętu teraz hostingu usługi w chmurze.

Przykładowy skrypt dla usunięcie i ponowne utworzenie usługi w chmurze za pomocą nowy rozmiar pamięci Wirtualnej można znaleźć [tutaj](https://github.com/Azure/azure-vm-scripts). 


## <a name="next-steps"></a>Następne kroki

- [Zmienianie rozmiaru maszyny utworzone w modelu wdrożenia Menedżera zasobów](virtual-machines-windows-resize-vm.md).