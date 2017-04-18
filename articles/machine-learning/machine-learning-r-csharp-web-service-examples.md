<properties 
    pageTitle="Przykłady skonstruowane za pomocą R usług sieci web uczenia komputera | Microsoft Azure" 
    description="Znajdź przydatne zestaw web przykłady usług utworzone za pomocą kodu R i nauka komputera i publikowane Azure Marketplace." 
    keywords="języka CSharp, kod r przykłady usług sieci web"
    services="machine-learning" 
    documentationCenter="" 
    authors="jaymathe" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/14/2016" 
    ms.author="jaymathe"/> 


#<a name="web-services-examples-using-r-code-on-azure-machine-learning-and-published-to-microsoft-azure-marketplace"></a>Sieci Web usługi przykłady użycia kodu R po otrzymaniu komputera Azure i opublikowany w programie Microsoft Azure Marketplace

W tym artykule przedstawiono przykład web usług utworzone za pomocą Azure maszynowego uczenia i opublikowany w Azure Marketplace. Każdy przykład usługi sieci web ma pełna załączony dokument, osadzanie zestawów danych przykładowych do testowania usług i wyjaśnienie, jak użytkownik może utworzyć podobną usługę się. 

Użytkownicy mogą w Azure maszynowego uczenia Studio, pisanie kodu R i za pomocą kilku kliknięć opublikować go jako usługi sieci web dla aplikacji i urządzeń do korzystania z całego świata. 


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


Od wytwarzania kalkulatory proste, dostarczających statystycznych funkcji tworzenia niestandardowych predykcyjnych analizy upodobania wyszukiwania tekstu, zarówno nowego, jak i doświadczonych użytkowników R mogą korzystać z ułatwienia, z którym użytkownicy Azure maszynowego uczenia można operationalize kod R. Gdy te sieci web usług może być używana przez użytkowników, potencjalnie za pomocą aplikacji dla urządzeń przenośnych lub witrynę sieci Web, celem w tych przykładach usług sieci web jest pokazanie, jak można operationalize maszynowego uczenia R skrypty do analiz i można używać do tworzenia usług sieci web u góry kod R.

Każdy przykład zawiera przykład C# zużycia usługi sieci web.


![Diagram kodu R w Azure maszynowego uczenia: R rozwiązań dla zastrzeżone Użyj lub opublikować Azure Marketplace.][1]

Rozważ następujące scenariusze.

##<a name="scenario-1-generic-model"></a>Scenariusz 1: Model ogólny 
Użytkownik pracuje z ogólnego modelu, który można stosować do danych nowego użytkownika, takich jak podstawowe prognozowania czasu serii danych lub niestandardowej metody R z zaawansowanej analizy. Ten użytkownik publikuje modelu jako usługi sieci web dla innych osób korzystać z danymi.



* [Binarny klasyfikatora](machine-learning-r-csharp-binary-classifier.md)
* [Klaster modelu](machine-learning-r-csharp-cluster-model.md)
* [Wielu zmiennych regresji liniowej.](machine-learning-r-csharp-multivariate-linear-regression.md)
* [Prognozowanie — Wygładzanie wykładnicze](machine-learning-r-csharp-forecasting-exponential-smoothing.md)
* [ETS + STL prognozowania](machine-learning-r-csharp-retail-demand-forecasting.md)
* [Prognozowanie — Autoregressive zintegrowanego przenoszenia średnia (ARIMA)](machine-learning-r-csharp-arima.md)
* [Analiza o](machine-learning-r-csharp-survival-analysis.md)


##<a name="scenario-2-trained-model--specific-data"></a>Scenariusz 2: Model przeszkolony — określonych danych 
Użytkownik ma dane, które znajdują się przydatne przewidywań przez kod R, na przykład dużych próbek kwestionariuszy osobowość grupowany za pomocą algorytmu k oznacza przewidywanie typu osobowości użytkownika lub dane ankiety kondycji, który może być używany do przewidywania osoby ryzyka dla raka płuc przy użyciu pakietu analysis R o. Użytkownik publikuje danych za pomocą usługi sieci web, która przewiduje wynik nowego użytkownika.

##<a name="scenario-3-trained-model--generic-data"></a>Scenariusz 3: Model przeszkolony — ogólnego danych 
Użytkownik ma ogólnego danych (na przykład Boże tekst), które umożliwia usługi sieci web wbudowane i ogólnie stosowana różnego rodzaju przypadków użycia i scenariuszy.

* [Analiza upodobania podstawie słownika](machine-learning-r-csharp-lexicon-based-sentiment-analysis.md)

##<a name="scenario-4-advanced-calculator"></a>Scenariusz 4: Kalkulator zaawansowane 
Użytkownik udostępnia zaawansowanych obliczeń lub symulacji, które nie wymagają ani przeszkolony modelu dopasowania modelu danych użytkownika.

* [Różnica w badaniu proporcji](machine-learning-r-csharp-difference-in-two-proportions.md)
* [Pakiet rozkład normalny](machine-learning-r-csharp-normal-distribution.md)
* [Pakiet rozkład dwumianowy.](machine-learning-r-csharp-binomial-distribution.md)

##<a name="faq"></a>FAQ
Dla często zadawane pytania na temat zużycia usługi sieci web lub publikowania do witryny Marketplace programu, [w tym miejscu](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-web-service-examples/machine-learning-r-code-options-for-using-and-sharing-cloud.png


 
