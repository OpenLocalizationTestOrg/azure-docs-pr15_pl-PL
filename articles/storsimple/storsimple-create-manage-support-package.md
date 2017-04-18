<properties
   pageTitle="Tworzenie pakietu pomocy technicznej StorSimple | Microsoft Azure"
   description="Dowiedz się, jak tworzyć, odszyfrowywanie i edytować pakiet pomocy technicznej dla swojego urządzenia StorSimple."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/17/2016"
   ms.author="alkohli" />


# <a name="create-and-manage-a-storsimple-support-package"></a>Tworzenie i zarządzanie nimi StorSimple pakiet pomocy technicznej

## <a name="overview"></a>Omówienie

Pakiet pomocy technicznej StorSimple jest mechanizm prostych w użyciu zbiera wszystkich odpowiednich dzienników ułatwiających Support firmy Microsoft dotyczącą rozwiązywania problemów StorSimple urządzenia. Dzienniki zbierane są szyfrowane i skompresowany.

Ten samouczek zawiera instrukcje krok po kroku, tworzyć i zarządzać nimi pakietu pomocy technicznej.

## <a name="create-and-upload-a-support-package-in-the-azure-classic-portal"></a>Tworzenie i przekazywanie pakietu pomocy technicznej w portalu klasyczny Azure

Można tworzyć i przekazywanie pakietu pomocy technicznej do witryny Microsoft Support za pomocą strony **konserwacji** usługi w portalu klasyczny Azure.

> [AZURE.NOTE] Przekazywanie wymaga klucza pomocy technicznej. Z pracownikiem pomocy technicznej powinien zawierać to dla Ciebie w wiadomości e-mail.

Pakiet pomocy technicznej zaszyfrowane i skompresowany (cab) jest tworzony i przekazać do witryny pomocy technicznej. Z pracownikiem pomocy technicznej mogą następnie pobrać ten pakiet z witryny pomocy technicznej dla rozwiązywania problemu.

W portalu klasycznego, aby utworzyć pakiet pomocy technicznej, należy wykonać następujące czynności.

#### <a name="to-create-a-support-package-in-the-azure-classic-portal"></a>Aby utworzyć pakiet pomocy technicznej w portalu klasyczny Azure

1. Wybierz pozycję **urządzenia do** > **Konserwacja**.

2. W sekcji **pomocy technicznej pakietu** wybierz **Tworzenie i przekazywanie pomocy technicznej**.

3. W oknie dialogowym **Tworzenie i przekazywanie pakietu pomocy technicznej** wykonaj następujące czynności:

    ![Tworzenie pakietu pomocy technicznej](./media/storsimple-create-manage-support-package/IC740923.png)

    - W polu tekstowym **Klucz pomocy technicznej** wprowadź klucz. Z pracownikiem pomocy technicznej firmy Microsoft, należy wysłać ten klucz do Ciebie w wiadomości e-mail.

    - Zaznacz pole wyboru, aby zapewnić zgody Aby automatycznie przekazywać pakietu pomocy technicznej do witryny Microsoft Support.

    - Kliknij ikonę wyboru ![Ikona modułu sprawdzania](./media/storsimple-create-manage-support-package/IC740895.png).


## <a name="manually-create-a-support-package"></a>Ręczne tworzenie pakietu pomocy technicznej

W niektórych przypadkach musisz ręcznie utworzyć pakiet pomocy technicznej przy użyciu programu Windows PowerShell dla StorSimple. Na przykład:

- Jeśli potrzebujesz usunąć informacji poufnych z plików dziennika przed udostępnianie Support firmy Microsoft.

- Jeśli masz problemy przekazywanie pakietu z powodu problemów z łącznością.

Możesz udostępnić pakietu ręcznie wygenerowane pomocy technicznej firmy Microsoft Support w wiadomości e-mail. Wykonaj poniższe kroki, aby utworzyć pakiet pomocy technicznej w programie Windows PowerShell dla StorSimple.

#### <a name="to-create-a-support-package-in-windows-powershell-for-storsimple"></a>Aby utworzyć pakiet pomocy technicznej w programie Windows PowerShell dla StorSimple

1. Aby rozpocząć sesję środowiska Windows PowerShell jako administrator na komputerze zdalnym, który jest używany do łączenia z urządzeniem StorSimple, wpisz następujące polecenie:

    `Start PowerShell`

