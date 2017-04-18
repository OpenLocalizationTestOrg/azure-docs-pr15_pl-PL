<properties 
    pageTitle="Monitorowanie i zarządzanie zadaniami analizy strumieniu przy użyciu programu PowerShell | Microsoft Azure" 
    description="Dowiedz się, jak za pomocą programu PowerShell Azure i polecenia cmdlet monitorowania i zarządzania zadaniami analizy strumieniu." 
    keywords="Azure programu powershell, azure poleceń cmdlet, polecenia programu powershell skryptów programu powershell" 
    services="stream-analytics" 
    documentationCenter="" 
    authors="jeffstokes72" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="09/26/2016" 
    ms.author="jeffstok"/>


# <a name="monitor-and-manage-stream-analytics-jobs-with-azure-powershell-cmdlets"></a>Monitorowanie i zarządzanie zadaniami analizy strumieniu przy użyciu poleceń cmdlet środowiska PowerShell Azure

Dowiedz się, jak do monitorowania i zarządzania zasobami analizy strumieniu przy użyciu poleceń cmdlet środowiska PowerShell Azure i skryptów programu powershell, które wykonywanie podstawowych zadań analizy strumieniu.

## <a name="prerequisites-for-running-azure-powershell-cmdlets-for-stream-analytics"></a>Wymagania wstępne dotyczące uruchomiony poleceń cmdlet środowiska PowerShell Azure analizy strumieniu

 - Tworzenie grupy zasobów Azure w ramach subskrypcji. Poniżej przedstawiono przykładowy skrypt programu PowerShell Azure. Uzyskać Azure programu PowerShell, zobacz [Instalowanie i konfigurowanie programu PowerShell Azure](../powershell-install-configure.md);  

Azure programu PowerShell 0.9.8:  

        # Log in to your Azure account
        Add-AzureAccount

        # Select the Azure subscription you want to use to create the resource group if you have more than one subscription on your account.
        Select-AzureSubscription -SubscriptionName <subscription name>
 
        # If Stream Analytics has not been registered to the subscription, remove remark symbol below (#) to run the Register-AzureProvider cmdlet to register the provider namespace.
        #Register-AzureProvider -Force -ProviderNamespace 'Microsoft.StreamAnalytics'

        # Create an Azure resource group
        New-AzureResourceGroup -Name <YOUR RESOURCE GROUP NAME> -Location <LOCATION>

Azure programu PowerShell 1.0:  

        # Log in to your Azure account
        Login-AzureRmAccount

        # Select the Azure subscription you want to use to create the resource group.
        Get-AzureRmSubscription –SubscriptionName “your sub” | Select-AzureRmSubscription

        # If Stream Analytics has not been registered to the subscription, remove remark symbol below (#) to run the Register-AzureProvider cmdlet to register the provider namespace.
        #Register-AzureRmResourceProvider -Force -ProviderNamespace 'Microsoft.StreamAnalytics'
        
        # Create an Azure resource group
        New-AzureRMResourceGroup -Name <YOUR RESOURCE GROUP NAME> -Location <LOCATION>
        


> [AZURE.NOTE] Zadania analizy strumieniu utworzone programowo monitorowania, domyślnie nie jest konieczne.  Można ręcznie włączyć monitorowanie Azure Portal Przechodzenie do strony Monitor to zadanie, a następnie klikając przycisk Włącz lub programowy to zrobić, wykonując kroki znajdujący się w [Analizy strumieniu Azure - Monitor strumienia analizy zadań programowo](stream-analytics-monitor-jobs.md).

## <a name="azure-powershell-cmdlets-for-stream-analytics"></a>Azure poleceń cmdlet programu PowerShell dla analizy strumieniu
Następujące polecenia cmdlet programu PowerShell Azure może służyć do monitorowania i zarządzania zadaniami Azure analizy strumieniu. Należy zauważyć, że Azure programu PowerShell zawiera różnych wersji. 
**W przykładach na liście pierwszego polecenia dotyczy Azure programu PowerShell 0.9.8, drugie polecenie dotyczy Azure PowerShell 1.0.** Polecenia Azure PowerShell 1.0 zawsze będą miały "AzureRM" w tym poleceniu.

### <a name="get-azurestreamanalyticsjob--get-azurermstreamanalyticsjob"></a>Get-AzureStreamAnalyticsJob | Get-AzureRMStreamAnalyticsJob
Wyświetla listę wszystkich zadań analizy strumieniu zdefiniowana w subskrypcji Azure lub określoną grupę zasobów lub uzyskuje informacje o zadaniu dotyczące określonego zadania w grupie zasobów.

