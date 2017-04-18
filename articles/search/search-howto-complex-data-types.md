<properties
    pageTitle="Jak modelu złożonymi typami danych w wyszukiwaniu Azure | Wyszukiwanie Microsoft Azure"
    description="Zagnieżdżone lub danych hierarchicznych struktur można modelowania do indeksu wyszukiwania Azure za pomocą spłaszczoną wierszy i typ zbiorów danych."
    services="search"
    documentationCenter=""
    authors="LiamCa"
    manager="pablocas"
    editor=""
    tags="complex data types; compound data types; aggregate data types"
/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.date="09/07/2016"
    ms.author="liamca"
/>

# <a name="how-to-model-complex-data-types-in-azure-search"></a>Jak modelu złożonymi typami danych w wyszukiwaniu Azure

Zestawy danych zewnętrznych, użyte do zapełnienia indeksu wyszukiwania Azure czasami zawierać hierarchicznych lub zagnieżdżona struktur podrzędnych, które nie Podziel się starannie do wierszy tabeli. Przykłady te struktury może obejmują wiele lokalizacji i numerów telefonów dla jednego odbiorcy, wiele kolorów i rozmiarów dla jednej wersji produktu wielu autorów jednej książki i tak dalej. Modelowanie warunki, może zostać wyświetlony te struktury określane jako *złożonymi typami danych*, *złożone typy danych*, *złożonych typów danych*i *agregowanie typów danych*, kilka.

Złożonymi typami danych nie są oryginalnie obsługiwane w wyszukiwaniu Azure, ale sprawdzone obejście zawiera procesem dwuetapowym spłaszczanie strukturę i Przywróć wewnętrznej struktury przy użyciu typu **zbioru** danych. Wykonanie techniki opisane w tym artykule umożliwi ma być przeszukiwana zawartość aspektowej, filtrować i sortować.

## <a name="example-of-a-complex-data-structure"></a>Przykład struktury złożonych danych

Zazwyczaj w danym znajdują się dane jako zestawu dokumentów JSON lub XML, lub elementów w sklepie NoSQL, takich jak DocumentDB. Strukturę największym wyzwaniem wynika z o wiele elementów podrzędnych, które muszą być przeszukiwane i filtrowane.  Jako punkt początkowy dla ilustrujący ten problem wykonaj następujący dokument JSON zawierającym zbiór kontaktów, na przykład:

~~~~~
[
  {
    "id": "1",
    "name": "John Smith",
    "company": "Adventureworks",
    "locations": [
      {
        "id": "1",
        "description": "Adventureworks Headquarters"
      },
      {
        "id": "2",
        "description": "Home Office"
      }
    ]
  }, 
  {
    "id": "2",
    "name": "Jen Campbell",
    "company": "Northwind",
    "locations": [
      {
        "id": "3",
        "description": "Northwind Headquarter"
      },
      {
        "id": "4",
        "description": "Home Office"
      }
    ]
}]
~~~~~

Gdy pola o nazwie "identyfikator", "Nazwa" i "firm" można łatwo mapować jeden jako pola w obrębie indeksu wyszukiwania Azure, pole "lokalizacje" zawiera tablicę lokalizacji, o obu zestaw identyfikatorów lokalizacji, a także opisy lokalizacji. Zakładając, że Azure wyszukiwania nie ma typ danych, który obsługuje tę funkcję, musisz w inny sposób modelu to w wyszukiwaniu Azure. 

