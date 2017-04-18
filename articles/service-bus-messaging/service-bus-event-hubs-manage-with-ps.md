<properties
    pageTitle="Zarządzanie zasobami usługi Bus i koncentratory zdarzeń za pomocą programu PowerShell | Microsoft Azure"
    description="Tworzenie i zarządzanie zasobami Bus usługi i koncentratory zdarzeń za pomocą programu PowerShell"
    services="service-bus,event-hubs"
    documentationCenter=".NET"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/04/2016"
    ms.author="sethm"/>

# <a name="use-powershell-to-manage-service-bus-and-event-hubs-resources"></a>Za pomocą programu PowerShell do zarządzania zasobami usługi Bus i koncentratory zdarzenia

Microsoft Azure programu PowerShell jest środowiskiem skryptów, które umożliwiają kontrolę i automatyzację wdrażania i zarządzania usług Azure. Ten artykuł zawiera opis sposobu obsługi administracyjnej i zarządzanie nimi podmioty Bus usługi, takie jak przestrzenie nazw, kolejki i koncentratory wydarzenia przy użyciu lokalnej konsoli programu PowerShell Azure za pomocą programu PowerShell.

## <a name="prerequisites"></a>Wymagania wstępne

Zanim zaczniesz, musisz następujące czynności:

- Subskrypcję usługi Azure. Azure jest platformą subskrybowanej. Aby uzyskać więcej informacji na temat uzyskiwania subskrypcji zobacz [Opcje zakupu][], [zawiera element członkowski][]lub [bezpłatne konto][].

- Komputer z programem PowerShell Azure. Aby uzyskać instrukcje, zobacz [Instalowanie i konfigurowanie programu PowerShell Azure][].

- Ogólne zrozumienie skryptów programu PowerShell, NuGet pakietów i .NET Framework.

## <a name="include-a-reference-to-the-net-assembly-for-service-bus"></a>Odwołanie do zestawu .NET dla usługi Bus

Istnieje ograniczoną liczbę poleceń cmdlet środowiska PowerShell do zarządzania Bus usługi. Do zapewniania obsługi obiektów, które nie są dostępne za pośrednictwem istniejącego polecenia cmdlet, służy klienta programu .NET dla usługi Bus z poziomu programu PowerShell przez odwoływanie się do [usługi Bus NuGet pakietu].

Najpierw upewnij się, że skrypt można znaleźć zestawu **Microsoft.ServiceBus.dll** , która jest instalowana z pakietem NuGet. W celu elastyczne, skrypt wykonuje następujące czynności:

1. Określa ścieżkę, z którego został wywołany.
2. Przechodzi ścieżkę, aż do znalezienia folder o nazwie `packages`. Podczas instalowania pakietów NuGet jest tworzony ten folder.
3. Wyszukiwanie lokalizacji `packages` folder dla zestawu o nazwie **Microsoft.ServiceBus.dll**.
4. Odwołania zestawu, dzięki czemu typy są dostępne w celu późniejszego użycia.

Oto implementacji poniższe czynności w obszarze skrypt programu PowerShell:

```powershell

try
{
    # IMPORTANT: Make sure to reference the latest version of Microsoft.ServiceBus.dll
    Write-Output "Adding the [Microsoft.ServiceBus.dll] assembly to the script..."
    $scriptPath = Split-Path (Get-Variable MyInvocation -Scope 0).Value.MyCommand.Path
    $packagesFolder = (Split-Path $scriptPath -Parent) + "\packages"
    $assembly = Get-ChildItem $packagesFolder -Include "Microsoft.ServiceBus.dll" -Recurse
    Add-Type -Path $assembly.FullName

    Write-Output "The [Microsoft.ServiceBus.dll] assembly has been successfully added to the script."
}

catch [System.Exception]
{
    Write-Error("Could not add the Microsoft.ServiceBus.dll assembly to the script. Make sure you build the solution before running the provisioning script.")
}

```

## <a name="provision-a-service-bus-namespace"></a>Inicjowanie obsługi usługi Bus przestrzeń nazw

Podczas pracy z nazw usługi Bus, istnieją dwa polecenia cmdlet zamiast .NET SDK można użyć: [Get-AzureSBNamespace][] i [AzureSBNamespace nowy][].

