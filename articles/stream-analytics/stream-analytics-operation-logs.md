<properties 
    pageTitle="Debugowanie przy użyciu operacji i usługa Dzienniki w analizy strumieniu | Microsoft Azure" 
    description="Dzienniki operacji analizy strumieniu instrukcje użycia" 
    keywords="dzienniki usługi"
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

# <a name="debug-stream-analytics-jobs-using-service-and-operation-logs"></a>Debugowanie analizy strumieniu zadań przy użyciu usługi i działania dzienników

Wszystkie usługi Azure dostarczania wiadomości operacyjne rejestrowania dla użytkowników do rejestrowania szczegółów związanych z operacjami zarządzania. Do analizy strumieniu Azure te informacje może być używany do debugowania, takie jak stan zadania, informacje o postępie zadania, wyświetlanie i komunikaty o błędach do śledzenia postępu zadania w czasie, z rozpoczęcie przetwarzania w celu wyprowadzenia.

## <a name="find-operation-logs-in-the-azure-management-portal"></a>Znajdowanie dzienniki operacji w portalu zarządzania Azure

Dzienniki operacji jest możliwy na dwa sposoby:  

- Pulpit nawigacyjny zadania analizy strumieniu  
- Usługi zarządzania w portalu Azure klasyczny  

## <a name="dashboard-of-the-stream-analytics-job"></a>Pulpit nawigacyjny zadania analizy strumieniu

Łącze do odpowiednich dzienników analizy strumieniu zadania jest wyświetlany na karta Pulpit nawigacyjny zadania. Kliknięcie tego łącza, będzie Ustaw filtry, w ten sposób, że jest wyświetlany najnowszą dzienniki dla tego określonego zadania.

  ![Wybierz pozycję dzienniki usługi zarządzania](./media/stream-analytics-operation-logs/01-stream-analytics-operation-logs.png)  

## <a name="management-services"></a>Usługi zarządzania

Aby ręcznie przejść do dzienników operacji analizy strumieniu i inne usługi w portalu Azure klasyczny:

1.  Polecenie **Usługi zarządzania** w [portal Azure klasyczny](https://manage.windowsazure.com).
2.  Wybierz pozycję **Analizy strumieniu** **Typ** , a nazwa zadania dla **Nazwy usługi**.  

  ![Wybierz pozycję analizy strumieniu](./media/stream-analytics-operation-logs/02-stream-analytics-operation-logs.png)  

## <a name="find-audit-logs-in-the-azure-portal"></a>Znajdowanie dzienników inspekcji w portalu Azure ##

Aby znaleźć dzienniki operacyjne zadania analizy strumieniu Azure portal, kliknij przycisk **Przejdź** , a następnie wybierz pozycję **Dzienniki inspekcji**.

  ![Portal Azure analizy strumieniu wybierz](./media/stream-analytics-operation-logs/06-stream-analytics-operation-logs.png)  

Spowoduje to otwarcie karta wydarzeń w ciągu ostatnich 7 dni dla wszystkich zasobów w ramach subskrypcji.  Można filtrować, aby przeglądać zdarzenia typu określ lub przedziału czasu, klikając polecenie **Filtruj** .

  ![Portal Azure analizy strumieniu wybierz](./media/stream-analytics-operation-logs/07-stream-analytics-operation-logs.png)  

## <a name="get-log-details"></a>Wyświetlanie szczegółów dziennika

Można filtrować według przedziału czasu i stan, aby wyświetlić dzienniki dla zadania.

W portalu zarządzania Azure kliknij przycisk **Szczegóły** u dołu okna, aby wyświetlić więcej szczegółów dotyczących wybranego zdarzenia. 

  ![Wybierz pozycję Szczegóły](./media/stream-analytics-operation-logs/03-stream-analytics-operation-logs.png)  

W portalu Azure kliknij pozycję dziennika, aby wyświetlić szczegółowe zdarzenia w nim.

  ![Portal Azure Wybierz szczegóły](./media/stream-analytics-operation-logs/08-stream-analytics-operation-logs.png)  

W tym miejscu możesz otworzyć karta **Szczegóły** , klikając pozycję na zdarzenie.

  ![Portal Azure Wybierz szczegóły](./media/stream-analytics-operation-logs/09-stream-analytics-operation-logs.png)  

## <a name="debug-a-failed-job"></a>Debugowanie zadania zakończonego niepowodzeniem

W portalu zarządzania Azure kliknij ikonę wyszukiwania i typ "nie powiodło się". Daje w wyniku wszystkich dzienników z błędami. 

  ![Debugowanie zadania zakończonego niepowodzeniem](./media/stream-analytics-operation-logs/04-stream-analytics-operation-logs.png)  

W portalu usługi Azure można filtrować według poziomu wiadomość, aby wyświetlić zdarzenia **krytyczne** .

  ![Azure debugowania portalu](./media/stream-analytics-operation-logs/10-stream-analytics-operation-logs.png)  

Można wybrać dowolny błędów, a następnie kliknij polecenie **Szczegóły** , aby uzyskać więcej informacji o błędzie.  Komunikaty o błędach zapewniają również uzyskać informacje na temat zmniejszeniu problem. 

  ![Szczegóły operacji](./media/stream-analytics-operation-logs/05-stream-analytics-operation-logs.png)  

W przypadku, gdy trzeba kontakt z [pomocą techniczną](https://azure.microsoft.com/support/options/) lub przekazanie informacji do zespołu za pośrednictwem [forum w witrynie MSDN](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics), pamiętaj, szczegóły operacji, w szczególności **Identyfikator korelacji**. 

## <a name="get-help"></a>Uzyskiwanie pomocy
Aby uzyskać dodatkową pomoc spróbuj nasze [forum analizy strumieniu Azure](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Następne kroki

- [Wprowadzenie do analizy strumieniu Azure](stream-analytics-introduction.md)
- [Wprowadzenie do korzystania z analizy strumieniu Azure](stream-analytics-get-started.md)
- [Skalowanie Azure analizy strumieniu zadania](stream-analytics-scale-jobs.md)
- [Dokumentacja języka kwerend analizy strumieniu Azure](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Strumień Azure analizy zarządzania pozostałych interfejsu API odwołania](https://msdn.microsoft.com/library/azure/dn835031.aspx)
