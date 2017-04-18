<properties
    pageTitle="Automatyczne poprawianie dla maszyny wirtualne programu SQL Server (klasyczny) | Microsoft Azure"
    description="W tym miejscu wyjaśniono funkcji automatycznego poprawiania dla programu SQL Server maszyn wirtualnych działa w trybie klasycznym wdrożenia Azure."
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

# <a name="automated-patching-for-sql-server-in-azure-virtual-machines-classic"></a>Automatyczne poprawianie dla programu SQL Server w środowisku maszyn wirtualnych Azure (klasyczny)

> [AZURE.SELECTOR]
- [Menedżer zasobów](virtual-machines-windows-sql-automated-patching.md)
- [Klasyczny](virtual-machines-windows-classic-sql-automated-patching.md)

Automatyczne Patching określa oknem konserwacji dla Azure maszyny wirtualnej uruchomiony program SQL Server. Aktualizacje automatyczne można zainstalować tylko w tym oknie konserwacji. Dla programu SQL Server gwarantuje, że aktualizacje systemu i wszystkie skojarzone uruchomieniu występują w najlepszej godziny dla bazy danych. Automatyczne Patching zależy od [Rozszerzenia agenta programu SQL Server IaaS](virtual-machines-windows-classic-sql-server-agent-extension.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Aby wyświetlić wersję Menedżera zasobów tego artykułu, zobacz [Automatyczne poprawianie dla programu SQL Server w Menedżerze zasobów maszyn wirtualnych Azure](virtual-machines-windows-sql-automated-patching.md).

## <a name="prerequisites"></a>Wymagania wstępne

Aby użyć automatycznego poprawiania, należy wziąć pod uwagę następujące wymagania:

**System operacyjny**:

- System Windows Server 2012
- Windows Server 2012 R2

**Wersja programu SQL Server**:

- Program SQL Server 2012
- Program SQL Server 2014 r.
- Program SQL Server 2016

**Azure programu PowerShell**:

- [Instalowanie najnowszych polecenia programu PowerShell Azure](../powershell-install-configure.md).

**Rozszerzenie IaaS serwera SQL**:

- [Zainstaluj rozszerzenie IaaS serwera SQL](virtual-machines-windows-classic-sql-server-agent-extension.md).

## <a name="settings"></a>Ustawienia

W poniższej tabeli opisano opcje, które można skonfigurować dla automatycznego poprawiania. Dla klasyczny maszyny wirtualne należy użyć programu PowerShell, aby skonfigurować te ustawienia.

|Ustawienie|Możliwe wartości|Opis|
|---|---|---|
|**Automatyczne poprawianie**|Włączanie i wyłączanie (wyłączone)|Włącza lub wyłącza automatycznego poprawiania dla Azure maszyn wirtualnych.|
|**Planowanie obsługi**|Codzienny, poniedziałek, Wtorek, Środa, czwartek, piątek, sobota, niedziela|Harmonogram pobieranie i instalowanie aktualizacji systemu Windows, SQL Server i Microsoft komputera wirtualnych.|
|**Godzina rozpoczęcia konserwacji**|0-24|Godzina rozpoczęcia lokalne zaktualizować maszyny wirtualnej.|
|**Czas trwania okna konserwacji**|30-180|Liczbę minut można ukończyć pobieranie i instalowanie aktualizacji.|
|**Kategoria poprawki**|Ważne|Kategoria aktualizacje, aby pobrać i zainstalować.|

## <a name="configuration-with-powershell"></a>Konfigurowanie za pomocą programu PowerShell

W poniższym przykładzie programu PowerShell umożliwia konfigurowanie automatycznego poprawiania na istniejące maszyn wirtualnych serwera SQL. Polecenie **Nowy AzureVMSqlServerAutoPatchingConfig** konfiguruje nowe okno konserwacji aktualizacje automatyczne.

    $aps = New-AzureVMSqlServerAutoPatchingConfig -Enable -DayOfWeek "Thursday" -MaintenanceWindowStartingHour 11 -MaintenanceWindowDuration 120  -PatchCategory "Important"

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -AutoPatchingSettings $aps | Update-AzureVM

W zależności od w tym przykładzie, w poniższej tabeli opisano praktyce miejsca docelowego maszyn wirtualnych Azure:

|Parametr|Efekt|
|---|---|
|**DayOfWeek**|Każdy czwartek zainstalowanych poprawek.|
|**MaintenanceWindowStartingHour**|Aktualizacje początkowego o 11:00.|
|**MaintenanceWindowsDuration**|Poprawki musi być zainstalowany w ciągu 120 minut. W zależności od momentu rozpoczęcia, ich muszą wykonać przez 1:00 pm.|
|**PatchCategory**|Jedynym możliwym ustawienie ten parametr jest "Ważne".|

Może zająć kilka minut, aby zainstalować i skonfigurować agenta IaaS programu SQL Server.

Aby wyłączyć automatyczne poprawianie, uruchom taki sam skrypt bez parametru - Włącz, aby AzureVMSqlServerAutoPatchingConfig nowy. Podobnie jak w przypadku instalacji może potrwać kilka minut, aby wyłączyć automatyczne poprawianie.

## <a name="next-steps"></a>Następne kroki

Aby uzyskać informacji na temat innych zadań dostępne automatyzacji zobacz [Rozszerzenie agenta programu SQL Server IaaS](virtual-machines-windows-classic-sql-server-agent-extension.md).

Aby uzyskać więcej informacji o uruchamianiu programu SQL Server na maszyny wirtualne Azure zobacz [Programu SQL Server w środowisku maszyn wirtualnych systemu Azure Przegląd](virtual-machines-windows-sql-server-iaas-overview.md).
