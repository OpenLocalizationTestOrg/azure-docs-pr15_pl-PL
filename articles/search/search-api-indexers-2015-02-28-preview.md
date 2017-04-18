<properties 
pageTitle="Operacje indeksatora (interfejsu API usługi REST usługi Azure wyszukiwania: 2015-02-28-Podgląd) | Podgląd wyszukiwania Azure interfejsu API" 
description="Operacje indeksatora (interfejsu API usługi REST usługi Azure wyszukiwania: 2015-02-28-Preview)" 
services="search" 
documentationCenter="" 
authors="chaosrealm" 
manager="pablocas"
editor="" />

<tags 
ms.service="search" 
ms.devlang="rest-api" 
ms.workload="search" 
ms.topic="article"  
ms.tgt_pltfrm="na" 
ms.date="09/07/2016" 
ms.author="eugenesh" />

#<a name="indexer-operations-azure-search-service-rest-api-2015-02-28-preview"></a>Operacje indeksatora (interfejsu API usługi REST usługi Azure wyszukiwania: 2015-02-28-Preview)#

> [AZURE.NOTE] W tym artykule opisano indeksatorów w [interfejsu API usługi REST 2015-02-28-Podgląd](search-api-2015-02-28-preview.md). Ta wersja API dodaje wersji preview indeksatora magazyn obiektów Blob platformy Azure z wyodrębniania dokumentu i indeksatora magazyn tabel platformy Azure, a także inne ulepszenia. Interfejs API obsługuje także ogólnodostępne indeksatory (GA), w tym indeksatory dla bazy danych SQL Azure, SQL Server Azure maszyny wirtualne i Azure DocumentDB.

## <a name="overview"></a>Omówienie ##

Azure wyszukiwania można zintegrować bezpośrednio z niektóre typowe źródła danych, usuwając konieczność pisania kodu do indeksowania danych. Aby skonfigurować to konto, możesz zadzwonić interfejsu API wyszukiwania Azure tworzyć i zarządzać nimi **indeksatory** i **źródeł danych**. 

**Indeksowanie** jest zasobu łączącego źródeł danych z indeksów wyszukiwania docelowej. Indeksowanie jest używany w następujący sposób: 

- Wykonaj kopię jednorazowego dane służące do wypełnienia indeksu.
- Synchronizowanie indeksu o zmiany wprowadzone w źródle danych zgodnie z harmonogramem. Harmonogram jest częścią definicji indeksowania.
- Wywołania na żądanie aktualizowania indeksu, stosownie do potrzeb. 

