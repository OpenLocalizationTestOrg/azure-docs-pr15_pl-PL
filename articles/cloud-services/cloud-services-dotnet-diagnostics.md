<properties
    pageTitle="Jak używać Azure diagnostyki (.NET) z usługami w chmurze | Microsoft Azure"
    description="Za pomocą diagnostyki Azure do zbierania danych z usług w chmurze Azure debugowania, pomiaru wydajności, monitorowanie, analiza ruchu i inne."
    services="cloud-services"
    documentationCenter=".net"
    authors="rboucher"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="01/25/2016"
    ms.author="robb"/>



# <a name="enabling-azure-diagnostics-in-azure-cloud-services"></a>Włączanie Azure diagnostyki w usług w chmurze Azure

Zobacz [Omówienie diagnostyki Azure](../azure-diagnostics.md) tła na diagnostyki Azure.


## <a name="how-to-enable-diagnostics-in-a-worker-role"></a>Jak włączyć diagnostyki w roli pracownika

Ten kolejne kroki opisano, jak zaimplementować roli Pracownik Azure emituje telemetrycznego danych przy użyciu klasy .NET źródła zdarzeń. Diagnostyka Azure służy do zbierania danych telemetrycznych i zapisać go w konto Azure miejsca do magazynowania. Podczas tworzenia roli Pracownik Visual Studio umożliwia automatycznie 1.0 diagnostyki jako część rozwiązania w Azure SDK dla 2,4 .NET lub starszej wersji. Poniższe instrukcje przedstawiają proces tworzenia roli Pracownik, wyłączanie 1.0 diagnostyki rozwiązanie i wdrażanie diagnostyki 1.2 lub 1.3 do roli danego pracownika.

### <a name="pre-requisites"></a>Wymagania wstępne
W tym artykule założono, masz subskrypcję usługi Azure i za pomocą programu Visual Studio 2013 Azure SDK. Jeśli nie masz subskrypcji usługi Azure, można Załóż [Bezpłatnej wersji próbnej][]. Upewnij się, że [Instalowanie i konfigurowanie programu PowerShell Azure wersji 0.8.7 lub nowszej][].

### <a name="step-1-create-a-worker-role"></a>Krok 1: Tworzenie roli pracownika
1.  Uruchom program **Visual Studio 2013**.
2.  Tworzenie nowego projektu **Usługi w chmurze Azure** w szablonie **chmury** na tym .NET Framework 4,5 elementów docelowych.  Nazwa projektu "WadExample", a następnie kliknij przycisk Ok.
3.  Wybierz **Rolę pracownika** , a następnie kliknij przycisk Ok. Projekt zostanie utworzony.
4.  W **Eksploratorze rozwiązań**kliknij dwukrotnie plik właściwości **WorkerRole1** .
5.  W **konfiguracji** karcie wyczyść **Włącz diagnostyki** wyłączenia 1.0 diagnostyki (2,4 SDK Azure i eariler).
6.  Utworzyć rozwiązanie, aby sprawdzić, czy masz żadne błędy.

### <a name="step-2-instrument-your-code"></a>Krok 2: Instrumentu kodu
Zamień zawartość WorkerRole.cs poniższy kod. Cztery metody rejestrowania wykonuje klasy SampleEventSourceWriter, dziedziczone z [Zajęć źródła zdarzeń][]: **SendEnums**, **MessageMethod**, **SetOther** i **HighFreq**. Pierwszy parametr metody **WriteEvent** definiuje identyfikator dla odpowiednich zdarzenia. Metody Run wykonuje nieskończonej pętli nawiąże połączenie z tych metod rejestrowania zaimplementowana w klasie **SampleEventSourceWriter** co 10 sekund.

    using Microsoft.WindowsAzure.ServiceRuntime;
    using System;
    using System.Diagnostics;
    using System.Diagnostics.Tracing;
    using System.Net;
    using System.Threading;

    namespace WorkerRole1
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

    public class WorkerRole : RoleEntryPoint
    {
        public override void Run()
        {
            // This is a sample worker implementation. Replace with your logic.
            Trace.TraceInformation("WorkerRole1 entry point called");

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

        public override bool OnStart()
        {
            // Set the maximum number of concurrent connections
            ServicePointManager.DefaultConnectionLimit = 12;

            // For information on handling configuration changes
            // see the MSDN topic at http://go.microsoft.com/fwlink/?LinkId=166357.

            return base.OnStart();
        }
    }
    }


### <a name="step-3-deploy-your-worker-role"></a>Krok 3: Wdrażanie Rola pracownika
1.  Wdrażanie rola Pracownik Azure z poziomu programu Visual Studio, wybierając projektu **WadExample** w Eksploratorze rozwiązań, a następnie **publikowania** z menu **Tworzenie** .
2.  Wybierz subskrypcję.
3.  W oknie dialogowym **Ustawienia publikowania usługi Microsoft Azure** wybierz polecenie **Utwórz nowy...**.
4.  W oknie dialogowym **Tworzenie usługi w chmurze i konta miejsca do magazynowania** wprowadź **nazwę** (na przykład "WadExample"), a następnie wybierz pozycję regionu lub grupy koligacji.
5.  Ustaw **środowisko** **tymczasowego**.
6.  Modyfikowanie dowolne inne **Ustawienia** zależnie od potrzeb, a następnie kliknij pozycję **Publikuj**.
7.  Po zakończeniu wdrożenia sprawdzić, w portalu klasyczny Azure czy usługi w chmurze jest stan **uruchomiony** .

