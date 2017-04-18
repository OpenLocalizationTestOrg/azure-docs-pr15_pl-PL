<properties
    pageTitle="Ustawianie alertów dla kwerend w analizy strumieniu | Microsoft Azure"
    description="Opis analizy strumieniu alertu"
    keywords="Ustawianie alertów"
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


# <a name="set-up-alerts-for-azure-stream-analytics-jobs"></a>Ustawianie alertów dla zadań analizy strumieniu Azure

## <a name="introduction-monitor-page"></a>Wprowadzenie: Strona Monitor

Można ustawić alerty alert, gdy metryki osiągnie określony warunek.

Na przykład "w przypadku zdarzenia wyjścia dla ostatniego 15 minut < 100, Wyślij powiadomienie pocztą e-mail do identyfikatora wiadomości e-mail: xyz@company.com”.

Reguły można skonfigurować na metryki za pośrednictwem portalu lub może być skonfigurowany [programowo](https://code.msdn.microsoft.com/windowsazure/Receive-Email-Notifications-199e2c9a) nad danymi dzienniki operacji.

## <a name="set-up-alerts-through-the-azure-classic-portal"></a>Skonfiguruj alerty za pośrednictwem portalu klasyczny Azure

Istnieją dwa sposoby skonfiguruj alerty w klasycznym Azure portalu:  

1.  Karta **Monitor** zadania analizy strumieniu  
2.  Dziennik działań w usługach zarządzania  

## <a name="set-up-alert-through-the-monitor-tab-of-the-job-in-the-portal"></a>Konfigurowanie alertów na karcie Monitor zadania w portalu

1.  Wybierz metryki na karcie monitor i kliknij przycisk **Dodaj regułę** w dolnej części pulpitu nawigacyjnego i konfigurowanie reguł.  

    ![Pulpit nawigacyjny](./media/stream-analytics-set-up-alerts/01-stream-analytics-set-up-alerts.png)  

2.  Definiowanie nazwy i opisu alertu  

    ![Tworzenie reguły](./media/stream-analytics-set-up-alerts/02-stream-analytics-set-up-alerts.png)  

3.  Wprowadź wartości progowe, okna oceny alertów i akcje alertu  

    ![Definiowanie warunków](./media/stream-analytics-set-up-alerts/03-stream-analytics-set-up-alerts.png)  

## <a name="set-up-alerts-through-the-operations-logs"></a>Konfigurowanie alertów za pomocą dzienników operacji

1.  Przejdź na kartę **alerty** w usługach zarządzania w [Klasycznym Portal Azure](https://manage.windowsazure.com).  
2.  Kliknij przycisk **Dodaj regułę**  

    ![Kryteria](./media/stream-analytics-set-up-alerts/04-stream-analytics-set-up-alerts.png)  

3.  Definiowanie nazwy i opisu alertu. Wybierz pozycję "Analizy strumieniu" jako typ usługi, a nazwa zadania jako nazwa usługi.  

    ![Definiowanie alertu](./media/stream-analytics-set-up-alerts/05-stream-analytics-set-up-alerts.png)  

## <a name="set-up-alerts-in-the-azure-portal"></a>Ustawianie alertów w portalu Azure ##

W portalu usługi Azure przejdź do zadania analizy strumieniu, który Cię interesuje alertu na, a następnie kliknij sekcję, **Monitorowanie** .  Na kartę **metryki** , która zostanie otwarta kliknij polecenie **Dodaj alert** .

  ![Ustawienia portal Azure](./media/stream-analytics-set-up-alerts/06-stream-analytics-set-up-alerts.png)  

Nazwa reguły alertów i wybierz opis, który pojawi się w wiadomość e-mail z powiadomieniem.

Po wybraniu metryki będzie wybierz warunek i próg wartość metryki.

  ![Azure portalu wybierz pozycję metryczne](./media/stream-analytics-set-up-alerts/07-stream-analytics-set-up-alerts.png)  

Więcej szczegółowych informacji na temat konfigurowania alertów w portalu Azure zobacz [odbieranie alertów](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).  

## <a name="get-help"></a>Uzyskiwanie pomocy
Aby uzyskać dodatkową pomoc spróbuj nasze [forum analizy strumieniu Azure](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Następne kroki

- [Wprowadzenie do analizy strumieniu Azure](stream-analytics-introduction.md)
- [Wprowadzenie do korzystania z analizy strumieniu Azure](stream-analytics-get-started.md)
- [Skalowanie Azure analizy strumieniu zadania](stream-analytics-scale-jobs.md)
- [Dokumentacja języka kwerend analizy strumieniu Azure](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Strumień Azure analizy zarządzania pozostałych interfejsu API odwołania](https://msdn.microsoft.com/library/azure/dn835031.aspx)