W tym przykładzie tworzy kilka zmienne lokalne w skrypt; `$Namespace` and `$Location`.

- `$Namespace`jest to nazwa nazw Bus usługi, z którą chcemy pracy.
- `$Location`Służy do identyfikowania centrum danych, w której będzie możemy obsługi administracyjnej obszaru nazw.
- `$CurrentNamespace`przechowuje nazw dla odwołania możemy pobrać (lub Utwórz).

W polu Rzeczywisty skrypt `$Namespace` i `$Location` mogą być przekazywane jako parametry.

Ta część skrypt wykonuje następujące czynności:

1. Pozwala podjąć próbę pobrania przestrzeń nazw Bus usługi o podanej nazwie.
2. Przestrzeń nazw zostanie znaleziony, raportów, co można odnaleźć.
3. Jeśli nie ma robocze, tworzy obszaru nazw, a następnie pobierze nowo utworzonego nazw.

    ``` powershell

    $Namespace = "MyServiceBusNS"
    $Location = "West US"

    # Query to see if the namespace currently exists
    $CurrentNamespace = Get-AzureSBNamespace -Name $Namespace

    # Check if the namespace already exists or needs to be created
    if ($CurrentNamespace)
    {
        Write-Output "The namespace [$Namespace] already exists in the [$($CurrentNamespace.Region)] region."
    }
    else
    {
        Write-Host "The [$Namespace] namespace does not exist."
        Write-Output "Creating the [$Namespace] namespace in the [$Location] region..."
        New-AzureSBNamespace -Name $Namespace -Location $Location -CreateACSNamespace false -NamespaceType Messaging
        $CurrentNamespace = Get-AzureSBNamespace -Name $Namespace
        Write-Host "The [$Namespace] namespace in the [$Location] region has been successfully created."
    }
    ```
Do zapewniania obsługi innych podmiotów Bus usługi, należy utworzyć wystąpienia `NamespaceManager` obiekt z zestawu SDK. Za pomocą polecenia cmdlet [Get-AzureSBAuthorizationRule][] do pobierania reguła autoryzacji, która jest używana do dostarczania parametry połączenia. W tym przykładzie zawiera odwołanie do `NamespaceManager` wystąpienia w `$NamespaceManager` zmiennej. Skrypt później używa `$NamespaceManager` do zapewniania obsługi innych podmiotów.

    ``` powershell
    $sbr = Get-AzureSBAuthorizationRule -Namespace $Namespace
    # Create the NamespaceManager object to create the Event Hub
    Write-Output "Creating a NamespaceManager object for the [$Namespace] namespace..."
    $NamespaceManager = [Microsoft.ServiceBus.NamespaceManager]::CreateFromConnectionString($sbr.ConnectionString);
    Write-Output "NamespaceManager object for the [$Namespace] namespace has been successfully created."
    ```

## <a name="provisioning-other-service-bus-entities"></a>Obsługa administracyjna osoby Bus usługi

Inicjowanie obsługi innymi obiektami, takich jak kolejki, tematy i koncentratory zdarzenia umożliwia [.NET interfejsu API usługi Bus][]. Przykłady bardziej szczegółowe, łącznie z innymi obiektami, odwołuje się na końcu tego artykułu.

### <a name="create-an-event-hub"></a>Tworzenie Centrum zdarzenia

Ta część skrypt tworzy cztery więcej zmienne lokalne. Są one używane wystąpienia `EventHubDescription` obiektu. Skrypt wykonuje następujące czynności:

