<properties
    pageTitle="Przechwycony obraz maszyn wirtualnych systemu Windows Azure | Microsoft Azure"
    description="Przechwycony obraz maszyn wirtualnych systemu Windows Azure utworzone za pomocą modelu Klasyczny wdrożenia."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="cynthn"/>

#<a name="capture-an-image-of-an-azure-windows-virtual-machine-created-with-the-classic-deployment-model"></a>Przechwycony obraz maszyn wirtualnych systemu Windows Azure utworzone za pomocą modelu Klasyczny wdrożenia.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Menedżer zasobów modelu informacji można uzyskać [działa Utwórz kopię maszyn wirtualnych systemu Windows Azure](virtual-machines-windows-vhd-copy.md).


W tym artykule pokazano, jak przechwycić Azure maszyn wirtualnych z systemem Windows, aby móc używać jako obraz do tworzenia innych maszyn wirtualnych. Ten obraz zawiera dysku systemu operacyjnego i wszelkie dyski danych, które zostały dołączone do maszyny wirtualnej. Nie zawiera konfiguracje sieciowe, więc musisz skonfigurować te podczas tworzenia pozostałych maszyn wirtualnych korzystające z obrazu.

Azure przechowuje obraz w obszarze **Moje obrazy**. Jest to miejsce, w którym są przechowywane wszystkie obrazy, które zostały przekazane. Aby uzyskać szczegółowe informacje o obrazach zobacz [informacje o obrazami dla maszyn wirtualnych](virtual-machines-linux-classic-about-images.md).

##<a name="before-you-begin"></a>Przed rozpoczęciem##

W tej procedurze założono, że już utworzone Azure maszyn wirtualnych i skonfigurować system operacyjny, łącznie z dołączanie wszelkie dyski danych. Jeśli nie zrobiono tego jeszcze, zobacz te instrukcje:

- [Tworzenie maszyny wirtualnej z obrazu](virtual-machines-windows-classic-createportal.md)
- [Sposoby dołączania danych dyskiem maszyn wirtualnych](virtual-machines-windows-classic-attach-disk.md)
- Upewnij się, że role serwerów są obsługiwane za pomocą narzędzia Sysprep. Aby uzyskać więcej informacji zobacz [Obsługa narzędzia Sysprep dla ról serwera](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles).

> [AZURE.WARNING] Ten proces usuwa oryginalną maszyny wirtualnej po jest rejestrowany. 

Przed caputuring obraz Azure maszyn wirtualnych zaleca się kopię zapasową maszyny wirtualnej docelowej. Azure maszyn wirtualnych zapasową można wykonywać przy użyciu kopii zapasowej Azure. Aby uzyskać szczegółowe informacje zobacz [Tworzenie kopii zapasowej Azure maszyn wirtualnych](../backup/backup-azure-vms.md). Inne rozwiązania są dostępne certyfikowanych partnerów. Aby dowiedzieć się, co to jest obecnie dostępny, wyszukaj Azure Marketplace.


##<a name="capture-the-virtual-machine"></a>Przechwytywanie maszyny wirtualnej

1. W [portal Azure klasyczny](http://manage.windowsazure.com) **Połącz** maszyn wirtualnych. Aby uzyskać instrukcje zobacz [jak logować się do maszyny wirtualnej z systemem Windows Server] [].

2.  Otwórz okno wiersza polecenia jako administrator.

3.  Zmienianie katalogu `%windir%\system32\sysprep`, a następnie uruchom sysprep.exe.

4.  Zostanie wyświetlone okno dialogowe **Narzędzie przygotowywania systemu** . Wykonaj następujące czynności:

    - W oknie dialogowym **Akcja oczyszczania systemu**wybierz **Wprowadź System z gotowych do obsługi (pierwszego uruchomienia)** i upewnij się, że **Generalize** jest zaznaczone pole wyboru. Aby uzyskać więcej informacji na temat korzystania z narzędzia Sysprep, zobacz [sposobu użycia Sysprep: wprowadzenie][].

    - W oknie dialogowym **Opcje zamykania**wybierz pozycję **Zamknij**.

    - Kliknij **przycisk OK**.

    ![Narzędzia Sysprep.](./media/virtual-machines-windows-classic-capture-image/SysprepGeneral.png)

7.  Sysprep zamyka maszyny wirtualnej, co spowoduje zmianę stanu maszyny wirtualnej w portalu klasyczny Azure do **zatrzymania**.

8.  W portalu klasyczny Azure kliknij **maszyn wirtualnych** i wybierz maszyny wirtualnej, który chcesz przechwycić.

9.  Na pasku poleceń kliknij **Przechwytywanie**.

    ![Przechwytywanie maszyn wirtualnych](./media/virtual-machines-windows-classic-capture-image/CaptureVM.png)

    Zostanie wyświetlone okno dialogowe **Przechwytywanie maszyny wirtualnej** .

10. W polu **Nazwa obrazu**wpisz nazwę dla nowego obrazu.

11. Przed dodaniem obraz systemu Windows Server do zestawu niestandardowych obrazów musi być uogólniony, uruchamiając Sysprep zgodnie z instrukcjami w poprzednich krokach. Kliknij przycisk **uruchomię Sysprep na komputerze wirtualnych** oznaczającą, że które Tobie.

12. Kliknij znacznik wyboru, aby przechwycić obraz. Nowy obraz jest już dostępny w obszarze **obrazy**.

    ![Przechwytywanie obrazu pomyślnie](./media/virtual-machines-windows-classic-capture-image/VMCapturedImageAvailable.png)

##<a name="next-steps"></a>Następne kroki

Obraz jest gotowy do można używać do tworzenia maszyn wirtualnych. Aby to zrobić, utworzysz maszyny wirtualnej, za pomocą polecenia menu **Z galerii** i wybierając pozycję obraz, który został utworzony. Aby uzyskać instrukcje zobacz [Tworzenie maszyny wirtualnej z obrazu](virtual-machines-windows-classic-createportal.md).



[Jak zalogować się do maszyny wirtualnej z systemem Windows Server]: virtual-machines-windows-classic-connect-logon.md
[Jak używać Sysprep: wprowadzenie]: http://technet.microsoft.com/library/bb457073.aspx
[Run Sysprep.exe]: ./media/virtual-machines-capture-image-windows-server/SysprepCommand.png
[Enter Sysprep.exe options]: ./media/virtual-machines-windows-classic-capture-image/SysprepGeneral.png
[The virtual machine is stopped]: ./media/virtual-machines-capture-image-windows-server/SysprepStopped.png
[Capture an image of the virtual machine]: ./media/virtual-machines-windows-classic-capture-image/CaptureVM.png
[Enter the image name]: ./media/virtual-machines-capture-image-windows-server/Capture.png
[Image capture successful]: ./media/virtual-machines-capture-image-windows-server/CaptureSuccess.png
[Use the captured image]: ./media/virtual-machines-capture-image-windows-server/MyImagesWindows.png
