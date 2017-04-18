<properties
   pageTitle="Tworzenie i Przekaż obraz maszyn wirtualnych FreeBSD | Microsoft Azure"
   description="Dowiedz się, jak tworzyć i przekazywać wirtualnego dysku twardego (VHD) z systemem operacyjnym FreeBSD, aby utworzyć Azure maszyn wirtualnych"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="KylieLiang"
   manager="timlt"
   editor=""
   tags="azure-service-management"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="08/29/2016"
   ms.author="kyliel"/>

# <a name="create-and-upload-a-freebsd-vhd-to-azure"></a>Tworzenie i przekazywanie wirtualnego dysku twardego FreeBSD Azure

W tym artykule pokazano, jak tworzyć i przekazywać wirtualnego dysku twardego (VHD) z systemem operacyjnym FreeBSD. Po wysłaniu, można go jako własnego obrazu tworzenia maszyny wirtualnej (maszyn wirtualnych) platformy Azure.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


## <a name="prerequisites"></a>Wymagania wstępne
W tym artykule założono, że następujące elementy:

- **Subskrypcja Azure**— Jeśli nie masz konta, możesz utworzyć jeden na kilka minut. Jeśli masz subskrypcję MSDN, zobacz [kredytowej Azure miesięczne dla subskrybentów programu Visual Studio.](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/). W przeciwnym razie Dowiedz się, jak [utworzyć bezpłatne konto wersji próbnej](https://azure.microsoft.com/pricing/free-trial/).  

- **Narzędzia azure programu PowerShell**— PowerShell Azure moduł musi być zainstalowany i skonfigurowany do korzystania z subskrypcji usługi Azure. Aby pobrać moduł, zobacz [Pobieranie Azure](https://azure.microsoft.com/downloads/). Samouczek opisujący jak zainstalować i skonfigurować moduł jest dostępna w tym miejscu. Aby przekazać wirtualny dysk twardy, należy użyć polecenia cmdlet [Azure do pobrania](https://azure.microsoft.com/downloads/) .

- Musi być zainstalowany **system operacyjny FreeBSD zainstalowanych w pliku VHD**— obsługiwane FreeBSD system operacyjny wirtualnego dysku twardego. Istnieje wiele narzędzi do tworzenia plików VHD. Na przykład umożliwia rozwiązanie wirtualizacji, takie jak funkcji Hyper-V Utwórz plik VHD i zainstaluj system operacyjny. Aby uzyskać instrukcje dotyczące instalowania i używania funkcji Hyper-V, zobacz [instalacji funkcji Hyper-V i tworzenia maszyny wirtualnej](http://technet.microsoft.com/library/hh846766.aspx).

> [AZURE.NOTE] Nowszy format VHDX nie jest obsługiwana w Azure. Za pomocą Menedżera funkcji Hyper-V lub polecenia cmdlet [wirtualnego dysku twardego Konwertuj](https://technet.microsoft.com/library/hh848454.aspx)można konwertować na dysk Format wirtualnego dysku twardego. Ponadto jest [Samouczek na temat używania FreeBSD przy użyciu funkcji Hyper-V w witrynie MSDN](http://blogs.msdn.com/b/kylie/archive/2014/12/25/running-freebsd-on-hyper-v.aspx).

To zadanie zawiera następujące pięć kroków.

## <a name="step-1-prepare-the-image-for-upload"></a>Krok 1: Przygotowanie obrazu do przekazania

Na komputerze wirtualnych miejsce, w którym został zainstalowany system operacyjny FreeBSD należy wykonać następujące procedury:

1. Włącz DHCP.

        # echo 'ifconfig_hn0="SYNCDHCP"' >> /etc/rc.conf
        # service netif restart

2. Włącz SSH.

    SSH jest domyślnie włączona po zakończeniu instalacji z dysku. Jeśli nie został włączony jakiegoś powodu lub używać bezpośrednio FreeBSD wirtualny dysk twardy, wpisz następujące polecenie:

        # echo 'sshd_enable="YES"' >> /etc/rc.conf
        # ssh-keygen -t dsa -f /etc/ssh/ssh_host_dsa_key
        # ssh-keygen -t rsa -f /etc/ssh/ssh_host_rsa_key
        # service sshd restart

3. Konfigurowanie szeregowego konsoli.

        # echo 'console="comconsole vidconsole"' >> /boot/loader.conf
        # echo 'comconsole_speed="115200"' >> /boot/loader.conf

4. Zainstaluj sudo.

    Konta administratora jest wyłączona w Azure. Oznacza to, że należy korzystać sudo nie posiadająca uprawnień użytkownika, aby uruchomić polecenia z podwyższonym poziomem uprawnień.

        # pkg install sudo
;
5. Wymagania wstępne dotyczące agenta Azure.

        # pkg install python27  
        # pkg install Py27-setuptools27   
        # ln -s /usr/local/bin/python2.7 /usr/bin/python   
        # pkg install git

6. Instalowanie agenta Azure.

    Najnowszej wersji agenta Azure zawsze znajduje się na [github](https://github.com/Azure/WALinuxAgent/releases). Wersja 2.0.10 + oficjalnie obsługuje FreeBSD 10 & 10.1 i wersji 2.1.4 oficjalnie obsługuje FreeBSD 10.2 i późniejszych wersjach.

        # git clone https://github.com/Azure/WALinuxAgent.git  
        # cd WALinuxAgent  
        # git tag  
        …
        WALinuxAgent-2.0.16
        …
        v2.1.4
        v2.1.4.rc0
        v2.1.4.rc1

    2.0 Przejdźmy 2.0.16 jako przykładu użyto:

        # git checkout WALinuxAgent-2.0.16
        # python setup.py install  
        # ln -sf /usr/local/sbin/waagent /usr/sbin/waagent  

    Aby uzyskać 2.1 Przejdźmy 2.1.4 jako przykładu użyto:

        # git checkout v2.1.4
        # python setup.py install  
        # ln -sf /usr/local/sbin/waagent /usr/sbin/waagent  
        # ln -sf /usr/local/sbin/waagent2.0 /usr/sbin/waagent2.0

    >[AZURE.IMPORTANT] Po zainstalowaniu agenta Azure jest dobrym pomysłem, aby sprawdzić, czy jest uruchomiony:

        # waagent -version
        WALinuxAgent-2.1.4 running on freebsd 10.3
        Python: 2.7.11
        # service –e | grep waagent
        /etc/rc.d/waagent
        # cat /var/log/waagent.log

7. Deprovision systemu.

    Deprovision systemu go oczyścić i wprowadź odpowiednie do ponownego inicjowania obsługi administracyjnej. Następujące polecenie powoduje także usunięcie starego konta użytkownika ustanawianie i skojarzonych z nim danych:

        # echo "y" |  /usr/local/sbin/waagent -deprovision+user  
        # echo  'waagent_enable="YES"' >> /etc/rc.conf

    Teraz można zamknąć usługi maszyn wirtualnych.

## <a name="step-2-create-a-storage-account-in-azure"></a>Krok 2: Tworzenie konta magazynu platformy Azure ##

Wymagane jest konto miejsca do magazynowania w Azure, aby przekazać plik VHD, więc może służyć do tworzenia maszyny wirtualnej. Za pomocą portalu klasyczny Azure do utworzenia konta miejsca do magazynowania.

1. Zaloguj się do [portalu klasyczny Azure](https://manage.windowsazure.com).

2. Na pasku poleceń wybierz pozycję **Nowy**.

3. Wybieranie **usług danych** > **miejsca do magazynowania** > **Szybkie tworzenie**.

    ![Szybkie tworzenie konta miejsca do magazynowania](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/Storage-quick-create.png)

4. Wypełnij pola w następujący sposób:

    - W polu **adres URL** wpisz nazwę poddomeny używać w adresie URL konta miejsca do magazynowania. Wpis może zawierać od liczby 3-24 i małe litery. Ta nazwa staje się nazwa hosta w adres URL, który jest używany do adresowania magazyn obiektów Blob platformy Azure, magazyn kolejek Azure lub tabeli Azure zasobów miejsca do magazynowania dla subskrypcji.

    - W menu rozwijanym **Lokalizacji/koligacji grupy** wybierz **lokalizację w grupie koligacji** dla konta miejsca do magazynowania. Grupa koligacji umożliwia umieszczenie z usługami w chmurze i miejsca do magazynowania w tym samym centrum danych.

    - W polu **replikacji** zdecydować, czy replikacji **Geo zbędne** konta miejsca do magazynowania. Replikacja Geo jest domyślnie włączona. Ta opcja replikuje danych do lokalizacji pomocniczej, bezpłatnie, tak aby magazynie przełącza do tej lokalizacji przypadku głównych awarii w lokalizacji podstawowej. Pomocnicza lokalizacja jest przypisywana automatycznie i nie można zmienić. Jeśli potrzebujesz mieć większą kontrolę nad lokalizacji usługi w chmurze ze względu na przepisy prawne lub zasad organizacji, możesz wyłączyć geo replikacji. Należy jednak pamiętać, że jeśli później włączyć replikacji geo zostanie naliczona opłatę transfer danych jednorazowy replikacji istniejących danych do lokalizacji pomocniczej. Usługi Magazyn bez replikacji geo oferowanych w rabat. Więcej informacji o zarządzaniu geo replikacji konta przestrzeni dyskowej można znaleźć tutaj: [Tworzenie, zarządzanie, lub usunąć konto miejsca do magazynowania](../storage-create-storage-account/#replication-options).

    ![Wprowadź szczegółowe informacje o koncie miejsca do magazynowania](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/Storage-create-account.png)


5. Wybierz opcję **Utwórz konto miejsca do magazynowania**. Konto zostanie wyświetlony w obszarze **miejsca do magazynowania**.

    ![Pomyślnie utworzono konto miejsca do magazynowania](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/Storagenewaccount.png)

6. Następnie należy utworzyć kontener pliki przekazane VHD. Zaznacz nazwę konta magazynu, a następnie wybierz pozycję **kontenerów**.

    ![Szczegóły konta miejsca do magazynowania](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/storageaccount_detail.png)

7. Wybierz polecenie **Utwórz kontenera**.

    ![Szczegóły konta miejsca do magazynowania](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/storageaccount_container.png)

8. W polu **Nazwa** wpisz nazwę dla swojego kontenera. Następnie w menu **dostęp do** listy rozwijanej wybierz jakiego rodzaju zasadę dostępu.

    ![Nazwa kontenera](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/storageaccount_containervalues.png)

    > [AZURE.NOTE] Domyślnie kontener jest prywatna i jest możliwy tylko przez właściciela konta. Aby umożliwić publicznej dostęp do odczytu do obiektów blob w kontenerze, a nie do kontenera właściwości i metadane, za pomocą opcji **Publicznej obiektów Blob** . Aby umożliwić publicznej dostęp do odczytu dla kontenerów i obiektów blob, za pomocą opcji **Publicznej kontener** .

## <a name="step-3-prepare-the-connection-to-azure"></a>Krok 3: Przygotowywanie połączenie Azure

Aby możesz przekazać plik VHD, musisz ustanawianie bezpiecznego połączenia między komputerem a Azure subskrypcji. W tym celu można użyć metody usługi Azure Active Directory (Azure AD) lub certyfikatu.

### <a name="use-the-azure-ad-method-to-upload-a-vhd-file"></a>Użyj metody Azure AD, aby przekazać plik VHD

1. Otwórz konsolę Azure programu PowerShell.

2. Wpisz następujące polecenie:  
    `Add-AzureAccount`

    To polecenie otwiera okno logowania miejsce, w którym możesz zalogować się przy użyciu konta służbowego.

    ![Okno programu PowerShell](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/add_azureaccount.png)

3. Azure uwierzytelnianie i zapisuje informacje poświadczeń. Następnie zamyka okno.

### <a name="use-the-certificate-method-to-upload-a-vhd-file"></a>Użyj metody certyfikatu, aby przekazać plik VHD

1. Otwórz konsolę Azure programu PowerShell.

2. Typ:  `Get-AzurePublishSettingsFile`.

3. Zostanie otwarty i zostanie wyświetlony monit o pobranie pliku .publishsettings okna przeglądarki. Ten plik zawiera informacje i certyfikat dla subskrypcji Azure.

    ![Stronę pobierania przeglądarki](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/Browser_download_GetPublishSettingsFile.png)

3. Zapisz plik .publishsettings.

4. Typ:  `Import-AzurePublishSettingsFile <PathToFile>`, gdzie `<PathToFile>` jest pełną ścieżką do pliku .publishsettings.

   Aby uzyskać więcej informacji zobacz [Wprowadzenie do poleceń cmdlet Azure](http://msdn.microsoft.com/library/windowsazure/jj554332.aspx).

   Aby uzyskać więcej informacji na temat instalowania i konfigurowania środowiska PowerShell zobacz [jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md).

## <a name="step-4-upload-the-vhd-file"></a>Krok 4: Przekazać plik VHD

Podczas przekazywania pliku VHD można umieścić go dowolne miejsce w magazynie obiektów Blob. Poniżej przedstawiono niektóre terminy, których będziesz używać podczas przekazywania pliku:
-  **BlobStorageURL** jest adresem URL dla konta miejsca do magazynowania, utworzony w kroku 2.
-  **YourImagesFolder** jest kontenerze magazyn obiektów Blob miejsce, w którym mają być przechowywane obrazy.
- **VHDName** jest etykietę, która pojawia się w portalu klasyczny Azure do identyfikowania wirtualnego dysku twardego.
- **PathToVHDFile** jest pełną ścieżkę i nazwę pliku VHD.


W oknie programu PowerShell Azure użytym w poprzednim kroku należy wpisać:

        Add-AzureVhd -Destination "<BlobStorageURL>/<YourImagesFolder>/<VHDName>.vhd" -LocalFilePath <PathToVHDFile>

## <a name="step-5-create-a-vm-with-the-uploaded-vhd-file"></a>Krok 5: Tworzenie maszyn wirtualnych z pliku VHD przekazane
Po przekazaniu pliku VHD można można dodawać jako obraz do listy niestandardowe obrazy, które są skojarzone z subskrypcji i tworzenia maszyny wirtualnej z ten obraz niestandardowy.

1. W oknie programu PowerShell Azure użytym w poprzednim kroku należy wpisać:

        Add-AzureVMImage -ImageName <Your Image's Name> -MediaLocation <location of the VHD> -OS <Type of the OS on the VHD>

    > [AZURE.NOTE]Używanie Linux jako typ systemu operacyjnego. Bieżąca wersja programu PowerShell Azure akceptuje tylko "Linux" lub "Windows" jako parametru.

2. Po wykonaniu powyższych czynności nowy obraz znajduje się po wybraniu kartę **obrazy** w portalu klasyczny Azure.  

    ![Wybierz obraz](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/addfreebsdimage.png)

3. Tworzenie maszyny wirtualnej z galerii. Ten nowy obraz jest teraz dostępny w obszarze **Moje obrazy**.
4. Wybierz nowy obraz. Następnie przejdź do z instrukcjami, aby skonfigurować nazwa hosta, hasła, klucz SSH i tak dalej.

    ![Obraz niestandardowy](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/createfreebsdimageinazure.png)

4. Po ukończeniu inicjowania obsługi administracyjnej, pojawi się z maszyn wirtualnych FreeBSD uruchomione w Azure.

    ![Obraz FreeBSD platformy azure](./media/virtual-machines-linux-classic-freebsd-create-upload-vhd/freebsdimageinazure.png)
