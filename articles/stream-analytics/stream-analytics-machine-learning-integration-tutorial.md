<properties
    pageTitle="Analiza upodobania przy użyciu analizy strumieniu Azure i nauka maszynowego Azure | Microsoft Azure"
    description="Jak używać funkcji zdefiniowanych przez użytkownika i nauka komputera w zadaniu analizy strumieniu"
    keywords=""
    documentationCenter=""
    services="stream-analytics"
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"
/>


<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="10/04/2016" 
    ms.author="jeffstok"
/>

# <a name="sentiment-analysis-by-using-azure-stream-analytics-and-azure-machine-learning"></a>Analiza upodobania przy użyciu analizy strumieniu Azure i nauka maszynowego Azure #

Ten artykuł jest przeznaczony pozwala szybko ustalić prostych zadań analizy strumieniu Azure, z integracji Azure maszynowego uczenia. Firma Microsoft będzie analizowanie przesyłanie strumieniowe danych tekstowych przy użyciu modelu upodobania analizy nauki komputera z galerii analizy Cortana, a następnie określić wynik upodobania w czasie rzeczywistym. Informacje w tym artykule może pomóc w scenariuszy, takich jak analizy w czasie rzeczywistym upodobania na strumieniowego przesyłania danych Twitter, analizowanie rekordy klientów rozmowy z pracowników pomocy technicznej i ocenić komentarzy do forów, blogów i klipy wideo, oprócz wielu innych w czasie rzeczywistym, przewidywanych wyników scenariuszach.

Ten artykuł zawiera przykładowy plik CSV z tekstem jako dane wejściowe w magazynie obiektów Blob platformy Azure, pokazano na poniższej ilustracji. Zadanie dotyczy modelu analizy upodobania jako funkcja zdefiniowana przez użytkownika (UDF) na przykładowych danych tekst z magazynu obiektów blob. Wynik końcowy zostanie umieszczony w tym samym magazynie obiektów blob w innym pliku CSV. 

![Analizy strumieniu komputera szkoleniowe](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-figure-2.png)  

