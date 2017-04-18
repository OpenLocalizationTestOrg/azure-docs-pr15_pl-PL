<properties
    pageTitle="Zarządzanie Bus usługi przy użyciu programu PowerShell | Microsoft Azure"
    description="Zarządzanie Bus usługi za pomocą skryptów programu PowerShell"
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="sethm"/>

# <a name="manage-service-bus-with-powershell"></a>Zarządzanie Bus usługi przy użyciu programu PowerShell

## <a name="overview"></a>Omówienie

Microsoft Azure PowerShell to środowisko skryptów, używanego do kontrolowania i automatycznego wdrożenia i zarządzania z obciążeń pracą w Azure. Ten artykuł zawiera opis sposobu obsługi administracyjnej i zarządzanie nimi podmioty Bus usługi, takie jak przestrzenie nazw, kolejki i koncentratory wydarzenia przy użyciu lokalnej konsoli programu PowerShell Azure za pomocą programu PowerShell.

## <a name="prerequisites"></a>Wymagania wstępne

Przed rozpoczęciem tego artykułu, musi mieć następujące wymagania:

- Subskrypcję usługi Azure. Azure jest platformą subskrybowanej. Aby uzyskać więcej informacji na temat uzyskiwania subskrypcji zobacz [Opcje zakupu][], [Oferuje członka][]lub [Bezpłatnej wersji próbnej][].

- Komputer z programem PowerShell Azure. Aby uzyskać instrukcje, zobacz [Instalowanie i konfigurowanie programu PowerShell Azure][].

- Ogólne zrozumienie skryptów programu PowerShell, NuGet pakietów i .NET Framework.

## <a name="including-a-reference-to-the-net-assembly-for-service-bus"></a>W tym odwołanie do zestawu .NET dla usługi Bus

Istnieje ograniczoną liczbę poleceń cmdlet środowiska PowerShell do zarządzania Bus usługi. Za pomocą klienta usługi .NET dla usługi Bus [pakiet NuGet Bus usługi][], zainicjować obiektów, które nie są dostępne za pośrednictwem istniejących poleceń cmdlet.

Najpierw upewnij się, że skrypt można znaleźć zestawu **Microsoft.ServiceBus.dll** , która jest instalowana z pakietem NuGet. W celu elastyczne, skrypt wykonuje następujące czynności:

1. Określa ścieżkę, w którym został wywołany.
2. Przechodzi ścieżkę, aż do znalezienia folder o nazwie `packages`. Podczas instalowania pakietów NuGet jest tworzony ten folder.
3. Wyszukiwanie lokalizacji `packages` folder dla zestawu o nazwie **Microsoft.ServiceBus.dll**.
4. Odwołania zestawu, dzięki czemu typy są dostępne w celu późniejszego użycia.

Oto implementacji poniższe czynności w obszarze skrypt programu PowerShell:

```
try
{
    # WARNING: Make sure to reference the latest version of Microsoft.ServiceBus.dll
    Write-Output "Adding the [Microsoft.ServiceBus.dll] assembly to the script..."
    $scriptPath = Split-Path (Get-Variable MyInvocation -Scope 0).Value.MyCommand.Path
    $packagesFolder = (Split-Path $scriptPath -Parent) + "\packages"
    $assembly = Get-ChildItem $packagesFolder -Include "Microsoft.ServiceBus.dll" -Recurse
    Add-Type -Path $assembly.FullName

    Write-Output "The [Microsoft.ServiceBus.dll] assembly has been successfully added to the script."
}

catch [System.Exception]
{
    Write-Error "Could not add the Microsoft.ServiceBus.dll assembly to the script. Make sure you build the solution before running the provisioning script."
}
```

## <a name="provision-a-service-bus-namespace"></a>Inicjowanie obsługi usługi Bus przestrzeń nazw

Dwa polecenia cmdlet programu PowerShell obsługuje Bus usługi nazw operacji. Zamiast interfejsów API SDK .NET można użyć [Get-AzureSBNamespace][] i [AzureSBNamespace nowy][].

W tym przykładzie tworzy kilka zmienne lokalne w skrypt; `$Namespace` and `$Location`.

- `$Namespace`to nazwa nazw Bus usługi, z którą chcemy pracy.
- `$Location`Służy do identyfikowania centrum danych, w którym skrypt przepisy obszaru nazw.
- `$CurrentNamespace`przechowuje nazw dla odwołania skrypt pobiera (lub tworzy).

W polu Rzeczywisty skrypt `$Namespace` i `$Location` mogą być przekazywane jako parametry.

Ta część skrypt wykonuje następujące zadania:

1. Pozwala podjąć próbę pobrania przestrzeń nazw Bus usługi o podanej nazwie.
2. Przestrzeń nazw zostanie znaleziony, raportów, co można odnaleźć.
3. Jeśli nie ma robocze, tworzy obszaru nazw, a następnie pobierze nowo utworzonego nazw.

    ```
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
        New-AzureSBNamespace -Name $Namespace -Location $Location -CreateACSNamespace $false -NamespaceType Messaging
        $CurrentNamespace = Get-AzureSBNamespace -Name $Namespace
        Write-Host "The [$Namespace] namespace in the [$Location] region has been successfully created."
    }
    ```

