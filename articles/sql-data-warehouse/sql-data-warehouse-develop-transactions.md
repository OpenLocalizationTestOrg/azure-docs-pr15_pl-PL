<properties
   pageTitle="Transakcje w magazynie danych SQL | Microsoft Azure"
   description="Porady dotyczące wykonywania transakcji w magazynie danych SQL Azure do opracowania rozwiązań."
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
   ms.date="07/31/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="transactions-in-sql-data-warehouse"></a>Transakcje w magazynie danych SQL

Jak można oczekiwać magazynu danych SQL obsługuje transakcje jako część obciążenia pracą magazynu danych. Jednak aby upewnić się, że wydajność magazynu danych SQL są obsługiwane w skali niektóre funkcje są ograniczone w porównaniu z programem SQL Server. W tym artykule są wyróżniane różnice i innych list. 

## <a name="transaction-isolation-levels"></a>Poziomów izolacji transakcji
Program SQL Data Warehouse wykonuje kwasu transakcje. Jednak izolacji obsługę transakcji jest ograniczone do `READ UNCOMMITTED` i nie będzie możliwa. Można zaimplementować liczbę kodowania metody zapobiegania dirty Odczyt danych, jeśli jest to istotny. Najczęściej używanych metod wykorzystać aby uniemożliwić użytkownikom badania danych, który jest nadal przygotowany CTAS i przełączanie partition tabeli (często nazywane przesuwania deseń okna). Widoki, które wstępnie filtrowanie danych jest również podejście popularne.  

## <a name="transaction-size"></a>Rozmiar transakcji
Rozmiar danych jednej transakcji modyfikacji jest ograniczony. Limit dzisiaj jest stosowana "na rozkład". W związku z tym łączną alokacją Oblicza iloczyn limit liczby rozkładu. Aby przybliżonego maksymalna liczba wierszy w transakcji podzielić zakończenie dystrybucji przez całkowity rozmiar każdego wiersza. Dla kolumn zmiennej długości warto rozważyć biorąc długość średnia kolumny, a nie o maksymalnym rozmiarze.

W tabeli poniżej następujące założenia wprowadzono:

* Wystąpił Rozdziel danych 
* Długość średnia wiersza wynosi 250 bajtów

| [DWU][]    | Caps na dystrybucyjnych (nosek) | Liczba dystrybucji | Maksymalny rozmiar transakcji (nosek) | # Wierszy na rozkładu | Maksymalna liczba wierszy dla każdej transakcji |
| ------ | -------------------------- | ----------------------- | -------------------------- | ----------------------- | ------------------------ |
| DW100  |  1                         | 60                      |   60                       |   4,000,000             |    240,000,000           |
| DW200  |  1,5                       | 60                      |   o 90 stopni.                       |   6,000,000             |    360,000,000           |
| DW300  |  2,25                      | 60                      |  135                       |   9,000,000             |    540,000,000           |
| DW400  |  3                         | 60                      |  180                       |  12,000,000             |    720,000,000           |
| DW500  |  3,75                      | 60                      |  225                       |  15,000,000             |    900,000,000           |
| DW600  |  4,5                       | 60                      |  270 stopni                       |  18,000,000             |  1,080,000,000           |
| DW1000 |  7.5                       | 60                      |  450                       |  30,000,000             |  1,800,000,000           |
| DW1200 |  9                         | 60                      |  540                       |  36,000,000             |  2,160,000,000           |
| DW1500 | 11,25                      | 60                      |  675                       |  45,000,000             |  2,700,000,000           |
| DW2000 | 15                         | 60                      |  900                       |  60,000,000             |  3,600,000,000           |
| DW3000 | 22.5                       | 60                      |  1,350                     |  90,000,000             |  5,400,000,000           |
| DW6000 | 45                         | 60                      |  2,700                     | 180,000,000             | 10,800,000,000           |

Limit rozmiaru transakcji jest stosowana do liczby transakcji lub operacji. Nie jest stosowana przez wszystkich transakcji jednocześnie. W związku z tym każdej transakcji może zapisywać tej ilości danych w dzienniku. 

Optymalizacja i zminimalizować ilość danych zapisane w dzienniku można znaleźć w artykule [Transakcje najważniejsze wskazówki][] .

> [AZURE.WARNING] Rozmiar maksymalny transakcji uzyskuje się tylko na mieszania lub nawet ROUND_ROBIN distributed tabel w przypadku rozkładu danych. Jeśli transakcji zapisywania danych w sposób skośny podziału, limit wynosi mogą się z Tobą przed transakcji maksymalny rozmiar.
<!--REPLICATED_TABLE-->

## <a name="transaction-state"></a>Stan transakcji
Magazyn danych SQL funkcja XACT_STATE() raport transakcji nie powiodło się za pomocą wartość -2. Oznacza to, że transakcji zostało zakończone niepowodzeniem i będzie mieć oznaczenie na potrzeby wycofywania tylko

> [AZURE.NOTE] Użyj -2 przy użyciu funkcji XACT_STATE do oznaczania niepowodzeniu transakcji przedstawia różne zachowanie programu SQL Server. Program SQL Server używa wartości -1 reprezentować nie committable transakcji. Program SQL Server przeszkadzają błędy wewnątrz transakcji bez konieczności mają być oznaczane jako nie committable. Na przykład `SELECT 1/0` czy powodują wystąpienie błędu, ale nie wymusić usunięcie committable stan. Program SQL Server pozwala Odczyt nie committable transakcji. Jednak SQL Data Warehouse nie zezwala na to zrobić. W przypadku wystąpienia błędu wewnątrz transakcji magazynu danych SQL automatycznie wprowadzany stan-2 i nie będziesz w stanie się dodatkowo zaznacz stwierdzenia, dopóki nie została wycofana instrukcji. Dlatego należy sprawdzić kodzie aplikacji, aby zobaczyć, jeśli używa XACT_STATE() podczas może być konieczne zmodyfikuj kod.

