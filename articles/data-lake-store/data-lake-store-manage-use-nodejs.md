<properties 
   pageTitle="Rozpoczynanie pracy z Azure magazynów Lake przy użyciu zestawu SDK Azure dla Node.js | Microsoft Azure"
   description="Dowiedz się, jak pracować z kontami magazynu Lake danych i system plików za pomocą Node.js." 
   services="data-lake-store" 
   documentationCenter="" 
   authors="nitinme" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="09/27/2016"
   ms.author="nitinme"/>

# <a name="get-started-with-azure-data-lake-store-using-azure-sdk-for-nodejs"></a>Rozpoczynanie pracy z magazynu Lake danych Azure za pomocą Azure SDK dla Node.js

> [AZURE.SELECTOR]
- [Portal](data-lake-store-get-started-portal.md)
- [Programu PowerShell](data-lake-store-get-started-powershell.md)
- [ZESTAW SDK PROGRAMU .NET](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [INTERFEJSU API USŁUGI REST](data-lake-store-get-started-rest-api.md)
- [Polecenie Azure](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)


Dowiedz się, jak utworzyć konto Azure magazynu Lake danych i wykonywania podstawowych operacji, takich jak tworzyć foldery, przekazywanie i pobieranie plików danych za pomocą SDK Azure dla Node.js, usuwanie swojego konta, itp. Aby uzyskać więcej informacji na temat magazynu Lake danych zobacz [Omówienie pakietu Lake magazynu danych](data-lake-store-overview.md). Obecnie obsługuje SDK

  *  **Wersja node.js: 0.10.0 lub nowszy**
  *  **Wersja interfejsu API usługi REST konta: 2015-10-01-Podgląd**
  *  **Wersja interfejsu API usługi REST dla plików: 2015-10-01-Podgląd**

## <a name="prerequisites"></a>Wymagania wstępne

Przed rozpoczęciem tego artykułu, musisz mieć następujące czynności:

- **Azure subskrypcji**. Zobacz [Azure pobrać bezpłatną wersję próbną](https://azure.microsoft.com/pricing/free-trial/).

- **Tworzenie aplikacji usługi Azure Active Directory**. Za pomocą aplikacji Azure AD uwierzytelniania aplikacji magazynu Lake danych z usługą Azure Active Directory. Istnieją różne metody do uwierzytelniania Azure AD, które są **uwierzytelniania użytkowników końcowych** lub **do usługi**. Instrukcje i uzyskać więcej informacji na temat uwierzytelniania Zobacz [uwierzytelniania z magazynu Lake danych za pomocą usługi Azure Active Directory](data-lake-store-authenticate-using-active-directory.md).

## <a name="how-to-install"></a>Jak zainstalować

```bash
npm install azure-arm-datalake-store
```

## <a name="authenticate-using-azure-active-directory"></a>Uwierzytelnianie za pomocą usługi Azure Active Directory

Wstawki poniżej pokazano dwa sposoby osobnych uwierzytelniania z magazynu Lake danych przy użyciu Azure AD. Szczegółowe omówienie różnych metod uwierzytelniania z magazynu Lake danych za pomocą zobacz [uwierzytelniania z magazynu Lake danych za pomocą usługi Azure Active Directory](data-lake-store-authenticate-using-active-directory.md).

Poniżej wstawek wymaga również danych wejściowych, takich jak nazwy domeny Azure AD, identyfikator klienta dla aplikacji Azure AD itd. Szczegóły te mogą być pobierane z Azure AD aplikacji należy utworzony, informacje o znajdują się też w łącze powyżej.

 ```javascript
 var msrestAzure = require('ms-rest-azure');
 //user authentication
 var credentials = new msRestAzure.UserTokenCredentials('your-client-id', 'your-domain', 'your-username', 'your-password', 'your-redirect-uri');
 //service principal authentication
 var credentials = new msRestAzure.ApplicationTokenCredentials('your-client-id', 'your-domain', 'your-secret');
 ```

## <a name="create-the-data-lake-store-clients"></a>Tworzenie magazynu Lake danych klientów

```javascript
var adlsManagement = require("azure-arm-datalake-store");
var acccountClient = new adlsManagement.DataLakeStoreAccountClient(credentials, "your-subscription-id");
var filesystemClient = new adlsManagement.DataLakeStoreFileSystemClient(credentials);
```

## <a name="create-a-data-lake-store-account"></a>Tworzenie konta magazynu Lake danych

```javascript
var util = require('util');
var resourceGroupName = 'testrg';
var accountName = 'testadlsacct';
var location = 'eastus2';

// account object to create
var accountToCreate = {
  tags: {
    testtag1: 'testvalue1',
    testtag2: 'testvalue2'
  },
  name: accountName,
  location: location
};

client.account.create(resourceGroupName, accountName, accountToCreate, function (err, result, request, response) {
  if (err) {
    console.log(err);
    /*err has reference to the actual request and response, so you can see what was sent and received on the wire.
      The structure of err looks like this:
      err: {
        code: 'Error Code',
        message: 'Error Message',
        body: 'The response body if any',
        request: reference to a stripped version of http request
        response: reference to a stripped version of the response
      }
    */
  } else {
    console.log('result is: ' + util.inspect(result, {depth: null}));
  }
});
```

## <a name="create-a-file-with-content"></a>Tworzenie pliku z zawartością
```javascript
var util = require('util');
var accountName = 'testadlsacct';
var fileToCreate = '/myfolder/myfile.txt';
var options = {
  streamContents: new Buffer('some string content')
}

filesystemClient.fileSystem.listFileStatus(accountName, fileToCreate, options, function (err, result, request, response) {
  if (err) {
    console.log(err);
  } else {
    // no result is returned, only a 201 response for success.
    console.log('response is: ' + util.inspect(response, {depth: null}));
  }
});
```

## <a name="get-a-list-of-files-and-folders"></a>Lista plików i folderów

```javascript
var util = require('util');
var accountName = 'testadlsacct';
var pathToEnumerate = '/myfolder';
filesystemClient.fileSystem.listFileStatus(accountName, pathToEnumerate, function (err, result, request, response) {
  if (err) {
    console.log(err);
  } else {
    console.log('result is: ' + util.inspect(result, {depth: null}));
  }
});
```

## <a name="see-also"></a>Zobacz też

- [Zestaw SDK platformy Microsoft Azure dla Node.js](https://github.com/azure/azure-sdk-for-node)
- [Zestaw SDK platformy Microsoft Azure dla Node.js — Zarządzanie Lake analizy danych](https://www.npmjs.com/package/azure-arm-datalake-analytics)
