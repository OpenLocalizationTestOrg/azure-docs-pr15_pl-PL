<properties
   pageTitle="Migrowanie istniejący magazyn danych SQL Azure do magazynowania premium | Microsoft Azure"
   description="Instrukcje dotyczące migrowania istniejący magazyn danych SQL do magazynowania premium"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="happynicolle"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/24/2016"
   ms.author="nicw;barbkess;sonyama"/>

# <a name="migration-to-premium-storage-details"></a>Migracja do szczegółów miejsca do magazynowania Premium
Program SQL Data Warehouse ostatnio wprowadzonych [Premium miejsca do magazynowania dla większej przewidywalności wydajności][].  Teraz możemy przystąpić do migrowania istniejących danych magazynów obecnie standardowego magazynu do przechowywania Premium.  Aby uzyskać więcej informacji na temat sposobu i czasu automatycznego migracji występują i jak przeprowadzić migrację samodzielnie, aby kontrolować, gdy wystąpi przestoje, Czytaj dalej.

Jeśli masz więcej niż jednego magazynu danych, umożliwia [Planowanie migracji automatyczne][] poniżej określić, kiedy także zostaną migrowane.

## <a name="determine-storage-type"></a>Określanie typu magazynu
Jeśli utworzono DW przed dat poniżej aktualnie używasz standardowego magazynu.  Każdego magazynu danych na standardowy magazynowania, który podlega automatyczna migracja ma powiadomienia u góry karta Data Warehouse w [Azure Portal][] informacją, że "*nadchodzące uaktualnienie do magazynu premium wymaga awarii.  Dowiedz się więcej ->*. "

| **Region**          | **DW utworzone przed tą datą**   |
| :------------------ | :-------------------------------- |
| Wschód Australia      | Nie są jeszcze dostępne magazynowania Premium |
| Australia kopiec | 5 sierpnia 2016                    |
| Brazylia Południowej        | 5 sierpnia 2016                    |
| Kanada Środkowa      | 25 maja 2016                      |
| Wschód Kanada         | 26 maja 2016                      |
| Centralny Stany Zjednoczone          | 26 maja 2016                      |
| Chiny wschód          | Nie są jeszcze dostępne magazynowania Premium |
| Ameryka Północna w Chinach         | Nie są jeszcze dostępne magazynowania Premium |
| Wschodnioazjatyckie           | 25 maja 2016                      |
| Wschodniej Stany Zjednoczone             | 26 maja 2016                      |
| Wschodniej US2            | 27 maja 2016                      |
| Centralny Indie       | 27 maja 2016                      |
| Południe Indie         | 26 maja 2016                      |
| Indie zachód          | Nie są jeszcze dostępne magazynowania Premium |
| Japonia wschód          | 5 sierpnia 2016                    |
| Japonia zachód          | Nie są jeszcze dostępne magazynowania Premium |
| Ameryka Północna centralnej w Stanach Zjednoczonych    | Nie są jeszcze dostępne magazynowania Premium |
| Europa Północna        | 5 sierpnia 2016                    |
| Południowej centralnej Stany Zjednoczone    | 27 maja 2016                      |
| Kraje Azji Południowo      | 24 maja 2016                      |
| Europa Zachodnia         | 25 maja 2016                      |
| Stany Zjednoczone centralnej zachód     | 26 sierpnia 2016                   |
| Zachód Stany Zjednoczone             | 26 maja 2016                      |
| Zachód US2            | 26 sierpnia 2016                   |

## <a name="automatic-migration-details"></a>Automatyczna migracja szczegóły
Domyślnie firma Microsoft będzie migrację bazy danych dla Ciebie podczas 6 pm i 6: 00 w danym regionie czas lokalny podczas [migracji automatyczne planowanie][] poniżej.  Istniejące magazynu danych będzie nie będzie można używać podczas migracji.  Firma Microsoft oszacować, że migracji potrwa około godzinę na TB miejsca na magazyn danych.  Firma Microsoft zapewni również możesz nie jest naliczany podczas migracji automatyczne jego części.

