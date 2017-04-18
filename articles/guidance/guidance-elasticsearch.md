
<properties
   pageTitle="Elasticsearch zaleceniami Azure | Microsoft Azure"
   description="Elasticsearch zaleceniami Azure."
   services=""
   documentationCenter="na"
   authors="dragon119"
   manager="bennage"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/22/2016"
   ms.author="masashin"/>

# <a name="elasticsearch-on-azure-guidance"></a>Elasticsearch zaleceniami Azure 

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Elasticsearch jest wysoce skalowalna Otwórz źródło wyszukiwarki i bazy danych. Okazuje się przydatne w sytuacjach, które wymagają szybkiego analizowania i odnajdowanie informacji przechowywanych w dużych zestawów danych. Te wskazówki analizuje niektóre aspektami brać pod uwagę podczas projektowania klastrze Elasticsearch z fokusem na sposobów optymalizacji i Przetestuj konfigurację.

> [AZURE.NOTE]Te wskazówki przyjęto założenie, niektóre podstawowe znajomości Elasticsearch. Aby uzyskać więcej szczegółowych informacji odwiedź stronę [Elasticsearch witryny sieci Web](https://www.elastic.co/products/elasticsearch). 

- **[Uruchomione Elasticsearch Azure][]** zawiera krótkie wprowadzenie do ogólnej struktury Elasticsearch i opisano, jak zaimplementować klaster Elasticsearch przy użyciu Azure. 
- **[Wydajność spożyciu danych dostrajania Elasticsearch Azure][]** w tym artykule opisano wdrożenia programu klaster Elasticsearch i bardziej szczegółowej analizy różne opcje konfiguracji brać pod uwagę podczas można się spodziewać dużą liczbę spożyciu danych.
- **[Agregacja danych dostrajania i wydajności kwerend dla Elasticsearch Azure][]** oferuje bardziej szczegółowej analizy opcji do rozważenia podczas podejmowania jak zoptymalizować systemu wydajność kwerend i wyszukiwania.
- **[Konfigurowanie odporność i odzyskiwania Elasticsearch Azure][]** opisano niektóre ważne funkcje klaster Elasticsearch, które mogą być pomocne zminimalizować prawdopodobieństwo utraty danych i godzin odzyskiwania danych rozszerzonych.
- **[Tworzenie środowisku testów wydajności dla Elasticsearch Azure][]** opisano, jak skonfigurować środowisko do testowania spożyciu danych wydajności i obciążenia kwerendy w klastrze Elasticsearch. 
- **[Implementacji JMeter testowanie plan Elasticsearch][]** podsumowano uruchomionych testów wydajności zaimplementować za pomocą planów testu JMeter razem z kodu języka Java dołączane jako test JUnit do wykonywania zadań, takich jak przekazywanie danych w grupie Elasticsearch.
- **[Wdrażanie przykłady JMeter JUnit do testowania wydajności Elasticsearch][]** opisano tworzenie i używanie przykłady JUnit, którą można wygenerować i przekazywać do klastrów Elasticsearch danych. Dzięki temu wysoce elastyczne podejście załadować badań, generowany dużych ilości danych test bez w zależności od plików danych zewnętrznych. 
- **[Uruchamianie testów elastyczność Elasticsearch][]** podsumowano sposoby Uruchom testy elastyczność używane w tej serii. 
- **[Uruchamianie testów wydajności Elasticsearch][]** podsumowano sposoby uruchomieniu testów wydajności używane w tej serii.


[Uruchamianie Elasticsearch Azure]: guidance-elasticsearch-running-on-azure.md
[Dostosowywanie wydajności spożyciu danych dla Elasticsearch Azure]: guidance-elasticsearch-tuning-data-ingestion-performance.md
[Tworzenie testowania środowiska dla Elasticsearch Azure]: guidance-elasticsearch-creating-performance-testing-environment.md
[Wykonywania planu testowania JMeter do Elasticsearch]: guidance-elasticsearch-implementing-jmeter-test-plan.md
[Rozmieszczanie przykłady JMeter JUnit dla testów wydajności Elasticsearch]: guidance-elasticsearch-deploying-jmeter-junit-sampler.md
[Dostosowywanie agregacji danych i wydajność kwerendy dla Elasticsearch Azure]: guidance-elasticsearch-tuning-data-aggregation-and-query-performance.md
[Konfigurowanie odporność i odzyskiwania na Elasticsearch Azure]: guidance-elasticsearch-configuring-resilience-and-recovery.md
[Uruchamianie testów elastyczność automatyczną Elasticsearch]: guidance-elasticsearch-running-automated-resilience-tests.md
[Uruchamianie automatyczne Elasticsearch testów wydajności]: guidance-elasticsearch-running-automated-performance-tests.md
