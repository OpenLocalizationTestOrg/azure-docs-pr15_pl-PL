<properties
   pageTitle="Tworzenie i modyfikowanie obwód ExpressRoute za pomocą Menedżera zasobów i programu PowerShell | Microsoft Azure"
   description="Ten artykuł zawiera opis sposobu tworzenia, obsługi administracyjnej, sprawdź, aktualizowanie, usuwanie i deprovision obwód ExpressRoute."
   documentationCenter="na"
   services="expressroute"
   authors="ganesr"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/10/2016"
   ms.author="ganesr"/>


# <a name="create-and-modify-an-expressroute-circuit"></a>Tworzenie i modyfikowanie obwód ExpressRoute

> [AZURE.SELECTOR]
[Portal Azure - Menedżer zasobów](expressroute-howto-circuit-portal-resource-manager.md)
[programu PowerShell — Menedżer zasobów](expressroute-howto-circuit-arm.md)
[PowerShell — klasyczny](expressroute-howto-circuit-classic.md)


Ten artykuł zawiera opis sposobu tworzenia obwód Azure ExpressRoute przy użyciu poleceń cmdlet programu Windows PowerShell i model wdrożenia Azure Menedżera zasobów. W tym artykule będzie również pokazano, jak sprawdzić stan obwodu, aktualizowanie lub usuwanie i deprovision go.

**Informacje dotyczące modeli Azure wdrażania**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="before-you-begin"></a>Przed rozpoczęciem


- Uzyskać najnowszą wersję programu PowerShell Azure modułów (co najmniej w wersji 1.0). Aby uzyskać instrukcje krok po kroku dotyczące konfigurowania komputera do moduły programu PowerShell postępuj zgodnie z instrukcjami w [temacie jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md).

- Przed rozpoczęciem konfiguracji, przejrzyj [Warunki wstępne](expressroute-prerequisites.md) i [przepływy pracy](expressroute-workflows.md) .

## <a name="create-and-provision-an-expressroute-circuit"></a>Tworzenie i obsługi administracyjnej obwód ExpressRoute

### <a name="1-sign-in-to-your-azure-account-and-select-your-subscription"></a>1. Zaloguj się do konta Azure i wybierz pozycję subskrypcji

Aby rozpocząć konfiguracji, zaloguj się do swojego konta Azure. Aby uzyskać więcej informacji na temat programu PowerShell zobacz [Przy użyciu programu Windows PowerShell przy użyciu Menedżera zasobów](../powershell-azure-resource-manager.md). Użyj poniższych przykładach umożliwiające nawiązywanie połączeń:

    Login-AzureRmAccount

Sprawdź subskrypcji dla tego konta:

    Get-AzureRmSubscription

Wybierz subskrypcję, do której chcesz utworzyć układ ExpressRoute dla:

    Select-AzureRmSubscription -SubscriptionId "<subscription ID>"

### <a name="2-get-the-list-of-supported-providers-locations-and-bandwidths"></a>2. uzyskać listę obsługiwanych dostawców, lokalizacji i przepustowości

Przed utworzeniem obwód ExpressRoute muszą się na liście obsługiwanych łączności dostawców, lokalizacje i opcje przepustowości.

Polecenia cmdlet programu PowerShell `Get-AzureRmExpressRouteServiceProvider` zwraca informacje, które będą używane w dalszych krokach:

    Get-AzureRmExpressRouteServiceProvider

Sprawdź, czy dostawcy łączności znajduje się tam. Ponieważ musisz go później podczas tworzenia obwodu, należy zwrócić uwagę następujące informacje:

- Nazwa

- PeeringLocations

- BandwidthsOffered

Teraz możesz utworzyć obwód ExpressRoute.   

### <a name="3-create-an-expressroute-circuit"></a>3. Tworzenie obwodu ExpressRoute

Jeśli nie masz jeszcze grupa zasobów, musisz utworzyć jeden przed utworzeniem usługi obwód ExpressRoute. Aby to zrobić, uruchamiając następujące polecenie:


    New-AzureRmResourceGroup -Name "ExpressRouteResourceGroup" -Location "West US"