Do zapewniania obsługi innych podmiotów Bus usługi, należy utworzyć wystąpienie klasy [NamespaceManager][] z zestawu SDK.
Za pomocą polecenia cmdlet [Get-AzureSBAuthorizationRule][] do pobierania reguła autoryzacji, która jest używana do dostarczania parametry połączenia. Będą przechowywane odwołanie do `NamespaceManager` wystąpienia w `$NamespaceManager` zmiennej. Użyjemy `$NamespaceManager` później w obszarze skrypt do zapewniania obsługi innych podmiotów.

``` powershell
$sbr = Get-AzureSBAuthorizationRule -Namespace $Namespace
# Create the NamespaceManager object to create the event hub
Write-Output "Creating a NamespaceManager object for the [$Namespace] namespace..."
$NamespaceManager = [Microsoft.ServiceBus.NamespaceManager]::CreateFromConnectionString($sbr.ConnectionString);
Write-Output "NamespaceManager object for the [$Namespace] namespace has been successfully created."
```

## <a name="provisioning-other-service-bus-entities"></a>Obsługa administracyjna osoby Bus usługi

Dysponująca innymi obiektami, takich jak kolejki, tematy i koncentratory zdarzeń za pomocą [Interfejsu API usługi Bus .NET][]. W tym artykule omówiono tylko koncentratory zdarzenia, ale są podobne kroki dla innych obiektów. Ponadto bardziej szczegółowych przykładów, między innymi obiektami, odwołuje się na końcu tego artykułu.

Ta część skrypt tworzy cztery więcej zmienne lokalne. Są one używane wystąpienia `EventHubDescription` obiektu. Skrypt wykonuje następujące zadania:

1. Za pomocą `NamespaceManager` obiekt, sprawdź, jeśli Centrum zdarzeń oznaczonej numerem `$Path` istnieje.
2. Jeśli nie istnieje, Utwórz `EventHubDescription` i przekazać do `NamespaceManager` klasy `CreateEventHubIfNotExists` metody.
3. Po określeniu, że Centrum zdarzenie jest dostępna, Utwórz grupę dla klientów indywidualnych przy użyciu `ConsumerGroupDescription` i `NamespaceManager`.

    ```
    $Path  = "MyEventHub"
    $PartitionCount = 12
    $MessageRetentionInDays = 7
    $UserMetadata = $null
    $ConsumerGroupName = "MyConsumerGroup"
        
    # Check to see if the Event Hub already exists
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

## <a name="migrate-a-namespace-to-another-azure-subscription"></a>Migrowanie obszaru nazw do innej subskrypcji Azure

Następującej procedury przenosi przestrzeń nazw z jedną subskrypcję Azure do innej. Do wykonania tej operacji, nazw już musi być aktywna, a użytkownik uruchamiający polecenia programu PowerShell, musisz być administratorem dla subskrypcji źródłowej i docelowej.

```
# Create a new resource group in target subscription
Select-AzureRmSubscription -SubscriptionId 'ffffffff-ffff-ffff-ffff-ffffffffffff'
New-AzureRmResourceGroup -Name 'targetRG' -Location 'East US'

# Move namespace from source subscription to target subscription
Select-AzureRmSubscription -SubscriptionId 'aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa'
$res = Find-AzureRmResource -ResourceNameContains mynamespace -ResourceType 'Microsoft.ServiceBus/namespaces'
Move-AzureRmResource -DestinationResourceGroupName 'targetRG' -DestinationSubscriptionId 'ffffffff-ffff-ffff-ffff-ffffffffffff' -ResourceId $res.ResourceId
```

## <a name="next-steps"></a>Następne kroki

W tym artykule przewidziane konspektu podstawowego inicjowania obsługi administracyjnej podmioty Bus usługi przy użyciu programu PowerShell. Wszystko możesz zrobić przy użyciu bibliotek klienta .NET, można także wykonywać w skrypt programu PowerShell.

Dostępne są bardziej szczegółowe przykłady te wpisy w blogu:

- [Jak utworzyć Bus usługi kolejek, tematy i subskrypcji za pomocą skryptu programu PowerShell](http://blogs.msdn.com/b/paolos/archive/2014/12/02/how-to-create-a-service-bus-queues-topics-and-subscriptions-using-a-powershell-script.aspx)
- [Jak utworzyć Namespace Bus usługi i Centrum zdarzeń za pomocą skryptu programu PowerShell](http://blogs.msdn.com/b/paolos/archive/2014/12/01/how-to-create-a-service-bus-namespace-and-an-event-hub-using-a-powershell-script.aspx)

Niektóre gotowe skrypty są również dostępne do pobrania:
- [Usługa Bus skryptów programu PowerShell](https://code.msdn.microsoft.com/Service-Bus-PowerShell-a46b7059)

<!--Link references-->
[Opcje zakupu]: http://azure.microsoft.com/pricing/purchase-options/
[Element członkowski ofert]: http://azure.microsoft.com/pricing/member-offers/
[Bezpłatna wersja próbna]: http://azure.microsoft.com/pricing/free-trial/
[Instalowanie i konfigurowanie programu PowerShell Azure]: ../powershell-install-configure.md
[Pakiet NuGet Bus usługi]: http://www.nuget.org/packages/WindowsAzure.ServiceBus/
[Get-AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495122.aspx
[Nowy AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495165.aspx
[Get-AzureSBAuthorizationRule]: https://msdn.microsoft.com/library/azure/dn495113.aspx
[Interfejs API programu .NET dla usługi Bus]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.aspx
[NamespaceManager]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx
