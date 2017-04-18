<properties
    pageTitle="Jak używać Azure diagnostyki w środowisku maszyn wirtualnych systemu | Microsoft Azure"
    description="Zbierz dane od maszyn wirtualnych Azure debugowania, pomiaru wydajności, monitorowanie, analiza ruchu i innych za pomocą diagnostyki Azure"
    services="virtual-machines"
    documentationCenter=".net"
    authors="rboucher"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="virtual-machines"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="02/20/2016"
    ms.author="robb"/>



# <a name="enabling-diagnostics-in-azure-virtual-machines"></a>Włączanie narzędzia diagnostyczne w środowisku maszyn wirtualnych Azure

Zobacz [Omówienie diagnostyki Azure](azure-diagnostics.md) tła na diagnostyki Azure.

## <a name="how-to-enable-diagnostics-in-a-virtual-machine"></a>Jak włączyć diagnostyki w maszyny wirtualnej

Ten kolejne kroki opisano sposób zdalnego instalowania diagnostyki Azure maszyn wirtualnych z komputera. Możesz także Dowiedz się, jak wdrażać aplikacja działa na tym Azure maszyn wirtualnych i emituje telemetrycznego danych przy użyciu.NET [Klasy źródła zdarzeń][]. Diagnostyka Azure służy do zbierania danych telemetrycznych i zapisać go w konto Azure miejsca do magazynowania.

### <a name="pre-requisites"></a>Wymagania wstępne
Ten kolejne kroki przyjęto założenie, masz subskrypcję usługi Azure i za pomocą programu Visual Studio 2013 Azure SDK. Jeśli nie masz subskrypcji usługi Azure, można Załóż [Bezpłatnej wersji próbnej][]. Upewnij się, że [Instalowanie i konfigurowanie programu PowerShell Azure wersji 0.8.7 lub nowszej][].

### <a name="step-1-create-a-virtual-machine"></a>Krok 1: Tworzenie maszyny wirtualnej
1.  Uruchom program Visual Studio 2013 na komputerze dewelopera.
2.  W programie Visual Studio **Server Explorer** rozwiń **Azure**, kliknij prawym przyciskiem myszy **maszyn wirtualnych** , a następnie wybierz pozycję **Tworzenie maszyn wirtualnych**.
3.  Wybierz subskrypcję Azure w oknie dialogowym **Wybierz subskrypcję** , a następnie kliknij przycisk **Dalej**.
4.  Zaznacz w oknie dialogowym **Wybieranie obrazu maszyn wirtualnych** **Systemu Windows Server 2012 R2 Datacenter listopad 2014** , a następnie kliknij przycisk **Dalej**.
5.  W obszarze **Ustawienia podstawowe maszyn wirtualnych**skonfiguruj nazwę maszyn wirtualnych do "wadexample". Ustaw swoją nazwę użytkownika administratora i hasło, a następnie kliknij przycisk **Dalej**.
6.  W oknie dialogowym **Ustawienia usługi w chmurze** Tworzenie nowej usługi w chmurze o nazwie "wadexampleVM". Utwórz nowe konto miejsca do magazynowania o nazwie "wadexample", a następnie kliknij przycisk **Dalej**.
7.  Kliknij przycisk **Utwórz**.

### <a name="step-2-create-your-application"></a>Krok 2: Tworzenie aplikacji
1.  Uruchom program Visual Studio 2013 na komputerze dewelopera.
2.  Utwórz nową Visual C# konsoli aplikację, która jest przeznaczony dla programu .NET Framework 4,5. Nadaj nazwę projektu "WadExampleVM".
    ![CloudServices_diag_new_project](./media/virtual-machines-dotnet-diagnostics/NewProject.png)
