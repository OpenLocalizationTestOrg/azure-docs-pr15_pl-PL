<properties
    pageTitle="Opcje i wydajności bazy danych SQL: usługi warstwy | Microsoft Azure"
    description="Porównanie funkcji bazy danych SQL wydajności i małych firm ciągłości poziomów usług do Saldo kosztów i możliwości podczas skalowania."
    keywords="Opcje bazy danych, wydajności bazy danych"
    services="sql-database"
    documentationCenter=""
    authors="CarlRabeler"
    manager="jhubbard"
    editor="CarlRabeler"/>

<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="data-management"
    ms.date="08/10/2016"
    ms.author="carlrab"/>

# <a name="sql-database-options-and-performance-understand-whats-available-in-each-service-tier"></a>Opcje bazy danych SQL i wydajności: opis, jakie opcje są dostępne w każdej warstwie usługi

[Bazy danych SQL Azure](sql-database-technical-overview.md) oferuje trzy warstwy usługi z wieloma poziomami wydajności do obsługi różnych obciążenia. Każdy poziom wydajności udostępnia rosnącymi zasobów formantem rosnącej złożoności wyższej przepustowości. Możesz zarządzać każdej bazy danych w osobnym [warstwa usług](sql-database-service-tiers.md#standalone-database-service-tiers-and-performance-levels) z poziomu wydajności. Można również zarządzać wiele baz danych w [puli elastyczną](sql-database-service-tiers.md#elastic-pool-service-tiers-and-performance-in-edtus) z zestawem udostępnionych zasobów. Pod względem jednostki transakcji bazy danych (DTUs) i elastycznym puli elastyczną DTUs lub eDTUs, wyrażone są zasobów dostępnych dla autonomicznego baz danych. Aby uzyskać więcej informacji o DTUs i eDTUs zobacz [Co to jest DTU](sql-database-what-is-a-dtu.md). 

W obu przypadkach warstwy usługi zawierać **podstawowe**, **Standard**i **Premium**. Opcje bazy danych w tych poziomów są podobne autonomicznego baz danych i pule elastyczne, ale istnieją dodatkowe zagadnienia związane z pul elastyczne. Ten artykuł zawiera szczegółowe warstwy usługi autonomicznej baz danych i pule elastyczne.

## <a name="service-tiers-and-database-options"></a>Warstwy usługi i opcje bazy danych
Podstawowe, standardowy, oraz warstwy usługi Premium wszystkie przestojów SLA 99,99% i oferują przewidywalne wydajności, firm elastyczne opcje ciągłości, funkcje zabezpieczeń i rozliczeń co godzinę. Poniższa tabela zawiera przykłady poziomów najlepiej nadaje się do innej aplikacji.

| Warstwa usług | Obciążenia docelowej |
|---|---|
| **Podstawowe** | Najlepiej dopasowane do małych baz danych, pomocnicze zwykle jednej aktywnej operacji w danej chwili. Jako przykład można wymienić baz danych na potrzeby rozwój lub badania lub drobnych rzadko używana aplikacji. |
| **Standardowe** | Opcja Przejdź do większości aplikacje w chmurze, obsługi wielu zapytań jednocześnie. Jako przykład można wymienić aplikacji grupy roboczej lub sieci web. |
| **Premium** | Przeznaczony dla dużej transakcji, obsługi wielu użytkowników jednocześnie i wymagających najwyższego poziomu ciągłości biznesowej. Obsługa aplikacji krytycznych dla misji baz danych należą. |

>[AZURE.NOTE] W wersji Web i biznesowych są wycofana. Przeczytaj [Zachód słońca — często zadawane pytania](https://azure.microsoft.com/pricing/details/sql-database/web-business/) , jeśli planujesz przy użyciu wersji sieci Web i małych firm.

## <a name="standalone-database-service-tiers-and-performance-levels"></a>Warstwy usługi autonomicznej bazy danych i poziomy wydajności
Autonomiczna baz danych istnieje wiele poziomów wydajności w obrębie każdego poziomu usług. Istnieje możliwość wybierz poziom, który najlepiej spełnia wymagania z pracą. Jeśli potrzebujesz skalowanie w górę lub w dół, można łatwo zmienić poziomy bazy danych. Aby uzyskać szczegółowe informacje, zobacz [Zmienianie poziomów usługi bazy danych i poziomy wydajności](sql-database-scale-up.md) .

Stosowanie parametrów na liście baz danych utworzonych przy użyciu [Wersji 12 bazy danych SQL](sql-database-v12-whats-new.md). Niezależnie od liczby baz danych bazy danych jest gwarantowana zestaw zasobów, a nie występuje cech obniżenie wydajności bazy danych.

[AZURE.INCLUDE [SQL DB service tiers table](../../includes/sql-database-service-tiers-table.md)]

>[AZURE.NOTE] Szczegółowy opis wszystkich wierszy w tej tabeli poziomów usługi zobacz [możliwości poziomu usług i ograniczenia](sql-database-performance-guidance.md#service-tier-capabilities-and-limits).

## <a name="elastic-pool-service-tiers-and-performance-in-edtus"></a>Warstwy usługi puli elastycznych i wydajność w eDTUs
Oprócz tworzenia i skalowanie autonomicznego bazy danych, możesz mieć także opcji zarządzania wiele baz danych w [puli elastyczne](sql-database-elastic-pool.md). Wszystkie bazy danych w puli elastyczne współużytkują ten sam zestaw zasobów. Parametrów są mierzone przez *Elastyczne jednostki transakcji bazy danych* (eDTUs). Zgodnie z bazami danych autonomicznego pul okazać się trzy warstwy usługi: **podstawowe**, **Standard**i **Premium**. Puli warstwy tych trzech usługi nadal zdefiniować limity ogólną wydajność i kilka funkcji.

Pule Zezwalaj baz danych, udostępnianie i używanie zasobów DTU bez konieczności Przypisywanie poziomu wydajności specyficzna dla każdej bazy danych w puli. Na przykład autonomicznego bazy danych w standardowej puli można przejść przy użyciu 0 eDTUs do eDTU maksymalna bazy danych, które skonfigurowano podczas konfigurowania puli. Pule Zezwalaj na wiele baz danych z różnych obciążenia efektywnie używać eDTU zasobów dostępnych do całej puli. Aby uzyskać szczegółowe informacje, zobacz [Wydajność i cena zagadnienia związane z puli elastyczne](sql-database-elastic-pool-guidance.md) .

W poniższej tabeli opisano właściwości warstwy puli usługi.

[AZURE.INCLUDE [SQL DB service tiers table for elastic pools](../../includes/sql-database-service-tiers-table-elastic-db-pools.md)]

Każdej bazy danych w puli jest również zgodny autonomicznego właściwości bazy danych dla tego poziomu. Na przykład podstawowe Pula ma limit maksymalną liczbę sesji na pulę 4800-28800, ale poszczególnych bazy danych w puli podstawowe ma ograniczenie bazy danych do sesji 300.

## <a name="choosing-a-service-tier"></a>Wybieranie poziomu usług

Określanie w warstwie usługi, zacznij od Określanie, czy bazy danych należy autonomicznego bazy danych lub być częścią elastyczne puli. 

### <a name="choosing-a-service-tier-for-a-standalone-database"></a>Wybieranie warstwa usług dla autonomicznego bazy danych

Decydowanie o warstwa usług dla autonomicznego bazy danych, zacznij od określania funkcje bazy danych, które należy wybrać wydanie bazy danych SQL:

- Rozmiar bazy danych (2 GB maksymalna podstawowe, niż 250 GB standardu i 500 GB do 1 TB maksymalna Premium — w zależności od poziomu wydajności)
- Okres przechowywania kopii zapasowej bazy danych (7 dni dla podstawowe, 35 dni standardu i 35 dni Premium)

Po określeniu edition bazy danych SQL, możesz przystąpić do określenia stopnia wydajności bazy danych (liczba DTUs). Czy przypuszczenie, a następnie [Skalowanie w górę lub w dół dynamicznie](sql-database-scale-up.md) na podstawie doświadczeń rzeczywistych. Za pomocą [Kalkulatora DTU](http://dtucalculator.azurewebsites.net/) do przybliżenia liczbę DTUs potrzebne. 

### <a name="choosing-a-service-tier-for-an-elastic-database-pool"></a>Po wybraniu opcji Warstwa usług dla puli elastyczne bazy danych.

Określanie w warstwie usługi dla puli elastyczne bazy danych, uruchom określając funkcje bazy danych, które należy wybrać poziomu usług dla użytkownika.

- Rozmiar bazy danych (2 GB w przypadku Basic, 250 GB standardu i 500 GB w przypadku Premium)
- Okres przechowywania kopii zapasowej bazy danych (7 dni dla podstawowe, 35 dni standardu i 35 dni Premium)
- Liczba baz danych na puli (400 Basic, 400 standardu i 50 dla Premium)
- Maksymalna liczba miejsce w magazynie na puli (117 GB w przypadku podstawowe, 1200 standardu i 750 dla Premium)

Po określeniu poziomu usług dla użytkownika, możesz przystąpić do określenia stopnia wydajności puli (eDTUs). Można przypuszczenie i następnie [rozbudowy lub przeskalować dynamicznie](sql-database-elastic-pool-manage-portal.md#change-performance-settings-of-a-pool) oparte na rzeczywiste środowiska. Za pomocą [Kalkulatora DTU](http://dtucalculator.azurewebsites.net/) do przybliżenia liczbę DTUs wymagane dla każdej bazy danych w puli pomagają w określeniu górną granicę puli.

## <a name="next-steps"></a>Następne kroki
- Dowiedz się więcej o cenach dla tych poziomów na [Ceny bazy danych SQL](https://azure.microsoft.com/pricing/details/sql-database/).
- Poznaj szczegóły [pul elastycznych](sql-database-elastic-pool-guidance.md) i [ceny i wydajności zagadnienia związane z pul elastyczne](sql-database-elastic-pool-guidance.md).
- Dowiedz się, jak [Monitor, zarządzać i zmienianie rozmiaru pul elastycznych](sql-database-elastic-pool-manage-portal.md) i [Monitorowanie wydajności autonomicznego baz danych](sql-database-single-database-monitor.md).
- Po zapoznaniu się o poziomów bazy danych SQL, wypróbuj przy użyciu [bezpłatnego konta](https://azure.microsoft.com/pricing/free-trial/) i Dowiedz się, [jak tworzenie pierwszej bazy danych SQL](sql-database-get-started.md).

## <a name="additional-resources"></a>Dodatkowe zasoby

- [Projektowanie deseni dla wielu dzierżawy władz akredytacji bezpieczeństwa aplikacji za pomocą bazy danych SQL Azure](sql-database-design-patterns-multi-tenancy-saas-applications.md)
- [Microsoft Virtual Academy kursu wideo na możliwości elastyczne bazy danych w bazie danych SQL Azure](https://mva.microsoft.com/en-US/training-courses/elastic-database-capabilities-with-azure-sql-db-16554)
