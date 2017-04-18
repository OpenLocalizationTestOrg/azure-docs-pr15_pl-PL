<properties
    pageTitle="Automatyczne poprawianie dla maszyny wirtualne programu SQL Server (Menedżer zasobów) | Microsoft Azure"
    description="W tym miejscu wyjaśniono funkcji automatycznego poprawiania dla programu SQL Server maszyn wirtualnych z systemem platformy Azure za pomocą Menedżera zasobów."
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
    ms.date="08/19/2016"
    ms.author="jroth" />

# <a name="automated-patching-for-sql-server-in-azure-virtual-machines-resource-manager"></a>Automatyczne poprawianie dla programu SQL Server w środowisku maszyn wirtualnych Azure (Menedżer zasobów)

> [AZURE.SELECTOR]
- [Menedżer zasobów](virtual-machines-windows-sql-automated-patching.md)
- [Klasyczny](virtual-machines-windows-classic-sql-automated-patching.md)

Automatyczne Patching określa oknem konserwacji dla Azure maszyny wirtualnej uruchomiony program SQL Server. Aktualizacje automatyczne można zainstalować tylko w tym oknie konserwacji. Dla programu SQL Server ten rescriction gwarantuje, że aktualizacje systemu i wszystkie skojarzone uruchomieniu występują w najlepszej godziny dla bazy danych. Automatyczne Patching zależy od [Rozszerzenia agenta programu SQL Server IaaS](virtual-machines-windows-sql-server-agent-extension.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]model klasyczny wdrożenia. Aby wyświetlić w klasycznej wersji programu w tym artykule, zobacz [Automatyczne poprawianie dla programu SQL Server w klasycznym maszyn wirtualnych Azure](virtual-machines-windows-classic-sql-automated-patching.md).

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

- [Instalowanie najnowszych polecenia programu PowerShell Azure](../powershell-install-configure.md) , jeśli zamierzasz skonfigurować automatyczne poprawianie przy użyciu programu PowerShell.

>[AZURE.NOTE] Automatyczne Patching zależy od rozszerzenia agenta programu SQL Server IaaS. Bieżący zdjęcia z galerii maszyn wirtualnych SQL dodać to rozszerzenie domyślnie. Aby uzyskać więcej informacji zobacz [Rozszerzenie agenta programu SQL Server IaaS](virtual-machines-windows-sql-server-agent-extension.md).

## <a name="settings"></a>Ustawienia

W poniższej tabeli opisano opcje, które można skonfigurować dla automatycznego poprawiania. Procedura rzeczywista konfiguracja się różnić w zależności od tego, czy korzystasz z Azure portal lub polecenia programu PowerShell systemu Windows Azure.

|Ustawienie|Możliwe wartości|Opis|
|---|---|---|
|**Automatyczne poprawianie**|Włączanie i wyłączanie (wyłączone)|Włącza lub wyłącza automatycznego poprawiania dla Azure maszyn wirtualnych.|
|**Planowanie obsługi**|Codzienny, poniedziałek, Wtorek, Środa, czwartek, piątek, sobota, niedziela|Harmonogram pobieranie i instalowanie aktualizacji systemu Windows, SQL Server i Microsoft komputera wirtualnych.|
|**Godzina rozpoczęcia konserwacji**|0-24|Godzina rozpoczęcia lokalne zaktualizować maszyny wirtualnej.|
|**Czas trwania okna konserwacji**|30-180|Liczbę minut można ukończyć pobieranie i instalowanie aktualizacji.|
|**Kategoria poprawki**|Ważne|Kategoria aktualizacje, aby pobrać i zainstalować.|

## <a name="configuration-in-the-portal"></a>Konfiguracja w portalu
Azure portal umożliwia konfigurowanie automatycznego poprawiania podczas inicjowania obsługi administracyjnej lub istniejącej maszyny wirtualne.

### <a name="new-vms"></a>Nowe maszyny wirtualne
Azure portal umożliwia skonfigurowanie automatycznego poprawiania, podczas tworzenia nowej maszyny wirtualnej SQL Server w modelu wdrożenia Menedżera zasobów.

W karta **Ustawienia programu SQL Server** wybierz opcję **automatycznego poprawiania**. Następujące Azure portalu zrzucie ekranu pokazano karta **Automatycznego poprawiania SQL** .

![SQL automatycznego poprawiania w Azure portal](./media/virtual-machines-windows-sql-automated-patching/azure-sql-arm-patching.png)

