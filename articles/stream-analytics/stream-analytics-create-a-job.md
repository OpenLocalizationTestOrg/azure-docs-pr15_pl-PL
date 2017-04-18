<properties 
    pageTitle="Jak utworzyć zadanie przetwarzania analizy danych dla analizy strumieniu | Microsoft Azure" 
    description="Tworzenie zadania przetwarzania analizy danych dla analizy strumieniu | Nauka segmentu ścieżki."
    keywords="przetwarzanie analizy danych"
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

# <a name="how-to-create-a-data-analytics-processing-job-for-stream-analytics"></a>Jak utworzyć zadanie przetwarzania analizy danych dla analizy strumieniu

Zasobów najwyższego poziomu w Azure analizy strumieniu jest zadaniem analizy strumieniu.  Składa się z jednego lub kilku źródeł danych wejściowych, kwerendy wyrażanie przekształcenie danych i cele dane wyjściowe, które wyniki są zapisywane w. Razem te umożliwiają przeprowadzanie analiz danych przetwarzania przesyłania strumieniowego scenariuszy danych.

Aby rozpocząć korzystanie z analizy strumieniu, Rozpocznij, tworząc nowe zadanie analizy strumieniu.  Należy zauważyć, że ta akcja ma nie skutków rozliczeń do momentu rozpoczęcia zadania.

1.  Zaloguj się na online [portal Azure klasyczny](http://manage.windowsazure.com) lub [Azure portal](https://portal.azure.com/).
2.  W portalu: kliknięciu **Usług danych** lub **Analizy danych** , w zależności od portalem **Kliknij przycisk Nowy**, a następnie kliknij pozycję **Analizy strumieniu Azure** lub **Analizy strumieniu** , a następnie **Szybkiego tworzenia**.

    ![Przetwarzanie analizy danych Kreatora zadań](./media/stream-analytics-create-a-job/1-stream-analytics-create-a-job.png)  

    ![Tworzenie zadanie przetwarzania analizy danych](./media/stream-analytics-create-a-job/4-stream-analytics-create-a-job.png)  

3.  Określ żądaną konfiguracją zadania analizy strumieniu.
    - W polu **Nazwa zadania** wprowadź nazwę identyfikującą zadanie analizy strumieniu. Po uwierzytelnieniu **Nazwa zadania** w polu Nazwa zadania zostanie wyświetlony zielony znacznik wyboru. **Nazwa zadania** może zawierać tylko znaki alfanumeryczne oraz "-" znak, a musi być od 3 do 63 znaków.
    - Użyj **regionu** Azure portal lub **lokalizacji** w portalu Azure Określ lokalizację geograficzną, której chcesz uruchomić zadanie.
    - Jeśli używasz Azure portal, wybierz lub Utwórz konto miejsca do magazynowania ma być używane jako **Konto regionalne monitorowanie miejsca do magazynowania**. To konto przestrzeni dyskowej służy do przechowywania danych monitorowania dla wszystkich zadań analizy strumieniu uruchomione w tym regionie.
    - Jeśli używasz Azure portal, określ nowej lub istniejącej **Grupy zasobów** do przechowywania zasoby pokrewne dla aplikacji.

4.  Po nowych opcji zadania analizy strumieniu są skonfigurowane, kliknij przycisk **Utwórz zadanie analizy strumieniu**. Może potrwać kilka minut można utworzyć zadanie analizy strumieniu. Aby sprawdzić stan, można monitorować postęp w Centrum powiadomienia.

    ![Dane analizy przetwarzanie zadania notfications Centrum](./media/stream-analytics-create-a-job/2-stream-analytics-create-a-job.png)  

    ![Azure analizy danych portalu przetwarzania zadania utworzyć zadanie](./media/stream-analytics-create-a-job/5-stream-analytics-create-a-job.png)  

5.  Nowe zadanie pojawi się ze stanem **utworzone**. Zwróć uwagę, że przycisk **Start** jest wyłączona. Przed rozpoczęciem tego zadania musisz skonfigurować zadania wejście, kwerendy i dane wyjściowe.

    ![Zadanie przetwarzania analizy danych stanu](./media/stream-analytics-create-a-job/3-stream-analytics-create-a-job.png)  

    ![Azure analizy danych portalu przetwarzania stanu zadań wykonywania zadania](./media/stream-analytics-create-a-job/6-stream-analytics-create-a-job.png)  

## <a name="get-help"></a>Uzyskiwanie pomocy
Aby uzyskać dodatkową pomoc spróbuj nasze [forum analizy strumieniu Azure](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Następne kroki

- [Wprowadzenie do analizy strumieniu Azure](stream-analytics-introduction.md)
- [Wprowadzenie do korzystania z analizy strumieniu Azure](stream-analytics-get-started.md)
- [Skalowanie Azure analizy strumieniu zadania](stream-analytics-scale-jobs.md)
- [Dokumentacja języka kwerend analizy strumieniu Azure](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Strumień Azure analizy zarządzania pozostałych interfejsu API odwołania](https://msdn.microsoft.com/library/azure/dn835031.aspx)
