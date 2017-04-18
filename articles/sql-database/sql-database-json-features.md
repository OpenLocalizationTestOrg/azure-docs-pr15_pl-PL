<properties
    pageTitle="Funkcje JSON bazy danych SQL Azure | Microsoft Azure"
    description="Baza danych SQL Azure umożliwia analizy, kwerendy i formatowanie danych w notacji notacji obiektu języka JavaScript (JSON)."
    services="sql-database"
    documentationCenter=""
    authors="jovanpop-msft"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="08/17/2016"
    ms.author="jovanpop"
   ms.workload="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>



# <a name="getting-started-with-json-features-in-azure-sql-database"></a>Wprowadzenie do funkcji JSON w bazie danych SQL Azure

Baza danych SQL Azure umożliwia analizowanie i kwerendy danych reprezentowane w formacie JavaScript Object Notation [(JSON)](http://www.json.org/) i eksportowanie danych relacyjnych jako tekst JSON.

JSON to format popularne dane używane do wymiany danych w nowoczesnym sieci web i aplikacji dla urządzeń przenośnych. JSON służy także do przechowywania półstrukturalnych danych w plikach dziennika lub NoSQL baz danych, takich jak [Azure DocumentDB](https://azure.microsoft.com/services/documentdb/). Wiele usług sieci web REST zwróconych wyników sformatowane jako tekst JSON lub akceptowanie danych w formacie JSON. Najbardziej Azure usług, takich jak [Wyszukiwania Azure](https://azure.microsoft.com/services/search/), [Magazyn Azure](https://azure.microsoft.com/services/storage/)i [Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) mają punkty końcowe pozostałych, które zwracają lub używanie JSON.

Baza danych SQL Azure umożliwia pracę z danymi JSON łatwo i integracja z usługami nowoczesne bazy danych.

## <a name="overview"></a>Omówienie

Baza danych SQL Azure udostępnia następujące funkcje do pracy z danymi JSON:

![Funkcje JSON](./media/sql-database-json-features/image_1.png)

Jeśli masz JSON tekstu, możesz wyodrębnianie danych z JSON lub sprawdź, czy JSON został prawidłowo sformatowany przy użyciu wbudowanych funkcji [JSON_VALUE](https://msdn.microsoft.com/library/dn921898.aspx), [JSON_QUERY](https://msdn.microsoft.com/library/dn921884.aspx)i [ISJSON](https://msdn.microsoft.com/library/dn921896.aspx). Funkcja [JSON_MODIFY](https://msdn.microsoft.com/library/dn921892.aspx) umożliwia aktualizowanie wartości w tekście JSON. Bardziej zaawansowanych kwerend i analizy, funkcja [OPENJSON](https://msdn.microsoft.com/library/dn921885.aspx) można przekształcić tablicę obiektów JSON do zestaw wierszy. Jakiekolwiek zapytania SQL mogą być wykonywane na stronie Ustaw zwracany wynik. Na koniec jest klauzuli [FOR JSON](https://msdn.microsoft.com/library/dn921882.aspx) , która pozwala na formatowanie danych przechowywanych w tabelach relacyjnych jako tekst JSON.

## <a name="formatting-relational-data-in-json-format"></a>Formatowanie danych relacyjnych w formacie JSON
Jeśli masz usługi sieci web, że ma warstwy danych z bazy danych, a także odpowiedzi w formacie JSON formatowanie lub kodu JavaScript RAM lub bibliotek, które akceptowanie danych po stronie klienta w formacie JSON, bezpośrednio na zapytania SQL można formatować zawartość bazy danych jako JSON. Nie jest już masz to pisanie kodu aplikacji formaty wyniki z bazy danych SQL Azure jako JSON lub zawiera niektóre biblioteki szeregowania JSON, aby przekonwertować wyników kwerendy tabelarycznych, a następnie szeregować obiektów do formatu JSON. Zamiast tego można klauzuli JSON do wyników zapytania SQL w formacie JSON w bazie danych SQL Azure i używać go bezpośrednio w aplikacji.

W poniższym przykładzie wiersze z tabeli Sales.Customer zostały sformatowane jako JSON za pomocą klauzuli JSON dla:

```
select CustomerName, PhoneNumber, FaxNumber
from Sales.Customers
FOR JSON PATH
```

Klauzula dla ścieżki JSON formatuje wyników kwerendy jako tekst JSON. Nazwy kolumn są używane jako klawiszy, a wartości komórek są generowane jako JSON wartości:

```
[
{"CustomerName":"Eric Torres","PhoneNumber":"(307) 555-0100","FaxNumber":"(307) 555-0101"},
{"CustomerName":"Cosmina Vlad","PhoneNumber":"(505) 555-0100","FaxNumber":"(505) 555-0101"},
{"CustomerName":"Bala Dixit","PhoneNumber":"(209) 555-0100","FaxNumber":"(209) 555-0101"}
]
```

Zestaw wyników jest sformatowany jako tablicę JSON, gdzie każdy wiersz jest sformatowana jako oddzielny obiekt JSON.

ŚCIEŻKA wskazuje, że przy użyciu notacji kropki w aliasów kolumn można dostosować format wyjściowy wynik JSON. Poniższe zapytanie zmienia nazwę klucza "Pola" w formacie JSON dane wyjściowe i umieszcza numer telefonu i faksu w obiekcie podrzędnego "Kontakt":

```
select CustomerName as Name, PhoneNumber as [Contact.Phone], FaxNumber as [Contact.Fax]
from Sales.Customers
where CustomerID = 931
FOR JSON PATH, WITHOUT_ARRAY_WRAPPER
```

Wynik kwerendy wygląda następująco:

```
{
    "Name":"Nada Jovanovic",
    "Contact":{
           "Phone":"(215) 555-0100",
           "Fax":"(215) 555-0101"
    }
}
```

W tym przykładzie możemy zwracane przez określenie opcji [WITHOUT_ARRAY_WRAPPER](https://msdn.microsoft.com/library/mt631354.aspx) jeden obiekt JSON zamiast tablicy. Można użyć tej opcji, jeśli wiesz, że zwracasz jeden obiekt w wyniku kwerendy.

Wartość głównym klauzuli JSON dla to, że umożliwia zwrócenie złożonych danych hierarchicznych z bazy danych sformatowane jako obiekty zagnieżdżone JSON lub tablicami. W poniższym przykładzie pokazano dołączaniu zamówienia, które należą do klienta jako tablicę zagnieżdżonych zamówienia:

```
select CustomerName as Name, PhoneNumber as Phone, FaxNumber as Fax,
        Orders.OrderID, Orders.OrderDate, Orders.ExpectedDeliveryDate
from Sales.Customers Customer
    join Sales.Orders Orders
        on Customer.CustomerID = Orders.CustomerID
where Customer.CustomerID = 931
FOR JSON AUTO, WITHOUT_ARRAY_WRAPPER

```

Zamiast wysyłania oddzielaj kwerendy w celu uzyskania danych klientów, a następnie pobierać lista zamówień powiązanych, można znaleźć wszystkie niezbędne dane za pomocą jednej kwerendy, jak pokazano na poniższe przykładowe wyniki:

```
{
  "Name":"Nada Jovanovic",
  "Phone":"(215) 555-0100",
  "Fax":"(215) 555-0101",
  "Orders":[
    {"OrderID":382,"OrderDate":"2013-01-07","ExpectedDeliveryDate":"2013-01-08"},
    {"OrderID":395,"OrderDate":"2013-01-07","ExpectedDeliveryDate":"2013-01-08"},
    {"OrderID":1657,"OrderDate":"2013-01-31","ExpectedDeliveryDate":"2013-02-01"}
]
}
```

## <a name="working-with-json-data"></a>Praca z danymi JSON

Jeśli nie masz danych strukturalnych ściśle, jeśli masz złożone obiekty podrzędne, tablice lub danych hierarchicznych lub struktury danych użytkownika są obsługiwane w czasie, JSON format pomoże Ci odpowiada każdą strukturę złożonych danych.

JSON jest w formacie tekstowym, który może być używany w innych typów parametrów w bazie danych SQL Azure. Można wysyłać i przechowywania danych JSON jako standardowy NVARCHAR:

```
CREATE TABLE Products (
  Id int identity primary key,
  Title nvarchar(200),
  Data nvarchar(max)
)
go
CREATE PROCEDURE InsertProduct(@title nvarchar(200), @json nvarchar(max))
AS BEGIN
    insert into Products(Title, Data)
    values(@title, @json)
END
```

W tym przykładzie użyto danych JSON jest reprezentowany przez przy użyciu typu NVARCHAR(MAX). JSON można wstawione w tej tabeli lub podana jako argument procedura składowana za pomocą standardowej składni języka Transact-SQL, jak pokazano w poniższym przykładzie:

```
EXEC InsertProduct 'Toy car', '{"Price":50,"Color":"White","tags":["toy","children","games"]}'
```

Język po stronie klienta lub bibliotekę, w której działa ciąg danych w bazie danych SQL Azure będą również działać z danymi JSON. JSON mogą być przechowywane w dowolnej tabeli, który obsługuje typ NVARCHAR, na przykład pamięć zoptymalizowana tabelę lub tabelę wersji systemu. JSON nie wprowadzają wszelkie ograniczenia w kodzie po stronie klienta lub w warstwie bazy danych.

## <a name="querying-json-data"></a>Badania danych JSON

Jeśli masz danych w formacie JSON przechowywane w tabelach Azure SQL, JSON funkcje umożliwiają używać tych danych w jakiekolwiek zapytania SQL.

Funkcje JSON, które są dostępne w umożliwiają bazy danych Azure SQL Potraktuj danych w formacie JSON jako inny typ danych SQL. Można łatwo wyodrębnić wartości w tekście JSON i używanie JSON danych w jakiejkolwiek kwerendy:

```
select Id, Title, JSON_VALUE(Data, '$.Color'), JSON_QUERY(Data, '$.tags')
from Products
where JSON_VALUE(Data, '$.Color') = 'White'

update Products
set Data = JSON_MODIFY(Data, '$.Price', 60)
where Id = 1
```

Funkcja JSON_VALUE wyodrębnia wartości z tekstu JSON przechowywanej w kolumnie danych. Ta funkcja używa ścieżki JavaScript przypominających odwoływać się do wartości w tekście JSON, aby wyodrębnić. Wyodrębnionej wartość może być używana w dowolnej części kwerendy SQL.

Funkcja JSON_QUERY jest podobna do JSON_VALUE. W odróżnieniu od JSON_VALUE ta funkcja wyodrębnia złożonych podrzędne obiektu, takiego jak tablicami lub obiekty, które są umieszczane w tekście JSON.

Funkcja JSON_MODIFY pozwala określić ścieżkę wartości w JSON tekst, który należy zaktualizować, a także nową wartość, która powoduje zastąpienie starego. W ten sposób można łatwo aktualizować JSON tekst bez reparsing całą strukturę.

Ponieważ JSON jest przechowywany w postaci zwykłego tekstu, istnieje żadnych gwarancji, że wartości znajdujących się w kolumnach tekstu są prawidłowo sformatowane. Możesz sprawdzić, czy tekst są przechowywane w kolumnie JSON prawidłowo został sformatowany przy użyciu standardowych ograniczeń check bazy danych SQL Azure i funkcji ISJSON:

```
ALTER TABLE Products
    ADD CONSTRAINT [Data should be formatted as JSON]
        CHECK (ISJSON(Data) > 0)
```

Jeśli wprowadzania tekstu jest prawidłowo sformatowana JSON, funkcja ISJSON zwraca wartość 1. Na każdej insert lub update JSON kolumny to ograniczenie zostanie sprawdzić, czy nowe wartości tekstowej jest nieprawidłowo JSON.

## <a name="transforming-json-into-tabular-format"></a>Przekształcania JSON w formacie tabelarycznym

Baza danych SQL Azure umożliwia również Przekształcanie kolekcji JSON do dane tabelaryczne JSON formatowanie i ładowanie lub kwerendy.

OPENJSON jest to funkcja wartość tabeli, która analizuje tekst JSON, zwraca tablicę obiektów JSON iterację elementów tablicy i zwraca jeden wiersz w wyniku wynik dla każdego elementu w tablicy.

![JSON tabelaryczny](./media/sql-database-json-features/image_2.png)

W powyższym przykładzie firma Microsoft można określić gdzie umieścić tablicy JSON, która musi być otwarty (w $. Ścieżka zamówienia), które kolumny powinny być zwracane jako wynik i gdzie można znaleźć wartości JSON, które są przedstawiane jako komórki.

Firma Microsoft przekształcanie tablicę JSON w @orders zmiennej na zestaw wierszy, analizowanie zestaw wyników lub wstawianie wierszy do standardowej tabeli:

```
CREATE PROCEDURE InsertOrders(@orders nvarchar(max))
AS BEGIN

    insert into Orders(Number, Date, Customer, Quantity)
    select Number, Date, Customer, Quantity
    OPENJSON (@orders)
     WITH (
            Number varchar(200),
            Date datetime,
            Customer varchar(200),
            Quantity int
     )

END
```
Kolekcji zamówień sformatowane jako tablicę JSON i opisane parametr procedura składowana można przeanalizować i wstawione w tabeli zamówienia.

## <a name="next-steps"></a>Następne kroki

Aby dowiedzieć się, jak zintegrować JSON aplikacji, zapoznaj się z następujących zasobów:

- [Blogu w witrynie TechNet](https://blogs.technet.microsoft.com/dataplatforminsider/2016/01/05/json-in-sql-server-2016-part-1-of-4/)
- [Dokumentacja w witrynie MSDN](https://msdn.microsoft.com/library/dn921897.aspx)
- [Kanał wideo 9](https://channel9.msdn.com/Shows/Data-Exposed/SQL-Server-2016-and-JSON-Support)

Aby uzyskać informacje o różnych scenariuszach zintegrować JSON aplikacji, zobacz pokazy w tym [9 kanału wideo](https://channel9.msdn.com/Events/DataDriven/SQLServer2016/JSON-as-a-bridge-betwen-NoSQL-and-relational-worlds) lub znaleźć scenariusza, która jest zgodna z przypadków użycia w [wpisów w blogu JSON](http://blogs.msdn.com/b/sqlserverstorageengine/archive/tags/json/).
