<properties
   pageTitle="Opracowywanie z wielu regionów w DocumentDB | Microsoft Azure"
   description="Dowiedz się, jak korzystać z danych w wielu regionach DocumentDB Azure, w pełni zarządzane usługi bazy danych NoSQL."
   services="documentdb"
   documentationCenter=""
   authors="kiratp"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="documentdb"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/25/2016"
   ms.author="kipandya"/>
   
# <a name="developing-with-multi-region-documentdb-accounts"></a>Opracowywanie z kontami DocumentDB wielu regionu

> [AZURE.NOTE] Globalne rozkład DocumentDB baz danych jest zazwyczaj dostępny i automatycznie włączone dla wszystkich nowo utworzonych kont DocumentDB. Pracujemy nad włączyć globalnego rozkładu dla wszystkich istniejących kont, ale w międzyczasie ma być globalnym rozkładu włączone dla swojego konta, należy [kontakt z pomocą techniczną](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) i firma Microsoft będzie ją włączyć można teraz.

Aby móc skorzystać z [globalnej rozkład](documentdb-distribute-data-globally.md), aplikacje klienckie można określić Lista numerowana preferencji regionów może być używany do wykonywania operacji dokumentu. Można to zrobić przez ustawienie zasad połączenia. Na podstawie konfiguracji konta Azure DocumentDB, bieżącą dostępność regionalne i na liście preferencji określonych, najbardziej optymalny punkt końcowy zostanie wybrany przez zestaw SDK do wykonywania zapisu i operacji odczytu. 

Ta lista preferencji jest określona podczas inicjowania połączenia przy użyciu klienta DocumentDB SDK. SDK zaakceptować opcjonalny parametr "PreferredLocations" to jest uporządkowana lista regionów Azure.

Zestaw SDK wyśle automatycznie zapisuje wszystkie do bieżącego zapisu region. 

Wszystkie operacje odczytu będą wysyłane do pierwszej region dostępnych na liście PreferredLocations. Jeśli żądanie nie powiedzie się, klient się nie powieść w dół na liście, aby następny region i tak dalej. 

Klient, który tylko spróbuje SDK odczyt regionów określonych w PreferredLocations. Tak na przykład jeśli konto bazy danych znajduje się w trzech regionów, ale klienta tylko określa dwóch regionów i zapisu PreferredLocations, następnie nie odczytuje będzie udostępniania obszaru zapisu, nawet w przypadku pracy awaryjnej.

Aplikacja można sprawdzić bieżącego punktu końcowego zapisu, a następnie przeczytaj punkt końcowy wybrane przez zestaw SDK, zaznaczając pole wyboru dwie właściwości WriteEndpoint i ReadEndpoint, dostępne w wersji SDK 1.8 i powyżej. 

Jeśli właściwość PreferredLocations nie jest ustawiona, wszystkie żądania będą obsługiwane z bieżącego obszaru zapisu. 


## <a name="net-sdk"></a>ZESTAW SDK PROGRAMU .NET
Zestaw SDK może być używany bez żadnych zmian kodu. W tym przypadku zestawu SDK kieruje obu odczytuje i automatycznie zapisuje bieżący obszar zapisu. 

W wersji 1.8 i później .NET SDK parametr ConnectionPolicy dla konstruktora DocumentClient ma właściwość o nazwie Microsoft.Azure.Documents.ConnectionPolicy.PreferredLocations. Ta właściwość jest typu zbioru `<string>` i powinny zawierać listę nazw region. Wartości ciągu zostały sformatowane w kolumnie Nazwa regionu [Regionów Azure]  [ regions] strony, bez spacji przed i po pierwszym i ostatni znak odpowiednio.

Bieżący zapisu i odczytu punkty końcowe są odpowiednio dostępne w DocumentClient.WriteEndpoint i DocumentClient.ReadEndpoint.

> [AZURE.NOTE] Adresy URL dla punktów końcowych nie należy uwzględnić w stałych długotrwałe. Usługa może aktualizować w dowolnym momencie. Zestaw SDK automatycznie obsługuje tę zmianę.

    // Getting endpoints from application settings or other configuration location
    Uri accountEndPoint = new Uri(Properties.Settings.Default.GlobalDatabaseUri);
    string accountKey = Properties.Settings.Default.GlobalDatabaseKey;

    //Setting read region selection preference 
    connectionPolicy.PreferredLocations.Add(LocationNames.WestUS); // first preference
    connectionPolicy.PreferredLocations.Add(LocationNames.EastUS); // second preference
    connectionPolicy.PreferredLocations.Add(LocationNames.NorthEurope); // third preference

    // initialize connection
    DocumentClient docClient = new DocumentClient(
        accountEndPoint,
        accountKey,
        connectionPolicy);

    // connect to DocDB 
    await docClient.OpenAsync().ConfigureAwait(false);


