<properties
    pageTitle="Usuwanie serwerów i wyłączyć ochronę przed | Microsoft Azure" 
    description="W tym artykule opisano, jak Wyrejestrowywanie serwerów z magazynu Odzyskiwanie witryny, a także wyłączyć ochronę przed dla maszyn wirtualnych i serwerów fizycznych." 
    services="site-recovery" 
    documentationCenter="" 
    authors="rayne-wiselman" 
    manager="jwhit" 
    editor=""/>

<tags 
    ms.service="site-recovery" 
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery" 
    ms.date="10/05/2016" 
    ms.author="raynew"/>

# <a name="remove-servers-and-disable-protection"></a>Usuwanie serwerów i wyłączyć ochronę przed

Usługa Azure witryny odzyskiwania składa się na strategii odzyskiwania (BCDR) ciągłości i danych biznesowych przez orchestrating replikacji, pracy awaryjnej i odzyskiwanie maszyn wirtualnych i serwerów fizycznych. Urządzenia mogą być replikowane Azure lub centrum danych lokalnych pomocniczą. Krótki przegląd odczytywanie [Co to jest Odzyskiwanie witryny Azure?](site-recovery-overview.md)

## <a name="overview"></a>Omówienie

W tym artykule opisano, jak Wyrejestrowywanie serwerów z magazynu Odzyskiwanie witryny i jak wyłączyć ochronę przed dla maszyn wirtualnych chroniony przez Odzyskiwanie witryny. 

Publikowanie jakieś komentarze lub pytania u dołu tego artykułu lub [Azure odzyskiwania usług Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)w witrynie.

## <a name="unregister-a-vmm-server"></a>Unregister serwer VMM

Serwer VMM z magazynu jest unregister przez usunięcie serwera na karcie **Serwery** w portalu Odzyskiwanie witryny Azure. Należy zauważyć, że:

