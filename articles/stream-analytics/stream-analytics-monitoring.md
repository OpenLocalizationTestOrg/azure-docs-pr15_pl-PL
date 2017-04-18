<properties 
    pageTitle="Monitorowanie zadania analizy strumieniu opis | Microsoft Azure" 
    description="Opis zadania analizy strumieniu monitorowania" 
    keywords="monitorowanie kwerendy"
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
    ms.workload="data-services" 
    ms.date="09/26/2016" 
    ms.author="jeffstok"/>

# <a name="understand-stream-analytics-job-monitoring-and-how-to-monitor-queries"></a>Opis analizy strumieniu zadania monitorowania i sposobów monitorowania kwerendy

## <a name="introduction-the-monitor-page"></a>Wprowadzenie: Strona monitorowania

Portal Azure zarządzania i Azure Portal powierzchni kluczowe wskaźniki, który może być używany do monitorowania i rozwiązać problemy z wydajnością usługi kwerendy i zadania. 

W portalu zarządzania Azure kliknij na karcie **Monitor** uruchomione zadanie analizy strumieniu, aby wyświetlić te metryki. Istnieje opóźnienie najwyżej 1 minuta wyrażona za pomocą wskaźniki pojawiać się na stronie monitora.  

  ![Monitorowanie zadania pulpitu nawigacyjnego](./media/stream-analytics-monitoring/01-stream-analytics-monitoring.png)  

W portalu Azure przejdź do zadania analizy strumieniu interesuje oglądanie metryki dla i wyświetlanie sekcji **monitorowania** .  

  ![Azure portalu monitorowania zadania pulpitu nawigacyjnego](./media/stream-analytics-monitoring/06-stream-analytics-monitoring.png)  

Przy pierwszym uruchomieniu zadanie analizy strumieniu jest tworzona w danym regionie będzie musisz skonfigurować diagnostyki w danym regionie. Aby to zrobić, kliknij dowolne miejsce w sekcji **monitorowania** i karta **Diagnostyka** będą wyświetlane. Tutaj możesz włączyć narzędzia diagnostyczne i określ konto miejsca do magazynowania dla monitorowanie danych.  

  ![Azure Portal skonfigurować kwerendy diagnostyki](./media/stream-analytics-monitoring/07-stream-analytics-monitoring.png)  

## <a name="metrics-available-for-stream-analytics"></a>Metryki dostępne dla analizy strumieniu


| Metryka | Definicja |
|--------|-------------|
| SU % wykorzystania | Wykorzystanie podmiotów Streaming przypisany do zadania na karcie Skala zadania. Jeżeli wskaźnik osiągnie 80% lub powyżej, jest wysoka prawdopodobieństwo, że przetwarzanie zdarzenia może zostać opóźnione lub zatrzymania wprowadzania informacji o postępie. |
| Zdarzenia wprowadzania danych | Liczba odebranych przez zadanie analizy strumieniu, w polu Liczba zdarzeń. To może służyć do sprawdzania, czy zdarzenia są wysyłane do źródła danych wejściowych. |
| Dane wyjściowe zdarzenia | Ilość danych wysyłane przez zadanie analizy strumieniu do wyjściowego miejsca docelowego, a w polu Liczba zdarzeń. |
| W nowym oknie kolejność zdarzeń | Liczba zdarzeń otrzymano poza zamówienia, które zostały usunięte lub podane skorygowane sygnatury czasowej, na podstawie zasad kolejność zdarzeń. Można to dotyczy konfiguracji ustawienia okna uszkodzenia porządku w nowym oknie. |
| Błędów konwersji danych | Liczba błędów konwersji danych poniesione przez zadanie analizy strumieniu. |
| Błędy wykonania | Liczba błędów wykonywanych podczas wykonywania zadania analizy strumieniu. |
| Opóźnione zdarzeń wprowadzania danych | Liczba zdarzeń nadchodzi opóźnione ze źródła, które albo zostały porzucone lub ich sygnatury czasowej skorygowaniu na podstawie konfiguracji zasad kolejność zdarzeń ustawienia opóźnienia przylotów uszkodzenia okna. |

## <a name="customizing-monitoring-in-the-azure-management-portal"></a>Dostosowywanie monitorowania w portalu zarządzania Azure ##

Maksymalnie 6 metryki mogą być wyświetlane na wykresie.

Aby przełączyć się między wyświetlaniem wartości względne (końcowej tylko dla każdej jednostki metryczne) i wartości bezwzględnej (oś Y wyświetlane), wybierz względną lub bezwzględną w górnej części wykresu.

  ![Monitorowanie kwerendy względne bezwzględne](./media/stream-analytics-monitoring/02-stream-analytics-monitoring.png)  

