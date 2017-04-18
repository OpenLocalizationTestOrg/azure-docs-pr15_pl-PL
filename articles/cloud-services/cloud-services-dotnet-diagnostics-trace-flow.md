<properties
    pageTitle="Śledzenie przepływu w chmurze aplikacji usług Azure Diagnostyka | Microsoft Azure"
    description="Dodawanie śledzenia wiadomości Azure aplikacji ułatwiające debugowania pomiaru wydajności, monitorowanie, analiza ruchu i inne."
    services="cloud-services"
    documentationCenter=".net"
    authors="rboucher"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="02/20/2016"
    ms.author="robb"/>



# <a name="trace-the-flow-of-a-cloud-services-application-with-azure-diagnostics"></a>Śledzenie przepływu aplikacji usług w chmurze Diagnostyka Azure

Śledzenie jest sposób monitorowania wykonania aplikacji, gdy jest uruchomiony. Klas [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace.aspx), [System.Diagnostics.Debug](https://msdn.microsoft.com/library/system.diagnostics.debug.aspx)i [System.Diagnostics.TraceSource](https://msdn.microsoft.com/library/system.diagnostics.tracesource.aspx) umożliwia rejestrowanie informacji o błędach i uruchomienie aplikacji w dzienniki, plików tekstowych lub innych urządzeń na potrzeby analizy później. Aby uzyskać więcej informacji na temat śledzenia zobacz [Śledzenie i instrumentacji aplikacji](https://msdn.microsoft.com/library/zs6s4h68.aspx).


## <a name="use-trace-statements-and-trace-switches"></a>Używanie instrukcji śledzenia i przełączniki śledzenia

Śledzenie Implementowanie w aplikacji usług w chmurze, dodając [DiagnosticMonitorTraceListener](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitortracelistener.aspx) konfiguracji aplikacji i nawiązywanie połączeń System.Diagnostics.Trace lub System.Diagnostics.Debug w kodzie aplikacji. Za pomocą pliku konfiguracji *app.config* dla ról pracownika i *web.config* dla ról w sieci web. Po utworzeniu nowego hostowanej usługi przy użyciu szablonu programu Visual Studio diagnostyki Azure są automatycznie dodawane do projektu i DiagnosticMonitorTraceListener jest dodawany do pliku konfiguracji odpowiednie dla ról, które można dodać.

Aby uzyskać informacji na temat wprowadzania do śledzenia instrukcje, zobacz [jak: Dodawanie instrukcji śledzenia w celu kod aplikacji](https://msdn.microsoft.com/library/zd83saa2.aspx).

Zaznaczając [Przełączniki śledzenia](https://msdn.microsoft.com/library/3at424ac.aspx) w kodzie, można kontrolować, czy śledzenie występuje i jest ich zakresu. Dzięki temu można monitorować stan aplikacji w środowisku produkcyjnym. Jest to szczególnie ważne w aplikacji biznesowej, która korzysta z wielu składników działa na wielu komputerach. Aby uzyskać więcej informacji, zobacz [jak: Konfigurowanie przełączniki śledzenia](https://msdn.microsoft.com/library/t06xyy08.aspx).

## <a name="configure-the-trace-listener-in-an-azure-application"></a>Konfigurowanie odbiornika śledzenia w aplikacji Azure

Śledzenie debugowania i TraceSource, wymagane konfigurowanie "detektory" do zbierania i rejestrowania wiadomości, które są wysyłane. Detektory zbieranie, przechowywanie i przesyłać śledzenie wiadomości. Bezpośrednie ich wynik śledzenia do odpowiedniego miejsca docelowego, takich jak dziennika, okna lub pliku tekstowego. Diagnostyka Azure korzysta z klasą [DiagnosticMonitorTraceListener](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitortracelistener.aspx) .

Aby wykonać poniższą procedurę, musisz zainicjować Azure monitorowania diagnostycznego. Aby to zrobić, zobacz [Włączanie diagnostyki platformy Microsoft Azure](cloud-services-dotnet-diagnostics.md).

Należy zauważyć, że jeśli korzystasz z szablonów, które są dostarczane przez program Visual Studio, konfiguracja odbiornika jest automatycznie dodawany dla Ciebie.


### <a name="add-a-trace-listener"></a>Dodanie detektora śledzenia

1. Otwórz plik web.config lub app.config dla roli użytkownika.
2. Dodaj następujący kod do pliku. Zmieniaj atrybutu wersji, aby użyć numeru wersji zestawu, który odwołuje się. Wersję zestawu nie musi zmieniać po każdym wydaniu Azure SDK, o ile nie są dostępne aktualizacje do niego.

    ```
    <system.diagnostics>
        <trace>
            <listeners>
                <add type="Microsoft.WindowsAzure.Diagnostics.DiagnosticMonitorTraceListener,
                  Microsoft.WindowsAzure.Diagnostics,
                  Version=2.8.0.0,
                  Culture=neutral,
                  PublicKeyToken=31bf3856ad364e35"
                  name="AzureDiagnostics">
                  <filter type="" />
                </add>
            </listeners>
        </trace>
    </system.diagnostics>
    ```
    >[AZURE.IMPORTANT] Upewnij się, że masz odwołanie projektu do zestawu Microsoft.WindowsAzure.Diagnostics. Aktualizowanie numer wersji XML powyżej zgodnie z wersji zestawu Microsoft.WindowsAzure.Diagnostics, której dotyczy odwołanie.

3. Zapisz plik konfiguracji.

Aby uzyskać więcej informacji na temat detektory zobacz [Śledzenie detektory](https://msdn.microsoft.com/library/4y5y10s7.aspx).

Po zakończeniu czynności, aby dodać odbiornika, możesz dodać instrukcje śledzenia w kodzie.


### <a name="to-add-trace-statement-to-your-code"></a>Dodawanie instrukcji śledzenia w kodzie

1. Otwórz plik źródłowy aplikacji. Na przykład <RoleName>plik CS roli Pracownik lub ról w sieci web.
2. Dodaj następujący za pomocą instrukcji, jeśli nie został już dodany:
    ```
        using System.Diagnostics;
    ```
3. Dodawanie instrukcji śledzenia miejsce, w którym chcesz przechwytywać informacje o stanie aplikacji. Za pomocą różnych metod do formatowania danych wyjściowych instrukcji śledzenia. Aby uzyskać więcej informacji, zobacz [jak: Dodawanie instrukcji śledzenia w celu kod aplikacji](https://msdn.microsoft.com/library/zd83saa2.aspx).
4. Zapisz plik źródłowy.
