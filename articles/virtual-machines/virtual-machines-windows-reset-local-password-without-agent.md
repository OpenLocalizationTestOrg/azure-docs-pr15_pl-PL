<properties
   pageTitle="Resetowanie hasła lokalnego systemu Windows, gdy Azure gościa agent nie jest zainstalowany | Microsoft Azure"
   description="Jak zresetować hasło lokalnego konta użytkownika systemu Windows, gdy agent Azure gościa nie jest zainstalowana lub działa na maszyny"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="10/05/2016"
   ms.author="iainfou"/>

# <a name="how-to-reset-local-windows-password-for-azure-vm"></a>Jak zresetować hasło lokalnego systemu Windows Azure maszyn wirtualnych
Możesz zresetować lokalne hasło systemu Windows maszyn wirtualnych przy użyciu Azure [Azure portal lub Azure programu PowerShell](virtual-machines-windows-reset-rdp.md) pod warunkiem, że zainstalowano agenta Azure gościa. Ta metoda jest podstawowym sposobem Resetowanie hasła dla maszyn wirtualnych Azure. Jeśli występują problemy z agentem Azure gościa nie odpowiada lub nie można zainstalować po przekazaniu obraz niestandardowy, możesz ręcznie Resetowanie hasła w systemie Windows. W tym artykule opisano Resetowanie hasła konta lokalnego dołączając dysku wirtualnego źródłowego systemu operacyjnego do innego maszyn wirtualnych. 

> [AZURE.WARNING] Ten proces można używać tylko ostatecznym. Zawsze spróbuj zresetować hasło, używając najpierw [Azure portal lub Azure programu PowerShell](virtual-machines-windows-reset-rdp.md) .


## <a name="overview-of-the-process"></a>Omówienie procesu
Kroków core lokalne resetowania hasła dla maszyn wirtualnych systemu Windows w Azure po dostępowi do agenta Azure gościa nie jest w następujący sposób:

- Usuwanie źródła maszyn wirtualnych. Dyski wirtualne są zachowywane.
- Dołączanie źródła maszyn wirtualnych systemu operacyjnego dysku do innego maszyn wirtualnych w ramach subskrypcji usługi Azure. Ten maszyn wirtualnych nosi nazwę rozwiązywania problemów maszyn wirtualnych.
- Przy użyciu rozwiązywania problemów maszyn wirtualnych można tworzyć pliki konfiguracji na dysku z systemem operacyjnym maszyn wirtualnych źródła.
- Odłączanie maszyn wirtualnych systemu operacyjnego dysk z rozwiązywania problemów maszyn wirtualnych.
- Szablon Menedżera zasobów umożliwia tworzenie Głosowa, przy użyciu oryginalnego dysku wirtualnego.
- Podczas uruchamiania nowych maszyn wirtualnych tworzonych plików konfiguracji zaktualizuj hasło użytkownika wymagane.


## <a name="detailed-steps"></a>Szczegółowe kroki
Zawsze spróbuj zresetować hasło za pomocą [Azure portal lub Azure programu PowerShell](virtual-machines-windows-reset-rdp.md) przed próbą następujące czynności. Upewnij się, że masz kopię zapasową usługi maszyn wirtualnych przed rozpoczęciem pracy. 

1. Usuwanie dotyczy maszyn wirtualnych w Azure portal. Usuwanie maszyn wirtualnych usuwa tylko metadane, odwołanie maszyn wirtualnych w Azure. Dyski wirtualne są zachowywane po usunięciu maszyn wirtualnych:

    - Wybierz pozycję maszyn wirtualnych w portalu Azure, kliknij przycisk *Usuń*:

    ![Usuwanie istniejącej maszyn wirtualnych](./media/virtual-machines-windows-reset-local-password-without-guest-agent/delete_vm.png)

