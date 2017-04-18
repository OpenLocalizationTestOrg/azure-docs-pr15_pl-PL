<properties
   pageTitle="Zarządzanie analizy Lake danych Azure za pomocą Azure SDK dla Node.js | Azure"
   description="Dowiedz się, jak zarządzać kontami analizy Lake danych, źródła danych, zadania i użytkowników dla Node.js przy użyciu zestawu SDK Azure"
   services="data-lake-analytics"
   documentationCenter=""
   authors="edmacauley"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="05/16/2016"
   ms.author="edmaca"/>

# <a name="manage-azure-data-lake-analytics-using-azure-sdk-for-nodejs"></a>Zarządzanie analizy Lake danych Azure za pomocą Azure SDK dla Node.js


[AZURE.INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

Zestaw SDK Azure dla Node.js może służyć do zarządzania Azure danych Lake analizy kont, zadań i katalogi. Aby wyświetlić temat zarządzania przy użyciu innych narzędzi, kliknij powyższe wybierz kartę.

Prawo teraz obsługuje:

  *  **Wersja node.js: 0.10.0 lub nowszy**
  *  **Wersja interfejsu API usługi REST konta: 2015-10-01-Podgląd**
  *  **Wersja interfejsu API usługi REST dla wykazu: 2015-10-01-Podgląd**
  *  **Wersja interfejsu API usługi REST dla zadania: 2016-03-20-Podgląd**

## <a name="features"></a>Funkcje

- Zarządzania kontami: tworzenie, Uzyskaj, listy, aktualizowanie i usuwanie.
- Zadania zarządzania: przesyłanie, uzyskiwanie, listy i Anuluj.
- Zarządzanie wykazu: uzyskiwanie, listy, tworzenie (hasła), aktualizowanie (hasła) i usuwanie (hasła).

## <a name="how-to-install"></a>Jak zainstalować

```bash
npm install azure-arm-datalake-analytics
```

## <a name="authenticate-using-azure-active-directory"></a>Uwierzytelnianie za pomocą usługi Azure Active Directory

 ```javascript
 var msrestAzure = require('ms-rest-azure');
 //user authentication
 var credentials = new msRestAzure.UserTokenCredentials('your-client-id', 'your-domain', 'your-username', 'your-password', 'your-redirect-uri');
 //service principal authentication
 var credentials = new msRestAzure.ApplicationTokenCredentials('your-client-id', 'your-domain', 'your-secret');
 ```

## <a name="create-the-data-lake-analytics-client"></a>Tworzenie klienta analizy Lake danych

```javascript
var adlaManagement = require("azure-arm-datalake-analytics");
var acccountClient = new adlaManagement.DataLakeAnalyticsAccountClient(credentials, 'your-subscription-id');
var jobClient = new adlaManagement.DataLakeAnalyticsJobClient(credentials, 'azuredatalakeanalytics.net');
var catalogClient = new adlaManagement.DataLakeAnalyticsCatalogClient(credentials, 'azuredatalakeanalytics.net');
```

## <a name="create-a-data-lake-analytics-account"></a>Tworzenie konta analizy Lake danych

```javascript
var util = require('util');
var resourceGroupName = 'testrg';
var accountName = 'testadlaacct';
var location = 'eastus2';

// A Data Lake Store account must already have been created to create
// a Data Lake Analytics account. See the Data Lake Store readme for
// information on doing so. For now, we assume one exists already.
var datalakeStoreAccountName = 'existingadlsaccount';

// account object to create
var accountToCreate = {
  tags: {
    testtag1: 'testvalue1',
    testtag2: 'testvalue2'
  },
  name: accountName,
  location: location,
  properties: {
    defaultDataLakeStoreAccount: datalakeStoreAccountName,
    dataLakeStoreAccounts: [
      {
        name: datalakeStoreAccountName
      }
    ]
  }
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

## <a name="get-a-list-of-jobs"></a>Uzyskiwanie listy zadań

```javascript
var util = require('util');
var accountName = 'testadlaacct';
jobClient.job.list(accountName, function (err, result, request, response) {
  if (err) {
    console.log(err);
  } else {
    console.log('result is: ' + util.inspect(result, {depth: null}));
  }
});
```

## <a name="get-a-list-of-databases-in-the-data-lake-analytics-catalog"></a>Uzyskiwanie listy baz danych w wykazie danych Lake analizy
```javascript
var util = require('util');
var accountName = 'testadlaacct';
catalogClient.catalog.listDatabases(accountName, function (err, result, request, response) {
  if (err) {
    console.log(err);
  } else {
    console.log('result is: ' + util.inspect(result, {depth: null}));
  }
});
```

## <a name="see-also"></a>Zobacz też

- [Zestaw SDK platformy Microsoft Azure dla Node.js](https://github.com/azure/azure-sdk-for-node)
- [Zestaw SDK platformy Microsoft Azure dla Node.js — Zarządzanie magazynem Lake danych](https://github.com/Azure/azure-sdk-for-node/tree/autorest/lib/services/dataLake.Store)
