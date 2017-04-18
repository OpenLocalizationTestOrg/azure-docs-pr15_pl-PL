<properties 
   pageTitle="Wprowadzenie do analiz Lake danych za pomocą interfejsu API usługi REST | Microsoft Azure" 
   description="Używanie interfejsów API pozostałych WebHDFS do wykonywania operacji na danych Lake analizy" 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="mumian" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="10/19/2016"
   ms.author="jgao"/>

# <a name="get-started-with-azure-data-lake-analytics-using-rest-apis"></a>Wprowadzenie do analizy Lake danych Azure za pomocą interfejsów API REST

[AZURE.INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Dowiedz się, jak umożliwia zarządzanie kontami, zadań i wykaz danych Lake analizy danych Lake analizy pozostałych API i interfejsów API pozostałych WebHDFS. 

## <a name="prerequisites"></a>Wymagania wstępne

- **Azure subskrypcji**. Zobacz [Azure pobrać bezpłatną wersję próbną](https://azure.microsoft.com/pricing/free-trial/).

- **Tworzenie aplikacji usługi Azure Active Directory**. Za pomocą aplikacji Azure AD uwierzytelniania aplikacji analizy Lake danych z usługą Azure Active Directory. Istnieją różne metody do uwierzytelniania Azure AD, które są **uwierzytelniania użytkowników końcowych** lub **do usługi**. Instrukcje i uzyskać więcej informacji na temat uwierzytelniania Zobacz [uwierzytelniania analiz Lake danych za pomocą usługi Azure Active Directory](../data-lake-store/data-lake-store-authenticate-using-active-directory.md).

- [zawinięcie](http://curl.haxx.se/). W tym artykule Umożliwia zwinięcie przedstawiają sposoby nawiązywania połączeń interfejsu API usługi REST do konta analizy Lake danych.

## <a name="authenticate-with-azure-active-directory"></a>Uwierzytelnianie z usługą Azure Active Directory

Istnieją dwie metody uwierzytelniania z usługą Azure Active Directory.

### <a name="end-user-authentication-interactive"></a>Uwierzytelnianie użytkownika końcowego (interakcyjne)

Ta metoda aplikacji monituje użytkownika, aby zalogować się i wszystkie operacje są wykonywane w kontekście użytkownika. 

Wykonaj poniższe czynności uwierzytelniania interakcyjne:

1. Za pomocą aplikacji przekierować użytkownika do następujący adres URL:

        https://login.microsoftonline.com/<TENANT-ID>/oauth2/authorize?client_id=<CLIENT-ID>&response_type=code&redirect_uri=<REDIRECT-URI>

    >[AZURE.NOTE] \<Identyfikator URI PRZEKIERUJ > musi być zakodowany do użycia w adresie URL. Tak, aby https://localhost, użyj `https%3A%2F%2Flocalhost`)

    Do tego samouczka można zamienić wartości symbol zastępczy w adresie URL powyżej i wklej je na pasku adresu przeglądarki sieci web. Nastąpi przekierowanie do uwierzytelniania za pomocą usługi Azure logowania. Po pomyślnie możesz się zalogować, odpowiedź jest wyświetlane na pasku adresu przeglądarki. Odpowiedź będzie w następującym formacie:
        
        http://localhost/?code=<AUTHORIZATION-CODE>&session_state=<GUID>

2. Rejestrowanie kodu autoryzacji z odpowiedzi. Ten samouczek możesz skopiować kod autoryzacji na pasku adresu przeglądarki sieci web i przekazać je w wezwaniu na WPIS token punktu końcowego, tak jak pokazano poniżej:

        curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token \
        -F redirect_uri=<REDIRECT-URI> \
        -F grant_type=authorization_code \
        -F resource=https://management.core.windows.net/ \
        -F client_id=<CLIENT-ID> \
        -F code=<AUTHORIZATION-CODE>

    >[AZURE.NOTE] W tym przypadku \<identyfikatora URI PRZEKIERUJ > nie muszą być kodowane.

3. Odpowiedź jest obiektem JSON, który zawiera token dostępu (np. `"access_token": "<ACCESS_TOKEN>"`) i token odświeżania (np `"refresh_token": "<REFRESH_TOKEN>"`). Aplikacja używa token dostępu podczas uzyskiwania dostępu do magazynu Lake danych Azure i token odświeżania uzyskać inny token dostępu po wygaśnięciu token dostępu.

        {"token_type":"Bearer","scope":"user_impersonation","expires_in":"3599","expires_on":"1461865782","not_before": "1461861882","resource":"https://management.core.windows.net/","access_token":"<REDACTED>","refresh_token":"<REDACTED>","id_token":"<REDACTED>"}

4.  Po wygaśnięciu token dostępu, możesz przejąć tokenu dostępu przy użyciu tokenu odświeżania, tak jak pokazano poniżej:

         curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
            -F grant_type=refresh_token \
            -F resource=https://management.core.windows.net/ \
            -F client_id=<CLIENT-ID> \
            -F refresh_token=<REFRESH-TOKEN>
 
Aby uzyskać więcej informacji o uwierzytelnianiu użytkowników interakcyjnych zobacz [kodu autoryzacji udzielić przepływu](https://msdn.microsoft.com/library/azure/dn645542.aspx).

### <a name="service-to-service-authentication-non-interactive"></a>Aby usługa uwierzytelniania (non-interactive)

Ta metoda aplikacja zapewnia własnej poświadczeń w celu wykonywania operacji. W tym należy wygenerować żądanie POST, tak jak pokazano poniżej: 

    curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
      -F grant_type=client_credentials \
      -F resource=https://management.core.windows.net/ \
      -F client_id=<CLIENT-ID> \
      -F client_secret=<AUTH-KEY>

Wynik to żądanie będzie zawierać tokenu autoryzacji (oznaczona `access-token` w wyniku poniżej), która następnie przekaże z połączenia interfejsu API usługi REST. Zapisz ten token uwierzytelniania w pliku tekstowym; konieczne będzie to w dalszej części tego artykułu.

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1458245447","not_before":"1458241547","resource":"https://management.core.windows.net/","access_token":"<REDACTED>"}

W tym artykule wykorzystano **- interactive** podejście. Aby uzyskać więcej informacji na-interactive (połączeń do usługi) zobacz [Usługa do obsługi technicznej przy użyciu poświadczeń](https://msdn.microsoft.com/library/azure/dn645543.aspx).
## <a name="create-a-data-lake-analytics-account"></a>Tworzenie konta analizy Lake danych

Przed utworzeniem konta analizy Lake danych należy utworzyć grupy zasobów Azure i konta magazynu Lake danych.  Zobacz [Tworzenie konta magazynu Lake danych](../data-lake-store/data-lake-store-get-started-rest-api.md#create-a-data-lake-store-account).

Następujące polecenie zwinięcie pokazano, jak utworzyć konto:

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -H "Content-Type: application/json" https://management.azure.com/subscriptions/<AzureSubscriptionID>/resourceGroups/<AzureResourceGroupName>/providers/Microsoft.DataLakeAnalytics/accounts/<NewAzureDataLakeAnalyticsAccountName>?api-version=2016-11-01 -d@"C:\tutorials\adla\CreateDataLakeAnalyticsAccountRequest.json"

Zamienianie \< `REDACTED` \> z token autoryzacji \< `AzureSubscriptionID` \> z identyfikator subskrypcji \< `AzureResourceGroupName` \> z istniejącą nazwą grupy zasobów Azure, i \< `NewAzureDataLakeAnalyticsAccountName` \> pod nową nazwą konta analizy Lake danych. Ładunku żądania dla tego polecenia są zawarte w pliku **CreateDatalakeAnalyticsAccountRequest.json** , który został umieszczony na `-d` parametru powyżej. Zawartość pliku input.json wyglądać w następujący sposób:

    {  
        "location": "East US 2",  
        "name": "myadla1004",  
        "tags": {},  
        "properties": {  
            "defaultDataLakeStoreAccount": "my1004store",  
            "dataLakeStoreAccounts": [  
                {  
                    "name": "my1004store"  
                }     
            ]
        }  
    }  


## <a name="list-data-lake-analytics-accounts-in-a-subscription"></a>Listy danych Lake analizy kont w subskrypcji

Następujące polecenie zwinięcie pokazano, jak listy kont w subskrypcji:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/<AzureSubscriptionID>/providers/Microsoft.DataLakeAnalytics/Accounts?api-version=2016-11-01

Zamienianie \< `REDACTED` \> z token autoryzacji \< `AzureSubscriptionID` \> ze swojego identyfikatora subskrypcji. Wynik jest podobne do:

    {
        "value": [
            {
            "properties": {
                "provisioningState": "Succeeded",
                "state": "Active",
                "endpoint": "myadla0831.azuredatalakeanalytics.net",
                "accountId": "21e74660-0941-4880-ae72-b143c2615ea9",
                "creationTime": "2016-09-01T12:49:12.7451428Z",
                "lastModifiedTime": "2016-09-01T12:49:12.7451428Z"
            },
            "location": "East US 2",
            "tags": {},
            "id": "/subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/myadla0831rg/providers/Microsoft.DataLakeAnalytics/accounts/myadla0831",
            "name": "myadla0831",
            "type": "Microsoft.DataLakeAnalytics/accounts"
            },
            {
            "properties": {
                "provisioningState": "Succeeded",
                "state": "Active",
                "endpoint": "myadla1004.azuredatalakeanalytics.net",
                "accountId": "3ff9b93b-11c4-43c6-83cc-276292eeb350",
                "creationTime": "2016-10-04T20:46:42.287147Z",
                "lastModifiedTime": "2016-10-04T20:46:42.287147Z"
            },
            "location": "East US 2",
            "tags": {},
            "id": "/subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/myadla1004rg/providers/Microsoft.DataLakeAnalytics/accounts/myadla1004",
            "name": "myadla1004",
            "type": "Microsoft.DataLakeAnalytics/accounts"
            }
        ]
    }

## <a name="get-information-about-a-data-lake-analytics-account"></a>Uzyskiwanie informacji o koncie analizy Lake danych

Następujące polecenie zwinięcie przedstawia sposób uzyskać informacje o koncie:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/<AzureSubscriptionID>/resourceGroups/<AzureResourceGroupName>/providers/Microsoft.DataLakeAnalytics/accounts/<DataLakeAnalyticsAccountName>?api-version=2015-11-01

Zamienianie \< `REDACTED` \> z token autoryzacji \< `AzureSubscriptionID` \> z identyfikator subskrypcji \< `AzureResourceGroupName` \> z istniejącą nazwą Azure grupa zasobów, i \< `DataLakeAnalyticsAccountName` \> z nazwą istniejącego konta Lake analizy danych. Wynik jest podobne do:

    {
        "properties": {
            "defaultDataLakeStoreAccount": "my1004store",
            "dataLakeStoreAccounts": [
            {
                "properties": {
                "suffix": "azuredatalakestore.net"
                },
                "name": "my1004store"
            }
            ],
            "provisioningState": "Creating",
            "state": null,
            "endpoint": null,
            "accountId": "3ff9b93b-11c4-43c6-83cc-276292eeb350",
            "creationTime": null,
            "lastModifiedTime": null
        },
        "location": "East US 2",
        "tags": {},
        "id": "/subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/myadla1004rg/providers/Microsoft.DataLakeAnalytics/accounts/myadla1004",
        "name": "myadla1004",
        "type": "Microsoft.DataLakeAnalytics/accounts"
    }

## <a name="list-data-lake-stores-of-a-data-lake-analytics-account"></a>Lista magazynów Lake konta analizy Lake danych

Następujące polecenie zwinięcie pokazano, jak listy Lake magazynów konta:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/<AzureSubscriptionID>/resourceGroups/<AzureResourceGroupName>/providers/Microsoft.DataLakeAnalytics/accounts/<DataLakeAnalyticsAccountName>/DataLakeStoreAccounts/?api-version=2016-11-01

Zamienianie \< `REDACTED` \> z token autoryzacji \< `AzureSubscriptionID` \> z identyfikator subskrypcji \< `AzureResourceGroupName` \> z istniejącą nazwą grupy zasobów Azure, i \< `DataLakeAnalyticsAccountName` \> z nazwą istniejącego konta Lake analizy danych. Wynik jest podobne do:

    {
        "value": [
            {
            "properties": {
                "suffix": "azuredatalakestore.net"
            },
            "id": "/subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/myadla1004rg/providers/Microsoft.DataLakeAnalytics/accounts/myadla1004/dataLakeStoreAccounts/my1004store",
            "name": "my1004store",
            "type": "Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts"
            }
        ]
    }

## <a name="submit-u-sql-jobs"></a>Przesyłanie zadań U SQL

Następujące polecenie zwinięcie przedstawia sposób przesyłania zadań U SQL:

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" https://<DataLakeAnalyticsAccountName>.azuredatalakeanalytics.net/Jobs/<NewGUID>?api-version=2016-03-20-preview -d@"C:\tutorials\adla\SubmitADLAJob.json"

Zamienianie \< `REDACTED` \> z token autoryzacji \< `DataLakeAnalyticsAccountName` \> z nazwą istniejącego konta Lake analizy danych. Ładunku żądania dla tego polecenia są zawarte w pliku **SubmitADLAJob.json** , który został umieszczony na `-d` parametru powyżej. Zawartość pliku input.json wyglądać w następujący sposób:

    {
        "jobId": "8f8ebf8c-4b63-428a-ab46-a03d2cc5b65a",
        "name": "convertTSVtoCSV",
        "type": "USql",
        "degreeOfParallelism": 1,
        "priority": 1000,
        "properties": {
            "type": "USql",
            "script": "@searchlog =\n    EXTRACT UserId          int,\n            Start           DateTime,\n            Region          string,\n            Query          
        string,\n            Duration        int?,\n            Urls            string,\n            ClickedUrls     string\n    FROM \"/Samples/Data/SearchLog.tsv\"\n    US
        ING Extractors.Tsv();\n\nOUTPUT @searchlog   \n    TO \"/Output/SearchLog-from-Data-Lake.csv\"\nUSING Outputters.Csv();"
        }
    }

Wynik jest podobne do:

    {
        "jobId": "8f8ebf8c-4b63-428a-ab46-a03d2cc5b65a",
        "name": "convertTSVtoCSV",
        "type": "USql",
        "submitter": "myadl@SPI",
        "degreeOfParallelism": 1,
        "priority": 1000,
        "submitTime": "2016-10-05T13:54:59.9871859+00:00",
        "state": "Compiling",
        "result": "Succeeded",
        "stateAuditRecords": [
            {
            "newState": "New",
            "timeStamp": "2016-10-05T13:54:59.9871859+00:00",
            "details": "userName:myadl@SPI;submitMachine:N/A"
            }
        ],
        "properties": {
            "owner": "myadl@SPI",
            "resources": [],
            "runtimeVersion": "default",
            "rootProcessNodeId": "00000000-0000-0000-0000-000000000000",
            "algebraFilePath": "adl://myadls0831.azuredatalakestore.net/system/jobservice/jobs/Usql/2016/10/05/13/54/8f8ebf8c-4b63-428a-ab46-a03d2cc5b65a/algebra.xml",
            "compileMode": "Semantic",
            "errorSource": "Unknown",
            "totalCompilationTime": "PT0S",
            "totalPausedTime": "PT0S",
            "totalQueuedTime": "PT0S",
            "totalRunningTime": "PT0S",
            "type": "USql"
        }
    }


## <a name="list-u-sql-jobs"></a>Wyświetlanie listy zadań U SQL

Następujące polecenie zwinięcie zamieszczono listę zadań U SQL:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://<DataLakeAnalyticsAccountName>.azuredatalakeanalytics.net/Jobs?api-version=2016-11-01 

Zamienianie \< `REDACTED` \> z token autoryzacji i \< `DataLakeAnalyticsAccountName` \> z nazwą istniejącego konta Lake analizy danych. 


Wynik jest podobne do:

    {
    "value": [
        {
        "jobId": "65cf1691-9dbe-43cd-90ed-1cafbfb406fb",
        "name": "convertTSVtoCSV",
        "type": "USql",
        "submitter": "someone@microsoft.com",
        "account": null,
        "degreeOfParallelism": 1,
        "priority": 1000,
        "submitTime": "Wed, 05 Oct 2016 13:46:53 GMT",
        "startTime": "Wed, 05 Oct 2016 13:47:33 GMT",
        "endTime": "Wed, 05 Oct 2016 13:48:07 GMT",
        "state": "Ended",
        "result": "Succeeded",
        "errorMessage": null,
        "storageAccounts": null,
        "stateAuditRecords": null,
        "logFilePatterns": null,
        "properties": null
        },
        {
        "jobId": "8f8ebf8c-4b63-428a-ab46-a03d2cc5b65a",
        "name": "convertTSVtoCSV",
        "type": "USql",
        "submitter": "someoneadl@SPI",
        "account": null,
        "degreeOfParallelism": 1,
        "priority": 1000,
        "submitTime": "Wed, 05 Oct 2016 13:54:59 GMT",
        "startTime": "Wed, 05 Oct 2016 13:55:43 GMT",
        "endTime": "Wed, 05 Oct 2016 13:56:11 GMT",
        "state": "Ended",
        "result": "Succeeded",
        "errorMessage": null,
        "storageAccounts": null,
        "stateAuditRecords": null,
        "logFilePatterns": null,
        "properties": null
        }
    ],
    "nextLink": null,
    "count": null
    }


## <a name="get-catalog-items"></a>Pobieranie elementów wykazu

Następujące polecenie zwinięcie przedstawiono sposób uzyskiwania baz danych z katalogu:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://<DataLakeAnalyticsAccountName>.azuredatalakeanalytics.net/catalog/usql/databases?api-version=2016-11-01

Wynik jest podobne do:

    {
    "@odata.context":"https://myadla0831.azuredatalakeanalytics.net/sqlip/$metadata#databases","value":[
        {
        "computeAccountName":"myadla0831","databaseName":"mytest","version":"f6956327-90b8-4648-ad8b-de3ff09274ea"
        },{
        "computeAccountName":"myadla0831","databaseName":"master","version":"e8bca908-cc73-41a3-9564-e9bcfaa21f4e"
        }
    ]
    }

## <a name="see-also"></a>Zobacz też

- Aby uzyskać bardziej złożonych kwerend, zobacz [Dzienniki analiza witryny sieci Web przy użyciu analizy Lake danych Azure](data-lake-analytics-analyze-weblogs.md).
- Aby rozpocząć tworzenie aplikacji U SQL, zobacz [skryptów można opracowywać U-SQL przy użyciu narzędzia Lake danych dla programu Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).
- Aby dowiedzieć się U SQL, zobacz [Wprowadzenie do języka Azure danych Lake analizy U-SQL](data-lake-analytics-u-sql-get-started.md).
- Do zadań zarządzania zobacz [Zarządzanie analizy Lake danych Azure za pomocą portalu Azure](data-lake-analytics-manage-use-portal.md).
- Aby uzyskać omówienie analizy Lake danych, zobacz [Omówienie analizy Lake danych Azure](data-lake-analytics-overview.md).
- Aby wyświetlić samej samouczka przy użyciu innych narzędzi, kliknij selektory kartę w górnej części strony.
- Aby rejestrować informacje diagnostyczne, zobacz [Uzyskiwanie dostępu do dzienników Diagnostyka Azure danych Lake analiz](data-lake-analytics-diagnostic-logs.md)
