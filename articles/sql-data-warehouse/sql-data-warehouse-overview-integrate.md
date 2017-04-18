<properties
   pageTitle="Tworzenie rozwiązań zintegrowane z magazynu danych SQL | Microsoft Azure"
   description="Narzędzia i partnerom rozwiązań, które zostały zintegrowane z magazynu danych SQL. "
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="05/31/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

#<a name="leverage-other-services-with-sql-data-warehouse"></a>Korzystać z innych usług z magazynu danych SQL
Poza podstawowe funkcje magazynu danych SQL pozwala korzystać z wielu innych usług Azure obok.  W szczególności Pracujemy obecnie wykonywane czynności, aby mocno Integracja z następujących czynności:

+ Power BI
+ Factory Azure danych
+ Nauka Azure komputera
+ Analizy strumieniu Azure

Pracujemy nad do nawiązywania połączeń z innych usług w ekosystemie Azure.

##<a name="power-bi"></a>Power BI
Power BI integracji pozwala korzystać z obliczeń możliwości magazynu danych SQL z dynamicznego raportowania i wizualizacji usługi Power BI. Power BI integracji obecnie zawiera:

+ **Nawiązywanie bezpośredniego połączenia**: bardziej zaawansowane połączenie z logiczne przekazywanie przed magazynu danych SQL.  Zapewnia szybsze analizy w większej skali.
+ **Otwieranie w usłudze Power BI**: przycisku "Otwórz w usłudze Power BI" przekazuje informacje o wystąpieniu do usługi Power BI umożliwiające płynniejszą połączenia.

Zobacz [Integracja usługi Power BI](./sql-data-warehouse-integrate-power-bi.md) lub [dokumentacji usługi Power BI](http://blogs.msdn.com/b/powerbi/archive/2015/06/24/exploring-azure-sql-data-warehouse-with-power-bi.aspx) , aby uzyskać więcej informacji.

##<a name="azure-data-factory"></a>Factory Azure danych
Azure Factory danych umożliwia użytkownikom zarządzanych platformy do tworzenia złożonych procesy ładowania Wyodrębnij.  Integracja magazynu danych SQL z Azure Factory danych jest następująca:

+ **Procedury składowane**: dodać akompaniament wykonanie procedur składowanych w magazynie danych SQL.
+ **Kopiowanie**: używanie ADF przenoszenie danych do magazynu danych SQL.  Operacja za pomocą mechanizm standardowych danych firmy ADF lub PolyBase w obszarze okładki. 

Zobacz [Integracja z Factory danych Azure](./sql-data-warehouse-integrate-azure-data-factory.md) lub [dokumentacji Factory danych Azure](https://azure.microsoft.com/documentation/services/data-factory/) uzyskać więcej informacji.

##<a name="azure-machine-learning"></a>Nauka Azure komputera
Azure nauki komputera to usługa analizy w pełni zarządzane, która pozwala użytkownikom na tworzenie skomplikowanych modeli używanie dużych zestaw narzędzi przewidywanych.  Program SQL Data Warehouse jest obsługiwane jako źródła i miejsca docelowego dla tych modeli z następujących funkcji:

+ **Więcej danych:** Dysk modeli w skali przy użyciu T-SQL względem magazynu danych SQL.
+ **Zapisywanie danych:** Zatwierdź zmiany z każdego modelu powrót do magazynu danych SQL.

Zobacz [Integracja z Azure maszynowego uczenia](./sql-data-warehouse-integrate-azure-machine-learning.md) lub [dokumentacji Azure maszynowego uczenia](https://azure.microsoft.com/services/machine-learning/) , aby uzyskać więcej informacji.

##<a name="azure-stream-analytics"></a>Analizy strumieniu Azure
Azure analizy strumieniu jest złożone, w pełni zarządzane infrastruktury przetwarzania i przez inne dane zdarzenia wygenerowane z Centrum zdarzeń Azure.  Integracja z magazynu danych SQL umożliwia strumieniowe przesyłanie danych w celu efektywnego przetwarzane i przechowywane razem z pakietem Włączanie szczegółowego, bardziej zaawansowanych analiz danych relacyjnych.  

+ **Zadania wynik:** Wyślij dane wyjściowe z zadań analizy strumieniu bezpośrednio do magazynu danych SQL.

Zobacz [Integracja z analizy strumieniu Azure](./sql-data-warehouse-integrate-azure-stream-analytics.md) lub [dokumentacji Azure analizy strumieniu](https://azure.microsoft.com/documentation/services/stream-analytics/) uzyskać więcej informacji.

<!--Image references-->

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop/

[Azure Data Factory]: sql-data-warehouse-integrate-azure-data-factory.md
[Azure Machine Learning]: sql-data-warehouse-integrate-azure-machine-learning.md
[Azure Stream Analytics]: sql-data-warehouse-integrate-azure-stream-analytics.md
[Power BI]: sql-data-warehouse-integrate-power-bi.md
[Partners]: sql-data-warehouse-partner-business-intelligence.md

<!--MSDN references-->

<!--Other Web references-->
