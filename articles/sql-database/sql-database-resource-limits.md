<properties
    pageTitle="Limity dotyczące zasobów bazy danych Azure SQL"
    description="Na tej stronie opisano niektóre typowe ograniczenia zasobów dla bazy danych SQL Azure."
    services="sql-database"
    documentationCenter="na"
    authors="CarlRabeler"
    manager="jhubbard"
    editor="monicar" />


<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-management"
    ms.date="10/13/2016"
    ms.author="carlrab" />


# <a name="azure-sql-database-resource-limits"></a>Limity zasobów bazy danych SQL Azure

## <a name="overview"></a>Omówienie

Baza danych SQL Azure zarządza zasobów dostępnych dla bazy danych przy użyciu dwa różne mechanizmy: **Zarządzania zasobami** i **Wymuszania ograniczenia**. W tym temacie opisano następujące dwa główne obszary zarządzania zasobami.

## <a name="resource-governance"></a>Zarządzanie zasobów
Jest jednym z celów projektu warstwy usługi podstawowe, Standard i Premium dla bazy danych SQL Azure będzie działać tak, jakby bazy danych jest uruchomiona na osobnym komputerze, całkowicie samodzielnie z innej bazy danych. Zarządzanie zasobów emuluje zachowanie to. Jeśli wykorzystania zasobów zagregowane osiągnie maksymalna dostępne Procesora, pamięci, dziennika we/wy i danych we/wy zasobów przydzielonych do bazy danych, zarządzania zasobów kwerend na wykonanie w kolejce i przydzielanie zasobów do kwerend kolejce zgodnie z ich zwolnienia.

Jak na komputerze dedykowane wykorzystanie wszystkich dostępnych zasobów spowoduje dłużej wykonanie obecnie wykonywania kwerend, które mogą spowodować przekroczenia limitu czasu polecenia na komputerze klienckim. Aplikacje z rygorystyczne ponów próbę logicznych i aplikacje, które wykonują kwerendy do bazy danych o wysokiej częstotliwości pojawia się wiadomości błędów podczas próby wykonania nowych kwerend, gdy osiągnięto limit żądań.

### <a name="recommendations"></a>Zalecenia:
Monitorowanie wykorzystania zasobów, a także czas odpowiedzi średnia kwerend, gdy zbliża maksymalne wykorzystanie bazy danych. Gdy wystąpią wyższą opóźnienia kwerendy składają się trzy opcje:

1.  Zmniejsz ilość przychodzących wezwań do bazy danych, aby zapobiec limitu czasu i stos w górę żądań.

2.  Przypisz na wyższy poziom wydajności do bazy danych.

3.  Optymalizacja kwerend w celu zmniejszenia wykorzystania zasobów każda kwerenda. Aby uzyskać więcej informacji zobacz sekcję kwerendy dostrajania-Hinting w artykule wskazówki wydajności bazy danych SQL Azure.

## <a name="enforcement-of-limits"></a>Stosowania ograniczenia
Zasoby inne niż Procesora, pamięci dziennika we/wy i danych We/Wy są wymuszane przez odmawianie nowych wezwań po osiągnięciu limitów. Klienci zostanie wyświetlony [komunikat o błędzie](sql-database-develop-error-messages.md) w zależności od tego, że osiągnięto limit.

Na przykład liczba połączeń z bazą danych SQL, a także liczbę żądań, które mogą być przetwarzane są ograniczone. Baza danych SQL umożliwia liczbę połączeń z bazą danych powinien być większy niż liczba żądań do obsługi buforowanie połączeń. Ilość połączeń, które są dostępne można łatwo kontrolowane przez aplikację, ilość równoległe żądania jest często razy trudniejsze, aby oszacować i do sterowania. Szczególnie podczas obciążenia Szczyt gdy aplikacja albo wysyła zbyt wiele żądań lub bazy danych osiągnie jego zasób limitów i uruchamiania, pojawia się wątków z powodu dłużej uruchomionego kwerendy, błędy mogą występować.

## <a name="service-tiers-and-performance-levels"></a>Warstwy usługi i poziomy wydajności

Istnieją warstwy usługi i poziomy wydajności dla obu pule bazy danych i elastyczne autonomicznego.

### <a name="standalone-databases"></a>Autonomiczna baz danych

W bazie danych autonomicznego limity bazy danych są definiowane przez poziom warstwa i wydajności usługi bazy danych. W poniższej tabeli opisano właściwości podstawowe, Standard i Premium baz danych na różne poziomy wydajności.

[AZURE.INCLUDE [SQL DB service tiers table](../../includes/sql-database-service-tiers-table.md)]

### <a name="elastic-pools"></a>Elastyczne pul

[Elastyczne pul](sql-database-elastic-pool.md) współużytkowanie zasobów między bazami danych w puli. W poniższej tabeli opisano właściwości Basic, Standard i Premium pul elastyczne bazy danych.

[AZURE.INCLUDE [SQL DB service tiers table for elastic databases](../../includes/sql-database-service-tiers-table-elastic-db-pools.md)]

Rozwinięty definicja każdemu zasobowi wymienionych w poprzednich tabelach zobacz opisy w [możliwości poziomu usług i ograniczenia](sql-database-performance-guidance.md#service-tier-capabilities-and-limits). Aby uzyskać omówienie warstwy usługi zobacz [warstwy usługi bazy danych SQL Azure i poziomy wydajności](sql-database-service-tiers.md).

## <a name="other-sql-database-limits"></a>Inne limity bazy danych SQL

| Obszar | Limit | Opis |
|---|---|---|
| Eksportowanie bazy danych przy użyciu automatycznego na subskrypcję | 10 | Automatyczne eksportowanie umożliwia utworzenie harmonogramu niestandardowego do tworzenia kopii zapasowej bazy danych SQL. Aby uzyskać więcej informacji, zobacz [bazy danych programu SQL: Obsługa automatycznego eksportowanie do bazy danych SQL](http://weblogs.asp.net/scottgu/windows-azure-july-updates-sql-database-traffic-manager-autoscale-virtual-machines).|
| Bazy danych na serwerze | Do 5000 | Maksymalnie 5000 baz danych są dozwolone na serwer na serwerach w wersji 12. |  
| DTUs na serwer | 45000 | 45000 DTUs są dostępne na serwer na serwerach w wersji 12 obsługi administracyjnej baz danych, pul elastycznych i magazynów danych. |



## <a name="resources"></a>Zasoby

[Azure subskrypcji i limity dotyczące usługi, przydziałów i ograniczeń](../azure-subscription-service-limits.md)

[Warstwy usługi bazy danych Azure SQL i poziomy wydajności](sql-database-service-tiers.md)

[Komunikaty o błędach dla programów klienckich bazy danych SQL](sql-database-develop-error-messages.md)