W poniższym przykładzie pokazano, jak utworzyć elektrycznego ExpressRoute za pośrednictwem Equinix 200-MB/s w Dolinie Krzemowej. Jeśli korzystasz z innego dostawcy i innych ustawień, należy zastąpić te informacje po wprowadzeniu żądania. Oto przykład wezwanie na celu uzyskanie nowego klucza usługi:

    New-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup" -Location "West US" -SkuTier Standard -SkuFamily MeteredData -ServiceProviderName "Equinix" -PeeringLocation "Silicon Valley" -BandwidthInMbps 200

Upewnij się, można określić poprawny warstwa SKU i rodzina SKU:

- Jednostka SKU warstwa Określa, czy standardowy ExpressRoute lub dodatek premium ExpressRoute jest włączona. Możesz określić *Standardowy* uzyskać dodatek premium standard SKU lub *Premium* .

- Rodzina SKU Określa typ rozliczeń. Możesz określić *Metereddata* planu taryfowego danych i *Unlimiteddata* planu nieograniczoną danych. Należy zauważyć, że można zmienić typ rozliczeń z *Metereddata* na *Unlimiteddata*, ale nie można zmienić typu z *Unlimiteddata* *Metereddata*.


>[AZURE.IMPORTANT] Usługi elektrycznego ExpressRoute będą naliczane od momentu wystawienia klucza usługi. Upewnij się, wykonania tej operacji, gdy dostawca łączności będzie gotowa do obsługi administracyjnej obwodu.

Odpowiedź zawiera klucz usługi. Szczegółowe opisy wszystkich parametrów można uzyskać, wykonując następujące czynności:


    get-help New-AzureRmExpressRouteCircuit -detailed


### <a name="4-list-all-expressroute-circuits"></a>4. listy wszystkich obwodów ExpressRoute

Aby uzyskać listę wszystkich obwodów ExpressRoute utworzone przez użytkownika, uruchom `Get-AzureRmExpressRouteCircuit` polecenie:


    Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

Odpowiedź będzie wyglądać podobnie do następującego przykładu:


    Name                             : ExpressRouteARMCircuit
    ResourceGroupName                : ExpressRouteResourceGroup
    Location                         : westus
    Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                         "Name": "Standard_MeteredData",
                                         "Tier": "Standard",
                                         "Family": "MeteredData"
                                       }
    CircuitProvisioningState          : Enabled
    ServiceProviderProvisioningState  : NotProvisioned
    ServiceProviderNotes              :
    ServiceProviderProperties         : {
                                          "ServiceProviderName": "Equinix",
                                          "PeeringLocation": "Silicon Valley",
                                          "BandwidthInMbps": 200
                                        }
    ServiceKey                        : **************************************
    Peerings                          : []

Te informacje w dowolnym momencie można pobrać za pomocą `Get-AzureRmExpressRouteCircuit` polecenia cmdlet. Nawiązywanie połączenia bez parametrów wyświetla listę wszystkich obwodów. Klucz usługi zostaną wyświetlone w polu *klucz ServiceKey* :


    Get-AzureRmExpressRouteCircuit


Odpowiedź będzie wyglądać podobnie do następującego przykładu:


    Name                             : ExpressRouteARMCircuit
    ResourceGroupName                : ExpressRouteResourceGroup
    Location                         : westus
    Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                         "Name": "Standard_MeteredData",
                                         "Tier": "Standard",
                                         "Family": "MeteredData"
                                       }
    CircuitProvisioningState         : Enabled
    ServiceProviderProvisioningState : NotProvisioned
    ServiceProviderNotes             :
    ServiceProviderProperties        : {
                                         "ServiceProviderName": "Equinix",
                                         "PeeringLocation": "Silicon Valley",
                                         "BandwidthInMbps": 200
                                       }
    ServiceKey                       : **************************************
    Peerings                         : []


Szczegółowe opisy wszystkich parametrów można uzyskać, wykonując następujące czynności:


    get-help Get-AzureRmExpressRouteCircuit -detailed

### <a name="5-send-the-service-key-to-your-connectivity-provider-for-provisioning"></a>5. wysłać klucz usługi do dostawcy łączności dla inicjowania obsługi administracyjnej

