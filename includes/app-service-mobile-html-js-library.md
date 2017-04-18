##<a name="create-client"></a>Tworzenie połączenia klienta

Tworzenie połączenia klienta, tworząc `WindowsAzure.MobileServiceClient` obiektu.  Zamienianie `appUrl` z adresem URL do Twojej aplikacji dla urządzeń przenośnych.

```
var client = WindowsAzure.MobileServiceClient(appUrl);
```

##<a name="table-reference"></a>Praca z tabelami

Do programu access lub aktualizowanie danych tworzenie odwołania do tabeli wewnętrznej bazy danych. Zamienianie `tableName` o nazwie tabeli

```
var table = client.getTable(tableName);
```

Po umieszczeniu odwołania do tabeli, możesz pracować jeszcze tabeli:

* [Kwerendy tabeli](#querying)
  * [Filtrowanie danych](#table-filter)
  * [Nawigacji między stronami w danych](#table-paging)
  * [Sortowanie danych](#sorting-data)
* [Wstawianie danych](#inserting)
* [Modyfikowanie danych](#modifying)
* [Usuwanie danych](#deleting)

###<a name="querying"></a>Jak: kwerendy odwołania do tabeli

Po umieszczeniu odwołania do tabeli, można go kwerendy danych na serwerze.  Kwerendy są tworzone w języku "Przypominających LINQ".
Aby przywrócić wszystkie dane z tabeli, użyj następujących opcji:

```
/**
 * Process the results that are received by a call to table.read()
 *
 * @param {Object} results the results as a pseudo-array
 * @param {int} results.length the length of the results array
 * @param {Object} results[] the individual results
 */
function success(results) {
   var numItemsRead = results.length;

   for (var i = 0 ; i < results.length ; i++) {
       var row = results[i];
       // Each row is an object - the properties are the columns
   }
}

function failure(error) {
    throw new Error('Error loading data: ', error);
}

table
    .read()
    .then(success, failure);
```

Funkcja sukcesu jest wywoływana z wynikami.   Nie należy używać `for (var i in results)` powodzenia działają zgodnie z którą będzie iteracyjne informacje zawarte w wynikach po innych funkcji kwerendy (takie jak `.includeTotalCount()`) są używane.

Aby uzyskać więcej informacji o składnię kwerend zapoznaj się z [dokumentacją obiektu kwerendy].

####<a name="table-filter"></a>Filtrowanie danych na serwerze

Możesz użyć `where` klauzula w odwołaniu do tabeli:

```
table
    .where({ userId: user.userId, complete: false })
    .read()
    .then(success, failure);
```

Można także użyć funkcji filtrująca obiektu.  W tym przypadku `this` zmienna jest przypisana do bieżącego obiektu filtrowana.  Oto funkcjonalny odpowiednik poprzedniego przykładu:

```
function filterByUserId(currentUserId) {
    return this.userId === currentUserId && this.complete === false;
}

table
    .where(filterByUserId, user.userId)
    .read()
    .then(success, failure);
```

####<a name="table-paging"></a>Nawigacji między stronami w danych

Korzystanie z metod take() i skip().  Na przykład, jeśli chcesz podzielić wiersz 100 rekordów w tabeli:

```
var totalCount = 0, pages = 0;

// Step 1 - get the total number of records
table.includeTotalCount().take(0).read(function (results) {
    totalCount = results.totalCount;
    pages = Math.floor(totalCount/100) + 1;
    loadPage(0);
}, failure);

function loadPage(pageNum) {
    let skip = pageNum * 100;
    table.skip(skip).take(100).read(function (results) {
        for (var i = 0 ; i < results.length ; i++) {
            var row = results[i];
            // Process each row
        }
    }
}
```

`.includeTotalCount()` Metoda jest używana do dodawania pola totalCount do obiektu wyników.  W polu totalCount jest wyświetlana wartość całkowitą liczbę rekordów, które będzie zwracany użycie nie nawigacji między stronami.

Aby utworzyć listę strony; można użyć zmiennej stron i kilka przycisków interfejsu użytkownika loadPage() umożliwia ładowanie nowych rekordów dla każdej strony.  Wykonała jakiegoś wprowadzania do przyspieszyć dostęp do rekordów, które zostały już załadowane pamięci podręcznej.


####<a name="sorting-data"></a>Jak: zwracają dane posortowane

Użyj metody zapytania .orderBy() lub .orderByDescending():

```
table
    .orderBy('name')
    .read()
    .then(success, failure);
```

Aby uzyskać więcej informacji w obiekcie zapytania zajrzyj do [dokumentacji obiektu kwerendy].

###<a name="inserting"></a>Jak: wstawianie danych

Tworzenie obiektu JavaScript z odpowiednią datę i asynchroniczne połączeń table.insert():

```
var newItem = {
    name: 'My Name',
    signupDate: new Date()
};

table
    .insert(newItem)
    .done(function (insertedItem) {
        var id = insertedItem.id;
    }, failure);
```

Na pomyślne wstawiania wstawiony element jest zwracana z dodatkowych pól, które są wymagane do działania synchronizacji.  Należy zaktualizować własnej pamięci podręcznej z tych informacji dotyczących późniejsze aktualizacje.

Należy zauważyć, że Azure Mobile aplikacje Node.js Server SDK obsługuje dynamiczne schematu na potrzeby projektowania.
W przypadku schematu dynamiczne schemat tabeli zostanie następnie zaktualizowany w czasie rzeczywistym, co pozwala na dodawanie kolumn do tabeli po prostu, określając ich wstawiania lub aktualizowanie operacji.  Zaleca się wyłączenie schematu dynamiczne przed przeniesieniem aplikacji z produkcją.

###<a name="modifying"></a>Jak: modyfikowania danych

Podobnie jak metody .insert(), należy utworzyć obiekt aktualizacji, a następnie zadzwoń .update().  Obiekt aktualizacji musi zawierać identyfikator rekordu mają być aktualizowane — mogą być analizowane podczas czytania rekordu lub podczas połączenia .insert().

```
var updateItem = {
    id: '7163bc7a-70b2-4dde-98e9-8818969611bd',
    name: 'My New Name'
};

table
    .update(updateItem)
    .done(function (updatedItem) {
        // You can now update your cached copy
    }, failure);
```

###<a name="deleting"></a>Jak: usuwanie danych

Wywołaj metodę .del(), aby usunąć rekord.  Przekaż Identyfikator odwołania do obiektu:

```
table
    .del({ id: '7163bc7a-70b2-4dde-98e9-8818969611bd' })
    .done(function () {
        // Record is now deleted - update your cache
    }, failure);
```