**Przykład 1**

Azure programu PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsJob

Azure programu PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsJob

Tego polecenia programu PowerShell zwraca informacje o wszystkich zadań analizy strumieniu Azure subskrypcji.

**Przykład 2**

Azure programu PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US 

Azure programu PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US 

Tego polecenia programu PowerShell zwraca informacje o wszystkich zadań analizy strumieniu w grupie zasobów StreamAnalytics domyślne-centralna — USA.

**Przykład 3**

Azure programu PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob

Azure programu PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob

Tego polecenia programu PowerShell zwraca informacje o zadaniu analizy strumieniu StreamingJob w grupie zasobów StreamAnalytics domyślne-centralna — USA.

### <a name="get-azurestreamanalyticsinput--get-azurermstreamanalyticsinput"></a>Get-AzureStreamAnalyticsInput | Get-AzureRMStreamAnalyticsInput
Wyświetla listę wszystkich danych wejściowych zdefiniowanych w określonym zadaniu analizy strumieniu lub pobiera informacje o określonych danych wejściowych.

**Przykład 1**

Azure programu PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

Azure programu PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

Tego polecenia programu PowerShell zwraca informacje o wszystkich danych wejściowych zdefiniowanych w zadaniu StreamingJob.

**Przykład 2**

Azure programu PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name EntryStream

Azure programu PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name EntryStream

Tego polecenia programu PowerShell zwraca informacje o dane wejściowe o nazwie EntryStream zdefiniowane w zadaniu StreamingJob.

### <a name="get-azurestreamanalyticsoutput--get-azurermstreamanalyticsoutput"></a>Get-AzureStreamAnalyticsOutput | Get-AzureRMStreamAnalyticsOutput
Wyświetla listę wszystkich wyjściowe, zdefiniowane w określonym zadaniu analizy strumieniu lub pobiera informacje o określonych danych wyjściowych.

**Przykład 1**

Azure programu PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

Azure programu PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

Tego polecenia programu PowerShell zwraca informacje o wyjściowe zdefiniowane w zadaniu StreamingJob.

**Przykład 2**

Azure programu PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name Output

Azure programu PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name Output

Tego polecenia programu PowerShell zwraca informacje o nazwie dane wyjściowe zdefiniowane w zadaniu StreamingJob dane wyjściowe.

### <a name="get-azurestreamanalyticsquota--get-azurermstreamanalyticsquota"></a>Get-AzureStreamAnalyticsQuota | Get-AzureRMStreamAnalyticsQuota
Pobiera informacje o przydziału strumieniowych jednostki w danym regionie.

**Przykład 1**

Azure programu PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsQuota –Location "Central US" 

Azure programu PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsQuota –Location "Central US" 

Tego polecenia programu PowerShell zwraca informacje o przydział i zastosowania jednostek strumieniowych w regionie centralnej USA.

### <a name="get-azurestreamanalyticstransformation--getazurermstreamanalyticstransformation"></a>Get-AzureStreamAnalyticsTransformation | GetAzureRMStreamAnalyticsTransformation
Pobiera informacje o odpowiednie przekształcenie zdefiniowane w zadaniu analizy strumieniu.

**Przykład 1**

Azure programu PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name StreamingJob

Azure programu PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name StreamingJob

Tego polecenia programu PowerShell zwraca informacje o przekształcenie o nazwie StreamingJob w zadaniu StreamingJob.

### <a name="new-azurestreamanalyticsinput--new-azurermstreamanalyticsinput"></a>Nowy AzureStreamAnalyticsInput | Nowy AzureRMStreamAnalyticsInput
Tworzy nowe dane wejściowe w ramach zadania analizy strumieniu lub aktualizuje wartość istniejących określonych danych wejściowych.
  
W pliku .json lub w wierszu polecenia można określić nazwę danych wejściowych. Jeśli określone oba nazwę w wierszu polecenia musi być taka sama, jak w pliku.

Jeśli Określ wprowadzania, który już istnieje, a nie określisz — wymuszanie parametr, polecenia cmdlet zostanie wyświetlone pytanie, czy chcesz zamienić istniejące dane wejściowe.

Jeśli użytkownik określi — wymuszanie parametru i określ istniejącego wprowadzić nazwę, dane wejściowe zostaną zastąpione bez potwierdzenia.

Aby uzyskać szczegółowe informacje o zawartości i struktury pliku JSON, odwołują się do [Tworzenie wprowadzania (analizy strumieniu Azure)] [ msdn-rest-api-create-stream-analytics-input] sekcja [strumienia analizy zarządzania pozostałych interfejsu API Reference Library][stream.analytics.rest.api.reference].

