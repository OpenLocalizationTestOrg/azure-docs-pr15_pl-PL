<properties
   pageTitle="Widoki w magazynie danych SQL | Microsoft Azure"
   description="Porady dotyczące korzystania z widoków w języku Transact-SQL w magazynie danych SQL Azure dla opracowania rozwiązań."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="jrowlandjones"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="07/01/2016"
   ms.author="jrj;barbkess;sonyama"/>


# <a name="views-in-sql-data-warehouse"></a>Widoki w magazynie danych SQL

Widoki są szczególnie przydatne w magazynie danych SQL. Można ich używać na wiele różnych sposobów, aby poprawić jakość rozwiązania.  Ten artykuł jest wyróżniana kilka przykładów sposobu wzbogacanie rozwiązania z widoków, a także ograniczenia, które należy rozważyć.

> [AZURE.NOTE] Składnia `CREATE VIEW` nie są omawiane w tym artykule. Przeczytaj artykuł [Tworzenie WIDOKU][] w witrynie MSDN, aby uzyskać informacje na ten.

## <a name="architectural-abstraction"></a>Architektura abstrakcji
Często deseń aplikacji jest ponowne tworzenie tabel za pomocą utworzenia tabeli jako wybierz (CTAS) następuje obiektu, zmienianie nazwy wzorca podczas ładowania danych.

W poniższym przykładzie dodaje nową datę rekordów do wymiar Data. Zwróć uwagę, jak nowe tabela, DimDate_New, utworzenia, a następnie zmieniona na zastępowanie wersji oryginalnej tabeli.

```sql
CREATE TABLE dbo.DimDate_New
WITH (DISTRIBUTION = ROUND_ROBIN
, CLUSTERED INDEX (DateKey ASC)
)
AS
SELECT *
FROM   dbo.DimDate  AS prod
UNION ALL
SELECT *
FROM   dbo.DimDate_stg AS stg
;

RENAME OBJECT DimDate TO DimDate_Old;
RENAME OBJECT DimDate_New TO DimDate;

```

Jednak to podejście może spowodować w tabelach wyświetlane i znika z widoku użytkownika, a także "tabeli nie istnieje" komunikaty o błędach. Widoki można udostępnić użytkownikom warstwy spójne prezentacji jednocześnie obiektów są zmieniane. Udostępniając użytkownikom dostęp do danych za pomocą widoków, oznacza to, że użytkownicy nie muszą być widoczność tabel. Zapewnia spójny interfejs użytkownika, gdy zapewnianie, że dane w magazynie projektanci można są obsługiwane w modelu danych i zwiększyć wydajność przy użyciu CTAS podczas ładowania proces danych.    

## <a name="performance-optimization"></a>Optymalizacja wydajności
Widoki można również używać do wymuszenia wydajności zoptymalizowane sprzężenia między tabelami. Na przykład widok można dołączać klucza zbędne podziału w ramach łączenia kryteriów w celu zminimalizowania przenoszenia danych.  Kolejną zaletą widoku można wymusić określonej kwerendy lub łączących wskazówki. Korzystanie z widoków w ten sposób gwarantuje, że sprzężenia są zawsze wykonywane w sposób optymalnego uniknięcie dla użytkowników do zapamiętania poprawne konstrukcji dla ich sprzężenia.

## <a name="limitations"></a>Ograniczenia
Widoki w magazynie danych SQL są tylko metadanych.  W związku z tym nie dostępne są następujące opcje:

-   Istnieje opcja powiązanie schematu
-   Nie można zaktualizować tabel za pośrednictwem widoku
-   Nie można tworzyć widoki nad tabele tymczasowe
-   Nie obsługuje rozwiń / wskazówki NOEXPAND
-   Istnieją nie widoki indeksowane w magazynie danych SQL


## <a name="next-steps"></a>Następne kroki
Aby uzyskać więcej porad rozwoju zobacz [Omówienie rozwoju magazynu danych SQL][].
Aby uzyskać `CREATE VIEW` składni można znaleźć [Utwórz widok][].

<!--Image references-->

<!--Article references-->
[Omówienie tworzenia magazynu danych SQL]: ./sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[TWORZENIE WIDOKU]: https://msdn.microsoft.com/en-us/library/ms187956.aspx

<!--Other Web references-->