Na przykład w programie SQL Server może zostać wyświetlony transakcji, która wygląda następująco:

```sql
SET NOCOUNT ON;
DECLARE @xact_state smallint = 0;

BEGIN TRAN
    BEGIN TRY
        DECLARE @i INT;
        SET     @i = CONVERT(INT,'ABC');
    END TRY
    BEGIN CATCH
        SET @xact_state = XACT_STATE();

        SELECT  ERROR_NUMBER()    AS ErrNumber
        ,       ERROR_SEVERITY()  AS ErrSeverity
        ,       ERROR_STATE()     AS ErrState
        ,       ERROR_PROCEDURE() AS ErrProcedure
        ,       ERROR_MESSAGE()   AS ErrMessage
        ;
        
        IF @@TRANCOUNT > 0
        BEGIN
            PRINT 'ROLLBACK';
            ROLLBACK TRAN;
        END

    END CATCH;

IF @@TRANCOUNT >0
BEGIN
    PRINT 'COMMIT';
    COMMIT TRAN;
END

SELECT @xact_state AS TransactionState;
```

Jeśli pole Kod się powyżej, a następnie zostanie wyświetlony następujący komunikat o błędzie:

Msg 111233, poziom 16, stan: 1, w wierszu 1 111233; Bieżąca transakcja została przerwana i oczekujących zmian mają została wycofana. Przyczyna: Transakcji w stanie tylko do przywrócenia nie została jawnie wycofana przed DDL, DML lub instrukcji SELECT.

Zostanie również nie wyświetlony wynik funkcji ERROR_ *.

W magazynie danych SQL kod wymaga nieco zmiany:

```sql
SET NOCOUNT ON;
DECLARE @xact_state smallint = 0;

BEGIN TRAN
    BEGIN TRY
        DECLARE @i INT;
        SET     @i = CONVERT(INT,'ABC');
    END TRY
    BEGIN CATCH
        SET @xact_state = XACT_STATE();
        
        IF @@TRANCOUNT > 0
        BEGIN
            PRINT 'ROLLBACK';
            ROLLBACK TRAN;
        END

        SELECT  ERROR_NUMBER()    AS ErrNumber
        ,       ERROR_SEVERITY()  AS ErrSeverity
        ,       ERROR_STATE()     AS ErrState
        ,       ERROR_PROCEDURE() AS ErrProcedure
        ,       ERROR_MESSAGE()   AS ErrMessage
        ;
    END CATCH;

IF @@TRANCOUNT >0
BEGIN
    PRINT 'COMMIT';
    COMMIT TRAN;
END

SELECT @xact_state AS TransactionState;
```

Obserwuje się oczekiwane zachowanie. Błąd w transakcji odbywa się i funkcje ERROR_ * Podaj wartości, zgodnie z oczekiwaniami.

Wszystkie, która została zmieniona, który jest `ROLLBACK` transakcji ma nastąpić przed odczytu informacji o błędach w `CATCH` blok.

## <a name="errorline-function"></a>Funkcja ERROR_LINE() —
Warto również zauważyć, że magazynu danych SQL zaimplementowania lub nie obsługuje funkcji ERROR_LINE() —. Jeśli masz to w kodzie, należy usunąć, aby był zgodny z magazynu danych SQL. Użyj kwerendy etykiet w kodzie do wykonania podobne funkcje. Przeczytaj artykuł [etykiety][] , aby uzyskać więcej informacji na temat tej funkcji.

## <a name="using-throw-and-raiserror"></a>Za pomocą RZUTOWANIA i RAISERROR
RZUTOWANIA jest nowoczesną wykonania podnoszenie wyjątki w magazynie danych SQL, ale jest również obsługiwane RAISERROR. Istnieje kilka różnic, które warto jednak zwracając uwagę.

- Komunikaty o błędach, które liczb nie może być w zakresie 100 000 150,000 RZUTOWANIA zdefiniowane przez użytkownika
- Komunikaty o błędach RAISERROR są ustalone na poziomie 50 000
- Korzystanie z sys.messages nie jest obsługiwane.

## <a name="limitiations"></a>Limitiations
Program SQL Data Warehouse mieć kilka innych ograniczeń, które dotyczą transakcji.

Ta osoba jest w następujący sposób:

- Nie transakcje rozproszone
- Nie transakcji zagnieżdżonych dozwolone
- Brak Zapisz punkty dozwolone
- Nie transakcji
- Nie zaznaczonych transakcji
- Nie są obsługiwane DDL, takich jak `CREATE TABLE` wewnątrz użytkownika zdefiniowane transakcji

## <a name="next-steps"></a>Następne kroki
Aby dowiedzieć się więcej na temat optymalizowania transakcje, zobacz [Transakcje najważniejsze wskazówki][].  Aby uzyskać informacje o innych najważniejszych wskazówkach magazynu danych SQL, zobacz [Najważniejsze wskazówki magazynu danych SQL][].

<!--Image references-->

<!--Article references-->
[DWU]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units
[development overview]: ./sql-data-warehouse-overview-develop.md
[Transakcje najważniejsze wskazówki]: ./sql-data-warehouse-develop-best-practices-transactions.md
[Najważniejsze wskazówki dotyczące magazynu danych SQL]: ./sql-data-warehouse-best-practices.md
[ETYKIETA]: ./sql-data-warehouse-develop-label.md

<!--MSDN references-->

<!--Other Web references-->