2. Dołączanie źródła maszyn wirtualnych systemu operacyjnego dysku do rozwiązywania problemów maszyn wirtualnych. Rozwiązywania problemów maszyn wirtualnych muszą znajdować się w tym samym regionie jako źródło maszyn wirtualnych systemu operacyjnego dysku (takie jak `West US`):

    - Wybierz pozycję rozwiązywania problemów maszyn wirtualnych w portalu Azure. Kliknij polecenie *dyski* | *istniejące Dołącz*:

    ![Dołączanie istniejącego dysku](./media/virtual-machines-windows-reset-local-password-without-guest-agent/disks_attach_existing.png)

    Wybierz *Plik wirtualny dysk twardy* , a następnie wybierz konto miejsca do magazynowania, które zawiera źródło maszyn wirtualnych:

    ![Wybierz konto miejsca do magazynowania](./media/virtual-machines-windows-reset-local-password-without-guest-agent/disks_select_storageaccount.PNG)

    Wybierz kontener źródło. Kontener źródła jest zazwyczaj *wirtualnych dysków twardych*:

    ![Wybierz kontener miejsca do magazynowania](./media/virtual-machines-windows-reset-local-password-without-guest-agent/disks_select_container.png)

    Wybierz pozycję wirtualny dysk twardy OS do dołączenia. Kliknij przycisk *Wybierz* , aby ukończyć proces:

    ![Wybierz źródło dysk wirtualny](./media/virtual-machines-windows-reset-local-password-without-guest-agent/disks_select_source_vhd.png)

3. Nawiązywanie połączenia z rozwiązywania problemów maszyn wirtualnych za pomocą pulpitu zdalnego i upewnij się, że źródło maszyn wirtualnych systemu operacyjnego dysk jest widoczny:

    - Zaznacz rozwiązywania problemów maszyn wirtualnych w portalu Azure, a następnie kliknij przycisk *Połącz*.
    - Otwórz plik RDP do pobrania. Wprowadź nazwę użytkownika i hasło dotyczące rozwiązywania problemów maszyn wirtualnych.
    - W Eksploratorze plików Znajdź dysk danych, który dołączysz. Jeśli źródłem maszyn wirtualnych wirtualny dysk twardy jest dysku tylko dane dołączone do rozwiązywania problemów maszyn wirtualnych, należy dysk F::

    ![Wyświetlanie dysku załączonych danych](./media/virtual-machines-windows-reset-local-password-without-guest-agent/troubleshooting_vm_fileexplorer.png)

4. Tworzenie `gpt.ini` w `\Windows\System32\GroupPolicy` na dysku maszyn wirtualnych źródło (jeśli istnieje gpt.ini, Zmień nazwę na gpt.ini.bak):

    > [AZURE.WARNING] Upewnij się, nie przypadkowo utworzonych następujące pliki w C:\Windows, dysk systemu operacyjnego dla rozwiązywania problemów maszyn wirtualnych. Tworzenie następujące pliki do stacji dysków OS źródła maszyn wirtualnych dołączony jako dysk danych.

    - Dodaj następujące wiersze do `gpt.ini` utworzonego wcześniej pliku:

    ```
    [General]
    gPCFunctionalityVersion=2
    gPCMachineExtensionNames=[{42B5FAAE-6536-11D2-AE5A-0000F87571E3}{40B6664F-4972-11D1-A7CA-0000F87571E3}]
    Version=1
    ```

    ![Tworzenie gpt.ini](./media/virtual-machines-windows-reset-local-password-without-guest-agent/create_gpt_ini.png)
 
5. Tworzenie `scripts.ini` w `\Windows\System32\GroupPolicy\Machine\Scripts`. Upewnij się, że są wyświetlane foldery ukryte. W razie potrzeby utwórz `Machine` lub `Scripts` folderów.

    - Dodaj następujące wiersze `scripts.ini` utworzonego wcześniej pliku:

    ```
    [Startup]
    0CmdLine=C:\Windows\System32\FixAzureVM.cmd
    0Parameters=
    ```

    ![Tworzenie scripts.ini](./media/virtual-machines-windows-reset-local-password-without-guest-agent/create_scripts_ini.png)
 