> [AZURE.NOTE] Nie można używać istniejących magazynu danych podczas migracji.  Po zakończeniu migracji magazynu danych będzie powrocie do trybu online.

Szczegóły poniżej przedstawiono instrukcje, że Microsoft trwa w Twoim imieniu, aby przeprowadzić migrację i nie wymaga dowolnego zaangażowania ze strony użytkownika.  W tym przykładzie załóżmy, że do istniejącego DW ilość miejsca do magazynowania standardowy obecnie nosi nazwę "MyDW."

1.  Microsoft zmienia nazwę "MyDW" do "MyDW_DO_NOT_USE_ [sygnatura czasowa]"
2.  Microsoft można wstrzymać "MyDW_DO_NOT_USE_ [sygnatura czasowa]."  W tym czasie, jest przyjmowana kopii zapasowej.  Wiele Wstrzymaj/życiorysy mogą być wyświetlane, możemy wystąpienia problemów podczas tego procesu.
3.  Microsoft tworzy nowe DW, o nazwie "MyDW" ilość miejsca do magazynowania Premium z kopii zapasowej wykonanej w kroku 2.  "MyDW" nie będą wyświetlane do czasu, po zakończeniu przywracania.
4.  Po zakończeniu przywracania "MyDW" zwraca do samego DWUs i stanu wstrzymania lub aktywny, sprzed przed migracją.
5.  Po zakończeniu migracji Microsoft usuwa "MyDW_DO_NOT_USE_ [sygnatura czasowa]"
    
> [AZURE.NOTE] Te ustawienia nie mają ramach migracji:
> 
>   -  Inspekcji na poziomie bazy danych powinien być ponownie włączony
>   -  Reguły zapory na poziomie **bazy danych** muszą być ponownie dodano.  Reguły zapory na poziomie **serwera** , nie są objęte wpływem.

### <a name="automatic-migration-schedule"></a>Planowanie automatycznego migracji
Automatyczne migracji wystąpić z 6 pm — 6: 00 (czasu lokalnego w rozbiciu na regiony) w następującym harmonogramem awarii.

| **Region**          | **Szacowana data rozpoczęcia**     | **Szacowana data zakończenia**       |
| :------------------ | :--------------------------- | :--------------------------- |
| Wschód Australia      | Nie można określić jeszcze           | Nie można określić jeszcze           |
| Australia kopiec | 10 sierpnia 2016              | 24 sierpnia 2016              |
| Brazylia Południowej        | 10 sierpnia 2016              | 24 sierpnia 2016              |
| Kanada Środkowa      | 23 czerwca 2016                | 1 lipca 2016                 |
| Wschód Kanada         | 23 czerwca 2016                | 1 lipca 2016                 |
| Centralny Stany Zjednoczone          | 23 czerwca 2016                | 4 lipca 2016                 |
| Chiny wschód          | Nie można określić jeszcze           | Nie można określić jeszcze           |
| Ameryka Północna w Chinach         | Nie można określić jeszcze           | Nie można określić jeszcze           |
| Wschodnioazjatyckie           | 23 czerwca 2016                | 1 lipca 2016                 |
| Wschodniej Stany Zjednoczone             | 23 czerwca 2016                | 11 lipca 2016                |
| Wschodniej US2            | 23 czerwca 2016                | 8 lipca 2016                 |
| Centralny Indie       | 23 czerwca 2016                | 1 lipca 2016                 |
| Południe Indie         | 23 czerwca 2016                | 1 lipca 2016                 |
| Indie zachód          | Nie można określić jeszcze           | Nie można określić jeszcze           |
| Japonia wschód          | 10 sierpnia 2016              | 24 sierpnia 2016              |
| Japonia zachód          | Nie można określić jeszcze           | Nie można określić jeszcze           |
| Ameryka Północna centralnej w Stanach Zjednoczonych    | Nie można określić jeszcze           | Nie można określić jeszcze           |
| Europa Północna        | 10 sierpnia 2016              | 31 sierpnia 2016              |
| Południowej centralnej Stany Zjednoczone    | 23 czerwca 2016                | 2 lipca 2016                 |
| Kraje Azji Południowo      | 23 czerwca 2016                | 1 lipca 2016                 |
| Europa Zachodnia         | 23 czerwca 2016                | 8 lipca 2016                 |
| Stany Zjednoczone centralnej zachód     | 14 sierpnia 2016              | 31 sierpnia 2016              |
| Zachód Stany Zjednoczone             | 23 czerwca 2016                | 7 lipca 2016                 |
| Zachód US2            | 14 sierpnia 2016              | 31 sierpnia 2016              |

