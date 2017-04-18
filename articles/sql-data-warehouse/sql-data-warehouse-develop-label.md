<properties
   pageTitle="Użyj etykiet do dokumentu kwerend w magazynie danych SQL | Microsoft Azure"
   description="Porady dotyczące korzystania z etykiety do dokumentu kwerend w magazynie danych SQL Azure dla opracowania rozwiązań."
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
   ms.date="06/14/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="use-labels-to-instrument-queries-in-sql-data-warehouse"></a>Użyj etykiet do dokumentu kwerend w magazynie danych SQL
Program SQL Data Warehouse obsługuje koncepcji o nazwie etykiety kwerendy. Przed przejściem do dowolnej głębokości Przejdźmy zapoznanie się z przykładem jedną:

```sql
SELECT *
FROM sys.tables
OPTION (LABEL = 'My Query Label')
;
```

Ten ostatni wiersz tagi ciąg "Moje zapytania etykieta" do danej kwerendy. To jest szczególnie pomocne etykieta jest kwerendy mogą za pośrednictwem DMVs. To zapewnia mechanizm, aby zidentyfikować problem kwerendy, a także do identyfikowania informacji o postępie za pośrednictwem Uruchom ETL.

Warto konwencji nazewnictwa naprawdę pomaga w tym miejscu. Na przykład na przykład "projektu: procedura: instrukcja: komentarz" może pomóc w jednoznacznie identyfikować kwerendę w między cały kod w formancie źródła.

Wyszukiwanie etykiet można użyć następującej kwerendy korzystającej z widoków dynamicznego zarządzania:

```sql
SELECT  *
FROM    sys.dm_pdw_exec_requests r
WHERE   r.[label] = 'My Query Label'
;
```

> [AZURE.NOTE] Konieczne jest zawijanie nawiasów kwadratowych lub podwójnych cudzysłowów wokół etykiet programu word podczas wykonywania kwerend. Etykieta jest słowo zastrzeżone i będzie powodowanych komunikat o błędzie, jeśli nie został ograniczony.


## <a name="next-steps"></a>Następne kroki
Aby uzyskać więcej porad rozwoju zobacz [Omówienie tworzenia][].

<!--Image references-->

<!--Article references-->
[Omówienie tworzenia]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->

<!--Other Web references-->
