<properties 
    pageTitle="Rozwiązywanie problemów Factory danych Azure" 
    description="Dowiedz się, jak rozwiązywać problemy z używaniem Azure danych Factory." 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/31/2016" 
    ms.author="spelluru"/>

# <a name="troubleshoot-data-factory-issues"></a>Rozwiązywanie problemów Factory danych
Ten artykuł zawiera porady dotyczące rozwiązywania problemów dla problemy podczas korzystania z platformy Azure Factory danych. Ten artykuł nie są wyświetlane wszystkie możliwe problemy podczas korzystania z usługi, ale obejmuje niektóre problemy i ogólne porady dotyczące rozwiązywania problemów.   

## <a name="troubleshooting-tips"></a>Porady dotyczące rozwiązywania problemów

### <a name="error-the-subscription-is-not-registered-to-use-namespace-microsoftdatafactory"></a>Błąd: Subskrypcja nie jest zarejestrowany używać nazw "Microsoft.DataFactory"
Jeśli wystąpi ten błąd, dostawcy zasobów Factory danych Azure nie został zarejestrowany na tym komputerze. Wykonaj następujące czynności: 

1. Uruchom program Azure PowerShell. 
2. Zaloguj się do konta Azure przy użyciu następującego polecenia.
        AzureRmAccount logowania 
3. Uruchom następujące polecenie, aby zarejestrować dostawcy Azure danych Factory.
        Microsoft.DataFactory - ProviderNamespace AzureRmResourceProvider rejestru

### <a name="problem-unauthorized-error-when-running-a-data-factory-cmdlet"></a>Problem: Nieautoryzowanego błąd podczas uruchamiania polecenia cmdlet Factory danych
Prawdopodobnie nie używasz konta prawo Azure lub innej subskrypcji przy użyciu programu PowerShell Azure. Użyj następujące polecenia cmdlet, aby wybrać konto prawo Azure i subskrypcji do użytku z Azure programu PowerShell. 

1. AzureRmAccount logowania — użyj Identyfikatora użytkownika i hasła
2. Get-AzureRmSubscription - wyświetlanie wszystkich subskrypcji dla konta. 
3. Wybierz pozycję AzureRmSubscription &lt;nazwę subskrypcji&gt; — Wybierz subskrypcję, do prawej. Użyj tych samych język, który umożliwia tworzenie factory danych w portalu Azure.

### <a name="problem-fail-to-launch-data-management-gateway-express-setup-from-azure-portal"></a>Problem: Nie można uruchomić instalacja Express bramy zarządzania danymi z Azure portal
Express konfiguracji bramy zarządzania danymi wymaga przeglądarki Internet Explorer lub przeglądarki sieci web zgodne ClickOnce firmy Microsoft. Jeśli konfiguracja Express nie można uruchomić, wykonaj jedną z następujących czynności: 

