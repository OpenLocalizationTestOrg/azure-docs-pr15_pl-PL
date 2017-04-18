<properties
    pageTitle="Analizy strumieniu: Wykrywanie w czasie rzeczywistym oszustwa | Microsoft Azure"
    description="Dowiedz się, jak utworzyć rozwiązanie wykrywania oszustwa w czasie rzeczywistym przy użyciu analizy strumieniu. Używanie Centrum zdarzeń dla przetwarzania w czasie rzeczywistym."
    keywords="wykrywania anomalii, wykrywania oszustwa, wykrywania anomalii w czasie rzeczywistym"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok" />



# <a name="get-started-using-azure-stream-analytics-real-time-fraud-detection"></a>Wprowadzenie do korzystania z platformy Azure analizy strumieniu: wykrywanie oszustwa w czasie rzeczywistym

Dowiedz się, jak utworzyć rozwiązanie zakończenia do końca wykrywania oszustwa w czasie rzeczywistym przy użyciu analizy strumieniu Azure. Dostosowania wydarzenia do koncentratorów zdarzenia Azure, pisać zapytania analizy strumieniu dla agregacji lub alertu i przesyła wyniki do sink dane wyjściowe uzyskanie wniosków nad danymi przetwarzania w czasie rzeczywistym. Wykrywania anomalii w czasie rzeczywistym dla telekomunikacyjnych zostało wyjaśnione, ale metoda przykład jednakowo jest odpowiedni dla innych typów wykrywania oszustwa, takich jak karty kredytowej lub tożsamości scenariusze kradzieżą.

Analizy strumieniu jest w pełni zarządzane usługa, która zapewnia małym opóźnieniem wysokiej dostępności, skalowalna, złożonych przetwarzania przez strumieniowego przesyłania danych w chmurze. Aby uzyskać więcej informacji zobacz [Wprowadzenie do analizy strumieniu Azure](stream-analytics-introduction.md).


## <a name="scenario-telecommunications-and-sim-fraud-detection-in-real-time"></a>Scenariusz: Wykrywanie oszustwa telekomunikacyjnych i SIM w czasie rzeczywistym

Firma telekomunikacyjnych ma dużej ilości danych dla połączeń przychodzących. Firma musi następujące z jego danych:

* Zmniejsz danych kwocie łatwiejsze i uzyskać więcej informacji o korzystaniu z klienta w czasie i w regionach geograficznych.
* Wykrywa oszustwa SIM (wiele połączeń z tej samej tożsamości wokół tym samym czasie, ale w geograficznie różnych lokalizacjach) w czasie rzeczywistym, dzięki czemu firmy można łatwo odpowiadać powiadamianie o klientów lub zamykanie usługi.

Kanoniczny scenariusze Internet czynności (IoT) mają tona telemetrycznego lub danych z czujników. Klienci chcą agregowanie danych lub otrzymywać alerty o różnic w odniesieniu w czasie rzeczywistym.

## <a name="prerequisites"></a>Wymagania wstępne