*ServiceProviderProvisioningState* zawiera informacje o bieżącym stanie inicjowania obsługi administracyjnej na stronie dostawcy usługi. Stan pokazuje stan na stronie firmy Microsoft. Aby uzyskać więcej informacji o elektrycznego inicjowania obsługi administracyjnej Państwa zobacz artykuł [przepływy pracy](expressroute-workflows.md#expressroute-circuit-provisioning-states) .

Po utworzeniu nowy obwód ExpressRoute obwodu będzie w następującym stanie:


    ServiceProviderProvisioningState : NotProvisioned
    CircuitProvisioningState         : Enabled



Obwód będą się zmieniały następujący stan dostawcy łączności Trwa włączanie możesz:

    ServiceProviderProvisioningState : Provisioning
    Status                           : Enabled

Należy korzystać obwód ExpressRoute musi być w stanie następujące czynności:

    ServiceProviderProvisioningState : Provisioned
    CircuitProvisioningState         : Enabled

### <a name="6-periodically-check-the-status-and-the-state-of-the-circuit-key"></a>6. okresowego sprawdzania stanu i stan klucza elektrycznego

Sprawdzanie stanu i stan klucza elektrycznego umożliwia sprawdzenie, kiedy dostawcy włączono usługi elektrycznego. Po obwodu została skonfigurowana, *ServiceProviderProvisioningState* jest wyświetlany jako *Provisioned*, jak pokazano w poniższym przykładzie:


    Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"


Odpowiedź będzie wyglądać podobnie do następującego przykładu:


    Name                             : ExpressRouteARMCircuit
    ResourceGroupName                : ExpressRouteResourceGroup
    Location                         : westus
    Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                         "Name": "Standard_MeteredData",
                                         "Tier": "Standard",
                                         "Family": "MeteredData"
                                       }
    CircuitProvisioningState         : Enabled
    ServiceProviderProvisioningState : Provisioned
    ServiceProviderNotes             :
    ServiceProviderProperties        : {
                                         "ServiceProviderName": "Equinix",
                                         "PeeringLocation": "Silicon Valley",
                                         "BandwidthInMbps": 200
                                    }
    ServiceKey                       : **************************************
    Peerings                         : []

### <a name="7-create-your-routing-configuration"></a>7. Tworzenie konfiguracji routingu

Instrukcje krok po kroku zobacz artykuł [konfiguracji routingu elektrycznego ExpressRoute](expressroute-howto-routing-arm.md) do tworzenia i modyfikowania peerings elektrycznego.


>[AZURE.IMPORTANT] Te instrukcje dotyczą tylko obwodów, które są tworzone z dostawcami usług, które oferują usługi łączności 2 warstwy. Jeśli korzystasz z usługodawcy, który oferuje zarządzanych warstwy 3 usługi (zazwyczaj IP VPN, takich jak MPLS), dostawcy łączności skonfiguruje i zarządzanie routingu dla Ciebie.

### <a name="8-link-a-virtual-network-to-an-expressroute-circuit"></a>8. łącze wirtualną sieć obwodem ExpressRoute

Następnie łącze wirtualnej sieci do usługi obwód ExpressRoute. Użyj tego artykułu [Łączenie sieci wirtualne obwody ExpressRoute](expressroute-howto-linkvnet-arm.md) podczas pracy z modelem wdrożenia Menedżera zasobów.

## <a name="getting-the-status-of-an-expressroute-circuit"></a>Pobieranie stanu obwód ExpressRoute

Te informacje w dowolnym momencie można pobrać za pomocą `Get-AzureRmExpressRouteCircuit` polecenia cmdlet. Nawiązywanie połączenia bez parametrów wyświetla listę wszystkich obwodów.

    Get-AzureRmExpressRouteCircuit


