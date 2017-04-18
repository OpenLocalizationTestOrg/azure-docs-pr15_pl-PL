<properties
    pageTitle="Automatyczne wykonywanie kopii zapasowych dla maszyn wirtualnych programu SQL Server (klasyczny) | Microsoft Azure"
    description="W tym artykule wyjaśniono funkcji automatycznego kopia zapasowa dla programu SQL Server uruchomiony w Azure maszyn wirtualnych za pomocą Menedżera zasobów. "
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-service-management" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/26/2016"
    ms.author="jroth" />

# <a name="automated-backup-for-sql-server-in-azure-virtual-machines-classic"></a>Automatyczne wykonywanie kopii zapasowych dla programu SQL Server w środowisku maszyn wirtualnych Azure (klasyczny)

> [AZURE.SELECTOR]
- [Menedżer zasobów](virtual-machines-windows-sql-automated-backup.md)
- [Klasyczny](virtual-machines-windows-classic-sql-automated-backup.md)

Automatyczne wykonywanie kopii zapasowych automatycznie konfiguruje [Zarządzane kopia zapasowa Microsoft Azure](https://msdn.microsoft.com/library/dn449496.aspx) dla wszystkich istniejących, jak i nowych baz danych na maszyn wirtualnych Azure uruchomiony program SQL Server 2014 Standard lub Enterprise. Umożliwia konfigurowanie kopie zapasowe zwykłej bazie danych, które są używane trwałe magazynem obiektów blob Azure. Automatyczne wykonywanie kopii zapasowych zależy od [Rozszerzenia agenta programu SQL Server IaaS](virtual-machines-windows-classic-sql-server-agent-extension.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Aby wyświetlić wersję Menedżera zasobów w tym artykule, zobacz [Automatyczne wykonywanie kopii zapasowych dla programu SQL Server w Menedżerze zasobów maszyn wirtualnych Azure](virtual-machines-windows-sql-automated-backup.md).

## <a name="prerequisites"></a>Wymagania wstępne

Umożliwia automatyczne wykonywanie kopii zapasowych, należy wziąć pod uwagę następujące wymagania:

**System operacyjny**:

- System Windows Server 2012
- Windows Server 2012 R2

**Wersji wersję programu SQL Server**:

- SQL Server 2014 Standard
- SQL Server 2014 Enterprise

>[AZURE.NOTE] Program SQL Server 2016 nie jest jeszcze obsługiwane do automatycznego tworzenia kopii zapasowych.

**Konfigurowanie bazy danych**:

- Docelowych baz danych, należy użyć modelu pełnego odzyskiwania.

**Azure programu PowerShell**:

- [Instalowanie najnowszych polecenia programu PowerShell Azure](../powershell-install-configure.md).

**Rozszerzenie IaaS serwera SQL**:

- [Zainstaluj rozszerzenie IaaS serwera SQL](virtual-machines-windows-classic-sql-server-agent-extension.md).

## <a name="settings"></a>Ustawienia

W poniższej tabeli opisano opcje, które można skonfigurować do automatycznego tworzenia kopii zapasowych. Dla klasyczny maszyny wirtualne należy użyć programu PowerShell, aby skonfigurować te ustawienia.

|Ustawienie|Zakres (ustawienie domyślne)|Opis|
|---|---|---|
|**Automatyczne wykonywanie kopii zapasowych**|Włączanie i wyłączanie (wyłączone)|Włącza lub wyłącza automatyczne wykonywanie kopii zapasowych dla maszyn wirtualnych Azure uruchomiony program SQL Server 2014 Standard lub Enterprise.|
|**Okres przechowywania**|1-30 dni (30 dni)|Liczba dni przechowywania kopii zapasowej.|
|**Konto miejsca do magazynowania**|Azure przestrzeni dyskowej (rachunku miejsca do magazynowania utworzone dla określonej maszyn wirtualnych)|Konto Azure miejsca do magazynowania do przechowywania plików kopii zapasowej automatycznego w magazynie obiektów blob. Kontener jest tworzona w tej lokalizacji do przechowywania wszystkich plików kopii zapasowych. Konwencja nazewnictwa plik kopii zapasowej obejmuje daty i godziny oraz nazwa komputera.|
|**Szyfrowanie**|Włączanie i wyłączanie (wyłączone)|Włącza lub wyłącza szyfrowania. Po włączeniu szyfrowania certyfikaty używana do przywracania znajdują się na koncie określonego miejsca do magazynowania w tym samym kontenerze automaticbackup przy użyciu tej samej konwencji nazewnictwa. Zmiana hasła nowego certyfikatu jest generowany przy użyciu tego hasła, ale stary certyfikat pozostaje przywrócenie poprzednich kopie zapasowe.|
|**Hasło**|Tekst hasła, (Brak)|Hasło dla kluczy szyfrowania. Ta opcja jest wymagane, jeśli szyfrowanie jest włączone. Aby przywrócić zaszyfrowaną kopię zapasową, musisz mieć poprawne hasło i powiązanych certyfikat, który został użyty w momencie utworzenia kopii zapasowej.|

## <a name="configuration-with-powershell"></a>Konfigurowanie za pomocą programu PowerShell

W poniższym przykładzie programu PowerShell automatyczne wykonywanie kopii zapasowych jest skonfigurowana do istniejącego maszyn wirtualnych 2014 dla serwera SQL. Polecenie **Nowy AzureVMSqlServerAutoBackupConfig** konfiguruje ustawienia automatycznego kopii zapasowej do przechowywania kopii zapasowych w oknie konta magazynu platformy Azure określoną przez zmienną $storageaccount. Te kopie zapasowe zostaną zachowane przez 10 dni. Polecenie **Zestaw AzureVMSqlServerExtension** aktualizuje określoną maszyn wirtualnych Azure przy użyciu tych ustawień.

    $storageaccount = "<storageaccountname>"
    $storageaccountkey = (Get-AzureStorageKey -StorageAccountName $storageaccount).Primary
    $storagecontext = New-AzureStorageContext -StorageAccountName $storageaccount -StorageAccountKey $storageaccountkey
    $autobackupconfig = New-AzureVMSqlServerAutoBackupConfig -StorageContext $storagecontext -Enable -RetentionPeriod 10

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -AutoBackupSettings $autobackupconfig | Update-AzureVM

Może zająć kilka minut, aby zainstalować i skonfigurować agenta IaaS programu SQL Server.

Aby włączyć szyfrowanie, należy zmodyfikować poprzedni skrypt w celu przekazania parametr EnableEncryption wraz z hasła (ciąg bezpiecznego) parametru CertificatePassword. Poniższy skrypt włącza ustawienia automatyczne wykonywanie kopii zapasowych w poprzednim przykładzie i dodaje szyfrowania.

    $storageaccount = "<storageaccountname>"
    $storageaccountkey = (Get-AzureStorageKey -StorageAccountName $storageaccount).Primary
    $storagecontext = New-AzureStorageContext -StorageAccountName $storageaccount -StorageAccountKey $storageaccountkey
    $password = "P@ssw0rd"
    $encryptionpassword = $password | ConvertTo-SecureString -AsPlainText -Force  
    $autobackupconfig = New-AzureVMSqlServerAutoBackupConfig -StorageContext $storagecontext -Enable -RetentionPeriod 10 -EnableEncryption -CertificatePassword $encryptionpassword

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -AutoBackupSettings $autobackupconfig | Update-AzureVM

Aby wyłączyć automatyczne wykonywanie kopii zapasowej, uruchom ten sam skrypt bez **— Włączanie** parametr **AzureVMSqlServerAutoBackupConfig nowy**. Podobnie jak w przypadku instalacji może potrwać kilka minut, aby wyłączyć automatyczne wykonywanie kopii zapasowych.

>[AZURE.NOTE] Wyłączanie i odinstalowywania agenta IaaS programu SQL Server nie powoduje usunięcia wcześniej skonfigurowane ustawienia zarządzania kopii zapasowej. Wyłączanie lub odinstalowywanie agenta IaaS programu SQL Server, należy wyłączyć automatyczne wykonywanie kopii zapasowych.

## <a name="next-steps"></a>Następne kroki

Automatyczne wykonywanie kopii zapasowych konfiguruje maszyny wirtualne Azure zarządzane kopii zapasowej. Dlatego należy [zapoznać się z dokumentacją dla zarządzanych kopii zapasowej](https://msdn.microsoft.com/library/dn449496.aspx) należy rozumieć zachowanie i wpływ.

Można znaleźć dodatkowe wykonywanie kopii zapasowych i przywracanie wskazówki dotyczące programu SQL Server na maszyny wirtualne Azure w temacie: [Kopia zapasowa i przywracanie dla programu SQL Server w maszyn wirtualnych Azure](virtual-machines-windows-sql-backup-recovery.md).

Aby uzyskać informacji na temat innych zadań dostępne automatyzacji zobacz [Rozszerzenie agenta programu SQL Server IaaS](virtual-machines-windows-classic-sql-server-agent-extension.md).

Aby uzyskać więcej informacji o uruchamianiu programu SQL Server na maszyny wirtualne Azure zobacz [Programu SQL Server w środowisku maszyn wirtualnych systemu Azure Przegląd](virtual-machines-windows-sql-server-iaas-overview.md).