Poniższa ilustracja przedstawia tej konfiguracji. Bliższy scenariusza możesz zastąpić magazyn obiektów Blob strumieniowego przesyłania danych Twitter z wprowadzania koncentratory zdarzenia Azure. Ponadto można utworzyć wizualizacją w czasie rzeczywistym programu [Microsoft Power BI](https://powerbi.microsoft.com/) upodobania agregacji.    

![Analizy strumieniu komputera szkoleniowe](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-figure-1.png)  

## <a name="prerequisites"></a>Wymagania wstępne

Wymagania wstępne dotyczące wykonywania zadań, które przedstawiono w tym artykule są następujące:

-   Aktywną subskrypcję Azure.
-   Pliku CSV zawierającego dane w nim. Mogą pobrać plik pokazano na rysunku 1 z [GitHub](https://github.com/Azure/azure-stream-analytics/blob/master/Sample Data/sampleinput.csv), lub możesz utworzyć własny plik. W tym artykule przyjęto założenie, że używasz jednego dostępne do pobrania z GitHub.

Na wysokim poziomie do wykonania zadań pokazano w tym artykule, wykonaj następujące czynności:

1.  Przekaż plik CSV wprowadzania magazynem obiektów Blob platformy Azure.
2.  Dodawanie modelu analizy upodobania z galerii analizy Cortana do obszaru roboczego Azure maszynowego uczenia.
3.  Wdrażanie tego modelu jako usługi sieci web w obszarze roboczym nauki komputera.
4.  Utwórz zadanie analizy strumieniu połączeń ta usługa sieci web jako funkcja, aby określić upodobania wprowadzania tekstu.
5.  Uruchom zadanie analizy strumieniu i obserwować wynik.

## <a name="upload-the-csv-input-file-to-blob-storage"></a>Przekazywanie pliku CSV wprowadzania magazynem obiektów Blob

W tym kroku możesz użyć dowolnego pliku CSV, takich jak schemat już określony jako dostępne do pobrania w GitHub. Możesz przekazać plik za pomocą [Eksploratora magazynu Azure](http://storageexplorer.com/) lub Visual Studio, lub możesz użyć kodu niestandardowego. Firma Microsoft korzysta z przykładami według programu Visual Studio.

1.  W programie Visual Studio, kliknij pozycję **Azure** > **miejsca do magazynowania** > **Dołączyć pamięci zewnętrznej**. Wprowadź **nazwę konta** i **klucz konta**.  

    ![Analizy strumieniu komputera nauki Eksploratora serwera](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-server-explorer.png)  

2.  Rozwijanie przestrzeni dyskowej, które dołączone w kroku 1, kliknij przycisk **Utwórz kontenera obiektów Blob**, a następnie wprowadź nazwę logiczną. Po utworzeniu kontenera, otwórz go, aby wyświetlić jego zawartość. (Będzie on pusty w tym momencie).  

    ![Strumienia analizy maszynowego uczenia, tworzenie obiektów blob](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-create-blob.png)  

3.  Aby przekazać plik CSV, kliknij pozycję **Przekaż obiektów Blob**, a następnie kliknij **plik z dysku**.  

## <a name="add-the-sentiment-analytics-model-from-the-cortana-intelligence-gallery"></a>Dodawanie modelu analizy upodobania z galerii analizy Cortana

1.  Pobieranie [modelu analizy przewidywanych upodobania](https://gallery.cortanaintelligence.com/Experiment/Predictive-Mini-Twitter-sentiment-analysis-Experiment-1) z galerii analizy Cortana.  
2.  W Studio nauki komputera kliknij przycisk **Otwórz w Studio**.  

    ![Przesyłanie strumieniowe analizy maszynowego szkoleniowe, otwórz Studio nauki komputera](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-open-ml-studio.png)  

3.  Zaloguj się przejść do obszaru roboczego. Wybierz lokalizację, który najlepiej pasuje własnej lokalizacji.
4.  Kliknij przycisk **Uruchom** w dolnej części strony.  
5.  Po pomyślnym uruchomieniu procesu kliknij **Wdrażanie usługi sieci Web**.
6.  Model analizy upodobania jest gotowy do użycia. Aby sprawdzić poprawność, kliknij przycisk **Testuj** i podaj wprowadzania tekstu, takie jak "Świetnie Microsoft". Test należy zwracać wyniku podobny do następującego:

`'Predictive Mini Twitter sentiment analysis Experiment' test returned ["4","0.715057671070099"]...`  

![Nauki komputera analizy strumieniu, analizy danych](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-analysis-data.png)  

W kolumnie **aplikacje** kliknij łącze dotyczące **programu Excel 2010 lub wcześniejszej skoroszytu** uzyskać klucz interfejsu API i adres URL, który musisz później skonfigurować zadanie analizy strumieniu. (Ten krok jest wymagany tylko do używania modelu nauki komputera z innego konta Azure obszaru roboczego. W tym artykule założono, że jest to, aby rozwiązać tego scenariusza.)  

Zwróć uwagę klucza usługi sieci web adres URL i dostęp z pobranego pliku programu Excel, tak jak pokazano poniżej:  

![Nauki komputera analizy strumieniu, szybko dowiedzieć się](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-quick-glance.png)  

## <a name="create-a-stream-analytics-job-that-uses-the-machine-learning-model"></a>Tworzenie zadania analizy strumieniu korzystającego modelu nauki komputera

1.  Przejdź do [portalu Azure](https://manage.windowsazure.com).  
2.  Kliknij przycisk **Nowy** > **usług danych** > **analizy strumieniu** > **Szybkie tworzenie**. Wprowadź nazwę dla zadania w polu **Nazwa zadania**, wprowadź odpowiedni region dla zadania w **regionie**, a następnie wybierz konto na **Koncie regionalne monitorowanie miejsca do magazynowania**.    
3.  Po utworzeniu zadania, na karcie **danych wejściowych** , kliknij przycisk **Dodaj dane wejściowe**.  

    ![Przesyłanie strumieniowe analizy maszynowego uczenia, Dodaj wprowadzania nauki komputera](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-add-input-screen.png)  

4.  Na pierwszej stronie kreatora **Dodawanie wprowadzania** kliknij **strumienia danych**, a następnie kliknij przycisk **Dalej**. Na następnej stronie kliknij pozycję **Magazyn obiektów Blob** jako danych wejściowych, a następnie kliknij przycisk **Dalej**.  
5.  Na stronie **Ustawienia magazyn obiektów Blob** kreatora podaj nazwę konta magazynu obiektów blob kontenera zdefiniowanych wcześniej po przekazaniu danych. Kliknij przycisk **Dalej**. **Format szeregowania zdarzenia**kliknij pozycję **CSV**. Zaakceptuj wartości domyślne dla pozostałych na stronie **Ustawienia szeregowania** . Kliknij **przycisk OK**.  
6.  Na karcie **wyników** kliknij przycisk **Dodaj wynik**.  

    ![Strumienia analizy maszynowego uczenia, Dodaj format wyjściowy](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-add-output-screen.png)  

7.  Kliknij pozycję **Magazyn obiektów Blob**, a następnie wprowadź te same parametry, z wyjątkiem kontenerze. Wartość w polu **wprowadzania** został skonfigurowany do odczytu z kontenera o nazwie "test" miejsce, w którym przekazano pliku **CSV** . **Dane wyjściowe**wprowadź wartość "testoutput". Kontener nazwy muszą być różne. Sprawdź, czy istnieje ten kontener.     
8.  Kliknij przycisk **Dalej** na konfigurowanie wyjściowa **szeregowania ustawienia**. Podobnie jak w przypadku **wprowadzania**kliknij **CSV**, a następnie kliknij przycisk **OK** .
9.  Na karcie **Funkcje** kliknij przycisk **Dodaj funkcji nauki komputera**.  

    ![Strumienia analizy maszynowego uczenia, dodać funkcję nauki komputera](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-add-ml-function.png)  

10. Na stronie **Ustawienia usługi sieci Web uczenia maszynowego** Znajdź obszar roboczy maszynowego uczenia, usługi sieci web i domyślny punkt końcowy. W tym artykule należy zastosować ustawienia przejęcie ręcznie znajomości konfigurowania usługi sieci web dla dowolnego obszaru roboczego, dopóki znasz adres URL i masz klucz interfejsu API. Wprowadź **klucz interfejsu API**i punkt końcowy **adres URL** . Kliknij **przycisk OK**.    

    ![Nauka maszynowego analizy strumieniu, nauki komputera usługi sieci web](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-ml-web-service.png)    

11. Na karcie **Kwerenda** zmodyfikuj kwerendę w następujący sposób:    

 ```
    WITH subquery AS (  
        SELECT text, sentiment(text) as result from input  
    )  
 
    Select text, result.[Scored Labels]  
    Into output  
    From subquery  
 ```    
12. Kliknij przycisk **Zapisz** , aby zapisać kwerendę.

## <a name="start-the-stream-analytics-job-and-observe-the-output"></a>Uruchom zadanie analizy strumieniu i obserwować wynik

1.  Kliknij przycisk **Start** , w dolnej części strony zadania.
2.  W oknie **Uruchom okno dialogowe kwerendy**kliknij pozycję **Termin niestandardowe**, a następnie wybierz czas przed po przekazaniu CSV z magazynem obiektów Blob. Kliknij **przycisk OK**.  

    ![Nauka maszynowego analizy strumieniu, czas niestandardowe](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-custom-time.png)  

3.  Przejdź do magazyn obiektów Blob za pomocą narzędzia użytego do przekazania pliku CSV, na przykład programu Visual Studio.
4.  Kilka minut po rozpoczęciu zadania jest tworzony kontener dane wyjściowe i przekazaniu pliku CSV do niego.  
5.  Otwórz plik w edytorze CSV domyślne. Powinny być wyświetlane podobną do następującej:  

    ![Wyświetlanie nauki maszynowego analizy strumieniu, CSV](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-csv-view.png)  

## <a name="conclusion"></a>Wnioski

W tym artykule przedstawiono sposób utworzyć zadanie analizy strumieniu odczytuje przesyłanie strumieniowe danych tekstowych, które dotyczą upodobania analizy danych w czasie rzeczywistym. Można wykonywać wszystko to bez konieczności martwić mogli dokładnie zapoznać tworzenia modelu analizy upodobania. To jest jedną z zalet pakietu analizy Cortana.

To również wyświetlić metryki związane z funkcji Azure maszynowego uczenia. Aby to zrobić, kliknij kartę **Monitor** . Wyświetlane są trzy metryki związane z funkcji.  

- **Funkcja żądania** wskazuje liczbę wysyłanych do usługi sieci web uczenia komputera.  
- **Funkcja zdarzenia** wskazuje liczbę zdarzeń w wezwaniu na. Domyślnie każde żądanie usługi sieci web uczenia komputera zawiera zdarzenia do 1000.  
- **Niepowodzenie żądania funkcji** wskazuje liczba zakończonych niepowodzeniem żądań do nauki komputera usługi sieci web.  

    ![Nauka maszynowego analizy strumieniu, maszynowego uczenia monitorowanie widoku](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-ml-monitor-view.png)  
