<properties
   pageTitle="Publikowanie WebApplicationVM | Microsoft Azure"
   description="Dowiedz się, jak wdrożyć aplikację sieci web maszyn wirtualnych. Ten skrypt tworzy wymaganych zasobów w ramach subskrypcji Azure, jeśli nie są zachowywane."
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

# <a name="publish-webapplicationvm-windows-powershell-script"></a>Publikowanie WebApplicationVM (skrypt programu Windows PowerShell)

Wdrożenie aplikacji sieci web maszyn wirtualnych. Skrypt tworzy wymaganych zasobów w ramach subskrypcji Azure, jeśli nie są zachowywane.

```
Publish-WebApplicationVM
–Configuration <configuration>
-SubscriptionName <subscriptionName>
-WebDeployPackage <packageName>
-VMPassword @{Name = "name"; Password = "password")
-DatabaseServerPassword @{Name = "name"; Password = "password"}
-SendHostMessagesToOutput
-Verbose
```

### <a name="configuration"></a>Konfiguracja

Ścieżka do pliku konfiguracji JSON, który opisuje szczegóły wdrożenia.

|Aliasy|Brak|
|---|---|
|Wymagane?|wartość PRAWDA.|
|Pozycja|o nazwie|
|Wartość domyślna|Brak|
|Zaakceptuj wprowadzania potok?|FAŁSZ|
|Zaakceptuj symboli wieloznacznych?|FAŁSZ|

### <a name="subscriptionname"></a>SubscriptionName

Nazwa Azure subskrypcji, w której chcesz utworzyć maszyny wirtualnej.

|Aliasy|Brak|
|---|---|
|Wymagane?|FAŁSZ|
|Pozycja|o nazwie|
|Wartość domyślna|Używa pierwszej subskrypcji w pliku subskrypcji|
|Zaakceptuj wprowadzania potok?|FAŁSZ|
|Zaakceptuj symboli wieloznacznych?|FAŁSZ|

### <a name="webdeploypackage"></a>WebDeployPackage

Ścieżka do pakietu wdrażania sieci web, aby opublikować maszyn wirtualnych. Za pomocą Kreatora publikowania w sieci Web w programie Visual Studio, można utworzyć ten pakiet. Zobacz [jak: Tworzenie pakietu wdrażania sieci Web w programie Visual Studio](https://msdn.microsoft.com/library/dd465323.aspx).

|Aliasy|Brak|
|---|---|
|Wymagane?|FAŁSZ|
|Pozycja|o nazwie|
|Wartość domyślna|Brak|
|Zaakceptuj wprowadzania potok?|FAŁSZ|
|Zaakceptuj symboli wieloznacznych?|FAŁSZ|

### <a name="allowuntrusted"></a>AllowUntrusted

Jeśli wartość true, zezwala na użycie certyfikatów, które nie są podpisane przez zaufany urząd główny.

|Aliasy|Brak|
|---|---|
|Wymagane?|FAŁSZ|
|Pozycja|o nazwie|
|Wartość domyślna|FAŁSZ|
|Zaakceptuj wprowadzania potok?|FAŁSZ|
|Zaakceptuj symboli wieloznacznych?|FAŁSZ|

### <a name="vmpassword"></a>VMPassword

Poświadczenia konta maszyn wirtualnych. Przykład: - VMPassword @{Name = "Administrator"; Hasło = "hasło"}

|Aliasy|Brak|
|---|---|
|Wymagane?|FAŁSZ|
|Pozycja|o nazwie|
|Wartość domyślna|Brak|
|Zaakceptuj wprowadzania potok?|FAŁSZ|
|Zaakceptuj symboli wieloznacznych?|FAŁSZ|

### <a name="databaseserverpassword"></a>DatabaseServerPassword

Poświadczenia z bazą danych SQL platformy Azure. Przykład: - DatabaseServerPassword @{Name = "Administrator"; Hasło = "hasło"}

|Aliasy|Brak|
|---|---|
|Wymagane?|FAŁSZ|
|Pozycja|o nazwie|
|Wartość domyślna|Brak|
|Zaakceptuj wprowadzania potok?|FAŁSZ|
|Zaakceptuj symboli wieloznacznych?|FAŁSZ|

### <a name="sendhostmessagestooutput"></a>SendHostMessagesToOutput

Jeśli wartość true, Drukuj wiadomości od skrypt do strumienia wyjściowego.

|Aliasy|Brak|
|---|---|
|Wymagane?|FAŁSZ|
|Pozycja|o nazwie|
|Wartość domyślna|FAŁSZ|
|Zaakceptuj wprowadzania potok?|FAŁSZ|
|Zaakceptuj symboli wieloznacznych?|FAŁSZ|

## <a name="remarks"></a>Uwagi

Pełny opis sposobu używania skrypt do tworzenia środowiska deweloperów i badania, zobacz [Za pomocą skryptów programu PowerShell systemu Windows do opublikowania deweloperów i środowisk testowych](vs-azure-tools-publishing-using-powershell-scripts.md).

Plik konfiguracyjny JSON określa szczegóły, co ma być używany. Zawiera informacje, które zostało określone podczas tworzenia projektu, takie jak nazwa, grupa koligacji, obraz wirtualnego dysku twardego i rozmiar maszyny wirtualnej. Również zawiera punkty końcowe maszyny wirtualnej, bazy danych do obsługi administracyjnej, jeśli istnieją, a wdrożenia parametrów w sieci web. Przykładowy plik konfiguracji JSON kod:

```
{
    "environmentSettings": {
        "cloudService": {
            "name": "myvmname",
            "affinityGroup": "",
            "location": "West US",
            "virtualNetwork": "",
            "subnet": "",
            "availabilitySet": "",
            "virtualMachine": {
                "name": "myvmname",
                "vhdImage": "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-201404.01-en.us-127GB.vhd",
                "size": "Small",
                "user": "vmuser1",
                "password": "",
                "enableWebDeployExtension": true,
                "endpoints": [
                    {
                        "name": "Http",
                        "protocol": "TCP",
                        "publicPort": "80",
                        "privatePort": "80"
                    },
                    {
                        "name": "Https",
                        "protocol": "TCP",
                        "publicPort": "443",
                        "privatePort": "443"
                    },
                    {
                        "name": "WebDeploy",
                        "protocol": "TCP",
                        "publicPort": "8172",
                        "privatePort": "8172"
                    },
                    {
                        "name": "Remote Desktop",
                        "protocol": "TCP",
                        "publicPort": "3389",
                        "privatePort": "3389"
                    },
                    {
                        "name": "Powershell",
                        "protocol": "TCP",
                        "publicPort": "5986",
                        "privatePort": "5986"
                    }
                ]
            }
        },
        "databases": [
            {
                "connectionStringName": "",
                "databaseName": "",
                "serverName": "",
                "user": "",
                "password": ""
            }
        ],
        "webDeployParameters": {
            "iisWebApplicationName": "Default Web Site"
        }
    }
}
```

Można edytować plik konfiguracji JSON, aby zmienić co obsługi administracyjnej. Wymagane są maszyny wirtualnej i usługi w chmurze, ale sekcji Baza danych jest opcjonalna.
