<properties 
    pageTitle="Uruchamianie i zatrzymywanie maszyn wirtualnych przy użyciu automatyzacji Azure - przepływu pracy programu PowerShell | Microsoft Azure"
    description="Graficzne wersja scenariusz automatyzacji Azure, łącznie z runbooks do rozpoczynania i kończenia klasyczny maszyn wirtualnych."
    services="automation"
    documentationCenter=""
    authors="mgoedtel"
    manager="jwhit"
    editor="tysonn" />
<tags 
    ms.service="automation"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="07/06/2016"
    ms.author="bwren" />

# <a name="azure-automation-scenario---starting-and-stopping-virtual-machines"></a>Azure scenariusz automatyzacji - uruchamianie i zatrzymywanie maszyn wirtualnych

W tym scenariuszu automatyzacji Azure zawiera runbooks do rozpoczynania i kończenia klasycznym maszyn wirtualnych.  W tym scenariuszu służą do dowolnej z następujących czynności:  

- Użyj runbooks bez zmian w środowisku. 
- Modyfikowanie runbooks do wykonywania niestandardowych funkcjach.  
- Zadzwoń runbooks z innego działań aranżacji jako część ogólnego rozwiązania. 
- Użyj runbooks jako samouczki, aby dowiedzieć się działań aranżacji tworzenia pojęcia. 

> [AZURE.SELECTOR]
- [Graficzne](automation-solution-startstopvm-graphical.md)
- [Przepływ pracy programu PowerShell](automation-solution-startstopvm-psworkflow.md)

To jest wersja działań aranżacji przepływu pracy programu PowerShell tego scenariusza. Jest również dostępne przy użyciu [graficznego runbooks](automation-solution-startstopvm-graphical.md).

## <a name="getting-the-scenario"></a>Wprowadzenie tego scenariusza

W tym scenariuszu składa się z dwóch runbooks przepływu pracy programu PowerShell, które można pobrać z poniższych łączy.  Zobacz [graficzną wersję](automation-solution-startstopvm-graphical.md) programu w tym scenariuszu łącza do graficznego runbooks.