- Pobierz [TelcoGenerator.zip](http://download.microsoft.com/download/8/B/D/8BD50991-8D54-4F59-AB83-3354B69C8A7E/TelcoGenerator.zip) z Centrum pobierania Microsoft
- Opcjonalnie: Kodu źródłowego generator wydarzenia z [GitHub](https://aka.ms/azure-stream-analytics-telcogenerator)

## <a name="create-azure-event-hubs-input-and-consumer-group"></a>Tworzenie grupy danych wejściowych i dla klientów indywidualnych koncentratory zdarzenia Azure

Przykładowa aplikacja będzie generowanie zdarzeń i przekazać je do wystąpienia zdarzenia koncentratory przetwarzania w czasie rzeczywistym. Usługa Bus zdarzenia koncentratory są preferowanej metody spożyciu zdarzeń dla analizy strumieniu. Przedstawiono więcej informacji na temat koncentratory zdarzenia w [dokumentacji Bus usługi Azure](/documentation/services/service-bus/).

Aby utworzyć Centrum zdarzeń:

1.  W [portalu Azure](https://manage.windowsazure.com/), kliknij przycisk **Nowy** > **Aplikacji usług** > **BUS usługi** > **Centrum zdarzenia** > **Szybkiego tworzenia**. Podaj nazwę, region i nowym lub istniejącym nazw do utworzenia nowego koncentratora zdarzenia.  
2.  Zgodnie z zaleceniami dotyczącymi każdego zadania analizy strumieniu powinni zapoznać się z grupy dla klientów indywidualnych Centrum pojedyncze zdarzenie. Firma Microsoft przeprowadzi Cię przez proces tworzenia grupy dla klientów indywidualnych później. Aby [uzyskać więcej informacji o grupach dla klientów indywidualnych](https://msdn.microsoft.com/library/azure/dn836025.aspx). Aby utworzyć grupę dla klientów indywidualnych, przejdź do Centrum nowo utworzonego zdarzenia, kliknij kartę **Grupy dla klientów indywidualnych** , kliknij przycisk **Utwórz** w dolnej części strony, a następnie podaj nazwę dla grupy dla klientów indywidualnych.
3.  Aby udzielić dostępu do koncentratora zdarzenia, potrzebujemy Tworzenie zasad udostępniania. Kliknij kartę **Konfigurowanie** Twoim Centrum zdarzenia.
4.  W obszarze **Zasady dostępu UDOSTĘPNIONE**tworzenia nowych zasad, **Zarządzanie** uprawnieniami.

    ![Udostępnione miejsce, w którym można utworzyć zasadę z uprawnieniami Zarządzanie zasadami dostępu.](./media/stream-analytics-real-time-fraud-detection/stream-ananlytics-shared-access-policies.png)

5.  Kliknij pozycję **ZAPISZ** u dołu strony.
6.  Przejdź do **pulpitu nawigacyjnego**, kliknij pozycję **Informacje o połączeniu** w dolnej części strony, a następnie skopiuj i zapisywanie informacji o połączeniu.

## <a name="configure-and-start-the-event-generator-application"></a>Konfigurowanie i rozpoczynanie aplikacji generator zdarzenia

Utworzono aplikacji klienckiej, która będzie wygenerować metadanych połączenia przychodzące próbki i przekazać go do koncentratorów zdarzenia. Wykonaj następujące czynności, aby skonfigurować tę aplikację.  

1.  Pobierz [plik TelcoGenerator.zip](http://download.microsoft.com/download/8/B/D/8BD50991-8D54-4F59-AB83-3354B69C8A7E/TelcoGenerator.zip)i rozpakuj plik go do katalogu.

    > [AZURE.NOTE] System Windows może blokować pliku zip pobrany. Kliknij prawym przyciskiem myszy plik i wybierz polecenie **Właściwości**. Jeśli zostanie wyświetlony komunikat "ten plik pochodzi z innego komputera i może być zablokowany, aby zwiększyć ochronę tego komputera" zaznacz pole **Odblokuj** , a następnie kliknij przycisk Zastosuj w pliku zip.

2.  Zamienianie wartości Microsoft.ServiceBus.ConnectionString i EventHubName w telcodatagen.exe.config z parametrów połączenia Centrum zdarzeń i nazwę.

    Parametry połączenia, który został skopiowany z portalu usługi Azure umieszcza nazwę połączenia na końcu. Pamiętaj usunąć "; EntityPath =<value>"od" Dodaj klucz = "pola.

3.  Uruchom aplikację. Jest w następujący sposób:

    telcodatagen.exe [#NumCDRsPerHour] [prawdopodobieństwo oszustwa karty SIM] [#DurationHours]

Poniższy przykład wygeneruje 1000 zdarzeń przy prawdopodobieństwie 20 procent oszustwa w ciągu dwóch godzin.

    telcodatagen.exe 1000 .2 2

Zobaczysz rekordy wysyłane do Twojej Centrum zdarzeń. Niektóre pola klucza użyjemy w tej aplikacji wykrywania oszustwa w czasie rzeczywistym zdefiniowano poniżej:

| Rekord | Definicja |
| ------------- | ------------- |
| CallrecTime | Sygnatura czasowa dla godziny rozpoczęcia połączenia. |
| SwitchNum | Przełącznik telefoniczny używany do łączenia połączenie. |
| CallingNum | Numer telefonu rozmówcy. |
| CallingIMSI | Tożsamość międzynarodowe subskrybentów urządzeń przenośnych (firmy IMSI).  Unikatowy identyfikator rozmówcy. |
| CalledNum | Numer telefonu odbiorcy. |
| CalledIMSI | Tożsamość międzynarodowe subskrybentów urządzeń przenośnych (firmy IMSI).  Unikatowy identyfikator rozmówcy rozmowy. |


## <a name="create-a-stream-analytics-job"></a>Utwórz zadanie analizy strumieniu
Teraz gdy mamy już strumienia zdarzeń telekomunikacyjnych, możemy Skonfiguruj zadanie analizy strumieniu analizowanie tych zdarzeń w czasie rzeczywistym.

### <a name="provision-a-stream-analytics-job"></a>Obsługa administracyjna zadanie analizy strumieniu

1.  W portalu Azure, kliknij przycisk **Nowy** > **Usług danych** > **Analizy STRUMIENIU** > **Szybkiego tworzenia**.
2.  Określ następujące wartości, a następnie kliknij polecenie **Utwórz zadanie analizy STRUMIENIU**:

    * **Nazwa zadania**: Wprowadź nazwę zadania.

    * **REGION**: Wybierz region, w którym chcesz uruchomić zadanie. Należy rozważyć, umieszczając to zadanie i Centrum zdarzeń w tym samym regionie, aby zapewnić lepszą wydajność i upewnij się, że użytkownik będzie nie można opłaty do przenoszenia danych między regionami.

    * **Konto miejsca do magazynowania**: Wybierz konto Azure miejsca do magazynowania, którego chcesz używać do przechowywania danych monitorowania dla wszystkich zadań analizy strumieniu uruchamiane w tym regionie. Istnieje możliwość wybierz istniejące konto miejsca do magazynowania lub Utwórz nowy.

3.  Kliknij pozycję **Analizy STRUMIENIU** w okienku po lewej stronie, aby wyświetlić zadania analizy strumieniu.

    ![Ikona usługi analizy strumieniu](./media/stream-analytics-real-time-fraud-detection/stream-analytics-service-icon.png)

    Nowe zadanie pojawi się ze stanem **UTWORZONE**. Zwróć uwagę, że przycisk **START** na koniec strony jest wyłączona. Przed rozpoczęciem tego zadania musisz skonfigurować zadania wejście, dane wyjściowe i kwerendy.

### <a name="specify-job-input"></a>Określanie zadania wejście
1.  W swojej pracy analizy strumieniu kliknij **wartości wejściowych** w górnej części strony, a następnie kliknij **Dodawanie danych wejściowych**. Zostanie otwarte okno dialogowe przeprowadzi Cię przez kilka kroków konfigurowania dane wejściowe.
2.  Kliknij **strumienia danych**, a następnie kliknij przycisk prawy.
3.  Kliknij pozycję **Centrum zdarzenia**, a następnie kliknij przycisk prawy.
4.  Wpisz lub wybierz następujące wartości na trzeciej stronie:

    * **ALIAS danych wejściowych**: Wprowadź przyjazną nazwę, takich jak *CallStream*dla tego zadania. Należy zauważyć, że będziesz używać tej nazwy w kwerendzie później.

    * **Centrum zdarzenia**: w przypadku koncentratora zdarzenia, który został utworzony w tej samej subskrypcji jako zadanie analizy strumieniu, zaznacz obszar nazw, należącym do Centrum zdarzenia.

        Jeśli Twoim Centrum zdarzeń znajduje się w innej subskrypcji, wybierz pozycję **Centrum zdarzeń korzystanie z innej subskrypcji**i ręcznie wprowadź informacje dotyczące **Usługi BUS nazw**, **Nazwa centrum zdarzenia**, **Nazwa zasady Centrum zdarzenia**, **Klucz zasad Centrum zdarzenia**i **Liczba PARTITION Centrum zdarzeń**.

    * **Nazwa centrum zdarzenia**: Wybierz nazwę Centrum zdarzenia.

    * **Nazwa zasady Centrum zdarzenia**: Wybierz zasadę Centrum zdarzenia utworzonego wcześniej w tym samouczku.

    * **GRUPĘ dla klientów indywidualnych Centrum zdarzeń**: wpisz nazwę grupy dla klientów indywidualnych, utworzony wcześniej w tym samouczku.

5.  Kliknij przycisk prawy.
6.  Określ następujące wartości:

    * **FORMAT SERIALIZATORA zdarzenia**: JSON
    * **KODOWANIE**: UTF8
7.  Kliknij przycisk **Sprawdź** , aby dodać to źródło i sprawdzić, czy analizy strumieniu pomyślnie łączy do koncentratora zdarzenia.

### <a name="specify-job-query"></a>Określanie zapytania zadania

Analizy strumieniu obsługuje model prostej kwerendy deklaracyjnych opisującą przekształcenia przetwarzania w czasie rzeczywistym. Aby dowiedzieć się więcej na temat języka, zobacz [Dokumentacja języka kwerend analizy strumieniu Azure](https://msdn.microsoft.com/library/dn834998.aspx). Ten samouczek pomoże Ci autor i testowanie kilku kwerend w czasie rzeczywistym strumienia połączenia danych.

#### <a name="optional-sample-input-data"></a>Opcjonalnie: Przykładowych danych wejściowych
Aby sprawdzić poprawność kwerendy danych rzeczywistej pracy, umożliwia funkcja **PRZYKŁADOWYCH danych** wyodrębniania strumienia zdarzeń i Utwórz. Plik JSON zdarzeń do testowania.  Poniższe kroki pokazują jak to zrobić. Przykładowy plik [telco.json](https://github.com/Azure/azure-stream-analytics/blob/master/Sample%20Data/telco.json) do celów testowych także zostały zamieszczone.

1.  Zaznacz zakres wejściowy Centrum zdarzenia, a następnie kliknij **Przykładowe dane** w dolnej części strony.
2.  W wyświetlonym oknie dialogowym Określ **czas rozpoczęcia** zacząć zbierania danych i **czasu trwania** na używanie ilość dodatkowe dane.
3.  Kliknij przycisk **Sprawdź** , aby uruchomić pobierania danych z danych wejściowych.  Może potrwać minutę lub dwie pliku danych, które należy opracować.  Po zakończeniu procesu kliknij przycisk **Szczegóły**, Pobierz wygenerowane. JSON plik i zapisz go.

    ![Pobierz i Zapisz przetworzone dane w pliku JSON](./media/stream-analytics-real-time-fraud-detection/stream-analytics-download-save-json-file.png)

#### <a name="pass-through-query"></a>Kwerenda przekazująca

Jeśli chcesz zarchiwizować każde zdarzenie, służy kwerenda przekazująca czytanie wszystkich pól w ładunku zdarzenie lub wiadomości. Aby rozpocząć, wykonaj prostej kwerendy przekazującej tego wszystkie pola w przypadku projektów.

1.  Kliknij przycisk **kwerendy** u góry strony zadania analizy strumieniu.
2.  Dodaj następujący do edytora kodu:

        SELECT * FROM CallStream

    > [AZURE.IMPORTANT] Upewnij się, że nazwa źródła danych wejściowych odpowiada nazwie wprowadzenia określonej wcześniej.

3.  Kliknij przycisk **Testuj** w edytorze zapytań.
4.  Podaj plik testowy. Został utworzony przy użyciu poprzednich krokach użyć, lub [telco.json](https://github.com/Azure/azure-stream-analytics/blob/master/Samples/SampleDataFiles/Telco.json).
5.  Kliknij przycisk **Sprawdź** , a wyniki są wyświetlane poniżej definicją zapytania.

    ![Wyniki definicja zapytania](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sim-fraud-output.png)


### <a name="column-projection"></a>Rzut kolumny

Firma Microsoft będzie teraz ograniczyć zwracane pola do mniejszy zestaw.

1.  Zmienianie kwerendy w edytorze kodu:

        SELECT CallRecTime, SwitchNum, CallingIMSI, CallingNum, CalledNum
        FROM CallStream

2.  W edytorze zapytań, aby wyświetlić wyniki kwerendy, kliknij przycisk **Uruchom ponownie** .

    ![Wynik w edytorze zapytań.](./media/stream-analytics-real-time-fraud-detection/stream-analytics-query-editor-output.png)

### <a name="count-of-incoming-calls-by-region-tumbling-window-with-aggregation"></a>Liczba przychodzące połączenia według regionów: okno Tumbling z agregacji

Aby porównać liczbę przychodzące połączenia w rozbiciu na regiony, użyjemy [TumblingWindow](https://msdn.microsoft.com/library/azure/dn835055.aspx) pobrać liczby połączeń przychodzących pogrupowane według **SwitchNum** co pięć sekund.

1.  Zmienianie kwerendy w edytorze kodu:

        SELECT System.Timestamp as WindowEnd, SwitchNum, COUNT(*) as CallCount
        FROM CallStream TIMESTAMP BY CallRecTime
        GROUP BY TUMBLINGWINDOW(s, 5), SwitchNum

    Tej kwerendzie użyto kluczowego **Sygnatury czasowej** do określenia pola sygnatury czasowej w ładunku może być używany do obliczenia czasowy. Jeśli to pole nie zostanie określony, będzie można wykonać operacji Obsługa okien przy użyciu podczas każdego zdarzenia dotarła do Centrum zdarzenia. Zobacz [Dokumentacja języka kwerend "Nadejście czasu w porównaniu z aplikacji Time" w analizy strumieniu](https://msdn.microsoft.com/library/azure/dn834998.aspx).

    Należy zauważyć, że uzyskiwania dostępu do sygnaturę czasową na koniec każdego okna przy użyciu właściwości **System.Timestamp** .

2.  W edytorze zapytań, aby wyświetlić wyniki kwerendy, kliknij przycisk **Uruchom ponownie** .

    ![Wyniki kwerendy dla Timestand przez](./media/stream-analytics-real-time-fraud-detection/stream-ananlytics-query-editor-rerun.png)

### <a name="sim-fraud-detection-with-a-self-join"></a>Wykrywanie oszustwa SIM za pomocą samosprzężenia

Aby zidentyfikować potencjalnie fałszywych zastosowania, opisano dla połączeń, które pochodzą z tego samego użytkownika, ale w różnych lokalizacjach w ciągu mniej niż 5 sekund.  Firma Microsoft [sprzężenia](https://msdn.microsoft.com/library/azure/dn835026.aspx) strumienia zdarzeń połączenia ze sobą, aby wyszukać w takich przypadkach.

1.  Zmienianie kwerendy w edytorze kodu:

        SELECT System.Timestamp as Time, CS1.CallingIMSI, CS1.CallingNum as CallingNum1,
        CS2.CallingNum as CallingNum2, CS1.SwitchNum as Switch1, CS2.SwitchNum as Switch2
        FROM CallStream CS1 TIMESTAMP BY CallRecTime
        JOIN CallStream CS2 TIMESTAMP BY CallRecTime
        ON CS1.CallingIMSI = CS2.CallingIMSI
        AND DATEDIFF(ss, CS1, CS2) BETWEEN 1 AND 5
        WHERE CS1.SwitchNum != CS2.SwitchNum

2.  W edytorze zapytań, aby wyświetlić wyniki kwerendy, kliknij przycisk **Uruchom ponownie** .

    ![Wyniki kwerendy sprzężenia](./media/stream-analytics-real-time-fraud-detection/stream-ananlytics-query-editor-join.png)

### <a name="create-output-sink"></a>Tworzenie sink wyjścia

Teraz, gdy zdefiniowany strumienia zdarzenia koncentratora zdarzenia dane wejściowe mogły zjeść tej ostatniej zdarzeń i kwerenda przekształcenie nad strumienia, ostatnim krokiem jest określenie sink wynik dla zadania. Firma Microsoft będzie zapisywać zdarzenia fałszywej zachowanie magazyn obiektów Blob platformy Azure.

Wykonaj następujące czynności, aby utworzyć kontener magazyn obiektów Blob, jeśli nie masz jeszcze jedną.

1.  Użyj istniejącego konta miejsca do magazynowania lub Utwórz nowe konto miejsca do magazynowania, klikając pozycję **Nowy > usługi danych > miejsca do magazynowania > Szybkie tworzenie**i postępuj zgodnie z instrukcjami.
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

## <a name="start-job-for-real-time-processing"></a>Rozpoczynanie zadania do przetwarzania w czasie rzeczywistym

Ponieważ zadania wejście, kwerendy i wyjście wszystkich określono, możemy są gotowe do rozpoczęcia zadania analizy strumieniu wykrywania oszustwa w czasie rzeczywistym.

1.  Zadania **pulpitu NAWIGACYJNEGO**kliknij pozycję **Uruchom** w dolnej części strony.
2.  W wyświetlonym oknie dialogowym kliknij **czas rozpoczęcia zadania**, a następnie kliknij przycisk **Sprawdź** u dołu okna dialogowego. Stan zadania zostanie zmieniony na **Uruchamianie** i wkrótce zostanie zmieniona na **uruchomiony**.

## <a name="view-fraud-detection-output"></a>Wynik wykrywania oszustwa widoku

Narzędzie, takie jak [Eksplorator magazynu Azure](http://storageexplorer.com/) lub [Eksploratora Azure](http://www.cerebrata.com/products/azure-explorer/introduction) umożliwia wyświetlanie fałszywej zdarzenia, zgodnie z ich zapisaniu w danych wyjściowych w czasie rzeczywistym.  

![Wykrywanie oszustwa: fałszywej zdarzenia wyświetlane w czasie rzeczywistym](./media/stream-analytics-real-time-fraud-detection/stream-ananlytics-view-real-time-fraudent-events.png)

## <a name="get-support"></a>Uzyskiwanie pomocy technicznej
Aby uzyskać dodatkową pomoc spróbuj nasze [forum Azure analizy strumieniu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).


## <a name="next-steps"></a>Następne kroki

- [Wprowadzenie do analizy strumieniu Azure](stream-analytics-introduction.md)
- [Skalowanie Azure analizy strumieniu zadania](stream-analytics-scale-jobs.md)
- [Dokumentacja języka kwerend analizy strumieniu Azure](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Strumień Azure analizy zarządzania pozostałych interfejsu API odwołania](https://msdn.microsoft.com/library/azure/dn835031.aspx)
