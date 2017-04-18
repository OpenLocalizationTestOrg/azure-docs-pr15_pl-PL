<properties
    pageTitle="Tworzenie pierwszego factory danych (REST) | Microsoft Azure"
    description="W tym samouczku możesz utworzyć potok Azure Factory danych przykładowych za pomocą interfejsu API usługi REST Factory danych."
    services="data-factory"
    documentationCenter=""
    authors="spelluru"
    manager="jhubbard"
    editor="monicar"
/>

<tags
    ms.service="data-factory"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="08/16/2016"
    ms.author="spelluru"/>

# <a name="tutorial-build-your-first-azure-data-factory-using-data-factory-rest-api"></a>Samouczek: Tworzenie pierwszej firmie Azure danych za pomocą interfejsu API usługi REST Factory danych
> [AZURE.SELECTOR]
- [Omówienie i wymagania wstępne](data-factory-build-your-first-pipeline.md)
- [Azure portal](data-factory-build-your-first-pipeline-using-editor.md)
- [Programu Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
- [Programu PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
- [Szablon Menedżera zasobów](data-factory-build-your-first-pipeline-using-arm.md)
- [INTERFEJSU API USŁUGI REST](data-factory-build-your-first-pipeline-using-rest-api.md)

W tym artykule umożliwia interfejsu API usługi REST Factory danych tworzenie pierwszej firmie Azure danych.

## <a name="prerequisites"></a>Wymagania wstępne
- Przeczytaj artykuł [Omówienie samouczka](data-factory-build-your-first-pipeline.md) i wykonaj kroki **wymagania wstępne** .
- Zainstaluj [zawinięcie](https://curl.haxx.se/dlwiz/) na tym komputerze. Aby utworzyć factory danych jest używane narzędzie ZWINIĘCIE z pozostałych poleceń. 
- Wykonaj instrukcje z [tego artykułu](../resource-group-create-service-principal-portal.md) , aby: 
    1. Tworzenie aplikacji sieci Web o nazwie **ADFGetStartedApp** w usłudze Azure Active Directory.
    2. Uzyskaj **identyfikator klienta** i **klucz tajny**. 
    3. Uzyskaj **identyfikator dzierżawy**. 
    4. Przypisywanie aplikacji **ADFGetStartedApp** do roli **Współautora Factory danych** .  
- Instalowanie [programu PowerShell Azure](../powershell-install-configure.md).  
- Uruchamianie **programu PowerShell** i wpisz następujące polecenie. Nie zamykaj Azure programu PowerShell do momentu zakończenia tego samouczka. Po zamknięciu i ponownym otwarciu, należy ponownie uruchomić polecenia.
    1. Uruchamianie **AzureRmAccount logowania** i wprowadź nazwę użytkownika i hasło, którego używasz, aby zalogować się do portalu Azure.  
    2. Uruchom **Get-AzureRmSubscription** , aby wyświetlić wszystkie subskrypcje dla tego konta.
    3. Uruchamianie **Get AzureRmSubscription - SubscriptionName NameOfAzureSubscription | Ustawianie AzureRmContext** do Wybierz subskrypcję, którą chcesz pracować z. Zamień **NameOfAzureSubscription** nazwę subskrypcji Azure. 
3. Tworzenie grupy usługi Azure zasobów o nazwie **ADFTutorialResourceGroup** , uruchamiając następujące polecenie w programie PowerShell:  

        New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"

    Niektóre czynności opisane w tym samouczku Załóżmy, że grupa zasobów o nazwie ADFTutorialResourceGroup. Jeśli korzystasz z różnych grupach zasobów, należy użyć nazwy grupy zasobów zamiast ADFTutorialResourceGroup w tym samouczku.

## <a name="create-json-definitions"></a>Tworzenie definicji JSON
Tworzenie po JSON plików w folderze, w którym znajduje się curl.exe. 

### <a name="datafactoryjson"></a>DataFactory.JSON 
> [AZURE.IMPORTANT] Nazwa musi być globalnie unikatowe, więc warto prefiks i sufiks ADFCopyTutorialDF, aby była unikatowa nazwa. 

    {  
        "name": "FirstDataFactoryREST",  
        "location": "WestUS"
    }  

### <a name="azurestoragelinkedservicejson"></a>azurestoragelinkedservice.JSON
> [AZURE.IMPORTANT] Zastąp **Nazwa konta** i **accountkey** nazwę i klucz konta Azure miejsca do magazynowania. Aby dowiedzieć się, jak uzyskać klucz dostępu do magazynowania, zobacz [Wyświetlanie, Kopiuj i klawisze dostępu wyniku miejsca do magazynowania](../storage/storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys).

    {
        "name": "AzureStorageLinkedService",
        "properties": {
            "type": "AzureStorage",
            "typeProperties": {
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
            }
        }
    }


### <a name="hdinsightondemandlinkedservicejson"></a>hdinsightondemandlinkedservice.JSON

    {
        "name": "HDInsightOnDemandLinkedService",
        "properties": {
            "type": "HDInsightOnDemand",
            "typeProperties": {
                "version": "3.2",
                "clusterSize": 1,
                "timeToLive": "00:30:00",
                "linkedServiceName": "AzureStorageLinkedService"
            }
        }
    }

Poniższa tabela zawiera opisy właściwości JSON używane w wstawkę kodu:

| Właściwość | Opis |
| :------- | :---------- |
| Wersja | Określa, że należy 3,2 wersji HDInsight. | 
| ClusterSize | Rozmiar klaster HDInsight. | 
| Licznika TimeToLive | Określa, że czas bezczynności klaster HDInsight, zanim zostanie usunięty. |
| linkedServiceName | Określa konto miejsca do magazynowania, który służy do przechowywania dzienniki wygenerowane przez HDInsight |

Zwróć uwagę następujące punkty: 

- Factory danych utworzy klastrze HDInsight **systemu Windows** z powyższych JSON. Możesz również mogą mieć go utworzyć klaster HDInsight **systemem Linux** . Aby uzyskać szczegółowe informacje, zobacz [Połączone usługi HDInsight na żądanie](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) . 
- **Klaster HDInsight** można używać zamiast klastrze HDInsight na żądanie. Aby uzyskać szczegółowe informacje, zobacz [Połączone usługi HDInsight](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) .
- Klaster HDInsight tworzy **kontener domyślny** w magazynie obiektów blob, podane w JSON (**linkedServiceName**). Usługa HDInsight nie powoduje usunięcia tego kontenera, usunięcie klaster. To zachowanie jest zgodne z projektem. Usługa HDInsight połączone na żądanie HDInsight klaster jest tworzony każdorazowo wycinek jest przetwarzana, chyba że jest istniejący live klaster (**licznika timeToLive**) i zostanie usunięty po zakończeniu przetwarzania.

    Gdy więcej wycinki są przetwarzane, widzisz liczby kontenerów w magazynie obiektów blob platformy Azure. Jeśli nie ma potrzeby dotyczących rozwiązywania problemów z zadań, warto Usuń je, aby zajmowała miejsca do magazynowania. Nazwy tych kontenerów wykonaj deseń: "adf**yourdatafactoryname**-**linkedservicename**- datetimestamp". Aby usunąć kontenerów w magazynie obiektów blob platformy Azure, użyj narzędzi, takich jak [Miejsca do magazynowania w Eksploratorze](http://storageexplorer.com/) .

Aby uzyskać szczegółowe informacje, zobacz [Połączone usługi HDInsight na żądanie](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) . 

### <a name="inputdatasetjson"></a>inputdataset.JSON

    {
        "name": "AzureBlobInput",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "fileName": "input.log",
                "folderPath": "adfgetstarted/inputdata",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "availability": {
                "frequency": "Month",
                "interval": 1
            },
            "external": true,
            "policy": {}
        }
    }


JSON definiuje zestaw danych o nazwie **AzureBlobInput**, która reprezentuje danych wejściowych dla działania w potoku. Ponadto określa, że dane wejściowe znajduje się w kontenerze obiektów blob o nazwie **adfgetstarted** i folderu o nazwie **inputdata**.

Poniższa tabela zawiera opisy właściwości JSON używane w wstawkę kodu:

| Właściwość | Opis |
| :------- | :---------- |
| Typ | Właściwość Typ jest ustawiona na AzureBlob, ponieważ dane znajdują się w magazynie obiektów blob platformy Azure. |  
| linkedServiceName | odwołuje się do StorageLinkedService został utworzony wcześniej. |
| Nazwa pliku | Ta właściwość jest opcjonalna. Jeśli ta właściwość zostanie pominięty, wszystkie pliki z ścieżkafolderu są pobierane. W tym przypadku jest przetwarzana tylko input.log. |
| Typ | Pliki dziennika są w formacie tekstowym, więc firma Microsoft korzysta z TextFormat. | 
| columnDelimiter | kolumny w plikach dziennika są rozdzielane znakami przecinek () |
| częstotliwość/interwał | częstotliwość ustawionym na wartość miesiąc i interwału wynosi 1, co oznacza, że wprowadzania wycinki są dostępne co miesiąc. | 
| zewnętrzne | Ta właściwość jest ustawiona na PRAWDA, jeśli dane wejściowe nie jest generowany przez usługę Factory danych. | 

### <a name="outputdatasetjson"></a>outputdataset.JSON

    {
        "name": "AzureBlobOutput",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "adfgetstarted/partitioneddata",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "availability": {
                "frequency": "Month",
                "interval": 1
            }
        }
    }

JSON definiuje zestaw danych o nazwie **AzureBlobOutput**, reprezentujące dane wyjściowe dla działania w potoku. Ponadto określa, że wyniki są przechowywane w kontenerze obiektów blob o nazwie **adfgetstarted** i folderu o nazwie **partitioneddata**. W sekcji **dostępność** Określa, że zestaw danych wynik jest wyprodukowane miesięcznych.

### <a name="pipelinejson"></a>pipeline.JSON
> [AZURE.IMPORTANT] Zamień **storageaccountname** nazwę konta magazynu platformy Azure. 


    {
        "name": "MyFirstPipeline",
        "properties": {
            "description": "My first Azure Data Factory pipeline",
            "activities": [{
                "type": "HDInsightHive",
                "typeProperties": {
                    "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                    "scriptLinkedService": "AzureStorageLinkedService",
                    "defines": {
                        "inputtable": "wasb://adfgetstarted@<stroageaccountname>.blob.core.windows.net/inputdata",
                        "partitionedtable": "wasb://adfgetstarted@<stroageaccountname>t.blob.core.windows.net/partitioneddata"
                    }
                },
                "inputs": [{
                    "name": "AzureBlobInput"
                }],
                "outputs": [{
                    "name": "AzureBlobOutput"
                }],
                "policy": {
                    "concurrency": 1,
                    "retry": 3
                },
                "scheduler": {
                    "frequency": "Month",
                    "interval": 1
                },
                "name": "RunSampleHiveActivity",
                "linkedServiceName": "HDInsightOnDemandLinkedService"
            }],
            "start": "2016-07-10T00:00:00Z",
            "end": "2016-07-11T00:00:00Z",
            "isPaused": false
        }
    }

W wstawkę JSON tworzysz Potok składa się z jednego działania używającej gałęzi przetworzyć danych w klastrze HDInsight.

Plik skryptu gałęzi, **partitionweblogs.hql**znajduje się na koncie Azure przestrzeni dyskowej (określonego przez scriptLinkedService, o nazwie **StorageLinkedService**), a w folderze **skrypt** w kontenerze **adfgetstarted**.

Sekcja **definiuje** określa środowisko uruchomieniowe ustawień, które są przekazywane do skryptu gałęzi jako wartości konfiguracji gałęzi (np. ${hiveconf: inputtable}, ${hiveconf:partitionedtable}).

**Początek** i **koniec** właściwości potoku określa aktywnego okresu potoku.

W działaniu JSON możesz określić, że skrypt gałąź zostanie uruchomiona na obliczeń określony przez **linkedServiceName** — **HDInsightOnDemandLinkedService**.

> [AZURE.NOTE] Aby uzyskać szczegółowe informacje o właściwości JSON używane w poprzednim przykładzie, zobacz [Budowa potok](data-factory-create-pipelines.md#anatomy-of-a-pipeline) . 

## <a name="set-global-variables"></a>Ustawianie zmiennych globalnych

W programie PowerShell Azure zamieniając wartości na własny wykonanie następujących poleceń:

> [AZURE.IMPORTANT] Zobacz sekcję [wymagania wstępne](#prerequisites) , aby uzyskać instrukcje dotyczące uzyskiwania identyfikator klienta, tajny klienta dzierżawa identyfikator i identyfikator subskrypcji.   

    $client_id = "<client ID of application in AAD>"
    $client_secret = "<client key of application in AAD>"
    $tenant = "<Azure tenant ID>";
    $subscription_id="<Azure subscription ID>";

    $rg = "ADFTutorialResourceGroup"
    $adf = "FirstDataFactoryREST"



## <a name="authenticate-with-aad"></a>Typ poświadczeń uwierzytelniania AAD

    $cmd = { .\curl.exe -X POST https://login.microsoftonline.com/$tenant/oauth2/token  -F grant_type=client_credentials  -F resource=https://management.core.windows.net/ -F client_id=$client_id -F client_secret=$client_secret };
    $responseToken = Invoke-Command -scriptblock $cmd;
    $accessToken = (ConvertFrom-Json $responseToken).access_token;
    
    (ConvertFrom-Json $responseToken) 



## <a name="create-data-factory"></a>Tworzenie factory danych

W tym kroku utworzysz Azure Factory danych o nazwie **FirstDataFactoryREST**. Factory danych może zawierać jedną lub więcej procesy. Potok może zawierać jedną lub więcej czynności w nim. Na przykład kopiowanie działanie Aby skopiować dane ze źródła do miejsca docelowego przechowywanie danych oraz działaniem gałąź HDInsight uruchamianie skryptu gałęzi do przekształcania danych. Uruchom następujące polecenia do tworzenia factory danych: 

1. Polecenie Przypisz do zmiennej o nazwie **cmd**. 

    Upewnij się, że nazwa fabryki danych określone w tym miejscu (ADFCopyTutorialDF) odpowiada nazwie określonego w **datafactory.json**. 

        $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data “@datafactory.json” https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/FirstDataFactoryREST?api-version=2015-10-01};
2. Uruchamianie polecenia za pomocą **Polecenia Wywołaj**.

        $results = Invoke-Command -scriptblock $cmd;
3. Wyświetlanie wyników. Po pomyślnym utworzeniu factory danych zostanie wyświetlony JSON dla factory danych w **wynikach**; w przeciwnym razie zostanie wyświetlony komunikat o błędzie.  

        Write-Host $results

Zwróć uwagę następujące punkty:
 
- Nazwa fabryki danych Azure musi być globalnie unikatowa. Jeśli zostanie wyświetlony błąd w wynikach: **factory danych o nazwie "FirstDataFactoryREST" nie jest dostępna**, wykonaj następujące czynności:  
    1. Zmień nazwę (na przykład yournameFirstDataFactoryREST) w pliku **datafactory.json** . Temat [Danych Factory - reguł nazewnictwa](data-factory-naming-rules.md) dla reguł nazewnictwa artefakty Factory danych.
    2. W pierwszym poleceniu gdzie zmienna **$cmd** jest przypisana wartość Zamień FirstDataFactoryREST pod nową nazwą i uruchom polecenie. 
    3. Uruchamianie następnych dwóch poleceń do wywołania interfejsu API usługi REST, aby utworzyć factory danych i wydrukować wyniki operacji. 
- Aby utworzyć wystąpienia Factory danych, musisz być współautora administrator Azure subskrypcji
- Nazwa fabryki danych może być zarejestrowana w przyszłości i w związku z tym stają się publicznie widoczne nazwy DNS.
- Jeśli zostanie wyświetlony komunikat o błędzie: "**tej subskrypcji nie została zarejestrowana jako nazw Microsoft.DataFactory**", wykonaj jedną z następujących czynności, a następnie ponownie spróbuj opublikować: 

    - W programie PowerShell Azure uruchom następujące polecenie, aby zarejestrować dostawcy Factory danych: 
        
            Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    
        Można uruchomić następujące polecenie, aby upewnić się, że dostawca jest zarejestrowany fabryki danych: 
    
            Get-AzureRmResourceProvider
    - Zaloguj się przy użyciu Azure subskrypcji do [Azure portal](https://portal.azure.com) i przejdź do strony karta Factory danych (lub) tworzenie factory danych w portalu Azure. Ta akcja automatycznie rejestruje dostawcy dla Ciebie.

Przed utworzeniem potok, musisz najpierw utworzyć kilka obiektów Factory danych. Należy najpierw utworzyć połączonych usług łącze danych sklepy-wyrażenie oblicza do magazynu danych, definiowanie danych wejściowych i zestawy danych do przedstawiania danych w sklepach połączone dane wyjściowe. 

## <a name="create-linked-services"></a>Tworzenie połączonych usług 
W tym kroku do których łącza Twoje konto Azure miejsca do magazynowania i klaster Azure HDInsight na żądanie firmie danych. Konta magazynu platformy Azure przechowuje dane wejściowe i wyjściowe procesu, w tym przykładzie. Usługa HDInsight połączone jest używana do uruchomienia skryptu gałęzi określony w działaniu potoku w tym przykładzie. 

### <a name="create-azure-storage-linked-service"></a>Tworzenie usługi Magazyn Azure połączone
W tym kroku konta magazynu platformy Azure połączyć firmie danych. Z tego samouczka możesz używać tego samego konta magazyn Azure do przechowywania danych wejścia i wyjścia i plik skryptu HQL.

1. Polecenie Przypisz do zmiennej o nazwie **cmd**. 

        $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data “@azurestoragelinkedservice.json” https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/linkedservices/AzureStorageLinkedService?api-version=2015-10-01};
2. Uruchamianie polecenia za pomocą **Polecenia Wywołaj**.
 
        $results = Invoke-Command -scriptblock $cmd;
3. Wyświetlanie wyników. Po pomyślnym utworzeniu usługi połączone zobaczysz JSON usługi połączone w **wynikach**; w przeciwnym razie zostanie wyświetlony komunikat o błędzie.
  
        Write-Host $results

### <a name="create-azure-hdinsight-linked-service"></a>Tworzenie usługa Azure HDInsight połączone
W tym kroku klastrze HDInsight na żądanie połączyć firmie danych. Klaster HDInsight jest automatycznie tworzone w czasie rzeczywistym i usunięte po zakończeniu przetwarzania i bezczynne przez określony czas. Klaster HDInsight można używać zamiast klastrze HDInsight na żądanie. Aby uzyskać szczegółowe informacje, zobacz [Obliczyć usługi połączone](data-factory-compute-linked-services.md) .  

1. Polecenie Przypisz do zmiennej o nazwie **cmd**.
 
        $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@hdinsightondemandlinkedservice.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/linkedservices/hdinsightondemandlinkedservice?api-version=2015-10-01};
2. Uruchamianie polecenia za pomocą **Polecenia Wywołaj**.

        $results = Invoke-Command -scriptblock $cmd;
3. Wyświetlanie wyników. Po pomyślnym utworzeniu usługi połączone zobaczysz JSON usługi połączone w **wynikach**; w przeciwnym razie zostanie wyświetlony komunikat o błędzie.  

        Write-Host $results

## <a name="create-datasets"></a>Tworzenie zestawów danych
W tym kroku utworzysz zestawy danych w celu reprezentują dane wejściowe i wyjściowe danych do przetwarzania gałęzi. Te zestawy danych dotyczą **StorageLinkedService** został utworzony wcześniej w tym samouczku. Wskazywany usługi połączone konto Azure miejsca do magazynowania i zestawy danych określić kontener, folder, nazwy pliku w magazynie, w którym znajduje się dane wejściowe i wyjściowe danych.   

### <a name="create-input-dataset"></a>Tworzenie zestawu wprowadzania danych
W tym kroku utworzysz zestawu wprowadzania danych do przedstawiania danych wejściowych przechowywane w magazynie obiektów Blob platformy Azure.

1. Polecenie Przypisz do zmiennej o nazwie **cmd**. 

        $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@inputdataset.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/AzureBlobInput?api-version=2015-10-01};
2. Uruchamianie polecenia za pomocą **Polecenia Wywołaj**.

        $results = Invoke-Command -scriptblock $cmd;
3. Wyświetlanie wyników. Jeśli zestaw danych został utworzony pomyślnie, zostanie wyświetlona JSON dla zestawu danych w **wynikach**; w przeciwnym razie zostanie wyświetlony komunikat o błędzie.
  
        Write-Host $results
### <a name="create-output-dataset"></a>Tworzenie zestawu danych wyjściowych danych
W tym kroku utworzysz zestawu danych dane wyjściowe reprezentować dane przechowywane w magazynie obiektów Blob platformy Azure.

1. Polecenie Przypisz do zmiennej o nazwie **cmd**.
 
        $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@outputdataset.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/AzureBlobOutput?api-version=2015-10-01};
2. Uruchamianie polecenia za pomocą **Polecenia Wywołaj**.

        $results = Invoke-Command -scriptblock $cmd;
3. Wyświetlanie wyników. Jeśli zestaw danych został utworzony pomyślnie, zostanie wyświetlona JSON dla zestawu danych w **wynikach**; w przeciwnym razie zostanie wyświetlony komunikat o błędzie.
  
        Write-Host $results 

## <a name="create-pipeline"></a>Tworzenie procesu
W tym kroku utworzysz pierwszego planowaną z działaniem **HDInsightHive** . Wprowadzania jest dostępny co miesiąc (częstotliwość: miesiąc, interwał: 1), dane wyjściowe wycinek powstaje miesięczny i właściwość harmonogram działania również jest ustawiona na co miesiąc. Ustawienia zestawu danych dane wyjściowe i harmonogram aktywności musi odpowiadać. Dane wyjściowe zestawu danych jest obecnie, co dyski harmonogram, należy utworzyć zestaw danych wyjściowych, nawet jeśli działania generuje żadnego wyniku. Jeśli działanie nie trwa wszelkie wprowadzone dane, możesz pominąć tworzenie zestawu danych wejściowych.  

Upewnij się, zobacz plik **input.log** w folderze **adfgetstarted/inputdata** w magazynie obiektów blob platformy Azure i uruchom następujące polecenie wdrożenie proces. **Początek** i **koniec** godziny są ustawiane w przeszłości i **isPaused** jest ustawiona na wartość FAŁSZ, planowana (czynność w potoku) jest uruchamiany natychmiast po wdrożeniu. 

1. Polecenie Przypisz do zmiennej o nazwie **cmd**.
 
        $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@pipeline.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datapipelines/MyFirstPipeline?api-version=2015-10-01};