-  **Serwer połączony VMM**: zalecamy unregister serwera VMM, gdy jest podłączone do Azure. Dzięki temu, że ustawienia na serwerze VMM w wersji lokalnej i serwery VMM skojarzone z nim (serwery VMM, które zawierają chmury, które są mapowane na chmury na serwerze, który chcesz usunąć) są prawidłowo czyszczone. Zaleca się, że możesz usunąć tylko serwer odłączony w przypadku trwały problemu z połączeniem.
- **Niepołączone VMM serwera**: Jeśli serwer VMM nie jest połączony po jego usunięciu będzie potrzebne do uruchomienia skryptu ręcznie, aby wykonać oczyszczanie. Skrypt jest dostępny w [galerii firmy Microsoft](http://aka.ms/asr-cleanup-script-vmm). Uwaga identyfikator VMM serwera, aby ukończyć proces Oczyszczanie ręczne.
- **Serwer VMM w klastrze**: Jeśli musisz unregister serwera VMM, który został wdrożony w klastrze wykonaj następujące czynności:

    - Jeśli serwer jest połączony, Usuń połączonego serwera VMM na karcie **serwer** . Aby odinstalować dostawcy na serwerze, zaloguj się na każdym węźle klaster i ich odinstalować z poziomu Panelu sterowania. Uruchom skrypt Oczyszczanie wymienianych w poprzedniej sekcji we wszystkich węzłach pasywne w grupie, aby usunąć wpisy rejestru.
    - Jeśli serwer nie jest połączony musisz uruchomić skrypt oczyszczania na wszystkich węzłach.

### <a name="unregister-an-unconnected-vmm-server"></a>Unregister odłączony serwera VMM

Na serwerze VMM chcesz usunąć:

1. Unregister serwera VMM Azure portal.
2. Na serwerze VMM Pobierz skrypt oczyszczanie.
3. Otwieranie programu PowerShell z Uruchom jako Administrator opcję, aby zmiana zasad wykonanie zakres domyślny (LocalMachine).
4. Postępuj zgodnie z instrukcjami skrypt. 

Na serwerach VMM, które mają chmury, które są skojarzone z chmury na serwerze, który chcesz usunąć:

1. Uruchamianie skryptu oczyszczanie i wykonaj kroki od 2 do 4.
2. Określ identyfikator VMM dla serwera VMM, który został wyrejestrowany. 
3. Ten skrypt spowoduje usunięcie informacji o rejestracji dla serwera VMM i chmury sparowania informacji.


## <a name="unregister-a-hyper-v-server-in-a-hyper-v-site"></a>Unregister Hyper-V server w witrynie funkcji Hyper-V

Po wdrożeniu Odzyskiwanie witryny Azure ochrony maszyn wirtualnych na serwerze funkcji Hyper-V w witrynie funkcji Hyper-V (z serwerem nie VMM) możesz unregister Hyper-V server z magazynu w następujący sposób:

1. Wyłączanie ochrony dla maszyn wirtualnych znajdujących się na serwerze funkcji Hyper-V.
2. Na karcie **Serwery** portal Azure Odzyskiwanie witryny, wybierz serwer > Usuń. Serwer nie musi być połączony z Azure, gdy to zrobisz.
3. Uruchom następujący skrypt, aby oczyścić ustawienia na serwerze i wyrejestrowywanie go z magazynu. 

        pushd .
        try
        {
             $windowsIdentity=[System.Security.Principal.WindowsIdentity]::GetCurrent()
             $principal=new-object System.Security.Principal.WindowsPrincipal($windowsIdentity)
             $administrators=[System.Security.Principal.WindowsBuiltInRole]::Administrator
             $isAdmin=$principal.IsInRole($administrators)
             if (!$isAdmin)
             {
                "Please run the script as an administrator in elevated mode."
                $choice = Read-Host
                return;       
             }
    
            $error.Clear()    
            "This script will remove the old Azure Site Recovery Provider related properties. Do you want to continue (Y/N) ?"
            $choice =  Read-Host
        
            if (!($choice -eq 'Y' -or $choice -eq 'y'))
            {
            "Stopping cleanup."
            return;
            }
        
            $serviceName = "dra"
            $service = Get-Service -Name $serviceName
            if ($service.Status -eq "Running")
            {
                "Stopping the Azure Site Recovery service..."
                net stop $serviceName
            }
        
            $asrHivePath = "HKLM:\SOFTWARE\Microsoft\Azure Site Recovery"
            $registrationPath = $asrHivePath + '\Registration'
            $proxySettingsPath = $asrHivePath + '\ProxySettings'
            $draIdvalue = 'DraID'
        
            if (Test-Path $asrHivePath)
            {
                if (Test-Path $registrationPath)
                {
                    "Removing registration related registry keys."  
                    Remove-Item -Recurse -Path $registrationPath
                }
    
                if (Test-Path $proxySettingsPath)
            {
                    "Removing proxy settings"
                    Remove-Item -Recurse -Path $proxySettingsPath
                }
    
                $regNode = Get-ItemProperty -Path $asrHivePath
                if($regNode.DraID -ne $null)
                {            
                    "Removing DraId"
                    Remove-ItemProperty -Path $asrHivePath -Name $draIdValue
                }
                "Registry keys removed."
            }
    
            # First retrive all the certificates to be deleted
            $ASRcerts = Get-ChildItem -Path cert:\localmachine\my | where-object {$_.friendlyname.startswith('ASR_SRSAUTH_CERT_KEY_CONTAINER') -or $_.friendlyname.startswith('ASR_HYPER_V_HOST_CERT_KEY_CONTAINER')}
            # Open a cert store object
            $store = New-Object System.Security.Cryptography.X509Certificates.X509Store("My","LocalMachine")
            $store.Open('ReadWrite')
            # Delete the certs
            "Removing all related certificates"
            foreach ($cert in $ASRcerts)
            {
                $store.Remove($cert)
            }
        }catch
        {   
            [system.exception]
            Write-Host "Error occured" -ForegroundColor "Red"
            $error[0] 
            Write-Host "FAILED" -ForegroundColor "Red"
        }
        popd


## <a name="stop-protecting-a-hyper-v-virtual-machine"></a>Zatrzymaj ochronę maszyny wirtualnej funkcji Hyper-V

Jeśli chcesz zatrzymać ochrona maszyny wirtualnej funkcji Hyper-V musisz usunąć ochronę. W zależności od sposobu usunąć ochronę może być konieczne Oczyść ustawień ochrony ręcznie na komputerze. 

### <a name="remove-protection"></a>Usuwanie ochrony

1. Na karcie **maszyn wirtualnych** właściwości chmury wybierz maszyny wirtualnej > **Usuń**.
2. Na stronie **Potwierdzanie usunięcia maszyn wirtualnych** masz kilka opcji:

    - **Wyłączanie ochrony**— Jeśli włączysz i Zapisz tę opcję maszyny wirtualnej już nie będą one chronione za odzyskiwanie witryny. Ustawienia ochrony dla maszyny wirtualnej będzie można automatycznie czyszczone.
    - **Usuń z magazynu**— po wybraniu tej opcji maszyny wirtualnej zostanie usunięty tylko z magazynu Odzyskiwanie witryny. Ustawienia ochrony lokalnego maszyny wirtualnej nie ulegnie zmianie. Musisz go oczyścić ręcznie ustawienia, aby usunąć ustawienia ochrony i Usuń maszyny wirtualnej z Azure subskrypcji i usuwanie ustawień ochrony, musisz go oczyścić je ręcznie, wykonując poniższe instrukcje.

Jeśli wybierzesz opcję Usuń maszyny wirtualnej i jego dyskach twardych, które zostaną usunięte z lokalizacji docelowej.

### <a name="clean-up-protection-settings-manually-between-vmm-sites"></a>Czyszczenie ustawień ochrony ręcznie (między witrynami VMM)

Jeśli wybrano opcję **Zatrzymaj zarządzanie maszyny wirtualnej**, ręcznie wyczyścić ustawienia:

1. Na serwerze podstawowy, uruchomienie tego skryptu z konsoli VMM oczyszczania ustawień podstawowego maszyny wirtualnej. W konsoli VMM kliknij przycisk programu PowerShell, aby otworzyć konsoli programu PowerShell VMM. Zamień SQLVM1 nazwę komputera wirtualnych.

         $vm = get-scvirtualmachine -Name "SQLVM1"
         Set-SCVirtualMachine -VM $vm -ClearDRProtection

2. Na serwerze pomocniczym VMM Uruchom ten skrypt, aby oczyścić ustawień pomocniczej maszyny wirtualnej:

        $vm = get-scvirtualmachine -Name "SQLVM1"
        Remove-SCVirtualMachine -VM $vm -Force

3. Na serwerze pomocniczym VMM po uruchomieniu skrypt odświeżanie maszyn wirtualnych na serwerze hosta funkcji Hyper-V, aby pomocniczej maszyny wirtualnej otrzymuje ponownie wykryte w konsoli VMM.
4. Powyższe czynności wyczyści replikacji ustawienia tylko VMM serwera. Jeśli chcesz usunąć replikacji maszyn wirtualnych dla maszyny wirtualnej. Musisz wykonać poniższe instrukcje dotyczące obu podstawowy i pomocniczy maszyn wirtualnych. Uruchamianie poniżej skrypt, aby usunąć replikacji i zamienić SQLVM1 nazwę komputera wirtualnych.

        Remove-VMReplication –VMName “SQLVM1”


### <a name="clean-up-protection-settings-manually-between-on-premises-vmm-sites-and-azure"></a>Czyszczenie ustawień ochrony ręcznie (między witrynami VMM lokalnego i Azure)

1. Na serwerze źródłowym VMM Uruchom ten skrypt, aby oczyścić ustawień podstawowego maszyny wirtualnej:

        $vm = get-scvirtualmachine -Name "SQLVM1"
        Set-SCVirtualMachine -VM $vm -ClearDRProtection

2. Powyższe czynności wyczyści replikacji ustawienia tylko VMM serwera. Po usunięciu replikacji z serwera VMM upewnij się, aby usunąć replikacji maszyny wirtualnej uruchomionych na serwerze hosta funkcji Hyper-V z tego skryptu. Zamień SQLVM1 nazwy maszyn wirtualnych i host01.contoso.com o nazwie serwera hosta funkcji Hyper-V.

        $vmName = "SQLVM1"
        $hostName  = "host01.contoso.com"
        $vm = Get-WmiObject -Namespace "root\virtualization\v2" -Query "Select * From Msvm_ComputerSystem Where ElementName = '$vmName'" -computername $hostName
        $replicationService = Get-WmiObject -Namespace "root\virtualization\v2"  -Query "Select * From Msvm_ReplicationService"  -computername $hostName
        $replicationService.RemoveReplicationRelationship($vm.__PATH)

### <a name="clean-up-protection-settings-manually-between-hyper-v-sites-and-azure"></a>Czyszczenie ustawień ochrony ręcznie (między witrynami funkcji Hyper-V i Azure)

1. Na serwerze hosta funkcji Hyper-V źródła Aby usunąć replikacji maszyny wirtualnej Użyj tego skryptu. Zamień SQLVM1 nazwę komputera wirtualnych.

        $vmName = "SQLVM1"
        $vm = Get-WmiObject -Namespace "root\virtualization\v2" -Query "Select * From Msvm_ComputerSystem Where ElementName = '$vmName'"
        $replicationService = Get-WmiObject -Namespace "root\virtualization\v2"  -Query "Select * From Msvm_ReplicationService"
        $replicationService.RemoveReplicationRelationship($vm.__PATH)

## <a name="stop-protecting-a-vmware-virtual-machine-or-a-physical-server"></a>Zatrzymywanie ochrony maszyny wirtualnej VMware lub fizyczny serwer

Jeśli chcesz zatrzymać ochrony maszyny wirtualnej VMware lub serwera fizycznego musisz usunąć ochronę. W zależności od sposobu usunąć ochronę może być konieczne Oczyść ustawień ochrony ręcznie na komputerze. 

### <a name="remove-protection"></a>Usuwanie ochrony

1. Na karcie **maszyn wirtualnych** właściwości chmury wybierz maszyny wirtualnej > **Usuń**.
2. Na **Usuwanie maszyn wirtualnych** wybierz jedną z opcji:

    - **Wyłączanie ochrony (do użycia w przypadku zmiany rozmiaru rozwijania i wielkość odzyskiwania)**— będzie tylko Zobacz i włączyć tę opcję, jeśli masz:
        - **Resized wielkość maszyn wirtualnych**— po zmianie rozmiaru woluminu maszyny wirtualnej przechodzi do stanu krytyczne. W takim przypadku wybierz tę opcję. Wyłączany ochrony zachowując punktów odzyskiwania w Azure. Po ponownym włączeniu ochrony dla komputera danych po zmianie rozmiaru woluminu zostanie przeniesiona do Azure.
        - **Uruchom w trybie awaryjnym**— po przetestowaniu środowiska, uruchamiając trybie awaryjnym z VMware w lokalnym środowisku maszyn wirtualnych systemu lub serwerów fizycznych Azure, wybierz tę opcję, aby uruchomić ponownie ochrona maszyn wirtualnych lokalnego. Ta opcja powoduje wyłączenie każdej maszyny wirtualnej, a następnie musisz ponownie włączyć ochronę je. Należy zauważyć, że:
            - Wyłączanie maszyny wirtualnej to ustawienie nie wpływa na maszyny wirtualnej replice Azure.
            - Nie odinstalować usługę mobilności z maszyny wirtualnej.
    
    - **Wyłączanie ochrony**— Jeśli włączysz i Zapisz tę opcję, komputer nie jest już będą one chronione za odzyskiwanie witryny. Ustawienia ochrony dla komputera będzie można automatycznie czyszczone.
    - **Usuń z magazynu**— po wybraniu tej opcji komputer zostanie usunięty tylko z magazynu Odzyskiwanie witryny. Ustawienia ochrony lokalnego na komputerze nie ulegnie zmianie. Aby usunąć ustawienia na komputerze i usunąć maszyny wirtualnej z platformy Azure subskrypcji, a najpierw Oczyść ustawienia przez odinstalowanie usługi mobilności.
    
        ![Usuwanie opcji](./media/site-recovery-manage-registration-and-protection/remove-vm.png)

