Metryki można wyświetlać na wykresie monitora w agregacji 1 godzina 12 godzin, 24 godziny lub 7 dni.

Aby zmienić zakres czasu wyświetla wykres metryki, wybierz 1 godzina 24 godziny lub 7 dni w górnej części wykresu.

  ![Monitorowanie kwerendy skali czasu](./media/stream-analytics-monitoring/03-stream-analytics-monitoring.png)  

Możesz ustawić reguły, które można powiadomienie pocztą e-mail w przypadku, gdy zadanie przecina określonego progu. 

## <a name="customizing-monitoring-in-the-azure-portal"></a>Dostosowywanie monitorowanie Azure Portal ##

Można dopasować typ wykresu, metryki widoczny i czasu zakres w ustawieniach edytowanie wykresu. Aby uzyskać szczegółowe informacje zobacz [jak dostosować monitorowania](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).

  ![Skala czasu monitora Azure portalu kwerendy](./media/stream-analytics-monitoring/08-stream-analytics-monitoring.png)  

## <a name="job-status"></a>Stan zadania

Stan zadań analizy strumieniu można wyświetlać w klasycznym Portal Azure miejsce, w którym zostanie wyświetlona lista zadań. Zobaczysz listę zadań, klikając ikonę analizy strumieniu w portalu klasyczny Azure.

| Stan | Definicja |
|--------|------------|
| Utworzone | Zadanie został utworzony, jednak nie zostało uruchomione. |
| Uruchamianie | Użytkownik kliknie po uruchomieniu zadania i uruchamiania zadania |
| Uruchamianie | Zadania są przydzielane przetwarzania danych wejściowych lub oczekujące na przetwarzanie danych wejściowych. Jeśli zadanie wyświetlany stan uruchomiony bez wytwarzania dane wyjściowe, prawdopodobnie że okno czasu przetwarzania danych jest duży lub logiczny kwerendy jest skomplikowana. Innym powodem może być, że obecnie nie ma żadnych danych są wysyłane do zadania. |
| Zatrzymywanie | Użytkownik kliknie przy zatrzymaniu zadania i zatrzymywanie zadania. |
| Zatrzymano | Zadanie zostało zatrzymane. |
| Obniżonej wydajności | Ten stan wskazuje, że zadanie analizy strumieniu występują błędy przejściowych (dla ex. Dane wejściowe/wyjściowe błędów, błędy przetwarzania, błędów konwersji itp.). Zadanie jest nadal uruchomiony, jednak istnieje wiele błędów generowany. To zadanie wymaga uwagi klienta i klienta można uzyskać, zobacz operacje dzienniki błędów. |
| Nie powiodła się | Oznacza to, że zadanie zostało zakończone niepowodzeniem z powodu błędów i przetwarzanie zostało zatrzymane. Klient potrzebuje do przeanalizowania dzienników operacji w celu debugowania błędów. |
| Usuwanie | Oznacza to, że zadanie zostanie usunięty. |

## <a name="diagnosis"></a>Diagnostyka

W portalu zarządzania Azure pulpit nawigacyjny zadania zawiera informacje dotyczące wymagających Wyszukaj Wyświetla diagnostyki, to znaczy danych wejściowych i/lub operacji logowania. Klikając łącze, aby go w odpowiednie miejsce, aby przyjrzeć się diagnostyki.

  ![Monitorowanie kwerendy błędu](./media/stream-analytics-monitoring/04-stream-analytics-monitoring.png)  

Klikając wejściowe i wyjściowe zasobu zawiera szczegółowe informacje diagnostyczne. Jest to odświeżone przy użyciu najnowszych informacji diagnostycznych podczas uruchamiania zadania.

  ![Narzędzia diagnostyczne kwerendy](./media/stream-analytics-monitoring/05-stream-analytics-monitoring.png)  

## <a name="get-help"></a>Uzyskiwanie pomocy
Aby uzyskać dodatkową pomoc spróbuj nasze [forum analizy strumieniu Azure](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Następne kroki

- [Wprowadzenie do analizy strumieniu Azure](stream-analytics-introduction.md)
- [Wprowadzenie do korzystania z analizy strumieniu Azure](stream-analytics-get-started.md)
- [Skalowanie Azure analizy strumieniu zadania](stream-analytics-scale-jobs.md)
- [Dokumentacja języka kwerend analizy strumieniu Azure](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Strumień Azure analizy zarządzania pozostałych interfejsu API odwołania](https://msdn.microsoft.com/library/azure/dn835031.aspx)
