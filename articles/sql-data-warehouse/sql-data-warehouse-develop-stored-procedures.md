<properties
   pageTitle="Procedury składowane w magazynie danych SQL | Microsoft Azure"
   description="Porady dotyczące wykonywania procedur składowanych w magazynie danych SQL Azure do opracowania rozwiązań."
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
   ms.date="06/30/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="stored-procedures-in-sql-data-warehouse"></a>Procedur składowanych w magazynie danych SQL

Program SQL Data Warehouse obsługuje wiele funkcji Transact-SQL w programie SQL Server. Co więcej są skali określone funkcje, które będzie chcemy korzystać w celu zwiększenia wydajności rozwiązania.

Aby zachować skali i wydajności, magazynu danych SQL są jednak również niektóre funkcje oraz funkcje, które mają różnice funkcjonalne i innych osób, które nie są obsługiwane.

W tym artykule wyjaśniono, jak wdrażać procedur składowanych w obrębie magazynu danych SQL.

## <a name="introducing-stored-procedures"></a>Wprowadzenie do procedur składowanych
Procedury składowane to doskonały sposób na zawierający kod SQL; przechowywanie go zbliżony do danych w magazynie danych. Zawierający kod w jednostki zarządzane procedur składowanych pomóc deweloperów modularize ich rozwiązań. ułatwienie większa ponownego wykorzystywania kodu. Każdy procedura składowana również zaakceptować parametrów w celu ułatwienia ich jeszcze bardziej elastyczne.

Magazyn danych SQL zawiera implementacji uproszczony i usprawniony procedura składowana. Największa różnica w porównaniu z programem SQL Server jest procedura składowana nie wstępnie skompilowany kod. W magazynów danych możemy dotyczy zazwyczaj mniej czasu kompilacji. Należy bardziej kod procedura składowana poprawnie są zoptymalizowane działającego przed dużych ilości danych. Celem jest zapisanie godzin, minut i sekund nie milisekund. Warto więc więcej procedur składowanych można traktować jako kontenery logiczny SQL.     

Gdy program SQL Data Warehouse wykonuje procedura składowana instrukcji SQL pobieranych, tłumaczenia i zoptymalizowane w czasie wykonywania. W trakcie tego procesu każdej instrukcji jest konwertowany na zapytania rozproszone. Kod SQL wykonanych z danymi różni się do danej kwerendy przesłane.

## <a name="nesting-stored-procedures"></a>Zagnieżdżanie przechowywane procedury
Gdy procedur składowanych połączeń innych procedur składowanych lub wykonywanie dynamicznego kodu sql, a następnie wewnętrzne przechowywaną wywołania procedury lub kod jest nazywany można zagnieżdżać.

Program SQL Data Warehouse obsługuje maksymalnie 8 poziomów zagnieżdżenia. Jest to nieco inne do programu SQL Server. Poziom zagnieżdżone w programie SQL Server jest 32.

Połączenie najwyższego poziomu procedura składowana jest równa zagnieździć poziomu 1

```sql
EXEC prc_nesting
```
Jeśli procedura składowana powoduje innego WYKONYWALNA połączeń następnie zwiększy to poziom zagnieżdżone 2
```sql
CREATE PROCEDURE prc_nesting
AS
EXEC prc_nesting_2  -- This call is nest level 2
GO
EXEC prc_nesting
```
Jeśli druga procedura następnie wykonuje niektóre dynamicznego kodu sql, a następnie zwiększy to poziom zagnieżdżone 3
```sql
CREATE PROCEDURE prc_nesting_2
AS
EXEC sp_executesql 'SELECT 'another nest level'  -- This call is nest level 2
GO
EXEC prc_nesting
```

Uwaga SQL Data Warehouse nie obsługuje obecnie @@NESTLEVEL. Należy śledzić poziom zagnieżdżone samodzielnie. Prawdopodobnie będzie trafisz limit poziomu 8 zagnieżdżone, ale jeśli będzie konieczne ponowne działają kodu i "" jej spłaszczenia tak, aby zmieścił się w ten limit.

## <a name="insertexecute"></a>WSTAWIANIE. WYKONYWANIE
SQL Data Warehouse nie pozwala na używanie zestawu wyników procedura składowana w instrukcji INSERT. Istnieje jednak alternatywne metody, których można używać.

Zapoznaj się z następującym artykułem [tabele tymczasowe] , na przykład o tym, jak to zrobić.

## <a name="limitations"></a>Ograniczenia

Istnieje kilka aspektów procedury przechowywanej w języku Transact-SQL, które są niedostępne w magazynie danych SQL.

Są to:

- tymczasowe procedur składowanych
- numerowane procedur składowanych
- rozszerzone procedury składowane
- Procedur składowanych CLR
- Opcja szyfrowania
- Opcja replikacji
- Parametry zwracających tabelę
- Parametry tylko do odczytu
- domyślne parametry
- wykonanie konteksty
- Return, instrukcja

## <a name="next-steps"></a>Następne kroki
Aby uzyskać więcej porad rozwoju zobacz [Omówienie tworzenia][].

<!--Image references-->

<!--Article references-->
[tabele tymczasowe]: ./sql-data-warehouse-tables-temporary.md#modularizing-code
[Omówienie tworzenia]: ./sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[nest level]: https://msdn.microsoft.com/library/ms187371.aspx

<!--Other Web references-->
