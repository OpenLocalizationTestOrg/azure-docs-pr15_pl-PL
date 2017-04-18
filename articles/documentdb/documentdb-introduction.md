<properties 
    pageTitle="Wprowadzenie do DocumentDB, JSON bazy danych | Microsoft Azure" 
    description="Informacje na temat DocumentDB Azure, NoSQL JSON bazy danych. Ta baza danych dokument jest przeznaczony dla danych duży, elastyczne skalowalność i wysoką dostępność." 
    keywords="Baza danych JSON, dokument bazy danych"
    services="documentdb" 
    authors="mimig1" 
    manager="jhubbard" 
    editor="monicar" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="09/13/2016" 
    ms.author="mimig"/>

# <a name="introduction-to-documentdb-a-nosql-json-database"></a>Wprowadzenie do DocumentDB: NoSQL JSON bazy danych

##<a name="what-is-documentdb"></a>Co to jest DocumentDB?

DocumentDB jest w pełni zarządzane usługą bazy danych NoSQL utworzoną dla szybką i przewidywalne wydajności, wysokiej dostępności elastyczne skalowania, rozkład globalnej i łatwiejsze projektowanie. Jako wolny schematu NoSQL bazy danych DocumentDB zawiera zaawansowanych, znanych możliwość wykonywania kwerend SQL z spójne opóźnienia niska na JSON danych — zapewnienie, że 99% do odczytu są obsługiwane w obszarze 10 milisekund i 99% do zapisu są obsługiwane w obszarze 15 milisekund. Te tworzącej unikatowych zalet DocumentDB doskonały dopasowania dla sieci web urządzeń przenośnych, gier, a IoT i wielu innych aplikacji wymagających Bezproblemowa skali i globalnej replikacji.

## <a name="how-can-i-learn-about-documentdb"></a>Gdzie można znaleźć informacje o DocumentDB? 

Szybkie informacje o DocumentDB i wyświetlić go w działaniu polega na wykonaniu tych trzech kroków: 

