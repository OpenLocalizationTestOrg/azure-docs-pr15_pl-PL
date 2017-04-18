<properties
   pageTitle="Debugowanie aplikacji w programie Visual Studio | Microsoft Azure"
   description="Ulepsz niezawodności i wydajności usługi, opracowywanie i debugowanie je w programie Visual Studio w klastrze rozwoju lokalnego."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/21/2016"
   ms.author="vturecek;mikhegn"/>

# <a name="debug-your-service-fabric-application-by-using-visual-studio"></a>Debugowanie aplikacji usługi tkaninie przy użyciu programu Visual Studio

## <a name="debug-a-local-service-fabric-application"></a>Debugowanie miejscowego tkaninie usługi

Można zaoszczędzić czas i pieniądze, wdrażania i debugowanie aplikacji tkaninie usługi Azure w klastrze rozwoju komputera lokalnego. Program Visual Studio można wdrożyć aplikację do klastrów lokalne i automatycznie połączyć debugowania do wszystkich wystąpień aplikacji.

1. Rozpocznij klaster lokalny rozwój, wykonując kroki [konfigurowania środowiska programowania tkaninie usługi](service-fabric-get-started.md).

2. Naciśnij klawisz **F5** lub kliknij pozycję **Debugowanie** > **Rozpocznij debugowanie**.

    ![Uruchamianie debugowania aplikacji][startdebugging]

3. Ustaw punktów kontrolnych w kodzie i krok za pośrednictwem aplikacji, klikając polecenia w menu **Debugowanie** .

    > [AZURE.NOTE] Program Visual Studio dołącza do wszystkich wystąpień aplikacji. Podczas oddalasz przy użyciu kodu, punktów kontrolnych mogą przejść trafień przez wiele procesów, uzyskując sesje równoczesne. Spróbuj wyłączyć punktów przerwania, gdy są już trafienie dokonując każdego przerwania uzależnione Identyfikatora wątku lub za pomocą zdarzenia diagnostyczne.

4. Automatycznie zostanie otwarte okno **Zdarzenia diagnostyczne** , aby można było wyświetlić zdarzenia diagnostyczne w czasie rzeczywistym.

    ![Przeglądanie zdarzeń diagnostyczne w czasie rzeczywistym][diagnosticevents]

5. Można również otworzyć okno **Zdarzenia diagnostyczne** w Eksploratorze chmury.  W obszarze **Tkaninie usługi**kliknij prawym przyciskiem myszy dowolny węzeł i wybierz pozycję **Widok Streaming śledzenia**.

    ![Otwórz okno zdarzenia diagnostyczne][viewdiagnosticevents]

    Jeśli chcesz filtrować wyników śledzenia do określonej usługi lub aplikacji, po prostu włącz przesyłanie strumieniowe śledzenia w tym określonej usługi lub aplikacji.

6. Zdarzenia diagnostyczne można znaleźć w pliku **ServiceEventSource.cs** automatycznie wygenerowanego i są nazywane z kodem aplikacji.

    ```csharp
    ServiceEventSource.Current.ServiceMessage(this, "My ServiceMessage with a parameter {0}", result.Value.ToString());
    ```

7. Okno **Zdarzeń diagnostycznych** obsługuje filtrowanie, wstrzymywanie i inspekcji zdarzeń w czasie rzeczywistym.  Filtr jest prosty ciąg wyszukiwania wiadomości zdarzenia, w tym jego zawartość.

    ![Filtrowanie, Zatrzymaj wskaźnik myszy i wznowienie lub inspekcji zdarzeń w czasie rzeczywistym][diagnosticeventsactions]

8. Debugowanie usług przypomina debugowania dowolnej innej aplikacji. Punkty za pośrednictwem programu Visual Studio będzie zazwyczaj ustawiona do debugowania łatwe. Mimo że zaufanego zbiory replikacji w różnych węzłach, nadal zaimplementować IEnumerable. Oznacza to, że umożliwia wyświetlanie wyników w programie Visual Studio podczas debugowania Zobacz, co jest zapisany, wewnątrz. Po prostu ustawić przerwania w kodzie w dowolnym miejscu.

    ![Uruchamianie debugowania aplikacji][breakpoint]

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->

