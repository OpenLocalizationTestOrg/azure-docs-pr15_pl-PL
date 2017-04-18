<properties
    pageTitle="Przewidywanych rozwiązaniem ryzyko kredytowe z komputera nauki | Microsoft Azure"
    description="Szczegółowe instrukcje przedstawiający, jak utworzyć rozwiązanie przewidywanych analizy ocena ryzyka kredytowej w Azure maszynowego uczenia Studio."
    keywords="ryzyko kredytowe, rozwiązanie przewidywanych analizy i ocena ryzyka"
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/16/2016"
    ms.author="garye"/>


# <a name="walkthrough-develop-a-predictive-analytics-solution-for-credit-risk-assessment-in-azure-machine-learning"></a>Przewodnik: Utworzyć rozwiązanie przewidywanych analizy oceny ryzyka kredytowej w nauki maszynowego Azure

Załóżmy, że trzeba przewidywanie ryzyko kredytowe osoby na podstawie informacji z przydzielonej aplikacji kredytowej.  

Ocena ryzyka kredytu jest złożone problem, oczywiście, ale Przejdźmy nieco uprościć parametry pytanie. Następnie firma Microsoft może służyć jako przykład jak umożliwia Microsoft Azure maszynowego uczenia z komputera nauki Studio i usługi sieci web maszynowego uczenia utworzyć rozwiązanie przewidywanych analizy.  

W tym instruktażu szczegółowe możemy należy wykonać procesu tworzenia modelu analizy przewidywanych maszynowego uczenia Studio i wdrażanie go jako usługi sieci web Azure maszynowego uczenia. Firma Microsoft będzie rozpoczynać się dane kredytowej publicznej czynników ryzyka, opracowywanie przeszkolenie przewidywanych modelu, na podstawie tych danych i następnie wdrożeniu modelu jako usługi sieci web, który może być używany przez inne osoby ocena ryzyka kredytowej.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

<!-- -->

>[AZURE.TIP] Aby pobrać i wydrukować diagram zestawienie możliwości maszynowego uczenia Studio, zobacz [całościowego diagramu możliwości Azure maszynowego uczenia Studio](machine-learning-studio-overview-diagram.md).

Aby utworzyć rozwiązanie ocena ryzyka kredytowej, firma Microsoft będzie wykonaj następujące kroki:  

1.  [Tworzenie obszaru roboczego nauki komputera](machine-learning-walkthrough-1-create-ml-workspace.md)
2.  [Przekaż istniejące dane](machine-learning-walkthrough-2-upload-data.md)
3.  [Tworzenie nowego doświadczenia](machine-learning-walkthrough-3-create-new-experiment.md)
4.  [Szkolenie i ocenianie modeli](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5.  [Wdrażanie usługi sieci web](machine-learning-walkthrough-5-publish-web-service.md)
6.  [Dostęp do usługi sieci web](machine-learning-walkthrough-6-access-web-service.md)

W tym instruktażu jest oparty na uproszczoną wersję [dwójkową Klasyfikacja: kredytu przewidywania ryzyka](http://go.microsoft.com/fwlink/?LinkID=525270) próbki doświadczenia w [Galerii analizy Cortana](http://gallery.cortanaintelligence.com/).
