<properties 
    pageTitle="Jak tworzyć i zarządzać nimi Azure Redis pamięci podręcznej za pomocą interfejsu wiersza polecenia Azure (polecenie Azure) | Microsoft Azure" 
    description="Dowiedz się, jak zainstalować polecenie Azure na dowolnej platformie, jak z niego korzystać do nawiązywania połączenia z kontem Azure i jak tworzyć i zarządzać nimi pamięci podręcznej Redis z polecenie Azure." 
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
    ms.date="09/15/2016" 
    ms.author="sdanie"/>

# <a name="how-to-create-and-manage-azure-redis-cache-using-the-azure-command-line-interface-azure-cli"></a>Jak tworzyć i zarządzać nimi Azure Redis pamięci podręcznej za pomocą interfejsu wiersza polecenia Azure (polecenie Azure)

> [AZURE.SELECTOR]
- [Programu PowerShell](cache-howto-manage-redis-cache-powershell.md)
- [Polecenie Azure](cache-manage-cli.md)

Polecenie Azure to doskonały sposób na zarządzanie infrastrukturą Azure z dowolnej platformie. W tym artykule pokazano, jak tworzyć i zarządzać nimi z wystąpień Azure Redis w pamięci podręcznej, przy użyciu interfejsu wiersza polecenia Azure.

## <a name="prerequisites"></a>Wymagania wstępne

Aby utworzyć i zarządzać nimi przy użyciu interfejsu wiersza polecenia Azure wystąpienia Azure Redis w pamięci podręcznej, należy wykonać następujące czynności.

