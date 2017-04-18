<properties
    pageTitle="Dostosowywanie porady dotyczące wydajności bazy danych SQL | Microsoft Azure"
    description="Porady dotyczące Dostosowywanie w bazie danych SQL Azure za pomocą obliczeń i poprawy wydajności."
    services="sql-database"
    documentationCenter=""
    authors="v-shysun"
    manager="felixwu"
    editor=""
    keywords="Dostosowywanie, dostosowywanie, dostosowywanie porady, wydajności sql wydajności bazy danych SQL wydajności Dostosowywanie wydajności bazy danych sql"/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="v-shysun"/>

# <a name="sql-database-performance-tuning-tips"></a>Porady dotyczące dostosowywania wydajności bazy danych SQL
Możesz zmienić [warstwa usług](sql-database-service-tiers.md) jednej bazie danych lub zwiększanie eDTUs puli elastyczne bazy danych w dowolnym momencie, aby zwiększyć wydajność, ale może zajść potrzeba określenia możliwości w celu usprawnienia i najpierw zoptymalizowania wydajności kwerend. Brakujące indeksy i źle zoptymalizowane kwerendy są typowe przyczyny niskiej bazy danych wydajności. Ten artykuł zawiera wskazówki dotyczące wydajności Dostosowywanie w bazie danych SQL.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="steps-to-evaluate-and-tune-database-performance"></a>Procedura oceny i dostosować bazy danych wydajności
1.  W [Azure Portal](https://portal.azure.com)kliknij **baz danych programu SQL**, wybierz bazę danych, a następnie wyszukaj zasoby zbliża największe za pomocą wykresu monitorowania. Zużycie DTU jest domyślnie wyświetlany. Kliknij przycisk **Edytuj** , aby zmienić zakres czasu i wartości wyświetlane.
2.  Użyj [Kwerendy wydajności wglądu](sql-database-query-performance.md) do oszacowania zapytań za pomocą DTUs, a następnie użyj [Advisor bazy danych SQL](sql-database-advisor.md) do wyświetlania zalecenia dotyczące tworzenia i usuwanie indeksów, parametryzacja kwerend i rozwiązywanie problemów z schematu.
3.  Dynamiczne zarządzanie widokami (DMVs), rozszerzonego zdarzeń (Xevents) i magazynie kwerendy w SSMS umożliwia pobieranie parametrów wydajności w czasie rzeczywistym. Zobacz [temat wskazówki dotyczące wydajności](sql-database-performance-guidance.md) szczegółowe monitorowania i dostosowywanie porady.


    > [AZURE.IMPORTANT] Zalecane jest zawsze używać najnowszej wersji programu Management Studio do pozostawać aktualizacje Microsoft Azure i baza danych SQL. [Aktualizowanie programu SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).


## <a name="steps-to-improve-database-performance-with-more-resources"></a>Czynności, aby poprawić wydajność bazy danych przy użyciu więcej zasobów
1.  Pojedynczy baz danych możesz [zmienić warstwy usług](sql-database-scale-up.md) na żądanie, aby poprawić wydajność bazy danych.
2.  Wiele baz danych należy rozważyć, czy automatyczne skalowanie zasobów za pomocą [pul elastyczne bazy danych](sql-database-elastic-pool-guidance.md) .
