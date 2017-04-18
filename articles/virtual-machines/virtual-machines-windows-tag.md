<properties
   pageTitle="Jak oznaczać maszyny | Microsoft Azure"
   description="Więcej informacji na temat znakowania maszyny wirtualnej systemu Windows utworzone w Azure przy użyciu modelu wdrożenia Menedżera zasobów"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="mmccrory"
   manager="timlt"
   editor="tysonn"
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="07/05/2016"
   ms.author="memccror"/>

# <a name="how-to-tag-a-windows-virtual-machine-in-azure"></a>Jak oznaczać maszyny wirtualnej Windows platformy Azure


W tym artykule opisano różne sposoby znakowanie maszyny wirtualnej systemu Windows w Azure za pomocą modelu wdrożenia Menedżera zasobów. Tagi są pary klucz wartość zdefiniowane przez użytkownika, które mogą być umieszczone bezpośrednio na zasób lub grupa zasobów. Azure obsługuje obecnie maksymalnie 15 znaczników dla zasobu i grupy zasobów. Znaczniki mogą być umieszczone na zasób w momencie tworzenia lub dodane do istniejącego zasobu. Należy pamiętać, że znaczniki są obsługiwane dla zasobów utworzone za pomocą tylko model wdrożenia Menedżera zasobów. Jeśli chcesz oznakować maszyny wirtualnej Linux, zobacz [jak znakowanie maszyny wirtualnej Linux Azure](virtual-machines-linux-tag.md).

[AZURE.INCLUDE [virtual-machines-common-tag](../../includes/virtual-machines-common-tag.md)]

## <a name="tagging-with-powershell"></a>Znakowanie przy użyciu programu PowerShell

Aby utworzyć, dodawanie i usuwanie znaczników przy użyciu programu PowerShell, pierwszy konieczne jest skonfigurowanie [środowiska PowerShell przy użyciu Menedżera zasobów Azure][]. Po zakończeniu instalacji, możesz umieścić znaczniki obliczeń, sieci i magazynu zasobów podczas tworzenia lub zasobu została utworzona przy użyciu programu PowerShell. W tym artykule będzie skup się na wyświetlania i edytowania znaczników umieszczana w środowisku maszyn wirtualnych systemu.

Najpierw Przejdź maszyn wirtualnych za pośrednictwem `Get-AzureRmVM` polecenia cmdlet.

        PS C:\> Get-AzureRmVM -ResourceGroupName "MyResourceGroup" -Name "MyTestVM"

Jeśli komputer wirtualnych zawiera już znaczniki, następnie pojawią się wszystkie znaczniki do zasobu:

        Tags : {
                "Application": "MyApp1",
                "Created By": "MyName",
                "Department": "MyDepartment",
                "Environment": "Production"
               }

Jeśli chcesz dodać znaczniki przy użyciu programu PowerShell, możesz użyć `Set-AzureRmResource` polecenia. Uwaga podczas aktualizowania znaczników przy użyciu programu PowerShell, znaczniki są aktualizowane jako całość. Dlatego jeśli dodajesz jeden tag do zasobu, która już zawiera znaczniki, należy uwzględnić wszystkie znaczniki, które chcesz umieścić na zasób. Oto przykładowy sposób dodać kolejne tagi do zasobu za pomocą poleceń cmdlet środowiska PowerShell.

To polecenie cmdlet pierwszego ustawia wszystkich znaczników umieszczana w *MyTestVM* zmiennej *$tags* za pomocą `Get-AzureRmResource` i `Tags` właściwości.

        PS C:\> $tags = (Get-AzureRmResource -ResourceGroupName MyResourceGroup -Name MyTestVM).Tags

Drugie polecenie wyświetla znaczniki dla danej zmiennej.

        PS C:\> $tags

        Name        Value
        ----                           -----
        Value       MyDepartment
        Name        Department
        Value       MyApp1
        Name        Application
        Value       MyName
        Name        Created By
        Value       Production
        Name        Environment

Trzecie polecenie dodaje tag dodatkowe zmiennej *$tags* . Zwróć uwagę na zastosowanie **+=** Aby dołączyć nową parę klucz wartość do listy *$tags* .

        PS C:\> $tags += @{Name="Location";Value="MyLocation"}

Czwarte polecenie ustawia wszystkie znaczniki zdefiniowane w zmiennej *$tags* danego zasobu. W tym przypadku jest MyTestVM.

        PS C:\> Set-AzureRmResource -ResourceGroupName MyResourceGroup -Name MyTestVM -ResourceType "Microsoft.Compute/VirtualMachines" -Tag $tags

Polecenie piątym zostaną wyświetlone wszystkie znaczniki zasobu. Jak widać, *lokalizacji* jest teraz określane jako tag z *MyLocation* jako wartość.

        PS C:\> (Get-AzureRmResource -ResourceGroupName MyResourceGroup -Name MyTestVM).Tags

        Name        Value
        ----                           -----
        Value       MyDepartment
        Name        Department
        Value       MyApp1
        Name        Application
        Value       MyName
        Name        Created By
        Value       Production
        Name        Environment
        Value       MyLocation
        Name        Location

Aby dowiedzieć się więcej na temat znakowania przy użyciu programu PowerShell, zapoznaj się z [Poleceń cmdlet zasobu Azure][].

[AZURE.INCLUDE [virtual-machines-common-tag-usage](../../includes/virtual-machines-common-tag-usage.md)]

## <a name="next-steps"></a>Następne kroki

* Aby dowiedzieć się więcej o znakowaniu Azure zasobów, zobacz [Omówienie Menedżera zasobów Azure][] i [Za pomocą znaczników w celu organizowania zasobów Azure][].
* Aby zobaczyć, jak znaczniki są pomocne przy zarządzaniu użytkowanie zasobów Azure, zobacz [Opis rachunku Azure][] i [uzyskać więcej informacji do usługi Microsoft Azure zużycie zasobów][].

[Środowiska PowerShell przy użyciu Menedżera zasobów Azure]: ../powershell-azure-resource-manager.md
[Polecenia cmdlet Azure zasobów]: https://msdn.microsoft.com/library/azure/dn757692.aspx
[Omówienie Menedżera zasobów Azure]: ../azure-resource-manager/resource-group-overview.md
[Organizowanie zasobów Azure za pomocą znaczników]: ../resource-group-using-tags.md
[Opis rachunku Azure]: ../billing/billing-understand-your-bill.md
[Uzyskanie wniosków do usługi Microsoft Azure zużycie zasobów]: ../billing-usage-rate-card-overview.md
