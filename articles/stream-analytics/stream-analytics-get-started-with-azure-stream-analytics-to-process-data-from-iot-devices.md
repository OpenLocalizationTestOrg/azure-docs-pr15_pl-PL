<properties
    pageTitle="Wprowadzenie do analizy strumieniu Azure przetworzyć danych z urządzeń IoT. | Microsoft Azure"
    description="IoT strumienie znaczniki i dane czujnik z analizy strumieniu i przetwarzania danych w czasie rzeczywistym"
    keywords="rozwiązanie iot wprowadzenie iot"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"
/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="10/19/2016"
    ms.author="jeffstok"
/>

# <a name="get-started-with-azure-stream-analytics-to-process-data-from-iot-devices"></a>Wprowadzenie do analizy strumieniu Azure przetworzyć danych z urządzeń IoT

W tym samouczku dowiesz się, jak utworzyć strumienia przetwarzanie logiki do zbierania danych z urządzenia Internet czynności (IoT). Użyjemy przypadek użycia rzeczywistych, Internet czynności (IoT) w celu zademonstrowania sposobu tworzenia rozwiązania szybko i ekonomicznie.

## <a name="prerequisites"></a>Wymagania wstępne

-   [Azure subskrypcji](https://azure.microsoft.com/pricing/free-trial/)
-   Przykładowe można pobrać ze strony [GitHub](https://aka.ms/azure-stream-analytics-get-started-iot) pliki kwerend i danych

## <a name="scenario"></a>Scenariusz

Contoso, czyli firmy w obszarze automatyzacji przemysłowych, ma całkowicie zautomatyzowana procesu produkcji. Sprzęt w tym fabryki ma czujników, które są w stanie wysyłających strumieni danych w czasie rzeczywistym. W tym scenariuszu Menedżer powierzchnia produkcji chcą wniosków w czasie rzeczywistym z danych czujnik poszukaj desenie i wykonać akcję na ich. Użyjemy języka kwerend analizy strumieniu (SAQL) nad danymi czujnika znajdowanie interesujący wzorce z przychodzących strumienia danych.

W tym miejscu danych jest generowany z urządzenia znacznika czujnik Teksas dokumentów.

![Znacznik czujnik instrumentami Teksas](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-01.jpg)

Ładunek danych są zapisane w formacie JSON i wygląda następująco:


    {
        "time": "2016-01-26T20:47:53.0000000",  
        "dspl": "sensorE",  
        "temp": 123,  
        "hmdt": 34  
    }  

Scenariusz z rzeczywistych mogą być setki czujniki te generowania zdarzeń jako strumienia. Najlepiej, jeśli urządzenie bramy można uruchomić kod, aby przekazać te zdarzenia do [Koncentratory zdarzenia Azure](https://azure.microsoft.com/services/event-hubs/) lub [Koncentratorów IoT Azure](https://azure.microsoft.com/services/iot-hub/). Zadanie analizy strumieniu czy mogły zjeść tej ostatniej te zdarzenia z koncentratorów zdarzenia, a następnie uruchom na strumienie kwerendy analizy w czasie rzeczywistym. Następnie wyniki można wysłać do jednego z [obsługiwanych wyjściowe](stream-analytics-define-outputs.md).

W przypadku łatwość użytkowania ten przewodnik wprowadzenie uzyskiwania udostępnia przykładowy plik danych, którego został przechwycony z rzeczywistą czujnik znacznik urządzenia. Można uruchomić kwerendy na przykładowych danych i wyświetlić wyniki. W samouczkach kolejnych będzie Dowiedz się, jak połączyć zadanie do wprowadzania i wyświetla i wdrażanie ich z usługą Azure.

## <a name="create-a-stream-analytics-job"></a>Utwórz zadanie analizy strumieniu

1. W [portalu Azure](http://portal.azure.com)kliknij znak plus, a następnie wpisz **Analizy STRUMIENIU** w oknie tekst do prawej. Zaznacz **zadanie analizy strumieniu** na liście wyników.

    ![Utwórz nowe zadanie analizy strumieniu](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-02.png)

2. Wprowadź nazwę zadania unikatowych i sprawdź, czy subskrypcja jest poprawne dla zadania. Następnie utwórz nową grupę zasobów lub wybierz istniejący od Twojej subskrypcji.

3. Następnie wybierz lokalizację dla zadania. Dla szybkości przetwarzania i zmniejszenia kosztów w danych zaleca się przesyłanie wybranie tej samej lokalizacji jako grupa zasobów i konto zamierzonego miejsca do magazynowania.

    ![Utwórz nowe zadanie analizy strumieniu szczegóły](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-03.png)

    > [AZURE.NOTE] Należy utworzyć konto tego miejsca do magazynowania do tylko raz w rozbiciu na regiony. Przechowywania będzie udostępniany przez wszystkich zadań analizy strumieniu, które są tworzone w danym regionie.

4. Zaznacz to pole, aby umieścić zadania na pulpicie nawigacyjnym, a następnie kliknij przycisk **Utwórz**.

    ![Tworzenie zadania w toku](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-03a.png)

5. Powinien zostać wyświetlony "Wdrażanie rozpoczęte..." wyświetlane w prawym górnym rogu okna przeglądarki. Wkrótce zostanie on zmieniony na ukończone okna tak jak pokazano poniżej.

    ![Tworzenie zadania w toku](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-03b.png)

### <a name="create-an-azure-stream-analytics-query"></a>Tworzenie kwerendy analizy strumieniu Azure

Po utworzeniu zadania występują razem aby go otworzyć i Konstruowanie kwerendy. Łatwo dostępne zadania, klikając Kafelek dla niego.

![Zadanie kafelków](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-04.png)

W okienku **Zadań topologii** kliknij pole **zapytania** , aby przejść do edytora zapytań. Edytor **zapytań** pozwala na wprowadzanie kwerendę T-SQL, która wykonuje przekształcenie przychodzących danych zdarzenia.

![Pole zapytania](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-05.png)

### <a name="query-archive-your-raw-data"></a>Kwerenda: Archiwizowanie nieprzetworzonych danych

Najprostszym formularzu kwerendy jest kwerendy przekazującej, archiwa wszystkie dane wejściowe do wyznaczonego dane wyjściowe. Pobierz przykładowy plik danych z [GitHub](https://aka.ms/azure-stream-analytics-get-started-iot) do lokalizacji na komputerze. 

1. Wklej kwerendę z pliku PassThrough.txt. 

    ![Strumień wejściowy test](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-06.png)

2. Kliknij wielokropek obok dane wejściowe, a następnie zaznacz pole **przekazywanie przykładowych danych z pliku** .

    ![Strumień wejściowy test](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-06a.png)

3. Zostanie otwarte okienko po prawej stronie jako wynik, w wybierz plik danych HelloWorldASA InputStream.json z lokalizacji usługi pobrany i kliknij przycisk **OK** u dołu okienka.

    ![Strumień wejściowy test](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-06b.png)

4. Kliknij koło zębate **Testowanie** , w górnym lewym rogu okna i procesu kwerendy test względem zestawu danych przykładowych. Zostanie otwarte okno wyniki poniżej zapytania, tak jak przetwarzania.

    ![Wyniki testu](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-07.png)

### <a name="query-filter-the-data-based-on-a-condition"></a>Kwerenda: Filtrowanie danych na podstawie warunku

Spróbuj do filtrowania wyników na podstawie warunku. Chcemy powoduje wyświetlenie wyników dla tylko te zdarzenia, które pochodzą z "sensorA." Kwerenda znajduje się w pliku Filtering.txt.

![Filtrowanie strumienia danych](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-08.png)

Uwaga uwzględniania wielkości liter kwerendy różni się wartość ciągu. Kliknij koło zębate **Test** ponownie, aby wykonać kwerendę. Kwerenda powinna zwrócić 389 wierszy poza 1860 zdarzeń.

![Druga wyniki wynik testu kwerendy](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-09.png)

### <a name="query-alert-to-trigger-a-business-workflow"></a>Kwerenda: Alert wyzwalać biznesowy przepływ pracy

Załóżmy umożliwia bardziej szczegółowe naszych kwerendy. Dla każdego typu czujnika chcemy monitorować średnia temperatura na 30-sekundowe okna i wyświetlenie wyników, tylko wtedy, gdy średnia temperatura powyżej 100 stopni. Firma Microsoft będzie pisanie poniższe zapytanie, a następnie kliknij przycisk **Test** , aby wyświetlić wyniki. Kwerenda znajduje się w pliku ThresholdAlerting.txt.

![kwerendy 30-sekundowe filtru](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-10.png)

Powinien zostać wyświetlony wyniki zawierające tylko 245 nazwy czujników i wierszy, w których średnia umiarkowanych jest większa niż 100. Ta kwerenda strumienia zdarzeń są grupowane według **dspl**, czyli nazwę czujnika w **Oknie Tumbling** 30 sekund. Czasowy kwerendy muszą zawierać, jak chcemy czasu do informacji o postępie. Za pomocą klauzuli **Sygnatury czasowej** , możemy określone kolumny **OUTPUTTIME** chcesz skojarzyć wszystkie obliczenia czasowy razy. Aby uzyskać szczegółowe informacje Przeczytaj artykuły w witrynie MSDN dotyczące [Zarządzania czasem](https://msdn.microsoft.com/library/azure/mt582045.aspx) i [funkcji Obsługa okien](https://msdn.microsoft.com/library/azure/dn835019.aspx).

### <a name="query-detect-absence-of-events"></a>Kwerenda: Wykrywanie braku zdarzenia

Jak można napisać kwerendę w celu znalezienia braku wprowadzania zdarzeń? Przejdźmy Znajdź ostatniego, że czujnik wysyłane danych i następnie nie wysłał zdarzeń następnego minuty. Kwerenda znajduje się w pliku AbsenseOfEvent.txt.

![Wykrywanie braku zdarzenia](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-11.png)

W tym miejscu stosujemy sprzężenia **Zewnętrznego w lewo** , aby ten sam strumień danych (samosprzężenia). Dla sprzężenia **wewnętrznego** wynik jest zwracany tylko wtedy, gdy zostanie znaleziony pasujący element.  Do sprzężenia **Zewnętrznego w lewo** w przypadku wydarzenia po lewej stronie sprzężenia niepasujące dane, zwracany jest wiersz, który ma wartość NULL dla wszystkich kolumn z prawej strony. Ta metoda jest bardzo przydatne znaleźć nieobecności zdarzeń. Zobacz naszą dokumentację w witrynie MSDN, aby uzyskać więcej informacji na temat [Dołączanie](https://msdn.microsoft.com/library/azure/dn835026.aspx).

## <a name="conclusion"></a>Wnioski

Celem tego samouczka jest pokazano, jak napisać różnych kwerendach języka kwerend analizy strumieniu i wyświetlić wyniki w przeglądarce. Jednak to jest zaczynasz korzystać. Jak wiele innych z analizy strumieniu. Analizy strumieniu obsługuje wiele wartości wejściowych i wyświetla i nawet w można używać funkcji Azure maszynowego uczenia nawiązać niezawodnego narzędzia do analizy strumieni danych. Można rozpocząć eksplorowanie więcej informacji o analizy strumieniu przy użyciu naszych [nauki mapy](https://azure.microsoft.com/documentation/learning-paths/stream-analytics/). Aby uzyskać więcej informacji na temat tworzenia kwerend przeczytaj artykuł o [Typowe wzorce kwerendy](./stream-analytics-stream-analytics-query-patterns.md).
