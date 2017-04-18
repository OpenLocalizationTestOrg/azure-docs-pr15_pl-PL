<properties
    pageTitle="Ładowanie testowania aplikacji przy użyciu programu Visual Studio Team Services | Microsoft Azure"
    description="Dowiedz się, jak przetestować aplikacje tkaninie usługi Azure za pomocą programu Visual Studio Team Services."
    services="service-fabric"
    documentationCenter="na"
    authors="cawams"
    manager="timlt"
    editor="" />

<tags
    ms.service="multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="multiple"
    ms.date="07/29/2016"
    ms.author="cawa" />

# <a name="load-test-your-application-by-using-visual-studio-team-services"></a>Ładowanie testowania aplikacji przy użyciu programu Visual Studio Team Services

W tym artykule przedstawiono sposób przetestować za pomocą programu Microsoft Visual Studio ładowania testowanie funkcji aplikacji. Użyto tkaninie usługi Azure stanowe usługi wewnętrznej oraz stateless usługi frontonu sieci web. Przykład aplikacja używane w tym miejscu jest samolotem simulator lokalizacji. Podasz identyfikator samolotem, czas wyjścia oraz miejsca docelowego. Wewnętrznej aplikacji przetwarza żądania i fronton są wyświetlane na mapie samolotem, które spełniają kryteria.

Na poniższym diagramie przedstawiono aplikacji tkaninie usługa, która będzie można się testowania.

![Diagram przedstawiający przykład aplikacja lokalizacji samolotem][0]

## <a name="prerequisites"></a>Wymagania wstępne
Przed rozpoczęciem, należy wykonać następujące czynności:

