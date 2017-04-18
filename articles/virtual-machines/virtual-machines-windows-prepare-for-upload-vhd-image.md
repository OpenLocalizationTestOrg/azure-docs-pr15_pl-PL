<properties
    pageTitle="Przygotowywanie wirtualnego dysku twardego systemu Windows do przekazania Azure | Microsoft Azure"
    description="Najważniejsze wskazówki dotyczące przygotowywania wirtualnego dysku twardego systemu Windows przed przekazaniem Azure"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="genlin"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/18/2016"
    ms.author="glimoli;genli"/>

# <a name="prepare-a-windows-vhd-to-upload-to-azure"></a>Przygotowywanie wirtualnego dysku twardego systemu Windows do przekazania Azure
Aby przekazać maszyny systemu Windows Azure ze źródeł lokalnych, należy przygotować poprawnie wirtualnego dysku twardego (VHD). Istnieje kilka zalecane czynności można wykonać przed przekazaniem wirtualnego dysku twardego Azure. W tym artykule pokazano, jak przygotować wirtualnego dysku twardego systemu Windows do przekazania do Microsoft Azure, a ponadto również wyjaśniono, [Kiedy i jak używać Sysprep](#step23).

## <a name="prepare-the-virtual-disk"></a>Przygotowywanie dysk wirtualny

>[AZURE.NOTE] 
> Azure obsługuje tylko [maszyn wirtualnych Generowanie 1](http://blogs.technet.com/b/ausoemteam/archive/2015/04/21/deciding-when-to-use-generation-1-or-generation-2-virtual-machines-with-hyper-v.aspx) w formacie pliku wirtualnego dysku twardego. Nowszy format VHDX nie jest obsługiwana w Azure. 
>
> Wirtualny dysk twardy musi być stały rozmiar nie dynamiczne. W razie potrzeby poniższe instrukcje szczegółowo konwertowania VHDX lub dynamiczne dysków. Maksymalny rozmiar dozwolony dla wirtualny dysk twardy jest 1,023 GB.


Upewnij się, że Windows wirtualny dysk twardy działa poprawnie na serwerze lokalnym. Usuwanie dowolnego błędów w maszyn wirtualnych się przed próbą można konwertować przekazywanie Azure.

Jeśli potrzebujesz przekonwertować dysku wirtualnego wymagany format Azure, użyj jednej z metod ułatwień wymienionych w poniższych sekcjach. Wykonywanie kopii zapasowej maszyn wirtualnych przed uruchomieniem procesu konwersji dysku wirtualnego ani Sysprep.

### <a name="convert-using-hyper-v-manager"></a>Konwertowanie za pomocą Menedżera funkcji Hyper-V
- Otwórz Menedżera funkcji Hyper-V i wybierz komputera lokalnego po lewej stronie. W menu nad nim, kliknij opcję **Akcja** > **Edytowanie dysku**.
    - Na ekranie **Znajdź wirtualnego dysku twardego** przejdź do, a następnie wybierz dysk wirtualny.
    - Na następnym ekranie wybierz pozycję **Konwertuj**
        - Jeśli chcesz przekonwertować z VHDX, zaznacz **wirtualnego dysku twardego** i kliknij przycisk **Dalej**
        - Jeśli trzeba przekonwertować z dysku dynamicznego, wybierz **rozmiar stały** i kliknij przycisk **Dalej**

    - Przejdź do, a następnie wybierz **ścieżkę do nowego pliku wirtualnego dysku twardego**.
    - Kliknij przycisk **Zakończ** , aby zamknąć.

### <a name="convert-using-powershell"></a>Konwertowanie przy użyciu programu PowerShell
Możesz przekonwertować dysku wirtualnego przy użyciu [polecenia cmdlet programu PowerShell wirtualnego dysku twardego Konwertuj](http://technet.microsoft.com/library/hh848454.aspx). W poniższym przykładzie firma Microsoft są konwertowania VHDX na wirtualnego dysku twardego i konwertowania dynamiczny na stałe typ:

```powershell
Convert-VHD –Path c:\test\MY-VM.vhdx –DestinationPath c:\test\MY-NEW-VM.vhd -VHDType Fixed
```

### <a name="convert-from-vmware-vmdk-disk-format"></a>Konwersja z formatu dysku VMware VMDK
Jeśli masz obraz maszyn wirtualnych systemu Windows w [formacie pliku VMDK](https://en.wikipedia.org/wiki/VMDK)przekonwertować go wirtualnego dysku twardego za pomocą [Konwertera maszyn wirtualnych firmy Microsoft](https://www.microsoft.com/download/details.aspx?id=42497). Czytanie blogu [jak przekonwertować VMDK VMware do wirtualnego dysku twardego funkcji Hyper-V](http://blogs.msdn.com/b/timomta/archive/2015/06/11/how-to-convert-a-vmware-vmdk-to-hyper-v-vhd.aspx) , aby uzyskać więcej informacji.

## <a name="prepare-windows-configuration-for-upload"></a>Przygotowywanie konfiguracji systemu Windows do przekazania

> [AZURE.NOTE] Uruchom następujące polecenia z [uprawnieniami administratora](https://technet.microsoft.com/library/cc947813.aspx).

1. Usuń statyczne trwałą trasę w tabeli routingu:

    - Aby wyświetlić tabelę routingu, uruchom `route print`.
    - Zaznacz sekcje **Trasy trwałe** . W przypadku trwałą trasę umożliwia [Usuwanie rozsyłania](https://technet.microsoft.com/library/cc739598.apx) go usunąć.

2. Usuwanie serwera proxy WinHTTP:

    ```
    netsh winhttp reset proxy
    ```

3. Skonfiguruj zasady SAN [Onlineall](https://technet.microsoft.com/library/gg252636.aspx)dysku:

    ```
    diskpart san policy=onlineall
    ```

4. Używanie czas uniwersalny czas koordynowany (UTC) dla systemu Windows i Ustaw typ uruchamiania usługi Czas systemu Windows (w32time) do **automatycznie**:

    ```
    REG ADD HKLM\SYSTEM\CurrentControlSet\Control\TimeZoneInformation /v RealTimeIsUniversal /t REG_DWORD /d 1
    sc config w32time start= auto
    ```


## <a name="configure-windows-services"></a>Konfigurowanie usług systemu Windows
5. Upewnij się, każda z poniższych usług Windows jest ustawiona na **wartości domyślne systemu Windows**. Są one skojarzone z ustawienia uruchamiania ułatwień wymienionych na poniższej liście. Te polecenia, aby zresetować ustawienia uruchamiania można wykonać:

    ```
    sc config bfe start= auto

    sc config dcomlaunch start= auto

    sc config dhcp start= auto

    sc config dnscache start= auto

    sc config IKEEXT start= auto

    sc config iphlpsvc start= auto

    sc config PolicyAgent start= demand

    sc config LSM start= auto

    sc config netlogon start= demand

    sc config netman start= demand

    sc config NcaSvc start= demand

    sc config netprofm start= demand

    sc config NlaSvc start= auto

    sc config nsi start= auto

    sc config RpcSs start= auto

    sc config RpcEptMapper start= auto

    sc config termService start= demand

    sc config MpsSvc start= auto

    sc config WinHttpAutoProxySvc start= demand

    sc config LanmanWorkstation start= auto

    sc config RemoteRegistry start= auto
    ```


## <a name="configure-remote-desktop-configuration"></a>Konfigurację pulpitu zdalnego
6. W przypadku dowolnej certyfikatów z podpisem własnym powiązanych z odbiornika protokołu RDP (Remote Desktop), należy je usunąć:

    ```
    REG DELETE "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp\SSLCertificateSHA1Hash”
    ```

    Aby uzyskać więcej informacji o konfigurowaniu certyfikaty dla odbiornika RDP zobacz [Konfiguracji certyfikatu odbiornika w systemie Windows Server](https://blogs.technet.microsoft.com/askperf/2014/05/28/listener-certificate-configurations-in-windows-server-2012-2012-r2/)

7. Konfigurowanie wartości [utrzymywania aktywności](https://technet.microsoft.com/library/cc957549.aspx) RDP usługi:

    ```
    REG ADD "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v KeepAliveEnable /t REG_DWORD  /d 1 /f

    REG ADD "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v KeepAliveInterval /t REG_DWORD  /d 1 /f

    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp" /v KeepAliveTimeout /t REG_DWORD /d 1 /f
    ```

8. Konfigurowanie trybu uwierzytelniania usługi RDP:

    ```
    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v UserAuthentication /t REG_DWORD  /d 1 /f

    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v SecurityLayer /t REG_DWORD  /d 1 /f

    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v fAllowSecProtocolNegotiation /t REG_DWORD  /d 1 /f
    ```

9. Włączanie usługi RDP, dodając następujące podklucze rejestru:

    ```
    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v fDenyTSConnections /t REG_DWORD  /d 0 /f
    ```


## <a name="configure-windows-firewall-rules"></a>Konfigurowanie reguł zapory systemu Windows
10. Zezwalanie WinRM za pośrednictwem trzy profile zapory (domeny, prywatne i publiczne) i włączyć usługę zdalnego programu PowerShell:

    ```
    Enable-PSRemoting -force
    ```

11. Upewnij się, że obowiązują następujące reguły zapory systemu operacyjnego gościa:

    - Ruch przychodzący

    ```
    netsh advfirewall firewall set rule dir=in name="File and Printer Sharing (Echo Request - ICMPv4-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Network Discovery (LLMNR-UDP-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Network Discovery (NB-Datagram-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Network Discovery (NB-Name-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Network Discovery (Pub-WSD-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Network Discovery (SSDP-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Network Discovery (UPnP-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Network Discovery (WSD EventsSecure-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Windows Remote Management (HTTP-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Windows Remote Management (HTTP-In)" new enable=yes
    ```

    - Ruch przychodzący i wychodzący

    ```
    netsh advfirewall firewall set rule group="Remote Desktop" new enable=yes

    netsh advfirewall firewall set rule group="Core Networking" new enable=yes
    ```

    - Wychodzące

    ```
    netsh advfirewall firewall set rule dir=out name="Network Discovery (LLMNR-UDP-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (NB-Datagram-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (NB-Name-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (Pub-WSD-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (SSDP-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (UPnPHost-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (UPnP-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (WSD Events-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (WSD EventsSecure-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (WSD-Out)" new enable=yes
    ```


## <a name="additional-windows-configuration-steps"></a>Dodatkowe kroki konfiguracji systemu Windows
12. Uruchamianie `winmgmt /verifyrepository` o potwierdzenie, że interfejsu systemu Windows Management oprzyrządowania (repozytorium) jest spójna. Jeśli repozytorium jest uszkodzony, zobacz [Ten wpis w blogu](https://blogs.technet.microsoft.com/askperf/2014/08/08/wmi-repository-corruption-or-not).

13. Upewnij się, że ustawienia danych konfiguracji uruchamiania (BCD) zgodne z następujących czynności:

    ```
    bcdedit /set {bootmgr} integrityservices enable

    bcdedit /set {default} device partition=C:

    bcdedit /set {default} integrityservices enable

    bcdedit /set {default} recoveryenabled Off

    bcdedit /set {default} osdevice partition=C:

    bcdedit /set {default} bootstatuspolicy IgnoreAllFailures
    ```

14. Usuń wszystkie dodatkowe filtry interfejs sterownika transportu, takiego jak oprogramowanie, które analizuje pakiety TCP.
15. Aby upewnić się, dysk jest prawidłowy i spójność, uruchom `CHKDSK /f` polecenia.
16. Odinstaluj innych oprogramowania innych firm lub sterownik związanych z składnikami fizycznymi lub innych technologii wirtualizacji.
17. Upewnij się, że aplikacji innych firm nie korzysta z portu 3389. Ten port jest używany przez usługę RDP platformy Azure. Możesz użyć `netstat -anob` polecenie, aby sprawdzić porty, które są używane przez aplikacje.
18. Jeśli Windows wirtualny dysk twardy, który chcesz przekazać jest kontrolera domeny, wykonaj [następujące kroki](https://support.microsoft.com/kb/2904015) , aby przygotować dysk.
19. Uruchom ponownie komputer maszyn wirtualnych, aby upewnić się, że system Windows jest nadal prawidłowy można się z Tobą za pomocą połączenia RDP.
20. Resetuj bieżące hasło administratora lokalnego i upewnij się, że można użyć tego konta do zalogowania się do systemu Windows za pośrednictwem połączenia RDP.  To uprawnienie dostępu steruje obiektu zasad "Zezwalaj na logowanie za pomocą usług pulpitu zdalnego". Ten obiekt znajduje się w obszarze "Komputera komputera\Ustawienia systemu Windows\Ustawienia zabezpieczeń\Zasady Lokalne\przypisywanie praw."


## <a name="install-windows-updates"></a>Instalowanie aktualizacji systemu Windows
22. Instalowanie najnowszych aktualizacji dla systemu Windows. Jeśli nie jest to możliwe, upewnij się, że są zainstalowane następujące aktualizacje:

    - [KB3137061](https://support.microsoft.com/kb/3137061) Pośrednictwem Microsoft Azure SMS nie odzyskuje z awaria sieci i występują problemy uszkodzenia danych

    - [KB3115224](https://support.microsoft.com/kb/3115224) Ulepszenia niezawodności maszyny wirtualne, które są uruchomione na hoście Windows Server 2012 R2 lub Windows Server 2012

    - [KB3140410](https://support.microsoft.com/kb/3140410) MS16-031: Aktualizacja zabezpieczeń systemu Microsoft Windows do adresu umożliwia podniesienie uprawnień: 8 marca 2016

    - [KB3063075](https://support.microsoft.com/kb/3063075) Wiele zdarzeń 129 identyfikator są rejestrowane podczas uruchamiania systemu Windows Server 2012 R2 maszyny wirtualnej platformy Microsoft Azure

    - [KB3137061](https://support.microsoft.com/kb/3137061) Pośrednictwem Microsoft Azure SMS nie odzyskuje z awaria sieci i występują problemy uszkodzenia danych

    - [KB3114025](https://support.microsoft.com/kb/3114025) Spadek wydajności podczas uzyskiwania dostępu do plików Azure miejsca do magazynowania z systemem Windows 8.1 lub Server 2012 R2

    - [KB3033930](https://support.microsoft.com/kb/3033930) Poprawka zwiększa limit 64 KB buforów RIO na proces usługi Azure w systemie Windows

    - [KB3004545](https://support.microsoft.com/kb/3004545) Nie masz dostępu do maszyn wirtualnych, które są obsługiwane na Azure usług hostingu za pośrednictwem połączenia VPN w systemie Windows

    - [KB3082343](https://support.microsoft.com/kb/3082343) Połączenia VPN lokalnej krzyżowe są tracone, gdy Azure tuneli VPN witryny do witryny za pomocą systemu Windows Server 2012 R2 RRAS

    - [KB3140410](https://support.microsoft.com/kb/3140410) MS16-031: Aktualizacja zabezpieczeń systemu Microsoft Windows do adresu umożliwia podniesienie uprawnień: 8 marca 2016

    - [KB3146723](https://support.microsoft.com/kb/3146723) MS16-048: Opis aktualizacji zabezpieczeń dla CSRSS: 12 kwietnia 2016
    - [KB2904100](https://support.microsoft.com/kb/2904100) System zawiesza się podczas dysku w systemie Windows<a id="step23"></a>
23. Jeśli chcesz utworzyć obraz do wdrożenia na wielu komputerach z niego, musisz uogólnić obraz, uruchamiając `sysprep` przed przekazaniem wirtualny dysk twardy Azure. Nie trzeba uruchomić `sysprep` dla przy użyciu specjalnych wirtualnego dysku twardego. Aby uzyskać więcej informacji o tworzeniu obraz uogólniony zobacz następujące artykuły:

    - [Utworzyć obraz maszyn wirtualnych z przy użyciu modelu wdrożenia Menedżera zasobów istniejących maszyn wirtualnych Azure](virtual-machines-windows-create-vm-generalized.md)
    - [Utworzyć obraz maszyn wirtualnych z przy użyciu modem wdrożenia klasyczny istniejących maszyn wirtualnych Azure](virtual-machines-windows-classic-capture-image.md)
    - [Obsługa narzędzia Sysprep dla ról serwera](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)



## <a name="suggested-extra-configurations"></a>Sugerowane dodatkowych konfiguracji

Następujące ustawienia nie wpływają na przekazywanie wirtualnego dysku twardego. Zaleca się jednak masz ich skonfigurowane.

- Instalowanie [agenta Azure maszyn wirtualnych](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Po zainstalowaniu agenta, możesz włączyć rozszerzenia maszyn wirtualnych. Rozszerzenia maszyn wirtualnych zaimplementować większości funkcji krytyczne, który ma być używany z Twojej maszyny wirtualne jak resetowanie haseł, konfigurowanie RDP i wiele innych.

- Dziennik zrzutu może być pomocne w rozwiązywaniu problemów z awarii systemu Windows. Włącz zbieranie dzienników zrzutu:

    ```
    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 2 /f`

    REG ADD "HKLM\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps" /v DumpFolder /t REG_EXPAND_SZ /d "c:\CrashDumps" /f

    REG ADD "HKLM\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps" /v DumpCount /t REG_DWORD /d 10 /f

    REG ADD "HKLM\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps" /v DumpType /t REG_DWORD /d 2 /f

    sc config wer start= auto
    ```

- Po utworzeniu maszyn wirtualnych w Azure Konfigurowanie pliku systemu określonego rozmiaru strony na dysku D:

    ```
    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Session Manager\Memory Management" /t REG_MULTI_SZ /v PagingFiles /d "D:\pagefile.sys 0 0" /f
    ```


## <a name="next-steps"></a>Następne kroki

- [Przekaż obraz maszyn wirtualnych systemu Windows Azure w przypadku wdrożeń Menedżera zasobów](virtual-machines-windows-upload-image.md)
