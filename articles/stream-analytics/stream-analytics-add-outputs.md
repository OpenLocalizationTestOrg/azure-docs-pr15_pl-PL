<properties 
    pageTitle="Jak skonfigurować danych wyświetla dla zadań analizy strumieniu | Microsoft Azure" 
    description="Konfigurowanie wyjściowe dla zadań analizy strumieniu | Nauka segmentu ścieżki."
    keywords="dane wyjściowe, przenoszenia danych"
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

# <a name="how-to-configure-data-outputs-for-stream-analytics-jobs"></a>Jak skonfigurować danych wyświetla analizy strumieniu zadań

Azure analizy strumieniu zadania mogą być połączone z co najmniej jeden dane wyjściowe, definiujące połączenia do istniejącego obiektu ujścia danych. Zadanie analizy strumieniu przetwarza i przekształceń przychodzących danych, strumień zdarzenia dane wyjściowe danych są zapisywane do wyjścia do zadania.

Strumień analizy danych, które wyjściowe może służyć do źródła pulpitów nawigacyjnych w czasie rzeczywistym lub alertów, wykonywanie przepływów pracy przepływu danych lub po prostu archiwizowanie danych w celu przetwarzanie wsadowe później. Analizy strumieniu ma pierwszej klasie Integracja z kilku usług Azure, opisanych tutaj.

Aby dodać dane wyjściowe do zadania analizy strumieniu:

1. W portalu klasyczny Azure kliknij **wyjściowe** , a następnie kliknij **Dodaj wyjścia** w swojej pracy analizy strumieniu.

    ![Dodawanie wyjściowe](./media/stream-analytics-add-outputs/1-stream-analytics-add-outputs.png)  

    W portalu Azure kliknij **wyjściowe** w swojej pracy analizy strumieniu.

    ![Dodawanie Azure Porta wyjściowe](./media/stream-analytics-add-outputs/5-stream-analytics-add-outputs.png)

2. Określ typ danych wyjściowych:

    ![Wybierz typ przepływu danych](./media/stream-analytics-add-outputs/2-stream-analytics-add-outputs.png)  

    ![Azure portal wybierz typ przepływu danych](./media/stream-analytics-add-outputs/6-stream-analytics-add-outputs.png)

3. Podaj przyjazną nazwę dla tego raportu w polu **Alias dane wyjściowe** . Ta nazwa może służyć w kwerendzie zadania kopii później odwoływał się wynik.  
    
    Wypełnij pozostałą część właściwości połączenia wymagane, aby nawiązać połączenie danych wyjściowych.  Te pola zależy od typu Wyjście i są definiowane tutaj.  

    ![Dodaj dane wyjściowe właściwości](./media/stream-analytics-add-outputs/3-stream-analytics-add-outputs.png)  

4. W zależności od typu danych wyjściowych może być konieczne Określ, jak szeregować lub sformatowane dane. Poniżej opisano ustawienia szeregowania określonych dla każdego typu danych wyjściowych.

    Wypełnij pozostałą część właściwości wymagane połączenia, aby połączyć się ze źródłem danych. Te pola zależy od typu danych wejściowych i źródło i szczegółowo zdefiniowane [w tym miejscu](stream-analytics-create-a-job.md).  

    ![Dodawanie dane wyjściowe do Centrum zdarzenia](./media/stream-analytics-add-outputs/4-stream-analytics-add-outputs.png)  

    ![Azure portalu dane wyjściowe do koncentratora zdarzenia](./media/stream-analytics-add-outputs/7-stream-analytics-add-outputs.png)  

> [Azure.Note] Każdy element danych wyjściowych dodane do zadania, musi istnieć rozpoczęcia zadania oraz zdarzenia rozpocząć ułożony. Na przykład jeśli używasz magazyn obiektów Blob jako wynik zadania nie utworzy konto miejsca do magazynowania automatycznie. Musi być utworzone przez użytkownika, przed rozpoczęciem zadania ASA.

## <a name="get-help"></a>Uzyskiwanie pomocy
Aby uzyskać dodatkową pomoc spróbuj nasze [forum analizy strumieniu Azure](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Następne kroki

- [Wprowadzenie do analizy strumieniu Azure](stream-analytics-introduction.md)
- [Wprowadzenie do korzystania z analizy strumieniu Azure](stream-analytics-get-started.md)
- [Skalowanie Azure analizy strumieniu zadania](stream-analytics-scale-jobs.md)
- [Dokumentacja języka kwerend analizy strumieniu Azure](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Strumień Azure analizy zarządzania pozostałych interfejsu API odwołania](https://msdn.microsoft.com/library/azure/dn835031.aspx)