3.  Zamień zawartość plik Program.cs poniższy kod. Klasy **SampleEventSourceWriter** wykonuje cztery metody rejestrowania: **SendEnums**, **MessageMethod**, **SetOther** i **HighFreq**. Pierwszy parametr metody WriteEvent definiuje identyfikator dla odpowiednich zdarzenia. Metody Run wykonuje nieskończonej pętli nawiąże połączenie z tych metod rejestrowania zaimplementowana w klasie **SampleEventSourceWriter** co 10 sekund.

        using System;
        using System.Diagnostics;
        using System.Diagnostics.Tracing;
        using System.Threading;

        namespace WadExampleVM
        {
        sealed class SampleEventSourceWriter : EventSource
        {
            public static SampleEventSourceWriter Log = new SampleEventSourceWriter();
            public void SendEnums(MyColor color, MyFlags flags) { if (IsEnabled())  WriteEvent(1, (int)color, (int)flags); }// Cast enums to int for efficient logging.
            public void MessageMethod(string Message) { if (IsEnabled())  WriteEvent(2, Message); }
            public void SetOther(bool flag, int myInt) { if (IsEnabled())  WriteEvent(3, flag, myInt); }
            public void HighFreq(int value) { if (IsEnabled()) WriteEvent(4, value); }

        }

        enum MyColor
        {
            Red,
            Blue,
            Green
        }

        [Flags]
        enum MyFlags
        {
            Flag1 = 1,
            Flag2 = 2,
            Flag3 = 4
        }

        class Program
        {
        static void Main(string[] args)
        {
            Trace.TraceInformation("My application entry point called");

            int value = 0;

            while (true)
            {
                Thread.Sleep(10000);
                Trace.TraceInformation("Working");

                // Emit several events every time we go through the loop
                for (int i = 0; i < 6; i++)
                {
                    SampleEventSourceWriter.Log.SendEnums(MyColor.Blue, MyFlags.Flag2 | MyFlags.Flag3);
                }

                for (int i = 0; i < 3; i++)
                {
                    SampleEventSourceWriter.Log.MessageMethod("This is a message.");
                    SampleEventSourceWriter.Log.SetOther(true, 123456789);
                }

                if (value == int.MaxValue) value = 0;
                SampleEventSourceWriter.Log.HighFreq(value++);
            }

        }
        }
        }


4.  Zapisz plik i wybierz **Utworzyć rozwiązanie** z menu **Tworzenie** tworzenia kodu.


### <a name="step-3-deploy-your-application"></a>Krok 3: Wdrażanie aplikacji
1.  Kliknij prawym przyciskiem myszy nad projektem **WadExampleVM** w **Eksploratorze rozwiązań** i wybierz polecenie **Otwórz Folder w Eksploratorze plików**.
2.  Przejdź do folderu *bin\Debug* i skopiuj wszystkie pliki (WadExampleVM.*)
3.  **Server Explorer** kliknij prawym przyciskiem myszy maszyny wirtualnej i wybierz polecenie **Połącz przy użyciu pulpitu zdalnego**.
4.  Po połączeniu się maszyn wirtualnych Utwórz folder o nazwie WadExampleVM i Wklej aplikacji pliki do tego folderu.
5.  Uruchom aplikację WadExampleVM.exe. Powinien zostać wyświetlony okna konsoli puste.

### <a name="step-4-create-your-diagnostics-configuration-and-install-the-extension"></a>Krok 4: Tworzenie konfiguracji narzędzia diagnostyczne i zainstaluj rozszerzenie
1.  Pobierz definicja schematu pliku konfiguracji publicznej na komputer rozwoju, wykonując następujące polecenia programu PowerShell:

        (Get-AzureServiceAvailableExtension -ExtensionName 'PaaSDiagnostics' -ProviderNamespace 'Microsoft.Azure.Diagnostics').PublicConfigurationSchema | Out-File -Encoding utf8 -FilePath 'WadConfig.xsd'

2.  Otwórz nowy plik XML w programie Visual Studio, albo w projekcie już otwartych lub w programie Visual Studio wystąpienia z braku otwartych projektów. W programie Visual Studio, wybierz pozycję **Dodaj** -> **Nowy element...**  ->  **Visual C# elementów** -> **danych** -> **Pliku XML**. Nazwa pliku "WadExample.xml"
3.  Kojarzenie WadConfig.xsd z pliku konfiguracji. Upewnij się, że okno edytora WadExample.xml jest aktywne okno. Naciśnij klawisz **F4** , aby otworzyć okno **Właściwości** . Kliknij właściwość **schematów** w oknie **dialogowym właściwości** . Kliknij przycisk **...** w polu właściwości **schematów** . Kliknij przycisk **Dodaj** przycisk i przejdź do lokalizacji, w której zapisano plik XSD i wybierz plik WadConfig.xsd. Kliknij **przycisk OK**.
4.  Zamień zawartość pliku konfiguracji WadExample.xml następujące XML i Zapisz plik. Ten plik konfiguracyjny definiuje kilka liczniki wydajności do: jeden umożliwia użycie Procesora i jedną dla wykorzystanie pamięci. Następnie konfiguracji określa terminy cztery odpowiadające metod w klasie SampleEventSourceWriter.