2. Sesji środowiska Windows PowerShell nawiązanie połączenia z konsoli SSAdmin urządzenia:

    - W wierszu polecenia wpisz:

        `$MS = New-PSSession -ComputerName <IP address for DATA 0> -Credential SSAdmin -ConfigurationName "SSAdminConsole"`

    1. W wyświetlonym oknie dialogowym Wprowadź hasło administratora urządzenia. Hasło domyślne to:

        `Password1`

        ![Okno dialogowe poświadczeń programu PowerShell](./media/storsimple-create-manage-support-package/IC740962.png)

    2. Wybierz **przycisk OK**.
    1. W wierszu polecenia wpisz:

        `Enter-PSSession $MS`

3. W tej sesji zostanie otwarta wprowadź odpowiednie polecenie.

    - Sieć akcji, które są chronione hasłem wprowadź:

        `Export-HcsSupportPackage –PackageTag "MySupportPackage" –Credential "Username" -Force`

        Zostanie wyświetlony monit o podanie hasła ścieżkę do udostępnionego folderu sieciowego, a hasło szyfrowania (ponieważ pakiet pomocy technicznej są szyfrowane). Pakiet pomocy technicznej zostanie utworzona w określonym folderze.

    - Akcji, które nie są chronione hasłem, nie ma potrzeby `-Credential` parametru. Wprowadź następujące informacje:

        `Export-HcsSupportPackage –PackageTag "MySupportPackage" -Force`

        Dla obu kontrolerów w folderze udostępnionym określonej sieci jest tworzona pakietu pomocy technicznej. Jest zaszyfrowany, skompresowany plik, który mogą być wysyłane do firmy Microsoft Support dotyczących rozwiązywania problemów. Aby uzyskać więcej informacji zobacz [Kontakt z pomocą techniczną firmy Microsoft](storsimple-contact-microsoft-support.md).


### <a name="the-export-hcssupportpackage-cmdlet-parameters"></a>Parametry polecenia cmdlet HcsSupportPackage eksportu
Za pomocą następujących parametrów przy użyciu polecenia cmdlet HcsSupportPackage eksportu.

| Parametr            | Wymagane opcjonalne | Opis                                                                                                                                                             |
|----------------------|-------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `-Path`                 | Wymagane          | Za pomocą Podaj lokalizację udostępnionego folderu sieciowego w którym znajduje się pakiet pomocy technicznej.                                                                 |
| `-EncryptionPassphrase` | Wymagane          | Umożliwia podanie hasła ułatwiające szyfrowanie pakietu pomocy technicznej.                                                                                                        |
| `-Credential`           | Opcjonalne          | Umożliwia podanie poświadczeń dostępu do udostępnionego folderu sieciowego.                                                                                        |
| `-Force`                | Opcjonalne          | Użyj, aby pominąć etap potwierdzenia hasło szyfrowania.                                                                                                                |
| `-PackageTag`           | Opcjonalne          | Umożliwia określenie katalogu *ścieżkę* , w której znajduje się pakiet pomocy technicznej. Wartość domyślna to [nazwa]-[bieżącej daty i time:yyyy-MM-dd-HH-mm-ss].       |
| `-Scope`                | Opcjonalne          | Można określić jako **klaster** (domyślne) w celu utworzenia pakietu pomocy technicznej dla obu kontrolerów. Jeśli chcesz utworzyć pakiet tylko dla bieżącego kontrolera, określ **Kontroler**. |


## <a name="edit-a-support-package"></a>Edytuj pakiet pomocy technicznej

Gdy masz wygenerowany opakowania pomocy technicznej, może być konieczne edytowanie pakietu usuwać poufne informacje. Dotyczy to także głośność nazwy, adresy IP urządzenia i kopii zapasowej nazwy z plików dziennika.

> [AZURE.IMPORTANT] Można edytować tylko pakiet pomocy technicznej, który został wygenerowany przy użyciu programu Windows PowerShell dla StorSimple. Nie można edytować pakietu utworzony w portalu klasyczny Azure za pomocą Menedżera StorSimple usługi.

Aby edytować pakiet pomocy technicznej przed przesłaniem go w witrynie Microsoft Support, najpierw odszyfrować pakiet pomocy technicznej, edytować pliki, a następnie ponownie go zaszyfrować. Wykonaj następujące czynności.

