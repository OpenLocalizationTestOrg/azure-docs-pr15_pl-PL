<properties
    pageTitle="Program SQL Server Agent rozszerzenia dla maszyny wirtualne programu SQL Server (Menedżer zasobów) | Microsoft Azure"
    description="W tym temacie opisano, jak zarządzać rozszerzenie agenta programu SQL Server, który pozwala zautomatyzować określonych zadań administracyjnych programu SQL Server. Należą do automatycznego kopii zapasowej, automatycznego poprawiania i integracji magazynu klucza Azure. W tym temacie jest używany tryb wdrożenia Menedżera zasobów."
    services="virtual-machines-windows"
    documentationCenter=""
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
    ms.date="10/27/2016"
    ms.author="jroth"/>

# <a name="sql-server-agent-extension-for-sql-server-vms-resource-manager"></a>Program SQL Server Agent rozszerzenia dla maszyny wirtualne programu SQL Server (Menedżer zasobów)

> [AZURE.SELECTOR]
- [Menedżer zasobów](virtual-machines-windows-sql-server-agent-extension.md)
- [Klasyczny](virtual-machines-windows-classic-sql-server-agent-extension.md)

Rozszerzenie agenta IaaS SQL Server (SQLIaaSExtension) jest uruchamiany w Azure maszyn wirtualnych do automatyzowania zadań administracyjnych. W tym temacie omówiono usług obsługiwanych przez rozszerzenia, a także instrukcje dotyczące instalacji, status i usuwania.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]model klasyczny wdrożenia. Aby wyświetlić w klasycznej wersji programu w tym artykule, zobacz [Rozszerzenie agenta serwera SQL dla programu SQL Server maszyny wirtualne klasyczny](virtual-machines-windows-classic-sql-server-agent-extension.md).

## <a name="supported-services"></a>Obsługiwane usługi

Rozszerzenie agenta programu SQL Server IaaS obsługuje następujące zadania:

| Funkcja administrowania | Opis |
|---------------------|-------------------------------|
| **SQL zautomatyzowanego tworzenia kopii zapasowych** | Zautomatyzowanie planowania wykonywania kopii zapasowych dla wszystkich baz danych dla domyślnego wystąpienia programu SQL Server w maszyn wirtualnych. Aby uzyskać więcej informacji zobacz [Kopia zapasowa automatycznego dla programu SQL Server w maszyn wirtualnych Azure (Menedżer zasobów)](virtual-machines-windows-sql-automated-backup.md).|
| **SQL automatycznego poprawiania** | Konfiguruje oknem konserwacji, w którym aktualizacji usługi maszyn wirtualnych może potrwać miejscu, więc można uniknąć aktualizacje podczas Szczyt terminy z pracą. Aby uzyskać więcej informacji zobacz [automatycznego poprawiania dla programu SQL Server w maszyn wirtualnych Azure (Menedżer zasobów)](virtual-machines-windows-sql-automated-patching.md).|
| **Integracja Azure klucza magazynu** | Umożliwia automatyczne instalowania i konfigurowania magazynu klucza Azure na maszyn wirtualnych usługi SQL Server. Aby uzyskać więcej informacji zobacz [Konfigurowanie Azure klucza magazynu integracji programu SQL Server na Azure maszyny wirtualne (Menedżer zasobów)](virtual-machines-windows-ps-sql-keyvault.md).|

## <a name="prerequisites"></a>Wymagania wstępne

Wymagania dotyczące korzystania z rozszerzenia agenta programu SQL Server IaaS na swojej maszyn wirtualnych:

**System operacyjny**:

- System Windows Server 2012
- Windows Server 2012 R2

**Wersje programu SQL Server**:

- Program SQL Server 2012
- Program SQL Server 2014 r.
- Program SQL Server 2016

**Azure programu PowerShell**:

- [Pobierz i skonfiguruj najnowszą polecenia programu PowerShell Azure](../powershell-install-configure.md)

## <a name="installation"></a>Instalacji

