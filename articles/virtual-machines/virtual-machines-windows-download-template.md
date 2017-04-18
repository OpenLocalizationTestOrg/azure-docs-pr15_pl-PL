<properties
    pageTitle="Utworzyć obraz maszyn wirtualnych z maszyn wirtualnych Azure | Microsoft Azure"
    description="Dowiedz się, jak utworzyć obraz uogólniony maszyn wirtualnych z utworzone w modelu wdrożenia Menedżera zasobów istniejących maszyn wirtualnych Azure"
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
    ms.date="10/10/2016"
    ms.author="cynthn"/>


# <a name="download-the-template-for-a-vm"></a>Pobieranie szablonu dla maszyn wirtualnych

Po utworzeniu maszyny w Azure za pomocą portalu lub programu PowerShell szablonu Menedżera zasobów są tworzone automatycznie. Ten szablon umożliwia szybkie duplikowanie wdrożeniu. Szablon zawiera informacje o wszystkie zasoby w grupie zasobów. W przypadku maszyny wirtualnej oznacza to kontenery szablonu wszystko, co jest tworzona w celu spełnienia maszyn wirtualnych w tej grupie zasobów, w tym zasoby sieciowe.

## <a name="download-the-template-using-the-portal"></a>Pobieranie szablonu za pomocą portalu

1. Zaloguj się do [portalu Azure](https://portal.azure.com/).
2. Wybierz jedną z menu Centrum **maszyn wirtualnych**.
3. Z listy wybierz maszyny wirtualnej.
5. Wybierz **skrypt automatyzacji**.
6. Zaznacz pole wyboru **Pobierz** i Zapisz plik zip na komputerze lokalnym.
7. Otwórz plik zip i wyodrębnić pliki do folderu. Plik zip będzie zawierać:
    
    - Deploy.ps1
    - Deploy.sh 
    - deployer.RB
    - DeploymentHelper.cs
    - parameters.JSON
    - Template.JSON

Plik .json jest szablon.
    
## <a name="download-the-template-using-powershell"></a>Pobieranie szablonu za pomocą programu PowerShell

Możesz również pobrać plik szablonu .json przy użyciu polecenia cmdlet [AzureRMResourceGroup eksportu](https://msdn.microsoft.com/library/mt715427.aspx) . Możesz użyć `-path` parametru o podanie nazwy i ścieżki pliku .json. W tym przykładzie pokazano sposób pobrać szablon o nazwie **myResourceGroup** do folderu **C:\users\public\downloads** na komputerze lokalnym grupy zasobów.

```powershell
    Export-AzureRmResourceGroup -ResourceGroupName "myResourceGroup" -Path "C:\users\public\downloads"
```

## <a name="next-steps"></a>Następne kroki

Aby dowiedzieć się więcej na temat wdrażania zasobów za pomocą szablonów, zobacz [Instruktaż szablonu Menedżera zasobów](../resource-manager-template-walkthrough.md).