-   Musisz mieć konto Azure. Jeśli nie masz, możesz utworzyć [bezpłatne konto](https://azure.microsoft.com/pricing/free-trial/) za kilka minut.
-   [Instalowanie Azure interfejsu wiersza polecenia](../xplat-cli-install.md).
-   Łączenie instalacji usługi Azure interfejsu wiersza polecenia przy użyciu osobistego konta Azure lub z pracą lub szkolne konto Azure i logowania z za pomocą interfejsu wiersza polecenia Azure `azure login` polecenia. Aby poznać różnice i wybrać, zobacz [Łączenie z subskrypcji usługi Azure z interfejsu wiersza polecenia Azure (polecenie Azure)](../xplat-cli-connect.md).
-   Przed uruchomieniem dowolne z następujących poleceń, przełączanie polecenie Azure w trybie Menedżera zasobów, uruchamiając `azure config mode arm` polecenia. Aby uzyskać więcej informacji zobacz [Ustawianie trybu Azure Menedżera zasobów](../xplat-cli-azure-resource-manager.md#set-the-azure-resource-manager-mode).

## <a name="redis-cache-properties"></a>Redis właściwości pamięci podręcznej

Poniższe właściwości są używane podczas tworzenia i aktualizowania wystąpienia Redis pamięci podręcznej.

| Właściwość            | Przełączanie                                  | Opis                                                                                                                                                                                                                                          |
|---------------------|-----------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Nazwa                | -n — nazwę,                              | Nazwa Redis pamięci podręcznej.                                                                                                                                                                                                                             |
| Grupa zasobów      | -g, — grupa zasobów                    | Nazwa grupy zasobów.                                                                                                                                                                                                                          |
| Lokalizacja            | -l, — lokalizacja                          | Lokalizacja do tworzenia pamięci podręcznej.                                                                                                                                                                                                                            |
| rozmiar                | -z, — rozmiar                              | Rozmiar pamięci podręcznej Redis. Prawidłowe wartości: [C0 C1, C2, C3, C4, C5, C6, P1, P2, P3, P4]                                                                                                                                                                  |
| Jednostka SKU                 | -x, — sku                               | Redis SKU. Powinien być jednym z: [podstawowe, standardowy, Premium]                                                                                                                                                                                             |
| EnableNonSslPort    | -e, — Włącz non--port ssl               | Właściwość EnableNonSslPort Redis pamięci podręcznej. Dodawanie tej flagi, jeśli chcesz włączyć Port SSL nie dla pamięci podręcznej                                                                                                                                    |
| Redis konfiguracji | -c, — konfiguracja redis               | Redis konfiguracji. Wprowadź ciąg JSON sformatowane kluczy konfiguracji i wartości w tym miejscu. Format: "{" ":""," ":" "}"                                                                                                                                     |
| Redis konfiguracji | -f, — redis konfiguracji pliku          | Redis konfiguracji. Wpisz ścieżkę pliku zawierającego klucze konfiguracji i wartości w tym miejscu. Format wpisu pliku: {"": "","": ""}                                                                                                                |
| Liczba shard         | -r, — liczba shard                       | Liczba odłamki o utworzenie w pamięci podręcznej klaster Premium z klaster.                                                                                                                                                                               |
| Wirtualna sieć     | -v, — wirtualnej sieci                   | Gdy hostingu pamięć podręczną w VNET określa dokładnie ARM zasobów identyfikator wirtualnej sieci do wdrożenia pamięci podręcznej redis w. Przykład format: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1 |
| Typ klucza            | -t, — typ klucza                          | Wpisz klucz w celu odnowienia. Prawidłowe wartości: [podstawowej, pomocniczej]                                                                                                                                                                                             |
| StaticIP            | -p, — ip statyczne < statyczne ip >             | Gdy hostingu pamięć podręczną w VNET określa unikatowy adres IP w podsieć pamięci podręcznej. Jeśli nie zostanie podany, jedną wybierany jest dla Ciebie z podsieci.                                                                                                |
| Podsieć              | t, — podsieci<subnet>                    | Gdy hostingu pamięć podręczną w VNET, Nazwa podsieci, w którym chcesz wdrożyć pamięci podręcznej.                                                                                                                                                    |
| VirtualNetwork      | -v, — wirtualnej sieci < wirtualnej sieci > | Gdy hostingu pamięć podręczną w VNET określa dokładnie ARM zasobów identyfikator wirtualnej sieci do wdrożenia pamięci podręcznej redis w. Przykład format: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1 |
| Subskrypcji        | -s, — subskrypcji                      | Identyfikator subskrypcji.                                                                                                                                                                                                                         |

## <a name="see-all-redis-cache-commands"></a>Zobacz wszystkie polecenia Redis pamięci podręcznej

Aby wyświetlić wszystkie polecenia Redis pamięci podręcznej i ich parametry, użyj `azure rediscache -h` polecenia.

    C:\>azure rediscache -h
    help:    Commands to manage your Azure Redis Cache(s)
    help:
    help:    Create a Redis Cache
    help:      rediscache create [--name <name> --resource-group <resource-group> --location <location> [options]]
    help:
    help:    Delete an existing Redis Cache
    help:      rediscache delete [--name <name> --resource-group <resource-group> ]
    help:
    help:    List all Redis Caches within your Subscription or Resource Group
    help:      rediscache list [options]
    help:
    help:    Show properties of an existing Redis Cache
    help:      rediscache show [--name <name> --resource-group <resource-group>]
    help:
    help:    Change settings of an existing Redis Cache
    help:      rediscache set [--name <name> --resource-group <resource-group> --redis-configuration <redis-configuration>/--redis-configuration-file <redisConfigurationFile>]
    help:
    help:    Renew the authentication key for an existing Redis Cache
    help:      rediscache renew-key [--name <name> --resource-group <resource-group> ]
    help:
    help:    Lists Primary and Secondary key of an existing Redis Cache
    help:      rediscache list-keys [--name <name> --resource-group <resource-group>]
    help:
    help:    Options:
    help:      -h, --help  output usage information
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="create-a-redis-cache"></a>Tworzenie Redis pamięci podręcznej

Aby utworzyć Redis pamięci podręcznej, użyj następującego polecenia:

    azure rediscache create [--name <name> --resource-group <resource-group> --location <location> [options]]

Aby uzyskać więcej informacji na temat tego polecenia Uruchom `azure rediscache create -h` polecenia.

    C:\>azure rediscache create -h
    help:    Create a Redis Cache
    help:
    help:    Usage: rediscache create [--name <name> --resource-group <resource-group> --location <location> [options]]
    help:
    help:    Options:
    help:      -h, --help                                               output usage information
    help:      -v, --verbose                                            use verbose output
    help:      -vv                                                      more verbose with debug output
    help:      --json                                                   use json output
    help:      -n, --name <name>                                        Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>                    Name of the Resource Group
    help:      -l, --location <location>                                Location to create cache.
    help:      -z, --size <size>                                        Size of the Redis Cache. Valid values: [C0, C1, C2, C3, C4, C5, C6, P1, P2, P3, P4]
    help:      -x, --sku <sku>                                          Redis SKU. Should be one of : [Basic, Standard, Premium]
    help:      -e, --enable-non-ssl-port                                EnableNonSslPort property of the Redis Cache. Add this flag if you want to enable the Non SSL Port for your cache
    help:      -c, --redis-configuration <redis-configuration>          Redis Configuration. Enter a JSON formatted string of configuration keys and values here. Format:"{"<key1>":"<value1>","<key2>":"<value2>"}"
    help:      -f, --redis-configuration-file <redisConfigurationFile>  Redis Configuration. Enter the path of a file containing configuration keys and values here. Format for the file entry: {"<key1>":"<value1>","<key2>":"<value2>"}
    help:      -r, --shard-count <shard-count>                          Number of Shards to create on a Premium Cluster Cache
    help:      -v, --virtual-network <virtual-network>                  The exact ARM resource ID of the virtual network to deploy the redis cache in. Example format: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1
    help:      -t, --subnet <subnet>                                    Required when deploying a redis cache inside an existing Azure Virtual Network
    help:      -p, --static-ip <static-ip>                              Required when deploying a redis cache inside an existing Azure Virtual Network
    help:      -s, --subscription <id>                                  the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="delete-an-existing-redis-cache"></a>Usuwanie istniejącej pamięci podręcznej Redis

Aby usunąć Redis pamięci podręcznej, wykonaj następujące polecenie:

    azure rediscache delete [--name <name> --resource-group <resource-group> ]

Aby uzyskać więcej informacji na temat tego polecenia Uruchom `azure rediscache delete -h` polecenia.

    C:\>azure rediscache delete -h
    help:    Delete an existing Redis Cache
    help:
    help:    Usage: rediscache delete [--name <name> --resource-group <resource-group> ]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of the Resource Group under which the cache exists
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="list-all-redis-caches-within-your-subscription-or-resource-group"></a>Lista wszystkich Redis bufory w ramach Twojej subskrypcji lub grupa zasobów

Aby wyświetlić listę wszystkich Redis bufory w ramach Twojej subskrypcji lub grupa zasobów, użyj następującego polecenia:

    azure rediscache list [options]

Aby uzyskać więcej informacji na temat tego polecenia Uruchom `azure rediscache list -h` polecenia.

    C:\>azure rediscache list -h
    help:    List all Redis Caches within your Subscription or Resource Group
    help:
    help:    Usage: rediscache list [options]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -g, --resource-group <resource-group>  Name of the Resource Group
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="show-properties-of-an-existing-redis-cache"></a>Wyświetlanie właściwości istniejącego Redis pamięci podręcznej

Aby wyświetlić właściwości istniejącego Redis pamięci podręcznej, użyj następującego polecenia:

    azure rediscache show [--name <name> --resource-group <resource-group>]

Aby uzyskać więcej informacji na temat tego polecenia Uruchom `azure rediscache show -h` polecenia.

    C:\>azure rediscache show -h
    help:    Show properties of an existing Redis Cache
    help:
    help:    Usage: rediscache show [--name <name> --resource-group <resource-group>]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of the Resource Group
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

<a name="scale"></a>
## <a name="change-settings-of-an-existing-redis-cache"></a>Zmienianie ustawień istniejącej pamięci podręcznej Redis

Aby zmienić ustawienia istniejącej pamięci podręcznej Redis, użyj następującego polecenia:

    azure rediscache set [--name <name> --resource-group <resource-group> --redis-configuration <redis-configuration>/--redis-configuration-file <redisConfigurationFile>]

Aby uzyskać więcej informacji na temat tego polecenia Uruchom `azure rediscache set -h` polecenia.

    C:\>azure rediscache set -h
    help:    Change settings of an existing Redis Cache
    help:
    help:    Usage: rediscache set [--name <name> --resource-group <resource-group> --redis-configuration <redis-configuration>/--redis-configuration-file <redisConfigurationFile>]
    help:
    help:    Options:
    help:      -h, --help                                               output usage information
    help:      -v, --verbose                                            use verbose output
    help:      -vv                                                      more verbose with debug output
    help:      --json                                                   use json output
    help:      -n, --name <name>                                        Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>                    Name of the Resource Group
    help:      -c, --redis-configuration <redis-configuration>          Redis Configuration. Enter a JSON formatted string of configuration keys and values here.
    help:      -f, --redis-configuration-file <redisConfigurationFile>  Redis Configuration. Enter the path of a file containing configuration keys and values here.
    help:      -s, --subscription <subscription>                        the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="renew-the-authentication-key-for-an-existing-redis-cache"></a>Odnawiania klucza uwierzytelniania dla istniejących Redis pamięci podręcznej

Aby odnowić klucz uwierzytelniania dla istniejących Redis pamięci podręcznej, użyj następującego polecenia:

    azure rediscache renew-key [--name <name> --resource-group <resource-group> --key-type <key-type>]

Określanie `Primary` lub `Secondary` dla `key-type`.

Aby uzyskać więcej informacji na temat tego polecenia Uruchom `azure rediscache renew-key -h` polecenia.

    C:\>azure rediscache renew-key -h
    help:    Renew the authentication key for an existing Redis Cache
    help:
    help:    Usage: rediscache renew-key [--name <name> --resource-group <resource-group> ]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of the Resource Group under which cache exists
    help:      -t, --key-type <key-type>              type of key to renew. Valid values are: 'Primary', 'Secondary'.
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="list-primary-and-secondary-keys-of-an-existing-redis-cache"></a>Lista głównego i pomocniczego klawiszy istniejących Redis pamięci podręcznej

Lista głównego i pomocniczego kluczy istniejących Redis pamięci podręcznej Użyj następującego polecenia:

    azure rediscache list-keys [--name <name> --resource-group <resource-group>]

Aby uzyskać więcej informacji na temat tego polecenia Uruchom `azure rediscache list-keys -h` polecenia.

    C:\>azure rediscache list-keys -h
    help:    Lists Primary and Secondary key of an existing Redis Cache
    help:
    help:    Usage: rediscache list-keys [--name <name> --resource-group <resource-group>]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of the Resource Group under which Cache exists
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)
