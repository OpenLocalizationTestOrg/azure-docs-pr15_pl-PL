<properties
    pageTitle="Instalowanie ochrona punktu końcowego Symantec na maszyny | Microsoft Azure"
    description="Dowiedz się, jak zainstalować i skonfigurować rozszerzenie zabezpieczeń Ochrona punktu końcowego Symantec w nowym lub istniejącym maszyny Azure utworzone za pomocą modelu Klasyczny wdrożenia."
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

# <a name="how-to-install-and-configure-symantec-endpoint-protection-on-a-windows-vm"></a>Jak zainstalować i skonfigurować ochrona punktu końcowego Symantec na maszyn wirtualnych systemu Windows

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

W tym artykule pokazano, jak zainstalować i skonfigurować klienta ochrona punktu końcowego Symantec na istniejące maszyny wirtualnej (maszyn wirtualnych) z systemem Windows Server. To jest pełny klienta, który zawiera usług, takich jak wirusami i ochrony przed programami szpiegującymi, zapory i zapobieganie intruzów. Klient jest zainstalowany jako rozszerzenie zabezpieczeń przy użyciu agenta maszyn wirtualnych.

Jeśli masz istniejącej subskrypcji z Symantec dla lokalnego rozwiązania, można użyć go ochrony Azure maszyn wirtualnych. Jeśli nie masz klienta jeszcze, możesz zalogować dla subskrypcji próbnej. Aby uzyskać więcej informacji na temat tego rozwiązania zobacz [Symantec ochrona punktu końcowego na platformie Azure firmy Microsoft][Symantec]. Ta strona zawiera również łącza do informacji o licencjach i instrukcje dotyczące instalowania klienta, jeśli jesteś już klientem usługi Symantec.

## <a name="install-symantec-endpoint-protection-on-an-existing-vm"></a>Instalowanie ochrona punktu końcowego Symantec na istniejące maszyn wirtualnych

Przed rozpoczęciem potrzebne następujące elementy:

- Modułu programu PowerShell usługi Azure, wersja 0.8.2 lub nowszy na komputerze pracy. Można sprawdzić wersję programu PowerShell Azure, zainstalowanym z **azure Get-Module | wersja formatu tabeli** polecenia. Aby uzyskać instrukcje i łącza do najnowszej wersji, zobacz, [jak instalowanie i konfigurowanie programu PowerShell Azure][PS]. Zaloguj się przy użyciu subskrypcji Azure `Add-AzureAccount`.

- Agent maszyn wirtualnych uruchomiony na maszyny wirtualnej Azure.

Najpierw upewnij się, że Agent maszyn wirtualnych jest już zainstalowany na komputerze wirtualnych. Wypełnij pola Nazwa usługi w chmurze i maszyn wirtualnych nazwę, a następnie uruchom następujące polecenia w wierszu polecenia programu PowerShell Azure poziomie administratora. Zamień wszystko w obrębie ofert, łącznie z < i > znaków.

> [AZURE.TIP] Jeśli nie znasz nazwy maszyn wirtualnych usługi cloud i uruchom **Get-AzureVM** , aby wyświetlić listę nazw dla wszystkich maszyn wirtualnych w bieżącej subskrypcji.

    $CSName = "<cloud service name>"
    $VMName = "<virtual machine name>"
    $vm = Get-AzureVM -ServiceName $CSName -Name $VMName
    write-host $vm.VM.ProvisionGuestAgent

Jeśli polecenie **zapisu hosta** Wyświetla **wartość PRAWDA**, Agent maszyn wirtualnych jest zainstalowany. Jeśli zostanie wyświetlony **FAŁSZ**, zobacz instrukcje i łącza do pobrania w blogu Azure opublikować [agenta maszyn wirtualnych i rozszerzenia — część 2][Agent].

Jeśli Agent maszyn wirtualnych jest zainstalowany, uruchom następujące polecenia zainstalować agenta Symantec ochrona punktu końcowego.

    $Agent = Get-AzureVMAvailableExtension -Publisher Symantec -ExtensionName SymantecEndpointProtection

    Set-AzureVMExtension -Publisher Symantec –Version $Agent.Version -ExtensionName SymantecEndpointProtection -VM $vm | Update-AzureVM

Aby sprawdzić, czy rozszerzenie zabezpieczeń Symantec została zainstalowana i aktualność:

1.  Zaloguj się do maszyny wirtualnej. Aby uzyskać instrukcje, zobacz, [jak zalogować się do maszyn wirtualnych uruchamiania systemu Windows Server][Logon].
2.  W przypadku systemu Windows Server 2008 R2, kliknij **Start > Ochrona punktu końcowego Symantec**. Dla systemu Windows Server 2012 lub Windows Server 2012 R2, na ekranie startowym wpisz **Symantec**, a następnie kliknij **Symantec ochrona punktu końcowego**.
3.  Na karcie **Stan** w oknie **Ochrona punktu końcowego Symantec stanu** zastosować aktualizacje lub uruchom ponownie w razie potrzeby.

## <a name="additional-resources"></a>Dodatkowe zasoby

[Jak zalogować się do maszyny wirtualnej z systemem Windows Server][Logon]

[Rozszerzenia Azure maszyn wirtualnych i funkcje][Ext]


<!--Link references-->
[Symantec]: http://www.symantec.com/connect/blogs/symantec-endpoint-protection-now-microsoft-azure

[Portal]: http://manage.windowsazure.com

[Create]: virtual-machines-windows-classic-tutorial.md

[PS]: ../powershell-install-configure.md

[Agent]: http://go.microsoft.com/fwlink/p/?LinkId=403947

[Logon]: virtual-machines-windows-classic-connect-logon.md

[Ext]: http://go.microsoft.com/fwlink/p/?linkid=390493