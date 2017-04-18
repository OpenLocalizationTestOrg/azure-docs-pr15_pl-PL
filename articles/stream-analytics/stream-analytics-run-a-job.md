<properties 
    pageTitle="Jak rozpocząć strumieniowego przesyłania zadań w analizy strumieniu | Microsoft Azure" 
    description="Jak uruchomić przesyłanie strumieniowe zadanie do analizy strumieniu Azure | Nauka segmentu ścieżki."
    keywords="przesyłanie strumieniowe zadania"
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

# <a name="how-to-run-a-streaming-job-in-azure-stream-analytics"></a>Jak uruchomić przesyłanie strumieniowe zadania w analizy strumieniu Azure

Podczas wprowadzania zadania, kwerendy i wyjście wszystkich określono że można rozpocząć zadania analizy strumieniu.

Aby rozpocząć zadania:

1.  W portalu Azure klasyczny na pulpicie nawigacyjnym zadania kliknij pozycję **Uruchom** w dolnej części strony.

    ![Rozpoczęcie zadania przycisk](./media/stream-analytics-run-a-job/1-stream-analytics-run-a-job.png)  

    W portalu usługi Azure kliknij przycisk **Start** , w górnej części strony zadania.

    ![Azure portalu rozpoczęcia zadania przycisk](./media/stream-analytics-run-a-job/4-stream-analytics-run-a-job.png)  

2.  Określ wartość **Rozpocząć dane wyjściowe** , aby określić, kiedy to zadanie rozpocznie się generuje danych wyjściowych. Ustawieniem domyślnym dla zadania, które wcześniej nie zostały uruchomione jest **Czas rozpoczęcia zadania**, co oznacza, że zadanie natychmiast rozpocznie się przetwarzanie danych. Można również określić czas **niestandardowe** w przeszłości (umożliwiającego korzystanie z danych historycznych) lub przyszłości (opóźnienie przetwarzanie przyszłych czasu). W przypadku gdy zadanie została już uruchomiona i zatrzymana, opcja **Ostatniego zatrzymania** jest dostępna Aby wznowić zadanie z ostatniego dane wyjściowe i uniknąć utraty danych.  

    ![Rozpocznij przesyłanie strumieniowe zadania czasu](./media/stream-analytics-run-a-job/2-stream-analytics-run-a-job.png)  

    ![Azure portalu Rozpocznij przesyłanie strumieniowe zadania czasu](./media/stream-analytics-run-a-job/5-stream-analytics-run-a-job.png)  

3.  Potwierdź wybór. Stan zadania zostanie zmieniony na *Uruchamianie* i wkrótce zostaną przeniesione do *uruchamiania* po rozpoczęciu zadania. Można monitorować postęp **Rozpoczynanie** operacji w **Centrum powiadomień**:

    ![przesyłanie strumieniowe postępu zadania](./media/stream-analytics-run-a-job/3-stream-analytics-run-a-job.png)  

    ![Portal Azure streaming postępu zadania](./media/stream-analytics-run-a-job/6-stream-analytics-run-a-job.png)  

## <a name="get-help"></a>Uzyskiwanie pomocy
Aby uzyskać dodatkową pomoc spróbuj nasze [forum analizy strumieniu Azure](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Następne kroki

- [Wprowadzenie do analizy strumieniu Azure](stream-analytics-introduction.md)
- [Wprowadzenie do korzystania z analizy strumieniu Azure](stream-analytics-get-started.md)
- [Skalowanie Azure analizy strumieniu zadania](stream-analytics-scale-jobs.md)
- [Dokumentacja języka kwerend analizy strumieniu Azure](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Strumień Azure analizy zarządzania pozostałych interfejsu API odwołania](https://msdn.microsoft.com/library/azure/dn835031.aspx)