- Uzyskiwanie konta programu Visual Studio Team Services. Możesz uzyskać bezpłatnie w [Visual Studio Team Services](https://www.visualstudio.com).
- Pobieranie i instalowanie Visual Studio 2013 lub Visual Studio 2015 r. W tym artykule używa programu Visual Studio 2015 Enterprise edition, ale program Visual Studio 2013 oraz innych wersji powinny działać podobnie.
- Wdrażanie aplikacji środowiska wzorcowego. Zobacz, [jak wdrażać aplikacje do klastrów zdalnych przy użyciu programu Visual Studio](service-fabric-publish-app-remote-cluster.md) informacje na temat.
- Opis aplikacji zastosowania deseniu. Te informacje służy do symulacji deseniu ładowania.
- Opis cel testowania ładowania. Ułatwia interpretowanie i analizowanie wyników test ładowania.

## <a name="create-and-run-the-web-performance-and-load-test-project"></a>Tworzenie i uruchamianie wydajności sieci Web i badanie projektu

### <a name="create-a-web-performance-and-load-test-project"></a>Tworzenie projektu wydajności sieci Web i badanie

1. Otwórz program Visual Studio 2015 r. Wybierz **plik** > **Nowy** > **projektu** na pasku menu, aby otworzyć okno dialogowe **Nowy projekt** .

2. Rozwiń węzeł **Visual C#** i wybierz pozycję **Test** > **wydajności sieci Web i badanie projektu**. Nadaj nazwę projektu, a następnie wybierz przycisk **OK** .

    ![Zrzut ekranu: okno dialogowe Nowy projekt][1]

    Powinien zostać wyświetlony nowy projekt wydajności sieci Web i badanie w Eksploratorze rozwiązań.

    ![Zrzut ekranu: Eksplorator rozwiązań z nowego projektu][2]

### <a name="record-a-web-performance-test"></a>Rekord testu wydajności sieci web

1. Otwórz projekt .webtest.

2. Wybierz ikonę **Dodaj rejestrowania** , aby rozpocząć sesję nagrywania w przeglądarce.

    ![Zrzut ekranu przedstawiający ikonę Dodaj nagranie w przeglądarce][3]

    ![Zrzut ekranu przedstawiający przycisk Rejestruj w przeglądarce][4]

3. Przejdź do aplikacji usługi tkaninie. Panel nagrywania powinny być wyświetlane żądania sieci web.

    ![Zrzut ekranu przedstawiający żądania sieci web w panelu nagrywania][5]

4. Wykonywanie sekwencji czynności, które można się spodziewać użytkownikom wykonywanie. Te akcje są używane jako deseń do generowania Załaduj.

5. Po zakończeniu wybierz przycisk **Zatrzymaj** , aby zatrzymać rejestrowanie.

    ![Zrzut ekranu przedstawiający przycisk Zatrzymaj][6]

    Projekt .webtest w programie Visual Studio należy przechwycone seria żądań. Parametry dynamiczne są zastępowane automatycznie. W tym momencie możesz usunąć żądań współzależności dodatkowe, powtarzających się, które nie są częścią rozwiązania testowych.

6. Zapisz projekt, a następnie wybierz polecenie **Uruchom przetestować** , aby lokalnie uruchomić test wydajności sieci web i upewnij się, że wszystko działa poprawnie.

    ![Zrzut ekranu: polecenie Uruchom Test][7]

### <a name="parameterize-the-web-performance-test"></a>Definiowanie parametrów test wydajności sieci web

Definiowanie parametrów test wydajności sieci web jest można konwertować go na test wydajności zakodowany w sieci web, a następnie edytując kod. Alternatywnie można powiązać test wydajności sieci web do listy danych tak, aby test iterację danych. Aby uzyskać szczegółowe informacje o sposobie konwertowania test wydajności sieci web próbie kodowane jako, zobacz [Generuj i uruchom test wydajności zakodowany w sieci web](https://msdn.microsoft.com/library/ms182552.aspx) . Informacje na temat powiązać danych testu wydajności sieci web, zobacz temat [Dodawanie źródła danych do wydajności sieci web, testowanie](https://msdn.microsoft.com/library/ms243142.aspx) .

W tym przykładzie firma Microsoft będzie przekonwertować test wydajności sieci web kodowane test, można zastąpić identyfikator samolotem wygenerowane GUID lub dodawać więcej żądań wysyłanie lotów do różnych miejsc.

### <a name="create-a-load-test-project"></a>Tworzenie projektu badania ładowania

Projekt testowy ładowania składa się z jednego lub więcej scenariuszy opisanego przez test wydajności sieci web i testu jednostki, wraz z dodatkowych określone obciążenie testowanie ustawień. Poniższe kroki pokazują sposób tworzenia projektu badania obciążenia:

1. W menu skrótów wydajności sieci Web i badanie projektu, wybierz pozycję **Dodaj** > **Test ładowania**. W kreatorze **Ładowania testowanie** wybierz przycisk **Dalej** , aby skonfigurować ustawienia test.

2. W sekcji **Wzorzec ładowanie** wybierz opcję Załaduj stałej użytkownika lub obciążenia krok, który zaczyna się od kilku użytkowników i zwiększanie użytkowników w czasie.

    Jeśli masz dobrym oszacowaniem ilość ładowania użytkownika i chcesz zobaczyć, jak wykonuje bieżącego systemu, wybierz pozycję **Ładowanie stałej**. Jeśli celem jest, aby dowiedzieć się, czy system wykonuje spójne pod obciążeniem różnych, wybierz pozycję **Załaduj kroku**.

3. W sekcji **Testowanie Mix** wybierz przycisk **Dodaj** , a następnie wybierz pozycję test, który chcesz uwzględnić w teście ładowania. Kolumnę **rozkładu** umożliwia określić stopień testów sumy dla każdego testu.

4. W sekcji **Ustawienia uruchamiania** Określ czas trwania badania ładowania.

    >[AZURE.NOTE] Opcja **Testowanie iteracji** jest dostępna tylko wtedy, gdy testu obciążenia jest lokalnie, przy użyciu programu Visual Studio.

5. W sekcji **Lokalizacja** ustawień **Uruchamiania**Określ lokalizację miejsce, w którym są generowane obciążenie żądaniami testowych. Kreator może zostać wyświetlony komunikat Aby zalogować się do swojego konta usługi zespołu. Zaloguj się, a następnie wybierz pozycję lokalizacji geograficznej. Po zakończeniu wybierz przycisk **Zakończ** .

6. Po utworzeniu test ładowania Otwórz projekt .loadtest i wybierz pozycję bieżący element ustawienia, takie jak **Ustawienia Uruchom** > **Settings1 uruchomić [Active]**. Spowoduje to otwarcie ustawień uruchamiania w oknie **dialogowym właściwości** .

7. W sekcji **wyniki** w oknie **Uruchom ustawienia** właściwości ustawienie **Chronometraż szczegóły magazynowania** powinien mieć **Brak** jako wartość domyślną. Zmień tę wartość na **Wszystkich informacji o poszczególnych** , aby uzyskać więcej informacji na temat Załaduj wyniki testu. Aby uzyskać więcej informacji na temat nawiązać połączenie z programu Visual Studio Team Services i testu obciążenia, zobacz [Ładowanie testowania](https://www.visualstudio.com/load-testing.aspx) .

### <a name="run-the-load-test-by-using-visual-studio-team-services"></a>Uruchom test obciążenia przy użyciu programu Visual Studio Team Services

Wybierz polecenie **Uruchom testowanie obciążenia** , aby rozpocząć test uruchamianie.

![Zrzut ekranu: polecenie Uruchom Test ładowania][8]

## <a name="view-and-analyze-the-load-test-results"></a>Wyświetlanie i analizowanie wyników test ładowania

Co obciążenie testowanie wraz z postępem, informacje o wydajności jest graficznie. Powinien zostać wyświetlony podobnie do następujących wykresu.

![Zrzut ekranu przedstawiający wykres wydajności obciążenia wyniki testu][9]

1. Wybierz łącze **Pobierz raport** w górnej części strony. Po pobraniu raport, kliknij przycisk **Wyświetl raport** .

    Na karcie **Wykres** znajdują się wykresy dla różnych liczników wydajności. Na karcie **Podsumowanie** ogólnego wyniki testu zostaną wyświetlone. Karta **tabele** zawiera całkowitą liczbę badań ładowania przekazano i nie powiodło się.

2. Wybierz numer łącza w **Test** > **nie powiodło się** i **błędów** > **Liczba** kolumn, aby wyświetlić szczegóły błędu.

    Kartę **Szczegóły** zawiera wirtualnych test scenariusz informacji o użytkownikach i żądań zakończonych niepowodzeniem. Może to być przydatne, jeśli test ładowania zawiera wielu scenariuszy.

Aby uzyskać więcej informacji na temat wyświetlania wyników test ładowania, zobacz [Analizowanie ładowanie testowanie wyniki w widoku wykresy analizatora ładowanie testowanie](https://www.visualstudio.com/load-testing.aspx) .

## <a name="automate-your-load-test"></a>Automatyzowanie programu test ładowania

Visual Studio zespołu usługi ładowanie Test udostępnia interfejsy API ułatwiające zarządzanie testów obciążenia i analizowanie wyników na koncie usługi zespołu. Aby uzyskać więcej informacji, zobacz [API pozostałych testowania obciążenia chmury](http://blogs.msdn.com/b/visualstudioalm/archive/2014/11/03/cloud-load-testing-rest-apis-are-here.aspx) .

## <a name="next-steps"></a>Następne kroki
- [Monitorowanie i diagnozowania usług w ustawieniach rozwoju komputer lokalny](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[0]: ./media/service-fabric-vso-load-test/OverviewDiagram.png
[1]: ./media/service-fabric-vso-load-test/NewProjectDialog.png
[2]: ./media/service-fabric-vso-load-test/Project.png
[3]: ./media/service-fabric-vso-load-test/AddRecording.png
[4]: ./media/service-fabric-vso-load-test/AddRecording2.png
[5]: ./media/service-fabric-vso-load-test/ActionSequence.png
[6]: ./media/service-fabric-vso-load-test/StopRecording.png
[7]: ./media/service-fabric-vso-load-test/RunTest.png
[8]: ./media/service-fabric-vso-load-test/RunTest2.png
[9]: ./media/service-fabric-vso-load-test/Graph.png
