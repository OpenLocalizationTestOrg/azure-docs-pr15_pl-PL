<properties 
   pageTitle="Debugowanie usługi w chmurze opublikowanych z IntelliTrace i Visual Studio | Microsoft Azure"
   description="Debugowanie usługi w chmurze opublikowanych z IntelliTrace i Visual Studio"
   services="visual-studio-online"
   documentationCenter="n/a"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="visual-studio-online"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="debugging-a-published-cloud-service-with-intellitrace-and-visual-studio"></a>Debugowanie usługi w chmurze opublikowanych z IntelliTrace i Visual Studio

##<a name="overview"></a>Omówienie

Przy użyciu IntelliTrace można rejestrować szczegółowe informacje debugowania w przypadku wystąpienia roli podczas wykonywania platformy Azure. Jeśli chcesz znaleźć przyczynę problemu, umożliwia dzienniki IntelliTrace kroków kodu z programu Visual Studio tak, jakby były uruchomione w Azure. W praktyce rekordów IntelliTrace klawisz wykonywanie kodu i danych środowiska Azure aplikacji działa jako usługa w chmurze platformy Azure i pozwala odtwarzać zarejestrowane dane z programu Visual Studio. Alternatywnie umożliwia zdalne debugowanie podłączone bezpośrednio do usługi w chmurze, którym działa Azure. Zobacz [Debugowanie usług w chmurze](http://go.microsoft.com/fwlink/p/?LinkId=623041).

>[AZURE.IMPORTANT] IntelliTrace jest przeznaczony dla tylko scenariusze debugowania i nie może być używana do wdrożenia produkcji.

>[AZURE.NOTE] Za pomocą IntelliTrace, jeśli masz Visual Studio przedsiębiorstwa, zainstalowany i elementy docelowe Azure aplikacji .NET Framework 4 lub nowszym. IntelliTrace zbiera informacje dla poszczególnych ról Azure. Maszyn wirtualnych do tych ról są zawsze uruchamiane w 64-bitowych systemów operacyjnych.

## <a name="to-configure-an-azure-application-for-intellitrace"></a>Aby skonfigurować aplikację Azure dla IntelliTrace

Aby włączyć IntelliTrace Azure aplikacji, należy utworzyć i opublikować aplikację z projektu programu Visual Studio Azure. Przed opublikowaniem Azure, musisz skonfigurować IntelliTrace Azure aplikacji. Jeśli publikowanie aplikacji bez konfigurowania IntelliTrace, ale okaże się, że chcesz to zrobić, musisz opublikować aplikację ponownie z programu Visual Studio. Aby uzyskać więcej informacji zobacz [Publikowanie usługi w chmurze za pomocą narzędzi Azure](http://go.microsoft.com/fwlink/p/?LinkId=623012).

1. Jeśli możesz przystąpić do wdrażania aplikacji Azure, sprawdź, czy elementy docelowe kompilacji programu project są ustawione na **Debugowanie**.

1. Otwórz menu skrótów dla projektu Azure w Eksploratorze rozwiązań i wybierz pozycję **Publikuj**.
 
    Zostanie wyświetlony Kreator Publikowanie aplikacji Azure.

1. Zbieranie dzienników IntelliTrace aplikacji, po opublikowaniu w chmurze, zaznacz pole wyboru **Włącz IntelliTrace** .

    >[AZURE.NOTE] Możesz włączyć IntelliTrace lub profilowania podczas publikowania Azure aplikacji. Nie można włączyć oba.

1. Aby dostosować podstawowa konfiguracja IntelliTrace, wybierz polecenie hiperłącze **Ustawienia** .

    Zostanie wyświetlone okno dialogowe Ustawienia IntelliTrace, jak pokazano na poniższym rysunku. Możesz określić, które zdarzenia dziennika, czy na potrzeby zbierania informacji połączenia, które moduły i procesów zbieranie dzienników dla i ile miejsca przypisanej do nagrywania. Aby uzyskać więcej informacji o IntelliTrace zobacz [Debugowanie z IntelliTrace](http://go.microsoft.com/fwlink/?LinkId=214468).

    ![VST_IntelliTraceSettings](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/IC519063.png)

Dziennik IntelliTrace jest pliku dziennika cyklicznego maksymalny rozmiar określony w ustawieniach IntelliTrace (domyślny rozmiar wynosi 250 MB). Dzienniki IntelliTrace trafiają do pliku w systemie plików maszyny wirtualnej. Gdy użytkownik zażąda dzienniki, migawki jest wykonywane w danym momencie i zostało pobrane na komputer lokalny.

Po opublikowaniu aplikacji Azure Azure można określić, jeśli włączono IntelliTrace z węzła obliczyć Azure w Eksploratorze serwera, jak pokazano na poniższej ilustracji:

![VST_DeployComputeNode](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/IC744134.png)

## <a name="downloading-intellitrace-logs-for-a-role-instance"></a>Pobieranie dzienniki IntelliTrace w przypadku wystąpienia roli

Dzienniki IntelliTrace w przypadku wystąpienia roli można pobrać z węzła **Usług w chmurze** w **Eksploratorze serwera**. Rozwiń węzeł **Usług w chmurze** , dopóki nie ustalisz wystąpienie, który Cię interesuje, otwórz menu skrótów dla tego wystąpienia i wybierz pozycję **Wyświetl dzienniki IntelliTrace**. Dzienniki IntelliTrace są pobierane do pliku w katalogu na komputerze lokalnym. Za każdym razem żądania IntelliTrace dzienniki, zostanie utworzona nowa migawka.

Po pobraniu dzienniki Visual Studio Wyświetla postęp operacji w oknie Dziennik Azure. Jak pokazano na poniższym rysunku, możesz rozwinąć pozycji wyświetlić więcej szczegółów operacji.

![VST_IntelliTraceDownloadProgress](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/IC745551.png)

Czy kontynuować pracę w programie Visual Studio, podczas gdy dzienniki IntelliTrace są pobierane. Dziennik zakończył pobieranie, będą automatycznie otwierane w programie Visual Studio.

>[AZURE.NOTE] Dzienniki IntelliTrace może zawierać wyjątki ramach generuje, a następnie obsługuje. Kod wewnętrzną strukturę generuje następujące wyjątki w ramach normalnego uruchamiania roli, więc można je bezpiecznie zignorować.

## <a name="see-also"></a>Zobacz też

[Debugowanie usług w chmurze](https://msdn.microsoft.com/library/ee405479.aspx)

