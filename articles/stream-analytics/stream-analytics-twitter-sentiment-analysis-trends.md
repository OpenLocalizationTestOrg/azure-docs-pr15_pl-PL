<properties
    pageTitle="W czasie rzeczywistym analizy upodobania Twitter przy użyciu analizy strumieniu | Microsoft Azure"
    description="Dowiedz się, jak za pomocą analizy strumieniu analizy w czasie rzeczywistym upodobania Twitter. Instrukcje krok po kroku z generowanie zdarzeń do danych na żywo pulpitu nawigacyjnego."
    keywords="w czasie rzeczywistym twitter analizy trendu, upodobania analizy, analizy społecznościowych, przykład analizy trendu."
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>


# <a name="social-media-analysis-real-time-twitter-sentiment-analysis-in-azure-stream-analytics"></a>Analiza społecznościowych: w czasie rzeczywistym serwisu Twitter analizy upodobania w analizy strumieniu Azure

Dowiedz się, jak tworzyć rozwiązania do analizowania upodobania analiz społecznościowych dzięki w czasie rzeczywistym zdarzeń Twitter koncentratory zdarzenia Azure. Będzie Napisz kwerendę Azure analizy strumieniu do analizy danych. Możesz zostanie następnie przechowywać wyniki dla perusal nowszy albo umożliwia uzyskanie wniosków w czasie rzeczywistym pulpitu nawigacyjnego i [Usługi Power BI](https://powerbi.com/) .

Organizacje opis popularnych tematów, oznacza to, że tematy i postaw zawierających dużą liczbę wpisów w społecznościowych pomocy narzędzi analizy społecznościowych. Analiza upodobania jest nazywany *wyszukiwania opinię*, używa narzędzia analizy media społecznościowe, aby określić postaw kierunku produktu, pomysłów i tak dalej. W czasie rzeczywistym analizy trendu Twitter jest doskonałe przykład, ponieważ subskrypcji hashtagu umożliwia odsłuchiwanie określonych słów kluczowych i opracowanie analizy upodobania w kanale.

## <a name="scenario-sentiment-analysis-in-real-time"></a>Scenariusz: Analiza upodobania w czasie rzeczywistym

Firma, która ma witrynę internetową multimediów wiadomości jest dotyczącymi zaletą w stosunku do jego konkurentów za wyposażony w zawartości witryny, od razu zgodnych z jego czytników. Analiza społecznościowych są używane w firmie na tematy, które są odpowiednie dla czytelników, wykonując w czasie rzeczywistym upodobania analizy danych Twitter. W szczególności aby zidentyfikować popularnych tematy w czasie rzeczywistym w serwisie Twitter, firmy musi w czasie rzeczywistym analizy o wielkości tweet i upodobania tematy klucza. Tak w zasadzie jest aparat analizy analizy upodobania, oparty na tym społecznościowych kanału.

## <a name="prerequisites"></a>Wymagania wstępne
-   Serwisu Twitter konta i [token dostępu OAuth](https://dev.twitter.com/oauth/overview/application-owner-access-tokens)
-   [TwitterClient.zip](http://download.microsoft.com/download/1/7/4/1744EE47-63D0-4B9D-9ECF-E379D15F4586/TwitterClient.zip) z Centrum pobierania Microsoft
-   Opcjonalnie: Kod źródłowy twitter klienta z [GitHub](https://aka.ms/azure-stream-analytics-twitterclient)

## <a name="create-an-event-hub-input-and-a-consumer-group"></a>Tworzenie Centrum zdarzeń wprowadzania i grupę dla klientów indywidualnych

Przykładowa aplikacja będzie generowanie zdarzeń i przekazać je do wystąpienia zdarzenia koncentratory (zdarzenia Centrum, krótka). Koncentratory zdarzenia usługi Bus są preferowanej metody spożyciu zdarzeń dla analizy strumieniu. Zobacz dokumentację koncentratory zdarzenia w [dokumentacji usługi Bus](/documentation/services/service-bus/).

Umożliwia utworzenie Centrum zdarzeń następujące czynności.

1.  W portalu Azure, kliknij przycisk **Nowy** > **Aplikacji usług** > **BUS usługi** > **Centrum zdarzenia** > **Szybkie tworzenie**i podaj nazwę, region i nowym lub istniejącym nazw.  
2.  Zgodnie z zaleceniami dotyczącymi każdego zadania analizy strumieniu powinni zapoznać się z jednej grupy dla klientów indywidualnych koncentratory zdarzenia. Firma Microsoft przeprowadzi Cię przez proces tworzenia grupy dla klientów indywidualnych później. Więcej o grupach dla klientów indywidualnych na [Przegląd koncentratory zdarzenia Azure](https://msdn.microsoft.com/library/azure/dn836025.aspx)można znaleźć. Aby utworzyć grupę dla klientów indywidualnych, przejdź do Centrum nowo utworzonego zdarzenia, kliknij kartę **Grupy dla klientów indywidualnych** , kliknij przycisk **Utwórz** w dolnej części strony, a następnie podaj nazwę dla grupy dla klientów indywidualnych.
3.  Aby udzielić dostępu do koncentratora zdarzenia, potrzebujemy Tworzenie zasad udostępniania. Kliknij kartę **Konfigurowanie** Twoim Centrum zdarzenia.
4.  W obszarze **Zasady dostępu UDOSTĘPNIONE**należy utworzyć nową zasadę **Zarządzanie** uprawnieniami.

    ![Udostępnione miejsce, w którym można utworzyć zasadę z uprawnieniami Zarządzanie zasadami dostępu.](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-ananlytics-shared-access-policies.png)

5.  Kliknij pozycję **ZAPISZ** u dołu strony.
6.  Przejdź do **pulpitu NAWIGACYJNEGO**, kliknij pozycję **Informacje o połączeniu** w dolnej części strony, a następnie skopiuj i zapisywanie informacji o połączeniu. (Użyj ikonę Kopiuj, która będzie wyświetlana pod ikoną wyszukiwania).

## <a name="configure-and-start-the-twitter-client-application"></a>Konfigurowanie i rozpoczynanie z aplikacją kliencką Twitter

Utworzono aplikacji klienckiej, który będzie łączyć się Twitter danych za pośrednictwem [Interfejsów API w serwisie Twitter Streaming](https://dev.twitter.com/streaming/overview) zbieranie zdarzeń Tweet o parametryczne zestaw tematów. Narzędzie Otwórz źródło [Sentiment140](http://help.sentiment140.com/) umożliwia przypisanie wartości upodobania do każdego tweet.

- 0 = ujemne
- 2 = neutralność
- 4 = dodatnia

Następnie w serwisie Twitter zdarzenia są przenoszone do Centrum zdarzenia.  

Wykonaj poniższe czynności, aby skonfigurować aplikację:

1.  [Pobierz rozwiązanie TwitterClient](http://download.microsoft.com/download/1/7/4/1744EE47-63D0-4B9D-9ECF-E379D15F4586/TwitterClient.zip).
2.  Otwórz TwitterClient.exe.config i zastąpić tokeny Twitter, które mają wartości oauth_consumer_key, oauth_consumer_secret, oauth_token i oauth_token_secret.  

    [Czynności, aby wygenerować token dostępu OAuth](https://dev.twitter.com/oauth/overview/application-owner-access-tokens)  

    Należy zauważyć, że należy wprowadzić pustą aplikację do wygenerowania tokenu.  
3.  Zamień wartości EventHubConnectionString i EventHubName w TwitterClient.exe.config parametry połączenia i nazwę swojej Centrum zdarzeń. Parametry połączenia, który został skopiowany wcześniej umożliwia zarówno parametry połączenia, jak i nazwa Twoim Centrum zdarzenia, dlatego należy je rozdzielić i umieścić je w polu poprawne. Na przykład można rozważyć następujący ciąg połączenia:

        Endpoint=sb://your.servicebus.windows.net/;SharedAccessKeyName=yourpolicy;SharedAccessKey=yoursharedaccesskey;EntityPath=yourhub

    Plik TwitterClient.exe.config powinien zawierać ustawień, tak jak w poniższym przykładzie:

        add key="EventHubConnectionString" value="Endpoint=sb://your.servicebus.windows.net/;SharedAccessKeyName=yourpolicy;SharedAccessKey=yoursharedaccesskey"
        add key="EventHubName" value="yourhub"

    Należy zauważyć, że tekst "EntityPath =" czy __nie__ są wyświetlane w wartości EventHubName.

4.  *Opcjonalne:* Dostosowywanie słów kluczowych w celu wyszukiwania.  Domyślnie ta aplikacja wyszukuje "Azure, Skype, konsoli XBox, Microsoft, Seattle".  Możesz dostosować wartości **twitter_keywords** w TwitterClient.exe.config, w razie potrzeby.
5.  Uruchom TwitterClient.exe Aby uruchomić aplikację. Zostanie wyświetlona zdarzenia Tweet wartościami **CreatedAt**, **temacie**i **SentimentScore** wysyłane do Twojej Centrum zdarzeń.

    ![Analiza upodobania: wartości SentimentScore wysyłane do koncentratora zdarzenia.](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-twitter-sentiment-output-to-event-hub.png)

## <a name="create-a-stream-analytics-job"></a>Utwórz zadanie analizy strumieniu

Teraz, gdy zdarzeń Tweet strumieniowe w czasie rzeczywistym z Twitter, możemy Skonfiguruj zadanie analizy strumieniu analizowanie tych zdarzeń w czasie rzeczywistym.

### <a name="provision-a-stream-analytics-job"></a>Obsługa administracyjna zadanie analizy strumieniu

1.  W [portalu Azure](https://manage.windowsazure.com/), kliknij przycisk **Nowy** > **Usług danych** > **Analizy STRUMIENIU** > **Szybkiego tworzenia**.
2.  Określ następujące wartości, a następnie kliknij polecenie **Utwórz zadanie analizy STRUMIENIU**:

    * **Nazwa zadania**: Wprowadź nazwę zadania.
    * **REGION**: Wybierz region, w którym chcesz uruchomić zadanie. Należy rozważyć, umieszczając to zadanie i Centrum zdarzeń w tym samym regionie, aby zapewnić lepszą wydajność i upewnij się, że użytkownik będzie nie można opłaty do przenoszenia danych między regionami.
    * **Konto miejsca do magazynowania**: Wybierz konto Azure miejsca do magazynowania, którego chcesz używać do przechowywania danych monitorowania dla wszystkich zadań analizy strumieniu uruchamiane w tym regionie. Można wybrać, aby wybrać istniejącego konta miejsca do magazynowania lub Utwórz nowy.

3.  Kliknij pozycję **Analizy STRUMIENIU** w okienku po lewej stronie, aby wyświetlić zadania analizy strumieniu.  
    ![Ikona usługi analizy strumieniu](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-service-icon.png)

    Nowe zadanie pojawi się ze stanem **UTWORZONE**. Zwróć uwagę, że przycisk **START** na koniec strony jest wyłączona. Przed rozpoczęciem tego zadania musisz skonfigurować zadania wejście, dane wyjściowe i kwerendy.


### <a name="specify-job-input"></a>Określanie zadania wejście
1.  W swojej pracy analizy strumieniu kliknij **wartości wejściowych** w górnej części strony, a następnie kliknij **Dodawanie danych wejściowych**. Zostanie otwarte okno dialogowe przeprowadzi Cię przez kilka czynności, aby zdefiniować zakres wejściowy.
2.  Kliknij **strumienia danych**, a następnie kliknij przycisk prawy.
3.  Kliknij pozycję **Centrum zdarzenia**, a następnie kliknij przycisk prawy.
4.  Wpisz lub wybierz następujące wartości na trzeciej stronie:

    * **ALIAS wprowadzania**: Wprowadź przyjazną nazwę dla tego zadania do wprowadzania, takich jak *TwitterStream*. Należy zauważyć, że użyjesz tej nazwy w kwerendzie później.
    **Centrum zdarzenia**: w przypadku koncentratora zdarzenia, który został utworzony w tej samej subskrypcji jako zadanie analizy strumieniu, zaznacz obszar nazw, należącym do Centrum zdarzenia.

        Jeśli Twoim Centrum zdarzeń znajduje się w innej subskrypcji, kliknij pozycję **Centrum zdarzeń korzystanie z innej subskrypcji**i ręcznie wprowadź informacje dotyczące **Usługi BUS nazw**, **Nazwa centrum zdarzenia**, **Nazwa zasady Centrum zdarzenia**, **Klucz zasad Centrum zdarzenia**i **Liczba PARTITION Centrum zdarzeń**.

    * **Nazwa centrum zdarzenia**: Wybierz nazwę Centrum zdarzenia.

    * **Nazwa zasady Centrum zdarzenia**: Wybierz zasadę Centrum zdarzenia utworzonego wcześniej w tym samouczku.

    * **GRUPĘ dla klientów indywidualnych Centrum zdarzeń**: wpisz nazwę grupy dla klientów indywidualnych, utworzony wcześniej w tym samouczku.
5.  Kliknij przycisk prawy.
6.  Określ następujące wartości:

    * **FORMAT SERIALIZATORA zdarzenia**: JSON
    * **KODOWANIE**: UTF8

7.  Kliknij przycisk **Sprawdź** , aby dodać to źródło i sprawdzić, czy analizy strumieniu pomyślnie łączy do koncentratora zdarzenia.

### <a name="specify-job-query"></a>Określanie zapytania zadania

Analizy strumieniu obsługuje model prostej kwerendy deklaracyjnych opisującą przekształcenia. Aby dowiedzieć się więcej na temat języka, zobacz [Dokumentacja języka kwerend analizy strumieniu Azure](https://msdn.microsoft.com/library/azure/dn834998.aspx).  Ten samouczek pomoże Ci autor i testowanie kilku kwerend nad danymi Twitter.

#### <a name="sample-data-input"></a>Przykładowe dane

Aby sprawdzić poprawność kwerendy rzeczywistej pracy danych, funkcja **PRZYKŁADOWYCH danych** służy do wyodrębniania strumienia zdarzeń i utworzyć plik .json zdarzeń do testowania.

1.  Zaznacz zakres wejściowy Centrum zdarzenia, a następnie kliknij **Przykładowe dane** w dolnej części strony.
2.  W wyświetlonym oknie dialogowym Określ **czas rozpoczęcia** zacząć zbierania danych i **czasu trwania** na używanie ilość dodatkowe dane.
3.  Kliknij przycisk **Szczegóły** , a następnie kliknij łącze **kliknij tutaj** pobrać i Zapisz plik wygenerowane .json.

#### <a name="pass-through-query"></a>Kwerenda przekazująca
Aby rozpocząć, firma Microsoft nie wywołuje prostej kwerendy przekazującej tego wszystkie pola w przypadku projektów.

1.  Kliknij przycisk **kwerendy** w górnej części strony zadania analizy strumieniu.
2.  W edytorze kodu zamienić szablon zapytania początkowego następujące czynności:

        SELECT * FROM TwitterStream

    Upewnij się, że nazwa źródła danych wejściowych odpowiada nazwie wprowadzenia określonej wcześniej.

3.  Kliknij przycisk **Testuj** w edytorze zapytań.
4.  Przejdź do pliku .json próbki.
5.  Kliknij przycisk **Sprawdź** , a wyniki są wyświetlane poniżej definicją zapytania.

    ![Wyniki wyświetlane poniżej definicji zapytania](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-sentiment-by-topic.png)

#### <a name="count-of-tweets-by-topic-tumbling-window-with-aggregation"></a>Liczba tweety według tematu: okno Tumbling z agregacji

Aby porównać liczbę wzmianki między tematami, użyjemy [TumblingWindow](https://msdn.microsoft.com/library/azure/dn835055.aspx) pobrać liczby wzmianki według tematu co pięć sekund.

1.  Zmienianie kwerendy w edytorze kodu:

        SELECT System.Timestamp as Time, Topic, COUNT(*)
        FROM TwitterStream TIMESTAMP BY CreatedAt
        GROUP BY TUMBLINGWINDOW(s, 5), Topic

    Tej kwerendzie użyto kluczowego **Sygnatury czasowej** do określenia pola sygnatury czasowej w ładunku może być używany do obliczenia czasowy. Jeśli to pole nie zostanie określony, będzie można wykonać operacji Obsługa okien przy użyciu podczas każdego zdarzenia dotarła do Centrum zdarzenia.  Dowiedz się, jak w sekcji "Nadejście czasu w porównaniu z aplikacji Time" [Odwołanie do kwerendy analizy strumieniu](https://msdn.microsoft.com/library/azure/dn834998.aspx).

    Ta kwerenda również uzyskuje dostęp do sygnaturę czasową na koniec każdego okna przy użyciu właściwości **System.Timestamp** .

2.  W edytorze zapytań, aby wyświetlić wyniki kwerendy, kliknij przycisk **Uruchom ponownie** .

#### <a name="identify-trending-topics-sliding-window"></a>Identyfikowanie popularnych tematy: przesuwania okna

Aby zidentyfikować popularnych tematów, opisano tematy, które krzyżowe wartość progu dla wzmianek w określonym czasie. Na potrzeby tego samouczka firma Microsoft będzie Wyszukaj tematy, które wymieniono więcej niż 20 razy w ciągu ostatnich pięciu sekund przy użyciu [SlidingWindow](https://msdn.microsoft.com/library/azure/dn835051.aspx).

1.  Zmienianie kwerendy w edytorze kodu do: wybierz pozycję System.Timestamp jako czas, temat, liczba (*) jako wzmianki z TwitterStream sygnatura CZASOWA przez CreatedAt grupy przez SLIDINGWINDOW(s, 5), temat konieczności ZLICZANIA (*) > 20

2.  W edytorze zapytań, aby wyświetlić wyniki kwerendy, kliknij przycisk **Uruchom ponownie** .

    ![Przesuwanie okna wyników kwerendy](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-query-output.png)

#### <a name="count-of-mentions-and-sentiment-tumbling-window-with-aggregation"></a>Liczba wzmianek i upodobania: okno Tumbling z agregacji
Ostateczne zapytanie, które będą testowania używa **TumblingWindow** w celu uzyskania liczby wzmianki, średnia, minimum, maksimum i odchylenie standardowe upodobania wynik dla każdego tematu co pięć sekund.

1.  Zmienianie kwerendy w edytorze kodu:

        SELECT System.Timestamp as Time, Topic, COUNT(*), AVG(SentimentScore), MIN(SentimentScore),
        Max(SentimentScore), STDEV(SentimentScore)
        FROM TwitterStream TIMESTAMP BY CreatedAt
        GROUP BY TUMBLINGWINDOW(s, 5), Topic

2.  W edytorze zapytań, aby wyświetlić wyniki kwerendy, kliknij przycisk **Uruchom ponownie** .
3.  To jest kwerendę, która użyjemy dla naszych pulpitu nawigacyjnego.  Kliknij pozycję **ZAPISZ** u dołu strony.


## <a name="create-output-sink"></a>Tworzenie sink wyjścia

Teraz, gdy zdefiniowany strumienia zdarzenia koncentratora zdarzenia dane wejściowe mogły zjeść tej ostatniej zdarzeń i kwerenda przekształcenie nad strumienia, ostatnim krokiem jest określenie sink wynik dla zadania.  Firma Microsoft będzie zapisywać zdarzenia zagregowane tweet z naszych kwerendy zadania magazyn obiektów Blob platformy Azure.  Można również przekazać wyniki do Azure SQL Database, Magazyn tabel platformy Azure, lub biegami zdarzenia, w zależności od konkretnej aplikacji wymaga.

Aby utworzyć kontener magazyn obiektów Blob wykonaj następujące czynności, jeśli nie masz jeszcze jedną:

1.  Użyj istniejącego konta miejsca do magazynowania lub Utwórz nowe konto miejsca do magazynowania, klikając przycisk **Nowe** > **Usług danych** > **miejsca do magazynowania** > **Szybkiego tworzenia**, a następnie zgodnie z instrukcjami wyświetlanymi na ekranie.
2.  Wybierz konto miejsca do magazynowania, **kontenerów** w górnej części strony kliknij, a następnie kliknij przycisk **Dodaj**.
3.  Określ **nazwę** dla swojego kontenera i ustaw **dostęp** do **Publicznych obiektów Blob**.

## <a name="specify-job-output"></a>Określ format wyjściowy zadania

1.  W swojej pracy analizy strumieniu kliknij przycisk **dane wyjściowe** w górnej części strony, a następnie kliknij **Dodaj dane wyjściowe**. Zostanie otwarte okno dialogowe przeprowadzi Cię przez kilka kroków konfigurowania danych wyjściowych.
2.  Kliknij pozycję **Magazyn obiektów BLOB**, a następnie kliknij przycisk prawym.
3.  Wpisz lub wybierz następujące wartości na trzeciej stronie:

    * **Wyjściowy ALIAS**: wpisz przyjazną nazwę dla tego raportu zadania.

    * **SUBSKRYPCJI**: Jeśli magazyn obiektów Blob, który został utworzony w tej samej subskrypcji jako zadanie analizy strumieniu, kliknij przycisk **Użyj konta miejsca do magazynowania z bieżącej subskrypcji**. Jeśli magazyn znajduje się w innej subskrypcji, kliknij pozycję **Użyj konta miejsca do magazynowania z innej subskrypcji**i ręcznie wprowadź informacje dotyczące **Konta miejsca do magazynowania**, **Klucz konta miejsca do magazynowania**i **KONTENER**.

    * **Konto miejsca do magazynowania**: Wybierz nazwę konta magazynu.

    * **KONTENER**: Wybierz nazwę kontenera.

    * **PREFIKS nazwy pliku**: wpisz prefiks pliku używana podczas zapisywania danych wyjściowych obiektów blob.

4.  Kliknij przycisk prawy.
5.  Określ następujące wartości:
    * **FORMAT SERIALIZATORA zdarzenia**: JSON
    * **KODOWANIE**: UTF8
6.  Kliknij przycisk **Sprawdź** , aby dodać to źródło i sprawdzić, czy analizy strumieniu pomyślnie łączy do rachunku miejsca do magazynowania.

## <a name="start-job"></a>Rozpoczęcie zadania

Ponieważ zadania wejście, kwerendy i wyjście wszystkich określono, możemy są gotowe do rozpoczęcia zadania analizy strumieniu.

1.  Zadania **pulpitu NAWIGACYJNEGO**kliknij pozycję **Uruchom** w dolnej części strony.
2.  W wyświetlonym oknie dialogowym kliknij **czas rozpoczęcia zadania**, a następnie kliknij przycisk **Sprawdź** u dołu okna dialogowego. Stan zadania zostanie zmieniony na **Uruchamianie** i wkrótce zostanie zmieniona na **uruchomiony**.


## <a name="view-output-for-sentiment-analysis"></a>Widok danych wyjściowych upodobania analizy

Po pracy jest uruchomiony i przetwarzania w czasie rzeczywistym strumienia Twitter, wybierz, jak chcesz wyświetlić wyniki analizy upodobania. Narzędzie, takie jak [Eksplorator magazynu Azure](https://azurestorageexplorer.codeplex.com/) lub [Eksploratora Azure](http://www.cerebrata.com/products/azure-explorer/introduction) umożliwia wyświetlanie danych wyjściowych zadania w czasie rzeczywistym. W tym miejscu możesz rozszerzyć aplikację, aby uwzględnić dostosowane pulpitu nawigacyjnego, taki jak przedstawiony w następujących zrzut ekranu za pomocą [Usługi Power BI](https://powerbi.com/) .

![Analiza społecznościowych: dane wyjściowe (Opinia wyszukiwania) analizy upodobania analizy strumieniu na pulpicie nawigacyjnym Power BI.](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-output-power-bi.png)

## <a name="get-support"></a>Uzyskiwanie pomocy technicznej
Aby uzyskać dodatkową pomoc spróbuj nasze [forum Azure analizy strumieniu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).


## <a name="next-steps"></a>Następne kroki

- [Wprowadzenie do analizy strumieniu Azure](stream-analytics-introduction.md)
- [Wprowadzenie do korzystania z analizy strumieniu Azure](stream-analytics-get-started.md)
- [Skalowanie Azure analizy strumieniu zadania](stream-analytics-scale-jobs.md)
- [Dokumentacja języka kwerend analizy strumieniu Azure](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Strumień Azure analizy zarządzania pozostałych interfejsu API odwołania](https://msdn.microsoft.com/library/azure/dn835031.aspx)
