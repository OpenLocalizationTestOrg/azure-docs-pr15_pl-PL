<properties 
    pageTitle="Jak pisać zapytania w analizy strumieniu | Microsoft Azure" 
    description="Pisanie zapytań w analizy strumieniu i dane kwerendy | Nauka segmentu ścieżki."
    keywords="jak pisać zapytania, kwerenda danych, utworzyć kwerendę, Pisanie zapytań"
    documentationCenter=""
    services="stream-analytics"
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

# <a name="how-to-write-queries-in-stream-analytics"></a>Jak pisać zapytania w analizy strumieniu

Pisanie zapytań dla strumienia przetwarzania reguł w Azure analizy strumieniu jest zaimplementowana jako "Kwerenda stałego", zdefiniowanej przed zadanie rozpoczyna się i wykonywane na danych, jak osiągnie ona zadania. Przekształcenie danych jest wyrażony w języku zapytania SQL Lubię to głównie podzbiór T-SQL z niektóre rozszerzenia dodany język, takich jak [Obsługa okien](https://msdn.microsoft.com/library/azure/dn835019.aspx) użyty w celu wyrażenia czasowy znaczeń właściwych.

## <a name="writing-queries"></a>Zapisywanie kwerend: ##

1. W swojej pracy analizy strumieniu w portalu zarządzania Azure kliknij **kwerendę**.

    ![Wybierz kwerendę](./media/stream-analytics-write-queries/1-stream-analytics-write-queries.png)  

    W portalu Azure kliknij **kwerendę**.

    ![Wybierz pozycję Podgląd zapytania](./media/stream-analytics-write-queries/query-preview-portal.png)  

2.  Nowe zadania mają szablonu kwerendy ułatwiające rozpoczęcie pracy. Szablon kwerendy wykonuje kwerendy "przekazującej", która projektów wszystkie pola z wprowadzania zdarzeń w wyniku.  

    - Jeśli co najmniej jeden dane wejściowe i wyjściowe zdefiniowane dla zadania, możesz zastąpić symbol zastępczy "[YourOutputAlias]" i "[YourInputAlias]" pola z aliasy dane wejściowe i wyjściowe, którego chcesz użyć najpierw. Ponadto można nadal autor i testowanie kwerendy w portalu klasyczny Azure bez definiujące dane wejściowe i wyjściowe zadania.
    - Jeśli chcesz wykonać przetwarzanie więcej niż prosty przekazującą, można edytować definicją zapytania. Aby rozpocząć pracę z tworzenia kwerend, zapoznaj się kilka typowych kwerendy desenie są przechwytywane [w tym miejscu](stream-analytics-stream-analytics-query-patterns.md).  
  
    ![Dane kwerendy okna](./media/stream-analytics-write-queries/2-stream-analytics-write-queries.png)  

## <a name="to-validate-query-data-is-working"></a>Aby sprawdzić poprawność danych kwerendy działa: ##

Można sprawdzić, czy kwerenda działa zgodnie z oczekiwaniami, uruchamiając go w przeglądarce, przez co najmniej jeden JSON pliki lokalne danymi test. To nie będzie uruchomić zadanie lub mieć konsekwencje rozliczeń.

> [AZURE.NOTE] Obecnie testowanie kwerendy w przeglądarce nie jest obsługiwana w Azure Portal.  

1.  Upewnij się, że nie ma żadnych błędów w kwerendzie (w przeciwnym razie przycisk testu jest wyłączona), a następnie kliknij przycisk Testuj.  

    ![Dane kwerendy Test](./media/stream-analytics-write-queries/3-stream-analytics-write-queries.png)  

2.  Wyświetli monit o określenie plików dla każdego z danych wejściowych, do których odwołuje się zapytanie. W tym przykładzie pozostaje kwerendy szablonu-jest, aby okno dialogowe monit o wprowadzenie informacji o nazwie "yourinputalias".  

    ![Testuj kwerendę danych](./media/stream-analytics-write-queries/4-stream-analytics-write-queries.png)  

3.  Przejdź do pliku test. Kilka przykładowych plików są dostępne na [github](https://github.com/Azure/azure-stream-analytics/tree/master/Sample Data) i przykładowe dane można także pobrać z własnego strumienia wartości wejściowych danych za pomocą funkcji przykładowych danych na karcie danych wejściowych.  

    ![Wejście kwerendy](./media/stream-analytics-write-queries/5-stream-analytics-write-queries.png)  

4.  Po zamknięciu okna dialogowego, kwerenda zostanie uruchomiona nad danymi test i będą widoczne wyniki u dołu strony kwerendy.  

    ![Podsumowanie kwerendy](./media/stream-analytics-write-queries/6-stream-analytics-write-queries.png)  

## <a name="get-help"></a>Uzyskiwanie pomocy
Aby uzyskać dodatkową pomoc spróbuj nasze [forum analizy strumieniu Azure](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Następne kroki

- [Wprowadzenie do analizy strumieniu Azure](stream-analytics-introduction.md)
- [Wprowadzenie do korzystania z analizy strumieniu Azure](stream-analytics-get-started.md)
- [Skalowanie Azure analizy strumieniu zadania](stream-analytics-scale-jobs.md)
- [Dokumentacja języka kwerend analizy strumieniu Azure](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Strumień Azure analizy zarządzania pozostałych interfejsu API odwołania](https://msdn.microsoft.com/library/azure/dn835031.aspx)
