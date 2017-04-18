<properties 
    pageTitle="Pojazdu telemetrycznego analizy rozwiązanie playbook | Microsoft Azure" 
    description="Skorzystać z funkcji analizy Cortana uzyskanie wniosków w czasie rzeczywistym i przewidywanych ds pojazdu bo nawyków." 
    services="machine-learning" 
    documentationCenter="" 
    authors="bradsev" 
    manager="jhubbard" 
    editor="cgronlun" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/12/2016" 
    ms.author="bradsev" />


# <a name="vehicle-telemetry-analytics-solution-playbook"></a>Pojazdu telemetrycznego analizy rozwiązanie playbook

Ten **menu** linki do rozdziały w tym playbook. 

[AZURE.INCLUDE [cap-vehicle-telemetry-playbook-selector](../../includes/cap-vehicle-telemetry-playbook-selector.md)]

## <a name="overview"></a>Omówienie
Komputery Super zostały przeniesione poza ćwiczenia i element obecnie zaparkowany w naszym garaż! Te przełomowych samochodów zawierają dużą ilość czujników, umożliwiając śledzenie i monitorowanie miliony zdarzeń co sekundę. Oczekuje się, że 2020, większość tych samochodów będzie mieć został połączony z Internetem. Załóżmy naciskając w tym wiele dane, aby podać najważniejsze klasy bezpieczeństwa, niezawodności i obsługi kierowania! Microsoft wprowadził to marzy reality z Cortany analizy.

Analizy Cortana firmy Microsoft jest w pełni zarządzane big data i zestaw zaawansowanej analizy, który umożliwia przekształcenie danych inteligentnego akcji. Chcemy przedstawiono szablon rozwiązanie Cortana analizy pojazdu telemetrycznego analizy. To rozwiązanie pokazano, jak dealerów samochód, producentów samochodów i ubezpieczeń, można skorzystać z możliwości analizy Cortana uzyskanie w czasie rzeczywistym i nawyków przewidywanych rozeznanie pojazdu zdrowia i prowadzenie. 

Rozwiązanie jest zaimplementowana jako [Deseń architektura lambda](https://en.wikipedia.org/wiki/Lambda_architecture) przedstawiający pełnego potencjalne platformy analizy Cortany w czasie rzeczywistym i przetwarzanie wsadowe. Rozwiązanie: 

- udostępnia simulator telematyki pojazdu
- wykorzystuje koncentratory zdarzeń dla ingesting miliony symulowany pojazdu telemetrycznego zdarzeń do Azure 
- używa analizy strumieniu uzyskanie wniosków w czasie rzeczywistym na zdrowie pojazdu
-  będzie nadal występował dane do długotrwałych miejsca do magazynowania dla bardziej rozbudowane analizy partię. 
- korzysta z komputera nauki dla wykrywania anomalii w czasie rzeczywistym i partii przetwarzania uzyskanie wniosków przewidywanych.
- korzysta z usługi HDInsight do przekształcania danych w skali i Factory danych powinien obsługiwać aranżacji, Planowanie zarządzania zasobami i monitorowanie potoku przetwarzanie wsadowe 
- zapewnia to rozwiązanie sformatowanego pulpitu nawigacyjnego dane czasu rzeczywistego i wizualizacji przewidywanych analizy za pomocą usługi Power BI

## <a name="architecture"></a>Architektura

![](./media/cortana-analytics-playbook-vehicle-telemetry/fig1-vehicle-telemetry-annalytics-solution-architecture.png)
*Rysunek 1 — architektury rozwiązania analizy telemetrycznego pojazdu*

To rozwiązanie zawiera następujące **składniki analizy Cortana** i ilustrację ich integracji kompleksowego


- **Koncentratory zdarzeń** dla ingesting miliony pojazdu telemetrycznego zdarzeń do Azure.
- **Analizy strumieniu** do uzyskania w czasie rzeczywistym wniosków na pojazdu zdrowie i będzie nadal występował te dane do długotrwałych miejsca do magazynowania dla bardziej rozbudowane analizy partię.
- **Nauka maszynowego** wykrywania anomalii w czasie rzeczywistym i przetwarzanie wsadowe uzyskanie wniosków przewidywanych.
- **Usługa HDInsight** jest użyć do przekształcania danych w większej skali
- **Factory danych** obsługuje aranżacji, Planowanie zarządzania zasobami i monitorowanie potoku przetwarzanie wsadowe.
- **Power BI** zapewnia tego rozwiązania sformatowanego pulpitu nawigacyjnego w czasie rzeczywistym danych i wizualizacje przewidywanych analizy.

To rozwiązanie uzyskuje dostęp do dwóch różnych **źródeł danych**: 

- **Symulowany sygnały pojazdu i diagnostyki**: simulator telematyki pojazdu emituje informacje diagnostyczne i sygnały, które odpowiadają stan pojazdu i kierowania deseniu w danym punkcie w czasie. 
- **Wykaz pojazdu**: zestaw danych odwołania zawierające VIN mapowanie modelu.