### <a name="step-4-create-your-diagnostics-configuration-file-and-install-the-extension"></a>Krok 4: Tworzenie pliku konfiguracji narzędzia diagnostyczne i zainstaluj rozszerzenie
1.  Pobierz definicja schematu pliku konfiguracji publicznej, wykonując następujące polecenia programu PowerShell:
2.
        (Get AzureServiceAvailableExtension - Nazwa_rozszerzenia "PaaSDiagnostics" - ProviderNamespace "Microsoft.Azure.Diagnostics"). PublicConfigurationSchema | Wyjściowego pliku — kodowanie utf8 - ścieżka pliku "WadConfig.xsd"

2.  Dodawanie pliku XML do projektu **WorkerRole1** , klikając prawym przyciskiem myszy nad projektem **WorkerRole1** i wybierz polecenie **Dodaj** -> **Nowy element...**  ->  **Visual C# elementów** -> **danych** -> **Pliku XML**. Nazwij plik "WadExample.xml".

    ![CloudServices_diag_add_xml](./media/cloud-services-dotnet-diagnostics/AddXmlFile.png)

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

### <a name="step-5-install-diagnostics-on-your-worker-role"></a>Krok 5: Instalowanie diagnostyki na swojej roli pracownika
Polecenia cmdlet programu PowerShell do zarządzania Diagnostyka sieci web lub pracownika roli są: Ustawianie AzureServiceDiagnosticsExtension, Get-AzureServiceDiagnosticsExtension i Usuń AzureServiceDiagnosticsExtension.

1.  Otwórz Azure programu PowerShell.
2.  Wykonaj skrypt, aby zainstalować diagnostyki na swojej roli pracownika (Zamień *StorageAccountKey* klucz konta miejsca do magazynowania dla Twojego konta miejsca do magazynowania wadexample):

```
    $storage_name = "wadexample"
    $key = "<StorageAccountKey>"
    $config_path="c:\users\<user>\documents\visual studio 2013\Projects\WadExample\WorkerRole1\WadExample.xml"
    $service_name="wadexample"
    $storageContext = New-AzureStorageContext -StorageAccountName $storage_name -StorageAccountKey $key
    Set-AzureServiceDiagnosticsExtension -StorageContext $storageContext -DiagnosticsConfigurationPath $config_path -ServiceName $service_name -Slot Staging -Role WorkerRole1
```

### <a name="step-6-look-at-your-telemetry-data"></a>Krok 6: Przeglądanie danych telemetrycznych
W programie Visual Studio **Server Explorer** przejdź do konta wadexample miejsca do magazynowania. Po chmury usługi jest uruchomiony około 5 minut powinna być widoczna tabel **WADEnumsTable**, **WADHighFreqTable** **WADMessageTable**, **WADPerformanceCountersTable** i **WADSetOtherTable**. Kliknij dwukrotnie na jednej z tabel, aby wyświetlić telemetrycznego, które zostały zebrane.
    ![CloudServices_diag_tables](./media/cloud-services-dotnet-diagnostics/WadExampleTables.png)


## <a name="configuration-file-schema"></a>Schemat pliku konfiguracji

Plik konfiguracyjny diagnostyki określa wartości, które są używane do inicjowania ustawienia konfiguracji diagnostyczne wraz z agentem diagnostyki. Zobacz [najnowsze odwołanie schematu](https://msdn.microsoft.com/library/azure/mt634524.aspx) prawidłowe wartości i przykłady.

## <a name="troubleshooting"></a>Rozwiązywanie problemów

Jeśli masz problemy, zobacz [Rozwiązywanie problemów z diagnostyki Azure](../azure-diagnostics-troubleshooting.md) , aby uzyskać pomoc dotyczącą typowych problemów.

## <a name="next-steps"></a>Następne kroki
[Zobacz listę maszyn wirtualnych powiązanych artykułów Azure diagnostyki](azure-diagnostics.md#cloud-services) zmieniać dane, które są pobierane Rozwiązywanie problemów lub dowiedzieć się więcej o diagnostyki ogólnie.


[Źródła zdarzeń zajęć]: http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource(v=vs.110).aspx

[Debugging an Azure Application]: http://msdn.microsoft.com/library/windowsazure/ee405479.aspx   
[Collect Logging Data by Using Azure Diagnostics]: http://msdn.microsoft.com/library/windowsazure/gg433048.aspx
[Bezpłatna wersja próbna]: http://azure.microsoft.com/pricing/free-trial/
[Instalowanie i konfigurowanie programu PowerShell Azure wersji 0.8.7 lub nowszym]: http://azure.microsoft.com/documentation/articles/install-configure-powershell/