```
        <?xml version="1.0" encoding="utf-8"?>
        <PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
            <WadCfg>
                <DiagnosticMonitorConfiguration overallQuotaInMB="25000">
                <PerformanceCounters scheduledTransferPeriod="PT1M">
                    <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT1M" unit="percent" />
                    <PerformanceCounterConfiguration counterSpecifier="\Memory\Committed Bytes" sampleRate="PT1M" unit="bytes"/>
                    </PerformanceCounters>
                    <EtwProviders>
                        <EtwEventSourceProviderConfiguration provider="SampleEventSourceWriter" scheduledTransferPeriod="PT5M">
                            <Event id="1" eventDestination="EnumsTable"/>
                            <Event id="2" eventDestination="MessageTable"/>
                            <Event id="3" eventDestination="SetOtherTable"/>
                            <Event id="4" eventDestination="HighFreqTable"/>
                            <DefaultEvents eventDestination="DefaultTable" />
                        </EtwEventSourceProviderConfiguration>
                    </EtwProviders>
                </DiagnosticMonitorConfiguration>
            </WadCfg>
        </PublicConfig>
```

### <a name="step-5-remotely-install-diagnostics-on-your-azure-virtual-machine"></a>Krok 5: Zdalnie instalować diagnostyki na komputerze wirtualnych Azure
Polecenia cmdlet programu PowerShell do zarządzania diagnostyki na maszyny są: Ustawianie AzureVMDiagnosticsExtension, Get-AzureVMDiagnosticsExtension i Usuń AzureVMDiagnosticsExtension.

1.  Na komputerze dewelopera Otwórz Azure programu PowerShell.
2.  Wykonaj skrypt zdalnie zainstalować diagnostyki na swojej maszyn wirtualnych (Zamień *StorageAccountKey* klucz konta miejsca do magazynowania dla Twojego konta miejsca do magazynowania wadexamplevm):

        $storage_name = "wadexamplevm"
        $key = "<StorageAccountKey>"
        $config_path="c:\users\<user>\documents\visual studio 2013\Projects\WadExampleVM\WadExampleVM\WadExample.xml"
        $service_name="wadexamplevm"
        $vm_name="WadExample"
        $storageContext = New-AzureStorageContext -StorageAccountName $storage_name -StorageAccountKey $key
        $VM1 = Get-AzureVM -ServiceName $service_name -Name $vm_name
        $VM2 = Set-AzureVMDiagnosticsExtension -DiagnosticsConfigurationPath $config_path -Version "1.*" -VM $VM1 -StorageContext $storageContext
        $VM3 = Update-AzureVM -ServiceName $service_name -Name $vm_name -VM $VM2.VM


### <a name="step-6-look-at-your-telemetry-data"></a>Krok 6: Przeglądanie danych telemetrycznych
W programie Visual Studio **Server Explorer** przejdź do konta wadexample miejsca do magazynowania. Po maszyn wirtualnych działaniu około 5 minut powinna być widoczna tabel **WADEnumsTable**, **WADHighFreqTable**, **WADMessageTable**, **WADPerformanceCountersTable** i **WADSetOtherTable**. Kliknij dwukrotnie na jednej z tabel, aby wyświetlić telemetrycznego, które zostały zebrane.

![CloudServices_diag_wadexamplevm_tables](./media/virtual-machines-dotnet-diagnostics/WadExampleVMTables.png)

## <a name="configuration-file-schema"></a>Schemat pliku konfiguracji

Plik konfiguracyjny diagnostyki określa wartości, które są używane do inicjowania ustawienia konfiguracji diagnostyczne wraz z agentem diagnostyki. Zobacz [najnowsze odwołanie schematu](https://msdn.microsoft.com/library/azure/mt634524.aspx) prawidłowe wartości i przykłady.

## <a name="troubleshooting"></a>Rozwiązywanie problemów

Aby uzyskać więcej informacji, zobacz [Rozwiązywanie problemów z diagnostyki Azure](azure-diagnostics-troubleshooting.md) .


## <a name="next-steps"></a>Następne kroki
[Zobacz listę maszyn wirtualnych powiązanych artykułów Azure diagnostyki](azure-diagnostics.md#virtual-machines-using-azure-diagnostics) zmieniać dane, które są pobierane Rozwiązywanie problemów lub dowiedzieć się więcej o diagnostyki ogólnie.


[Źródła zdarzeń zajęć]: http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource(v=vs.110).aspx

[Debugging an Azure Application]: http://msdn.microsoft.com/library/windowsazure/ee405479.aspx   
[Collect Logging Data by Using Azure Diagnostics]: http://msdn.microsoft.com/library/windowsazure/gg433048.aspx
[Bezpłatna wersja próbna]: http://azure.microsoft.com/pricing/free-trial/
[Instalowanie i konfigurowanie programu PowerShell Azure wersji 0.8.7 lub nowszym]: http://azure.microsoft.com/documentation/articles/install-configure-powershell/