Odpowiedź będzie podobny do następującego przykładu:


    Name                             : ExpressRouteARMCircuit
    ResourceGroupName                : ExpressRouteResourceGroup
    Location                         : westus
    Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                         "Name": "Standard_MeteredData",
                                         "Tier": "Standard",
                                         "Family": "MeteredData"
                                       }
    CircuitProvisioningState         : Enabled
    ServiceProviderProvisioningState : Provisioned
    ServiceProviderNotes             :
    ServiceProviderProperties        : {
                                         "ServiceProviderName": "Equinix",
                                         "PeeringLocation": "Silicon Valley",
                                         "BandwidthInMbps": 200
                                       }
    ServiceKey                       : **************************************
    Peerings                         : []


Dowiedz się o określonych obwód ExpressRoute, przekazując nazwy grupy zasobów i obwód nazwy jako parametr do połączenia:


    Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"


Odpowiedź będzie wyglądać podobnie do następującego przykładu:


    Name                             : ExpressRouteARMCircuit
    ResourceGroupName                : ExpressRouteResourceGroup
    Location                         : westus
    Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                         "Name": "Standard_MeteredData",
                                         "Tier": "Standard",
                                         "Family": "MeteredData"
                                       }
    CircuitProvisioningState         : Enabled
    ServiceProviderProvisioningState : Provisioned
    ServiceProviderNotes             :
    ServiceProviderProperties        : {
                                         "ServiceProviderName": "Equinix",
                                         "PeeringLocation": "Silicon Valley",
                                         "BandwidthInMbps": 200
                                       }
    ServiceKey                       : **************************************
    Peerings                         : []


Szczegółowe opisy wszystkich parametrów można uzyskać, wykonując następujące czynności:

    get-help get-azurededicatedcircuit -detailed


## <a name="modify"></a>Modyfikowanie obwód ExpressRoute

Niektóre właściwości obwód ExpressRoute można modyfikować bez wpływania łączności.

Możesz wykonać następujące czynności bez przestojów:

- Włączanie lub wyłączanie dodatek premium ExpressRoute dla usługi obwodu ExpressRoute.
- Zwiększanie przepustowości sieci obwodu ExpressRoute. Należy zauważyć, że obniżanie wersji przepustowości obwodu nie jest obsługiwana.
- Zmienianie pomiaru planu z taryfowe danych na nieograniczoną danych. Należy zauważyć, że zmiana pomiaru planu z danych nieograniczoną taryfowe danych nie jest obsługiwana.
-  Można włączać i wyłączać *Operacje klasyczny*.

Aby uzyskać więcej informacji na ograniczenia i limity zapoznaj się z [ExpressRoute — często zadawane pytania](expressroute-faqs.md).

### <a name="to-enable-the-expressroute-premium-add-on"></a>Aby włączyć dodatek premium ExpressRoute

Możesz włączyć dodatek premium ExpressRoute dla swojego istniejącego obwodu za pomocą następujących wstawkę kodu programu PowerShell:

    $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    $ckt.Sku.Tier = "Premium"
    $ckt.sku.Name = "Premium_MeteredData"

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt


Układ będzie teraz ExpressRoute premium dodatku funkcje są włączone. Należy zauważyć, że rozpocznie się rozliczenia możliwości dodatek premium zaraz po pomyślnym wykonaniu polecenia.

### <a name="to-disable-the-expressroute-premium-add-on"></a>Aby wyłączyć dodatek premium ExpressRoute

>[AZURE.IMPORTANT] Operacja może zakończyć się niepowodzeniem, jeśli korzystasz z zasobów, które są większe niż co to jest dozwolone w przypadku elektrycznego standardowy.

Pamiętaj o następujących kwestiach:

- Przed starszą wersję z premium na standardowy, należy się upewnić, że liczbę wirtualnych sieci, które są połączone z obwodem jest mniejsza niż 10. Jeśli nie możesz to zrobić, żądania aktualizacji kończy się niepowodzeniem, a zostanie obciążony możesz szybkością premium.

- Musisz odłączyć wszystkich wirtualnych sieci w innych regionach geopolitycznych. Jeśli nie możesz to zrobić, żądania aktualizacji zakończy się niepowodzeniem, a zostanie obciążony możesz szybkością premium.

- Tabela rozsyłania musi być mniejsza niż 4000 trasy zaglądanie prywatne. Jeśli rozmiar tabeli rozsyłania jest większa niż 4000 przekierowuje, sesji BGP umieści i nie będzie reenabled, aż liczba prefiksów anonsowanych przechodzi poniżej 4 000.