> [AZURE.NOTE] Ta metoda jest również opisany przez Bator Kirk do wpisu w blogu [Indeksowania DocumentDB za pomocą wyszukiwania Azure](https://blogs.msdn.microsoft.com/kaevans/2015/03/09/indexing-documentdb-with-azure-seach/), która zawiera techniką o nazwie "spłaszczanie danych", zgodnie z którą chcesz mieć pole o nazwie `locationsID` i `locationsDescription` , które są zarówno [zbiorów](https://msdn.microsoft.com/library/azure/dn798938.aspx) (lub tablica ciągów).   

## <a name="part-1-flatten-the-array-into-individual-fields"></a>Część 1: Spłaszcz tablicy na poszczególne pola

Aby utworzyć indeks wyszukiwania Azure uwzględniający tego zestawu danych, utworzyć poszczególnych pól dla zagnieżdżonych konstrukcja: `locationsID` i `locationsDescription` o typie danych [kolekcji](https://msdn.microsoft.com/library/azure/dn798938.aspx) (lub tablica ciągów). W tych polach czy indeksowanie wartości "1" i "2" do `locationsID` pól Tomasz Bator i wartości "3" i "4" do `locationsID` dla Mac Campbell pole.  

Dane w Azure wyszukiwania będzie miała następującą postać: 

![Przykładowe dane, 2 wiersze](./media/search-howto-complex-data-types/sample-data.png)


## <a name="part-2-add-a-collection-field-in-the-index-definition"></a>Część 2: Dodawanie pola zbioru w definicji indeksu

W schemacie indeks definicje pól może wyglądać podobnie do w tym przykładzie.

~~~~
var index = new Index()
{
    Name = indexName,
    Fields = new[]
    {
        new Field("id", DataType.String) { IsKey = true },
        new Field("name", DataType.String) { IsSearchable = true, IsFilterable = false, IsSortable = false, IsFacetable = false },
        new Field("company", DataType.String) { IsSearchable = true, IsFilterable = false, IsSortable = false, IsFacetable = false },
        new Field("locationsId", DataType.Collection(DataType.String)) { IsSearchable = true, IsFilterable = true, IsFacetable = true },
        new Field("locationsDescription", DataType.Collection(DataType.String)) { IsSearchable = true, IsFilterable = true, IsFacetable = true }
    }
};
~~~~

## <a name="validate-search-behaviors-and-optionally-extend-the-index"></a>Sprawdź poprawność zachowania wyszukiwania i opcjonalnie rozszerzenie indeksu

Przy założeniu utworzony indeks i załadować dane, możesz teraz przetestować rozwiązanie do weryfikacji wykonywania kwerend wyszukiwania przed zestawu danych. Każde pole **zbioru** powinny być **wyszukiwania**, **podatne** i **facetable**. Należy możliwość wykonywania kwerend, takich jak:

* Znajdowanie wszystkich osób, które pracują w "Adventureworks Headquarters".
* Zliczanie liczby osób pracujących w biurze Narzędzia główne.  
* Osób, które pracują w Office Narzędzia główne Pokaż jakie inne biura działają wraz z liczbą osób, w każdym miejscu.  

Miejsce, w którym przypada tej techniki od siebie jest, gdy należy wykonać wyszukiwania, która łączy funkcje zarówno identyfikator lokalizacji, jak i opis lokalizacji. Na przykład:

* Znajdź wszystkie osoby, której mają domu i identyfikatorze 4 w lokalizacji.  

Jeśli odwołać treści oryginalnej elementu w następujący sposób:

~~~~
   {
        id: '4',
        description: 'Home Office'
   }
~~~~

Teraz, gdy mamy zostały oddzielone dane w oddzielnych polach, firma Microsoft jednak żadnym wiedząc, że jeśli strona główna pakietu Office dla Mac Campbell odnosi się do `locationsID 3` lub `locationsID 4`.  

Obsługę tej sprawy, należy zdefiniować pole innego w indeksie, który łączy wszystkie dane w pojedynczą kolekcję.  W naszym przykładzie zostanie nazywamy to pole `locationsCombined` i będzie oddzielić zawartości za pomocą `||` mimo że można wybrać dowolny separator sądzisz będzie unikatowy zestaw znaków dla zawartości. Na przykład: 

![Przykładowe dane, 2 wiersze z separatora](./media/search-howto-complex-data-types/sample-data-2.png)

Dzięki `locationsCombined` pola, firma Microsoft może teraz pomieścić jeszcze więcej kwerend, takich jak:

* Wyświetlana liczba osób pracujących w temacie "Strona główna pakietu Office" z lokalizacją identyfikator "4".  
* Wyszukaj osoby, które pracują w biurze główne z lokalizacją identyfikator "4". 

## <a name="limitations"></a>Ograniczenia

Ta metoda jest przydatna dla liczby scenariusze, ale nie ma zastosowania w każdym przypadku.  Na przykład:

1. Jeśli nie masz statyczne zestawu pól Typ złożonych danych i nie można mapować wszystkich możliwych typów do jednego pola. 
2. Aktualizowanie obiektów zagnieżdżonych wymaga niektórych dodatkowej pracy, aby określić dokładnie co trzeba będzie aktualizowany w indeksie wyszukiwania Azure

## <a name="sample-code"></a>Przykładowy kod

Przykład można wyświetlić na jak indeks złożony zestaw danych JSON do wyszukiwania Azure i wykonywać tego zestawu danych w tym [GitHub repo](https://github.com/liamca/AzureSearchComplexTypes)liczba kwerend.

## <a name="next-step"></a>Następny krok

[Wbudowana obsługa złożonymi typami danych w głosowaniu](https://feedback.azure.com/forums/263029-azure-search) w możliwości wyszukiwania Azure strony i podanie dodatkowych danych wejściowych, z której chcesz, abyśmy należy rozważyć, czy odnoszących się do wykonania funkcji. Możesz można również docieranie do mnie bezpośrednio w serwisie Twitter u @liamca.


 