## <a name="nodejs-javascript-and-python-sdks"></a>NodeJS, JavaScript i Python SDK
Zestaw SDK może być używany bez żadnych zmian kodu. W tym przypadku zestawu SDK zostanie automatycznie przekierowany, zarówno odczytu i zapisu do bieżącego zapisu region. 

W wersji 1.8 lub nowszy zestawu SDK dla każdego parametru ConnectionPolicy dla konstruktora DocumentClient nowej właściwości nazywane DocumentClient.ConnectionPolicy.PreferredLocations. To jest parametr jest tablica ciągów przyjmuje listę nazwy regionów. Imiona i nazwiska są formatowane w kolumnie Nazwa regionu w [Regionach Azure]  [ regions] strony. Można także użyć wstępnie zdefiniowanych stałych w obiekt wygody AzureDocuments.Regions

Bieżący zapisu i odczytu punkty końcowe są odpowiednio dostępne w DocumentClient.getWriteEndpoint i DocumentClient.getReadEndpoint.

> [AZURE.NOTE] Adresy URL dla punktów końcowych nie należy uwzględnić w stałych długotrwałe. Usługa może aktualizować w dowolnym momencie. Zestaw SDK będą automatycznie obsługiwać tę zmianę.

Poniżej znajduje przykład kodu dla NodeJS i Javascript. Python i Java będą zgodne z takim samym wzorcem.

    // Creating a ConnectionPolicy object
    var connectionPolicy = new DocumentBase.ConnectionPolicy();
    
    // Setting read region selection preference, in the following order -
    // 1 - West US
    // 2 - East US
    // 3 - North Europe
    connectionPolicy.PreferredLocations = ['West US', 'East US', 'North Europe'];
    
    // initialize the connection
    var client = new DocumentDBClient(host, { masterKey: masterKey }, connectionPolicy);


## <a name="rest"></a>POZOSTAŁE 
Gdy konto bazy danych został udostępniony w wielu regionów, klienci mogą wysyłać kwerendy ich dostępność, wykonując żądania GET następujące identyfikatora URI.

    https://{databaseaccount}.documents.azure.com/

Usługa zwróci listę regionów i ich odpowiednich DocumentDB punkt końcowy identyfikatory URI dla repliki. Bieżący obszar zapisu będą wyświetlane w odpowiedzi. Klienta można wybrać odpowiedni punkt końcowy dla wszystkie kolejne żądania interfejsu API usługi REST w następujący sposób.

Przykład odpowiedzi

    {
        "_dbs": "//dbs/",
        "media": "//media/",
        "writableLocations": [
            {
                "Name": "West US",
                "DatabaseAccountEndpoint": "https://globaldbexample-westus.documents.azure.com:443/"
            }
        ],
        "readableLocations": [
            {
                "Name": "East US",
                "DatabaseAccountEndpoint": "https://globaldbexample-eastus.documents.azure.com:443/"
            }
        ],
        "MaxMediaStorageUsageInMB": 2048,
        "MediaStorageUsageInMB": 0,
        "ConsistencyPolicy": {
            "defaultConsistencyLevel": "Session",
            "maxStalenessPrefix": 100,
            "maxIntervalInSeconds": 5
        },
        "addresses": "//addresses/",
        "id": "globaldbexample",
        "_rid": "globaldbexample.documents.azure.com",
        "_self": "",
        "_ts": 0,
        "_etag": null
    }


-   Przejdź do zapisu wskazanej identyfikatora URI żądania położenie, OPUBLIKUJ i Usuń
-   Pobiera wszystkie i inne żądania tylko do odczytu (na przykład zapytania) może prowadzić do dowolnego punktu końcowego wybranych przez klienta

Napisz żądania w trybie tylko do odczytu regionach zakończy się niepowodzeniem z kodem błędu HTTP 403 ("Dostęp zabroniony").

Zmiana regionu zapisu po etapu początkowego odnajdowanie klienta kolejnych zapisów do poprzedniej sekcji zapisu zakończy się niepowodzeniem z kodem błędu HTTP 403 ("Dostęp zabroniony"). Klient następnie należy UZYSKAĆ listę regionów ponownie, aby uzyskać regionu zaktualizowane zapisu.

## <a name="next-steps"></a>Następne kroki

Dowiedz się więcej na temat dystrybucji danych globalnie z DocumentDB w następujących artykułach:

- [Rozpowszechnianie danych globalnie za pomocą DocumentDB](documentdb-distribute-data-globally.md)
- [Poziomy zgodności](documentdb-consistency-levels.md)
- [Jak działa przepustowości z wielu regionów](documentdb-manage.md#how-throughput-works-with-multiple-regions)
- [Dodawanie regionów za pomocą portalu Azure](documentdb-portal-global-replication.md)

[regions]: https://azure.microsoft.com/regions/ 
