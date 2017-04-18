<properties 
    pageTitle="Przewodnik Szybki start: maszynowego uczenia zalecenia API | Microsoft Azure" 
    description="Azure maszynowego uczenia zalecenia — Przewodnik Szybki Start" 
    services="machine-learning" 
    documentationCenter="" 
    authors="LuisCabrer" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/08/2016" 
    ms.author="luisca"/>

# <a name="quick-start-guide-for-the-machine-learning-recommendations-api"></a>Przewodnik Szybki start dla interfejsu API zalecenia nauki komputera

>[AZURE.NOTE]Należy zacząć korzystać z usługi poznawcze interfejsu API zalecenia zamiast tej wersji. Usługa poznawcze zalecenia będzie zamieniania tej usługi, a opracowane zostaną wszystkie nowe funkcje. Ma nowych funkcji, takich jak tworzeniu partii pomocy technicznej lepiej interfejsu API Eksploratora, oczyszczania środowisko tworzenia konta i rozliczenia powierzchniowy, bardziej spójny interfejs API, itp.
> Dowiedz się więcej na temat [migrowania do nowej usługi poznawcze](http://aka.ms/recomigrate)


W tym dokumencie opisano sposób do wewnętrznego usługi lub aplikację Microsoft Azure maszynowego uczenia zalecenia. Więcej informacji na temat interfejsu API zalecenia można znaleźć w [galerii](http://gallery.cortanaanalytics.com/MachineLearningAPI/Recommendations-2).

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

##<a name="general-overview"></a>Ogólne omówienie

Aby użyć Azure maszynowego uczenia zalecenia, należy wykonać następujące czynności:

* Tworzenie modelu — model jest kontenerem danych użycia, wykazu danych i rekomendacji modelu.
* Importowanie wykazu danych — wykaz zawiera metadane dla elementów. 
* Importowanie danych dotyczących użycia - danych dotyczących użycia można przekazywać sposoby 2 (lub oba):
    * Przekazując plik, który zawiera dane użycia.
    * Wysyłając zdarzenia nabycia danych.
    Zazwyczaj przekazać plik zastosowania, aby móc tworzyć model rekomendacji początkowej (uruchamiania) i go używać, dopóki system zbiera za mało dane przy użyciu formatu nabycia danych.
* Tworzenie modelu rekomendacji — jest to operacja asynchroniczna, w którym system rekomendacji pobiera wszystkie dane użycia i tworzy model rekomendacji. Operacja może potrwać kilka minut lub kilka godzin w zależności od rozmiaru danych i parametry konfiguracji kompilacji. Gdy powodujące kompilacji, zostanie wyświetlony identyfikatora kompilacji Go używać, aby sprawdzić, kiedy procesu tworzenia zostało zakończone przed rozpoczęciem korzystania zalecenia.
* Korzystanie ze zalecenia — Uzyskaj zalecenia dotyczące określonego elementu lub listę elementów.

Powyższe czynności są wykonywane za pomocą API zalecenia Azure maszynowego uczenia.  Możesz pobrać przykładowej aplikacji, które wykonuje każdy z tych kroków z [także Galeria.](http://1drv.ms/1xeO2F3)

##<a name="limitations"></a>Ograniczenia

* Maksymalna liczba modeli na subskrypcję wynosi 10.
* Maksymalna liczba elementów, które mogą być przechowywane w katalogu wynosi 100 000.
* Maksymalna liczba punktów zastosowania, które są przechowywane jest ~ 5,000,000. Najstarszego zostaną usunięte, jeśli nowe zostaną przekazane lub zgłoszone.
* Maksymalny rozmiar danych, które mogą być wysyłane wpisu (np. importowanie wykazu danych, importowanie danych dotyczących użycia) w wynosi 200MB.
* Liczba transakcji na sekundę dla kompilacji modelu rekomendacji, która nie jest aktywna jest ~ 2TPS. Rekomendacji kompilacji modelu, który jest aktywny mogą być przechowywane w górę 20TPS.

##<a name="integration"></a>Integracja z programem

###<a name="authentication"></a>Uwierzytelnianie
Analizy Azure Marketplace obsługuje Basic albo OAuth metody uwierzytelniania. Klucze konta łatwo można znaleźć, przechodząc do klawiszy na rynku w obszarze [Ustawienia konta](https://datamarket.azure.com/account/keys). 
####<a name="basic-authentication"></a>Uwierzytelnianie podstawowe
Dodaj nagłówek autoryzacji:

    Authorization: Basic <creds>
               
    Where <creds> = ConvertToBase64("AccountKey:" + yourAccountKey);  
    
Konwertowanie Base64 (C#)

    var bytes = Encoding.UTF8.GetBytes("AccountKey:" + yourAccountKey);
    var creds = Convert.ToBase64String(bytes);
    
Konwertowanie Base64 (JavaScript)

    var creds = window.btoa("AccountKey" + ":" + yourAccountKey);
    



###<a name="service-uri"></a>Identyfikator URI usługi
Główny usługi identyfikator URI dla Azure maszynowego uczenia zalecenia API jest [tutaj.](https://api.datamarket.azure.com/amla/recommendations/v2/)

Identyfikator URI usługi pełny jest wyrażona za pomocą elementów specyfikacji OData.

###<a name="api-version"></a>Wersja API
Na końcu każdego połączenia interfejsu API uzyskuje parametru zapytania o nazwie apiVersion, która powinna być równa "1.0".

###<a name="ids-are-case-sensitive"></a>Identyfikatory jest uwzględniana wielkość liter
Identyfikatory, zwracane przez dowolną interfejsów API, jest uwzględniana wielkość liter i powinien być używany jako takie, po upływie jako parametrów w kolejnych interfejsu API. Na przykład identyfikatory modelu i identyfikatory wykazu jest uwzględniana wielkość liter.

###<a name="create-a-model"></a>Tworzenie modelu
Tworzenie żądania "tworzenie modeli":

| Metoda HTTP | IDENTYFIKATOR URI |
|:--------|:--------|
|WPIS     |`<rootURI>/CreateModel?modelName=%27<model_name>%27&apiVersion=%271.0%27`<br><br>Przykład:<br>`<rootURI>/CreateModel?modelName=%27MyFirstModel%27&apiVersion=%271.0%27`|

|   Nazwa parametru  |   Prawidłowe wartości                        |
|:--------          |:--------                              |
|   modelName   |   Tylko litery (A-Z, a do z), cyfry (0 – 9), łączniki (--) i znak podkreślenia (_) są dozwolone.<br>Maksymalna długość: 20 |
|   apiVersion      | 1.0 |
|||
| Treść żądania | BRAK |


**Odpowiedź**:

Kod stanu HTTP: 200

- `feed/entry/content/properties/id`-Zawiera identyfikator modelu.
**Uwaga**: identyfikator modelu jest uwzględniana wielkość liter.

OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Create A New Model</subtitle>
      <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T06:35:21Z</updated>
     <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">CreateANewModelEntity2</title>
    <updated>2014-10-05T06:35:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">a658c626-2baa-43a7-ac98-f6ee26120a12</d:Id>
        <d:Name m:type="Edm.String">MyFirstModel</d:Name>
        <d:Date m:type="Edm.String">10/5/2014 6:35:19 AM</d:Date>
        <d:Status m:type="Edm.String">Created</d:Status>
        <d:HasActiveBuild m:type="Edm.String">false</d:HasActiveBuild>
        <d:BuildId m:type="Edm.String">-1</d:BuildId>
        <d:Mpr m:type="Edm.String">0</d:Mpr>
        <d:UserName m:type="Edm.String">5-4058-ab36-1fe254f05102@dm.com</d:UserName>
        <d:Description m:type="Edm.String"></d:Description>
      </m:properties>
    </content>
      </entry>
    </feed>


###<a name="import-catalog-data"></a>Importowanie danych z wykazu

Jeśli przekazujesz kilka plików wykazu do tego samego modelu kilka połączeń możemy wstawi tylko nowych elementów wykazu. Istniejące elementy pozostaną z wartości pierwotne.

| Metoda HTTP | IDENTYFIKATOR URI |
|:--------|:--------|
|WPIS     |`<rootURI>/ImportCatalogFile?modelId=%27<modelId>%27&filename=%27<fileName>%27&apiVersion=%271.0%27`<br><br>Przykład:<br>`<rootURI>/ImportCatalogFile?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&filename=%27catalog10_small.txt%27&apiVersion=%271.0%27`|

|   Nazwa parametru  |   Prawidłowe wartości                        |
|:--------          |:--------                              |
|   modelId |   Unikatowy identyfikator modelu (jest uwzględniana wielkość liter)  |
| Nazwa pliku | Identyfikator tekstowy wykazu.<br>Tylko litery (A-Z, a do z), cyfry (0 – 9), łączniki (--) i znak podkreślenia (_) są dozwolone.<br>Maksymalna długość: 50 |
|   apiVersion      | 1.0 |
|||
| Treść żądania | Wykazu danych. Format:<br>`<Item Id>,<Item Name>,<Item Category>[,<description>]`<br><br><table><tr><th>Nazwa</th><th>Obowiązkowe</th><th>Typ</th><th>Opis</th></tr><tr><td>Identyfikator elementu</td><td>Tak</td><td>Alfanumeryczny, maksymalna długość 50</td><td>Unikatowy identyfikator elementu</td></tr><tr><td>Nazwa elementu</td><td>Tak</td><td>Alfanumeryczny, maksymalna długość 255 znaków</td><td>Nazwa elementu</td></tr><tr><td>Element kategorii</td><td>Tak</td><td>Alfanumeryczny, maksymalna długość 255 znaków</td><td>Kategorii, do której ten element należy (np. gotowania książki teatralne...)</td></tr><tr><td>Opis</td><td>Brak</td><td>Alfanumeryczny, maksymalna długość 4000 znaków</td><td>Opis tego elementu</td></tr></table><br>Maksymalny rozmiar pliku jest 200MB.<br><br>Przykład:<br><pre>2406e770-769c-4189-89de-1c9283f93a96, Clara Callan książki<br>21bf8088-b6c0-4509-870c-e1c7ac78304a, pokoju zapomniane: książki fikcja (Byzantium skoroszyt),<br>Książki 3bb5cb44-d143-4bdd-a55c-443964bf4b23, Spadework,<br>552a1940-21e4-4399-82bb-594b46d7ed54, ograniczeń bestii, książki</pre> |


**Odpowiedź**:

Kod stanu HTTP: 200

- `Feed\entry\content\properties\LineCount`— Zaakceptowane liczba wierszy.
- `Feed\entry\content\properties\ErrorCount`-Liczba wierszy, które nie były wstawione z powodu błędu.

OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
        <subtitle type="text">Import catalog file</subtitle>
        <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-05T06:58:04Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">ImportCatalogFileEntity2</title>
    <updated>2014-10-05T06:58:04Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:LineCount m:type="Edm.String">4</d:LineCount>
        <d:ErrorCount m:type="Edm.String">0</d:ErrorCount>
      </m:properties>
    </content>
     </entry>
    </feed>


###<a name="import-usage-data"></a>Importowanie danych dotyczących użycia

####<a name="uploading-a-file"></a>Przekazywanie pliku
W tej sekcji przedstawiono sposób przekazywania danych dotyczących użycia za pomocą pliku. Możesz zadzwonić do tego interfejsu API z danych dotyczących użycia. Wszystkie dane użycia są zapisywane dla wszystkich połączeń.

| Metoda HTTP | IDENTYFIKATOR URI |
|:--------|:--------|
|WPIS     |`<rootURI>/ImportUsageFile?modelId=%27<modelId>%27&filename=%27<fileName>%27&apiVersion=%271.0%27`<br><br>Przykład:<br>`<rootURI>/ImportUsageFile?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&filename=%27ImplicitMatrix10_Guid_small.txt%27&apiVersion=%271.0%27`|

|   Nazwa parametru  |   Prawidłowe wartości                        |
|:--------          |:--------                              |
|   modelId |   Unikatowy identyfikator modelu (jest uwzględniana wielkość liter) |
| Nazwa pliku | Identyfikator tekstowy wykazu.<br>Tylko litery (A-Z, a do z), cyfry (0 – 9), łączniki (--) i znak podkreślenia (_) są dozwolone.<br>Maksymalna długość: 50 |
|   apiVersion      | 1.0 |
|||
| Treść żądania | Dane dotyczące użycia. Format:<br>`<User Id>,<Item Id>[,<Time>,<Event>]`<br><br><table><tr><th>Nazwa</th><th>Obowiązkowe</th><th>Typ</th><th>Opis</th></tr><tr><td>Identyfikator użytkownika</td><td>Tak</td><td>Alfanumeryczny</td><td>Unikatowy identyfikator użytkownika</td></tr><tr><td>Identyfikator elementu</td><td>Tak</td><td>Alfanumeryczny, maksymalna długość 50</td><td>Unikatowy identyfikator elementu</td></tr><tr><td>Czas</td><td>Brak</td><td>Data w formacie: YYYY-MM-ddTHH (przykład 2013-06-20T10:00:00)</td><td>Czas danych</td></tr><tr><td>Zdarzenia</td><td>Nie, jeśli dostarczane, a następnie należy również wprowadzić daty</td><td>Jedną z następujących czynności:<br>• Kliknij przycisk<br>• RecommendationClick<br>• AddShopCart<br>• RemoveShopCart<br>• Zakupu</td><td></td></tr></table><br>Maksymalny rozmiar pliku jest 200MB.<br><br>Przykład:<br><pre>149452, 1b3d95e2-84e4-414c-bb38-be9cf461c347<br>6360, 1b3d95e2-84e4-414c-bb38-be9cf461c347<br>50321, 1b3d95e2-84e4-414c-bb38-be9cf461c347<br>71285, 1b3d95e2-84e4-414c-bb38-be9cf461c347<br>224450, 1b3d95e2-84e4-414c-bb38-be9cf461c347<br>236645, 1b3d95e2-84e4-414c-bb38-be9cf461c347<br>107951, 1b3d95e2-84e4-414c-bb38-be9cf461c347</pre> |

**Odpowiedź**:

Kod stanu HTTP: 200

- `Feed\entry\content\properties\LineCount`— Zaakceptowane liczba wierszy.
- `Feed\entry\content\properties\ErrorCount`-Liczba wierszy, które nie były wstawione z powodu błędu.
- `Feed\entry\content\properties\FileId`-Identyfikator pliku.


OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Add bulk usage data (usage file)</subtitle>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-05T07:21:44Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">AddBulkUsageDataUsageFileEntity2</title>
    <updated>2014-10-05T07:21:44Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:LineCount m:type="Edm.String">33</d:LineCount>
        <d:ErrorCount m:type="Edm.String">0</d:ErrorCount>
        <d:FileId m:type="Edm.String">fead7c1c-db01-46c0-872f-65bcda36025d</d:FileId>
      </m:properties>
    </content>
    </entry>
    </feed>


####<a name="using-data-acquisition"></a>Przy użyciu danych
W tej sekcji przedstawiono sposoby wysyłania zdarzeń w czasie rzeczywistym do Azure maszynowego uczenia zalecenia, zwykle z witryny sieci Web.

| Metoda HTTP | IDENTYFIKATOR URI |
|:--------|:--------|
|WPIS     |`<rootURI>/AddUsageEvent?apiVersion=%271.0%27-f6ee26120a12%27&filename=%27catalog10_small.txt%27&apiVersion=%271.0%27`|

|   Nazwa parametru  |   Prawidłowe wartości                        |
|:--------          |:--------                              |
|   apiVersion      | 1.0 |
|||
|Treść żądania| Wprowadzanie danych zdarzenia dla każdego zdarzenia, które chcesz wysłać. Należy wysłać dla tej samej sesji użytkownika lub przeglądarki ten sam identyfikator w polu Identyfikator sesji. (Zobacz przykładowe zdarzenia treści poniżej).|


- Przykład dla zdarzenia "Kliknięcia":

        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
        <SessionId>11112222</SessionId>
        <EventData>
        <EventData>
        <Name>Click</Name>
        <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
        </EventData>
        </EventData>
        </Event>

- Przykład dla zdarzenia "RecommendationClick":

        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
        <SessionId>11112222</SessionId>
        <EventData>
        <EventData>
        <Name>RecommendationClick</Name>
        <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
        </EventData>
        </EventData>
        </Event>

- Przykład dla zdarzenia "AddShopCart":

        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
        <SessionId>11112222</SessionId>
        <EventData>
        <EventData>
        <Name>AddShopCart</Name>
        <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
        </EventData>
        </EventData>
        </Event>

- Przykład dla zdarzenia "RemoveShopCart":

        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
        <SessionId>11112222</SessionId>
        <EventData>
        <EventData>
        <Name>RemoveShopCart</Name>
        <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
        </EventData>
        </EventData>
        </Event>

- Przykład dla zdarzenia "Sprzedaży":

        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
        <SessionId>11112222</SessionId>
        <EventData>
        <EventData>
            <Name>Purchase</Name> 
            <PurchaseItems>
            <PurchaseItems>
                <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
                <Count>3</Count>
            </PurchaseItems>
        </PurchaseItems>
        </EventData>
        </EventData>
        </Event>

- Przykład 2 zdarzenia, "Kliknij" i "AddShopCart":

        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
        <SessionId>11112222</SessionId>
        <EventData>
        <EventData>
        <Name>Click</Name>
        <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
        <ItemName>itemName</ItemName>
        <ItemDescription>item description</ItemDescription>
        <ItemCategory>category</ItemCategory>
        </EventData>
        <EventData>
        <Name>AddShopCart</Name>
        <ItemId>552A1940-21E4-4399-82BB-594B46D7ED54</ItemId>
        </EventData>
        </EventData>
        </Event>

**Odpowiedź**: kod stanu HTTP: 200

###<a name="build-a-recommendation-model"></a>Tworzenie modelu rekomendacji

| Metoda HTTP | IDENTYFIKATOR URI |
|:--------|:--------|
|WPIS     |`<rootURI>/BuildModel?modelId=%27<modelId>%27&userDescription=%27<description>%27&apiVersion=%271.0%27`<br><br>Przykład:<br>`<rootURI>/BuildModel?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&userDescription=%27First%20build%27&apiVersion=%271.0%27`|

|   Nazwa parametru  |   Prawidłowe wartości                        |
|:--------          |:--------                              |
| modelId | Unikatowy identyfikator modelu (jest uwzględniana wielkość liter)  |
| userDescription | Identyfikator tekstowy wykazu. Uwaga Jeśli używasz spacje musi kodowanie go z % 20 zamiast tego. Zobacz powyższym przykładzie.<br>Maksymalna długość: 50 |
| apiVersion | 1.0 |
|||
| Treść żądania | BRAK |

**Odpowiedź**:

Kod stanu HTTP: 200

To jest interfejs API asynchroniczne. Zostanie wyświetlony identyfikator kompilacji w odpowiedzi. Aby dowiedzieć się, gdy kompilacji został zakończony, należy połączenie interfejsu API "Pobierz tworzy stanu modelu" i znajdź ten identyfikator kompilacji w odpowiedzi. Należy zauważyć, że kompilacji można wykonywać minut do godziny, w zależności od rozmiaru danych.

Nie mogą korzystać z zalecenia do kompilacji kończy się.

Stan konstruowania prawidłowe:

- Tworzenie — Model został utworzony.
- W kolejce — Tworzenie modelu wyzwolone i go znajduje się w kolejce.
- Skonstruowaniu jest konstrukcyjnych — modelu.
- Powodzenia — Tworzenie zakończone pomyślnie.
- Błąd — Tworzenie zakończone z błędem.
- Anulowane — Tworzenie zostało anulowane.
- Anulowanie — anulowania kompilacji.


Zwróć uwagę, że identyfikator kompilacji można znaleźć w następującej ścieżce:`Feed\entry\content\properties\Id`

OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Build a Model with RequestBody</subtitle>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First build'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-05T08:56:34Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First%20build'&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First build'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">BuildAModelEntity2</title>
    <updated>2014-10-05T08:56:34Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First%20build'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">1000272</d:Id>
        <d:UserName m:type="Edm.String"></d:UserName>
        <d:ModelId m:type="Edm.String">9559872f-7a53-4076-a3c7-19d9385c1265</d:ModelId>
        <d:ModelName m:type="Edm.String">docTest</d:ModelName>
        <d:Type m:type="Edm.String">Recommendation</d:Type>
        <d:CreationTime m:type="Edm.String">2014-10-05T08:56:31.893</d:CreationTime>
        <d:Progress_BuildId m:type="Edm.String">1000272</d:Progress_BuildId>
        <d:Progress_ModelId m:type="Edm.String">9559872f-7a53-4076-a3c7-19d9385c1265</d:Progress_ModelId>
        <d:Progress_UserName m:type="Edm.String">5-4058-ab36-1fe254f05102@dm.com</d:Progress_UserName>
        <d:Progress_IsExecutionStarted m:type="Edm.String">false</d:Progress_IsExecutionStarted>
        <d:Progress_IsExecutionEnded m:type="Edm.String">false</d:Progress_IsExecutionEnded>
        <d:Progress_Percent m:type="Edm.String">0</d:Progress_Percent>
        <d:Progress_StartTime m:type="Edm.String">0001-01-01T00:00:00</d:Progress_StartTime>
        <d:Progress_EndTime m:type="Edm.String">0001-01-01T00:00:00</d:Progress_EndTime>
        <d:Progress_UpdateDateUTC m:type="Edm.String"></d:Progress_UpdateDateUTC>
        <d:Status m:type="Edm.String">Queued</d:Status>
        <d:Key1 m:type="Edm.String">UseFeaturesInModel</d:Key1>
        <d:Value1 m:type="Edm.String">False</d:Value1>
      </m:properties>
    </content>
    </entry>
    </feed>

###<a name="get-build-status-of-a-model"></a>Pobierz stan konstruowania modelu

| Metoda HTTP | IDENTYFIKATOR URI |
|:--------|:--------|
|Pobierz     |`<rootURI>/GetModelBuildsStatus?modelId=%27<modelId>%27&onlyLastBuild=<bool>&apiVersion=%271.0%27`<br><br>Przykład:<br>`<rootURI>/GetModelBuildsStatus?modelId=%279559872f-7a53-4076-a3c7-19d9385c1265%27&onlyLastBuild=true&apiVersion=%271.0%27`|



|   Nazwa parametru  |   Prawidłowe wartości                        |
|:--------          |:--------                              |
|   modelId         |   Unikatowy identyfikator modelu (jest uwzględniana wielkość liter)    |
|   onlyLastBuild   |   Wskazuje, czy zwraca cała historia kompilacji modelu lub tylko stan najnowszej kompilacji. |
|   apiVersion      |   1.0                                 |


**Odpowiedź**:

Kod stanu HTTP: 200

Odpowiedź zawiera pojedyncze wpisy w poszczególnych kompilacji. Każdy wpis ma poniższe dane:

- `feed/entry/content/properties/UserName`— Nazwa użytkownika.
- `feed/entry/content/properties/ModelName`— Nazwa modelu.
- `feed/entry/content/properties/ModelId`— Unikatowy identyfikator modelu.
- `feed/entry/content/properties/IsDeployed`— Czy to kompilacja zostanie wdrożony (alias Tworzenie aktywnej).
- `feed/entry/content/properties/BuildId`— Tworzenie Unikatowy identyfikator.
- `feed/entry/content/properties/BuildType`— Typ kompilacji.
- `feed/entry/content/properties/Status`— Tworzenie stanu. Może być jedną z następujących czynności: błąd, konstrukcyjnych, kolejce, Cancelling, odwołania, sukcesu
- `feed/entry/content/properties/StatusMessage`— Komunikat o stanie szczegółowe (dotyczy tylko określonych Zjednoczone).
- `feed/entry/content/properties/Progress`— Tworzenie postępu (%).
- `feed/entry/content/properties/StartTime`— Godzina rozpoczęcia kompilacji.
- `feed/entry/content/properties/EndTime`— Tworzenie czas zakończenia.
- `feed/entry/content/properties/ExecutionTime`— Tworzenie czas trwania.
- `feed/entry/content/properties/ProgressStep`— Szczegóły dotyczące bieżącego etapu należącym do kompilacji w toku.

Stan konstruowania prawidłowe:
- Utworzone — wpis żądanie kompilacji został utworzony.
- W kolejce — wyzwolone żądanie kompilacji i jego znajduje się w kolejce.
- Tworzenie — tworzenie jest w toku.
- Powodzenia — Tworzenie zakończone pomyślnie.
- Błąd — Tworzenie zakończone z błędem.
- Anulowane — Tworzenie zostało anulowane.
- Anulowanie — anulowania kompilacji.

Prawidłowe wartości dla typu kompilacji:
- Pozycja — tworzenie pozycja. (Na pozycję utworzyć szczegóły, można znaleźć w dokumencie "Dokumentacji maszynowego uczenia rekomendacji interfejsu API").
- Zalecenie — tworzenie rekomendacji.
- Zmianie wysokości progów - zakupiony razem często kompilacji.

OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Get builds status of a model</subtitle>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-11-05T17:51:10Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">GetBuildsStatusEntity</title>
    <updated>2014-11-05T17:51:10Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:UserName m:type="Edm.String">b-434e-b2c9-84935664ff20@dm.com</d:UserName>
        <d:ModelName m:type="Edm.String">ModelName</d:ModelName>
        <d:ModelId m:type="Edm.String">1d20c34f-dca1-4eac-8e5d-f299e4e4ad66</d:ModelId>
        <d:IsDeployed m:type="Edm.String">true</d:IsDeployed>
        <d:BuildId m:type="Edm.String">1000272</d:BuildId>
        <d:BuildType m:type="Edm.String">Recommendation</d:BuildType>
        <d:Status m:type="Edm.String">Success</d:Status>
        <d:StatusMessage m:type="Edm.String"></d:StatusMessage>
        <d:Progress m:type="Edm.String">0</d:Progress>
        <d:StartTime m:type="Edm.String">2014-11-02T13:43:51</d:StartTime>
        <d:EndTime m:type="Edm.String">2014-11-02T13:45:10</d:EndTime>
        <d:ExecutionTime m:type="Edm.String">00:01:19</d:ExecutionTime>
        <d:IsExecutionStarted m:type="Edm.String">false</d:IsExecutionStarted>
        <d:ProgressStep m:type="Edm.String"></d:ProgressStep>
      </m:properties>
     </content>
    </entry>
    </feed>


###<a name="get-recommendations"></a>Uzyskaj zalecenia

| Metoda HTTP | IDENTYFIKATOR URI |
|:--------|:--------|
|Pobierz     |`<rootURI>/ItemRecommend?modelId=%27<modelId>%27&itemIds=%27<itemId>%27&numberOfResults=<int>&includeMetadata=<bool>&apiVersion=%271.0%27`<br><br>Przykład:<br>`<rootURI>/ItemRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&itemIds=%271003%27&numberOfResults=10&includeMetadata=false&apiVersion=%271.0%27`|



|   Nazwa parametru  |   Prawidłowe wartości                        |
|:--------          |:--------                              |
| modelId | Unikatowy identyfikator modelu (jest uwzględniana wielkość liter) |
| identyfikatory elementów | Rozdzielaną średnikami listę elementów do jest zalecane dla.<br>Maksymalna długość: 1024 |
| numberOfResults | Liczba wyników wymagane |
| includeMetatadata | Użycia w przyszłości, zawsze wartość logiczna FAŁSZ |
| apiVersion | 1.0 |

**Odpowiedź:**

Kod stanu HTTP: 200

Odpowiedź zawiera pojedyncze wpisy w poszczególnych elementów zalecane. Każdy wpis ma poniższe dane:

- `Feed\entry\content\properties\Id`— Identyfikator elementu polecane.
- `Feed\entry\content\properties\Name`-Nazwę elementu.
- `Feed\entry\content\properties\Rating`— Ocena zalecenia; większa niż liczba oznacza, że wyższa zaufania.
- `Feed\entry\content\properties\Reasoning`-Zalecenie uzasadnienie (np. objaśnienia rekomendacji).

OData XML

Odpowiedź przykładzie poniżej zawiera 10 zalecane elementy:

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
     <subtitle type="text">Get Recommendation</subtitle>
     <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">159</d:Id>
        <d:Name m:type="Edm.String">159</d:Name>
        <d:Rating m:type="Edm.Double">0.543343480387708</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '159'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">52</d:Id>
        <d:Name m:type="Edm.String">52</d:Name>
        <d:Rating m:type="Edm.Double">0.539588900748721</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '52'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">35</d:Id>
        <d:Name m:type="Edm.String">35</d:Name>
        <d:Rating m:type="Edm.Double">0.53842946443853</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '35'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">124</d:Id>
        <d:Name m:type="Edm.String">124</d:Name>
        <d:Rating m:type="Edm.Double">0.536712832792886</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '124'</d:Reasoning>
      </m:properties>
    </content>
    </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">120</d:Id>
        <d:Name m:type="Edm.String">120</d:Name>
        <d:Rating m:type="Edm.Double">0.533673023762878</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '120'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">96</d:Id>
        <d:Name m:type="Edm.String">96</d:Name>
        <d:Rating m:type="Edm.Double">0.532544826370521</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '96'</d:Reasoning>
      </m:properties>
    </content>
    </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">69</d:Id>
        <d:Name m:type="Edm.String">69</d:Name>
        <d:Rating m:type="Edm.Double">0.531678607847759</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '69'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">172</d:Id>
        <d:Name m:type="Edm.String">172</d:Name>
        <d:Rating m:type="Edm.Double">0.530957821375951</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '172'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">155</d:Id>
        <d:Name m:type="Edm.String">155</d:Name>
        <d:Rating m:type="Edm.Double">0.529093541481333</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '155'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
     <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">32</d:Id>
        <d:Name m:type="Edm.String">32</d:Name>
        <d:Rating m:type="Edm.Double">0.528917978168322</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '32'</d:Reasoning>
      </m:properties>
    </content>
     </entry>
    </feed>

###<a name="update-model"></a>Model aktualizacji
Możesz zaktualizować opis modelu lub identyfikatora aktywnej kompilacji.
*Identyfikator aktywnego kompilacji* — co kompilacji dla każdego modelu ma identyfikator kompilacji. Identyfikator aktywnego kompilacji jest pierwszego pomyślnego kompilacji każdy nowy model. Po masz identyfikator aktywnego kompilacji i jak tworzy dodatkowe dla tego samego modelu, należy jawnie ustawić go jako domyślny identyfikator kompilacji Jeśli chcesz. Gdy możesz wykorzystać zalecenia, jeśli nie określisz identyfikator kompilacji, który ma być używany, domyślny będzie automatycznie używana.

Ten mechanizm umożliwia - po umieszczeniu modelu rekomendacji w produkcja — do tworzenia nowych modeli i je przetestować, zanim promowanie produkcji.

| Metoda HTTP | IDENTYFIKATOR URI |
|:--------|:--------|
|UMIESZCZENIE     |`<rootURI>/UpdateModel?id=%27<modelId>%27&apiVersion=%271.0%27`<br><br>Przykład:<br>`<rootURI>/UpdateModel?id=%279559872f-7a53-4076-a3c7-19d9385c1265%27&apiVersion=%271.0%27`|


|   Nazwa parametru  |   Prawidłowe wartości                        |
|:--------          |:--------                              |
| Identyfikator | Unikatowy identyfikator modelu (jest uwzględniana wielkość liter) |
| apiVersion | 1.0 |
|||
| Treść żądania | `<ModelUpdateParams xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">`<br>`   <Description>New Description</Description>`<br>`          <ActiveBuildId>-1</ActiveBuildId>`<br>`</ModelUpdateParams>`<br><br>Należy zauważyć, że znaczniki XML, opis i ActiveBuildId są opcjonalne. Jeśli nie chcesz ustawić opis lub ActiveBuildId, należy usunąć całą znacznik. |

**Odpowiedź**:

Kod stanu HTTP: 200

OData XML

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/UpdateModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Update an Existing Model</subtitle>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/UpdateModel?id='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;apiVersion='1.0'</id>
    <rights type="text" />
     <updated>2014-10-05T10:27:17Z</updated>
     <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/UpdateModel?id='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;apiVersion='1.0'" />
    </feed>

##<a name="legal"></a>Prawne
Niniejszy dokument jest przeznaczony "jako-jest". Informacje i opinie w tym dokumencie, w tym adresy URL i inne odwołania do witryny sieci Web internetowe mogą ulec zmianie bez powiadomienia. Niektóre przedstawione przykłady są dostarczane wyłącznie jako ilustracja i są fikcyjny. Nie skojarzenia rzeczywistą połączenia jest przeznaczony lub zdarzeniami. Ten dokument nie umożliwiają jakiekolwiek prawa do jakiejkolwiek własności intelektualnej produktów firmy Microsoft. Użytkownik może kopiować i używać tego dokumentu wewnętrznych celach informacyjnych. © Microsoft 2014 r. Nazwa firmy. 
 