**Przykład 1**

Azure programu PowerShell 0.9.8:  

    New-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" 

Azure programu PowerShell 1.0:  

    New-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" 

Tego polecenia programu PowerShell tworzy nowe dane wejściowe z pliku Input.json. Jeśli już zdefiniowano istniejące dane wejściowe o podanej nazwie w pliku definicji wprowadzania, polecenia cmdlet zostanie wyświetlone pytanie, czy chcesz zastąpić ją.

**Przykład 2**

Azure programu PowerShell 0.9.8:  

    New-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream

Azure programu PowerShell 1.0:  

    New-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream

Tego polecenia programu PowerShell tworzy nowe dane wejściowe w zadaniu o nazwie EntryStream. Jeśli istniejące dane wejściowe o tej nazwie już jest zdefiniowana, polecenia cmdlet zostanie wyświetlone pytanie, czy chcesz zastąpić ją.

**Przykład 3**

Azure programu PowerShell 0.9.8:  

    New-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream -Force

Azure programu PowerShell 1.0:  

    New-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream -Force

Tego polecenia programu PowerShell zastępuje definicji istniejącego źródła wprowadzania danych o nazwie EntryStream z definicją z pliku.

### <a name="new-azurestreamanalyticsjob--new-azurermstreamanalyticsjob"></a>Nowy AzureStreamAnalyticsJob | Nowy AzureRMStreamAnalyticsJob
Tworzy nowe zadanie analizy strumieniu platformy Microsoft Azure lub aktualizuje wartość definicji istniejącego określonego zadania.

Nazwa zadania można określić w pliku .json lub w wierszu polecenia. Jeśli określone oba nazwę w wierszu polecenia musi być taka sama, jak w pliku.

Jeśli Określ nazwę zadania, która już istnieje, a nie określisz — wymuszanie parametr, polecenia cmdlet zostanie wyświetlone pytanie, czy chcesz zamienić istniejące zadanie.

Jeśli użytkownik określi — wymuszanie parametr, a następnie określ nazwę istniejącego zadania, definicji zadania zostaną zastąpione bez potwierdzenia.

Aby uzyskać szczegółowe informacje o zawartości i struktury pliku JSON, odwołują się do [Utworzyć zadanie analizy strumieniu] [ msdn-rest-api-create-stream-analytics-job] sekcja [strumienia analizy zarządzania pozostałych interfejsu API Reference Library][stream.analytics.rest.api.reference].

**Przykład 1**

Azure programu PowerShell 0.9.8:  

    New-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" 

Azure programu PowerShell 1.0:  

    New-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" 

Tego polecenia programu PowerShell tworzy nowe zadanie z definicji w JobDefinition.json. Jeśli już zdefiniowano istniejące zadanie o podanej nazwie w pliku definicji zadania, polecenia cmdlet zostanie wyświetlone pytanie, czy chcesz zastąpić ją.

**Przykład 2**

Azure programu PowerShell 0.9.8:  

    New-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" –Name StreamingJob -Force

Azure programu PowerShell 1.0:  

    New-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" –Name StreamingJob -Force

Tego polecenia programu PowerShell zastępuje definicji zadania dla StreamingJob.

### <a name="new-azurestreamanalyticsoutput--new-azurermstreamanalyticsoutput"></a>Nowy AzureStreamAnalyticsOutput | Nowy AzureRMStreamAnalyticsOutput
Tworzy nowe dane wyjściowe w ramach zadania analizy strumieniu lub aktualizuje wartość istniejące dane wyjściowe.  

Nazwa danych wyjściowych można określić w pliku .json lub w wierszu polecenia. Jeśli określone oba nazwę w wierszu polecenia musi być taka sama, jak w pliku.

Jeśli Określ wynik, który już istnieje, a nie określisz — wymuszanie parametr, polecenia cmdlet zostanie wyświetlone pytanie, czy chcesz zamienić istniejące dane wyjściowe.

Jeśli użytkownik określi — wymuszanie parametru i określ istniejące dane wyjściowe nazwy, dane wyjściowe zostaną zastąpione bez potwierdzenia.

Aby uzyskać szczegółowe informacje o zawartości i struktury pliku JSON, odwołują się do [Tworzenie wynik (analizy strumieniu Azure)] [ msdn-rest-api-create-stream-analytics-output] sekcja [strumienia analizy zarządzania pozostałych interfejsu API Reference Library][stream.analytics.rest.api.reference].

**Przykład 1**

