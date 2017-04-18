<properties
    pageTitle="Automatyczne wykonywanie kopii zapasowych dla maszyn wirtualnych programu SQL Server (Menedżer zasobów) | Microsoft Azure"
    description="W tym artykule wyjaśniono funkcji automatycznego kopia zapasowa dla programu SQL Server uruchomiony w Azure maszyn wirtualnych za pomocą Menedżera zasobów. "
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-resource-manager"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="07/14/2016"
    ms.author="jroth" />

# <a name="automated-backup-for-sql-server-in-azure-virtual-machines-resource-manager"></a>Automatyczne wykonywanie kopii zapasowych dla programu SQL Server w środowisku maszyn wirtualnych Azure (Menedżer zasobów)

> [AZURE.SELECTOR]
- [Menedżer zasobów](virtual-machines-windows-sql-automated-backup.md)
- [Klasyczny](virtual-machines-windows-classic-sql-automated-backup.md)

Automatyczne wykonywanie kopii zapasowych automatycznie konfiguruje [Zarządzane kopia zapasowa Microsoft Azure](https://msdn.microsoft.com/library/dn449496.aspx) dla wszystkich istniejących, jak i nowych baz danych na maszyn wirtualnych Azure uruchomiony program SQL Server 2014 Standard lub Enterprise. Umożliwia konfigurowanie kopie zapasowe zwykłej bazie danych, które są używane trwałe magazynem obiektów blob Azure. Automatyczne wykonywanie kopii zapasowych zależy od [Rozszerzenia agenta programu SQL Server IaaS](virtual-machines-windows-sql-server-agent-extension.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]model klasyczny wdrożenia. Aby wyświetlić w klasycznej wersji programu w tym artykule, zobacz [Automatyczne wykonywanie kopii zapasowych dla programu SQL Server w klasycznym maszyn wirtualnych Azure](virtual-machines-windows-classic-sql-automated-backup.md).

## <a name="prerequisites"></a>Wymagania wstępne

Umożliwia automatyczne wykonywanie kopii zapasowych, należy wziąć pod uwagę następujące wymagania:

**System operacyjny**:

- System Windows Server 2012
- Windows Server 2012 R2

**Wersji wersję programu SQL Server**:

- SQL Server 2014 Standard
- SQL Server 2014 Enterprise

**Konfigurowanie bazy danych**:

- Docelowych baz danych, należy użyć modelu pełnego odzyskiwania

**Azure programu PowerShell**:

- [Instalowanie najnowszych polecenia programu PowerShell Azure](../powershell-install-configure.md) , jeśli zamierzasz skonfigurować automatyczne wykonywanie kopii zapasowych przy użyciu programu PowerShell.

>[AZURE.NOTE] Automatyczne wykonywanie kopii zapasowych zależy od rozszerzenia agenta programu SQL Server IaaS. Bieżący zdjęcia z galerii maszyn wirtualnych SQL dodać to rozszerzenie domyślnie. Aby uzyskać więcej informacji zobacz [Rozszerzenie agenta programu SQL Server IaaS](virtual-machines-windows-sql-server-agent-extension.md).

## <a name="settings"></a>Ustawienia

W poniższej tabeli opisano opcje, które można skonfigurować do automatycznego tworzenia kopii zapasowych. Procedura rzeczywista konfiguracja się różnić w zależności od tego, czy korzystasz z Azure portal lub polecenia programu PowerShell systemu Windows Azure.

|Ustawienie|Zakres (ustawienie domyślne)|Opis|
|---|---|---|
|**Automatyczne wykonywanie kopii zapasowych**|Włączanie i wyłączanie (wyłączone)|Włącza lub wyłącza automatyczne wykonywanie kopii zapasowych dla maszyn wirtualnych Azure uruchomiony program SQL Server 2014 Standard lub Enterprise.|
|**Okres przechowywania**|1-30 dni (30 dni)|Liczba dni przechowywania kopii zapasowej.|
|**Konto miejsca do magazynowania**|Azure przestrzeni dyskowej (rachunku miejsca do magazynowania utworzone dla określonej maszyn wirtualnych)|Konto Azure miejsca do magazynowania do przechowywania plików kopii zapasowej automatycznego w magazynie obiektów blob. Kontener jest tworzona w tej lokalizacji do przechowywania wszystkich plików kopii zapasowych. Konwencja nazewnictwa plik kopii zapasowej obejmuje daty i godziny oraz nazwa komputera.|
|**Szyfrowanie**|Włączanie i wyłączanie (wyłączone)|Włącza lub wyłącza szyfrowania. Po włączeniu szyfrowania certyfikaty używana do przywracania znajdują się na koncie określonego miejsca do magazynowania w tym samym kontenerze automaticbackup przy użyciu tej samej konwencji nazewnictwa. Zmiana hasła nowego certyfikatu jest generowany przy użyciu tego hasła, ale stary certyfikat pozostaje przywrócenie poprzednich kopie zapasowe.|
|**Hasło**|Tekst hasła, (Brak)|Hasło dla kluczy szyfrowania. Ta opcja jest wymagane, jeśli szyfrowanie jest włączone. Aby przywrócić zaszyfrowaną kopię zapasową, musisz mieć poprawne hasło i powiązanych certyfikat, który został użyty w momencie utworzenia kopii zapasowej.|

## <a name="configuration-in-the-portal"></a>Konfiguracja w portalu
Azure Portal umożliwia skonfigurować automatyczne wykonywanie kopii zapasowych, podczas inicjowania obsługi administracyjnej lub dla istniejących maszyny wirtualne.

### <a name="new-vms"></a>Nowe maszyny wirtualne
Azure Portal umożliwia skonfigurowanie automatycznego kopii zapasowej, podczas tworzenia nowego maszyny wirtualnej 2014 SQL Server w modelu wdrożenia Menedżera zasobów.

W karta **Ustawienia programu SQL Server** wybierz pozycję **automatyczne wykonywanie kopii zapasowych**. Następujące Azure portalu zrzucie ekranu pokazano karta **Kopia zapasowa automatycznego SQL** .

![Konfiguracja kopia zapasowa automatycznego SQL w Azure portal](./media/virtual-machines-windows-sql-automated-backup/azure-sql-arm-autobackup.png)

Dla kontekstu zobacz temat wykonane na [inicjowania obsługi administracyjnej maszyny wirtualnej programu SQL Server Azure](virtual-machines-windows-portal-sql-server-provision.md).

### <a name="existing-vms"></a>Istniejące maszyny wirtualne
Dla istniejących maszyn wirtualnych programu SQL Server wybierz ten komputer wirtualnych programu SQL Server. Zaznacz sekcję karta **Ustawienia** **konfiguracji programu SQL Server** .

![Zautomatyzowany kopia zapasowa SQL istniejące maszyny wirtualne](./media/virtual-machines-windows-sql-automated-backup/azure-sql-rm-autobackup-existing-vms.png)

W karta **konfiguracji programu SQL Server** kliknij przycisk **Edytuj** w sekcji kopii zapasowej automatycznego.

![Konfigurowanie automatycznego kopia zapasowa SQL dla istniejących maszyny wirtualne](./media/virtual-machines-windows-sql-automated-backup/azure-sql-rm-autobackup-configuration.png)

Po zakończeniu kliknij przycisk **OK** w dolnej części karta **konfiguracji programu SQL Server** , aby zapisać zmiany.

Jeśli automatyczne wykonywanie kopii zapasowych jest włączana po raz pierwszy, Azure konfiguruje agenta IaaS programu SQL Server w tle. W tym czasie Azure portal może nie zostać pokazany skonfigurowano automatyczne wykonywanie kopii zapasowych. Poczekaj kilka minut agent jest zainstalowany, zostanie skonfigurowane. Po wykonaniu tej Azure portal zostaną zastosowane nowe ustawienia.

>[AZURE.NOTE] Można również skonfigurować automatyczne wykonywanie kopii zapasowych przy użyciu szablonu. Aby uzyskać więcej informacji zobacz [Szybki Start Azure szablonu do automatycznego tworzenia kopii zapasowych](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-autobackup-update).

## <a name="configuration-with-powershell"></a>Konfigurowanie za pomocą programu PowerShell

Po zainicjowaniu obsługi administracyjnej maszyn wirtualnych usługi SQL, należy użyć programu PowerShell w celu skonfigurowania automatyczne wykonywanie kopii zapasowych.

W poniższym przykładzie programu PowerShell automatyczne wykonywanie kopii zapasowych jest skonfigurowana do istniejącego maszyn wirtualnych 2014 dla serwera SQL. Polecenie **AzureRM.Compute\New AzureVMSqlServerAutoBackupConfig** konfiguruje ustawienia automatycznego kopii zapasowej do przechowywania kopii zapasowych w konto Azure magazynowania skojarzone z maszyny wirtualnej. Te kopie zapasowe zostaną zachowane przez 10 dni. Polecenie **Zestaw AzureRmVMSqlServerExtension** aktualizuje określoną maszyn wirtualnych Azure przy użyciu tych ustawień.

    $vmname = "vmname"
    $resourcegroupname = "resourcegroupname"
    $autobackupconfig = AzureRM.Compute\New-AzureVMSqlServerAutoBackupConfig -Enable -RetentionPeriodInDays 10 -ResourceGroupName $resourcegroupname

    Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig -VMName $vmname -ResourceGroupName $resourcegroupname

Może zająć kilka minut, aby zainstalować i skonfigurować agenta IaaS programu SQL Server.

Aby włączyć szyfrowanie, należy zmodyfikować poprzedni skrypt w celu przekazania parametr **EnableEncryption** wraz z hasła (ciąg bezpiecznego) parametru **CertificatePassword** . Poniższy skrypt włącza ustawienia automatyczne wykonywanie kopii zapasowych w poprzednim przykładzie i dodaje szyfrowania.

    $vmname = "vmname"
    $resourcegroupname = "resourcegroupname"
    $password = "P@ssw0rd"
    $encryptionpassword = $password | ConvertTo-SecureString -AsPlainText -Force  
    $autobackupconfig = AzureRM.Compute\New-AzureVMSqlServerAutoBackupConfig -Enable -RetentionPeriod 10 -EnableEncryption -CertificatePassword $encryptionpassword -ResourceGroupName $resourcegroupname

    Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig -VMName $vmname -ResourceGroupName $resourcegroupname

Aby wyłączyć automatyczne wykonywanie kopii zapasowej, uruchom ten sam skrypt bez **— Włączanie** do polecenia **AzureRM.Compute\New AzureVMSqlServerAutoBackupConfig** . Brak **— Włączanie** parametru sygnały polecenie, aby wyłączyć tę funkcję. Podobnie jak w przypadku instalacji może potrwać kilka minut, aby wyłączyć automatyczne wykonywanie kopii zapasowych.

>[AZURE.NOTE] Usunięcie agenta IaaS programu SQL Server nie powoduje usunięcia wcześniej skonfigurowane ustawienia automatycznego kopii zapasowej. Wyłączanie lub odinstalowywanie agenta IaaS programu SQL Server, należy wyłączyć automatyczne wykonywanie kopii zapasowych.

## <a name="next-steps"></a>Następne kroki

Automatyczne wykonywanie kopii zapasowych konfiguruje maszyny wirtualne Azure zarządzane kopii zapasowej. Dlatego należy [zapoznać się z dokumentacją dla zarządzanych kopii zapasowej](https://msdn.microsoft.com/library/dn449496.aspx) należy rozumieć zachowanie i wpływ.

Można znaleźć dodatkowe wykonywanie kopii zapasowych i przywracanie wskazówki dotyczące programu SQL Server na maszyny wirtualne Azure w temacie: [Kopia zapasowa i przywracanie dla programu SQL Server w maszyn wirtualnych Azure](virtual-machines-windows-sql-backup-recovery.md).

Aby uzyskać informacji na temat innych zadań dostępne automatyzacji zobacz [Rozszerzenie agenta programu SQL Server IaaS](virtual-machines-windows-sql-server-agent-extension.md).

Aby uzyskać więcej informacji o uruchamianiu programu SQL Server na maszyny wirtualne Azure zobacz [Programu SQL Server w środowisku maszyn wirtualnych systemu Azure Przegląd](virtual-machines-windows-sql-server-iaas-overview.md).
