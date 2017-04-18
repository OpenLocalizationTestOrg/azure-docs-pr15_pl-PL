<properties
    pageTitle="Baza danych SQL: Co to jest DTU? | Microsoft Azure"
    description="Opis jakie bazy danych SQL Azure jest jednostkę transakcji."
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
    ms.workload="NA"
    ms.date="09/06/2016"
    ms.author="carlrab"/>

# <a name="explaining-database-transaction-units-dtus-and-elastic-database-transaction-units-edtus"></a>Wyjaśnieniem jednostki transakcji bazy danych (DTUs) i elastycznym jednostki transakcji bazy danych (eDTUs)

W tym artykule wyjaśniono jednostki transakcji bazy danych (DTUs) i elastycznym jednostki transakcji bazy danych (eDTUs) oraz co się dzieje po naciśnięciu maksymalna DTUs lub eDTUs.  

## <a name="what-are-database-transaction-units-dtus"></a>Co to są jednostki transakcji bazy danych (DTUs)

DTU jest jednostką miary zasobów, które mogą być dostępne dla bazy danych Azure SQL autonomicznego na poziomie wydajności określonych w [autonomicznej warstwa usług bazy danych](sql-database-service-tiers.md#standalone-database-service-tiers-and-performance-levels). DTU jest miarą przejścia Procesora, pamięci i we/wy i transakcji dziennika we/wy w proporcji określona przez obciążenie pracą testu OLTP zaprojektowane jako typowe dla obciążenia OLTP rzeczywistych danych. Podwajanie DTUs, zwiększając poziom wydajności bazy danych jest równa Podwajanie zestaw zasobów dostępnych dla tej bazy danych. Na przykład Premium P11 bazy danych za pomocą 1750 DTUs zawiera 350 x więcej DTU obliczyć power niż podstawowe bazy danych za pomocą 5 DTUs. Aby zrozumieć metodologii za obciążenie pracą testu OLTP służące do ustalania mieszania DTU, zobacz [Omówienie testu bazy danych SQL](sql-database-benchmark-overview.md).

![Wprowadzenie do bazy danych SQL: pojedynczy DTUs bazy danych, warstwy i poziom](./media/sql-database-what-is-a-dtu/single_db_dtus.png)

Możesz [zmienić warstwy usługi](sql-database-scale-up.md) w dowolnej chwili z minimalnymi przestoje do aplikacji (zazwyczaj uśrednianie poniżej czterech sekund). Wiele firm i aplikacje można utworzyć bazy danych oraz wybieranie wydajności jednej bazie danych w górę lub w dół na żądanie wystarcza, zwłaszcza jeśli stosunkowo przewidywalne upodobania. Jednak jeśli masz nieprzewidywalny upodobania go mogą utrudnić do zarządzania kosztami i model firmy. W tym scenariuszu możesz za pomocą puli elastyczne określoną liczbę eDTUs.

## <a name="what-are-elastic-database-transaction-units-edtus"></a>Co to są elastyczne jednostki transakcji bazy danych (eDTUs)

EDTU jest jednostką miary zestawu zasobów (DTUs), które są udostępniane między zestaw baz danych na serwerze programu Azure SQL - o nazwie [elastyczne puli](sql-database-elastic-pool.png). Elastyczne pul zapewniają prosty, koszt skuteczne rozwiązanie do zarządzania cele wydajności dla wielu baz danych zawierających bardzo różne i nieprzewidywalny upodobania. Zobacz [pul elastycznych i warstwy usługi](sql-database-service-tiers.md#elastic-pool-service-tiers-and-performance-in-edtus) Aby uzyskać więcej informacji.

![Wprowadzenie do bazy danych SQL: eDTUs, warstwy i poziom](./media/sql-database-what-is-a-dtu/sqldb_elastic_pools.png)

Puli, wyznaczona ustalona liczba eDTUs ustalonej cenie. W puli pojedyncze bazy danych mają elastyczność skali automatycznie w ramach Ustawianie parametrów. Przy dużym obciążeniu bazy danych mogą używać więcej eDTUs do zapotrzebowania. Bazy danych pod obciążeniem uproszczonej zajmować mniej i baz danych nie obciążeniu używanie nie eDTUs. Inicjowania obsługi administracyjnej zasobów dla całej puli, a nie jednego bazach danych dla upraszcza zadania związane z zarządzaniem. Ponadto masz przewidywalne budżet puli.

Dodatkowe eDTUs można dodać do istniejącej puli brak przestojów bazy danych lub nie wpływu na baz danych w puli elastyczne. Analogicznie jeśli eDTUs dodatkowe nie są już potrzebne są może być usuwany ze istniejącej puli w dowolnym momencie w czasie. Możesz dodać lub odjąć baz danych do puli. Jeśli baza danych jest właściwie w obszarze — korzystanie z zasobów, przenieś ją w górę.

## <a name="how-can-i-determine-the-number-of-dtus-needed-by-my-workload"></a>Jak ustalić liczbę DTUs wymagane przez Moje obciążenie pracą?

Jeśli szukasz przeprowadzić migrację istniejącego lokalnego lub Obciążenie pracą maszyn wirtualnych programu SQL Server do bazy danych SQL Azure, można użyć [Kalkulatora DTU](http://dtucalculator.azurewebsites.net/) do przybliżenia liczbę DTUs potrzebne. Dla istniejących obciążenie pracą bazy danych SQL Azure umożliwia [Wgląd wydajności kwerendy bazy danych SQL](sql-database-query-performance.md) opis zużycie zasobów do bazy danych (DTUs), aby uzyskać bardziej zaawansowane wgląd w jak zoptymalizować z pracą. Aby uzyskać informacje o zużycie zasobów dla ostatniego godzinę umożliwia także [sys.dm_db_ resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) DMV. Możesz też widoku wykazu [sys.resource_stats](http://msdn.microsoft.com/library/dn269979.aspx) może również przeszukiwać uzyskać te same dane z ostatnich 14 dni, chociaż na dolnym wierności średnich 5 minut.

## <a name="how-do-i-know-if-i-could-benefit-from-an-elastic-pool-of-resources"></a>Jak ustalić, jeśli I mogą korzystać z elastycznych puli zasobów?

Pule są odpowiednie dla dużej liczby baz danych z określonego wykorzystania wzorców. W określonej bazie danych tego wzorca jest określony przez niskie średnie wykorzystanie ze stosunkowo rzadko wykorzystania tych najwyższych wartościach. Baza danych SQL automatycznie oblicza użycie zasobu historycznych baz danych w istniejącej bazy danych SQL server i konfiguracji odpowiednie puli w portalu Azure zaleca. Aby uzyskać więcej informacji, zobacz [po puli elastyczne bazy danych należy?](sql-database-elastic-pool-guidance.md)

## <a name="what-happens-when-i-hit-my-maximum-dtus"></a>Co się dzieje po naciśnięciu I Moje maksymalna DTUs

Poziomy wydajności są kalibrowane i podlega dostarczać zasoby potrzebne do uruchamiania usługi Obciążenie pracą bazy danych w granicach maksymalna dozwolona poziom wydajności poziomu wybranej usługi. Jeśli z pracą jest naciśnięcie limity w jednym z limity Jo ei-dziennika Procesora i danych, będzie nadal wyświetlany zasobów na poziomie dopuszczalną, ale prawdopodobnie Zobacz lepszą opóźnienia zapytań. Limity te nie powodują błędy, ale zamiast spowolnienie obciążenie pracą, chyba że spowalnianie staje się tak poważne, że kwerendy Uruchom chronometraż. Jeśli są naciśnięcie limity maksymalna dozwolona użytkownika równoczesne sesji i żądań (wątków), pojawi się jawne błędy. Zobacz [bazy danych SQL Azure zasobów ograniczenia](sql-database-resource-limits.md) dotyczące informacji na temat ograniczeń zasobów niż Procesora, pamięci, dane We/Wy i dziennik transakcji we/wy.

## <a name="next-steps"></a>Następne kroki

- Zobacz [warstwa usług](sql-database-service-tiers.md) informacji na temat DTUs i eDTUs dostępne dla autonomicznego baz danych i elastycznym puli.
- Zobacz [bazy danych SQL Azure zasobów ograniczenia](sql-database-resource-limits.md) dotyczące informacji na temat ograniczeń zasobów niż Procesora, pamięci, dane We/Wy i dziennik transakcji we/wy.
- Zobacz [Wglądu wydajności kwerendy bazy danych SQL](sql-database-query-performance.md) do zrozumienia usługi zużycie (DTUs).
- Zobacz [Omówienie testu bazy danych SQL](sql-database-benchmark-overview.md) do zrozumienia metodologii za obciążenie pracą testu OLTP służące do ustalania mieszania DTU.