Azure programu PowerShell 0.9.8:  

    New-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output

Azure programu PowerShell 1.0:  

    New-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output

Tego polecenia programu PowerShell tworzy nowe dane wyjściowe o nazwie "dane wyjściowe" w zadaniu StreamingJob. Jeśli istniejące dane wyjściowe o tej nazwie już jest zdefiniowana, polecenia cmdlet zostanie wyświetlone pytanie, czy chcesz zastąpić ją.

**Przykład 2**

Azure programu PowerShell 0.9.8:  

    New-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output -Force

Azure programu PowerShell 1.0:  

    New-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output -Force

Tego polecenia programu PowerShell zastępuje definicję "dane wyjściowe" w zadaniu StreamingJob.

### <a name="new-azurestreamanalyticstransformation--new-azurermstreamanalyticstransformation"></a>Nowy AzureStreamAnalyticsTransformation | Nowy AzureRMStreamAnalyticsTransformation
Tworzy nowe przekształcenie w ramach zadania analizy strumieniu lub aktualizuje wartość Przekształcanie istniejącego.
  
Nazwa transformacji można określić w pliku .json lub w wierszu polecenia. Jeśli określone oba nazwę w wierszu polecenia musi być taka sama, jak w pliku.

Jeśli Określ przekształcenie, które już istnieje, a nie określisz — wymuszanie parametr, polecenia cmdlet zostanie wyświetlone pytanie, czy chcesz zamienić istniejący przekształcenie.

Jeśli użytkownik określi — wymusić parametr, a następnie określ nazwę istniejącej przekształcenie, przekształcenie powoduje zastąpienie bez potwierdzenia.

Aby uzyskać szczegółowe informacje o zawartości i struktury pliku JSON, odwołują się do [Tworzenie przekształcenie (analizy strumieniu Azure)] [ msdn-rest-api-create-stream-analytics-transformation] sekcja [strumienia analizy zarządzania pozostałych interfejsu API Reference Library][stream.analytics.rest.api.reference].

**Przykład 1**

Azure programu PowerShell 0.9.8:  

    New-AzureStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform

Azure programu PowerShell 1.0:  

    New-AzureRMStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform

Tego polecenia programu PowerShell tworzy nowe przekształcenie o nazwie StreamingJobTransform w zadaniu StreamingJob. Jeśli o tej nazwie już zdefiniowano istniejących przekształcenie, polecenia cmdlet zostanie wyświetlone pytanie, czy chcesz zastąpić ją.

**Przykład 2**

Azure programu PowerShell 0.9.8:  

    New-AzureStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform -Force

Azure programu PowerShell 1.0:  

    New-AzureRMStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform -Force

 Tego polecenia programu PowerShell zastępuje definicji StreamingJobTransform w zadaniu StreamingJob.

### <a name="remove-azurestreamanalyticsinput--remove-azurermstreamanalyticsinput"></a>Usuń AzureStreamAnalyticsInput | Usuń AzureRMStreamAnalyticsInput
Asynchroniczne usuwa określonych danych wejściowych przy użyciu analizy strumieniu zadania w programie Microsoft Azure.  
Jeśli użytkownik określi — wymuszanie parametr, dane wejściowe zostaną usunięte bez potwierdzenia.

**Przykład 1**

Azure programu PowerShell 0.9.8:  

    Remove-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EventStream

Azure programu PowerShell 1.0:  

    Remove-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EventStream

Tego polecenia programu PowerShell usuwa wprowadzania EventStream zadania StreamingJob.  

### <a name="remove-azurestreamanalyticsjob--remove-azurermstreamanalyticsjob"></a>Usuń AzureStreamAnalyticsJob | Usuń AzureRMStreamAnalyticsJob
Usuwa asynchroniczne określonego zadania analizy strumieniu platformy Microsoft Azure.  
Jeśli użytkownik określi — wymuszanie parametr, zadanie zostanie usunięte bez potwierdzenia.

**Przykład 1**

Azure programu PowerShell 0.9.8:  

    Remove-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

Azure programu PowerShell 1.0:  

    Remove-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

Tego polecenia programu PowerShell usuwa zadania StreamingJob.  

### <a name="remove-azurestreamanalyticsoutput--remove-azurermstreamanalyticsoutput"></a>Usuń AzureStreamAnalyticsOutput | Usuń AzureRMStreamAnalyticsOutput
Asynchroniczne usuwa określone dane wyjściowe z zadanie analizy strumieniu platformy Microsoft Azure.  
Jeśli użytkownik określi — wymuszanie parametr, wynik jest usuwany bez potwierdzenia.

