<properties
    pageTitle="Instalowanie głębokości Security firmy Trend Micro na maszyny | Microsoft Azure"
    description="W tym artykule opisano, jak zainstalować i skonfigurować Trend Micro zabezpieczeń na maszyny utworzone za pomocą modelu Klasyczny wdrażania platformy Azure."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/24/2016"
    ms.author="iainfou"/>


# <a name="how-to-install-and-configure-trend-micro-deep-security-as-a-service-on-a-windows-vm"></a>Jak zainstalować i skonfigurować Trend Micro głębokości Security jako usługa na maszyn wirtualnych systemu Windows

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

W tym artykule pokazano, jak zainstalować i skonfigurować Trend Micro głębokości Security jako usługi w nowym lub istniejącym maszyny wirtualnej (maszyn wirtualnych) z systemem Windows Server. Głębokości zabezpieczeń jako usługa zawiera ochrona przed złośliwym, Zaporę systemu zapobieganie intruzów i monitorowanie integralności.

Klient jest zainstalowany jako rozszerzenie zabezpieczeń za pośrednictwem agenta maszyn wirtualnych. Na nowym komputerze wirtualnych należy zainstalować agenta maszyn wirtualnych wraz z głębokości Agent zabezpieczeń. W istniejącej maszyny wirtualnej, która nie zawiera agenta maszyn wirtualnych musisz pobrać i zainstalować je najpierw. W tym artykule opisano, jak obu sytuacjach.

Jeśli masz istniejącej subskrypcji z Trend Micro dla lokalnego rozwiązania, można je chronić Azure maszyn wirtualnych. Jeśli nie masz klienta jeszcze, możesz zalogować dla subskrypcji próbnej. Aby uzyskać więcej informacji na temat tego rozwiązania Zobacz Trend Micro blogu [Zabezpieczeń firmy Microsoft Azure maszyn wirtualnych agenta rozszerzenia dla głębokości](http://go.microsoft.com/fwlink/p/?LinkId=403945).

## <a name="install-the-deep-security-agent-on-a-new-vm"></a>Instalowanie głębokości agentem zabezpieczeń na nowy maszyn wirtualnych

[Portal Azure klasyczny](http://manage.windowsazure.com) pozwala zainstalować agenta maszyn wirtualnych i Trend Micro rozszerzenie zabezpieczeń podczas tworzenia maszyny wirtualnej za pomocą opcji **Z galerii** . Jeśli tworzysz jednym komputerze wirtualnych za pomocą portalu jest łatwym sposobem zabezpieczanie z Trend Micro.

Ta opcja **Z galerii** zostanie otwarty Kreator, który pomoże Ci skonfigurować maszyny wirtualnej. Zainstaluj rozszerzenie zabezpieczeń Trend Micro i Agent maszyn wirtualnych za pomocą ostatniej stronie kreatora. Aby uzyskać ogólne instrukcje zobacz [Tworzenie wirtualnych komputera z systemem Windows w portalu klasyczny Azure](virtual-machines-windows-classic-tutorial.md). Po przejściu do ostatniej stronie kreatora, wykonaj następujące czynności:

1.  W obszarze **Agenta maszyn wirtualnych**zaznacz **Zainstalować agenta maszyn wirtualnych**.

2.  W obszarze **Rozszerzenia zabezpieczeń**zaznacz **Trend Micro głębokości Agent zabezpieczeń**.

    ![Instalowanie agenta maszyn wirtualnych i agenta głębokości zabezpieczeń](./media/virtual-machines-windows-classic-install-trend/InstallVMAgentandTrend.png)

3.  Kliknij znacznik wyboru, aby utworzyć maszyny wirtualnej.

## <a name="install-the-deep-security-agent-on-an-existing-vm"></a>Instalowanie głębokości agentem zabezpieczeń na istniejące maszyn wirtualnych

Aby zainstalować agenta na istniejące maszyn wirtualnych, są potrzebne następujące elementy:

- Modułu programu PowerShell usługi Azure w wersji 0.8.2 lub nowszej, zainstalowany na komputerze lokalnym. Można sprawdzić wersję programu PowerShell Azure, zainstalowanym przy użyciu **azure Get-Module | wersja formatu tabeli** polecenia. Aby uzyskać instrukcje i łącza do najnowszej wersji Zobacz, [jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md). Zaloguj się przy użyciu subskrypcji Azure `Add-AzureAccount`.

- Agent maszyn wirtualnych, zainstalowany na komputerze wirtualnych docelowej.

Najpierw upewnij się, że Agent maszyn wirtualnych jest już zainstalowany. Wypełnij pola Nazwa usługi w chmurze i maszyn wirtualnych nazwę, a następnie uruchom następujące polecenia w wierszu polecenia programu PowerShell Azure poziomie administratora. Zamień wszystko w obrębie ofert, łącznie z < i > znaków.

    $CSName = "<cloud service name>"
    $VMName = "<virtual machine name>"
    $vm = Get-AzureVM -ServiceName $CSName -Name $VMName
    write-host $vm.VM.ProvisionGuestAgent

Jeśli nie wiesz, usługa w chmurze i nazwę maszyn wirtualnych, uruchom **Get-AzureVM** , aby wyświetlić te informacje dla wszystkich maszyn wirtualnych w bieżącej subskrypcji.

Jeśli polecenie **zapisu hosta** zwraca **wartość PRAWDA**, Agent maszyn wirtualnych jest zainstalowany. Jeśli zwraca **wartość False**, zobacz instrukcje i łącza do pobrania w blogu Azure opublikować [agenta maszyn wirtualnych i rozszerzenia — część 2](http://go.microsoft.com/fwlink/p/?LinkId=403947).

Jeśli Agent maszyn wirtualnych jest zainstalowany, uruchom następujące polecenia.

    $Agent = Get-AzureVMAvailableExtension TrendMicro.DeepSecurity -ExtensionName TrendMicroDSA

    Set-AzureVMExtension -Publisher TrendMicro.DeepSecurity –Version $Agent.Version -ExtensionName TrendMicroDSA -VM $vm | Update-AzureVM

## <a name="next-steps"></a>Następne kroki

Wystarczy kilka minut agent uruchomienia podczas instalacji. Po wykonaniu tej należy uaktywnienie głębokości zabezpieczeń na komputerze wirtualnych w celu mogą być zarządzane przez głębokości Menedżera zabezpieczeń. Zobacz poniższe czynności, aby uzyskać dodatkowe instrukcje:

- Artykuł trendu na temat tego rozwiązania [Instant-On chmury zabezpieczenia usługi Microsoft Azure](http://go.microsoft.com/fwlink/?LinkId=404101)
- [Przykładowy skrypt programu Windows PowerShell](http://go.microsoft.com/fwlink/?LinkId=404100) w celu skonfigurowania maszyny wirtualnej
- [Instrukcje](http://go.microsoft.com/fwlink/?LinkId=404099) dotyczące próbki

## <a name="additional-resources"></a>Dodatkowe zasoby

[Jak zalogować się do maszyny wirtualnej z systemem Windows Server]

[Azure rozszerzenia maszyn wirtualnych i funkcje]


<!--Link references-->
[Jak zalogować się do maszyny wirtualnej z systemem Windows Server]: virtual-machines-windows-classic-connect-logon.md
[Azure rozszerzenia maszyn wirtualnych i funkcje]: http://go.microsoft.com/fwlink/p/?linkid=390493&clcid=0x409
