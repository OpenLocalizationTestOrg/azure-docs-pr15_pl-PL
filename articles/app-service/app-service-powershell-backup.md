<properties
    pageTitle="Wykonywanie kopii zapasowych i przywracanie aplikacji usługi aplikacji za pomocą programu PowerShell"
    description="Dowiedz się, jak za pomocą programu PowerShell wykonywanie kopii zapasowych i przywracanie aplikacji w usłudze Azure aplikacji"
    services="app-service"
    documentationCenter=""
    authors="NKing92"
    manager="wpickett"
    editor="" />

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="nicking"/>
# <a name="use-powershell-to-back-up-and-restore-app-service-apps"></a>Wykonywanie kopii zapasowych i przywracanie aplikacji usługi aplikacji za pomocą programu PowerShell

> [AZURE.SELECTOR]
- [Programu PowerShell](app-service-powershell-backup.md)
- [INTERFEJSU API USŁUGI REST](../app-service-web/websites-csm-backup.md)

Dowiedz się, jak za pomocą programu PowerShell Azure wykonywanie kopii zapasowych i przywracanie [aplikacji usługi aplikacji](https://azure.microsoft.com/services/app-service/web/). Aby uzyskać więcej informacji na temat kopie zapasowe aplikacji sieci web, w tym wymagania i ograniczenia zobacz [Tworzenie kopii zapasowej aplikacji sieci web w usłudze Azure aplikacji](../app-service-web/web-sites-backup.md).

## <a name="prerequisites"></a>Wymagania wstępne
Aby użyć programu PowerShell do zarządzania kopie zapasowe aplikacji, są potrzebne następujące elementy:

- **Adres URL skojarzeń zabezpieczeń** umożliwiający odczytu i zapisu do kontenera magazyn Azure. Aby uzyskać wyjaśnienie skojarzeń zabezpieczeń adresów URL, zobacz [Opis modelu skojarzeń zabezpieczeń](../storage/storage-dotnet-shared-access-signature-part-1.md) . Zobacz [Przy użyciu programu PowerShell Azure z nośnikami Azure](../storage/storage-powershell-guide-full.md) przykłady Zarządzanie magazynem Azure przy użyciu programu PowerShell.
- **Ciąg połączenia bazy danych** , jeśli chcesz utworzyć kopię zapasową bazy danych oraz aplikacji sieci web.

### <a name="how-to-generate-a-sas-url-to-use-with-the-web-app-backup-cmdlets"></a>Jak Generowanie adresu URL skojarzenia zabezpieczeń do użytku z poleceń cmdlet kopii zapasowej aplikacji sieci web
Adres URL skojarzenia zabezpieczeń mogą być generowane przy użyciu programu PowerShell. Oto przykładowy sposób wygenerować jedną, który może być używany z poleceń cmdlet omawiane w tym artykule.

        $storageAccountName = "<your storage account's name>"
        $storageAccountRg = "<your storage account's resource group>"

        # This returns an array of keys for your storage account. Be sure to select the appropriate key. Here we select the first key as a default.
        $storageAccountKey = Get-AzureRmStorageAccountKey -ResourceGroupName $storageAccountRg -Name $storageAccountName
        $context = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey[0].Value

        $blobContainerName = "<name of blob container for app backups>"
        $sasUrl = New-AzureStorageContainerSASToken -Name $blobContainerName -Permission rwdl -Context $context -ExpiryTime (Get-Date).AddMonths(1) -FullUri

## <a name="install-azure-powershell-132-or-greater"></a>Instalowanie programu PowerShell Azure 1.3.2 lub nowszego

Aby uzyskać instrukcje dotyczące instalacji i używania Azure programu PowerShell, zobacz [Za pomocą programu Azure przy użyciu Menedżera zasobów Azure](../powershell-install-configure.md) .

## <a name="create-a-backup"></a>Tworzenie kopii zapasowej

Aby utworzyć kopię zapasową aplikacji sieci web, należy użyć polecenia cmdlet New-AzureRmWebAppBackup.

        $sasUrl = "<your SAS URL>"
        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"

        $backup = New-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -StorageAccountUrl $sasUrl

Spowoduje to utworzenie kopii zapasowej o nazwie automatycznie wygenerowanego. Jeśli chcesz podać nazwę kopii zapasowej, należy użyć parametru opcjonalne NazwaKopiiZapasowej.

        $backup = New-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -StorageAccountUrl $sasUrl -BackupName MyBackup

Aby dołączyć bazy danych w kopii zapasowej, najpierw utworzyć ustawienie kopii zapasowej bazy danych przy użyciu polecenia cmdlet AzureRmWebAppDatabaseBackupSetting nowy, a następnie podać to ustawienie w parametrze baz danych polecenia cmdlet New-AzureRmWebAppBackup. Parametr baz danych można tablicę ustawień bazy danych, co umożliwia wykonywanie kopii zapasowej więcej niż jedną bazę danych.

        $dbSetting1 = New-AzureRmWebAppDatabaseBackupSetting -Name DB1 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbSetting2 = New-AzureRmWebAppDatabaseBackupSetting -Name DB2 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbBackup = New-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -BackupName MyBackup -StorageAccountUrl $sasUrl -Databases $dbSetting1,$dbSetting2

## <a name="get-backups"></a>Pobieranie kopii zapasowych

Polecenia cmdlet Get-AzureRmWebAppBackupList zwraca tablicę wszystkich kopii zapasowych dla aplikacji sieci web. Należy podać nazwę aplikacji sieci web i jego grupa zasobów.

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        $backups = Get-AzureRmWebAppBackupList -Name $appName -ResourceGroupName $resourceGroupName

Aby uzyskać kopii zapasowej, należy użyć polecenia cmdlet Get-AzureRmWebAppBackup.

        $backup = Get-AzureRmWebAppBackup -Name $appName -ResourceGroupName $resourceGroupName -BackupId 10102

Można również potoku obiekt aplikacji sieci web do dowolnego polecenia cmdlet zarządzania kopii zapasowej dla wygody.

        $app = Get-AzureRmWebApp -Name ContosoApp -ResourceGroupName Default-Web-WestUS
        $backupList = $app | Get-AzureRmWebAppBackupList
        $backup = $app | Get-AzureRmWebAppBackup -BackupId 10102

## <a name="schedule-automatic-backups"></a>Planowanie automatycznego wykonywania kopii zapasowych

Można zaplanować wykonywania kopii zapasowych zostanie przeprowadzona automatycznie w określonych odstępach czasu. Aby skonfigurować harmonogram wykonywania kopii zapasowych, należy użyć polecenia cmdlet AzureRmWebAppBackupConfiguration edycji. To polecenie cmdlet przyjmuje kilka parametrów:

- **Nazwa** - Nazwa aplikacji sieci web.
- **ResourceGroupName** - nazwę grupy zasobów zawierające aplikacji sieci web.
- **Przedział** - opcjonalne. Nazwa przedział aplikacji sieci web.
- **StorageAccountUrl** - URL skojarzeń zabezpieczeń kontenera magazynu Azure służy do przechowywania kopii zapasowych.
- **FrequencyInterval** - wartość liczbową dla jak często należy kopie zapasowe. Musi być dodatnią liczbą całkowitą.
- **FrequencyUnit** - jednostki czasu dla jak często należy kopie zapasowe. Opcje są godziny i dni.
- **RetentionPeriodInDays** - ile dni automatycznych kopii zapasowych powinien być zapisany zanim zostaną automatycznie usunięte.
- **Czas rozpoczęcia** - opcjonalne. Czas rozpoczęcia automatycznych kopii zapasowych. Wykonywanie kopii zapasowych rozpoczyna się natychmiast, jeśli jest to wartość null. Musi być daty i godziny.
- **Bazy danych** — opcjonalnie. Tablica DatabaseBackupSettings baz danych do wykonania kopii zapasowej.
- **KeepAtLeastOneBackup** — opcjonalnie przełączenia parametru. Podanie tego jedna kopia zapasowa zawsze powinny być zachowywane na koncie miejsca do magazynowania, niezależnie od tego, jak stare jest.

Oto przykładowy sposób używać tego polecenia cmdlet.

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        $slotName = "StagingSlot"
        $dbSetting1 = New-AzureRmWebAppDatabaseBackupSetting -Name DB1 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbSetting2 = New-AzureRmWebAppDatabaseBackupSetting -Name DB2 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        Edit-AzureRmWebAppBackupConfiguration -Name $appName -ResourceGroupName $resourceGroupName -Slot $slotName `
          -StorageAccountUrl "<your SAS URL>" -FrequencyInterval 6 -FrequencyUnit Hour -Databases $dbSetting1,$dbSetting2 `
          -KeepAtLeastOneBackup -StartTime (Get-Date).AddHours(1)

Aby uzyskać bieżący harmonogram kopii zapasowej, należy użyć polecenia cmdlet Get-AzureRmWebAppBackupConfiguration. Może to być przydatne w przypadku modyfikowanie harmonogramu, która została już skonfigurowana.

        $configuration = Get-AzureRmWebAppBackupConfiguration -Name $appName -ResourceGroupName $resourceGroupName

        # Modify the configuration slightly
        $configuration.FrequencyInterval = 2
        $configuration.FrequencyUnit = "Day"

        # Apply the new configuration by piping it into the Edit-AzureRmWebAppBackupConfiguration cmdlet
        $configuration | Edit-AzureRmWebAppBackupConfiguration

## <a name="restore-a-web-app-from-a-backup"></a>Przywracanie z kopii zapasowej aplikacji sieci web

Aby przywrócić aplikacji sieci web z kopii zapasowej, należy użyć polecenia cmdlet AzureRmWebAppBackup przywracania. Najłatwiejszym sposobem za pomocą tego polecenia cmdlet jest potoku kopii zapasowej obiektu pobierane z polecenia cmdlet Get-AzureRmWebAppBackup lub polecenia cmdlet Get-AzureRmWebAppBackupList.

Po umieszczeniu kopii zapasowej obiektu, możesz go potoku do polecenia cmdlet AzureRmWebAppBackup przywracania. Ustaw wartość parametru przełącznika Zastąp oznaczającą zamierzają zastąpić zawartość kopii zapasowej zawartości aplikacji sieci web. Jeśli kopia zapasowa zawiera baz danych, także zostaną przywrócone bazy danych.

        $backup | Restore-AzureRmWebAppBackup -Overwrite

Oto przykład zastosowania AzureRmWebAppBackup przywracania, określając wszystkie parametry.

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        $slotName = "StagingSlot"
        $blobName = "ContosoBackup.zip"
        $dbSetting1 = New-AzureRmWebAppDatabaseBackupSetting -Name DB1 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbSetting2 = New-AzureRmWebAppDatabaseBackupSetting -Name DB2 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        Restore-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -Slot $slotName -StorageAccountUrl "<your SAS URL>" -BlobName $blobName -Databases $dbSetting1,$dbSetting2 -Overwrite

## <a name="delete-a-backup"></a>Usuwanie kopii zapasowej

Aby usunąć kopię zapasową, należy użyć polecenia cmdlet AzureRmWebAppBackup Usuń. Spowoduje to usunięcie kopii zapasowej z Twojego konta miejsca do magazynowania. Określ nazwę aplikacji, jego grupa zasobów i identyfikator kopii zapasowej, którą chcesz usunąć.

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        Remove-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -BackupId 10102

Można również potoku kopii zapasowej obiektu do polecenia cmdlet AzureRmWebAppBackup usuń go usunąć.

        $backup = Get-AzureRmWebAppBackup -Name $appName -ResourceGroupName $resourceGroupName -BackupId 10102
        $backup | Remove-AzureRmWebAppBackup -Overwrite