#### <a name="to-edit-a-support-package-in-windows-powershell-for-storsimple"></a>Aby edytować pakiet pomocy technicznej w programie Windows PowerShell dla StorSimple

1. Generowanie pakiet pomocy technicznej w sposób opisany w temacie [Aby utworzyć pakiet pomocy technicznej w programie Windows PowerShell dla StorSimple](#to-create-a-support-package-in-windows-powershell-for-storsimple).

2. [Pobierz skrypt](http://gallery.technet.microsoft.com/scriptcenter/Script-to-decrypt-a-a8d1ed65) lokalnie na komputerze klienckim.

3. Zaimportuj moduł programu Windows PowerShell. Określ ścieżkę do folderu lokalnego, w której są pobierane skrypt. Aby zaimportować moduł, wprowadź:

    `Import-module <Path to the folder that contains the Windows PowerShell script>`

4. Wszystkie pliki są *.aes* pliki, które są kompresowane i szyfrowane. Aby wyodrębnić i odszyfrowywanie plików, wprowadź:

    `Open-HcsSupportPackage <Path to the folder that contains support package files>`

    Należy zauważyć, że rzeczywista rozszerzenia nazw plików są teraz wyświetlane dla wszystkich plików.

    ![Edytuj pakiet pomocy technicznej](./media/storsimple-create-manage-support-package/IC750706.png)

5. Gdy zostanie wyświetlony monit o hasło szyfrowania, wprowadź hasło, które został użyty podczas tworzenia pakietu pomocy technicznej.

        cmdlet Open-HcsSupportPackage at command pipeline position 1

        Supply values for the following parameters:EncryptionPassphrase: ****

6. Przejdź do folderu zawierającego pliki dziennika. Ponieważ teraz dekompresowane i odszyfrować plików dziennika, te mają oryginalne rozszerzenia nazwy pliku. Zmodyfikuj te pliki, aby usunąć wszelkie informacje specyficzna dla klienta, takich jak głośność nazwy i adresy IP urządzenia i Zapisz pliki.

7. Zamykanie plików do kompresować gzip i szyfrowanie AES-256. Jest to szybkość i zabezpieczenia w przesyłania pakietu pomocy technicznej w sieci. Aby skompresować i szyfrowania plików, wprowadź następujące informacje:

    `Close-HcsSupportPackage <Path to the folder that contains support package files>`

    ![Edytuj pakiet pomocy technicznej](./media/storsimple-create-manage-support-package/IC750707.png)

8. Po wyświetleniu monitu podaj hasło szyfrowania pakietu zmienione pomocy technicznej.

        cmdlet Close-HcsSupportPackage at command pipeline position 1
        Supply values for the following parameters:EncryptionPassphrase: ****

9. Zanotuj nowe hasło, tak aby można udostępniać Microsoft Support zażądania.


### <a name="example-editing-files-in-a-support-package-on-a-password-protected-share"></a>Przykład: Edytowanie plików w pakiecie pomocy technicznej w udziale chronionych hasłem

W poniższym przykładzie pokazano sposób odszyfrowywanie, edytowanie i ponowne szyfrowanie pakiet pomocy technicznej.

        PS C:\WINDOWS\system32> Import-module C:\Users\Default\StorSimple\SupportPackage\HCSSupportPackageTools.psm1

        PS C:\WINDOWS\system32> Open-HcsSupportPackage \\hcsfs\Logs\TD48\TD48Logs\C0-A\etw

        cmdlet Open-HcsSupportPackage at command pipeline position 1

        Supply values for the following parameters:

        EncryptionPassphrase: ****

        PS C:\WINDOWS\system32> Close-HcsSupportPackage \\hcsfs\Logs\TD48\TD48Logs\C0-A\etw

        cmdlet Close-HcsSupportPackage at command pipeline position 1

        Supply values for the following parameters:

        EncryptionPassphrase: ****

        PS C:\WINDOWS\system32>

## <a name="next-steps"></a>Następne kroki

- Dowiedz się, jak za [pomocą urządzenia dzienniki rozwiązywać problemy z wdrożeniem urządzenia i pakietów pomocy technicznej](storsimple-troubleshoot-deployment.md#support-packages-and-device-logs-available-for-troubleshooting).

- Dowiedz się, jak [używać usługi Menedżera StorSimple administrowania urządzenia StorSimple](storsimple-manager-service-administration.md).
