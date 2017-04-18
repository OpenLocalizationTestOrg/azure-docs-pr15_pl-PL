<properties 
    pageTitle="Interfejs API .NET DocumentDB & SDK | Microsoft Azure" 
    description="Wszystkie informacje o interfejsie .NET API i zestawu SDK wraz z datami wersji, emerytury i zmiany wprowadzone między poszczególnymi wersjami DocumentDB .NET SDK." 
    services="documentdb" 
    documentationCenter=".net" 
    authors="rnagpal" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="10/27/2016" 
    ms.author="rnagpal"/>

# <a name="documentdb-apis-and-sdks"></a>Interfejsy API DocumentDB i SDK 

> [AZURE.SELECTOR]
- [.NET](documentdb-sdk-dotnet.md)
- [Node.js](documentdb-sdk-node.md)
- [Java](documentdb-sdk-java.md)
- [Python](documentdb-sdk-python.md)
- [POZOSTAŁE](https://go.microsoft.com/fwlink/?LinkId=402413)
- [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)

## <a name="documentdb-net-api-and-sdk"></a>Interfejs API .NET DocumentDB i zestaw SDK

<table>
<tr><td>**Pobierz zestaw SDK**</td><td>[NuGet](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/)</td></tr>
<tr><td>**Interfejs API dokumentacji**</td><td>[Interfejs API .NET dokumentacji](https://msdn.microsoft.com/library/azure/dn948556.aspx)</td></tr>
<tr><td>**Przykłady**</td><td>[Przykłady kodu .NET](documentdb-dotnet-samples.md)</td></tr>
<tr><td>**Rozpoczynanie pracy**</td><td>[Wprowadzenie do programu DocumentDB .NET SDK](documentdb-get-started.md)</td></tr>
<tr><td>**Samouczek aplikacji sieci Web**</td><td>[Programowania aplikacji sieci Web z DocumentDB](documentdb-dotnet-application.md)</td></tr>
<tr><td>**Bieżący framework obsługiwane**</td><td>[Program Microsoft .NET Framework 4,5](https://www.microsoft.com/download/details.aspx?id=30653)</td></tr>
</table></br>

## <a name="release-notes"></a>Informacje o wersji

> [AZURE.IMPORTANT] Począwszy od wersji 1.9.2 wydania, może pojawić się System.NotSupportedException podczas badania zbiory podzielone na partycje. Aby uniknąć tego błędu, upewnij się, że procesu hosta jest 64-bitowa. W przypadku projektów wykonywalny można to zrobić przez anulowanie zaznaczenia opcji "Preferuj 32-bitowej" w oknie właściwości projektu na karcie Tworzenie.

### <a name="a-name11001100httpswwwnugetorgpackagesmicrosoftazuredocumentdb1100"></a><a name="1.10.0"/>[1.10.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.10.0)

  - Obsługa dodano bezpośredniej łączności zbiory podzielone na partycje.
  - Zwiększona wydajność dla poziomu spójności Staleness ograniczony.
  - Dodano Wielokąt i typy obiektów LineString danych podczas zbioru indeksowania zasad ogrodzenia geo przestrzenna kwerend.
  - Dodano LINQ obsługę StringEnumConverter, IsoDateTimeConverter i UnixDateTimeConverter podczas tłumaczenia orzeczenia.
  - Różne SDK poprawki.

### <a name="a-name195195httpswwwnugetorgpackagesmicrosoftazuredocumentdb195"></a><a name="1.9.5"/>[1.9.5](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.9.5)

  - Rozwiązanie problemu związanego powodujący następujące NotFoundException: sesja odczytu nie jest dostępna dla tokenu sesji wprowadzania danych. W niektórych przypadkach wystąpił ten wyjątek, podczas badania obszaru czytania geo distributed konta.
  - Dostępne właściwości ResponseStream w klasie ResourceResponse umożliwia bezpośredni dostęp do odpowiedniego strumienia z odpowiedzią.

### <a name="a-name194194httpswwwnugetorgpackagesmicrosoftazuredocumentdb194"></a><a name="1.9.4"/>[pytanie 1.9.4](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.9.4)

  - Zmodyfikowane klasy ResourceResponse, FeedResponse, StoredProcedureResponse i MediaResponse do wykonania odpowiednich interfejsu publicznego, dlatego może być mocked do badania zmiennych wdrożenia (TDD).
  - Rozwiązanie problemu związanego powodujący nagłówka klucza partition utworzonym podczas korzystania z niestandardowym obiektem JsonSerializerSettings szeregowania danych.

### <a name="a-name193193httpswwwnugetorgpackagesmicrosoftazuredocumentdb193"></a><a name="1.9.3"/>[1.9.3](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.9.3)

  - Rozwiązanie problemu związanego powodujący długotrwałych kwerend kończy się niepowodzeniem z powodu błędu: token autoryzacji nie jest prawidłowa w danym momencie.
  - Rozwiązanie problemu związanego usunąć oryginalny SqlParameterCollection z krzyżowe partition góry według kwerendy.

### <a name="a-name192192httpswwwnugetorgpackagesmicrosoftazuredocumentdb192"></a><a name="1.9.2"/>[1.9.2](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.9.2)

  - Dodano obsługę zapytania równolegle dla zbiorów podzielone na partycje.
  - Dodano obsługę się między partycją ORDER BY i od góry kwerendy dla zbiorów podzielone na partycje.
  - Stałe brakujące odwołania do DocumentDB.Spatial.Sql.dll i Microsoft.Azure.Documents.ServiceInterop.dll, które są wymagane przy odwoływaniu się do projektu DocumentDB z odwołaniem do pakietu DocumentDB Nuget.
  - Stałe możliwość używania parametrów różnych typów podczas korzystania z funkcji zdefiniowanych przez użytkownika w programie LINQ. 
  - Stałe błędów w przypadku kont globalnie zreplikowanej, gdzie Upsert połączenia były są kierowane do odczytu lokalizacji zamiast lokalizacjach zapisu.
  - Dodano metody interfejsu IDocumentClient, które były widoczne: 
      - Metoda UpsertAttachmentAsync, która przyjmuje mediaStream i opcje jako parametrów
      - Metoda CreateAttachmentAsync, która przyjmuje opcje jako parametru
      - Metoda CreateOfferQuery, która przyjmuje querySpec jako parametru.
  - Dotyczącego niezapieczętowanych klasy publiczne, które są dostępne w interfejsie IDocumentClient.

### <a name="a-name180180httpswwwnugetorgpackagesmicrosoftazuredocumentdb180"></a><a name="1.8.0"/>[1.8.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.8.0)
  - Dodano obsługę dla bazy danych w przypadku kont.
  - Dodano obsługę ponów próbę na ograniczonej żądania.  Użytkownik może dostosować liczbę prób i czas oczekiwania max przez skonfigurowanie właściwości ConnectionPolicy.RetryOptions.
  - Dodany nowy interfejs IDocumentClient definiujące podpisów wszystkie właściwości DocumenClient i metod.  W ramach tej zmiany również zmienić metody rozszerzenia, tworzące IQueryable i IOrderedQueryable metod na samej klasy DocumentClient.
  - Dodać opcję konfiguracji, aby ustawić ServicePoint.ConnectionLimit dla danego punktu końcowego DocumentDB identyfikatora Uri.  Aby zmienić wartość domyślną, którą jest ustawiona na 50 za pomocą ConnectionPolicy.MaxConnectionLimit.
  - Przestarzałe IPartitionResolver i jego wykonania.  Obsługa IPartitionResolver jest teraz przestarzały. Zaleca się używanie partycją zbiory wyższą przechowywania i przepustowość.


### <a name="a-name171171httpswwwnugetorgpackagesmicrosoftazuredocumentdb171"></a><a name="1.7.1"/>[1.7.1](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.7.1)
  - Dodane podstawą przeciążeń identyfikator URI metody ExecuteStoredProcedureAsync, która przyjmuje RequestOptions jako parametru.
  
### <a name="a-name170170httpswwwnugetorgpackagesmicrosoftazuredocumentdb170"></a><a name="1.7.0"/>[1.7.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.7.0)
  - Dodano czas live (TTL) — Pomoc techniczna dla dokumentów.

### <a name="a-name163163httpswwwnugetorgpackagesmicrosoftazuredocumentdb163"></a><a name="1.6.3"/>[1.6.3](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.6.3)
  - Ustawiony błędu w opakowaniu Nuget .NET SDK do pakowania go jako części usługa w chmurze Azure.
  
### <a name="a-name162162httpswwwnugetorgpackagesmicrosoftazuredocumentdb162"></a><a name="1.6.2"/>[1.6.2](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.6.2)
  - Wdrożony [zbiory podzielone na partycje](documentdb-partition-data.md) i [poziomów wydajności zdefiniowane przez użytkownika](documentdb-performance-levels.md). 

### <a name="a-name153153httpswwwnugetorgpackagesmicrosoftazuredocumentdb153"></a><a name="1.5.3"/>[1.5.3](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.5.3)
  - **[Stała]** Kwerenda DocumentDB wystąpił punkt końcowy: "System.Net.Http.HttpRequestException: błąd podczas kopiowania zawartości ze strumieniem.

### <a name="a-name152152httpswwwnugetorgpackagesmicrosoftazuredocumentdb152"></a><a name="1.5.2"/>[1.5.2](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.5.2)
  - Rozwinięty LINQ obsługuje, łącznie z nowych operatorów wyrażeń stronicowania, warunkowe i zakresu porównania.
    - Sporządzanie operator umożliwiające zachowanie wybierz góry LINQ
    - Operator CompareTo, aby włączyć porównaniach ciągów znaków zakresu
    - Warunkowe (?), a połączenie operatory (?)
  - **[Stała]** ArgumentOutOfRangeException podczas łączenia rzut modelu z miejsca w programie linq kwerendy.  [#81](https://github.com/Azure/azure-documentdb-dotnet/issues/81)

### <a name="a-name151151httpswwwnugetorgpackagesmicrosoftazuredocumentdb151"></a><a name="1.5.1"/>[: 1.5.1](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.5.1)
 - **[Stała]** Jeśli wybierz nie jest ostatnim wyrażenie dostawcy LINQ zakłada, że nie rzut i wyprodukowano wybierz * niepoprawnie.  [#58](https://github.com/Azure/azure-documentdb-dotnet/issues/58)

### <a name="a-name150150httpswwwnugetorgpackagesmicrosoftazuredocumentdb150"></a><a name="1.5.0"/>[1.5.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.5.0)
 - Wdrożony Upsert, metody dodane UpsertXXXAsync
 - Ulepszenia dotyczące wydajności dla wszystkich żądań
 - Dostawca LINQ obsługę warunkowe, połączenie i metody CompareTo ciągów
 - **[Stała]** Dostawca LINQ--> Implementowanie zawiera metody na liście, aby wygenerować na IEnumerable i Tablica samej SQL jako
 - **[Stała]** BackoffRetryUtility używa samej HttpRequestMessage ponownie zamiast tworzenia nowej witryny ponów
 - **[Przestarzałych]** --> UriFactory.CreateCollection należy użyć UriFactory.CreateDocumentCollection
 
### <a name="a-name141141httpswwwnugetorgpackagesmicrosoftazuredocumentdb141"></a><a name="1.4.1"/>[1.4.1](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.4.1)
 - **[Stała]** Problemy związane z lokalizacją podczas korzystania z innych niż en kultury informacje takie jak nl-NL itp. 
 
### <a name="a-name140140httpswwwnugetorgpackagesmicrosoftazuredocumentdb140"></a><a name="1.4.0"/>[1.4.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.4.0)
  - Identyfikator podstawie routingu
    - Nowy UriFactory Pomocnik pomagające identyfikator tworzenia wspólnej podstawie łącza zasobów
    - Nowe overloads na DocumentClient podjęcie w polu Identyfikator URI
  - Dodano IsValid() i IsValidDetailed() w programie LINQ dla geograficznych
  - Obsługa dostawcy LINQ rozszerzony
    - **Obliczenia** — Asin moduł.liczby, Acos, Atan, ZAOKR.w.górę.matematyczne(liczba;[istotność];[tryb]) Cos Exp Floor, dziennika, Log10, Pow, ZAOKR, logowania, Sin, pierwiastek, Tan, należy obciąć
    - **Ciąg** – "concat", zawiera EndsWith IndexOf, liczba, ToLower, TrimStart, Zamień, odwrócić, TrimEnd, StartsWith, podciąg, ToUpper
    - **Tablica** - "concat", zawiera, liczba
    - Operator **w**

### <a name="a-name130130httpswwwnugetorgpackagesmicrosoftazuredocumentdb130"></a><a name="1.3.0"/>[1.3.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.3.0)
  - Dodano obsługę modyfikowanie zasad indeksowania
    - Nowa metoda ReplaceDocumentCollectionAsync w DocumentClient
    - Nowa właściwość IndexTransformationProgress w ResourceResponse<T> do śledzenia postępu procentu zmian zasad indeksu
    - DocumentCollection.IndexingPolicy jest teraz tych
  - Dodano obsługę przestrzenna indeksowania i kwerendy
    - Nowe nazw Microsoft.Azure.Documents.Spatial szeregowania i deserializacji przestrzenna typów, takie jak punkt i Wielokąt
    - Nową klasę SpatialIndex dla indeksowania GeoJSON dane przechowywane w DocumentDB
  - **[Stała]** : zapytania SQL niepoprawne wynikiem wyrażenia linq [#38](https://github.com/Azure/azure-documentdb-net/issues/38)

### <a name="a-name120120httpswwwnugetorgpackagesmicrosoftazuredocumentdb120"></a><a name="1.2.0"/>[1.2.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.2.0)
- Zależność Newtonsoft.Json v5.0.7 
- Zmiany do obsługi Order By
  - Obsługa dostawcy LINQ OrderBy() lub OrderByDescending()
  - IndexingPolicy do obsługi Order By 
  
        **NB: Possible breaking change** 
  
        If you have existing code that provisions collections with a custom indexing policy, then your existing code will need to be updated to support the new IndexingPolicy class. If you have no custom indexing policy, then this change does not affect you.

### <a name="a-name110110httpswwwnugetorgpackagesmicrosoftazuredocumentdb110"></a><a name="1.1.0"/>[1.1.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.1.0)
- Obsługa podziału danych za pomocą nowych klas HashPartitionResolver i RangePartitionResolver i IPartitionResolver
- Szeregowania DataContract
- Identyfikator GUID pomocy technicznej przez dostawcę LINQ
- Obsługa UDF w LINQ

### <a name="a-name100100httpswwwnugetorgpackagesmicrosoftazuredocumentdb100"></a><a name="1.0.0"/>[1.0.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.0.0)
- ZESTAW SDK GA

> [AZURE.NOTE]
Wystąpił zmianę nazwy pakietu NuGet między Podgląd i GA. Firma Microsoft przeniesiony z **Microsoft.Azure.Documents.Client** **Microsoft.Azure.DocumentDB**
<br/>


### <a name="a-name09x-preview09x-previewhttpswwwnugetorgpackagesmicrosoftazuredocumentsclient"></a><a name="0.9.x-preview"/>[0.9.x-Preview](https://www.nuget.org/packages/Microsoft.Azure.Documents.Client)
- Podgląd SDK [przestarzałego]

## <a name="release--retirement-dates"></a>Wersji i wycofywanie dat
Firma Microsoft udostępni powiadomienie co najmniej **12 miesięcy** przed emeryturę SDK było gładkie przejścia do nowszego nieobsługiwaną wersję.

Nowe funkcje i funkcje i optymalizacji tylko są dodawane do bieżącego zestawu SDK, co jest zalecane, zawsze uaktualnienie do najnowszej SDK wersji najwcześniej, jak to możliwe. 

Wszelkie żądanie DocumentDB przy użyciu wycofanych zestawu SDK zostaną odrzucone przez usługę.

> [AZURE.WARNING]
Wszystkie wersje SDK DocumentDB Azure dla środowiska .NET przed wersji **1.0.0** zostanie wycofana na **29 lutego 2016**. 
 
<br/>
 
| Wersja | Data wydania | Wycofywanie daty 
| ---     | ---          | ---
| [1.10.0](#1.10.0) | 27 września 2016 |---
| [1.9.5](#1.9.5) | 01 września 2016 |---
| [pytanie 1.9.4](#1.9.4) | 24 sierpnia 2016 |---
| [1.9.3](#1.9.3) | 15 sierpnia 2016 |---
| [1.9.2](#1.9.2) | 23 lipca 2016 |---
| 1.9.1 | Przestarzałe |---
| 1.9.0 | Przestarzałe |---
| [1.8.0](#1.8.0) | 14 czerwca 2016 |---
| [1.7.1](#1.7.1) | 06 maja 2016 |---
| [1.7.0](#1.7.0) | 26 kwietnia 2016 |---
| [1.6.3](#1.6.3) | 08 kwietnia 2016 |---
| [1.6.2](#1.6.2) | 29 marca 2016 |---
| [1.5.3](#1.5.3) | 19 lutego 2016 |---
| [1.5.2](#1.5.2) | 14 grudnia 2015 r. |---
| [: 1.5.1](#1.5.1) | 23 listopada 2015 r. |---
| [1.5.0](#1.5.0) | 05 października 2015 r. |---
| [1.4.1](#1.4.1) | 25 sierpnia 2015 r. |---
| [1.4.0](#1.4.0) | 13 sierpnia 2015 r. |---
| [1.3.0](#1.3.0) | 05 sierpnia 2015 r. |---
| [1.2.0](#1.2.0) | 06 lipca 2015 r. |---
| [1.1.0](#1.1.0) | 30 kwietnia 2015 r. |---
| [1.0.0](#1.0.0) | 08 kwietnia 2015 r. |---
| [0.9.3-prelease](#0.9.x-preview) | 12 marca 2015 r. | 29 lutego 2016
| [0.9.2-prelease](#0.9.x-preview) | Stycznia 2015 r. | 29 lutego 2016
| [.9.1-prelease](#0.9.x-preview) | 13 października 2014 r. | 29 lutego 2016
| [0.9.0-prelease](#0.9.x-preview) | 21 sierpnia 2014 r. | 29 lutego 2016

## <a name="faq"></a>FAQ
[AZURE.INCLUDE [documentdb-sdk-faq](../../includes/documentdb-sdk-faq.md)]

## <a name="see-also"></a>Zobacz też

Aby dowiedzieć się więcej na temat DocumentDB, zobacz strony usługi [Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) . 
