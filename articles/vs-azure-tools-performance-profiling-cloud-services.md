<properties 
   pageTitle="Testowanie wydajność usługi w chmurze | Microsoft Azure"
   description="Testowanie wydajność usługi w chmurze przy użyciu programu Visual Studio"
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


# <a name="testing-the-performance-of-a-cloud-service"></a>Testowanie wydajność usługi w chmurze 

##<a name="overview"></a>Omówienie

Wydajność usługi w chmurze można sprawdzić w następujący sposób:

- Narzędzia diagnostyczne Azure za pomocą zbieranie informacji na temat żądania i połączenia, a także przejrzeć statystyki witryny, które pokazują, jak usługa wykonuje z perspektywy klienta. Aby rozpocząć pracę z, zobacz [Konfigurowanie informacje diagnostyczne dla usług w chmurze Azure i maszyn wirtualnych]( http://go.microsoft.com/fwlink/p/?LinkId=623009).

- Użyj programu Visual Studio, aby uzyskać bardziej szczegółowej analizy obliczeniowa aspektów działania usługi. W tym temacie opisano, program programu do pomiaru wydajności, jak działa usługa platformy Azure. Aby dowiedzieć się, jak używać programu do pomiaru wydajności, jak działa usługa lokalnie w emulatora obliczeń zobacz [testów wydajności Azure chmury usługi lokalnie w obliczyć emulatora przy użyciu programu Visual Studio](http://go.microsoft.com/fwlink/p/?LinkId=262845).



## <a name="choosing-a-performance-testing-method"></a>Wybieranie metody testowania

###<a name="use-azure-diagnostics-to-collect"></a>Zbieranie przy użyciu narzędzia diagnostyczne Azure:###

- Statystyka stron sieci web lub usługi, takie jak żądania i połączenia.

- Statystyki dotyczące ról, takie jak częstotliwość uruchomieniu roli.

- Ogólne informacje o korzystaniu z pamięci, takich jak procent czas potrzebny zbierających śmieci, by lub pamięć zestaw uruchomionego roli.

###<a name="use-the-visual-studio-profiler-to"></a>Użyj programu Visual Studio do:###

- Określ, które funkcje w pełni potrwać.

- Zmierz ile czasu trwa każdą część praktyce intensywnie programu.

- Porównanie wydajności szczegółowych raportów dla dwóch wersji usługi.

- Analizowanie przydzielanie pamięci bardziej szczegółowo niż poziom alokacji poszczególnych pamięci.

- Analizowanie problemów współbieżności w kodzie wielowątkowe.

Jeśli używasz programu, po uruchomieniu usługi w chmurze lokalnie lub na Azure może zbierać dane.

###<a name="collect-profiling-data-locally-to"></a>Zbieranie danych profilowania lokalnie do:###

- Testowanie wydajności części usługi w chmurze, takich jak wykonywanie roli konkretnego pracownika, która nie wymaga rzeczywistych ładowania symulowany.

- Testowanie wydajność usługi w chmurze w izolacji warunkach kontroli.

- Testowanie wydajność usługi w chmurze przed wdrożeniem Azure.

- Przeprowadź test wydajność usługi w chmurze i prywatnie, bez zakłócenia istniejącego wdrożenia.

- Testowanie wydajności usługi bez naliczania opłat do uruchamiania w Azure.

###<a name="collect-profiling-data-in-azure-to"></a>Zbieranie danych profilowania w Azure, aby:###

- Testowanie wydajność usługi w chmurze obciążeniu symulowany lub rzeczywistych.

- Użyj metody oprzyrządowania zbieranie danych profilowania, jak w tym temacie opisano w dalszej części.

- Testowanie wydajność usługi w tym samym środowisku jako gdy jest uruchomiona usługa produkcji.

Zazwyczaj symuluje Załaduj do usług w chmurze test w normalnych lub warunków obciążenia.

## <a name="profiling-a-cloud-service-in-azure"></a>Profilowanie usługi w chmurze platformy Azure

Podczas publikowania do usługi w chmurze z programu Visual Studio, można profilu usługi i określ ustawienia profilowania, które zapewniają informacje, które mają. Profilowania sesji jest uruchamiany dla każdego wystąpienia roli. Aby uzyskać więcej informacji na temat publikowania usługi z programu Visual Studio zobacz [Publikowanie w usłudze Cloud Azure z programu Visual Studio](https://msdn.microsoft.com/library/azure/ee460772.aspx).

Aby dowiedzieć się więcej o profilowanie wydajności w programie Visual Studio, zobacz [Przewodnik po dla początkujących profilowanie wydajności](https://msdn.microsoft.com/library/azure/ms182372.aspx) i [Analizowanie wydajności aplikacji przy użyciu narzędzia profilowania](https://msdn.microsoft.com/library/azure/z9z62c29.aspx).

>[AZURE.NOTE] Możesz włączyć IntelliTrace lub profilowania podczas publikowania usługi w chmurze. Nie można włączyć oba.

###<a name="profiler-collection-methods"></a>Metody kolekcji Profiler

Umożliwiają różnych metod profilowanie, oparte na występują problemy z wydajnością:

- **Procesor pobierania** — ta metoda zbiera statystyki aplikacji, które są przydatne w przypadku wstępnej analizy problemów wykorzystania Procesora. Procesor pobierania jest sugerowane metody tworzenia większości dochodzenia wydajności. Na pasku aplikacji, które są profilowanie po zbieranie danych przy próbkowaniu Procesora jest niska wpływ.

- **Oprzyrządowania** — ta metoda zbiera dane szczegółowe chronometraż, które są przydatne do analizy mającego fokus i analizowania problemów z wydajnością wejścia i wyjścia. Metoda oprzyrządowania rekordów każdego wpisu, Zakończ i wywołaniu funkcji funkcji w module podczas profilowanie Uruchom. Ta metoda jest przydatne do zbierania szczegółowych informacji dotyczących kodu i opis wpływu wejściowe i wyjściowe operacje na wydajność aplikacji. Ta metoda jest wyłączona, na komputerze z systemem 32-bitowym systemie operacyjnym. Ta opcja jest dostępna tylko wtedy, gdy korzystania z usług w chmurze w Azure, nie lokalnie w emulatorze obliczeń.

- **Przydzielanie pamięci .NET** — ta metoda zbiera dane przydziału pamięci .NET Framework przy użyciu próbek profilowanie metody. Zebranych danych zawiera liczby i rozmiaru przydzielonego obiektów.

- **Współbieżności** — ta metoda gromadzi dane konfliktu zasobów i procesów i wątków dane wykonanie, które są przydatne w analizie wielowątkowe i wielokrotnego procesu aplikacji. Metoda współbieżności zbiera dane dla każdego zdarzenia, że bloków wykonywania kod, na przykład gdy wątek oczekuje na zablokowany dostęp do zasobu aplikacji używanych. Ta metoda jest przydatna do analizy aplikacje wielowątkowe.

- Można także włączyć **Profilowanie interakcji warstwy**, która zapewnia, że dodatkowe informacje o czasach wykonania synchroniczne ADO.NET połączeń w funkcjach wielopoziomowymi aplikacjom komunikować się z jedną lub więcej baz danych. Może zbierać dane interakcji warstwa z jednej z metod profilowania. Aby uzyskać więcej informacji na temat profilowanie interakcji warstwa zobacz [Wyświetlanie interakcji warstwa](https://msdn.microsoft.com/library/azure/dd557764.aspx).

## <a name="configuring-profiling-settings"></a>Konfigurowanie ustawień profilowania

Na poniższej ilustracji przedstawiono sposób konfigurowania ustawień profilowania w oknie dialogowym Publikowanie aplikacji Azure.

![Konfigurowanie ustawień profilowania](./media/vs-azure-tools-performance-profiling-cloud-services/IC526984.png)

>[AZURE.NOTE] Aby włączyć pole wyboru **Włącz profilowanie** , musisz mieć programu zainstalowane na komputerze lokalnym, używanym do opublikowania usługi w chmurze. Domyślnie programu jest instalowane podczas instalacji programu Visual Studio.

### <a name="to-configure-profiling-settings"></a>Aby skonfigurować ustawienia profilowania

1. W oknie Eksplorator rozwiązań Otwieranie menu skrótów dla Azure projektu, a następnie wybierz **Publikuj**. Aby uzyskać szczegółowe instrukcje dotyczące publikowania usługi w chmurze zobacz [Publikowanie w chmurze usługę za pomocą narzędzia Azure](http://go.microsoft.com/fwlink/p?LinkId=623012).

1. W oknie dialogowym **Publikowanie aplikacji Azure** wybierz kartę **Zaawansowane ustawienia** .

1. Aby włączyć profilowanie, zaznacz pole wyboru **Włącz profilowanie** .

1. Aby skonfigurować ustawienia profilowania, wybierz polecenie hiperłącze **Ustawienia** . Zostanie wyświetlone okno dialogowe Ustawienia profilowania.

1. Z **jakie metody profilowanie czy chcesz użyć** przycisków opcji wybierz typ profilowanie potrzebnych.

1. Zbieranie danych profilowania interakcji warstwa, zaznacz pole wyboru **Włącz profilowanie interakcji warstwa** .

1. Aby zapisać ustawienia, kliknij przycisk **OK** .

    Podczas publikowania tej aplikacji, te ustawienia są używane do tworzenia profilowania sesji dla poszczególnych ról.

## <a name="viewing-profiling-reports"></a>Wyświetlanie profilowanie raportów

Profilowania sesji jest tworzony dla każdego wystąpienia rola w usłudze w chmurze. Aby wyświetlić profilowania raportów każdej sesji z programu Visual Studio, można wyświetlić okno Eksploratora serwera, a następnie wybierz węzeł obliczyć Azure, aby zaznaczyć wystąpienie roli. Następnie możesz wyświetlić profilowania raportu, jak pokazano na poniższej ilustracji.

![Wyświetlanie raportu profilowania z platformy Azure](./media/vs-azure-tools-performance-profiling-cloud-services/IC748914.png)

### <a name="to-view-profiling-reports"></a>Wyświetlanie raportów profilowania

1. Aby wyświetlić okno Eksploratora serwera w programie Visual Studio, na pasku menu wybierz pozycję Widok, Server Explorer.

1. Wybierz węzeł Azure obliczenia, a następnie wybierz węzeł Azure wdrożenia usługi w chmurze, wybranego do profilu, podczas publikowania z programu Visual Studio.

1. Wyświetlanie raportów profilowania dla wystąpienia, wybierz rolę w usłudze, otwieranie menu skrótów dla konkretnego wystąpienia, a następnie wybierz pozycję **Wyświetl raport profilowania**.

    Raport pliku .vsp teraz są pobierane ze Azure i stan pobierania pojawia się w dzienniku aktywności Azure. Po zakończeniu pobierania pliku profilowania raport jest wyświetlany na karcie w edytorze programu Visual Studio o nazwie <Role name> _<Instance Number>_ <identifier>.vsp. Zostanie wyświetlone danych podsumowania w raporcie.

1. Aby wyświetlić różne widoki raportów, na liście Widok bieżący, wybierz typ widoku, który ma. Aby uzyskać więcej informacji zobacz [Profilowanie widoki raportów narzędzia](https://msdn.microsoft.com/library/azure/bb385755.aspx).

## <a name="next-steps"></a>Następne kroki

[Debugowanie usług w chmurze](https://msdn.microsoft.com/library/azure/ee405479.aspx)

[Publikowanie w usłudze w chmurze Azure z programu Visual Studio](https://msdn.microsoft.com/library/azure/ee460772.aspx)