## <a name="self-migration-to-premium-storage"></a>Własny migracji do magazynu Premium
Jeśli chcesz kontrolować, gdy wystąpi usługi przestoje, umożliwia następujące czynności Migrowanie istniejący magazyn danych standardowy ilość miejsca do magazynowania do przechowywania Premium.  Jeśli chcesz samodzielnie przeprowadzić migrację, należy wykonać własny migracji przed rozpoczęciem migracji automatycznych w danym regionie, aby uniknąć ryzyka automatyczna migracja powoduje konflikt (zobacz [Planowanie migracji automatyczne][]).

### <a name="self-migration-instructions"></a>Instrukcje własny migracji
Chcesz kontrolować usługi przestoje, możesz samodzielnie przeprowadzić migrację magazynu danych za pomocą tworzenia kopii zapasowych i przywracania.  Przywracanie części migracji ma trwać około godziny na TB miejsca na DW.  Jeśli chcesz zachować tę samą nazwę, po zakończeniu migracji, wykonaj czynności dotyczące [czynności, aby zmienić podczas migracji][]. 

1.  [Wstrzymaj][] do DW prowadzącej automatyczne wykonywanie kopii zapasowych
2.  [Przywracanie][] z ostatnich migawkę
3.  Usuwanie z istniejących DW standardowy ilość miejsca do magazynowania. **Jeśli nie możesz wykonać ten krok, zostanie obciążona opłatą za obu DWs.**

> [AZURE.NOTE] Te ustawienia nie mają ramach migracji:
> 
>   -  Inspekcji na poziomie bazy danych powinien być ponownie włączony
>   -  Reguły zapory na poziomie **bazy danych** muszą być ponownie dodano.  Reguły zapory na poziomie **serwera** , nie są objęte wpływem.

#### <a name="optional-steps-to-rename-during-migration"></a>Opcjonalnie: czynności, aby zmienić podczas migracji 
Dwie bazy danych na tym samym serwerze logiczne nie mogą mieć tej samej nazwy. Program SQL Data Warehouse obsługuje teraz możliwość zmiany nazwy DW.

W tym przykładzie załóżmy, że do istniejącego DW ilość miejsca do magazynowania standardowy obecnie nosi nazwę "MyDW."

1.  Zmienianie nazwy "MyDW" za pomocą polecenia ZMIEŃ bazę danych, że następujące elementy, takie jak "MyDW_BeforeMigration."  To polecenie kasuje wszystkich istniejących transakcji i musi odbywać się w bazie danych wzorca powiodła się.
```
ALTER DATABASE CurrentDatabasename MODIFY NAME = NewDatabaseName;
```
2.  [Wstrzymaj][] "MyDW_BeforeMigration" prowadzącej automatyczne wykonywanie kopii zapasowych
3.  [Przywracanie][] z najbardziej aktualne migawki nowej bazy danych o nazwie używane mają (ex: "MyDW")
4.  Usuwanie "MyDW_BeforeMigration".  **Jeśli nie możesz wykonać ten krok, zostanie obciążona opłatą za obu DWs.**