Dla kontekstu zobacz temat wykonane na [inicjowania obsługi administracyjnej maszyny wirtualnej programu SQL Server Azure](virtual-machines-windows-portal-sql-server-provision.md).

### <a name="existing-vms"></a>Istniejące maszyny wirtualne
Dla istniejących maszyn wirtualnych programu SQL Server wybierz ten komputer wirtualnych programu SQL Server. Zaznacz sekcję karta **Ustawienia** **konfiguracji programu SQL Server** .

![SQL automatyczne poprawianie dla istniejących maszyny wirtualne](./media/virtual-machines-windows-sql-automated-patching/azure-sql-rm-patching-existing-vms.png)

W karta **konfiguracji programu SQL Server** kliknij przycisk **Edytuj** w automatycznego poprawiania sekcji.

![Konfigurowanie automatycznego poprawiania SQL dla istniejących maszyny wirtualne](./media/virtual-machines-windows-sql-automated-patching/azure-sql-rm-patching-configuration.png)

Po zakończeniu kliknij przycisk **OK** w dolnej części karta **konfiguracji programu SQL Server** , aby zapisać zmiany.

Jeśli po raz pierwszy, jest włączana automatycznego poprawiania, Azure konfiguruje agenta IaaS programu SQL Server w tle. W tym czasie Azure portal może nie pokazać, że skonfigurowano automatycznego poprawiania. Poczekaj kilka minut agent jest zainstalowany, zostanie skonfigurowane. Po wykonaniu tej Azure portal odzwierciedla nowe ustawienia.

>[AZURE.NOTE] Można również skonfigurować automatyczne poprawianie przy użyciu szablonu. Aby uzyskać więcej informacji zobacz temat [szablonu Azure Szybki Start dla automatycznego poprawiania](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-autopatching-update).

## <a name="configuration-with-powershell"></a>Konfigurowanie za pomocą programu PowerShell

Po zainicjowaniu obsługi administracyjnej maszyn wirtualnych usługi SQL, Konfigurowanie automatycznego poprawiania przy użyciu programu PowerShell.

W poniższym przykładzie programu PowerShell umożliwia konfigurowanie automatycznego poprawiania na istniejące maszyn wirtualnych serwera SQL. Polecenie **AzureRM.Compute\New AzureVMSqlServerAutoPatchingConfig** konfiguruje nowe okno konserwacji aktualizacje automatyczne.

    $vmname = "vmname"
    $resourcegroupname = "resourcegroupname"
    $aps = AzureRM.Compute\New-AzureVMSqlServerAutoPatchingConfig -Enable -DayOfWeek "Thursday" -MaintenanceWindowStartingHour 11 -MaintenanceWindowDuration 120  -PatchCategory "Important"

    Set-AzureRmVMSqlServerExtension -AutoPatchingSettings $aps -VMName $vmname -ResourceGroupName $resourcegroupname

W zależności od w tym przykładzie, w poniższej tabeli opisano praktyce miejsca docelowego maszyn wirtualnych Azure:

|Parametr|Efekt|
|---|---|
|**DayOfWeek**|Każdy czwartek zainstalowanych poprawek.|
|**MaintenanceWindowStartingHour**|Aktualizacje początkowego o 11:00.|
|**MaintenanceWindowsDuration**|Poprawki musi być zainstalowany w ciągu 120 minut. W zależności od momentu rozpoczęcia, ich muszą wykonać przez 1:00 pm.|
|**PatchCategory**|Jedynym możliwym ustawienie ten parametr jest **Ważne**.|

Może zająć kilka minut, aby zainstalować i skonfigurować agenta IaaS programu SQL Server.

Aby wyłączyć automatyczne poprawianie, uruchom sam skrypt bez **— Włączanie** parametr **AzureRM.Compute\New AzureVMSqlServerAutoPatchingConfig**. Brak **— Włączanie** parametru sygnały polecenie, aby wyłączyć tę funkcję.

## <a name="next-steps"></a>Następne kroki

Aby uzyskać informacji na temat innych zadań dostępne automatyzacji zobacz [Rozszerzenie agenta programu SQL Server IaaS](virtual-machines-windows-sql-server-agent-extension.md).

Aby uzyskać więcej informacji o uruchamianiu programu SQL Server na maszyny wirtualne Azure zobacz [Programu SQL Server w środowisku maszyn wirtualnych systemu Azure Przegląd](virtual-machines-windows-sql-server-iaas-overview.md).