Możesz wyłączyć dodatek premium ExpressRoute dla istniejących obwodu przy użyciu następującego polecenia cmdlet programu PowerShell:


    $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    $ckt.Sku.Tier = "Standard"
    $ckt.sku.Name = "Standard_MeteredData"

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt


### <a name="to-update-the-expressroute-circuit-bandwidth"></a>Aby zaktualizować przepustowości elektrycznego ExpressRoute

Opcje obsługiwane przepustowości dla swojego dostawcy można znaleźć w sekcji [ExpressRoute — często zadawane pytania](expressroute-faqs.md). Można wybrać dowolny rozmiarze większym niż rozmiar obwodu istniejących.

>[AZURE.IMPORTANT] Nie można zmniejszyć przepustowość obwodu ExpressRoute bez zakłóceń. Obniżanie wersji przepustowości wymaga deprovision obwód ExpressRoute, a następnie reprovision nowy obwód ExpressRoute.

Po zdecydowaniu, rozmiar, jakiego potrzebujesz, użyj następującego polecenia, aby zmienić rozmiar do elektrycznego:


    $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    $ckt.ServiceProviderProperties.BandwidthInMbps = 1000

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt


Usługi obwód zostanie rozmiar w górę na stronie firmy Microsoft. Następnie należy skontaktuj się z dostawcą łączności, aby zaktualizować konfiguracji na swojej stronie zgodnie z tej zmiany. Po wprowadzeniu to powiadomienie rozpocznie się rozliczenia możesz dla opcji przepustowości zaktualizowane.


### <a name="to-move-the-sku-from-metered-to-unlimited"></a>Aby przenieść SKU z taryfowe nieograniczone

Jednostka SKU obwodu ExpressRoute można zmienić za pomocą następujących wstawkę kodu programu PowerShell:

    $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    $ckt.Sku.Family = "UnlimitedData"
    $ckt.sku.Name = "Premium_UnlimitedData"

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

### <a name="to-control-access-to-the-classic-and-resource-manager-environments"></a>Kontrolowanie dostępu do klasycznego i w środowisku Menedżera zasobów  

Przeczytaj instrukcje w [obwody ExpressRoute przechodzenie od klasycznych do modelu wdrożenia Menedżera zasobów](expressroute-howto-move-arm.md).  


## <a name="deprovisioning-and-deleting-an-expressroute-circuit"></a>Cofanie ubezpieczeń i usuwanie obwód ExpressRoute

Pamiętaj o następujących kwestiach:

- Musisz odłączyć wszystkich wirtualnych sieci z obwód ExpressRoute. Jeśli operacja nie powiedzie się, należy sprawdzić, jeśli dowolne wirtualnych sieci są połączone z obwodem.

- W przypadku stanu ExpressRoute elektrycznego usługi dostawcy obsługi administracyjnej **obsługi** lub **Provisioned** musi przetwarzania dostawca usług do deprovision elektrycznego na bok. Będziemy rezerwowanie zasobów i BOM, możesz do czasu dostawca usług wykonuje cofanie ubezpieczeń obwodu i powiadomienia z nami.

- Jeśli dostawca usług ma wstrzymano obsługę administracyjną elektrycznego (stan inicjowania obsługi administracyjnej dostawcy usługi jest ustawiona na **nie obsługi administracyjnej**) można usunąć obwodu. Spowoduje to zatrzymanie rozliczeniami dla układu

Możesz usunąć z obwodu ExpressRoute, uruchamiając następujące polecenie:

    Remove-AzureRmExpressRouteCircuit -ResourceGroupName "ExpressRouteResourceGroup" -Name "ExpressRouteARMCircuit"



## <a name="next-steps"></a>Następne kroki

Po utworzeniu usługi elektrycznego, upewnij się, możesz wykonać następujące czynności:

- [Tworzenie i modyfikowanie routingu dla swojego obwodu ExpressRoute](expressroute-howto-routing-arm.md)
- [Łącze wirtualnej sieci z obwodem ExpressRoute](expressroute-howto-linkvnet-arm.md)