| Działań aranżacji | Łącze | Typ | Opis |
|:---|:---|:---|:---|
| Rozpocznij AzureVMs | [Rozpoczynanie Azure klasyczny maszyny wirtualne](https://gallery.technet.microsoft.com/Start-Azure-Classic-VMs-86ef746b) | Przepływ pracy programu PowerShell | Powoduje uruchomienie wszystkich klasyczny maszyn wirtualnych w Azure subscriptionor wszystkich maszyn wirtualnych przy użyciu nazwy określonej usługi. |
| Zatrzymaj AzureVMs | [Zatrzymywanie Azure klasyczny maszyny wirtualne](https://gallery.technet.microsoft.com/Stop-Azure-Classic-VMs-7a4ae43e) | Przepływ pracy programu PowerShell | Zatrzymuje wszystkie maszyny wirtualne automatyzacji konta lub wszystkich maszyn wirtualnych przy użyciu nazwy określonej usługi.  |


## <a name="installing-and-configuring-the-scenario"></a>Instalowanie i konfigurowanie tego scenariusza

### <a name="1-install-the-runbooks"></a>1. Instalowanie runbooks

Po pobraniu runbooks, możesz zaimportować je przy użyciu procedury importowanie [działań aranżacji](http://msdn.microsoft.com/library/dn643637.aspx#ImportRunbook).

### <a name="2-review-the-description-and-requirements"></a>2. Przejrzyj opis i wymagania
Runbooks zawierać tekst komentarzy pomocy, który zawiera opis i wymaganych zasobów.  Możesz również wyświetlić te same informacje z tego artykułu. 

### <a name="3-configure-assets"></a>3. Konfigurowanie składników majątku
Runbooks wymagają następujące zasoby, które należy utworzyć i wypełniać odpowiednie wartości.

| Typ zawartości | Nazwy zasobów | Opis |
|:---|:---|:---|:---|
| Poświadczenia | AzureCredential | Zawiera poświadczenia dla konta, które ma uprawnienia do rozpoczynania i kończenia maszyn wirtualnych Azure subskrypcji.  Można też kliknąć można określić inny trwały poświadczeń w parametr **Credential** działania **AzureAccount Dodaj** . |
| Zmienna | AzureSubscriptionId | Zawiera identyfikator subskrypcji Azure subskrypcji. |

## <a name="using-the-scenario"></a>Za pomocą tego scenariusza

### <a name="parameters"></a>Parametry

Runbooks każdego mają następujące parametry.  Należy podać wartości parametrów obowiązkowe i opcjonalnie można podać wartości dla innych parametrów w zależności od potrzeb.

| Parametr | Typ | Obowiązkowe | Opis |
|:---|:---|:---|:---|
| Nazwausługi | ciąg | Brak | Jeśli wartość jest dostępne, wszystkie maszyn wirtualnych o tej nazwie usługi jest uruchomiona lub zatrzymana.  Jeśli wartość nie jest dostępne, wszystkie klasyczny środowisku maszyn wirtualnych systemu Azure subskrypcji jest uruchomiona lub zatrzymana. |
| AzureSubscriptionIdAssetName | ciąg | Brak | Zawiera nazwę [zmiennej zawartości](#installing-and-configuring-the-scenario) , który zawiera identyfikator subskrypcji subskrypcji usługi Azure.  Jeśli nie zostanie określona wartość, *AzureSubscriptionId* jest używany.  |
| AzureCredentialAssetName | ciąg | Brak | Zawiera nazwę [trwałego poświadczeń](#installing-and-configuring-the-scenario) , zawierający poświadczenia dla działań aranżacji korzystać.  Jeśli nie zostanie określona wartość, *AzureCredential* jest używany.  |

### <a name="starting-the-runbooks"></a>Uruchamianie runbooks

Umożliwia metod przy [uruchamianiu działań aranżacji w automatyzacji Azure](automation-starting-a-runbook.md) rozpocząć albo runbooks w tym scenariuszu.

Następujące polecenia przykładowy używa programu Windows PowerShell do uruchomienia **StartAzureVMs** , aby rozpocząć wszystkich maszyn wirtualnych z nazwą usługi *MyVMService*.

    $params = @{"ServiceName"="MyVMService"}
    Start-AzureAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Start-AzureVMs" –Parameters $params

### <a name="output"></a>Wynik

Runbooks będzie [wyjściowy wiadomości](automation-runbook-output-and-messages.md) dla każdego wskazująca maszyn wirtualnych czy instrukcji zacznij lub Przestań zostały pomyślnie przesłane.  Możesz wyszukiwać określony ciąg w wyniki w celu ustalenia wyników dla każdego działań aranżacji.  Ciągi danych wyjściowych są wymienione w poniższej tabeli.

| Działań aranżacji | Warunek | Komunikat |
|:---|:---|:---|
| Rozpocznij AzureVMs | Maszyn wirtualnych jest już uruchomiony.  | MyVM jest już uruchomiony. |
| Rozpocznij AzureVMs | Żądanie uruchomienia dla maszyn wirtualnych pomyślnie | Rozpoczęto MyVM |
| Rozpocznij AzureVMs | Żądanie rozpoczęcia maszyn wirtualnych nie powiodło się  | Nie można uruchomić MyVM |
| Zatrzymaj AzureVMs | Już zatrzymaniu maszyn wirtualnych  | MyVM jest już zatrzymany |
| Zatrzymaj AzureVMs | Zatrzymywanie żądania maszyn wirtualnych pomyślnie | MyVM został zatrzymany |
| Zatrzymaj AzureVMs | Żądanie zatrzymania maszyn wirtualnych nie powiodło się  | Nie można zatrzymać MyVM |

Na przykład poniższy fragment kodu z działań aranżacji próbuje uruchomić wszystkich maszyn wirtualnych o nazwie usługi *MyServiceName*.  Jeśli dowolny z błędów żądania rozpoczęcia mogą być wykonywane akcje błędu. 

    $results = Start-AzureVMs -ServiceName "MyServiceName"
    foreach ($result in $results) {
        if ($result -like "* has been started" ) {
            # Action to take in case of success.
        }
        else {
            # Action to take in case of error.
        }
    }


## <a name="detailed-breakdown"></a>Szczegółowy podział

Oto szczegółowy podział runbooks w tym scenariuszu.  Za pomocą tych informacji do dostosowywania runbooks albo tylko informacje z nich do tworzenia własnych scenariusze automatyzacji.

### <a name="parameters"></a>Parametry

    param (
        [Parameter(Mandatory=$false)] 
        [String]  $AzureCredentialAssetName = 'AzureCredential',
        
        [Parameter(Mandatory=$false)]
        [String] $AzureSubscriptionIdAssetName = 'AzureSubscriptionId',

        [Parameter(Mandatory=$false)] 
        [String] $ServiceName
    )

Przepływ pracy jest uruchamiany przez wprowadzenie wartości [parametrów wejściowych](#using-the-scenario).  Jeśli nazwy zasobów nie mają nazw domyślnych są używane.

### <a name="output"></a>Wynik

    # Returns strings with status messages
    [OutputType([String])]

Ten wiersz oświadcza, że wynik działań aranżacji będzie ciąg.  Nie jest wymagane, ale jest najlepiej w przypadku działań aranżacji jest używana jako [działań aranżacji podrzędny](automation-child-runbooks.md) , dzięki czemu będą wiedzieć, działań aranżacji nadrzędny typ danych wyjściowych mogą się spodziewać.

### <a name="authentication"></a>Uwierzytelnianie

    # Connect to Azure and select the subscription to work against
    $Cred = Get-AutomationPSCredential -Name $AzureCredentialAssetName
    $null = Add-AzureAccount -Credential $Cred -ErrorAction Stop
    $SubId = Get-AutomationVariable -Name $AzureSubscriptionIdAssetName
    $null = Select-AzureSubscription -SubscriptionId $SubId -ErrorAction Stop

Następne wiersze ustaw [poświadczenia](automation-configuring.md#configuring-authentication-to-azure-resources) i Azure subskrypcji, która będzie używana dla każdego zestawu działań aranżacji.
Najpierw stosujemy **Get-AutomationPSCredential** , aby uzyskać zawartości, który przechowuje poświadczenia z dostępem do rozpoczynania i kończenia środowisku maszyn wirtualnych systemu Azure subskrypcji. Następnie **Dodaj AzureAccount** używa tej zawartości pozwala ustawić poświadczenia.  Wynik jest przypisana do zmiennej fikcyjna, dzięki czemu nie został uwzględniony w wyniku działań aranżacji.  

Zmienna elementu z Identyfikatorem subskrypcji następnie jest pobierana z **Get-AutomationVariable** i zestaw z **AzureSubscription wybierz**subskrypcję.

### <a name="get-vms"></a>Uzyskiwanie maszyny wirtualne

    # If there is a specific cloud service, then get all VMs in the service,
    # otherwise get all VMs in the subscription.
    if ($ServiceName) 
    { 
        $VMs = Get-AzureVM -ServiceName $ServiceName
    }
    else 
    { 
        $VMs = Get-AzureVM
    }

**Get-AzureVM** jest używana do pobierania maszyn wirtualnych, który pomoże działań aranżacji.  Jeśli wartość jest zmienną wprowadzania **NazwaUsługi** , tylko maszyn wirtualnych o tej nazwie usługi są pobierane.  Jeśli **NazwaUsługi** jest pusta, są pobierane wszystkie maszyn wirtualnych.

### <a name="startstop-virtual-machines-and-send-output"></a>Rozpocznij i Zatrzymaj maszyn wirtualnych i Wyślij dane wyjściowe

    # Start each of the stopped VMs
    foreach ($VM in $VMs)
    {
        if ($VM.PowerState -eq "Started")
        {
            # The VM is already started, so send notice
            Write-Output ($VM.InstanceName + " is already running")
        }
        else
        {
            # The VM needs to be started
            $StartRtn = Start-AzureVM -Name $VM.Name -ServiceName $VM.ServiceName -ErrorAction Continue

            if ($StartRtn.OperationStatus -ne 'Succeeded')
            {
                # The VM failed to start, so send notice
                Write-Output ($VM.InstanceName + " failed to start")
            }
            else
            {
                # The VM started, so send notice
                Write-Output ($VM.InstanceName + " has been started")
            }
        }
    }

Następne wiersze kroków poszczególnych maszyn wirtualnych.  Najpierw **PowerState** maszyny wirtualnej jest zaznaczone pole wyboru, aby sprawdzić, czy jest już uruchomiona lub zatrzymana, w zależności od działań aranżacji.  Jeśli jest już w stanie docelowej, wiadomość jest wysyłana do dane wyjściowe i zakończeniem działań aranżacji.  Jeśli nie, następnie **Start AzureVM** lub **Zatrzymaj AzureVM** służą próbuje uruchomić lub zatrzymać maszyny wirtualnej w wyniku żądania przechowywana w zmiennej.  Wiadomości są następnie wysyłane w celu wyprowadzenia, określając, czy żądania uruchamianie lub zatrzymywanie został pomyślnie przesłany.


## <a name="next-steps"></a>Następne kroki

- Aby uzyskać więcej informacji na temat pracy z runbooks podrzędny, zobacz [runbooks podrzędny w automatyzacji Azure](automation-child-runbooks.md) 
- Aby uzyskać więcej informacji na temat wyjściowe komunikaty podczas wykonywania działań aranżacji i rejestrowania w celu ułatwienia rozwiązania, zobacz [dane wyjściowe działań aranżacji i wiadomości w automatyzacji Azure](automation-runbook-output-and-messages.md)