6. Tworzenie `FixAzureVM.cmd` w `\Windows\System32` z następujące zagadnienia, zamieniając `<username>` i `<newpassword>` z własne wartości:

    ```
    NET USER <username> <newpassword>
    ```

    ![Tworzenie FixAzureVM.cmd](./media/virtual-machines-windows-reset-local-password-without-guest-agent/create_fixazure_cmd.png)

    Musi spełniać wymagania złożoność haseł skonfigurowane dla swojego maszyn wirtualnych, podczas definiowania nowego hasła.

7. W portalu Azure Odłącz dysk z rozwiązywania problemów maszyn wirtualnych:

    - Wybierz rozwiązywania problemów maszyn wirtualnych w portalu Azure kliknij pozycję *dysków*.
    - Wybierz dysk danych dołączone w kroku 2, kliknij przycisk *Odłącz*:

    ![Odłączanie dysku](./media/virtual-machines-windows-reset-local-password-without-guest-agent/detach_disk.png)

8. Przed utworzeniem maszyny uzyskać identyfikator URI na dysk z systemem operacyjnym źródła:

    - Wybierz konto, miejsca do magazynowania w portalu Azure, kliknij przycisk *obiektów blob*.
    - Wybierz kontener. Kontener źródła jest zazwyczaj *wirtualnych dysków twardych*:

    ![Zaznaczanie obiektów blob konta miejsca do magazynowania](./media/virtual-machines-windows-reset-local-password-without-guest-agent/select_storage_details.png)

    Wybierz źródło wirtualnego dysku twardego maszyn wirtualnych systemu operacyjnego, a następnie kliknij przycisk *Kopiuj* obok Nazwa *adresu URL* :

    ![Skopiuj identyfikator URI](./media/virtual-machines-windows-reset-local-password-without-guest-agent/copy_source_vhd_uri.png)

9. Tworzenie maszyn wirtualnych z źródła maszyn wirtualnych systemu operacyjnego dysku:

    - [Ten szablon Menedżera zasobów Azure](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd) umożliwia utworzenie maszyn wirtualnych z specjalistyczne wirtualnego dysku twardego. Kliknij pozycję `Deploy to Azure` przycisk, aby otworzyć Azure portal przy użyciu szablonu szczegółów wypełnione.
    - Jeśli chcesz zachować wszystkie poprzednie ustawienia maszyn wirtualnych, wybierz pozycję *Edytowanie szablonu* , aby podać istniejących VNet, podsieci, karty sieciowej lub publiczny adres IP.
    - W `OSDISKVHDURI` parametru pola tekstowego, Wklej uzyskać identyfikator URI źródła wirtualnego dysku twardego w poprzednim kroku:

    ![Tworzenie maszyn wirtualnych na podstawie szablonu](./media/virtual-machines-windows-reset-local-password-without-guest-agent/create_new_vm_from_template.png)

10. Po uruchomieniu nowych maszyn wirtualnych nawiązać maszyn wirtualnych, pulpitu zdalnego przy użyciu nowego hasła, określonego w `FixAzureVM.cmd` skrypt.

11. Z usługi zdalnej sesji do nowych maszyn wirtualnych Usuń następujące pliki do oczyszczenia środowiska:

    - Z %windir%\System32
        - Usuwanie FixAzureVM.cmd
    - Z %windir%\System32\GroupPolicy\Machine\
        - Usuwanie scripts.ini
    - Z %windir%\System32\GroupPolicy
        - Usuwanie gpt.ini (jeśli istnieje gpt.ini przed i zostanie zmieniona na gpt.ini.bak, Zmień nazwę pliku bak z powrotem do gpt.ini)

## <a name="next-steps"></a>Następne kroki
Jeśli nadal nie możesz połączyć za pomocą pulpitu zdalnego, zobacz [Przewodnik rozwiązywania problemów RDP](virtual-machines-windows-troubleshoot-rdp-connection.md). [Szczegółowy przewodnik rozwiązywania problemów RDP](virtual-machines-windows-detailed-troubleshoot-rdp.md) analizuje Rozwiązywanie problemów z metod zamiast kolejne etapy. Możesz również [otworzyć żądanie obsługi Azure](https://azure.microsoft.com/support/options/) praktycznych pomocy.