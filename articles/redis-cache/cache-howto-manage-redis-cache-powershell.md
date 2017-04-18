<properties
    pageTitle="Zarządzanie Azure Redis pamięci podręcznej Azure programu PowerShell | Microsoft Azure"
    description="Dowiedz się, jak wykonywać zadania administracyjne Azure Redis pamięci podręcznej przy użyciu programu PowerShell Azure."
    services="redis-cache"
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="manage-azure-redis-cache-with-azure-powershell"></a>Zarządzanie Azure Redis pamięci podręcznej Azure programu PowerShell

> [AZURE.SELECTOR]
- [Programu PowerShell](cache-howto-manage-redis-cache-powershell.md)
- [Polecenie Azure](cache-manage-cli.md)

W tym temacie przedstawiono możesz jak wykonywanie typowych zadań do wykonania takich jak tworzenie, aktualizowanie i skalowanie wystąpienia usługi Azure Redis w pamięci podręcznej, jak ponownie wygenerować klawiszy dostępu i sposobu wyświetlania informacji na temat swojej pamięci podręcznej. Aby uzyskać pełną listę poleceń cmdlet środowiska PowerShell pamięci podręcznej Redis Azure zobacz [polecenia cmdlet Azure Redis w pamięci podręcznej](https://msdn.microsoft.com/library/azure/mt634513.aspx).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)][Klasyczny model](../resource-manager-deployment-model.md#classic-deployment-characteristics).

## <a name="prerequisites"></a>Wymagania wstępne

Jeśli masz już zainstalowany Azure programu PowerShell, musisz mieć Azure programu PowerShell wersji 1.0.0 lub nowszej. Możesz sprawdzić wersję programu PowerShell Azure, zainstalowanym przy użyciu tego polecenia w wierszu polecenia programu PowerShell Azure.

    Get-Module azure | format-table version

[AZURE.INCLUDE [powershell-preview](../../includes/powershell-preview-inline-include.md)]

Najpierw musisz zalogować się do Azure za pomocą tego polecenia.

    Login-AzureRmAccount

Określ adres e-mail konta Azure i jej hasło w oknie dialogowym Microsoft Azure logowania.

Następnie Jeśli masz wiele subskrypcji Azure, należy ustawić Azure subskrypcji. Aby wyświetlić listę bieżącej subskrypcji, uruchom polecenie.

    Get-AzureRmSubscription | sort SubscriptionName | Select SubscriptionName

Aby określić subskrypcji, uruchom następujące polecenie. W poniższym przykładzie jest nazwę subskrypcji `ContosoSubscription`.

    Select-AzureRmSubscription -SubscriptionName ContosoSubscription

Zanim będzie można używać programu Windows PowerShell przy użyciu Menedżera zasobów Azure, są potrzebne następujące elementy:

- Środowiska Windows PowerShell w wersji 3.0 lub 4.0. Aby znaleźć wersję programu Windows PowerShell, wpisz:`$PSVersionTable` i sprawdź, czy wartość `PSVersion` 3.0 lub 4.0. Aby zainstalować wersję, zobacz [Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) lub [Windows Management Framework 4.0](http://www.microsoft.com/download/details.aspx?id=40855).

Aby uzyskać szczegółową pomoc dotyczącą dowolnego polecenia cmdlet, które są widoczne w tym samouczku, należy użyć polecenia cmdlet Get-Help.

    Get-Help <cmdlet-name> -Detailed

Na przykład, aby uzyskać pomoc dotyczącą `New-AzureRmRedisCache` polecenia cmdlet, wpisz:

    Get-Help New-AzureRmRedisCache -Detailed

### <a name="how-to-connect-to-azure-government-cloud-or-azure-china-cloud"></a>Jak utworzyć połączenie ze Azure Government Cloud lub w chmurze Chinach Azure

Domyślnie Azure środowisko jest `AzureCloud`, przedstawiający wystąpienie chmury globalnej Azure. Aby połączyć się do innego wystąpienia, użyj `Add-AzureRmAccount` polecenia z `-Environment` lub -`EnvironmentName` przełącznik wiersza polecenia przy użyciu odpowiedniej środowiska lub nazwę środowiska.

Aby wyświetlić listę dostępnych środowisk, uruchom `Get-AzureRmEnvironment` polecenia cmdlet.

### <a name="to-connect-to-the-azure-government-cloud"></a>Aby nawiązać połączenie z chmurą dla instytucji rządowych Azure

Aby nawiązać połączenie z chmurą dla instytucji rządowych Azure, wykonaj jedną z następujących poleceń.

    Add-AzureRMAccount -EnvironmentName AzureUSGovernment

lub

    Add-AzureRmAccount -Environment (Get-AzureRmEnvironment -Name AzureUSGovernment)

Aby utworzyć pamięci podręcznej w chmurze dla instytucji rządowych Azure, użyj jednej z następujących lokalizacji.

-   USGov Virginia
-   USGov Iowa

Aby uzyskać więcej informacji na temat w chmurze dla instytucji rządowych Azure zobacz [Microsoft Azure dla instytucji rządowych](https://azure.microsoft.com/features/gov/) i [Microsoft Azure dla instytucji rządowych Developer Guide](../azure-government-developer-guide.md).

### <a name="to-connect-to-the-azure-china-cloud"></a>Aby nawiązać połączenie z chmurą Chin Azure

Aby nawiązać połączenie z chmurą Chinach Azure, wykonaj jedną z następujących poleceń.

    Add-AzureRMAccount -EnvironmentName AzureChinaCloud

lub

    Add-AzureRmAccount -Environment (Get-AzureRmEnvironment -Name AzureChinaCloud)

Aby utworzyć pamięci podręcznej w chmurze Chinach Azure, wykonaj jedną z następujących lokalizacji.

-   Chiny wschód
-   Ameryka Północna w Chinach

Aby uzyskać więcej informacji na temat w chmurze Chinach Azure zobacz [AzureChinaCloud dla Azure obsługiwana przez firmę 21Vianet w Chinach](http://www.windowsazure.cn/).

### <a name="properties-used-for-azure-redis-cache-powershell"></a>Właściwości używane dla środowiska PowerShell pamięci podręcznej Redis Azure

Poniższa tabela zawiera opisy parametrów często używane podczas tworzenia i zarządzania nimi wystąpienia usługi Azure Redis w pamięci podręcznej, przy użyciu programu PowerShell Azure i właściwości.

| Parametr          | Opis                                                                                                                                                                                                        | Domyślne  |
|--------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| Nazwa               | Nazwy pamięci podręcznej                                                                                                                                                                                                  |          |
| Lokalizacja           | Lokalizacja pamięci podręcznej                                                                                                                                                                                              |          |
| ResourceGroupName  | Nazwa grupy zasobów, w której chcesz utworzyć pamięci podręcznej                                                                                                                                                                   |          |
| Rozmiar               | Rozmiar pamięci podręcznej. Prawidłowe wartości: P1, P2, P3, P4, C0, C1, C2, C3, C4, C5, C6, 250MB, 1GB, 2,5 GB, 6 GB, 13 GB, 26 GB, 53 GB                                                                     | 1GB      |
| ShardCount         | Liczba odłamki, aby utworzyć podczas tworzenia pamięci podręcznej premium z klaster włączone. Prawidłowe wartości: 1, 2, 3, 4, 5, 6, 7, 8, 9, 10                                                                                                      |          |
| JEDNOSTKA SKU                | Określa SKU pamięci podręcznej. Prawidłowe wartości to: podstawowe, standardowy, Premium                                                                                                                                         | Standardowe |
| RedisConfiguration | Określa ustawienia konfiguracji Redis. Aby uzyskać szczegółowe informacje o każdym ustawieniu zobacz w poniższej tabeli [RedisConfiguration właściwości](#redisconfiguration-properties) . |          |
| EnableNonSslPort   | Wskazuje, czy port SSL nie jest włączone.                                                                                                                                                                     | FAŁSZ    |
| MaxMemoryPolicy    | Ten parametr została zastąpiona — zamiast tego użyj RedisConfiguration.                                                                                                                                              |          |
| StaticIP           | Gdy hostingu pamięć podręczną w VNET określa unikatowy adres IP w podsieć pamięci podręcznej. Jeśli nie zostanie podany, jedną wybierany jest dla Ciebie z podsieci.                                                                                                                     |          |
| Podsieć             | Gdy hostingu pamięć podręczną w VNET, Nazwa podsieci, w którym chcesz wdrożyć pamięci podręcznej.                                                                                                                  |          |
| VirtualNetwork     | W przypadku hostingu pamięć podręczną w VNET Określa identyfikator zasobu VNET, w którym chcesz wdrożyć pamięci podręcznej.                                                                                                             |          |
| KeyType            | Określa, które klawisz dostępu do generowania podczas odnawiania klawiszy dostępu. Prawidłowe wartości: podstawową, pomocniczą |  |                                                                                                                                                                                                              |          |


### <a name="redisconfiguration-properties"></a>Właściwości RedisConfiguration

| Właściwość                      | Opis                                                                                                          | Cennik warstwy       |
|-------------------------------|----------------------------------------------------------------------------------------------------------------------|---------------------|
| włączone kopii zapasowej RDB            | Czy jest włączony [Redis utrzymywanie danych](cache-how-to-premium-persistence.md)                                     | Tylko Premium        |
| RDB-miejsca do magazynowania — — parametry połączenia | Parametry połączenia z kontem miejsca do magazynowania dla [Redis utrzymywanie danych](cache-how-to-premium-persistence.md)       | Tylko Premium        |
| RDB kopii zapasowej częstotliwości          | Częstotliwość kopii zapasowej [Redis utrzymywanie danych](cache-how-to-premium-persistence.md)                               | Tylko Premium        |
| zastrzeżone maxmemory            | Konfiguruje [pamięci zarezerwowanych](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) dla procesów innych niż pamięci podręcznej | Standardowe i Premium |
| zasady maxmemory              | Konfiguruje [zasady eksmisji zwiększany](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) pamięci podręcznej           | Wszystkie poziomy cennik   |
| Powiadom keyspace zdarzenia        | Konfiguruje [keyspace powiadomienia](cache-configure.md#keyspace-notifications-advanced-settings)                     | Standardowe i Premium |
| Skrót max-ziplist zapisy      | Konfiguruje [Optymalizacja pamięci](http://redis.io/topics/memory-optimization) typu small agregowanie danych          | Standardowe i Premium |
| mieszania maks.-ziplist        | Konfiguruje [Optymalizacja pamięci](http://redis.io/topics/memory-optimization) typu small agregowanie danych          | Standardowe i Premium |
| Ustawianie max-intset zapisy        | Konfiguruje [Optymalizacja pamięci](http://redis.io/topics/memory-optimization) typu small agregowanie danych          | Standardowe i Premium |
| zset max-ziplist zapisy      | Konfiguruje [Optymalizacja pamięci](http://redis.io/topics/memory-optimization) typu small agregowanie danych          | Standardowe i Premium |
| zset maks.-ziplist-value        | Konfiguruje [Optymalizacja pamięci](http://redis.io/topics/memory-optimization) typu small agregowanie danych          | Standardowe i Premium |
| bazy danych                     | Konfiguruje liczbę baz danych. Tej właściwości można skonfigurować tylko na tworzenie pamięci podręcznej.                          | Standardowe i Premium |

## <a name="to-create-a-redis-cache"></a>Aby utworzyć Redis pamięci podręcznej

Nowe wystąpienia Azure Redis w pamięci podręcznej są tworzone przy użyciu polecenia cmdlet [New-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634517.aspx) .

>[AZURE.IMPORTANT] Tworzenie pamięci podręcznej Redis w subskrypcji za pomocą portalu Azure po raz pierwszy rejestruje portalu `Microsoft.Cache` nazw dla tej subskrypcji. Jeśli spróbujesz utworzyć pierwszą pamięci podręcznej Redis w subskrypcji przy użyciu programu PowerShell, należy najpierw zarejestrować tego obszaru nazw przy użyciu następującego polecenia; inny sposób polecenia cmdlet, takich jak `New-AzureRmRedisCache` i `Get-AzureRmRedisCache` się nie powieść.
>
>`Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.Cache"`

Aby wyświetlić listę parametrów dostępnych i ich opisy `New-AzureRmRedisCache`, uruchom następujące polecenie.

    PS C:\> Get-Help New-AzureRmRedisCache -detailed
    
    NAME
        New-AzureRmRedisCache
    
    SYNOPSIS
        Creates a new redis cache.
    
    
    SYNTAX
        New-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -Location <String> [-RedisVersion <String>]
        [-Size <String>] [-Sku <String>] [-MaxMemoryPolicy <String>] [-RedisConfiguration <Hashtable>] [-EnableNonSslPort
        <Boolean>] [-ShardCount <Integer>] [-VirtualNetwork <String>] [-Subnet <String>] [-StaticIP <String>]
        [<CommonParameters>]
    
    
    DESCRIPTION
        The New-AzureRmRedisCache cmdlet creates a new redis cache.
    
    
    PARAMETERS
        -Name <String>
            Name of the redis cache to create.
    
        -ResourceGroupName <String>
            Name of resource group in which to create the redis cache.
    
        -Location <String>
            Location in which to create the redis cache.
    
        -RedisVersion <String>
            RedisVersion is deprecated and will be removed in future release.
    
        -Size <String>
            Size of the redis cache. The default value is 1GB or C1. Possible values are P1, P2, P3, P4, C0, C1, C2, C3,
            C4, C5, C6, 250MB, 1GB, 2.5GB, 6GB, 13GB, 26GB, 53GB.
    
        -Sku <String>
            Sku of redis cache. The default value is Standard. Possible values are Basic, Standard and Premium.
    
        -MaxMemoryPolicy <String>
            The 'MaxMemoryPolicy' setting has been deprecated. Please use 'RedisConfiguration' setting to set
            MaxMemoryPolicy. e.g. -RedisConfiguration @{"maxmemory-policy" = "allkeys-lru"}
    
        -RedisConfiguration <Hashtable>
            All Redis Configuration Settings. Few possible keys: rdb-backup-enabled, rdb-storage-connection-string,
            rdb-backup-frequency, maxmemory-reserved, maxmemory-policy, notify-keyspace-events, hash-max-ziplist-entries,
            hash-max-ziplist-value, set-max-intset-entries, zset-max-ziplist-entries, zset-max-ziplist-value, databases.
    
        -EnableNonSslPort <Boolean>
            EnableNonSslPort is used by Azure Redis Cache. If no value is provided, the default value is false and the
            non-SSL port will be disabled. Possible values are true and false.
    
        -ShardCount <Integer>
            The number of shards to create on a Premium Cluster Cache.
    
        -VirtualNetwork <String>
            The exact ARM resource ID of the virtual network to deploy the redis cache in. Example format: /subscriptions/{
            subid}/resourceGroups/{resourceGroupName}/providers/Microsoft.ClassicNetwork/VirtualNetworks/{vnetName}
    
        -Subnet <String>
            Required when deploying a redis cache inside an existing Azure Virtual Network.
    
        -StaticIP <String>
            Required when deploying a redis cache inside an existing Azure Virtual Network.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

Aby utworzyć pamięci podręcznej z parametrami domyślnymi, uruchom następujące polecenie.

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US"

`ResourceGroupName`, `Name`, i `Location` są wymagane parametry, ale pozostałe są opcjonalne i mają wartości domyślne. Uruchomione polecenie poprzednie tworzy wystąpienie pamięci podręcznej Redis Azure SKU standardowy z określoną nazwę, lokalizację i grupa zasobów, której rozmiar z portem bez użycia protokołu SSL wyłączone 1 GB.

Aby utworzyć pamięci podręcznej premium, określ wielkość P1 (6 GB – 60 GB), P2 (13 GB - 130 GB), P3 (26 GB - 260 GB), lub P4 (53 GB - 530 GB). Aby włączyć klaster, określ liczba shard za pomocą `ShardCount` parametru. Poniższy przykład tworzy pamięci podręcznej premium P1 z odłamki 3. Pamięć podręczna P1 premium jest 6 GB rozmiar, a od określonej udostępniamy trzy odłamki całkowity rozmiar jest 18 GB (3 x 6 GB).

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -Sku Premium -Size P1 -ShardCount 3

Aby określić wartości dla `RedisConfiguration` parametr, wartości wewnątrz, ujmij `{}` jako klucz i wartość pary, takich jak `@{"maxmemory-policy" = "allkeys-random", "notify-keyspace-events" = "KEA"}`. Poniższy przykład tworzy standardowy 1 GB pamięci podręcznej `allkeys-random` maxmemory zasad keyspace powiadomienia o i skonfigurowano `KEA`. Aby uzyskać więcej informacji, zobacz [powiadomienia Keyspace (ustawienia zaawansowane)](cache-configure.md#keyspace-notifications-advanced-settings) i [Maxmemory zasad i zastrzeżone maxmemory](cache-configure.md#maxmemory-policy-and-maxmemory-reserved).

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -RedisConfiguration @{"maxmemory-policy" = "allkeys-random", "notify-keyspace-events" = "KEA"}

<a name="databases"></a>
## <a name="to-configure-the-databases-setting-during-cache-creation"></a>Aby skonfigurować ustawienie podczas tworzenia pamięci podręcznej bazy danych

`databases` Ustawienie można skonfigurować tylko podczas tworzenia pamięci podręcznej. Poniższy przykład tworzy premium P3 48 bazy danych przy użyciu polecenia cmdlet [New-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634517.aspx) pamięci podręcznej (26 GB).

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -Sku Premium -Size P3 -RedisConfiguration @{"databases" = "48"}

Aby uzyskać więcej informacji na temat `databases` właściwości, zobacz [domyślne pamięci podręcznej Redis Azure konfiguracji serwera](cache-configure.md#default-redis-server-configuration). Aby uzyskać więcej informacji na temat tworzenia pamięci podręcznej za pomocą polecenia cmdlet [New-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634517.aspx) Zobacz poprzedniej sekcji [do utworzenia Redis pamięci podręcznej](#to-create-a-redis-cache) .

## <a name="to-update-a-redis-cache"></a>Aby zaktualizować Redis pamięci podręcznej

Azure wystąpienia Redis pamięci podręcznej zostaną zaktualizowane przy użyciu polecenia cmdlet [Set-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634518.aspx) .

Aby wyświetlić listę parametrów dostępnych i ich opisy `Set-AzureRmRedisCache`, uruchom następujące polecenie.

    PS C:\> Get-Help Set-AzureRmRedisCache -detailed
    
    NAME
        Set-AzureRmRedisCache
    
    SYNOPSIS
        Set redis cache updatable parameters.
    
    SYNTAX
        Set-AzureRmRedisCache -Name <String> -ResourceGroupName <String> [-Size <String>] [-Sku <String>]
        [-MaxMemoryPolicy <String>] [-RedisConfiguration <Hashtable>] [-EnableNonSslPort <Boolean>] [-ShardCount
        <Integer>] [<CommonParameters>]
    
    DESCRIPTION
        The Set-AzureRmRedisCache cmdlet sets redis cache parameters.
    
    PARAMETERS
        -Name <String>
            Name of the redis cache to update.
    
        -ResourceGroupName <String>
            Name of the resource group for the cache.
    
        -Size <String>
            Size of the redis cache. The default value is 1GB or C1. Possible values are P1, P2, P3, P4, C0, C1, C2, C3,
            C4, C5, C6, 250MB, 1GB, 2.5GB, 6GB, 13GB, 26GB, 53GB.
    
        -Sku <String>
            Sku of redis cache. The default value is Standard. Possible values are Basic, Standard and Premium.
    
        -MaxMemoryPolicy <String>
            The 'MaxMemoryPolicy' setting has been deprecated. Please use 'RedisConfiguration' setting to set
            MaxMemoryPolicy. e.g. -RedisConfiguration @{"maxmemory-policy" = "allkeys-lru"}
    
        -RedisConfiguration <Hashtable>
            All Redis Configuration Settings. Few possible keys: rdb-backup-enabled, rdb-storage-connection-string,
            rdb-backup-frequency, maxmemory-reserved, maxmemory-policy, notify-keyspace-events, hash-max-ziplist-entries,
            hash-max-ziplist-value, set-max-intset-entries, zset-max-ziplist-entries, zset-max-ziplist-value.
    
        -EnableNonSslPort <Boolean>
            EnableNonSslPort is used by Azure Redis Cache. The default value is null and no change will be made to the
            currently configured value. Possible values are true and false.
    
        -ShardCount <Integer>
            The number of shards to create on a Premium Cluster Cache.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

`Set-AzureRmRedisCache` Polecenia cmdlet może służyć do aktualizowania właściwości, takie jak `Size`, `Sku`, `EnableNonSslPort`oraz `RedisConfiguration` wartości. 

Następujące polecenie aktualizuje zasady maxmemory pamięci podręcznej Redis o nazwie myCache.

    Set-AzureRmRedisCache -ResourceGroupName "myGroup" -Name "myCache" -RedisConfiguration @{"maxmemory-policy" = "allkeys-random"}

<a name="scale"></a>
## <a name="to-scale-a-redis-cache"></a>Aby przeskalować Redis pamięci podręcznej

`Set-AzureRmRedisCache`Umożliwia skalowanie pamięci podręcznej Azure Redis wystąpienia, kiedy `Size`, `Sku`, lub `ShardCount` właściwości zostaną zmienione. 

>[AZURE.NOTE]Skalowanie pamięci podręcznej przy użyciu programu PowerShell podlega tę samą limity i wskazówki dotyczące jako skalowanie pamięci podręcznej z portalu usługi Azure. Można skalować różnych poziomu cen z następującymi ograniczeniami.
>
>-  To nie można skalować z wyższego poziomu cen niższy poziom cennik.
>    -    Nie można skalować z pamięci podręcznej **Premium** w dół do **standardowego** lub **podstawowej** pamięci podręcznej.
>    -    To nie można skalować z **Standardowy** pamięci podręcznej do **podstawowej** pamięci podręcznej.
>-  Można skalować z **podstawowe** pamięci podręcznej **Standardowy** pamięci podręcznej, ale nie można zmienić rozmiar w tym samym czasie. Jeśli potrzebujesz inny rozmiar, jak kolejnych operacji skalowania osiągnie wymagany rozmiar.
>-  Nie mogę przeskalować z **podstawowe** pamięci podręcznej, bezpośrednio do pamięci podręcznej **Premium** . Musisz skalowanie od **podstawowe** do **standardowego** w jednej operacji skalowania, a następnie ze **standardowego** do **Premium** w kolejnych operacji skalowania.
>-  Nie mogę przeskalować z większym rozmiarze w dół do **C0 (250 MB)** rozmiar.
>
>Aby uzyskać więcej informacji zobacz [jak skala Azure Redis w pamięci podręcznej](cache-how-to-scale.md).

W poniższym przykładzie pokazano sposób Skalowanie pamięci podręcznej o nazwie `myCache` do 2,5 GB pamięci podręcznej. Zauważ, że to polecenie działa dla obu Basic lub standardowy pamięci podręcznej.

    Set-AzureRmRedisCache -ResourceGroupName myGroup -Name myCache -Size 2.5GB

Po wydaniu tego polecenia, zwracany jest stan pamięci podręcznej (podobne do telefonicznej `Get-AzureRmRedisCache`). Należy zauważyć, że `ProvisioningState` jest `Scaling`.

    PS C:\> Set-AzureRmRedisCache -Name myCache -ResourceGroupName myGroup -Size 2.5GB
    
    
    Name               : mycache
    Id                 : /subscriptions/12ad12bd-abdc-2231-a2ed-a2b8b246bbad4/resourceGroups/mygroup/providers/Mi
                         crosoft.Cache/Redis/mycache
    Location           : South Central US
    Type               : Microsoft.Cache/Redis
    HostName           : mycache.redis.cache.windows.net
    Port               : 6379
    ProvisioningState  : Scaling
    SslPort            : 6380
    RedisConfiguration : {[maxmemory-policy, volatile-lru], [maxmemory-reserved, 150], [notify-keyspace-events, KEA],
                         [maxmemory-delta, 150]...}
    EnableNonSslPort   : False
    RedisVersion       : 3.0
    Size               : 1GB
    Sku                : Standard
    ResourceGroupName  : mygroup
    PrimaryKey         : ....
    SecondaryKey       : ....
    VirtualNetwork     :
    Subnet             :
    StaticIP           :
    TenantSettings     : {}
    ShardCount         :

Po zakończeniu operacji skalowania, `ProvisioningState` zmieni się na `Succeeded`. Jeśli chcesz wprowadzić kolejnej operacji skalowania, takie jak zmiana z podstawowe na standardowy, a następnie zmieniając rozmiar, należy poczekać zakończeniu poprzedniej operacji lub komunikat o błędzie podobny do następującego.

    Set-AzureRmRedisCache : Conflict: The resource '...' is not in a stable state, and is currently unable to accept the update request.

## <a name="to-get-information-about-a-redis-cache"></a>Aby uzyskać informacje o Redis pamięci podręcznej

Można pobrać informacji o pamięci podręcznej za pomocą polecenia cmdlet [Get-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634514.aspx) .

Aby wyświetlić listę parametrów dostępnych i ich opisy `Get-AzureRmRedisCache`, uruchom następujące polecenie.

    PS C:\> Get-Help Get-AzureRmRedisCache -detailed
    
    NAME
        Get-AzureRmRedisCache
    
    SYNOPSIS
        Gets details about a single cache or all caches in the specified resource group or all caches in the current
        subscription.
    
    SYNTAX
        Get-AzureRmRedisCache [-Name <String>] [-ResourceGroupName <String>] [<CommonParameters>]
    
    DESCRIPTION
        The Get-AzureRmRedisCache cmdlet gets the details about a cache or caches depending on input parameters. If both
        ResourceGroupName and Name parameters are provided then Get-AzureRmRedisCache will return details about the
        specific cache name provided.
    
        If only ResourceGroupName is provided than it will return details about all caches in the specified resource group.
    
        If no parameters are given than it will return details about all caches the current subscription.
    
    PARAMETERS
        -Name <String>
            The name of the cache. When this parameter is provided along with ResourceGroupName, Get-AzureRmRedisCache
            returns the details for the cache.
    
        -ResourceGroupName <String>
            The name of the resource group that contains the cache or caches. If ResourceGroupName is provided with Name
            then Get-AzureRmRedisCache returns the details of the cache specified by Name. If only the ResourceGroup
            parameter is provided, then details for all caches in the resource group are returned.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

Aby zwrócić informacji na temat wszystkich pamięci podręcznej w bieżącej subskrypcji, uruchom `Get-AzureRmRedisCache` bez parametrów.

    Get-AzureRmRedisCache

Aby zwrócić informacji na temat wszystkich pamięci podręcznej w danej grupy zasobów, uruchom `Get-AzureRmRedisCache` z `ResourceGroupName` parametru.

    Get-AzureRmRedisCache -ResourceGroupName myGroup

Aby zwrócić informacji na temat określonej pamięci podręcznej, uruchom `Get-AzureRmRedisCache` z `Name` parametr zawierający nazwy pamięci podręcznej oraz `ResourceGroupName` parametru z grupy zasobów zawierające tej pamięci podręcznej.

    PS C:\> Get-AzureRmRedisCache -Name myCache -ResourceGroupName myGroup
    
    Name               : mycache
    Id                 : /subscriptions/12ad12bd-abdc-2231-a2ed-a2b8b246bbad4/resourceGroups/myGroup/providers/Mi
                         crosoft.Cache/Redis/mycache
    Location           : South Central US
    Type               : Microsoft.Cache/Redis
    HostName           : mycache.redis.cache.windows.net
    Port               : 6379
    ProvisioningState  : Succeeded
    SslPort            : 6380
    RedisConfiguration : {[maxmemory-policy, volatile-lru], [maxmemory-reserved, 62], [notify-keyspace-events, KEA],
                         [maxclients, 1000]...}
    EnableNonSslPort   : False
    RedisVersion       : 3.0
    Size               : 1GB
    Sku                : Standard
    ResourceGroupName  : myGroup
    VirtualNetwork     :
    Subnet             :
    StaticIP           :
    TenantSettings     : {}
    ShardCount         :

## <a name="to-retrieve-the-access-keys-for-a-redis-cache"></a>Aby pobrać klawisze dostępu dla Redis pamięci podręcznej

Aby pobrać klawisze dostępu dla pamięci podręcznej, można użyć polecenia cmdlet [Get-AzureRmRedisCacheKey](https://msdn.microsoft.com/library/azure/mt634516.aspx) .

Aby wyświetlić listę parametrów dostępnych i ich opisy `Get-AzureRmRedisCacheKey`, uruchom następujące polecenie.

    PS C:\> Get-Help Get-AzureRmRedisCacheKey -detailed
    
    NAME
        Get-AzureRmRedisCacheKey
    
    SYNOPSIS
        Gets the accesskeys for the specified redis cache.
    
    
    SYNTAX
        Get-AzureRmRedisCacheKey -Name <String> -ResourceGroupName <String> [<CommonParameters>]
    
    DESCRIPTION
        The Get-AzureRmRedisCacheKey cmdlet gets the access keys for the specified cache.
    
    PARAMETERS
        -Name <String>
            Name of the redis cache.
    
        -ResourceGroupName <String>
            Name of the resource group for the cache.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

Aby pobrać klucza pamięć podręczną, zadzwoń do `Get-AzureRmRedisCacheKey` polecenia cmdlet i przekazać nazwy pamięci podręcznej nazwę grupy zasobów, która zawiera pamięci podręcznej.

    PS C:\> Get-AzureRmRedisCacheKey -Name myCache -ResourceGroupName myGroup
    
    PrimaryKey   : b2wdt43sfetlju4hfbryfnregrd9wgIcc6IA3zAO1lY=
    SecondaryKey : ABhfB757JgjIgt785JgKH9865eifmekfnn649303JKL=

## <a name="to-regenerate-access-keys-for-your-redis-cache"></a>Aby ponownie wygenerować klawisze dostępu dla pamięci podręcznej Redis

Aby ponownie wygenerować klawisze dostępu dla pamięci podręcznej, możesz użyć polecenia cmdlet [AzureRmRedisCacheKey nowy](https://msdn.microsoft.com/library/azure/mt634512.aspx) .

Aby wyświetlić listę parametrów dostępnych i ich opisy `New-AzureRmRedisCacheKey`, uruchom następujące polecenie.

    PS C:\> Get-Help New-AzureRmRedisCacheKey -detailed
    
    NAME
        New-AzureRmRedisCacheKey
    
    SYNOPSIS
        Regenerates the access key of a redis cache.
    
    SYNTAX
        New-AzureRmRedisCacheKey -Name <String> -ResourceGroupName <String> -KeyType <String> [-Force] [<CommonParameters>]
    
    DESCRIPTION
        The New-AzureRmRedisCacheKey cmdlet regenerate the access key of a redis cache.
    
    PARAMETERS
        -Name <String>
            Name of the redis cache.
    
        -ResourceGroupName <String>
            Name of the resource group for the cache.
    
        -KeyType <String>
            Specifies whether to regenerate the primary or secondary access key. Possible values are Primary or Secondary.
    
        -Force
            When the Force parameter is provided, the specified access key is regenerated without any confirmation prompts.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).
    
Aby ponownie wygenerować klucz podstawowy i pomocniczy dla pamięci podręcznej, połączenie `New-AzureRmRedisCacheKey` polecenia cmdlet i przekazać w nazwie, grupa zasobów i określ jedną `Primary` lub `Secondary` dla `KeyType` parametru. W poniższym przykładzie jest generowane klawisz dostępu pomocniczą w pamięci podręcznej.

    PS C:\> New-AzureRmRedisCacheKey -Name myCache -ResourceGroupName myGroup -KeyType Secondary
    
    Confirm
    Are you sure you want to regenerate Secondary key for redis cache 'myCache'?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y
    
    
    PrimaryKey   : b2wdt43sfetlju4hfbryfnregrd9wgIcc6IA3zAO1lY=
    SecondaryKey : c53hj3kh4jhHjPJk8l0jji785JgKH9865eifmekfnn6=

## <a name="to-delete-a-redis-cache"></a>Aby usunąć Redis pamięci podręcznej

Aby usunąć Redis pamięci podręcznej, należy użyć polecenia cmdlet [AzureRmRedisCache Usuń](https://msdn.microsoft.com/library/azure/mt634515.aspx) .

Aby wyświetlić listę parametrów dostępnych i ich opisy `Remove-AzureRmRedisCache`, uruchom następujące polecenie.

    PS C:\> Get-Help Remove-AzureRmRedisCache -detailed
    
    NAME
        Remove-AzureRmRedisCache
    
    SYNOPSIS
        Remove redis cache if exists.
    
    SYNTAX
        Remove-AzureRmRedisCache -Name <String> -ResourceGroupName <String> [-Force] [-PassThru] [<CommonParameters>
    
    DESCRIPTION
        The Remove-AzureRmRedisCache cmdlet removes a redis cache if it exists.
    
    PARAMETERS
        -Name <String>
            Name of the redis cache to remove.
    
        -ResourceGroupName <String>
            Name of the resource group of the cache to remove.
    
        -Force
            When the Force parameter is provided, the cache is removed without any confirmation prompts.
    
        -PassThru
            By default Remove-AzureRmRedisCache removes the cache and does not return any value. If the PassThru par
            is provided then Remove-AzureRmRedisCache returns a boolean value indicating the success of the operatio
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

W poniższym przykładzie o nazwie pamięci podręcznej `myCache` zostanie usunięty.

    PS C:\> Remove-AzureRmRedisCache -Name myCache -ResourceGroupName myGroup
    
    Confirm
    Are you sure you want to remove redis cache 'myCache'?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y


## <a name="to-import-a-redis-cache"></a>Aby zaimportować Redis pamięci podręcznej

Dane można zaimportować do wystąpienia Azure Redis w pamięci podręcznej za pomocą `Import-AzureRmRedisCache` polecenia cmdlet.

>[AZURE.IMPORTANT] Import/Eksport jest dostępna tylko dla [poziomu premium](cache-premium-tier-intro.md) pamięci podręcznej. Aby uzyskać więcej informacji na temat importowania i eksportowania zobacz [Importowanie i eksportowanie danych w pamięci podręcznej Azure Redis](cache-how-to-import-export-data.md).

Aby wyświetlić listę parametrów dostępnych i ich opisy `Import-AzureRmRedisCache`, uruchom następujące polecenie.

    PS C:\> Get-Help Import-AzureRmRedisCache -detailed
    
    NAME
        Import-AzureRmRedisCache
    
    SYNOPSIS
        Import data from blobs to Azure Redis Cache.
    
    
    SYNTAX
        Import-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -Files <String[]> [-Format <String>] [-Force]
        [-PassThru] [<CommonParameters>]
    
    
    DESCRIPTION
        The Import-AzureRmRedisCache cmdlet imports data from the specified blobs into Azure Redis Cache.
    
    
    PARAMETERS
        -Name <String>
            The name of the cache.
    
        -ResourceGroupName <String>
            The name of the resource group that contains the cache.
    
        -Files <String[]>
            SAS urls of blobs whose content should be imported into the cache.
    
        -Format <String>
            Format for the blob.  Currently "rdb" is the only supported, with other formats expected in the future.
    
        -Force
            When the Force parameter is provided, import will be performed without any confirmation prompts.
    
        -PassThru
            By default Import-AzureRmRedisCache imports data in cache and does not return any value. If the PassThru
            parameter is provided then Import-AzureRmRedisCache returns a boolean value indicating the success of the
            operation.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).
    

Następujące polecenie importuje dane z blob określonego przez identyfikator uri skojarzenia zabezpieczeń do Azure Redis w pamięci podręcznej.


    PS C:\>Import-AzureRmRedisCache -ResourceGroupName "resourceGroupName" -Name "cacheName" -Files @("https://mystorageaccount.blob.core.windows.net/mycontainername/blobname?sv=2015-04-05&sr=b&sig=caIwutG2uDa0NZ8mjdNJdgOY8%2F8mhwRuGNdICU%2B0pI4%3D&st=2016-05-27T00%3A00%3A00Z&se=2016-05-28T00%3A00%3A00Z&sp=rwd") -Force

## <a name="to-export-a-redis-cache"></a>Aby wyeksportować Redis pamięci podręcznej

Można eksportować dane z wystąpienia Azure Redis w pamięci podręcznej za pomocą `Export-AzureRmRedisCache` polecenia cmdlet.

>[AZURE.IMPORTANT] Import/Eksport jest dostępna tylko dla [poziomu premium](cache-premium-tier-intro.md) pamięci podręcznej. Aby uzyskać więcej informacji na temat importowania i eksportowania zobacz [Importowanie i eksportowanie danych w pamięci podręcznej Azure Redis](cache-how-to-import-export-data.md).

Aby wyświetlić listę parametrów dostępnych i ich opisy `Export-AzureRmRedisCache`, uruchom następujące polecenie.

    PS C:\> Get-Help Export-AzureRmRedisCache -detailed
    
    NAME
        Export-AzureRmRedisCache
    
    SYNOPSIS
        Exports data from Azure Redis Cache to a specified container.
    
    
    SYNTAX
        Export-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -Prefix <String> -Container <String> [-Format
        <String>] [-PassThru] [<CommonParameters>]
    
    
    DESCRIPTION
        The Export-AzureRmRedisCache cmdlet exports data from Azure Redis Cache to a specified container.
    
    
    PARAMETERS
        -Name <String>
            The name of the cache.
    
        -ResourceGroupName <String>
            The name of the resource group that contains the cache.
    
        -Prefix <String>
            Prefix to use for blob names.
    
        -Container <String>
            SAS url of container where data should be exported.
    
        -Format <String>
            Format for the blob.  Currently "rdb" is the only supported, with other formats expected in the future.
    
        -PassThru
            By default Export-AzureRmRedisCache does not return any value. If the PassThru parameter is provided
            then Export-AzureRmRedisCache returns a boolean value indicating the success of the operation.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).


Następujące polecenie eksportuje dane z wystąpienia Azure Redis w pamięci podręcznej w kontenerze określonego przez identyfikator uri skojarzeń zabezpieczeń.


        PS C:\>Export-AzureRmRedisCache -ResourceGroupName "resourceGroupName" -Name "cacheName" -Prefix "blobprefix"
        -Container "https://mystorageaccount.blob.core.windows.net/mycontainer?sv=2015-04-05&sr=c&sig=HezZtBZ3DURmEGDduauE7
        pvETY4kqlPI8JCNa8ATmaw%3D&st=2016-05-27T00%3A00%3A00Z&se=2016-05-28T00%3A00%3A00Z&sp=rwdl"

## <a name="to-reboot-a-redis-cache"></a>Aby ponownie uruchomić Redis pamięci podręcznej

Uruchom ponownie usługi Azure Redis w pamięci podręcznej wystąpienia przy użyciu `Reset-AzureRmRedisCache` polecenia cmdlet.

>[AZURE.IMPORTANT] Uruchom ponownie komputer jest dostępna tylko dla [poziomu premium](cache-premium-tier-intro.md) pamięci podręcznej. Aby uzyskać więcej informacji o ponowne uruchomienie pamięć podręczną zobacz [Administrowanie pamięci podręcznej — Uruchom ponownie](cache-administration.md#reboot).

Aby wyświetlić listę parametrów dostępnych i ich opisy `Reset-AzureRmRedisCache`, uruchom następujące polecenie.

    PS C:\> Get-Help Reset-AzureRmRedisCache -detailed
    
    NAME
        Reset-AzureRmRedisCache
    
    SYNOPSIS
        Reboot specified node(s) of an Azure Redis Cache instance.
    
    
    SYNTAX
        Reset-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -RebootType <String> [-ShardId <Integer>]
        [-Force] [-PassThru] [<CommonParameters>]
    
    
    DESCRIPTION
        The Reset-AzureRmRedisCache cmdlet reboots the specified node(s) of an Azure Redis Cache instance.
    
    
    PARAMETERS
        -Name <String>
            The name of the cache.
    
        -ResourceGroupName <String>
            The name of the resource group that contains the cache.
    
        -RebootType <String>
            Which node to reboot. Possible values are "PrimaryNode", "SecondaryNode", "AllNodes".
    
        -ShardId <Integer>
            Which shard to reboot when rebooting a premium cache with clustering enabled.
    
        -Force
            When the Force parameter is provided, reset will be performed without any confirmation prompts.
    
        -PassThru
            By default Reset-AzureRmRedisCache does not return any value. If the PassThru parameter is provided
            then Reset-AzureRmRedisCache returns a boolean value indicating the success of the operation.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).
    

Następujące polecenie ponowne uruchomienie oba węzły określonej pamięci podręcznej.

    
        PS C:\>Reset-AzureRmRedisCache -ResourceGroupName "resourceGroupName" -Name "cacheName" -RebootType "AllNodes"
        -Force
    

## <a name="next-steps"></a>Następne kroki

Aby dowiedzieć się więcej o korzystaniu z platformy Azure programu Windows PowerShell, zobacz następujące zasoby:

- [Azure dokumentacji polecenia cmdlet Redis pamięci podręcznej w witrynie MSDN](https://msdn.microsoft.com/library/azure/mt634513.aspx)
- [Polecenia cmdlet Menedżera zasobów Azure](http://go.microsoft.com/fwlink/?LinkID=394765): Dowiedz się, jak użyć poleceń cmdlet w AzureResourceManager module.
- [Grupy przy użyciu zasobów do zarządzania zasobami Azure](../resource-group-template-deploy-portal.md): Dowiedz się, jak tworzyć i zarządzać nimi grup zasobów w portalu Azure.
- [Azure blogu](http://blogs.msdn.com/windowsazure): Dowiedz się więcej o nowych funkcjach w Azure.
- [Blog dotyczący programu Windows PowerShell](http://blogs.msdn.com/powershell): Dowiedz się więcej o nowych funkcjach w programie Windows PowerShell.
- ["Hej, skryptów specjalisty z zakresu!" Blog](http://blogs.technet.com/b/heyscriptingguy/): Poszukaj rzeczywisty porady i wskazówki w społeczności programu Windows PowerShell.
