<properties 
    pageTitle="Praca z danymi geograficzne w Azure DocumentDB | Microsoft Azure" 
    description="Opis sposobu tworzenia, indeks i kwerendy przestrzenna obiektów z Azure DocumentDB." 
    services="documentdb" 
    documentationCenter="" 
    authors="arramac" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="documentdb" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="10/25/2016" 
    ms.author="arramac"/>
    
# <a name="working-with-geospatial-data-in-azure-documentdb"></a>Praca z danymi geograficzne w Azure DocumentDB

Ten artykuł dotyczy wprowadzenie do funkcji geograficzne w [Azure DocumentDB](https://azure.microsoft.com/services/documentdb/). Po zapoznaniu się tak, można odpowiedzieć na następujące pytania:

- Jak przechowywanie danych przestrzenna w Azure DocumentDB?
- Jak można wysyłać kwerendy danych geograficznych w DocumentDB Azure SQL i LINQ?
- Jak włączyć lub wyłączyć przestrzenna indeksowanie w DocumentDB?

Zobacz przykłady kodu tego [projektu Github](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/Geospatial/Program.cs) .

## <a name="introduction-to-spatial-data"></a>Wprowadzenie do przestrzenna danych

Dane przestrzenna opisano położenie i kształt obiektów w miejscu. W większości aplikacji odpowiadają one obiektów na ziemi, to znaczy danych geograficznych. Przestrzenna danych może służyć reprezentować lokalizacji osoby, miejsce zainteresowania lub granicy miasta lub lake. Typowe przypadki użycia obejmować często kwerendy odległość, np "Znajdź wszystkie kawiarni u swoją bieżącą lokalizację". 

### <a name="geojson"></a>GeoJSON
DocumentDB obsługuje indeksowania i wykonywanie zapytań za geograficzne punktu danych, który jest reprezentowana za pomocą [Specyfikacja GeoJSON](http://geojson.org/geojson-spec.html). Struktury danych GeoJSON są zawsze prawidłowych obiektów JSON, aby mogli oni mogą być przechowywane i kwerendy przy użyciu DocumentDB bez specjalistyczne narzędzia lub bibliotek. SDK DocumentDB Podaj klasy pomocy i metod, które ułatwiają pracę z danymi przestrzenna. 

### <a name="points-linestrings-and-polygons"></a>Punkty, linestrings i wielokątów
**Punkt** oznacza w jednym miejscu w miejsca. W danych geograficznych punktu reprezentuje konkretnej lokalizacji, który może być ulicy sklep spożywczy, kiosk, samochodu lub miasta.  Punkt jest reprezentowany w pary GeoJSON (i DocumentDB) przy użyciu jego współrzędnych lub długości i szerokości geograficznych. Oto przykład JSON punktu.

**Punkty DocumentDB**

    {
       "type":"Point",
       "coordinates":[ 31.9, -4.8 ]
    }

>[AZURE.NOTE] Specyfikacja GeoJSON umożliwia określenie długości geograficznej imię i szerokości drugiego. Podobnie jak w innych aplikacjach mapowania szerokości i długości geograficznej są kątami i są reprezentowane w stopniach. Długość geograficzna wartości są mierzone od południka technologii Prime i między-180 i 180.0 stopni i szerokości wartości są mierzone od równika i między-90.0 i 90.0 stopni. 
>
> DocumentDB współrzędne są interpretowane jako przedstawione na układ odniesienia WGS 84. Zobacz poniżej uzyskać więcej szczegółowych informacji o systemach współrzędnych odwołania.

To można osadzić w dokumencie DocumentDB, jak pokazano w tym przykładzie danymi lokalizację profilu użytkownika:

**Używanie profilu z lokalizacji przechowywania w DocumentDB**

    {
       "id":"documentdb-profile",
       "screen_name":"@DocumentDB",
       "city":"Redmond",
       "topics":[ "NoSQL", "Javascript" ],
       "location":{
          "type":"Point",
          "coordinates":[ 31.9, -4.8 ]
       }
    }

Oprócz punktów GeoJSON obsługuje także LineStrings i wielokąty. **LineStrings** reprezentują dwa lub więcej punktów w miejsce i segmentów, które je łączą. W polu dane geograficzne linestrings są często używane do przedstawiania autostrady i rzeki. **Wielokąt** jest granicę połączone punkty formularzy z zamkniętych obiektów LineString. Wielokąty są często używane do przedstawiania formacji naturalne, takie jak lak i Stanach krajach, takich jak miasta i Państwa. Oto przykład wielokąta w DocumentDB. 

**Wielokąty DocumentDB**

    {
       "type":"Polygon",
       "coordinates":[
           [ 31.8, -5 ],
           [ 31.8, -4.7 ],
           [ 32, -4.7 ],
           [ 32, -5 ],
           [ 31.8, -5 ]
       ]
    }

>[AZURE.NOTE] Specyfikacja GeoJSON wymaga dla prawidłowych wieloboków, ostatnią parę współrzędnych opisane należy taki sam, jak pierwsze, aby utworzyć kształt zamknięty.
>
>Punkty w wielokąta musi być określony w kolejności przeciwnie do ruchu wskazówek zegara. Wielokąta określona w kolejności do ruchu wskazówek zegara reprezentuje odwrotność region znajdujące się w nim.

Oprócz punktu, obiektów LineString i Wielokąt GeoJSON określa reprezentacją sposób grupowania wielu lokalizacje geograficzne, a także jak skojarzyć właściwości dowolnego z geolokalizacja jako **Funkcja**. Ponieważ tych obiektów są prawidłowe JSON, mogą wszystkie być przechowywane i przetwarzane w DocumentDB. Jednak DocumentDB obsługuje tylko, automatyczne indeksowanie punktów.

### <a name="coordinate-reference-systems"></a>Koordynowanie systemów odniesienia

Ponieważ kształt ziemi jest o nieregularnym kształcie, współrzędne danych geograficznych jest reprezentowany w wielu systemach współrzędnych odwołanie (CRS), każda z własnych ramy odniesienia i jednostki miary. Na przykład "krajowe siatki z Brytanii" to jest bardzo dokładny układ odniesienia dla Zjednoczonego Królestwa, ale nie poza nią. 

Najpopularniejsze używany dzisiaj jest System geodezyjny świata [WGS 84](http://earth-info.nga.mil/GandG/wgs84/). Urządzenia GPS i wiele usług mapowania, łącznie z mapami Google i interfejsów API mapy Bing za pomocą WGS 84. DocumentDB obsługuje indeksowanie i wyszukiwanie przy użyciu WGS 84 komputerowy system Rezerwacji tylko danych geograficznych. 

## <a name="creating-documents-with-spatial-data"></a>Tworzenie dokumentów z danymi przestrzenna
Tworząc dokumenty, które zawierają wartości GeoJSON one są zgodnie z indeksowania zasady zbioru automatycznie indeksowane z indeksem przestrzenna. Jeśli pracujesz z zestawu SDK DocumentDB typami języka, takie jak Python lub Node.js, musisz utworzyć prawidłowy GeoJSON.

**Tworzenie dokumentu zawierającego dane geograficzne w Node.js**

    var userProfileDocument = {
       "name":"documentdb",
       "location":{
          "type":"Point",
          "coordinates":[ -122.12, 47.66 ]
       }
    };

    client.createDocument(`dbs/${databaseName}/colls/${collectionName}`, userProfileDocument, (err, created) => {
        // additional code within the callback
    });

Jeśli pracujesz z .NET (lub Java) SDK, możesz za pomocą nowych klas punktu i Wielokąt w obszarze nazw Microsoft.Azure.Documents.Spatial osadzanie informacji o lokalizacji w obiekty aplikacji. Te klasy znacznie upraszcza szeregowania i deserializacji przestrzenna danych do GeoJSON.

**Tworzenie dokumentu zawierającego dane geograficzne w .NET**

    using Microsoft.Azure.Documents.Spatial;
    
    public class UserProfile
    {
        [JsonProperty("name")]
        public string Name { get; set; }

        [JsonProperty("location")]
        public Point Location { get; set; }
        
        // More properties
    }
    
    await client.CreateDocumentAsync(
        UriFactory.CreateDocumentCollectionUri("db", "profiles"), 
        new UserProfile 
        { 
            Name = "documentdb", 
            Location = new Point (-122.12, 47.66) 
        });

Jeśli nie masz informacje o szerokości i długości geograficznej, ale adresów fizycznych lub nazwy lokalizacji, takiej jak miasto lub kraj, współrzędne rzeczywiste można wyszukiwać przy użyciu usług geokodowania, takich jak usługi REST mapy Bing. Dowiedz się więcej o geokodowania mapy Bing [w tym miejscu](https://msdn.microsoft.com/library/ff701713.aspx).

## <a name="querying-spatial-types"></a>Kwerenda przestrzenna typów

Teraz, gdy firma Microsoft podjęciu przegląd sposobów wstawiania danych geograficznych, Przejdźmy zapoznaj się jak wyszukiwać te dane za pomocą DocumentDB przy użyciu SQL i LINQ.

### <a name="spatial-sql-built-in-functions"></a>Funkcje wbudowane przestrzenna SQL
DocumentDB obsługuje następujące funkcje wbudowane Otwórz geograficzne Consortium (OGC) do wykonywania kwerend geograficznych. Więcej informacji o kompletny zestaw wbudowanych funkcji w języku SQL można znaleźć w [DocumentDB kwerendy](documentdb-sql-query.md).

<table>
<tr>
  <td><strong>Użycie</strong></td>
  <td><strong>Opis</strong></td>
</tr>
<tr>
  <td>ST_DISTANCE (point_expr, point_expr)</td>
  <td>Zwraca odległości między dwoma wyrażeniami punktu GeoJSON.</td>
</tr>
<tr>
  <td>ST_WITHIN (point_expr, polygon_expr)</td>
  <td>Zwraca wartość wskazującą, czy punktu GeoJSON określonej w pierwszym argumencie jest w Wielokąt GeoJSON w drugim argumencie Wyrażenie logiczne.</td>
</tr>
<tr>
  <td>ST_ISVALID</td>
  <td>Zwraca wartość logiczną wskazującą, czy określone wyrażenie punkt lub Wielokąt GeoJSON jest prawidłowy.</td>
</tr>
<tr>
  <td>ST_ISVALIDDETAILED</td>
  <td>Zwraca wartość JSON zawierających wartość logiczną wartość, jeśli określony punkt lub Wielokąt wyrażenie GeoJSON jest prawidłowe i nieprawidłowe, ponadto przyczyny jako wartość ciągu.</td>
</tr>
</table>

Funkcje przestrzenna może służyć do wykonywania kwerend odległość przestrzenna danych. Na przykład Oto kwerendę, która zwraca wszystkie dokumenty rodziny, które znajdują się w 30 km we wskazanej lokalizacji przy użyciu wbudowanych funkcji ST_DISTANCE. 

**Kwerendy**

    SELECT f.id 
    FROM Families f 
    WHERE ST_DISTANCE(f.location, {'type': 'Point', 'coordinates':[31.9, -4.8]}) < 30000

**Wyniki**

    [{
      "id": "WakefieldFamily"
    }]

Jeśli dołączysz przestrzenna indeksowania w indeksowania zasad, następnie "odległość kwerendy" będzie udostępniania efektywne za pośrednictwem indeks. Aby uzyskać więcej informacji na temat indeksowania przestrzenna zobacz poniższą sekcję. Jeśli nie masz przestrzenna indeksu dla określonej ścieżki, można wykonywać przestrzenna zapytania, określając `x-ms-documentdb-query-enable-scan` żądanie nagłówka z wartość "prawda". W .NET można to zrobić przez przekazanie argument opcjonalny **FeedOptions** do kwerendy z [EnableScanInQuery](https://msdn.microsoft.com/library/microsoft.azure.documents.client.feedoptions.enablescaninquery.aspx#P:Microsoft.Azure.Documents.Client.FeedOptions.EnableScanInQuery) ustawioną na wartość PRAWDA. 

ST_WITHIN może służyć do sprawdzania, jeśli punkt znajduje się w obrębie wielokąta. Często wielokąty są używane do przedstawiania ograniczenia, takie jak kody pocztowe, stan granic lub naturalne kształty. Ponownie Jeśli przestrzenna indeksowania w indeksowania zasad, następnie "w" kwerend będzie udostępniania efektywne za pośrednictwem indeks. 

Argumenty Wielokąt ST_WITHIN może zawierać tylko pojedynczy dzwonek, to znaczy wielokątami nie może zawierać dziury w nich. 

**Kwerendy**

    SELECT * 
    FROM Families f 
    WHERE ST_WITHIN(f.location, {
        'type':'Polygon', 
        'coordinates': [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]
    })

**Wyniki**

    [{
      "id": "WakefieldFamily",
    }]
    
>[AZURE.NOTE] Podobnie jak Niedopasowane typy działa w kwerendzie DocumentDB, jeśli wartość lokalizacji określona w albo argument jest nieprawidłowo lub nieprawidłowe, a następnie będzie oceniać **Nieokreślone** i szacowane dokumentu, aby pominięte z wyników kwerendy. Jeśli kwerenda zwraca żadnych wyników, uruchom ST_ISVALIDDETAILED do debugowania Dlaczego typ spatail jest nieprawidłowy.     

DocumentDB obsługuje również wykonywanie kwerendy odwrotne, to znaczy możesz indeksu wielokątów lub linii w DocumentDB, a następnie kwerendy dla obszarów, które zawierają określony punkt. Ten wzorzec jest rzadko używana w logistyki do identyfikowania np., gdy wózek wprowadza lub pozostawia oznaczonym obszarem. 

**Kwerendy**

    SELECT * 
    FROM Areas a 
    WHERE ST_WITHIN({'type': 'Point', 'coordinates':[31.9, -4.8]}, a.location)


**Wyniki**

    [{
      "id": "MyDesignatedLocation",
      "location": {
        "type":"Polygon", 
        "coordinates": [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]
      }
    }]

ST_ISVALID i ST_ISVALIDDETAILED można sprawdzić, czy przestrzenna obiekt jest prawidłowy. Na przykład poniższa kwerenda sprawdza ważność punktu z nowym wartość szerokości zakresu (-132.8). ST_ISVALID zwraca wartość logiczną i ST_ISVALIDDETAILED zwraca wartość logiczna i ciąg zawierający powód, dlaczego jest traktowany jako nieprawidłowe.

**Kwerendy**

    SELECT ST_ISVALID({ "type": "Point", "coordinates": [31.9, -132.8] })

**Wyniki**

    [{
      "$1": false
    }]

Aby sprawdzić poprawność wielokątów można także te funkcje. Na przykład, w tym miejscu firma Microsoft korzysta z ST_ISVALIDDETAILED do sprawdzania poprawności wielokąt, który nie jest zamknięty. 

**Kwerendy**

    SELECT ST_ISVALIDDETAILED({ "type": "Polygon", "coordinates": [[ 
        [ 31.8, -5 ], [ 31.8, -4.7 ], [ 32, -4.7 ], [ 32, -5 ] 
        ]]})

**Wyniki**

    [{
       "$1": { 
          "valid": false, 
          "reason": "The Polygon input is not valid because the start and end points of the ring number 1 are not the same. Each ring of a polygon must have the same start and end points." 
        }
    }]
    
### <a name="linq-querying-in-the-net-sdk"></a>LINQ kwerend w zestawie SDK .NET

Zestaw SDK programu .NET DocumentDB również dostawców wejściowy metody `Distance()` i `Within()` do użytku w obrębie wyrażenia LINQ. Dostawca DocumentDB LINQ tłumaczy te wywołania metody do odpowiednich połączeń wbudowanej funkcji SQL (ST_DISTANCE i ST_WITHIN odpowiednio). 

Oto przykład kwerendy LINQ, która umożliwia znalezienie wszystkich dokumentów w zbiorze DocumentDB, której wartość "położenie" jest w promieniu 30km określonej wskaż przy użyciu LINQ.

**Zapytanie w języku LINQ odległość**

    foreach (UserProfile user in client.CreateDocumentQuery<UserProfile>(UriFactory.CreateDocumentCollectionUri("db", "profiles"))
        .Where(u => u.ProfileType == "Public" && a.Location.Distance(new Point(32.33, -4.66)) < 30000))
    {
        Console.WriteLine("\t" + user);
    }

Podobnie Oto kwerendy dla wszystkich dokumentów, których "położenie" znajduje się w określonym polu wielokąt. 

**LINQ kwerendy w**

    Polygon rectangularArea = new Polygon(
        new[]
        {
            new LinearRing(new [] {
                new Position(31.8, -5),
                new Position(32, -5),
                new Position(32, -4.7),
                new Position(31.8, -4.7),
                new Position(31.8, -5)
            })
        });

    foreach (UserProfile user in client.CreateDocumentQuery<UserProfile>(UriFactory.CreateDocumentCollectionUri("db", "profiles"))
        .Where(a => a.Location.Within(rectangularArea)))
    {
        Console.WriteLine("\t" + user);
    }


Teraz, gdy mamy podjęciu przyjrzeć się jak wyszukiwać dokumentów przy użyciu LINQ i języka SQL, Spójrzmy na sposób konfigurowania DocumentDB przestrzenna indeksowania.

## <a name="indexing"></a>Indeksowanie

Zgodnie z opisem możemy w [Schematu niezależne od indeksowania przy użyciu Azure DocumentDB](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf) papieru, możemy przeznaczona DocumentDB przez aparat bazy danych należy naprawdę niezależne od schematu i podaj pierwszej klasie obsługę JSON. Aparat bazy danych zapisu zoptymalizowane programu DocumentDB rozumie oryginalnie przestrzenna danych (punktów, wielokąty i linie) reprezentowany w standardzie GeoJSON.

W skrócie geometrię jest planowanego geodezyjnej współrzędne na płaszczyznę 2D, a następnie stopniowo podzielone komórki przy użyciu **quadtree**. Te komórki są mapowane na 1D na podstawie lokalizacji komórki znajdujące się wewnątrz **Hilberta miejsca wypełnianie krzywej**, który zachowuje miejscowości punktów. Ponadto w przypadku, gdy jest indeksowane danych lokalizacji, wywołuje proces znany jako **Tesselacja**, to znaczy wszystkie komórki, które przecinają się lokalizację, w której są zidentyfikowane i przechowywane jako klucze w indeksie DocumentDB. W momencie kwerendy argumenty, takich jak punkty i wielokąty są również tesselowaną aby wyodrębnić zakresów identyfikator odpowiednich komórek, a następnie używane do pobierania danych z indeksu.

Jeśli Określ indeksowania zasady, które zawiera przestrzenna indeks / * (wszystkie ścieżki), wszystkie punkty znaleziony w zbiorze są indeksowane wydajność kwerend przestrzenna (ST_WITHIN i ST_DISTANCE), a następnie. Indeksy przestrzenna nie ma wartość precyzji i zawsze używaj domyślną wartość dokładności.

>[AZURE.NOTE] DocumentDB obsługuje automatyczne indeksowanie punktów, wielokątów i LineStrings

Poniższy fragment JSON zawiera zasad indeksowania z przestrzenna indeksowania włączone, to znaczy indeksu punkcie GeoJSON znaleziony w dokumentach dla przestrzenna kwerendy. W przypadku modyfikowania indeksowania zasady za pomocą portalu Azure, można określić następujące JSON zasady indeksowania umożliwiające przestrzenna indeksowania w zbiorze.

**Kolekcja indeksowania zasad JSON z Spatial dla punktów i wielokąty**

    {
       "automatic":true,
       "indexingMode":"Consistent",
       "includedPaths":[
          {
             "path":"/*",
             "indexes":[
                {
                   "kind":"Range",
                   "dataType":"String",
                   "precision":-1
                },
                {
                   "kind":"Range",
                   "dataType":"Number",
                   "precision":-1
                },
                {
                   "kind":"Spatial",
                   "dataType":"Point"
                },
                {
                   "kind":"Spatial",
                   "dataType":"Polygon"
                }                
             ]
          }
       ],
       "excludedPaths":[
       ]
    }

Poniżej przedstawiono wstawkę kodu, w którym pokazano, jak utworzyć zbiór z przestrzenna indeksowania włączone dla wszystkich ścieżek zawierające punktów. 

**Tworzenie zbioru z przestrzenna indeksowania**

    DocumentCollection spatialData = new DocumentCollection()
    spatialData.IndexingPolicy = new IndexingPolicy(new SpatialIndex(DataType.Point)); //override to turn spatial on by default
    collection = await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), spatialData);

A Oto jak można zmodyfikować istniejącego zbioru skorzystać z przestrzenna indeksowanie wszelkie punktów, które są przechowywane w dokumentach.

**Modyfikowanie istniejącego zbioru z przestrzenna indeksowania**

    Console.WriteLine("Updating collection with spatial indexing enabled in indexing policy...");
    collection.IndexingPolicy = new IndexingPolicy(new SpatialIndex(DataType.Point));
    await client.ReplaceDocumentCollectionAsync(collection);

    Console.WriteLine("Waiting for indexing to complete...");
    long indexTransformationProgress = 0;
    while (indexTransformationProgress < 100)
    {
        ResourceResponse<DocumentCollection> response = await client.ReadDocumentCollectionAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"));
        indexTransformationProgress = response.IndexTransformationProgress;

        await Task.Delay(TimeSpan.FromSeconds(1));
    }

> [AZURE.NOTE] W przypadku lokalizacji wartość GeoJSON w dokumencie utworzonym lub jest nieprawidłowy, następnie go będzie nie indeksowanie dla przestrzenna kwerend. Można sprawdzić lokalizację wartości przy użyciu ST_ISVALID i ST_ISVALIDDETAILED.
>
> Jeśli do definicji zbioru zawiera klucz partition, indeksowania postępu transformacja nie jest zgłaszany. 

## <a name="next-steps"></a>Następne kroki
Teraz, gdy zostanie zostały wcześniej o wprowadzenie obsługi geograficzne w DocumentDB, można wykonywać następujące czynności:

- Rozpoczynanie kodowania [Przykłady kodu .NET Geoprzestrzennych na Github](https://github.com/Azure/azure-documentdb-dotnet/blob/fcf23d134fc5019397dcf7ab97d8d6456cd94820/samples/code-samples/Geospatial/Program.cs)
- Uzyskaj wskazówki z geograficzne kwerendy na [Playground kwerendy DocumentDB](http://www.documentdb.com/sql/demo#geospatial)
- Dowiedz się więcej o [DocumentDB kwerendy](documentdb-sql-query.md)
- Dowiedz się więcej o [Zasadach indeksowania DocumentDB](documentdb-indexing-policies.md)