- Za pomocą programu Internet Explorer lub przeglądarki sieci web zgodne ClickOnce firmy Microsoft.

    Jeśli używasz przeglądarki Chrome, przejdź do [Sklep internetowy Chrome](https://chrome.google.com/webstore/)wyszukiwanie przy użyciu słów kluczowych "ClickOnce", wybierz jedną z rozszerzeniem ClickOnce i zainstaluj ją. 
    
    Zrób to samo dla przeglądarki Firefox (Zainstaluj dodatek). Kliknij przycisk Otwórz Menu na pasku narzędzi (trzy linie poziome w prawym górnym rogu), kliknij pozycję Dodatki, wyszukiwanie przy użyciu słów kluczowych "ClickOnce", wybierz jedną z rozszerzeniem ClickOnce i zainstaluj go. 

- Użyj łącza **Ręcznego konfigurowania** wyświetlane na tym samym karta w portalu. Ta metoda umożliwia pobieranie pliku instalacyjnego i uruchomić go ręcznie. Po pomyślnym zakończeniu instalacji, pojawi się okno dialogowe konfiguracji bramy zarządzania danymi. Skopiuj **klucza** na ekranie portalu i jej używać w Menedżerze konfiguracji ręcznie Zarejestruj bramę z usługą.  

### <a name="problem-fail-to-connect-to-on-premises-sql-server"></a>Problem: Nie można nawiązać połączenia lokalnego programu SQL Server 
Uruchom **Menedżer konfiguracji bramy zarządzania danymi** na komputerze bramy i karta **Rozwiązywanie problemów** do testowania połączenia z programem SQL Server z komputera bramy. Zobacz [bramy Rozwiązywanie problemów](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) , aby uzyskać porady dotyczące rozwiązywania problemów z połączenia/brama problemy związane z.   
 

### <a name="problem-input-slices-are-in-waiting-state-for-ever"></a>Problem: Wycinków wprowadzania znajdują się w oczekujące na kiedykolwiek Państwa

Wycinki może być w stanie **oczekiwania** z różnych powodów. Typowe przyczyny jest, że **zewnętrznych** właściwość nie jest ustawiona na **wartość true**. Dowolny zestaw danych powstaje poza zakresem fabryki danych Azure powinna być oznaczona właściwość **zewnętrznych** . Właściwość ta wskazuje, czy dane zewnętrzne i nie kopii przez wszystkie procesy w firmie danych. Fragmenty danych są oznaczone jako **Gotowe** , gdy dane są dostępne w sklepie odpowiednich. 

Zobacz przykład poniżej z użyciem właściwości **zewnętrznych** . Opcjonalnie można określić **externalData*** podczas ustawiania zewnętrznych na PRAWDA.

Zobacz [zestawy danych](data-factory-create-datasets.md) artykuł, aby uzyskać więcej informacji na temat tej właściwości.
    
    {
      "name": "CustomerTable",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "MyLinkedService",
        "typeProperties": {
          "folderPath": "MyContainer/MySubFolder/",
          "format": {
            "type": "TextFormat",
            "columnDelimiter": ",",
            "rowDelimiter": ";"
          }
        },
        "external": true,
        "availability": {
          "frequency": "Hour",
          "interval": 1
        },
        "policy": {
          }
        }
      }
    }

Aby rozwiązać problem, Dodaj właściwość **zewnętrznych** i sekcji opcjonalne **externalData** do definicji JSON Tabela wejściowa i Utwórz ponownie tabelę. 

### <a name="problem-hybrid-copy-operation-fails"></a>Problem: Operacja kopii hybrydowych nie powiedzie się.
Zobacz [Rozwiązywanie problemów bramy z](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) procedurą dotyczącą rozwiązywać problemy z kopiowanie z lokalnego magazynu danych za pomocą bramy zarządzania danymi. 

### <a name="problem-on-demand-hdinsight-provisioning-fails"></a>Problem: HDInsight na żądanie obsługi administracyjnej kończy się niepowodzeniem
Podczas korzystania z usługi połączone typu HDInsightOnDemand, musisz określić linkedServiceName, która kieruje do magazyn obiektów Blob platformy Azure. Usługa Factory danych korzysta z tego magazynu do przechowywania dzienniki i pliki pomocnicze klaster HDInsight na żądanie.  Czasami inicjowania obsługi administracyjnej klastrze HDInsight na żądanie nie powiodło się następujący komunikat o błędzie:

        Failed to create cluster. Exception: Unable to complete the cluster create operation. Operation failed with code '400'. Cluster left behind state: 'Error'. Message: 'StorageAccountNotColocated'.

Ten błąd wskazuje, że konta miejsca do magazynowania w linkedServiceName jest w tym samym miejscu centrum danych, gdzie się dzieje HDInsight inicjowania obsługi administracyjnej. Przykład: Jeśli firmie danych znajduje się w USA Zachodnia i Azure magazyn jest wschodniego w USA, na żądanie inicjowania obsługi administracyjnej zakończy się niepowodzeniem w USA Zachód.

