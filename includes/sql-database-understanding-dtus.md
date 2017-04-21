Jednostka transakcji bazy danych (DTU) jest jednostki miary w bazie danych SQL, reprezentująca względne możliwości baz danych na podstawie wartości rzeczywistych miary: transakcji bazy danych. Firma Microsoft zajęła zestawu działań, które są typowe dla transakcji online przetwarzania żądania (OLTP), a następnie mierzone, ile transakcji można ukończyć na sekundę w pełni załadowany warunków (jest krótka wersja, można przeczytać szczegóły kategorii w [Omówienie testu](../articles/sql-database/sql-database-benchmark-overview.md)). 

Na przykład Premium P11 bazy danych za pomocą 1750 DTUs zawiera 350 x więcej DTU obliczyć power niż podstawowe bazy danych za pomocą 5 DTUs. 

![Wprowadzenie do bazy danych SQL: pojedynczy DTUs bazy danych, warstwy i poziom.](./media/sql-database-understanding-dtus/single_db_dtus.png)

>[AZURE.NOTE] Jeśli są migrowane z istniejącą bazą danych programu SQL Server, w narzędzia innej firmy, [Kalkulator DTU bazy danych SQL Azure](http://dtucalculator.azurewebsites.net/)umożliwia uzyskiwanie Szacowany poziom wydajności i świadczenie usług warstwy, które wymagają bazy danych w bazie danych SQL Azure.

### <a name="dtu-vs-edtu"></a>DTU a eDTU

DTU dla pojedynczego baz danych przekłada się bezpośrednio do eDTU elastyczną baz danych. Na przykład bazy danych w puli podstawowe elastyczną bazy danych oferuje eDTUs maksymalnie 5. To działanie sam jako pojedynczy podstawowe bazy danych. Różnica polega na elastyczne bazy danych nie używają dowolnego eDTUs z puli dopóki do. 

![Wprowadzenie do bazy danych SQL: elastyczne pul według poziomu.](./media/sql-database-understanding-dtus/sqldb_elastic_pools.png)

Prosty przykład pomaga. Sporządzanie puli podstawowe elastyczne bazy danych przy użyciu 1000 DTUs i upuść 800 baz danych w nim. Jak tylko 200 800 baz danych są używane w dowolnym momencie w czasie (5 DTU X 200 = 1000) trafisz nie będzie możliwości puli, przy czym nie zmniejsza wydajność bazy danych. W tym przykładzie jest uproszczony dla jasności. Obliczenia rzeczywistą jest nieco bardziej skomplikowane. Portal wykonuje obliczenia i sprawia, że zalecenie oparte na zastosowania baza danych. Zobacz [ceny i wydajności zagadnienia związane z puli elastyczne bazy danych](../articles/sql-database/sql-database-elastic-pool-guidance.md) , aby dowiedzieć się, jak działają zalecenia lub wykonywanie obliczeń samodzielnie. 
