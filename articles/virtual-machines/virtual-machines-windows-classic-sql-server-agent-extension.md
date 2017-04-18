<properties
    pageTitle="Program SQL Server Agent rozszerzenia dla maszyny wirtualne programu SQL Server (klasyczny) | Microsoft Azure"
    description="W tym temacie opisano, jak zarządzać rozszerzenie agenta programu SQL Server, który pozwala zautomatyzować określonych zadań administracyjnych programu SQL Server. Należą do automatycznego kopii zapasowej, automatycznego poprawiania i integracji magazynu klucza Azure. W tym temacie korzysta z trybu klasycznego wdrożenia."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/27/2016"
    ms.author="jroth"/>

# <a name="sql-server-agent-extension-for-sql-server-vms-classic"></a>Program SQL Server Agent rozszerzenia dla maszyny wirtualne programu SQL Server (klasyczny)

> [AZURE.SELECTOR]
- [Menedżer zasobów](virtual-machines-windows-sql-server-agent-extension.md)
- [Klasyczny](virtual-machines-windows-classic-sql-server-agent-extension.md)

Rozszerzenie agenta IaaS SQL Server (SQLIaaSAgent) jest uruchamiany w Azure maszyn wirtualnych do automatyzowania zadań administracyjnych. W tym temacie omówiono usług obsługiwanych przez rozszerzenia, a także instrukcje dotyczące instalacji, status i usuwania.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Aby wyświetlić wersję Menedżera zasobów w tym artykule, zobacz [Rozszerzenie agenta serwera SQL dla programu SQL Server maszyny wirtualne Menedżera zasobów](virtual-machines-windows-sql-server-agent-extension.md).

## <a name="supported-services"></a>Obsługiwane usługi

Rozszerzenie agenta programu SQL Server IaaS obsługuje następujące zadania:

| Funkcja administrowania | Opis |
|---------------------|-------------------------------|
| **SQL zautomatyzowanego tworzenia kopii zapasowych** | Zautomatyzowanie planowania wykonywania kopii zapasowych dla wszystkich baz danych dla domyślnego wystąpienia programu SQL Server w maszyn wirtualnych. Aby uzyskać więcej informacji zobacz [automatyczne wykonywanie kopii zapasowych dla programu SQL Server w maszyn wirtualnych Azure (klasyczny)](virtual-machines-windows-classic-sql-automated-backup.md).|
| **SQL automatycznego poprawiania** | Konfiguruje oknem konserwacji, w którym aktualizacji usługi maszyn wirtualnych może potrwać miejscu, więc można uniknąć aktualizacje podczas Szczyt terminy z pracą. Aby uzyskać więcej informacji zobacz [Automatyczne poprawianie dla programu SQL Server w maszyn wirtualnych Azure (klasyczny)](virtual-machines-windows-classic-sql-automated-patching.md).|
| **Integracja Azure klucza magazynu** | Umożliwia automatyczne instalowania i konfigurowania magazynu klucza Azure na maszyn wirtualnych usługi SQL Server. Aby uzyskać więcej informacji zobacz [Konfigurowanie Azure klucza magazynu integracji programu SQL Server na Azure maszyny wirtualne (klasyczny)](virtual-machines-windows-classic-ps-sql-keyvault.md).|

## <a name="prerequisites"></a>Wymagania wstępne

Wymagania dotyczące korzystania z rozszerzenia agenta programu SQL Server IaaS na swojej maszyn wirtualnych:

### <a name="operating-system"></a>System operacyjny:

- System Windows Server 2012
- Windows Server 2012 R2

### <a name="sql-server-versions"></a>Wersje programu SQL Server:

- Program SQL Server 2012
- Program SQL Server 2014 r.
- Program SQL Server 2016

### <a name="azure-powershell"></a>Azure programu PowerShell:

[Pobierz i skonfiguruj najnowszą polecenia programu PowerShell Azure](../powershell-install-configure.md).

Uruchom program Windows PowerShell i połączyć go z subskrypcji usługi Azure przy użyciu polecenia **Dodaj AzureAccount** .

    Add-AzureAccount

Jeśli masz wiele subskrypcji, za pomocą **AzureSubscription wybierz pozycję** Wybierz subskrypcję, która zawiera docelowy klasyczny maszyn wirtualnych.

    Select-AzureSubscription -SubscriptionName <subscriptionname>

Na tym etapie można uzyskać listę klasyczny maszyn wirtualnych i ich nazwy skojarzonej usługi przy użyciu polecenia **Get-AzureVM** .

    Get-AzureVM

## <a name="installation"></a>Instalacji

Dla klasyczny maszyny wirtualne należy użyć programu PowerShell do rozszerzenia agenta programu SQL Server IaaS zainstalować i skonfigurować skojarzonych usług. Zainstaluj rozszerzenie przy użyciu polecenia cmdlet **Set-AzureVMSqlServerExtension** programu PowerShell. Na przykład następujące polecenie instaluje rozszerzenia w systemie Windows Server maszyn wirtualnych (klasyczny) i nadaje "SQLIaaSExtension".

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -ReferenceName "SQLIaasExtension" -Version "1.2" | Update-AzureVM

Zostanie zaktualizowane do najnowszej wersji rozszerzenia agenta IaaS SQL, należy ponownie uruchomić komputer wirtualnych po zaktualizowaniu rozszerzenia.

>[AZURE.NOTE] Klasyczny maszyn wirtualnych nie ma odpowiednią opcję, aby zainstalować i skonfigurować rozszerzenie agenta IaaS SQL za pośrednictwem portalu.

## <a name="status"></a>Stan

Jeden sposób, aby sprawdzić, czy jest zainstalowana rozszerzenie jest wyświetlanie stanu agenta w Azure Portal. Zaznacz **wszystkie ustawienia** w karta maszyn wirtualnych, a następnie kliknij na **rozszerzenia**. Należy skontaktować się z rozszerzeniem **SQLIaaSAgent** na liście.

![Program SQL Server IaaS Agent rozszerzenie w Azure Portal](./media/virtual-machines-windows-classic-sql-server-agent-extension/azure-sql-server-iaas-agent-portal.png)

Umożliwia także polecenia cmdlet **Get-AzureVMSqlServerExtension** Azure programu Powershell.

    Get-AzureVM –ServiceName "service" –Name "vmname" | Get-AzureVMSqlServerExtension

## <a name="removal"></a>Usuwanie   

W portalu Azure po odinstalowaniu rozszerzenia, klikając pozycję Wielokropek na karta **rozszerzenia** usługi właściwości maszyn wirtualnych. Następnie kliknij przycisk **Usuń**.

![Odinstalowywanie rozszerzenia programu SQL Server IaaS Agent w Azure Portal](./media/virtual-machines-windows-classic-sql-server-agent-extension/azure-sql-server-iaas-agent-uninstall.png)

Można też użyć polecenia cmdlet programu Powershell **AzureVMSqlServerExtension Usuń** .

    Get-AzureVM –ServiceName "service" –Name "vmname" | Remove-AzureVMSqlServerExtension | Update-AzureVM

## <a name="next-steps"></a>Następne kroki

Rozpocznij przy użyciu jednego z obsługiwanych przez rozszerzenie usług. Aby uzyskać więcej informacji zobacz tematy, do których odwołują się [obsługiwane usług](#supported-services) części tego artykułu.

Aby uzyskać więcej informacji o uruchamianiu programu SQL Server na maszyn wirtualnych Azure zobacz [Programu SQL Server w środowisku maszyn wirtualnych systemu Azure Przegląd](virtual-machines-windows-sql-server-iaas-overview.md).