**Indeksowanie** jest przydatne, gdy chcesz regularne aktualizacje do indeksu. Możesz skonfigurować harmonogram w tekście jako część definicji indeksatora lub uruchom go na żądanie przy użyciu [Uruchomić indeksowania](#RunIndexer). 

**Źródła danych** Określa, jakie dane musi być indeksowane, poświadczenia, aby uzyskać dostęp do danych i zasady, aby włączyć wyszukiwanie Azure efektywne identyfikowania zmian w danych (takie jak zmodyfikowane lub usunięte wiersze w tabeli bazy danych). Zdefiniowano jako niezależnych zasobów, aby mogą być używane przez wiele indeksatory.

Obecnie obsługiwane są następujące źródła danych:

- **Baza danych SQL Azure** i **SQL Server na Azure maszyny wirtualne**. Dla wybranych samouczek zobacz [Ten artykuł](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers-2015-02-28.md). 
- **Azure DocumentDB**. Dla wybranych samouczek zobacz [Ten artykuł](../documentdb/documentdb-search-indexer.md). 
- **Magazyn obiektów Blob platformy azure**, w tym dokumencie w następujących formatach: PDF, pakiet Microsoft Office (DOC i DOCX, XSLX i XLS, PPT i PPTX, MSG), HTML, XML, ZIP i zwykły tekst pliki (w tym JSON). Dla wybranych samouczek zobacz [Ten artykuł](search-howto-indexing-azure-blob-storage.md).
- **Magazyn tabel platformy azure**. Dla wybranych samouczek zobacz [Ten artykuł](search-howto-indexing-azure-tables.md).
     
Firma Microsoft uznajesz, dodawanie obsługi kolejnych źródeł danych w przyszłości. Aby ułatwić nam Wyznaczanie priorytetów tych decyzji, podaj swoją opinię na [forum opinii Azure wyszukiwania](http://feedback.azure.com/forums/263029-azure-search).

Zobacz [Usługa limity](search-limits-quotas-capacity.md) dla maksymalną związane z zasobami źródła danych i indeksowania.

## <a name="typical-usage-flow"></a>Typowe zastosowania przepływu ##

Można tworzyć i zarządzać indeksatory i źródeł danych za pośrednictwem prostych żądań HTTP (WPIS: GET, położenie, Usuń) przed danym `data source` lub `indexer` zasobów.

Konfigurowanie automatycznego indeksowania jest zazwyczaj proces czterech kroków:

1. Zidentyfikuj źródła danych, która zawiera dane, których potrzebuje do indeksowania. Należy pamiętać, że Azure wyszukiwania może nie obsługuje wszystkich typów danych w źródle danych. Zobacz [obsługiwane typy danych](https://msdn.microsoft.com/library/azure/dn798938.aspx) dla listy.

2. Tworzenie indeksu wyszukiwania Azure, którego schemat jest zgodny z źródła danych.
  
3. Tworzenie źródła danych wyszukiwania Azure, zgodnie z opisem w [Tworzenie źródła danych](#CreateDataSource).
  
4. Tworzenie indeksowanie wyszukiwania Azure jako opisano [Tworzenie indeksowania](#CreateIndexer).

Należy zaplanować na tworzenie jednej indeksowania dla każdej kombinacji źródła docelowej, jak indeksu i danych. Można mieć wiele indeksatory pismo ręczne na ten sam indeks, a następnie można ponownie użyć tego samego źródła danych dla wielu indeksatory. Jednak indeksatora mogą zajmować tylko jedno źródło danych w czasie i można zapisywać tylko jeden indeks. 

Po utworzeniu indeksatora, możesz pobrać jego stan wykonywanie operacji [Pobierz stan indeksowania](#GetIndexerStatus) . Możesz również uruchomić indeksatora w dowolnym momencie (zamiast lub razem z nim okresowo zgodnie z harmonogramem) za pomocą [Uruchamianie](#RunIndexer) indeksatora.

<!-- MSDN has 2 art files plus a API topic link list -->


## <a name="create-data-source"></a>Tworzenie źródła danych ##

W polu Wyszukaj Azure źródła danych jest używana z indeksatory, dostarczając informacje o połączeniu danych ad hoc lub zaplanowane odświeżanie indeksu docelowej. Można utworzyć nowe źródło danych w ramach usługi Azure wyszukiwania przy użyciu żądania HTTP POST.
    
    POST https://[service name].search.windows.net/datasources?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

Można również za pomocą funkcji i określ nazwę źródła danych na identyfikator URI. Jeśli źródło danych nie istnieje, zostanie on utworzony.

    PUT https://[service name].search.windows.net/datasources/[datasource name]?api-version=[api-version]

**Uwaga**: Maksymalna liczba źródeł danych dozwolonych zależy od ceny warstwy. Bezpłatna usługa umożliwia maksymalnie 3 źródeł danych. Standardowa usługa umożliwia 50 źródeł danych. Aby uzyskać szczegółowe informacje, zobacz [Limity dotyczące usługi](search-limits-quotas-capacity.md) .

**Żądanie**

Protokół HTTPS jest wymagane dla wszystkich żądania obsługi. Żądanie **Utwórz źródło danych** może być skonstruowane za pomocą metody POST lub położenie. Gdy używasz WPIS, należy podać nazwę źródła danych w treści wezwania wraz z definicji źródła danych. POŁOŻENIE nazwę ma część adresu URL. Jeśli źródło danych nie istnieje, zostanie utworzony. Jeśli już istnieje, jest aktualizowana do nowej definicji. 

Nazwa źródła danych musi zawierać małe litery, zaczynać literą lub numer, nie ukośników lub kropki i zawierać mniej niż 128 znaków. Po uruchomieniu nazwa źródła danych z list lub numer pozostałą część nazwy mogą zawierać dowolną literę, liczbę i kreski, ile kreski nie są kolejne. Aby uzyskać szczegółowe informacje, zobacz artykuł [reguły nazw](https://msdn.microsoft.com/library/azure/dn857353.aspx) .

`api-version` Jest wymagane. Bieżąca wersja jest `2015-02-28`.

**Żądanie nagłówków**

Na poniższej liście przedstawiono nagłówków żądania wymaganych i opcjonalnych. 

- `Content-Type`: Wymagane. Można ustawić`application/json`
- `api-key`: Wymagane. `api-key` Jest używany do uwierzytelniania żądania usługi wyszukiwania. Jest to wartość ciągu, unikatowe dla tej usługi. Żądanie **Tworzenie źródła danych** może zawierać `api-key` ustawione na wartość klucza administratora (zamiast klawisza kwerendy). 
 
Konieczne będzie również nazwę usługi do utworzenia adresu URL żądania. Zostanie wyświetlony nazwy usługi i `api-key` z pulpitu nawigacyjnego usług w [Azure Portal](https://portal.azure.com/). Zobacz [Tworzenie usługi wyszukiwania w portalu](search-create-service-portal.md) dla Nawigacja między stronami pomocy.

<a name="CreateDataSourceRequestSyntax"></a>
**Żądanie składni treści**

Treść żądania zawiera definicji źródła danych, zawierający poświadczenia, aby odczytywanie danych, a także dane opcjonalne Zmienianie wykrywanie i zasady wykrywania usunięcia danych, które są używane do identyfikowania efektywne zmianie lub usunięciu danych w źródle danych używanego z systemami okresowo zaplanowane indeksatora typu źródła danych. 

Składnia struktury ładunku żądanie wygląda następująco: Przykładowe żądanie znajduje się bardziej na w tym temacie.

    { 
        "name" : "Required for POST, optional for PUT. The name of the data source",
        "description" : "Optional. Anything you want, or nothing at all",
        "type" : "Required. Must be one of 'azuresql', 'documentdb', 'azureblob', or 'azuretable'",
        "credentials" : { "connectionString" : "Required. Connection string for your data source" },
        "container" : { "name" : "Required. The name of the table, collection, or blob container you wish to index" },
        "dataChangeDetectionPolicy" : { Optional. See below for details }, 
        "dataDeletionDetectionPolicy" : { Optional. See below for details }
    }

Żądanie zawiera następujące właściwości: 

- `name`: Wymagane. Nazwa źródła danych. Nazwa źródła danych musi tylko zawierać małe litery, cyfry lub kreski, nie można uruchamiać ani kończy się pauzy i jest ograniczona do 128 znaków.
- `description`: Opcjonalny opis. 
- `type`: Wymagane. Musi być jednego z obsługiwanych typów źródeł:
    - `azuresql`-Baza danych SQL azure lub SQL Server na Azure maszyny wirtualne
    - `documentdb`-Azure DocumentDB
    - `azureblob`-Magazyn obiektów Blob azure
    - `azuretable`-Magazyn tabel azure
- `credentials`:
    - Wymagane `connectionString` właściwość określa parametry połączenia dla źródła danych. Format parametrów połączenia zależy od typu źródła danych: 
        - W przypadku Azure SQL jest zwykle parametry połączenia programu SQL Server. Jeśli używasz Azure portal pobieranie parametrów połączenia, za pomocą `ADO.NET connection string` opcji.
        - Aby DocumentDB, parametry połączenia musi być w następującym formacie: `"AccountEndpoint=https://[your account name].documents.azure.com;AccountKey=[your account key];Database=[your database id]"`. Wszystkie wartości są wymagane. Można je znaleźć w [Azure portal](https://portal.azure.com/).  
        - W przypadku obiektów Blob platformy Azure i Magazyn tabel jest parametry połączenia konta miejsca do magazynowania. Format jest opisany [poniżej](https://azure.microsoft.com/documentation/articles/storage-configure-connection-string/). Wymagane jest protokołu HTTPS punktu końcowego.  
- `container`, wymagane: Określa dane do indeksu przy użyciu `name` i `query` właściwości: 
    - `name`Wymagane:
        - Azure SQL: Określa tabeli lub widoku. Można używać kwalifikowanym schematu nazw, takich jak `[dbo].[mytable]`.
        - DocumentDB: Określa kolekcji. 
        - Magazyn obiektów Blob platformy Azure: Określa kontener miejsca do magazynowania.
        - Magazyn tabel platformy Azure: Nazwa tabeli. 
    - `query`, opcjonalne:
        - DocumentDB: pozwala określić kwerendę, która spłaszcza dowolnego układu dokumentu JSON do schematu prostym, które pozwalają na indeksowanie wyszukiwania Azure.  
        - Magazyn obiektów Blob platformy Azure: pozwala określić folder wirtualny w kontenerze obiektów blob. Na przykład dla obiektów blob ścieżki `mycontainer/documents/blob.pdf`, `documents` może być używany jako folder wirtualny.
        - Magazyn tabel platformy Azure: umożliwia określenie Kwerenda filtrująca zestaw wierszy, które mają zostać zaimportowane.
        - Azure SQL: kwerenda nie jest obsługiwane. Jeśli potrzebujesz tej funkcji, zobacz Zagłosuj na [Sugestia ta](https://feedback.azure.com/forums/263029-azure-search/suggestions/9893490-support-user-provided-query-in-sql-indexer)
- Opcjonalne `dataChangeDetectionPolicy` i `dataDeletionDetectionPolicy` właściwości opisane poniżej.

<a name="DataChangeDetectionPolicies"></a>
**Dane zmiany wykrywania zasad**

Celem zasad wykrywania zmiany danych jest efektywne identyfikowania elementów zmienionych danych. Obsługiwane zasady zależą od typu źródła danych. Poniżej opisano każdej zasady. 

***Górny limit zasad wykrywania zmian*** 

Użyj tych zasad, gdy źródło danych zawiera kolumnie lub właściwość, która spełnia następujące warunki:
 
- Wstawia wszystkie określ wartości w kolumnie. 
- Wszystkie aktualizacje do elementu również zmienić wartość kolumny. 
- Zwiększa wartości w tej kolumnie przy każdej zmianie.
- Zapytania, które za pomocą klauzuli filtru podobny do następującego `WHERE [High Water Mark Column] > [Current High Water Mark Value]` mogą być wykonywane wydajność.

Na przykład przy użyciu źródeł danych Azure SQL, indeksowane `rowversion` kolumna jest idealnym candidate dla systemu zasad CPU wysoki. 

Tych zasad można określić w następujący sposób:

    { 
        "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
        "highWaterMarkColumnName" : "[a row version or last_updated column name]" 
    } 

> [AZURE.NOTE] Gdy używasz DocumentDB źródeł danych, należy użyć `_ts` właściwość dostarczony przez DocumentDB. 

> [AZURE.NOTE] Podczas korzystania z obiektów Blob platformy Azure źródeł danych, wyszukiwania Azure automatycznie przy użyciu znaku wodnego wysoki zmiana zasad wykrywania według sygnatura czasowa ostatniej modyfikacji obiektów blob; nie musisz samodzielnie określić tych zasad.   

***SQL zintegrowanego zasad wykrywania zmian***

Jeśli baza danych SQL obsługuje, [śledzenia zmian](https://msdn.microsoft.com/library/bb933875.aspx), zalecamy używanie SQL zintegrowane Zmienianie zasady śledzenia. Zasada ta umożliwia śledzenie zmian najskuteczniejszą i umożliwia wyszukiwanie Azure do identyfikowania usuniętych wierszy bez konieczności mieć jawne "wygładzone Usuń" kolumnę w schematu.

Zintegrowane śledzenie zmian jest obsługiwane, zaczynając od następujących wersji bazy danych programu SQL Server: 
- Program SQL Server 2008 R2 Jeśli korzystasz z programu SQL Server na maszyny wirtualne Azure.
- Azure SQL bazy danych w wersji 12, jeśli korzystasz z bazy danych SQL Azure.  

Gdy przy użyciu zintegrowanego śledzenia zmian programu SQL zasad nie zostanie określony zasady wykrywania usunięcie osobnych danych — tych zasad ma wbudowanych funkcji identyfikowania usunięte wiersze. 

Tych zasad można używać tylko z tabelami; Nie można używać z widokami. Musisz włączyć śledzenie dla tabeli, którego używasz, zanim będzie można używać tych zasad zmian. Aby uzyskać instrukcje, zobacz [Włączanie i wyłączanie śledzenia zmian](https://msdn.microsoft.com/library/bb964713.aspx) .    
 
Podczas tworzenia struktury żądania **Utwórz źródło danych** , zasad śledzenia zmian SQL scałkowanej w przedziale można określić w następujący sposób:

    { 
        "@odata.type" : "#Microsoft.Azure.Search.SqlIntegratedChangeTrackingPolicy" 
    }

<a name="DataDeletionDetectionPolicies"></a>
**Zasady wykrywania usunięcia danych**

Celem zasad wykrywania usunięcia danych jest efektywne identyfikowania elementów usuniętych danych. Obecnie jest obsługiwane tylko zasady `Soft Delete` zasady, które umożliwia identyfikowanie usunięte elementy na podstawie wartości z `soft delete` kolumnie lub właściwość w źródle danych. Tych zasad można określić w następujący sposób:

    { 
        "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
        "softDeleteColumnName" : "the column that specifies whether a row was deleted", 
        "softDeleteMarkerValue" : "the value that identifies a row as deleted" 
    }

**Uwaga:** Obsługiwane są tylko kolumny zawierające ciąg znaków, liczba całkowita lub wartości logiczne. Wartość używana jako `softDeleteMarkerValue` musi być ciągiem, nawet jeśli odpowiedniej kolumny zawiera liczby całkowite i wartości logiczne. Na przykład, jeśli wartość, która jest wyświetlana w źródle danych jest 1, za pomocą `"1"` jako `softDeleteMarkerValue`.    

<a name="CreateDataSourceRequestExamples"></a>
**Przykłady treści wezwania**

Jeśli zamierzasz użyć źródła danych z indeksatora uruchamia zgodnie z harmonogramem, w tym przykładzie przedstawiono sposób określania oraz ich modyfikowanie i usuwanie zasad wykrywania: 

    { 
        "name" : "asqldatasource",
        "description" : "a description",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "Server=tcp:....database.windows.net,1433;Database=...;User ID=...;Password=...;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;" },
        "container" : { "name" : "sometable" },
        "dataChangeDetectionPolicy" : { "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy", "highWaterMarkColumnName" : "RowVersion" }, 
        "dataDeletionDetectionPolicy" : { "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy", "softDeleteColumnName" : "IsDeleted", "softDeleteMarkerValue" : "true" }
    }

Jeśli zamierzasz użyć źródła danych dla jednorazowego kopię danych, można pominąć zasad:

    { 
        "name" : "asqldatasource",
        "description" : "anything you want, or nothing at all",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "Server=tcp:....database.windows.net,1433;Database=...;User ID=...;Password=...;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;" },
        "container" : { "name" : "sometable" }
    } 

**Odpowiedź**

Żądania pomyślnego: 201 utworzony. 

<a name="UpdateDataSource"></a>
## <a name="update-data-source"></a>Aktualizacja źródła danych ##

Możesz zaktualizować istniejącego źródła danych przy użyciu żądanie HTTP umieszczenie. Określ nazwę źródła danych, aby zaktualizować żądanie URI:

    PUT https://[service name].search.windows.net/datasources/[datasource name]?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

`api-version` Jest wymagane. Bieżąca wersja jest `2015-02-28`. [Przechowywanie wersji Azure Search](https://msdn.microsoft.com/library/azure/dn864560.aspx) zawiera szczegółowe informacje oraz więcej informacji na temat alternatywne wersje.

`api-key` Musi być kluczem administratora (zamiast klawisza kwerendy). Zapoznaj się z sekcją uwierzytelniania w [Interfejsu API usługi REST Usługa wyszukiwania](https://msdn.microsoft.com/library/azure/dn798935.aspx) , aby dowiedzieć się więcej na temat kluczy. [Tworzenie usługi wyszukiwania w portalu](search-create-service-portal.md) wyjaśniono, jak uzyskać adres URL usługi i kluczowe właściwości używane w wezwaniu.

**Żądanie**

Składnia treści wezwania jest taka sama jak [żądania tworzenie źródła danych](#CreateDataSourceRequestSyntax).

> [AZURE.NOTE] Nie można zaktualizować niektóre właściwości w istniejącym źródle danych. Nie można na przykład zmienić typ istniejącego źródła danych.  

> [AZURE.NOTE] Jeśli nie chcesz zmieniać parametry połączenia dla istniejącego źródła danych, można określić literał `<unchanged>` ciągu połączenia. Jest to pomocne w sytuacji, w której chcesz zaktualizować dane źródłowe, ale nie masz dogodnego dostępu do parametry połączenia, ponieważ jest zależne od zabezpieczeń danych.

**Odpowiedź**

Żądania pomyślnego: 201 utworzony jeśli nowe źródło danych zostało utworzone i 204 Brak zawartości jeżeli została zaktualizowana istniejącego źródła danych.

<a name="ListDataSource"></a>
## <a name="list-data-sources"></a>Lista źródeł danych ##

Operacja **Źródeł danych typu Lista** zwraca listę źródeł danych w usłudze Azure wyszukiwania. 

    GET https://[service name].search.windows.net/datasources?api-version=[api-version]
    api-key: [admin key]

`api-version` Jest wymagane. Bieżąca wersja jest `2015-02-28`. 

`api-key` Musi być kluczem administratora (zamiast klawisza kwerendy). Zapoznaj się z sekcją uwierzytelniania w [Interfejsu API usługi REST Usługa wyszukiwania](https://msdn.microsoft.com/library/azure/dn798935.aspx) , aby dowiedzieć się więcej na temat kluczy. [Tworzenie usługi wyszukiwania w portalu](search-create-service-portal.md) wyjaśniono, jak uzyskać adres URL usługi i kluczowe właściwości używane w wezwaniu.

**Odpowiedź**

Żądania pomyślnego: 200 OK.

Oto przykład treść odpowiedzi:

    {
      "value" : [
        {
          "name": "datasource1",
          "type": "azuresql",
          ... other data source properties
        }]
    }

Należy zauważyć, że można filtrować odpowiedź w dół, aby tylko właściwości, które mogą Cię zainteresować. Na przykład tylko listę nazw źródeł danych, należy użyć OData `$select` kwerendy opcję:

    GET /datasources?api-version=205-02-28&$select=name

W tym przypadku odpowiedzi z powyższego przykładu będzie wyglądać następująco: 

    {
      "value" : [ { "name": "datasource1" }, ... ]
    }

Jest to metoda przydatne, aby zapisać przepustowości, jeśli masz wiele źródeł danych w tej usługi wyszukiwania.

<a name="GetDataSource"></a>
## <a name="get-data-source"></a>Uzyskiwanie źródła danych ##

Operacja **Uzyskiwanie źródła danych** pobiera definicji źródła danych z wyszukiwania Azure.

    GET https://[service name].search.windows.net/datasources/[datasource name]?api-version=[api-version]
    api-key: [admin key]

`api-version` Jest wymagane. Bieżąca wersja jest `2015-02-28`. 

`api-key` Musi być kluczem administratora (zamiast klawisza kwerendy). Zapoznaj się z sekcją uwierzytelniania w [Interfejsu API usługi REST Usługa wyszukiwania](https://msdn.microsoft.com/library/azure/dn798935.aspx) , aby dowiedzieć się więcej na temat kluczy. [Tworzenie usługi wyszukiwania w portalu](search-create-service-portal.md) wyjaśniono, jak uzyskać adres URL usługi i kluczowe właściwości używane w wezwaniu.

**Odpowiedź**

Kod stanu: 200 OK jest zwracana na pomyślne odpowiedź.

Odpowiedź jest podobne do przykładów w [wezwań na przykład utworzyć źródło danych](#CreateDataSourceRequestExamples): 

    { 
        "name" : "asqldatasource",
        "description" : "a description",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "Server=tcp:....database.windows.net,1433;Database=...;User ID=...;Password=...;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;" },
        "container" : { "name" : "sometable" },
        "dataChangeDetectionPolicy" : { 
            "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
            "highWaterMarkColumnName" : "RowVersion" }, 
        "dataDeletionDetectionPolicy" : { 
            "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
            "softDeleteColumnName" : "IsDeleted", 
            "softDeleteMarkerValue" : "true" }
    }

> [AZURE.NOTE] Nie ustawiono `Accept` posługując się `application/json;odata.metadata=none` podczas nawiązywania połączeń z tego interfejsu API jako to spowoduje, że `@odata.type` atrybut można pominąć odpowiedzi, a nie będą mogli rozróżniania zmiany danych oraz zasady wykrywania usunięcie dane różnych typów. 

<a name="DeleteDataSource"></a>
## <a name="delete-data-source"></a>Usuwanie źródła danych ##

Operacja **Usuwania źródła danych** powoduje usunięcie źródła danych z usługi Azure wyszukiwania.

    DELETE https://[service name].search.windows.net/datasources/[datasource name]?api-version=[api-version]
    api-key: [admin key]

> [AZURE.NOTE] Jeśli dowolny indeksatory odwołuje się do źródła danych, którą chcesz usunąć, nadal będzie kontynuować operację usuwania. Jednak tych indeksatory przechodzi w stanie błędu po jego następnym uruchomieniu.  

`api-version` Jest wymagane. Bieżąca wersja jest `2015-02-28`. 

`api-key` Musi być kluczem administratora (zamiast klawisza kwerendy). Zapoznaj się z sekcją uwierzytelniania w [Interfejsu API usługi REST Usługa wyszukiwania](https://msdn.microsoft.com/library/azure/dn798935.aspx) , aby dowiedzieć się więcej na temat kluczy. [Tworzenie usługi wyszukiwania w portalu](search-create-service-portal.md) wyjaśniono, jak uzyskać adres URL usługi i kluczowe właściwości używane w wezwaniu.

**Odpowiedź**

Kod stanu: 204 Brak zawartości jest zwracana na pomyślne odpowiedź.

<a name="CreateIndexer"></a>
## <a name="create-indexer"></a>Tworzenie indeksowania ##

Możesz utworzyć nowy indeksatora w ramach usługi Azure wyszukiwania przy użyciu żądania HTTP POST.
    
    POST https://[service name].search.windows.net/indexers?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

Można również za pomocą funkcji i określ nazwę źródła danych na identyfikator URI. Jeśli źródło danych nie istnieje, zostanie on utworzony.

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=[api-version]

> [AZURE.NOTE] Maksymalna liczba indeksatory dozwolone zależy od ceny warstwy. Bezpłatna usługa umożliwia indeksatory maksymalnie 3. Standardowa usługa umożliwia indeksatory 50. Aby uzyskać szczegółowe informacje, zobacz [Limity dotyczące usługi](search-limits-quotas-capacity.md) .

`api-version` Jest wymagane. Bieżąca wersja jest `2015-02-28`. 

`api-key` Musi być kluczem administratora (zamiast klawisza kwerendy). Zapoznaj się z sekcją uwierzytelniania w [Interfejsu API usługi REST Usługa wyszukiwania](https://msdn.microsoft.com/library/azure/dn798935.aspx) , aby dowiedzieć się więcej na temat kluczy. [Tworzenie usługi wyszukiwania w portalu](search-create-service-portal.md) wyjaśniono, jak uzyskać adres URL usługi i kluczowe właściwości używane w wezwaniu.


<a name="CreateIndexerRequestSyntax"></a>
**Żądanie składni treści**

Treść żądania zawiera definicję indeksatora, który określa źródła danych i indeks docelowej indeksowanie, a także opcjonalne harmonogram indeksowania i parametrów. 


Składnia struktury ładunku żądanie wygląda następująco: Przykładowe żądanie znajduje się bardziej na w tym temacie.

    { 
        "name" : "Required for POST, optional for PUT. The name of the indexer",
        "description" : "Optional. Anything you want, or null",
        "dataSourceName" : "Required. The name of an existing data source",
        "targetIndexName" : "Required. The name of an existing index",
        "schedule" : { Optional. See Indexing Schedule below. },
        "parameters" : { Optional. See Indexing Parameters below. },
        "fieldMappings" : { Optional. See Field Mappings below. },
        "disabled" : Optional boolean value indicating whether the indexer is disabled. False by default.  
    }

**Harmonogram indeksowania**

Indeksowanie Opcjonalnie można określić harmonogram. Jeśli ma zgodnie z harmonogramem, indeksatora uruchomi okresowo zgodnie z harmonogramem. Harmonogram ma następujące atrybuty:

- `interval`: Wymagane. Wartość czasu trwania, która określa zakres lub termin indeksatora działa. Najmniejsza dopuszczalna interwałem jest 5 minut; najdłuższej jest jeden dzień. Musi być sformatowany jako wartość "dayTimeDuration" XSD (ograniczony podzbiór wartość [czasu trwania ISO 8601](http://www.w3.org/TR/xmlschema11-2/#dayTimeDuration) ). Deseń to: `"P[nD][T[nH][nM]]"`. Przykłady: `PT15M` dla co 15 minut, `PT2H` dla co dwie godziny. 

- `startTime`: Wymagane. Daty/godziny UTC po indeksatora powinna rozpoczynać się uruchomiony. 

**Parametry indeksowania**

Indeksowanie Opcjonalnie można określić kilka parametrów, które mają wpływ na działanie. Wszystkie parametry są opcjonalne.  

- `maxFailedItems`: Liczba elementów, które może się nie powieść mają być indeksowane przed Uruchom indeksatora jest traktowany jako błąd. Domyślnie wynosi 0. Informacje o niepowodzeniu elementów został zwrócony przez operację [Pobierz stan indeksowania](#GetIndexerStatus) . 

- `maxFailedItemsPerBatch`: Liczbę elementów, które może nie być indeksowane w każdej partii przed Uruchom indeksatora jest traktowany jako błąd. Domyślnie wynosi 0.

- `base64EncodeKeys`: Określa, czy klawisze dokument będzie kodowanie base-64. Wyszukiwanie Azure nakłada ograniczenia na znaki, które mogą występować w kluczu dokumentu. Jednak wartości w źródle danych może zawierać znaki, które są nieprawidłowe. Jeśli jest to konieczne do indeksowania takie wartości jako klucze dokumentu, ta flaga można ustawić na wartość true. Wartością domyślną jest `false`.

- `batchSize`: Określa liczbę elementów, które przeczytaj ze źródła danych i indeksowane jako pojedynczej partii w celu zwiększenia wydajności. Wartość domyślna zależy od typu źródła danych: 1000 Azure SQL i DocumentDB i 10 dla magazyn obiektów Blob platformy Azure.

**Mapowania pól**

Mapowania pól umożliwia mapowanie nazwy pola w źródle danych na inną nazwę pola w indeksie docelowej. Na przykład można rozważyć tabeli źródłowej z polem `_id`. Azure wyszukiwania nie umożliwia nazwę pola, zaczynając od znaku podkreślenia, należy zmienić nazwę pola. Można to zrobić przy użyciu `fieldMappings` właściwości indeksatora w następujący sposób: 
    
    "fieldMappings" : [ { "sourceFieldName" : "_id", "targetFieldName" : "id" } ] 

Można określić wiele mapowania pól: 

    "fieldMappings" : [ 
        { "sourceFieldName" : "_id", "targetFieldName" : "id" },
        { "sourceFieldName" : "_timestamp", "targetFieldName" : "timestamp" },
     ]

Nazwy pól zarówno źródłowej i docelowej wielkość liter.

<a name="FieldMappingFunctions"></a>
***Funkcje mapowania pola***

Można także mapowania pól do przekształcenia wartości pól źródłowych przy użyciu *funkcji mapowania*.

Tylko jeden takiej funkcji jest obecnie obsługiwane: `jsonArrayToStringCollection`. Analizuje pola zawierającego ciąg sformatowany jako tablicę JSON w polu Collection(Edm.String) w indeksie docelowej. Ma na celu używanej w połączeniu z indeksatora Azure SQL w szczególności, ponieważ SQL nie ma typu natywnych zbioru danych. Mogą być używane w następujący sposób: 

    "fieldMappings" : [ { "sourceFieldName" : "tags", "mappingFunction" : { "name" : "jsonArrayToStringCollection" } } ] 

Na przykład, jeśli pole źródłowe zawiera ciąg `["red", "white", "blue"]`, następnie pola docelowego typu `Collection(Edm.String)` zostanie wypełniona trzy wartości `"red"`, `"white"` i `"blue"`.

Należy zauważyć, że `targetFieldName` właściwość jest opcjonalna; Jeśli w lewo, `sourceFieldName` jest używana wartość. 

<a name="CreateIndexerRequestExamples"></a>
**Przykłady treści wezwania**

Poniższy przykład tworzy indeksatora kopiuje dane z tabeli odwołuje się `ordersds` źródła danych do `orders` indeksu zgodnie z harmonogramem, który rozpoczyna się 1 stycznia 2015 r UTC i uruchamia co godzinę. Każdego wywołania indeksatora zakończy się pomyślnie, jeśli nie więcej niż 5 elementów nie mają być indeksowane w każdej partii, a nie więcej niż 10 elementów nie mają być indeksowane w sumie. 

    {
        "name" : "myindexer",
        "description" : "a cool indexer",
        "dataSourceName" : "ordersds",
        "targetIndexName" : "orders",
        "schedule" : { "interval" : "PT1H", "startTime" : "2015-01-01T00:00:00Z" },
        "parameters" : { "maxFailedItems" : 10, "maxFailedItemsPerBatch" : 5, "base64EncodeKeys": false }
    }

**Odpowiedź**

201 utworzono żądania pomyślnie.


<a name="UpdateIndexer"></a>
## <a name="update-indexer"></a>Aktualizowanie indeksowania ##

Możesz zaktualizować indeksatora istniejących przy użyciu żądanie HTTP umieszczenie. Określ nazwę indeksatora, aby zaktualizować na identyfikator URI żądania:

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

`api-version` Jest wymagane. Bieżąca wersja jest `2015-02-28`. 

`api-key` Musi być kluczem administratora (zamiast klawisza kwerendy). Zapoznaj się z sekcją uwierzytelniania w [Interfejsu API usługi REST Usługa wyszukiwania](https://msdn.microsoft.com/library/azure/dn798935.aspx) , aby dowiedzieć się więcej na temat kluczy. [Tworzenie usługi wyszukiwania w portalu](search-create-service-portal.md) wyjaśniono, jak uzyskać adres URL usługi i kluczowe właściwości używane w wezwaniu.

**Żądanie**

Składnia treści wezwania jest taka sama jak [Tworzenie indeksatora żądania](#CreateIndexerRequestSyntax).

**Odpowiedź**

Żądania pomyślnego: 201 utworzony w przypadku nowych indeksatora utworzone i 204 Brak zawartości jeśli zaktualizowano istniejące indeksowania.


<a name="ListIndexers"></a>
## <a name="list-indexers"></a>Indeksatory listy ##

Operacja **Indeksatory listy** zwraca listę indeksatory w usłudze Azure wyszukiwania. 

    GET https://[service name].search.windows.net/indexers?api-version=[api-version]
    api-key: [admin key]


`api-version` Jest wymagane. Wersja jest `2015-02-28-Preview`. [Przechowywanie wersji Azure Search](https://msdn.microsoft.com/library/azure/dn864560.aspx) zawiera szczegółowe informacje oraz więcej informacji na temat alternatywne wersje.

`api-key` Musi być kluczem administratora (zamiast klawisza kwerendy). Zapoznaj się z sekcją uwierzytelniania w [Interfejsu API usługi REST Usługa wyszukiwania](https://msdn.microsoft.com/library/azure/dn798935.aspx) , aby dowiedzieć się więcej na temat kluczy. [Tworzenie usługi wyszukiwania w portalu](search-create-service-portal.md) wyjaśniono, jak uzyskać adres URL usługi i kluczowe właściwości używane w wezwaniu.

**Odpowiedź**

Żądania pomyślnego: 200 OK.

Oto przykład treść odpowiedzi:

    {
      "value" : [
      {
        "name" : "myindexer",
        "description" : "a cool indexer",
        "dataSourceName" : "ordersds",
        "targetIndexName" : "orders",
        ... other indexer properties
      }]
    }

Należy zauważyć, że można filtrować odpowiedź w dół, aby tylko właściwości, które mogą Cię zainteresować. Na przykład tylko listę nazw indeksatora, należy użyć OData `$select` kwerendy opcję:

    GET /indexers?api-version=2014-10-20-Preview&$select=name

W tym przypadku odpowiedzi z powyższego przykładu będzie wyglądać następująco: 

    {
      "value" : [ { "name": "myindexer" } ]
    }

Jest to metoda przydatne, aby zapisać przepustowości, jeśli masz wiele indeksatory w tej usługi wyszukiwania.


<a name="GetIndexer"></a>
## <a name="get-indexer"></a>Uzyskiwanie indeksowania ##

**Uzyskiwanie** indeksatora otrzymuje definicji indeksatora z wyszukiwania Azure.

    GET https://[service name].search.windows.net/indexers/[indexer name]?api-version=[api-version]
    api-key: [admin key]

`api-version` Jest wymagane. Wersja jest `2015-02-28-Preview`. 

`api-key` Musi być kluczem administratora (zamiast klawisza kwerendy). Zapoznaj się z sekcją uwierzytelniania w [Interfejsu API usługi REST Usługa wyszukiwania](https://msdn.microsoft.com/library/azure/dn798935.aspx) , aby dowiedzieć się więcej na temat kluczy. [Tworzenie usługi wyszukiwania w portalu](search-create-service-portal.md) wyjaśniono, jak uzyskać adres URL usługi i kluczowe właściwości używane w wezwaniu.

**Odpowiedź**

Kod stanu: 200 OK jest zwracana na pomyślne odpowiedź.

Odpowiedź jest podobne do przykładów w [wezwań na przykład utworzyć indeksatora](#CreateIndexerRequestExamples): 

    {
        "name" : "myindexer",
        "description" : "a cool indexer",
        "dataSourceName" : "ordersds",
        "targetIndexName" : "orders",
        "schedule" : { "interval" : "PT1H", "startTime" : "2015-01-01T00:00:00Z" },
        "parameters" : { "maxFailedItems" : 10, "maxFailedItemsPerBatch" : 5, "base64EncodeKeys": false }
    }


<a name="DeleteIndexer"></a>
## <a name="delete-indexer"></a>Usuwanie indeksowania ##

**Usuwanie** indeksatora usuwa indeksatora z usługi Azure wyszukiwania.

    DELETE https://[service name].search.windows.net/indexers/[indexer name]?api-version=[api-version]
    api-key: [admin key]

Po usunięciu indeksatora wykonania indeksatora w toku w danym momencie, spowoduje uruchomienie do ukończenia, ale nie dalszej wykonania są planowane. Prób użycia indeksatora nieistniejącego spowoduje kodu stanu HTTP 404 Nie można odnaleźć. 
 
`api-version` Jest wymagane. Wersja jest `2015-02-28-Preview`. 

`api-key` Musi być kluczem administratora (zamiast klawisza kwerendy). Zapoznaj się z sekcją uwierzytelniania w [Interfejsu API usługi REST Usługa wyszukiwania](https://msdn.microsoft.com/library/azure/dn798935.aspx) , aby dowiedzieć się więcej na temat kluczy. [Tworzenie usługi wyszukiwania w portalu](search-create-service-portal.md) wyjaśniono, jak uzyskać adres URL usługi i kluczowe właściwości używane w wezwaniu.

**Odpowiedź**

Kod stanu: 204 Brak zawartości jest zwracana na pomyślne odpowiedź.

<a name="RunIndexer"></a>
## <a name="run-indexer"></a>Uruchamianie indeksowania ##

Oprócz uruchomiony okresowo, zgodnie z harmonogramem, indeksatora mogą być wywoływane na żądanie za pośrednictwem indeksatora **Uruchamianie** : 

    POST https://[service name].search.windows.net/indexers/[indexer name]/run?api-version=[api-version]
    api-key: [admin key]

`api-version` Jest wymagane. Wersja jest `2015-02-28-Preview`. 

`api-key` Musi być kluczem administratora (zamiast klawisza kwerendy). Zapoznaj się z sekcją uwierzytelniania w [Interfejsu API usługi REST Usługa wyszukiwania](https://msdn.microsoft.com/library/azure/dn798935.aspx) , aby dowiedzieć się więcej na temat kluczy. [Tworzenie usługi wyszukiwania w portalu](search-create-service-portal.md) wyjaśniono, jak uzyskać adres URL usługi i kluczowe właściwości używane w wezwaniu.

**Odpowiedź**

Kod stanu: 202 zaakceptowane jest zwracana na pomyślne odpowiedź.

<a name="GetIndexerStatus"></a>
## <a name="get-indexer-status"></a>Pobierz stan indeksowania ##

Operacja **Pobierz stan indeksowania** pobiera bieżący stan i wykonanie historii indeksatora: 

    GET https://[service name].search.windows.net/indexers/[indexer name]/status?api-version=[api-version]
    api-key: [admin key]


`api-version` Jest wymagane. Wersja jest `2015-02-28-Preview`. 

`api-key` Musi być kluczem administratora (zamiast klawisza kwerendy). Zapoznaj się z sekcją uwierzytelniania w [Interfejsu API usługi REST Usługa wyszukiwania](https://msdn.microsoft.com/library/azure/dn798935.aspx) , aby dowiedzieć się więcej na temat kluczy. [Tworzenie usługi wyszukiwania w portalu](search-create-service-portal.md) wyjaśniono, jak uzyskać adres URL usługi i kluczowe właściwości używane w wezwaniu.

**Odpowiedź**

Kod stanu: 200 OK pomyślną odpowiedź.

Treść odpowiedzi zawiera informacje dotyczące ogólnego stanu kondycji indeksatora, ostatniego wywołania indeksatora, a także historii ostatnich wywołania indeksatora, (jeśli istnieje). 

Treść odpowiedzi przykładowe wygląda następująco: 

    {
        "status":"running",
        "lastResult": {
            "status":"success",
            "errorMessage":null,
            "startTime":"2014-11-26T03:37:18.853Z",
            "endTime":"2014-11-26T03:37:19.012Z",
            "errors":[],
            "itemsProcessed":11,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
         },
        "executionHistory":[ {
            "status":"success",
            "errorMessage":null,
            "startTime":"2014-11-26T03:37:18.853Z",
            "endTime":"2014-11-26T03:37:19.012Z",
            "errors":[],
            "itemsProcessed":11,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
        }]
    }

**Stan indeksowania**

Stan indeksowania może mieć jedną z następujących wartości:

- `running`Wskazuje, że indeksatora działa normalnie. Zauważ, że nadal mogą część wykonania indeksatora nie, dlatego warto sprawdzić `lastResult` także właściwości. 

- `error`Wskazuje, że indeksatora wystąpił błąd, których nie można rozwiązać bez udziału rozmówcy. Na przykład wygasł poświadczeń źródła danych lub schemat źródła danych lub indeksu docelowej uległ zmianie w ostatniej chwili sposób. 

**Wynik wykonanie indeksowania**

Wynik wykonanie indeksatora zawiera informacje o wykonanie jednej indeksowania. Najnowsze wynik jest wyświetlana jako `lastResult` właściwości stan indeksowania. Inne ostatnio używane, jeśli istnieją, zwracane wyniki są liczbami `executionHistory` właściwości stan indeksowania. 

Wynik wykonanie indeksatora zawiera następujące właściwości: 

- `status`: stan realizacji. Aby uzyskać szczegółowe informacje, zobacz [Stan wykonanie indeksatora](#IndexerExecutionStatus) poniżej. 

- `errorMessage`: komunikat o błędzie wykonywania nie powiodło się. 

- `startTime`: godziny w czasie UTC Rozpoczęcie to wykonanie.

- `endTime`: godziny w czasie UTC po zakończeniu to wykonanie. Ta wartość nie jest ustawiona, jeśli wykonanie jest nadal w toku.

- `errors`: tablica błędów na poziomie elementu, jeśli istnieją. Każdy wpis zawiera klucz dokumentu (`key` właściwości) i komunikat o błędzie (`errorMessage` właściwości). 

- `itemsProcessed`: liczba źródła danych elementów (na przykład wiersze tabeli), które indeksatora próby indeksu podczas wykonywania tego. 

- `itemsFailed`: liczbę elementów, które to wykonanie nie powiodło się.  
 
- `initialTrackingState`: zawsze `null` pierwszego wykonywania indeksatora, lub zmiana zasad śledzenie danych nie jest włączona w źródle danych używane. Po włączeniu tych zasad tę wartość w kolejnych wykonania wskazuje pierwszy śledzenia wartość przetwarzanych przez to wykonanie zmian (najniższej). 

- `finalTrackingState`: zawsze `null` zmiana zasad śledzenie danych nie jest włączona w źródle danych używane. W przeciwnym razie wskazuje śledzenia wartość pomyślnie przetworzona przez to wykonanie najnowszych zmian (najwyższy). 

<a name="IndexerExecutionStatus"></a>
**Stan wykonania indeksowania**

Stan wykonanie indeksatora przechwytuje stanu wykonanie jednej indeksowania. Może mieć następujące wartości:

- `success`Wskazuje, wykonanie indeksatora została pomyślnie ukończona.

- `inProgress`Wskazuje, że wykonanie indeksatora jest w toku. 

- `transientFailure`Wskazuje, że wykonanie indeksatora nie powiodła się. Zobacz `errorMessage` właściwości, aby uzyskać szczegółowe informacje. Błąd może lub nie może wymagać interwencja rozwiązać problem — na przykład rozwiązywanie niezgodności schematów między źródła danych i indeks docelowej wymaga akcji użytkownika, a przestoje źródła danych tymczasowych nie. Wywołania indeksatora będzie według harmonogramu, jeśli jest dostępny. 

- `persistentFailure`Wskazuje, że indeksatora nie powiodła się w sposób, który wymaga interwencja. Zatrzymaj wykonania indeksatora według harmonogramu. Po adresowania ten problem, należy użyć resetowanie interfejsu API indeksatora ponowne wykonania według harmonogramu. 

- `reset`Wskazuje, że indeksatora zostało zresetowane, numer telefonu, aby zresetować indeksatora interfejsu API (patrz poniżej). 

<a name="ResetIndexer"></a>
## <a name="reset-indexer"></a>Resetowanie indeksowania ##

**Resetowanie** indeksatora resetuje stan skojarzone z indeksatora śledzenia zmian. Dzięki temu można wyzwalać od podstaw ponownego indeksowania (na przykład schemat źródła danych został zmieniony) lub zmienić zasad wykrywania zmiany danych w źródle danych skojarzone z indeksatora.   

    POST https://[service name].search.windows.net/indexers/[indexer name]/reset?api-version=[api-version]
    api-key: [admin key]

`api-version` Jest wymagane. Wersja jest `2015-02-28-Preview`. 

`api-key` Musi być kluczem administratora (zamiast klawisza kwerendy). Zapoznaj się z sekcją uwierzytelniania w [Interfejsu API usługi REST Usługa wyszukiwania](https://msdn.microsoft.com/library/azure/dn798935.aspx) , aby dowiedzieć się więcej na temat kluczy. [Tworzenie usługi wyszukiwania w portalu](search-create-service-portal.md) wyjaśniono, jak uzyskać adres URL usługi i kluczowe właściwości używane w wezwaniu.

**Odpowiedź**

Kodu stanu: 204 Brak zawartości na pomyślne odpowiedź.

## <a name="mapping-between-sql-data-types-and-azure-search-data-types"></a>Mapowanie między Azure wyszukiwania typy danych i typy danych języka SQL ##

<table style="font-size:12">
<tr>
<td>Typ danych SQL</td>  
<td>Dozwolone indeks docelowej typy pól</td>
<td>Notatki</td>
</tr>
<tr>
<td>bit</td>
<td>Edm.Boolean, Edm.String</td>
<td></td>
</tr>
<tr>
<td>int, smallint, tinyint</td>
<td>Edm.String Edm.Int32, Edm.Int64,</td>
<td></td>
</tr>
<tr>
<td>bigint</td>
<td>Edm.Int64, Edm.String</td>
<td></td>
</tr>
<tr>
<td>liczba rzeczywista, ruchomości</td>
<td>Edm.Double, Edm.String</td>
<td></td>
</tr>
<tr>
<td>Smallmoney, pieniądze<br>dziesiętna<br>liczbowe
</td>
<td>Edm.String</td>
<td>Ponieważ spowoduje to utratę dokładności konwertowania typów dziesiętną Edm.Double nie obsługuje Azure wyszukiwania
</td>
</tr>
<tr>
<td>znak, nchar, varchar, nvarchar</td>
<td>Edm.String<br/>Collection(EDM.String)</td>
<td>Zobacz [Funkcje mapowania pól](#FieldMappingFunctions) w tym dokumencie, aby uzyskać szczegółowe informacje na temat kolumny ciąg umożliwia przekształcenie Collection(Edm.String)</td>
</tr>
<tr>
<td>smalldatetime, daty i godziny, datetime2, Data, datetimeoffset</td>
<td>Edm.DateTimeOffset, Edm.String</td>
<td></td>
</tr>
<tr>
<td>uniqueidentifer</td>
<td>Edm.String</td>
<td></td>
</tr>
<tr>
<td>geograficznych</td>
<td>Edm.GeographyPoint</td>
<td>Obsługiwane są tylko Geografia wystąpienia typu punkt z 4326 SRID, (jest to ustawienie domyślne)</td>
</tr>
<tr>
<td>ROWVERSION</td>
<td>N/D!</td>
<td>Kolumny wersji wiersza nie mogą być przechowywane w indeksie wyszukiwania, ale może służyć do śledzenia zmian</td>
</tr>
<tr>
<td>czas, przedziału czasu<br>format binarny, zmiennej liczby dwójkowej, obraz<br>Kod XML, geometria typy CLR</td>
<td>N/D!</td>
<td>Brak obsługi</td>
</tr>
</table>

## <a name="mapping-between-json-data-types-and-azure-search-data-types"></a>Mapowanie między JSON typów danych i typy danych Azure wyszukiwania ##

<table style="font-size:12">
<tr>
<td>Typ danych JSON</td> 
<td>Dozwolone indeks docelowej typy pól</td>
<td>Notatki</td>
</tr>
<tr>
<td>wartość logiczna</td>
<td>Edm.Boolean, Edm.String</td>
<td></td>
</tr>
<tr>
<td>Integralną liczb</td>
<td>Edm.String Edm.Int32, Edm.Int64,</td>
<td></td>
</tr>
<tr>
<td>Liczby zmiennoprzecinkowe</td>
<td>Edm.Double, Edm.String</td>
<td></td>
</tr>
<tr>
<td>ciąg</td>
<td>Edm.String</td>
<td></td>
</tr>
<tr>
<td>typy tablic pierwotny, np. ["a", "b", "c"]</td>
<td>Collection(EDM.String)</td>
<td></td>
</tr>
<tr>
<td>Ciągi, które wyglądają daty</td>
<td>Edm.DateTimeOffset, Edm.String</td>
<td></td>
</tr>
<tr>
<td>Obiekty punktu GeoJSON</td>
<td>Edm.GeographyPoint</td>
<td>Punkty GeoJSON są JSON obiektów w następującym formacie: {"typ": "Punkt", "współrzędne": [czas, lat]} </td>
</tr>
<tr>
<td>Inne obiekty JSON</td>
<td>N/D!</td>
<td>Nieobsługiwane. Wyszukiwanie Azure obsługuje obecnie tylko typy pierwotne i zbiorów ciągu</td>
</tr>
</table>