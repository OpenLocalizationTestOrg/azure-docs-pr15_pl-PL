<properties
    pageTitle="Odłączanie dyskiem danych z maszyn wirtualnych systemu Windows | Microsoft Azure"
    description="Dowiedz się odłączyć dysk danych z maszyny wirtualnej platformy Azure przy użyciu modelu wdrożenia Menedżera zasobów."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="cynthn"/>



# <a name="how-to-detach-a-data-disk-from-a-windows-virtual-machine"></a>Jak odłączanie dysku danych na komputerze wirtualnych systemu Windows


Jeśli nie potrzebujesz już dysku danych podłączonego do maszyny wirtualnej, można łatwo można odłączyć go. Usuwa dysk z maszyny wirtualnej, ale nie powoduje jego usunięcia z magazynu. 

> [AZURE.WARNING] Jeśli odłączanie dysku, które nie są automatycznie usuwane. Jeśli subskrybujesz Premium miejsca do magazynowania, będzie nadal ponoszenia opłat miejsca do magazynowania dla dysku. Aby uzyskać więcej informacji można znaleźć [ceny](../storage/storage-premium-storage.md#pricing-and-billing)i rozliczeń podczas korzystania z miejsca do magazynowania Premium. 

Jeśli chcesz ponownie użyć istniejących danych na dysku, możesz dołączyć go do tej samej maszyny wirtualnej, albo inną.  


## <a name="detach-a-data-disk-using-the-portal"></a>Odłączanie dyskiem danych za pomocą portalu

1. W portalu Centrum wybierz **maszyn wirtualnych**.

2. Wybierz pozycję maszyny wirtualnej, która ma dysk danych, który chcesz odłączyć, a następnie kliknij pozycję **wszystkie ustawienia**.

3. W karta **Ustawienia** wybierz **dysków**.

4. W karta **dyski** wybierz dysk danych, który chcesz odłączyć.

5. W karta dysku danych kliknij przycisk **Odłącz**.


    ![Zrzut ekranu przedstawiający przycisk Odłącz.](./media/virtual-machines-windows-detach-disk/detach-disk.png)

Dysk pozostanie w magazynie, ale nie jest już dołączony do maszyny wirtualnej.


## <a name="detach-a-data-disk-using-powershell"></a>Odłączanie dysku danych przy użyciu programu PowerShell

W tym przykładzie pierwsze polecenie otrzymuje maszyny wirtualnej o nazwie **MyVM07** w grupie zasobów **RG11** przy użyciu polecenia cmdlet Get-AzureRmVM. Polecenie zapisuje maszyny wirtualnej w zmiennej **$VirtualMachine** . 

Drugie polecenie usuwa dysku danych o nazwie DataDisk3 z maszyny wirtualnej. 

Ostatnim poleceniu aktualizacji stanu maszyny wirtualnej, aby ukończyć proces usuwania dysku danych.

    $VirtualMachine = Get-AzureRmVM -ResourceGroupName "RG11" -Name "MyVM07" 
    Remove-AzureRmVMDataDisk -VM $VirtualMachine -Name "DataDisk3"
    Update-AzureRmVM -ResourceGroupName "RG11" -Name "MyVM07" -VM $VirtualMachine


Aby uzyskać więcej informacji zobacz [Usuwanie AzureRmVMDataDisk](https://msdn.microsoft.com/library/mt603614.aspx)

## <a name="next-steps"></a>Następne kroki

Jeśli chcesz ponownie użyć dysku danych, możesz po prostu [dołączyć go do innego maszyn wirtualnych](virtual-machines-windows-attach-disk-portal.md)