2. Uruchamianie polecenia za pomocą **Polecenia Wywołaj**.

        $results = Invoke-Command -scriptblock $cmd;
3. Wyświetlanie wyników. Jeśli zestaw danych został utworzony pomyślnie, zostanie wyświetlona JSON dla zestawu danych w **wynikach**; w przeciwnym razie zostanie wyświetlony komunikat o błędzie.  

        Write-Host $results
5. Gratulacje, pomyślnie utworzono pierwszy planowaną przy użyciu programu PowerShell Azure!

## <a name="monitor-pipeline"></a>Potok monitora
W tym kroku możesz monitorowanie za pomocą interfejsu API usługi REST Factory danych wycinków wyprodukowane przez proces.

    $ds ="AzureBlobOutput"

    $cmd = {.\curl.exe -X GET -H "Authorization: Bearer $accessToken" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/$ds/slices?start=1970-01-01T00%3a00%3a00.0000000Z"&"end=2016-08-12T00%3a00%3a00.0000000Z"&"api-version=2015-10-01};

    $results2 = Invoke-Command -scriptblock $cmd;

    IF ((ConvertFrom-Json $results2).value -ne $NULL) {
        ConvertFrom-Json $results2 | Select-Object -Expand value | Format-Table
    } else {
            (convertFrom-Json $results2).RemoteException
    }


