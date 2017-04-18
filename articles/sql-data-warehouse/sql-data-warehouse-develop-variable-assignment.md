<properties
   pageTitle="Przypisywanie zmiennych w magazynie danych SQL | Microsoft Azure"
   description="Porady dotyczące przypisywanie zmiennych w języku Transact-SQL w magazynie danych SQL Azure dla opracowania rozwiązań."
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

# <a name="assign-variables-in-sql-data-warehouse"></a>Przypisywanie zmiennych w magazynie danych SQL
Zmienne w magazynie danych SQL są ustawione przy użyciu `DECLARE` instrukcji lub `SET` instrukcji.

Wszystkie z następujących sposoby idealnie prawidłową wartość zmiennych:

## <a name="setting-variables-with-declare"></a>Ustawienie zmienne DECLARE

Inicjowanie zmiennych z DECLARE jest jednym z najbardziej elastyczna sposobów ustawiania wartości zmiennej w magazynie danych SQL.

```sql
DECLARE @v  int = 0
;
```

Aby ustawić więcej niż jednej zmiennej jednocześnie umożliwia także DECLARE. Nie można używać `SELECT` lub `UPDATE` w tym celu:

```sql
DECLARE @v  INT = (SELECT TOP 1 c_customer_sk FROM Customer where c_last_name = 'Smith')
,       @v1 INT = (SELECT TOP 1 c_customer_sk FROM Customer where c_last_name = 'Jones')
;
```

Brak możliwości zainicjowania i użyć zmiennej w tej samej instrukcji DECLARE. Aby przedstawić punktu w poniższym przykładzie **nie** może być @p1 jest zainicjowany i używane w tej samej instrukcji DECLARE. Spowoduje to wystąpienie błędu.

```sql
DECLARE @p1 int = 0
,       @p2 int = (SELECT COUNT (*) FROM sys.types where is_user_defined = @p1 )
;
```

## <a name="setting-values-with-set"></a>Ustawianie wartości przy użyciu zestawu
Zestaw jest często metodę określania zmiennej.

Wszystkie poniższe przykłady są prawidłowe metody ustawienia zmiennej z ZESTAWEM:

```sql
SET     @v = (Select max(database_id) from sys.databases);
SET     @v = 1;
SET     @v = @v+1;
SET     @v +=1;
```

Jedna zmienna w danej chwili można ustawić tylko z ZESTAWEM. Jednak widoczne powyżej operatory złożone są dopuszczalna.

## <a name="limitations"></a>Ograniczenia
Nie można użyć SELECT lub UPDATE przypisanie zmiennej.


## <a name="next-steps"></a>Następne kroki
Aby uzyskać więcej porad rozwoju zobacz [Omówienie tworzenia][].

<!--Image references-->

<!--Article references-->
[Omówienie tworzenia]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->

<!--Other Web references-->
