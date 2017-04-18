<properties
    pageTitle="Uogólnianie instalacji systemu Windows wirtualnego dysku twardego | Microsoft Azure"
    description="Dowiedz się użyć narzędzia Sysprep uogólniać maszyn wirtualnych systemu Windows do użytku z modelu wdrożenia Menedżera zasobów."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="cynthn"/>
    
    
    
    
# <a name="generalize-a-windows-virtual-machine-using-sysprep"></a>Generalize maszyny wirtualnej systemu Windows za pomocą programu Sysprep

W tej sekcji pokazano, jak uogólniać komputera wirtualnych do użycia jako obraz. Narzędzie Sysprep usuwa wszystkie informacje osobiste konto, między innymi i przygotowuje komputer może być używany jako obraz. Aby uzyskać szczegółowe informacje na temat narzędzia Sysprep, zobacz [sposobu użycia Sysprep: wprowadzenie](http://technet.microsoft.com/library/bb457073.aspx).

Upewnij się, że role serwerów uruchomione na komputerze są obsługiwane przez to narzędzie. Aby uzyskać więcej informacji zobacz [Obsługa narzędzia Sysprep dla ról serwera](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)

>[AZURE.IMPORTANT] Jeśli używasz Sysprep przed przekazaniem do wirtualnego dysku twardego Azure po raz pierwszy, upewnij się, że masz [przygotować się do maszyn wirtualnych](virtual-machines-windows-prepare-for-upload-vhd-image.md) przed uruchomieniem narzędzia Sysprep. 

1. Zaloguj się maszyn wirtualnych systemu Windows.

2. Otwórz okno wiersza polecenia jako administrator. Zmienianie katalogu **%windir%\system32\sysprep**, a następnie uruchom `sysprep.exe`.

3. W oknie dialogowym **Narzędzie przygotowywania systemu** wybierz **Wprowadź System z gotowych do obsługi (pierwszego uruchomienia)**, a następnie upewnij się, że jest zaznaczone pole wyboru **Generalize** .

4. W oknie dialogowym **Opcje zamykania**wybierz pozycję **Zamknij**.

5. Kliknij **przycisk OK**.

    ![Uruchom narzędzie Sysprep](./media/virtual-machines-windows-upload-image/sysprepgeneral.png)

6. Po zakończeniu działania narzędzia Sysprep zamyka maszyny wirtualnej. 

## <a name="next-steps"></a>Następne kroki

- Jeśli maszyn wirtualnych jest lokalnego, możesz teraz [przekazać wirtualny dysk twardy Azure](virtual-machines-windows-upload-image.md).
- Jeśli maszyn wirtualnych jest już Azure, możesz go teraz [utworzyć obraz z uogólniony maszyn wirtualnych](virtual-machines-windows-capture-image.md).