> [AZURE.IMPORTANT] 
> Tworzenie klastrze HDInsight na żądanie zwykle trwa daty (około 20 minut). W związku z tym proces zajmie **około 30 minut** przetwarzania wycinek z oczekiwaniami.  

Uruchom polecenie Wywołaj i następny, aż zobaczysz pozycję wycinek w stanie **gotowości** lub **awaria** . Gdy wycinek jest gotowa do drukowania, należy sprawdzić folder **partitioneddata** w kontenerze **adfgetstarted** w magazynie obiektów blob dla danych wyjściowych.  Tworzenie klastrze HDInsight na żądanie zazwyczaj trwa trochę czasu.

![dane wyjściowe](./media/data-factory-build-your-first-pipeline-using-rest-api/three-ouptut-files.png)

> [AZURE.IMPORTANT] Plik wejściowy otrzymuje usunięte po pomyślnym przetworzeniu wycinek. W związku z tym jeśli chcesz ponownie uruchomić wycinek lub wykonaj ponownie samouczka, Przekaż plik wejściowy (input.log) do folderu inputdata kontenera adfgetstarted.

Azure portal umożliwia również monitorowanie wycinków i rozwiązywanie problemów. Zobacz szczegóły [można monitorować za pomocą portalu Azure procesy](data-factory-build-your-first-pipeline-using-editor.md##monitor-pipeline) .  

## <a name="summary"></a>Podsumowanie 
W tym samouczku utworzono fabryki Azure danych, aby dane procesu, uruchamiając skrypt gałęzi w klastrze hadoop HDInsight. Edytor Factory danych jest używane w portalu Azure wykonywania następujących czynności:  

1.  Utworzony Azure **factory danych**.
2.  Tworzone dwa **połączone usług**:
    1.  **Magazyn Azure** połączone usługi do połączenia z magazynem obiektów blob Azure zawierający pliki wejścia i wyjścia do fabryki danych.
    2.  **Usługa Azure HDInsight** na żądanie powiązanych z utworzyć łącze z klastrem HDInsight Hadoop na żądanie factory danych. Azure Factory danych tworzy HDInsight Hadoop klaster tylko na czas przetwarzanie danych wejściowych i warzywa danych wyjściowych. 
3.  Tworzone dwa **zestawy danych**, które opisują dane wejściowe i wyjściowe wykonania gałąź HDInsight w potoku. 
4.  **Potok** utworzone za pomocą działaniem **Gałąź HDInsight** . 

## <a name="next-steps"></a>Następne kroki
W tym artykule utworzono potok aktywności przekształcenie (czynność HDInsight) uruchamia skrypt gałęzi w klastrze Azure HDInsight na żądanie. Aby zobaczyć, jak za pomocą działaniem kopii skopiuj dane z obiektów Blob platformy Azure SQL Azure, zobacz [Samouczek: Skopiuj dane z obiektów Blob platformy Azure SQL Azure](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

## <a name="see-also"></a>Zobacz też
| Temat | Opis |
| :---- | :---- |
| [Data Factory pozostałych API Reference](https://msdn.microsoft.com/library/azure/dn906738.aspx) |  Pełna w dokumentacji na polecenia cmdlet Factory danych |
| [Działania przekształcania danych](data-factory-data-transformation-activities.md) | Ten artykuł zawiera listę działań przekształcania danych (na przykład transformacja gałąź HDInsight używane w tym samouczku) obsługiwanych przez Azure danych Factory. |
| [Planowanie i wykonywanie](data-factory-scheduling-and-execution.md) | W tym artykule wyjaśniono, planowania i wykonanie aspektów model aplikacji Azure danych Factory. |
| [Procesy](data-factory-create-pipelines.md) | Ten artykuł pozwoli Ci zrozumienie potoki i działania w Azure Factory danych oraz sposób ich używać do tworzenia przepływów pracy opartych na danych kompleksowe — scenariusz lub firm. |
| [Zestawy danych](data-factory-create-datasets.md) | Ten artykuł ułatwia zrozumienie zestawów danych w Azure danych Factory.
| [Monitorowanie i zarządzanie procesy przy użyciu karty portal Azure](data-factory-monitor-manage-pipelines.md) | W tym artykule opisano, jak można monitorować, zarządzanie i debugowania usługi procesy przy użyciu karty portal Azure. |
| [Monitorowanie i zarządzanie nimi procesy przy użyciu aplikacji monitorowania](data-factory-monitor-manage-app.md) | W tym artykule opisano, jak można monitorować, zarządzanie i debugowanie procesy za pomocą monitorowania i zarządzania aplikacji. 

