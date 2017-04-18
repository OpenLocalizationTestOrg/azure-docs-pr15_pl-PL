<properties
   pageTitle="Używanie Factory Azure danych z magazynu danych SQL | Microsoft Azure"
   description="Porady dotyczące używania Azure Factory danych (ADF) z magazynu danych SQL Azure dla opracowania rozwiązań."
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
   ms.date="08/08/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# <a name="use-azure-data-factory-with-sql-data-warehouse"></a>Używanie fabrycznych Azure danych z magazynu danych SQL

Azure Factory danych zapewnia w pełni zarządzane metodę orchestrating transfer danych i wykonanie procedur składowanych w magazynie danych SQL.  Dzięki temu będzie można łatwiej konfiguracji i planowanie złożonych wyodrębnić przekształcenie i ładowania (ETL) procedur z magazynu danych SQL. Dokładniejszy omówienie Factory danych Azure zobacz [dokumentację Factory danych Azure][].

## <a name="data-movement"></a>Przenoszenie danych

Azure fabrycznych danych umożliwia przenoszenie danych między zarówno lokalnych źródeł i różnych usług Azure.  Ogólny, bieżącą Integracja z fabrycznych danych Azure obsługuje przenoszenie danych do i z następujących lokalizacji:

+ Magazyn obiektów blob platformy Azure
+ Baza danych SQL Azure
+ Lokalnego programu SQL Server
+ Program SQL Server na IaaS

Aby uzyskać informacje dotyczące konfigurowania danych aktywności kopii zobacz [Kopiowanie danych za pomocą fabrycznych danych Azure][]

## <a name="stored-procedures"></a>Procedury składowane
 W ten sam sposób, który może służyć do planowania transfer danych Azure Factory danych można również dodać akompaniament wykonanie procedur składowanych.  Dzięki temu bardziej złożone procesy ma zostać utworzony, a rozszerza możliwości Azure danych Factory możliwości obliczeniowych magazynu danych SQL.

## <a name="next-steps"></a>Następne kroki
Aby uzyskać omówienie integracji zobacz [Omówienie integracji magazynu danych SQL][].
Aby uzyskać więcej porad rozwoju zobacz [Omówienie rozwoju magazynu danych SQL][].

<!--Image references-->

<!--Article references-->

[Skopiuj dane z Factory danych Azure]: ../data-factory/data-factory-data-movement-activities.md
[Omówienie tworzenia magazynu danych SQL]: ./sql-data-warehouse-overview-develop.md
[Omówienie integracji z magazynu danych SQL]: ./sql-data-warehouse-overview-integrate.md

<!--MSDN references-->

<!--Other Web references-->
[Azure dokumentacji Factory danych]:https://azure.microsoft.com/documentation/services/data-factory/