Ponadto jest drugi additionalLinkedServiceNames właściwość JSON miejsce, w którym można określić konta dodatkowego miejsca do magazynowania w HDInsight na żądanie. Te dodatkowe miejsce do magazynowania połączone konta powinna być w tym samym miejscu co klaster HDInsight lub pojawia się ten sam błąd błąd.

### <a name="problem-custom-net-activity-fails"></a>Problem: Niestandardowe działania .NET nie powiodła się
Aby uzyskać szczegółowe instrukcje, zobacz [Debugowanie potok z niestandardowym działaniem](data-factory-use-custom-activities.md#debug-the-pipeline) . 

## <a name="use-azure-portal-to-troubleshoot"></a>Rozwiązywanie problemów za pomocą Azure portal 

### <a name="using-portal-blades"></a>Za pomocą karty portalu
Zobacz [Monitor planowana](data-factory-build-your-first-pipeline-using-editor.md#monitor-pipeline) dla czynności. 

### <a name="using-monitor-and-manage-app"></a>Za pomocą monitora i zarządzanie nimi aplikacji
Zobacz [Monitor i zarządzanie nimi procesy factory danych przy użyciu monitora i zarządzanie aplikacji](data-factory-monitor-manage-app.md) Aby uzyskać szczegółowe informacje. 

## <a name="use-azure-powershell-to-troubleshoot"></a>Rozwiązywanie problemów za pomocą programu PowerShell Azure

### <a name="use-azure-powershell-to-troubleshoot-an-error"></a>Rozwiązywanie problemów z błędem za pomocą programu PowerShell Azure  
Aby uzyskać szczegółowe informacje, zobacz [procesy Monitor Factory danych przy użyciu programu PowerShell Azure](data-factory-build-your-first-pipeline-using-powershell.md#monitor-pipeline) . 


[adfgetstarted]: data-factory-copy-data-from-azure-blob-storage-to-sql-database.md
[use-custom-activities]: data-factory-use-custom-activities.md
[troubleshoot]: data-factory-troubleshoot.md
[developer-reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[cmdlet-reference]: http://go.microsoft.com/fwlink/?LinkId=517456
[json-scripting-reference]: http://go.microsoft.com/fwlink/?LinkId=516971

[azure-portal]: https://portal.azure.com/

[image-data-factory-troubleshoot-with-error-link]: ./media/data-factory-troubleshoot/DataFactoryWithErrorLink.png

[image-data-factory-troubleshoot-datasets-with-errors-blade]: ./media/data-factory-troubleshoot/DatasetsWithErrorsBlade.png

[image-data-factory-troubleshoot-table-blade-with-problem-slices]: ./media/data-factory-troubleshoot/TableBladeWithProblemSlices.png

[image-data-factory-troubleshoot-activity-run-with-error]: ./media/data-factory-troubleshoot/ActivityRunDetailsWithError.png

[image-data-factory-troubleshoot-dataslice-blade-with-active-runs]: ./media/data-factory-troubleshoot/DataSliceBladeWithActivityRuns.png

[image-data-factory-troubleshoot-walkthrough2-with-errors-link]: ./media/data-factory-troubleshoot/Walkthrough2WithErrorsLink.png

[image-data-factory-troubleshoot-walkthrough2-datasets-with-errors]: ./media/data-factory-troubleshoot/Walkthrough2DataSetsWithErrors.png

[image-data-factory-troubleshoot-walkthrough2-table-with-problem-slices]: ./media/data-factory-troubleshoot/Walkthrough2TableProblemSlices.png

[image-data-factory-troubleshoot-walkthrough2-slice-activity-runs]: ./media/data-factory-troubleshoot/Walkthrough2DataSliceActivityRuns.png

[image-data-factory-troubleshoot-activity-run-details]: ./media/data-factory-troubleshoot/Walkthrough2ActivityRunDetails.png
 