## <a name="debug-a-remote-service-fabric-application"></a>Debugowanie zdalnego aplikacji tkaninie usługi

Jeśli aplikacji tkaninie usługi są uruchomione w klastrze tkaninie usługi platformy Azure, jest możliwe zdalne debugowanie tych bezpośrednio z programu Visual Studio.

> [AZURE.NOTE] Ta funkcja wymaga [usługi tkaninie SDK 2.0](http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015) i [Zestaw SDK Azure dla .NET 2.9](https://azure.microsoft.com/downloads/).    

<!-- -->
> [AZURE.WARNING] Zdalne debugowanie jest przeznaczona dla deweloperów/testowanie scenariuszy i nie ma być używana w środowisku produkcyjnym, ze względu na ich wpływu na uruchomione aplikacje.

1. Przejdź do klaster w **Eksploratorze w chmurze**, kliknij prawym przyciskiem myszy i wybierz polecenie **Włącz debugowanie**

    ![Włączanie debugowania zdalnego][enableremotedebugging]

    To spowoduje rozpoczęcie procesu Włączanie zdalnego debugowania rozszerzenia na węzłach oraz konfiguracji sieci wymagane.

2. Kliknij prawym przyciskiem myszy węzła w **Eksploratorze w chmurze**, a następnie wybierz pozycję **Dołącz debugowania**

    ![Dołączanie debugowania][attachdebugger]

3. W oknie dialogowym **Dołączanie przetwarzania** wybierz procesu, który chcesz debugowanie, a następnie kliknij przycisk **Dołącz**

    ![Wybierz proces][chooseprocess]

    Nazwa procesu, który chcesz dołączyć, równa się nazwie nazwę zestawu projektu usługi.

    Debugowania dołączy do wszystkich węzłów przeprowadzenie procesu.
    - W przypadku miejsce, w którym są debugowania usługi stateless wszystkie wystąpienia usługi we wszystkich węzłach są częścią sesji debugowania.
    - Jeśli debugowania usługi stanowe, tylko podstawowy replice dowolnego partition będzie aktywna i w związku z tym złowionych przez debugowania. Jeśli podstawowy replice przenosi podczas sesji debugowania, przetwarzania że w replice nadal będą częścią sesji debugowania.
    - Aby przechwytywać tylko partycje odpowiednich lub wystąpienia danej usługi, punkty warunkowe umożliwia tylko przerwać określonych partycją lub wystąpienia.

    ![Warunkowe przerwania][conditionalbreakpoint]

    > [AZURE.NOTE] Obecnie nie jest obsługiwana debugowania klastrze tkaninie usługi z wielu wystąpień tej samej nazwy wykonywalny usługi.

4. Po zakończeniu debugowania aplikacji, można wyłączyć zdalnego rozszerzenie debugowania, klikając prawym przyciskiem myszy klaster w **Eksploratorze chmury** i wybierz pozycję **Wyłącz debugowanie**

    ![Wyłączanie zdalnego debugowania][disableremotedebugging]

## <a name="streaming-traces-from-a-remote-cluster-node"></a>Przesyłanie strumieniowe śledzenia z zdalnego węzła

Jest również możliwe do śledzenia strumienia bezpośrednio z poziomu węzła klaster zdalny programu Visual Studio. Ta funkcja umożliwia zdarzeń śledzenia ETW strumienia wyprodukowano w węźle klaster tkaninie usługi bezpośrednio w programie Visual Studio.

> [AZURE.NOTE] Ta funkcja wymaga [usługi tkaninie SDK 2.0](http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015) i [Zestaw SDK Azure dla .NET 2.9](https://azure.microsoft.com/downloads/).

<!-- -->
> [AZURE.WARNING]Przesyłanie strumieniowe śledzenia jest przeznaczona dla deweloperów/testowanie scenariuszy i nie ma być używana w środowisku produkcyjnym, ze względu na ich wpływu na uruchomione aplikacje.
> Scenariusz z produkcji będą miały przekazywania zdarzeń za pomocą narzędzia diagnostyczne Azure.

1. Przejdź do klaster w **Eksploratorze w chmurze**, kliknij prawym przyciskiem myszy i wybierz pozycję **Włącz przesyłanie strumieniowe śledzenia**

    ![Włączanie śledzenia przesyłanie strumieniowe zdalnego][enablestreamingtraces]

    To spowoduje rozpoczęcie proces umożliwienie przesyłanie strumieniowe rozszerzenie śledzenia w węzłach, jak również konfiguracji sieci wymagane.

2. Rozwiń **węzły** elementu w **Eksploratorze w chmurze**, a następnie kliknij prawym przyciskiem myszy węzeł, który chcesz przesyłać strumieniowo śledzenia z i wybierz pozycję **Widok Streaming śledzenia**

    ![Widok zdalny streaming śledzenia][viewremotestreamingtraces]

    Powtórz krok 2 dla dowolnej liczby węzłów mają być wyświetlane śledzenia z. Każdego strumienia węzły będą widoczne w oknie dedykowane.

    Teraz są widoczne śledzenia dostarczanych przez usługę tkaninie i usług. Jeśli chcesz filtrować zdarzenia w celu wyświetlenia tylko określonej aplikacji, wystarczy wpisać nazwy aplikacji w filtrze.

    ![Wyświetlanie streaming śledzenia][viewingstreamingtraces]

4. Po zakończeniu przesyłanie strumieniowe śledzenia z klaster, można wyłączyć zdalnego śledzenia przesyłanie strumieniowe, klikając prawym przyciskiem myszy klaster w **Eksploratorze w chmurze** i wybierz pozycję **Wyłącz przesyłanie strumieniowe śledzenia**

    ![Wyłączanie śledzenia przesyłanie strumieniowe zdalnego][disablestreamingtraces]

## <a name="next-steps"></a>Następne kroki

- [Test usługi tkaninie usługi](service-fabric-testability-overview.md).
- [Zarządzaj aplikacjami usług tkaninie w programie Visual Studio](service-fabric-manage-application-in-visual-studio.md).

<!--Image references-->
[startdebugging]: ./media/service-fabric-debugging-your-application/startdebugging.png
[diagnosticevents]: ./media/service-fabric-debugging-your-application/diagnosticevents.png
[viewdiagnosticevents]: ./media/service-fabric-debugging-your-application/viewdiagnosticevents.png
[diagnosticeventsactions]: ./media/service-fabric-debugging-your-application/diagnosticeventsactions.png
[breakpoint]: ./media/service-fabric-debugging-your-application/breakpoint.png
[enableremotedebugging]: ./media/service-fabric-debugging-your-application/enableremotedebugging.png
[attachdebugger]: ./media/service-fabric-debugging-your-application/attachdebugger.png
[chooseprocess]: ./media/service-fabric-debugging-your-application/chooseprocess.png
[conditionalbreakpoint]: ./media/service-fabric-debugging-your-application/conditionalbreakpoint.png
[disableremotedebugging]: ./media/service-fabric-debugging-your-application/disableremotedebugging.png
[enablestreamingtraces]: ./media/service-fabric-debugging-your-application/enablestreamingtraces.png
[viewingstreamingtraces]: ./media/service-fabric-debugging-your-application/viewingstreamingtraces.png
[viewremotestreamingtraces]: ./media/service-fabric-debugging-your-application/viewremotestreamingtraces.png
[disablestreamingtraces]: ./media/service-fabric-debugging-your-application/disablestreamingtraces.png
