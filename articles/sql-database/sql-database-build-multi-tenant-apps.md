<properties
   pageTitle="Baza danych SQL Azure tworzy aplikacje wielu dzierżawy z izolacji i efektywności"
   description="Dowiedz się, jak baza danych SQL tworzy aplikacje wielu dzierżawy"
   keywords=""
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-management"
   ms.date="10/13/2016"
   ms.author="carlrab"/>

# <a name="builds-multi-tenant-apps-with-azure-sql-database-with-isolation-and-efficiency"></a>Tworzy aplikacje wielu dzierżawy z bazy danych Azure SQL za pomocą izolacji i wydajności

## <a name="leverage-elastic-pools-and-build-more-efficient-multi-tenant-apps"></a>Korzystać z pul elastycznych i tworzenie bardziej efektywne aplikacje wielu dzierżawy

Jeśli jesteś deweloperów władz akredytacji bezpieczeństwa, pisania aplikacji wielu dzierżawy i obsługi wielu klientów, często wprowadzane kompromisów w klienta wydajności, zarządzanie i zabezpieczeń. Z Azure SQL bazy danych elastyczne bazy danych pule nie należy tego kompromisów. Te pul ułatwiają zarządzanie i monitorowanie aplikacji w wielu dzierżawy i korzyści izolacji jednego klienta na bazy. Zobacz [Desenie projektu dla wielu dzierżawy władz akredytacji bezpieczeństwa aplikacji z bazy danych Azure SQL](sql-database-design-patterns-multi-tenancy-saas-applications.md).

![Tworzenie wielokrotne dzierżawy aplikacje](./media/sql-database-build-multi-tenant-apps/sql-database-build-multi-tenant-apps.png)

## <a name="auto-scaling-you-control"></a>Automatyczne skalowanie sterowania

Pule automatyczne skalowanie wydajność i pojemność elastyczne baz danych w czasie rzeczywistym. Można kontrolować działanie przypisane do puli, dodawanie lub usuwanie elastyczne baz danych na żądanie i definiowanie wydajności elastyczne baz danych bez wpływu na całkowity koszt puli. Oznacza to, że nie musisz się martwić o zarządzaniu zastosowania pojedyncze bazy danych.

[Zapoznaj się z dokumentacją](sql-database-elastic-pool.md)

## <a name="intelligent-management-of-your-environment"></a>Środowiska inteligentne zarządzanie

Zalecenia dotyczące zmiany rozmiaru wbudowanych identyfikowanie z wyprzedzeniem baz danych, które będą korzystać z pul. Tych zaleceń Zezwalaj "" analizy warunkowej szybkie zoptymalizować spotkania cele wydajności. Sformatowany monitorowania wydajności i rozwiązywanie problemów z pulpitów nawigacyjnych ułatwiają wizualizacja wykorzystania puli historycznych.

[Zapoznaj się z dokumentacją](sql-database-elastic-pool-guidance.md)

## <a name="performance-and-price-to-meet-your-needs"></a>Wydajność i cena do potrzeb użytkownika

Podstawowe, Standard i Premium pul Podaj szeroki zakres wydajności, przechowywanie i opcje ceny. Pule może zawierać do 400 elastyczne baz danych. Elastyczne baz danych można automatycznie skali do 1000 elastyczne bazy danych transakcji jednostki (eDTU).

[Zapoznaj się z dokumentacją](https://azure.microsoft.com/pricing/details/sql-database/?b=16.50)

## <a name="elastic-tools"></a>Elastyczne narzędzia

Oprócz elastyczne pul istnieją bazy danych SQL funkcje ułatwiające zarządzanie działania operacyjne przez wiele baz danych:

**Wykonywanie kwerendy między bazami danych i raportowania.**  
[Kwerenda elastyczne bazy danych](sql-database-elastic-query-overview.md) umożliwia uruchomienie zapytania lub raportów między bazami danych w puli użytkownika elastycznych i dostępu zdalnego danych przechowywanych w bazach danych dla wielu puli jednocześnie.

**Uruchom transakcje między bazami danych.**  
[Transakcje elastyczne bazy danych](sql-database-elastic-transactions-overview.md) pozwala na uruchamianie transakcji, które obejmują kilka baz danych w bazach danych programu SQL, wykonywania operacji (to znaczy, podczas przetwarzania transakcji finansowych całej bazy danych lub podczas aktualizowania zapasami w jednej bazie danych i zamówienia).

**Wykonywanie operacji na kilka baz danych.**  
[Elastyczne bazy danych zadania](sql-database-elastic-jobs-overview.md) wykonać administracyjnych operacji, takich jak odbudowywania indeksów lub zaktualizować schematy w każdej bazy danych w sieci elastyczne puli.

Przejdź do strony głównej, sprawdź, co ma do oferowania jeszcze bazy danych SQL.
[Wyewidencjonowywać](https://azure.microsoft.com/services/sql-database/) 

## <a name="next-steps"></a>Następne kroki

Uzyskaj [bezpłatna subskrypcja Azure](https://azure.microsoft.com/get-started/) i [Tworzenie pierwszej bazy danych SQL Azure](sql-database-get-started.md).

## <a name="additional-resources"></a>Dodatkowe zasoby

Zapoznaj się wszystkie [Funkcje bazy danych SQL](https://azure.microsoft.com/services/sql-database/).
 
Przejrzyj [Omówienie kwestii technicznych bazy danych SQL](sql-database-technical-overview.md).  
