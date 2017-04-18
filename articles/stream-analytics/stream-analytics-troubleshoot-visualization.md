<properties
    pageTitle="Wizualizowanie i rozwiązywanie problemów z zadaniami analizy strumieniu | Microsoft Azure"
    description="Dowiedz się, jak wizualizowanie potok analizy strumieniu zadania na samodzielne rozwiązywanie problemów za pomocą funkcji Diagnostyka diagramu."
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
    ms.date="09/26/2016"
    ms.author="jeffstok"
/>


# <a name="visualize-and-troubleshoot-stream-analytics-jobs"></a>Wizualizowanie i rozwiązywanie problemów z analizy strumieniu zadania

W analizy strumieniu podobnie jak w przypadku innych technologii opartej na chmurze, rozwiązywanie problemów z jest czasami potrzebne do przeanalizowania dlaczego zadania warzywa nie oczekiwany wynik (lub dowolne dane wyjściowe dla tej sprawie). Pamiętając o tym analizy strumieniu obsługuje funkcję do wizualizacji przesyłanie strumieniowe zadania. To jest również przydatne jako narzędzie do modelowania i rozwiązanie po stronie tych wymagające dokumentacji swojej pracy.

W panelu wizualizacji danych wejściowych są widoczne oraz zapytania, a następnie wszystkie wyjściowe skonfigurowane. Problemy z łącznością i konfiguracji może okazać się więcej, a także może być przydatne wyświetlić będący wizualną reprezentacją konfiguracji.

## <a name="using-the-diagnosis-diagram-tool"></a>Za pomocą narzędzia Diagnostyka diagramu

Aby uzyskać dostęp do tego Wizualizator, po prostu kliknij przycisk "Diagnostyki diagram" w karta "Ustawienia" części zadania analizy strumieniu.

![Stream-Analytics-Troubleshoot-visualization-Diagnosis-diagram](./media/stream-analytics-troubleshoot-visualization/stream-analytics-troubleshoot-visualization-diagnosis-diagram1.png)

Co dane wejściowe i wyjściowe jest kolorami wskaż bieżący stan tego składnika, tak jak pokazano poniżej.

![Stream-Analytics-Troubleshoot-visualization-Input-map](./media/stream-analytics-troubleshoot-visualization/stream-analytics-troubleshoot-visualization-input-map.png)

Gdy użytkownik chce przeglądać kroków zapytania pośrednie zrozumieć wzorce przepływu danych wewnątrz zadanie, narzędzie do wizualizacji udostępnia widok podziału kwerendy do jego kroków składnik i sekwencji przepływu. Kliknięcie na każdym kroku zapytania spowoduje wyświetlenie odpowiedniej sekcji w kwerendzie okienko przedstawionym do edycji. 

![Stream-Analytics-Troubleshoot-visualization-Intermediate-Steps](./media/stream-analytics-troubleshoot-visualization/stream-analytics-troubleshoot-visualization-intermediate-steps.png)




## <a name="next-steps"></a>Następne kroki

- [Wprowadzenie do analizy strumieniu Azure](stream-analytics-introduction.md)
- [Wprowadzenie do korzystania z analizy strumieniu Azure](stream-analytics-get-started.md)
- [Skalowanie Azure analizy strumieniu zadania](stream-analytics-scale-jobs.md)
- [Dokumentacja języka kwerend analizy strumieniu Azure](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Strumień Azure analizy zarządzania pozostałych interfejsu API odwołania](https://msdn.microsoft.com/library/azure/dn835031.aspx)