**Przykład 1**

Azure programu PowerShell 0.9.8:  

    Remove-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

Azure programu PowerShell 1.0:  

    Remove-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

Ten wynik wyjściowy w zadaniu StreamingJob usuwa polecenia programu PowerShell.  

### <a name="start-azurestreamanalyticsjob--start-azurermstreamanalyticsjob"></a>Rozpocznij AzureStreamAnalyticsJob | Rozpocznij AzureRMStreamAnalyticsJob
Asynchroniczne wdrożenie i rozpoczyna zadanie analizy strumieniu platformy Microsoft Azure.

**Przykład 1**

Azure programu PowerShell 0.9.8:  

    Start-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob -OutputStartMode CustomTime -OutputStartTime 2012-12-12T12:12:12Z

Azure programu PowerShell 1.0:  

    Start-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob -OutputStartMode CustomTime -OutputStartTime 2012-12-12T12:12:12Z

Tego polecenia programu PowerShell uruchamia zadanie StreamingJob czas rozpoczęcia niestandardowy format wyjściowy równa 12 grudnia 2012 12:12:12 UTC.


### <a name="stop-azurestreamanalyticsjob--stop-azurermstreamanalyticsjob"></a>Zatrzymaj AzureStreamAnalyticsJob | Zatrzymaj AzureRMStreamAnalyticsJob
Asynchroniczne przestaje analizy strumieniu zadania w programie Microsoft Azure i Anuluj przydzielania zasobów, które były był używany. Definicji zadania i metadane pozostają dostępne w ramach subskrypcji za pośrednictwem interfejsów API, zarządzanie i Azure portal tak, aby to zadanie można edytować i ponownie uruchomić. Nie zostanie obciążona dla zadania w stanie zatrzymania.

**Przykład 1**

Azure programu PowerShell 0.9.8:  

    Stop-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

Azure programu PowerShell 1.0:  

    Stop-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

Tego polecenia programu PowerShell zatrzymuje zadania StreamingJob.  

### <a name="test-azurestreamanalyticsinput--test-azurermstreamanalyticsinput"></a>Test AzureStreamAnalyticsInput | Test AzureRMStreamAnalyticsInput
Testy analizy strumieniu możliwość nawiązywania połączenia z określoną danych wejściowych.

**Przykład 1**

Azure programu PowerShell 0.9.8:  

    Test-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EntryStream

Azure programu PowerShell 1.0:  

    Test-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EntryStream

Tego polecenia programu PowerShell testów stanu połączenia wprowadzania EntryStream w StreamingJob.  

###<a name="test-azurestreamanalyticsoutput--test-azurermstreamanalyticsoutput"></a>Test AzureStreamAnalyticsOutput | Test AzureRMStreamAnalyticsOutput
Testuje możliwość nawiązywania połączenia z określoną wynik analizy strumieniu.

**Przykład 1**

Azure programu PowerShell 0.9.8:  

    Test-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

Azure programu PowerShell 1.0:  

    Test-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

Ten stan połączenia danych wyjściowych danych wyjściowych w StreamingJob testy polecenia programu PowerShell.  

## <a name="get-support"></a>Uzyskiwanie pomocy technicznej
Aby uzyskać dodatkową pomoc spróbuj nasze [forum Azure analizy strumieniu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics). 


## <a name="next-steps"></a>Następne kroki

- [Wprowadzenie do analizy strumieniu Azure](stream-analytics-introduction.md)
- [Wprowadzenie do korzystania z analizy strumieniu Azure](stream-analytics-get-started.md)
- [Skalowanie Azure analizy strumieniu zadania](stream-analytics-scale-jobs.md)
- [Dokumentacja języka kwerend analizy strumieniu Azure](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Strumień Azure analizy zarządzania pozostałych interfejsu API odwołania](https://msdn.microsoft.com/library/azure/dn835031.aspx)
 



[msdn-switch-azuremode]: http://msdn.microsoft.com/library/dn722470.aspx
[powershell-install]: http://azure.microsoft.com/documentation/articles/powershell-install-configure/
[msdn-rest-api-create-stream-analytics-job]: https://msdn.microsoft.com/library/dn834994.aspx
[msdn-rest-api-create-stream-analytics-input]: https://msdn.microsoft.com/library/dn835010.aspx
[msdn-rest-api-create-stream-analytics-output]: https://msdn.microsoft.com/library/dn835015.aspx
[msdn-rest-api-create-stream-analytics-transformation]: https://msdn.microsoft.com/library/dn835007.aspx

[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-get-started.md
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
 