1. Za pomocą `NamespaceManager` obiekt, sprawdź, jeśli Centrum zdarzeń oznaczonej numerem `$Path` istnieje.
2. Jeśli nie istnieje, Utwórz `EventHubDescription` i przekazać do `NamespaceManager` klasy `CreateEventHubIfNotExists` metody.
3. Po określeniu, że Centrum zdarzenie jest dostępna, Utwórz grupę dla klientów indywidualnych przy użyciu `ConsumerGroupDescription` i `NamespaceManager`.

    ``` powershell

    $Path  = "MyEventHub"
    $PartitionCount = 12
    $MessageRetentionInDays = 7
    $UserMetadata = $null
    $ConsumerGroupName = "MyConsumerGroup"

    # Check if the Event Hub already exists
    if ($NamespaceManager.EventHubExists($Path))
    {
        Write-Output "The [$Path] event hub already exists in the [$Namespace] namespace."  
    }
    else
    {
        Write-Output "Creating the [$Path] event hub in the [$Namespace] namespace: PartitionCount=[$PartitionCount] MessageRetentionInDays=[$MessageRetentionInDays]..."
        $EventHubDescription = New-Object -TypeName Microsoft.ServiceBus.Messaging.EventHubDescription -ArgumentList $Path
        $EventHubDescription.PartitionCount = $PartitionCount
        $EventHubDescription.MessageRetentionInDays = $MessageRetentionInDays
        $EventHubDescription.UserMetadata = $UserMetadata
        $EventHubDescription.Path = $Path
        $NamespaceManager.CreateEventHubIfNotExists($EventHubDescription);
        Write-Output "The [$Path] event hub in the [$Namespace] namespace has been successfully created."
    }

    # Create the consumer group if it doesn't exist
    Write-Output "Creating the consumer group [$ConsumerGroupName] for the [$Path] event hub..."
    $ConsumerGroupDescription = New-Object -TypeName Microsoft.ServiceBus.Messaging.ConsumerGroupDescription -ArgumentList $Path, $ConsumerGroupName
    $ConsumerGroupDescription.UserMetadata = $ConsumerGroupUserMetadata
    $NamespaceManager.CreateConsumerGroupIfNotExists($ConsumerGroupDescription);
    Write-Output "The consumer group [$ConsumerGroupName] for the [$Path] event hub has been successfully created."
    ```

### <a name="create-a-queue"></a>Tworzenie kolejki

Aby utworzyć kolejki lub tematu, sprawdzanie nazw tak jak w poprzedniej sekcji.

    if ($NamespaceManager.QueueExists($Path))
    {
        Write-Output "The [$Path] queue already exists in the [$Namespace] namespace."
    }
    else
    {
        Write-Output "Creating the [$Path] queue in the [$Namespace] namespace..."
        $QueueDescription = New-Object -TypeName Microsoft.ServiceBus.Messaging.QueueDescription -ArgumentList $Path
        if ($AutoDeleteOnIdle -ge 5)
        {
            $QueueDescription.AutoDeleteOnIdle = [System.TimeSpan]::FromMinutes($AutoDeleteOnIdle)
        }
        if ($DefaultMessageTimeToLive -gt 0)
        {
            $QueueDescription.DefaultMessageTimeToLive = [System.TimeSpan]::FromMinutes($DefaultMessageTimeToLive)
        }
        if ($DuplicateDetectionHistoryTimeWindow -gt 0)
        {
            $QueueDescription.DuplicateDetectionHistoryTimeWindow = [System.TimeSpan]::FromMinutes($DuplicateDetectionHistoryTimeWindow)
        }
        $QueueDescription.EnableBatchedOperations = $EnableBatchedOperations
        $QueueDescription.EnableDeadLetteringOnMessageExpiration = $EnableDeadLetteringOnMessageExpiration
        $QueueDescription.EnableExpress = $EnableExpress
        $QueueDescription.EnablePartitioning = $EnablePartitioning
        $QueueDescription.ForwardDeadLetteredMessagesTo = $ForwardDeadLetteredMessagesTo
        $QueueDescription.ForwardTo = $ForwardTo
        $QueueDescription.IsAnonymousAccessible = $IsAnonymousAccessible
        if ($LockDuration -gt 0)
        {
            $QueueDescription.LockDuration = [System.TimeSpan]::FromSeconds($LockDuration)
        }
        $QueueDescription.MaxDeliveryCount = $MaxDeliveryCount
        $QueueDescription.MaxSizeInMegabytes = $MaxSizeInMegabytes
        $QueueDescription.RequiresDuplicateDetection = $RequiresDuplicateDetection
        $QueueDescription.RequiresSession = $RequiresSession
        if ($EnablePartitioning)
        {
            $QueueDescription.SupportOrdering = $False
        }
        else
        {
            $QueueDescription.SupportOrdering = $SupportOrdering
        }
        $QueueDescription.UserMetadata = $UserMetadata
        $NamespaceManager.CreateQueue($QueueDescription);
    }

