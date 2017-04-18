<properties
   pageTitle="Pętle w magazynie danych SQL | Microsoft Azure"
   description="Porady dotyczące pętli języku Transact-SQL i zastąpienie kursory w magazynie danych SQL Azure dla opracowania rozwiązań."
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

# <a name="loops-in-sql-data-warehouse"></a>Pętle w magazynie danych SQL
Program SQL Data Warehouse obsługuje pętli [podczas][] wielokrotnie wykonywania instrukcji bloków. Będzie dla jak określone warunki są prawdziwe, lub gdy kod kończy specjalnie przy użyciu pętli `BREAK` słów kluczowych. Pętle są szczególnie przydatne w przypadku zastępowania kursory zdefiniowane w języku SQL. Na szczęście niemal wszystkich kursory, które są zapisywane w języku SQL są z przodu, przeczytaj tylko odmiany. Dlatego pętle [podczas] są doskonałe alternatywny, jeśli okaże się, że konieczności zastąpić jeden.

## <a name="leveraging-loops-and-replacing-cursors-in-sql-data-warehouse"></a>Używanie pętle i zamieniania kursory w magazynie danych SQL
Jednak przed do nurkowania w głowy należy poprosić sobie następujące pytania: "Może tego kursora ponownie w celu zapisania używać operacji zestawu podstawie?". W większości przypadków odpowiedzi będzie tak oraz jest zazwyczaj najlepszym rozwiązaniem. Zależności operację często wykonuje się znacznie szybciej niż metody iteracyjne, wiersz po wierszu.

Do przodu kursory tylko do odczytu, można łatwo zastąpić konstrukcji pętli. Poniżej przedstawiono prosty przykład. W tym przykładzie kodu aktualizacji statystyki dla każdej tabeli w bazie danych. Przez Iterowanie nad tabele w pętli możemy wykonywanie każdego polecenia w kolejności.

Najpierw utwórz tabelę tymczasową zawierającą liczbę wierszy unikatowe używane do identyfikowania pojedyncze instrukcje:

```
CREATE TABLE #tbl
WITH
( DISTRIBUTION = ROUND_ROBIN
)
AS
SELECT  ROW_NUMBER() OVER(ORDER BY (SELECT NULL)) AS Sequence
,       [name]
,       'UPDATE STATISTICS '+QUOTENAME([name]) AS sql_code
FROM    sys.tables
;
```

Drugi zainicjować zmiennych wymagany do wykonania pętli:

```
DECLARE @nbr_statements INT = (SELECT COUNT(*) FROM #tbl)
,       @i INT = 1
;
```

Teraz pętli instrukcji wykonywania jedną naraz:

```
WHILE   @i <= @nbr_statements
BEGIN
    DECLARE @sql_code NVARCHAR(4000) = (SELECT sql_code FROM #tbl WHERE Sequence = @i);
    EXEC    sp_executesql @sql_code;
    SET     @i +=1;
END
```

Na koniec usunąć tabelę tymczasową utworzony w pierwszym kroku

```
DROP TABLE #tbl;
```


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->

## <a name="next-steps"></a>Następne kroki
Aby uzyskać więcej porad rozwoju zobacz [Omówienie tworzenia][].

<!--Image references-->

<!--Article references-->
[Omówienie tworzenia]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[PODCZAS]: https://msdn.microsoft.com/library/ms178642.aspx


<!--Other Web references-->