1. Obejrzyj dwóch minutę [Co to jest DocumentDB?](https://azure.microsoft.com/documentation/videos/what-is-azure-documentdb/) wideo, które wprowadza korzyści wynikające z używania DocumentDB.
2. Obejrzyj trzy minuty [Tworzenie DocumentDB Azure](https://azure.microsoft.com/documentation/videos/create-documentdb-on-azure/) wideo, które przedstawia, jak rozpocząć pracę z DocumentDB przy użyciu Azure Portal.
3. Odwiedź stronę [Playground kwerendy](http://www.documentdb.com/sql/demo), gdzie szczegółową różnych czynności, aby uzyskać informacje o zaawansowanych funkcji podczas badania dostępnych w DocumentDB. Następnie głowy na kartę piaskownicy i uruchamianie własnych niestandardowych kwerend SQL i poeksperymentować z DocumentDB.

Następnie wróć do niniejszego artykułu, miejsce, w którym firma Microsoft będzie wyświetlić elementy.  

## <a name="what-capabilities-and-key-features-does-documentdb-offer"></a>Jakie możliwości i kluczowe funkcje DocumentDB oferuje system?  

Azure DocumentDB oferuje następujące kluczowe funkcje i zalety:

-   **Elastically skalowalna przepustowość i miejsca do magazynowania:** Łatwe rozbudowy lub skali DocumentDB JSON bazy danych stosownie do potrzeb aplikacji. Dane są przechowywane na dyskach stanie stałym (SSD) dla niskiej opóźnienia przewidywalne. DocumentDB obsługuje kontenerów do przechowywania danych JSON o nazwie kolekcje, które można skalować rozmiarów nieograniczoną miejsca do magazynowania i przepustowość ustanawianie. Można elastically skalować DocumentDB z wydajnością przewidywalne bezproblemowo w miarę aplikacji. 

-   **Replikacji wielu region:** DocumentDB przezroczysty replikuje dane na wszystkie regiony, które jest skojarzony z kontem usługi DocumentDB, umożliwiając tworzenie aplikacji, które wymagają globalnego dostęp do danych, zapewniając kompromisów między spójności, dostępności i wydajności, wszystkie odpowiednie gwarancje. DocumentDB zawiera przezroczyste awaryjnego regionalne z wielu interfejsów API i możliwość skalowania elastically przepustowość i miejsca do magazynowania na całym świecie. Dowiedz się więcej o [rozpowszechnianie danych globalnie za pomocą DocumentDB](documentdb-distribute-data-globally.md).

-   **Ad hoc kwerend za pomocą znanych składni języka SQL:** Przechowywanie dokumentów niejednorodnymi JSON w obrębie DocumentDB i tych dokumentów za pomocą znanych składnia SQL kwerendy. DocumentDB wykorzystuje blokada wysoce równoczesne bezpłatne, dziennika strukturę technologii indeksowania automatycznie indeksowania całej zawartości dokumentu. Dzięki temu zaawansowanych kwerend w czasie rzeczywistym bez konieczności Określ schemat wskazówki, indeksów pomocniczych lub widoków. Dowiedz się, jak w [DocumentDB kwerendy](documentdb-sql-query.md). 

-   **Wykonywanie kodu JavaScript w bazie danych:** Pokazywanie logiki aplikacji jako procedur składowanych, wyzwalaczami i funkcje zdefiniowane przez użytkownika (UDF) za pomocą standardowej języka JavaScript. Dzięki temu logiki aplikacji do pracy nad danymi, nie martwiąc się o niezgodności między aplikacją i schemat bazy danych. DocumentDB zawiera pełnego transakcji wykonywanie kodu JavaScript logiki aplikacji bezpośrednio wewnątrz aparat bazy danych. Ścisła integracja języka JavaScript umożliwia wykonanie Wstawianie, ZAMIEŃ, Usuń i wybierz operacji z poziomu programu JavaScript jako odizolowanych. Dowiedz się, jak w [programowaniu po stronie serwera DocumentDB](documentdb-programming.md).

-   **Poziomy ustawiane spójności:** Wybierz z czterech poziomów dobrze zdefiniowanych spójności uzyskanie optymalnej zależność między wydajność i spójność. Dla kwerend i operacji odczytu DocumentDB dostępne są cztery poziomy odrębnych spójności: znaczący, ograniczone staleness, sesji i ewentualne. Te poziomy spójności szczegółowego, przejrzyste umożliwiają dźwięku korzystnych rozwiązań między spójności, dostępność i opóźnienie. Dowiedz się, jak za [pomocą poziomów spójności maksymalizować dostępności i wydajności w DocumentDB](documentdb-consistency-levels.md).

-   **Pełna:** Uniknąć zarządzanie zasobami bazy danych i komputera. Jako usługi Microsoft Azure w pełni zarządzane nie trzeba zarządzać maszyn wirtualnych, wdrażania i skonfigurować oprogramowanie, skalowania lub zajęcie się uaktualnień warstwy złożonych danych. Co baza danych jest automatycznie kopii zapasowej i chronione przed regionalnych błędy. Można łatwo dodawać konta DocumentDB i obsługi administracyjnej możliwości potrzebne, co umożliwia skoncentrowanie się na aplikacji zamiast operacyjnym i zarządzania bazą danych. 

-   **Otwórz zamierzone:** Szybko rozpocząć przy użyciu posiadanych umiejętności i narzędzi. Programowanie przed DocumentDB jest proste, przystępny, a nie wymaga o przyjęciu nowych narzędzi lub zgodny rozszerzenia niestandardowe JSON lub kodu JavaScript. Możesz uzyskać dostęp do wszystkich funkcji bazy danych, takich jak OBSŁUGIWAŁ, kwerendy i JavaScript przetwarzania przez prosty interfejs RESTful HTTP. DocumentDB obejmuje istniejące formaty, języków i standardy zapewniając przy tym wysokiej wartości możliwości bazy danych na bieżąco je.

-   **Automatyczne indeksowanie:** Domyślnie DocumentDB [zostaną automatycznie zindeksowane](documentdb-indexing.md) wszystkie dokumenty w bazie danych i nie spodziewać lub wymaga schematu ani tworzenia wskaźników pomocniczą. Nie chcesz, aby wszystkie elementy indeksu? Nie martw się, możesz [zaprzestać korzystania z ścieżek w plikach JSON](documentdb-indexing-policies.md) zbyt.

##<a name="data-management"></a>Jak DocumentDB zarządzać danych?

Azure DocumentDB zarządza JSON danych przy użyciu zasobów przejrzyste bazy danych. Te zasoby są replikowane wysokiej dostępności i są jednoznacznie adresach przez ich logiczne identyfikatora URI. DocumentDB oferuje prosty HTTP podstawie RESTful model programowania dla wszystkich zasobów. 

Konto bazy danych DocumentDB jest unikatowych nazw, który umożliwia dostęp do Azure DocumentDB. Przed utworzeniem konta bazy danych, musisz mieć subskrypcję usługi Azure, który umożliwia dostęp do różnych usług Azure. 

Wszystkie zasoby w DocumentDB są modelowania i przechowywane jako dokumenty JSON. Zasoby są zarządzane jako elementów, które są JSON dokumenty zawierające metadanych, a jako źródła, które są kolekcje elementów. Zestawów elementów są zawarte w odpowiednich źródeł danych.

Na poniższej ilustracji pokazano relacje między zasoby DocumentDB:

![Hierarchicznych relacji między zasobów w DocumentDB, NoSQL JSON bazy danych][1] 

Konto bazy danych zawiera zestaw baz danych, każdy zawiera wiele kolekcji, z których każdy może zawierać procedur składowanych wyzwalaczy, funkcji zdefiniowanych przez użytkownika oraz dokumenty pokrewne załączników. Również bazy danych jest skojarzony z zestawem uprawnień dostępu do różnych innych zbiorów, procedur składowanych, wyzwalaczy, funkcji zdefiniowanych przez użytkownika, dokumenty lub załączniki użytkowników. Gdy baz danych, użytkowników, uprawnień i kolekcje są zdefiniowane przez system zasobów za pomocą znanych Schematy — dokumentów, procedur składowanych wyzwalaczy, funkcji zdefiniowanych przez użytkownika i załączniki zawierają dowolnego, zdefiniowane przez użytkownika zawartości JSON.  

##<a name="develop"></a>Jak można opracowywać aplikacje z DocumentDB?

Azure DocumentDB udostępnia zasoby za pośrednictwem interfejsu API usługi REST, który może być wywoływana przez dowolny język umożliwiający żądania HTTP/HTTPS. Ponadto DocumentDB oferuje programowania biblioteki do kilku popularnych językach. Te biblioteki uprościć wielu aspektów pracy z Azure DocumentDB poprzez obsługę szczegóły, takie jak adres pamięci podręcznej, Zarządzanie wyjątkami, automatyczne ponowne próby i tak dalej. Biblioteki są obecnie dostępne dla następujących języków i platformy:  

Plik do pobrania | Dokumentacja
--- | ---
[ZESTAW SDK PROGRAMU .NET](http://go.microsoft.com/fwlink/?LinkID=402989) | [Biblioteka .NET](https://msdn.microsoft.com/library/azure/dn948556.aspx)
[Zestaw SDK node.js](http://go.microsoft.com/fwlink/?LinkID=402990) | [Biblioteka node.js](http://azure.github.io/azure-documentdb-node/)
[Java SDK](http://go.microsoft.com/fwlink/?LinkID=402380) | [Biblioteka języka Java](http://azure.github.io/azure-documentdb-java/)
[Zestaw SDK języka JavaScript](http://go.microsoft.com/fwlink/?LinkID=402991) | [Biblioteka kodu JavaScript](http://azure.github.io/azure-documentdb-js/)
n/d! | [SDK JavaScript po stronie serwera](http://azure.github.io/azure-documentdb-js-server/)
[Python SDK](https://pypi.python.org/pypi/pydocumentdb) | [Biblioteka Python](http://azure.github.io/azure-documentdb-python/)

Poza podstawowe tworzenie, odczytywanie, aktualizowanie i usuwanie operacji, DocumentDB udostępnia interfejs sformatowanego zapytania SQL do pobierania dokumentów JSON i obsługę po stronie serwera transakcji wykonywanie kodu JavaScript logiki aplikacji. Interfejsów wykonywania kwerend i skrypt są dostępne wszystkich bibliotek platform, a także za pośrednictwem interfejsów API pozostałych. 

### <a name="sql-query"></a>Kwerenda SQL
Kwerenda dokumentów przy użyciu języka SQL, osadzonego w system typów JavaScript i wyrażenia z obsługą kwerend relacyjnych, hierarchicznych i przestrzenna Azure obsługuje DocumentDB. Język kwerend DocumentDB to interfejs prostego i użytecznego dokumentom JSON kwerendy. Język obsługuje języka SQL ANSI gramatyki i dodaje ścisła integracja obiektu, tablice, budowy obiektu i wywołania funkcji JavaScript. DocumentDB zawiera jego model kwerendy bez dowolny schematów jawne lub wskazówki indeksowania przez dewelopera.

Funkcje zdefiniowane przez użytkownika (UDF) można zarejestrowane DocumentDB i odwołanie do jako część zapytania SQL, w związku z tym rozszerzanie gramatyki w programach do obsługi logiki niestandardowej aplikacji. Tych funkcji zdefiniowanych przez użytkownika są zapisywane jako programy JavaScript i uruchamiane w bazie danych. 

Dla deweloperów .NET DocumentDB oferuje dostawcę LINQ kwerendy jako część [Zestawu SDK usługi .NET](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.aspx). 

### <a name="transactions-and-javascript-execution"></a>Transakcje i wykonywanie kodu JavaScript
DocumentDB umożliwia pisanie logiki aplikacji jako nazwany programów napisanych w całości JavaScript. Programy te są rejestrowane w zbiorze i może wydawać operacji bazy danych w dokumentach w danym zbiorze. JavaScript można rejestrować wykonywania jako wyzwalacza, procedura składowana lub funkcja zdefiniowana przez użytkownika. Wyzwalaczami i procedur składowanych można tworzenie, odczytywanie, aktualizowanie i usuwanie dokumentów, należy wykonać funkcje zdefiniowane przez użytkownika jako część logiki wykonanie kwerendy bez uprawnienia do zapisu w tym zbiorze.

Wykonywanie kodu JavaScript w DocumentDB w modelu po pojęcia obsługiwanych przez systemach relacyjnych baz danych przy użyciu języka JavaScript jako nowoczesne zamiennik języku Transact-SQL. Wszystkie logiczny JavaScript jest wykonywane w otoczeniu transakcja kwasu izolacji migawki. W trakcie jego realizacji Jeśli kod JavaScript generuje wyjątek, następnie całej transakcji zostało przerwane.

## <a name="next-steps"></a>Następne kroki
Masz już konto Azure? Następnie możesz mogą rozpocząć pracę z DocumentDB w [Azure Portal](https://portal.azure.com/#gallery/Microsoft.DocumentDB) , [tworząc DocumentDB konta bazy danych](documentdb-create-account.md).

Nie masz konta usługi Azure? Można:

- Załóż konto [Azure bezpłatnej wersji próbnej](https://azure.microsoft.com/free/), co pozwala na 30 dni i 200 zł do wypróbowania wszystkich usług Azure. 
- Jeśli masz subskrypcję MSDN, masz prawo do [150 zł w bezpłatnej środków Azure miesięcznie](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) używać na wszystkie inne usługi Azure. 

Następnie gdy będzie gotowe dowiedzieć się więcej, odwiedź stronę naszych [ścieżka nauki](https://azure.microsoft.com/documentation/learning-paths/documentdb/) Przejdź wszystkie zasoby szkoleniowe dla Ciebie dostępne. 


[1]: ./media/documentdb-introduction/json-database-resources1.png
 