### <a name="create-a-topic"></a>Tworzenie tematu

    if ($NamespaceManager.TopicExists($Path))
    {
        Write-Output "The [$Path] topic already exists in the [$Namespace] namespace."
    }
    else
    {
        Write-Output "Creating the [$Path] topic in the [$Namespace] namespace..."
        $TopicDescription = New-Object -TypeName Microsoft.ServiceBus.Messaging.TopicDescription -ArgumentList $Path
        if ($AutoDeleteOnIdle -ge 5)
        {
            $TopicDescription.AutoDeleteOnIdle = [System.TimeSpan]::FromMinutes($AutoDeleteOnIdle)
        }
        if ($DefaultMessageTimeToLive -gt 0)
        {
            $TopicDescription.DefaultMessageTimeToLive = [System.TimeSpan]::FromMinutes($DefaultMessageTimeToLive)
        }
        if ($DuplicateDetectionHistoryTimeWindow -gt 0)
        {
            $TopicDescription.DuplicateDetectionHistoryTimeWindow = [System.TimeSpan]::FromMinutes($DuplicateDetectionHistoryTimeWindow)
        }
        $TopicDescription.EnableBatchedOperations = $EnableBatchedOperations
        $TopicDescription.EnableExpress = $EnableExpress
        $TopicDescription.EnableFilteringMessagesBeforePublishing = $EnableFilteringMessagesBeforePublishing
        $TopicDescription.EnablePartitioning = $EnablePartitioning
        $TopicDescription.IsAnonymousAccessible = $IsAnonymousAccessible
        $TopicDescription.MaxSizeInMegabytes = $MaxSizeInMegabytes
        $TopicDescription.RequiresDuplicateDetection = $RequiresDuplicateDetection
        if ($EnablePartitioning)
        {
            $TopicDescription.SupportOrdering = $False
        }
        else
        {
            $TopicDescription.SupportOrdering = $SupportOrdering
        }
        $TopicDescription.UserMetadata = $UserMetadata
        $NamespaceManager.CreateTopic($TopicDescription);
    }

## <a name="next-steps"></a>Następne kroki

W tym artykule udostępniana podstawowe przepływu pracy dla inicjowania obsługi administracyjnej podmioty Bus usługi przy użyciu programu PowerShell. Mimo że dostępnych ograniczoną liczbę poleceń cmdlet środowiska PowerShell do zarządzania Bus usługi wiadomości jednostki, odwołując się zestawu Microsoft.ServiceBus.dll praktycznie dowolnej operacji można wykonywać przy użyciu bibliotek klienta .NET, które można wykonać w obszarze skrypt programu PowerShell.

Przykłady bardziej szczegółowe są dostępne w tych wpisów w blogu:

- [Jak utworzyć Bus usługi kolejek, tematy i subskrypcji za pomocą skryptu programu PowerShell](http://blogs.msdn.com/b/paolos/archive/2014/12/02/how-to-create-a-service-bus-queues-topics-and-subscriptions-using-a-powershell-script.aspx)
- [Jak utworzyć Namespace Bus usług i Centrum zdarzeń za pomocą skryptu programu PowerShell](http://blogs.msdn.com/b/paolos/archive/2014/12/01/how-to-create-a-service-bus-namespace-and-an-event-hub-using-a-powershell-script.aspx)

Niektóre gotowe skrypty są również dostępne do pobrania:

- [Usługa Bus skryptów programu PowerShell](https://code.msdn.microsoft.com/Service-Bus-PowerShell-a46b7059)

<!--Anchors-->

[Opcje zakupu]: http://azure.microsoft.com/pricing/purchase-options/
[element członkowski ofert]: http://azure.microsoft.com/pricing/member-offers/
[bezpłatne konto]: http://azure.microsoft.com/pricing/free-trial/
[Pakiet NuGet Bus usług]: http://www.nuget.org/packages/WindowsAzure.ServiceBus/
[Get-AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495122.aspx
[Nowy AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495165.aspx
[Get-AzureSBAuthorizationRule]: https://msdn.microsoft.com/library/azure/dn495113.aspx
[Interfejs API programu .NET dla usługi Bus]: https://msdn.microsoft.com/en-us/library/azure/mt419900.aspx
[Instalowanie i konfigurowanie programu PowerShell Azure]: ../powershell-install-configure.md