Rozszerzenie agenta programu SQL Server IaaS jest instalowane automatycznie, gdy obsługi administracyjnej jeden z galerii obrazów maszyn wirtualnych programu SQL Server.

Jeśli tworzysz maszyn wirtualnych tylko do systemu operacyjnego Windows Server, możesz zainstalować rozszerzenie ręcznie przy użyciu polecenia cmdlet **Set-AzureVMSqlServerExtension** programu PowerShell. Na przykład następujące polecenie instaluje rozszerzenia na maszyn wirtualnych tylko do systemu operacyjnego Windows Server i nadaje "SQLIaaSExtension".

    Set-AzureRmVMSqlServerExtension -ResourceGroupName "resourcegroupname" -VMName "vmname" -Name "SQLIaasExtension" -Version "1.2"

Zostanie zaktualizowane do najnowszej wersji rozszerzenia agenta IaaS SQL, należy ponownie uruchomić komputer wirtualnych po zaktualizowaniu rozszerzenia.

>[AZURE.NOTE] Jeśli ręcznie zainstalować rozszerzenie agenta programu SQL Server IaaS na maszyn wirtualnych systemu Windows Server, należy użyć i zarządzać jego funkcji za pomocą polecenia programu PowerShell. Interfejs portalu jest dostępna tylko dla programu SQL Server galerii obrazów.

## <a name="status"></a>Stan

Jeden sposób, aby sprawdzić, czy jest zainstalowana rozszerzenie jest wyświetlanie stanu agenta w Azure Portal. Zaznacz **wszystkie ustawienia** w karta maszyn wirtualnych, a następnie kliknij na **rozszerzenia**. Należy skontaktować się z rozszerzeniem **SQLIaaSExtension** na liście.

![Program SQL Server IaaS Agent rozszerzenie w Azure Portal](./media/virtual-machines-windows-sql-server-agent-extension/azure-rm-sql-server-iaas-agent-portal.png)

Umożliwia także polecenia cmdlet **Get-AzureVMSqlServerExtension** Azure programu Powershell.

    Get-AzureRmVMSqlServerExtension -VMName "vmname" -ResourceGroupName "resourcegroupname"

Polecenie poprzednie potwierdza agent jest zainstalowany, a także informacje o stanie ogólne. Możesz również wyświetlić informacje dotyczące automatycznego tworzenia kopii zapasowych i Patching określonego stanu z następujących poleceń.

    $sqlext = Get-AzureRmVMSqlServerExtension -VMName "vmname" -ResourceGroupName "resourcegroupname"
    $sqlext.AutoPatchingSettings
    $sqlext.AutoBackupSettings

## <a name="removal"></a>Usuwanie   

W portalu Azure po odinstalowaniu rozszerzenia, klikając pozycję Wielokropek na karta **rozszerzenia** usługi właściwości maszyn wirtualnych. Następnie kliknij przycisk **Usuń**.

![Odinstalowywanie rozszerzenia programu SQL Server IaaS Agent w Azure Portal](./media/virtual-machines-windows-sql-server-agent-extension/azure-rm-sql-server-iaas-agent-uninstall.png)

Można też użyć polecenia cmdlet programu Powershell **AzureRmVMSqlServerExtension Usuń** .

    Remove-AzureRmVMSqlServerExtension -ResourceGroupName "resourcegroupname" -VMName "vmname" -Name "SQLIaasExtension"

## <a name="next-steps"></a>Następne kroki

Rozpocznij przy użyciu jednego z obsługiwanych przez rozszerzenie usług. Aby uzyskać więcej informacji zobacz tematy, do których odwołują się [obsługiwane usług](#supported-services) części tego artykułu.

Aby uzyskać więcej informacji o uruchamianiu programu SQL Server na maszyn wirtualnych Azure zobacz [Programu SQL Server w środowisku maszyn wirtualnych systemu Azure Przegląd](virtual-machines-windows-sql-server-iaas-overview.md).
