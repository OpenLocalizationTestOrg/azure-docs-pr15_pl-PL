<properties
   pageTitle="Publikowanie WebApplicationWebSite (skrypt programu Windows PowerShell) | Microsoft Azure"
   description="Dowiedz się, jak opublikować projektu sieci web Azure witryny sieci Web. Ten skrypt tworzy wymaganych zasobów w ramach subskrypcji Azure, jeśli nie są zachowywane."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="publish-webapplicationwebsite-windows-powershell-script"></a>Publikowanie WebApplicationWebSite (skrypt programu Windows PowerShell)

##<a name="syntax"></a>W składni

Publikowanie projektu sieci web na Azure witryny sieci Web. Skrypt tworzy wymaganych zasobów w ramach subskrypcji Azure, jeśli nie są zachowywane.

    Publish-WebApplicationWebSite
    –Configuration <configuration>
    -SubscriptionName <subscriptionName>
    -WebDeployPackage <packageName>
    -DatabaseServerPassword @{Name = "name"; Password = "password"}
    -SendHostMessagesToOutput
    -Verbose


## <a name="configuration"></a>Konfiguracja

Ścieżka do pliku konfiguracji JSON, który opisuje szczegóły wdrożenia.

|Parametr|Wartość domyślna|
|---|---|
|Aliasy|Brak|
|Wymagane?|wartość PRAWDA.|
|Pozycja|o nazwie|
|Wartość domyślna|Brak|
|Zaakceptuj wprowadzania potok?|FAŁSZ|
|Zaakceptuj symboli wieloznacznych?|FAŁSZ|

## <a name="subscriptionname"></a>SubscriptionName

Nazwa Azure subskrypcję, do której chcesz utworzyć witrynę sieci Web w.

|Parametr|Wartość domyślna|
|---|---|
|Aliasy|Brak|
|Wymagane?|FAŁSZ|
|Pozycja|o nazwie|
|Wartość domyślna|Brak|
|Zaakceptuj wprowadzania potok?|FAŁSZ|
|Zaakceptuj symboli wieloznacznych?|FAŁSZ|

## <a name="webdeploypackage"></a>WebDeployPackage

Ścieżka do pakietu wdrażania sieci web do publikowania w witrynie sieci Web. Za pomocą Kreatora publikowania w sieci Web w programie Visual Studio, można utworzyć ten pakiet. Aby uzyskać więcej informacji zobacz [Rozpoczynanie pracy z usługami w chmurze Azure i ASP.NET](http://go.microsoft.com/fwlink/p/?LinkID=623089).

|Parametr|Wartość domyślna|
|---|---|
|Aliasy|Brak|
|Wymagane?|FAŁSZ|
|Pozycja|o nazwie|
|Wartość domyślna|Brak|
|Zaakceptuj wprowadzania potok?|FAŁSZ|
|Zaakceptuj symboli wieloznacznych?|FAŁSZ|

## <a name="databaseserverpassword"></a>DatabaseServerPassword

Nazwa użytkownika i hasło dla bazy danych SQL platformy Azure.

|Parametr|Wartość domyślna|
|---|---|
|Aliasy|Brak|
|Wymagane?|FAŁSZ|
|Pozycja|o nazwie|
|Wartość domyślna|Brak|
|Zaakceptuj wprowadzania potok?|FAŁSZ|
|Zaakceptuj symboli wieloznacznych?|FAŁSZ|

## <a name="sendhostmessagestooutput"></a>SendHostMessagesToOutput

Jeśli wartość true, Drukuj wiadomości od skrypt do strumienia wyjściowego.

|Parametr|Wartość domyślna|
|---|---|
|Aliasy|Brak|
|Wymagane?|FAŁSZ|
|Pozycja|o nazwie|
|Wartość domyślna|FAŁSZ|
|Zaakceptuj wprowadzania potok?|FAŁSZ|
|Zaakceptuj symboli wieloznacznych?|FAŁSZ|

## <a name="remarks"></a>Uwagi

Pełny opis sposobu używania skrypt do tworzenia środowiska deweloperów i badania, zobacz [Za pomocą skryptów programu PowerShell systemu Windows do opublikowania deweloperów i środowisk testowych](vs-azure-tools-publishing-using-powershell-scripts.md).

Plik konfiguracyjny JSON określa szczegóły, co ma być używany. Zawiera informacje określone podczas tworzenia projektu, takie jak nazwa i nazwa użytkownika dla witryny sieci Web. Zawiera również postanowienia w bazie danych, jeśli istnieją. Przykładowy plik konfiguracji JSON kod:

    {
        "environmentSettings": {
            "webSite": {
                "name": "WebApplication10554",
                "location": "West US"
            },
            "databases": [
                {
                    "connectionStringName": "DefaultConnection",
                    "databaseName": "WebApplication10554_db",
                    "serverName": "iss00brc88",
                    "user": "sqluser2",
                    "password": "",
                    "edition": "",
                    "size": "",
                    "collation": "",
                    "location": "West US"
                }
            ]
        }
    }

Można edytować plik konfiguracji JSON, aby zmienić, co to jest wdrożona. Sekcja witryny sieci Web jest wymagana, ale sekcji Baza danych jest opcjonalna.

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji zobacz [Publikowanie-WebApplicationVM (skrypt programu Windows PowerShell)](vs-azure-tools-publish-webapplicationvm.md)