> [AZURE.NOTE] Te ustawienia nie mają ramach migracji:
> 
>   -  Inspekcji na poziomie bazy danych powinien być ponownie włączony
>   -  Reguły zapory na poziomie **bazy danych** muszą być ponownie dodano.  Reguły zapory na poziomie **serwera** , nie są objęte wpływem.

## <a name="next-steps"></a>Następne kroki
W przypadku zmiany do magazynu Premium udało nam się również zwiększyć liczbę plików obiektów blob bazy danych źródłowych architektury magazynu danych.  Aby zmaksymalizować wydajność zalety tej zmiany, zaleca się ponownie utworzyć usługi grupowany Columnstore indeksów przy użyciu tego skryptu.  Skrypt poniżej polega na wymuszanie niektóre z istniejących danych do dodatkowych obiektów blob.  Podejmować żadnych dodatkowych działań, dane będą sposób naturalny rozpowszechniać czasie jak ładowanie dodatkowych danych w tabelach magazynu danych.

**Wymagania wstępne:**

1.  Data Warehouse powinna działać z 1000 DWUs lub nowszym (zobacz [power obliczeń skali][])
2.  Wykonywanie skryptu użytkownika należy w [roli mediumrc][] lub wyższej
    1.  Aby dodać użytkownika do tej roli, wykonaj następujące czynności: 
        1.  ````EXEC sp_addrolemember 'xlargerc', 'MyUser'````

````sql
-------------------------------------------------------------------------------
-- Step 1: Create Table to control Index Rebuild
-- Run as user in mediumrc or higher
--------------------------------------------------------------------------------
create table sql_statements
WITH (distribution = round_robin)
as select 
    'alter index all on ' + s.name + '.' + t.NAME + ' rebuild;' as statement,
    row_number() over (order by s.name, t.name) as sequence
from 
    sys.schemas s
    inner join sys.tables t
        on s.schema_id = t.schema_id
where
    is_external = 0
;
go
 
--------------------------------------------------------------------------------
-- Step 2: Execute Index Rebuilds.  If script fails, the below can be rerun to restart where last left off
-- Run as user in mediumrc or higher
--------------------------------------------------------------------------------

declare @nbr_statements int = (select count(*) from sql_statements)
declare @i int = 1
while(@i <= @nbr_statements)
begin
      declare @statement nvarchar(1000)= (select statement from sql_statements where sequence = @i)
      print cast(getdate() as nvarchar(1000)) + ' Executing... ' + @statement
      exec (@statement)
      delete from sql_statements where sequence = @i
      set @i += 1
end;
go
-------------------------------------------------------------------------------
-- Step 3: Cleanup Table Created in Step 1
--------------------------------------------------------------------------------
drop table sql_statements;
go
````

Jeśli występują problemy z magazynu danych, [Tworzenie bilet pomocy technicznej][] i odwołanie "Migracji do magazynowania Premium" jako możliwa przyczyna.

<!--Image references-->

<!--Article references-->
[Planowanie automatycznego migracji]: #automatic-migration-schedule
[self-migration to Premium Storage]: #self-migration-to-premium-storage
[Tworzenie bilet pomocy technicznej]: sql-data-warehouse-get-started-create-support-ticket.md
[Azure paired region]: best-practices-availability-paired-regions.md
[main documentation site]: services/sql-data-warehouse.md
[Wstrzymaj]: sql-data-warehouse-manage-compute-portal.md/#pause-compute
[Przywracanie]: sql-data-warehouse-restore-database-portal.md
[czynności, aby zmienić podczas migracji]: #optional-steps-to-rename-during-migration
[Skala power obliczeń]: sql-data-warehouse-manage-compute-portal/#scale-compute-power
[Rola mediumrc]: sql-data-warehouse-develop-concurrency/#workload-management

<!--MSDN references-->


<!--Other Web references-->
[Premium miejsca do magazynowania dla większej przewidywalności wydajności]: https://azure.microsoft.com/en-us/blog/azure-sql-data-warehouse-introduces-premium-storage-for-greater-performance/
[Azure Portal]: https://portal.azure.com
