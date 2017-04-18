<properties 
    pageTitle="Zarządzanie wyszukiwaniem Azure za pomocą skryptów programu Powershell | Microsoft Azure | Usługa wyszukiwania hostowanej chmury" 
    description="Zarządzanie usługą Azure wyszukiwania za pomocą skryptów programu PowerShell. Tworzenie lub aktualizowanie usługi Azure wyszukiwania i zarządzanie nimi klawiszy Administrator wyszukiwania Azure" 
    services="search" 
    documentationCenter="" 
    authors="seansaleh" 
    manager="mblythe" 
    editor=""
    tags="azure-resource-manager"/>

<tags 
    ms.service="search" 
    ms.devlang="na" 
    ms.workload="search" 
    ms.topic="article" 
    ms.tgt_pltfrm="powershell" 
    ms.date="08/15/2016" 
    ms.author="seasa"/>

# <a name="manage-your-azure-search-service-with-powershell"></a>Zarządzanie usługą Azure wyszukiwania przy użyciu programu PowerShell
> [AZURE.SELECTOR]
- [Portal](search-manage.md)
- [Programu PowerShell](search-manage-powershell.md)
- [INTERFEJSU API USŁUGI REST](search-get-started-management-api.md)

W tym temacie opisano polecenia programu PowerShell do wykonywania wielu zadań związanych z zarządzaniem usługi Azure wyszukiwania. Firma Microsoft będzie szczegółową Tworzenie usługi wyszukiwania, jej skalowania i zarządzania jego kluczy interfejsu API.
Te polecenia równoległa opcji zarządzania dostępnych w [Interfejsu API usługi REST zarządzania wyszukiwania Azure](http://msdn.microsoft.com/library/dn832684.aspx).

## <a name="prerequisites"></a>Wymagania wstępne
 
- Musi być Azure PowerShell wersji 1.0 lub nowszej. Aby uzyskać instrukcje, zobacz [Instalowanie i konfigurowanie programu PowerShell Azure](../powershell-install-configure.md).
- Użytkownik zalogowany do subskrypcji usługi Azure w programie PowerShell w sposób opisany poniżej.

Musisz najpierw zaloguj się do Azure za pomocą tego polecenia:

    Login-AzureRmAccount

Określ adres e-mail konta Azure i jej hasło w oknie dialogowym logowania Microsoft Azure.

Alternatywnie możesz [logowania - interakcyjnie z głównej usługi](../resource-group-authenticate-service-principal.md).

Jeśli masz wiele subskrypcji Azure, należy ustawić Azure subskrypcji. Aby wyświetlić listę bieżącej subskrypcji, uruchom polecenie.

    Get-AzureRmSubscription | sort SubscriptionName | Select SubscriptionName

Aby określić subskrypcji, uruchom następujące polecenie. W poniższym przykładzie jest nazwę subskrypcji `ContosoSubscription`.

    Select-AzureRmSubscription -SubscriptionName ContosoSubscription

## <a name="commands-to-help-you-get-started"></a>Polecenia ułatwiające rozpoczęcie pracy

    $serviceName = "your-service-name-lowercase-with-dashes"
    $sku = "free" # or "basic" or "standard" for paid services
    $location = "West US"
    # You can get a list of potential locations with
    # (Get-AzureRmResourceProvider -ListAvailable | Where-Object {$_.ProviderNamespace -eq 'Microsoft.Search'}).Locations
    $resourceGroupName = "YourResourceGroup" 
    # If you don't already have this resource group, you can create it with 
    # New-AzureRmResourceGroup -Name $resourceGroupName -Location $location

    # Register the ARM provider idempotently. This must be done once per subscription
    Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.Search" -Force

    # Create a new search service
    # This command will return once the service is fully created
    New-AzureRmResourceGroupDeployment `
        -ResourceGroupName $resourceGroupName `
        -TemplateUri "https://gallery.azure.com/artifact/20151001/Microsoft.Search.1.0.9/DeploymentTemplates/searchServiceDefaultTemplate.json" `
        -NameFromTemplate $serviceName `
        -Sku $sku `
        -Location $location `
        -PartitionCount 1 `
        -ReplicaCount 1
    
    # Get information about your new service and store it in $resource
    $resource = Get-AzureRmResource `
        -ResourceType "Microsoft.Search/searchServices" `
        -ResourceGroupName $resourceGroupName `
        -ResourceName $serviceName `
        -ApiVersion 2015-08-19
    
    # View your resource
    $resource
    
    # Get the primary admin API key
    $primaryKey = (Invoke-AzureRmResourceAction `
        -Action listAdminKeys `
        -ResourceId $resource.ResourceId `
        -ApiVersion 2015-08-19).PrimaryKey

    # Regenerate the secondary admin API Key
    $secondaryKey = (Invoke-AzureRmResourceAction `
        -ResourceType "Microsoft.Search/searchServices/regenerateAdminKey" `
        -ResourceGroupName $resourceGroupName `
        -ResourceName $serviceName `
        -ApiVersion 2015-08-19 `
        -Action secondary).SecondaryKey

    # Create a query key for read only access to your indexes
    $queryKeyDescription = "query-key-created-from-powershell"
    $queryKey = (Invoke-AzureRmResourceAction `
        -ResourceType "Microsoft.Search/searchServices/createQueryKey" `
        -ResourceGroupName $resourceGroupName `
        -ResourceName $serviceName `
        -ApiVersion 2015-08-19 `
        -Action $queryKeyDescription).Key
    
    # View your query key
    $queryKey

    # Delete query key
    Remove-AzureRmResource `
        -ResourceType "Microsoft.Search/searchServices/deleteQueryKey/$($queryKey)" `
        -ResourceGroupName $resourceGroupName `
        -ResourceName $serviceName `
        -ApiVersion 2015-08-19
        
    # Scale your service up
    # Note that this will only work if you made a non "free" service
    # This command will not return until the operation is finished
    # It can take 15 minutes or more to provision the additional resources
    $resource.Properties.ReplicaCount = 2
    $resource | Set-AzureRmResource
    
    # Delete your service
    # Deleting your service will delete all indexes and data in the service
    $resource | Remove-AzureRmResource
    
## <a name="next-steps"></a>Następne kroki
    
Teraz, gdy jest tworzony usługi, możesz podjąć kroki następnego: tworzenie [indeksu](search-what-is-an-index.md), [kwerendy indeksu](search-query-overview.md)i na końcu tworzenie i zarządzanie nimi własnych aplikacji wyszukiwania, w której są używane Azure wyszukiwania.

- [Tworzenie indeksu wyszukiwania Azure w portalu Azure](search-create-index-portal.md)

- [Kwerenda indeksu wyszukiwania Azure za pomocą Eksploratora wyszukiwania w portalu Azure](search-explorer.md)

- [Konfiguracja indeksowania, aby załadować dane z innych usług](search-indexer-overview.md)

- [Jak używać wyszukiwania Azure w .NET](search-howto-dotnet-sdk.md)

- [Analizowanie ruchu Azure wyszukiwania](search-traffic-analytics.md)
