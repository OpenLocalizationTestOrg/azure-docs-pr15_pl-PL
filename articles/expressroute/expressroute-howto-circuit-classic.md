<properties
   pageTitle="Tworzenie i modyfikowanie obwód ExpressRoute przy użyciu modelu Klasyczny wdrożenia i programu PowerShell | Microsoft Azure"
   description="W tym artykule opisano kroki tworzenia i inicjowania obsługi administracyjnej obwód ExpressRoute. W tym artykule również pokazano, jak sprawdzić stan, aktualizowanie lub usuwanie i deprovision do obwodu."
   documentationCenter="na"
   services="expressroute"
   authors="ganesr"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/10/2016"
   ms.author="ganesr;cherylmc"/>

# <a name="create-and-modify-an-expressroute-circuit"></a>Tworzenie i modyfikowanie obwód ExpressRoute

> [AZURE.SELECTOR]
[Portal Azure - Menedżer zasobów](expressroute-howto-circuit-portal-resource-manager.md)
[programu PowerShell — Menedżer zasobów](expressroute-howto-circuit-arm.md)
[PowerShell — klasyczny](expressroute-howto-circuit-classic.md)

W tym artykule opisano czynności, aby utworzyć układ Azure ExpressRoute przy użyciu poleceń cmdlet programu PowerShell i modelu Klasyczny wdrożenia. W tym artykule będzie również pokazano, jak sprawdzić stan, aktualizowanie lub usuwanie i deprovision obwód ExpressRoute.

**Informacje dotyczące modeli Azure wdrażania**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]


## <a name="before-you-begin"></a>Przed rozpoczęciem

### <a name="1-review-the-prerequisites-and-workflow-articles"></a>1. Sprawdź wymagania wstępne i artykuły przepływu pracy

Upewnij się, że przejrzeniu [wymagania wstępne](expressroute-prerequisites.md) i [przepływy pracy](expressroute-workflows.md) przed rozpoczęciem konfiguracji.  


### <a name="2-install-the-latest-versions-of-the-azure-powershell-modules"></a>2. Zainstaluj najnowsze wersje modułów Azure programu PowerShell 

Postępuj zgodnie z instrukcjami w [temacie jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md) instrukcje krok po kroku dotyczące konfigurowania komputera do moduły Azure programu PowerShell.

### <a name="3-log-in-to-your-azure-account-and-select-a-subscription"></a>3. Zaloguj się do konta Azure i wybierz subskrypcję

1. Uruchom następujące polecenie cmdlet przy użyciu wierszu pełnymi uprawnieniami programu Windows PowerShell:

        Add-AzureAccount
2. W logowania wyświetlonym ekranie Zaloguj się do swojego konta.

3. Pobierz listę subskrypcji.

        Get-AzureSubscription
4. Wybierz subskrypcję, do której chcesz użyć.
    
        Select-AzureSubscription -SubscriptionName "mysubscriptionname"

## <a name="create-and-provision-an-expressroute-circuit"></a>Tworzenie i obsługi administracyjnej obwód ExpressRoute

### <a name="1-import-the-powershell-modules-for-expressroute"></a>1. importowanie moduły programu PowerShell dla ExpressRoute

 Jeśli jeszcze tego nie zrobiono, należy zaimportować moduły Azure i ExpressRoute do sesji programu PowerShell, aby rozpocząć korzystanie z poleceń cmdlet ExpressRoute. Możesz zaimportować modułów z lokalizacji, w której zostały one zainstalowany na komputerze lokalnym. W zależności od metody, które zostało użyte do zainstalowania modułów lokalizacji mogą być inne niż w poniższym przykładzie. Modyfikowanie przykładzie, jeśli to konieczne.  

    Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
    Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'

### <a name="2-get-the-list-of-supported-providers-locations-and-bandwidths"></a>2. uzyskać listę obsługiwanych dostawców, lokalizacji i przepustowości

Przed utworzeniem obwód ExpressRoute muszą się na liście obsługiwanych łączności dostawców, lokalizacje i opcje przepustowości.

Polecenia cmdlet programu PowerShell `Get-AzureDedicatedCircuitServiceProvider` zwraca informacje, które będą używane w dalszych krokach:

    Get-AzureDedicatedCircuitServiceProvider

Sprawdź, czy dostawcy łączności znajduje się tam. Ponieważ musisz go później podczas tworzenia obwodu, należy zwrócić uwagę następujące informacje:

- Nazwa

- PeeringLocations

- BandwidthsOffered

Teraz możesz utworzyć obwód ExpressRoute.         

### <a name="3-create-an-expressroute-circuit"></a>3. Tworzenie obwodu ExpressRoute

W poniższym przykładzie pokazano, jak utworzyć elektrycznego ExpressRoute za pośrednictwem Equinix 200-MB/s w Dolinie Krzemowej. Jeśli korzystasz z innego dostawcy i innych ustawień, należy zastąpić te informacje po wprowadzeniu żądania.

>[AZURE.IMPORTANT] Usługi elektrycznego ExpressRoute będą naliczane od momentu wystawienia klucza usługi. Upewnij się, wykonania tej operacji, gdy dostawca łączności będzie gotowa do obsługi administracyjnej obwodu.


Oto przykład wezwanie na celu uzyskanie nowego klucza usługi:

    $Bandwidth = 200
    $CircuitName = "MyTestCircuit"
    $ServiceProvider = "Equinix"
    $Location = "Silicon Valley"

    New-AzureDedicatedCircuit -CircuitName $CircuitName -ServiceProviderName $ServiceProvider -Bandwidth $Bandwidth -Location $Location -sku Standard -BillingType MeteredData

Lub, jeśli chcesz utworzyć obwód ExpressRoute z dodatku premium, należy użyć następującego przykładu. Zapoznaj się z [ExpressRoute — często zadawane pytania](expressroute-faqs.md) , aby uzyskać więcej informacji o dodatku premium.

    New-AzureDedicatedCircuit -CircuitName $CircuitName -ServiceProviderName $ServiceProvider -Bandwidth $Bandwidth -Location $Location -sku Premium - BillingType MeteredData


Odpowiedź zawiera klucz usługi. Szczegółowe opisy wszystkich parametrów można uzyskać, wykonując następujące czynności:

    get-help new-azurededicatedcircuit -detailed

### <a name="4-list-all-the-expressroute-circuits"></a>4. listy wszystkich obwodów ExpressRoute

Można uruchamiać `Get-AzureDedicatedCircuit` polecenie, aby uzyskać listę wszystkich utworzonych obwodów ExpressRoute:


    Get-AzureDedicatedCircuit

Odpowiedź będzie podobny do następującego przykładu:

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : NotProvisioned
    Sku                              : Standard
    Status                           : Enabled

Te informacje w dowolnym momencie można pobrać za pomocą `Get-AzureDedicatedCircuit` polecenia cmdlet. Nawiązywanie połączenia bez parametrów wyświetla listę wszystkich obwodów. Klucz usługi zostaną zapisane w polu *klucz ServiceKey* .

    Get-AzureDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : NotProvisioned
    Sku                              : Standard
    Status                           : Enabled

Szczegółowe opisy wszystkich parametrów można uzyskać, wykonując następujące czynności:

    get-help get-azurededicatedcircuit -detailed

### <a name="5-send-the-service-key-to-your-connectivity-provider-for-provisioning"></a>5. wysłać klucz usługi do dostawcy łączności dla inicjowania obsługi administracyjnej


*ServiceProviderProvisioningState* zawiera informacje dotyczące bieżącego stanu inicjowania obsługi administracyjnej na stronie dostawcy usługi. *Stan* pokazuje stan na stronie firmy Microsoft. Aby uzyskać więcej informacji o elektrycznego inicjowania obsługi administracyjnej Państwa zobacz artykuł [przepływy pracy](expressroute-workflows.md#expressroute-circuit-provisioning-states) .

Po utworzeniu nowy obwód ExpressRoute obwodu będzie w następującym stanie:

    ServiceProviderProvisioningState : NotProvisioned
    Status                           : Enabled


Obwód zostaną przekierowane do następującym stanie, gdy dostawca łączności Trwa włączanie możesz:

    ServiceProviderProvisioningState : Provisioning
    Status                           : Enabled

Układ ExpressRoute musi być w stanie następujące dla Ciebie można było używać tej funkcji:

    ServiceProviderProvisioningState : Provisioned
    Status                           : Enabled


### <a name="6-periodically-check-the-status-and-the-state-of-the-circuit-key"></a>6. okresowego sprawdzania stanu i stan klucza elektrycznego

Dzięki temu będzie można ustalić, kiedy dostawcy włączono usługi elektrycznego. Po skonfigurowaniu obwód *ServiceProviderProvisioningState* będzie wyświetlany jako *Provisioned* , jak pokazano w poniższym przykładzie:

    Get-AzureDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

### <a name="7-create-your-routing-configuration"></a>7. Tworzenie konfiguracji routingu

Odwołują się do [obwód ExpressRoute konfiguracji routingu (Tworzenie i modyfikowanie peerings elektrycznego)](expressroute-howto-routing-classic.md) artykuł instrukcje krok po kroku.

>[AZURE.IMPORTANT] Te instrukcje dotyczą tylko obwodów, które są tworzone z dostawcami usług, które oferują usługi łączności 2 warstwy. Jeśli korzystasz z usługodawcy, który oferuje zarządzanych warstwy 3 usługi (zazwyczaj IP VPN, takich jak MPLS), dostawcy łączności skonfiguruje i zarządzanie routingu dla Ciebie.

### <a name="8-link-a-virtual-network-to-an-expressroute-circuit"></a>8. łącze wirtualną sieć obwodem ExpressRoute

Następnie łącze wirtualnej sieci do usługi obwód ExpressRoute. Zapoznaj się z [obwodów elektrycznych ExpressRoute tworzenie łączy do wirtualnych sieci](expressroute-howto-linkvnet-classic.md) instrukcje krok po kroku. Jeśli chcesz utworzyć wirtualnej sieci przy użyciu modelu Klasyczny wdrożenia dla ExpressRoute, zobacz [Tworzenie wirtualna sieć ExpressRoute](expressroute-howto-vnet-portal-classic.md) instrukcje.

## <a name="getting-the-status-of-an-expressroute-circuit"></a>Pobieranie stanu obwód ExpressRoute

Te informacje w dowolnym momencie można pobrać za pomocą `Get-AzureCircuit` polecenia cmdlet. Nawiązywanie połączenia bez parametrów wyświetla listę wszystkich obwodów.

    Get-AzureDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

    Bandwidth                        : 1000
    CircuitName                      : MyAsiaCircuit
    Location                         : Singapore
    ServiceKey                       : #################################
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

Wyświetlane są informacje dotyczące określonych obwodu ExpressRoute, przekazując klucza usługi jako parametr do połączenia:

    Get-AzureDedicatedCircuit -ServiceKey "*********************************"

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled


Szczegółowe opisy wszystkich parametrów można uzyskać, wykonując następujące czynności:

    get-help get-azurededicatedcircuit -detailed

## <a name="modifying-an-expressroute-circuit"></a>Modyfikowanie obwód ExpressRoute

Niektóre właściwości obwód ExpressRoute można modyfikować bez wpływania łączności.

Możesz wykonać następujące czynności bez przestojów:

- Włączanie lub wyłączanie dodatek premium ExpressRoute dla usługi obwodu ExpressRoute.
- Zwiększanie przepustowości sieci obwodu ExpressRoute. Należy zauważyć, że obniżanie wersji przepustowości obwodu nie jest obsługiwana.
- Zmienianie pomiaru planu z taryfowe danych na nieograniczoną danych. Należy zauważyć, że zmiana pomiaru planu z danych nieograniczoną taryfowe danych nie jest obsługiwana.
- Można włączać i wyłączać *Operacje klasyczny*.

Zapoznaj się z [ExpressRoute — często zadawane pytania](expressroute-faqs.md) , aby uzyskać więcej informacji na temat limitów i ograniczenia.

### <a name="to-enable-the-expressroute-premium-add-on"></a>Aby włączyć dodatek premium ExpressRoute

Możesz włączyć dodatek premium ExpressRoute dla swojego istniejącego obwodu przy użyciu następującego polecenia cmdlet programu PowerShell:

    Set-AzureDedicatedCircuitProperties -ServiceKey "*********************************" -Sku Premium

    Bandwidth                        : 1000
    CircuitName                      : TestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Premium
    Status                           : Enabled

Usługi elektrycznego będzie teraz ExpressRoute premium dodatku funkcje są włączone. Należy zauważyć, że firma Microsoft rozpocznie się rozliczenia możliwości dodatek premium zaraz po pomyślnym wykonaniu polecenia.

### <a name="to-disable-the-expressroute-premium-add-on"></a>Aby wyłączyć dodatek premium ExpressRoute

>[AZURE.IMPORTANT] Operacja może zakończyć się niepowodzeniem, jeśli korzystasz z zasobów, które są większe niż co to jest dozwolone w przypadku elektrycznego standardowy.

Pamiętaj o następujących kwestiach:

- Należy się upewnić, liczba wirtualnych sieci połączone z obwodem jest mniejsza niż 10, zanim starszą wersję z premium na standardowy. Jeśli nie jest to zrobisz, żądania aktualizacji zakończy się niepowodzeniem i będzie wystawiona stawki premium.

- Musisz odłączyć wszystkich wirtualnych sieci w innych regionach geopolitycznych. Jeśli nie jest to zrobisz, żądania aktualizacji zakończy się niepowodzeniem i będzie wystawiona stawki premium.

- Tabela rozsyłania musi być mniejsza niż 4000 trasy zaglądanie prywatne. Jeśli rozmiar tabeli rozsyłania jest większa niż 4000 marszrut, sesji BGP zmniejszy się i nie będzie reenabled, aż liczba prefiksów anonsowanych przechodzi poniżej 4 000.

Możesz wyłączyć dodatek premium ExpressRoute dla usługi istniejących obwodu przy użyciu następującego polecenia cmdlet programu PowerShell:

    Set-AzureDedicatedCircuitProperties -ServiceKey "*********************************" -Sku Standard

    Bandwidth                        : 1000
    CircuitName                      : TestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled



### <a name="to-update-the-expressroute-circuit-bandwidth"></a>Aby zaktualizować przepustowości elektrycznego ExpressRoute

Sprawdź [Często zadawane pytania dotyczące ExpressRoute](expressroute-faqs.md) dla opcji obsługiwanych przepustowości dla dostawcy. Możesz wybrać rozmiarze, która jest większa niż rozmiar obwodu istniejących, jak umożliwia fizycznego portu (na którym jest tworzony z elektrycznego).

>[AZURE.IMPORTANT] Nie można zmniejszyć przepustowość obwodu ExpressRoute bez zakłóceń. Obniżanie wersji przepustowości konieczne będzie deprovision obwód ExpressRoute, a następnie reprovision nowy obwód ExpressRoute.

Po zdecydowaniu, rozmiar, jakiego potrzebujesz, możesz zmiany rozmiaru do elektrycznego za pomocą następujące polecenie:

    Set-AzureDedicatedCircuitProperties -ServiceKey ********************************* -Bandwidth 1000

    Bandwidth                        : 1000
    CircuitName                      : TestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

Usługi elektrycznego będzie zostały rozmiar w górę na stronie firmy Microsoft. Musisz skontaktuj się z dostawcą łączności, aby zaktualizować konfiguracji na swojej stronie zgodnie z tej zmiany. Należy zauważyć, że firma Microsoft rozpocznie rozliczenia możesz dla opcji zaktualizowane przepustowości od tej chwili na.

Jeśli zostanie wyświetlony następujący komunikat o błędzie, gdy zwiększanie przepustowości elektrycznego, oznacza to pozostaje nie przepustowości wystarczającej na fizycznego portu, w której jest tworzona z istniejących elektrycznego. Musisz usunąć ten obwód i utworzyć nowy obwód wielkości, które są potrzebne. 

    Set-AzureDedicatedCircuitProperties : InvalidOperation : Insufficient bandwidth available to perform this circuit
    update operation
    At line:1 char:1
    + Set-AzureDedicatedCircuitProperties -ServiceKey ********************* ...
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : CloseError: (:) [Set-AzureDedicatedCircuitProperties], CloudException
        + FullyQualifiedErrorId : Microsoft.WindowsAzure.Commands.ExpressRoute.SetAzureDedicatedCircuitPropertiesCommand
    

## <a name="deprovisioning-and-deleting-an-expressroute-circuit"></a>Cofanie ubezpieczeń i usuwanie obwód ExpressRoute

Pamiętaj o następujących kwestiach:

- Musisz odłączyć wszystkich wirtualnych sieci z elektrycznego ExpressRoute dla tej operacji powiodła się. Sprawdź, czy masz wirtualnych sieci, które są połączone z obwodem, jeśli operacja nie powiedzie się.

- W przypadku stanu ExpressRoute elektrycznego usługi dostawcy obsługi administracyjnej **obsługi** lub **Provisioned** musi przetwarzania dostawca usług do deprovision elektrycznego na bok. Będziemy rezerwowanie zasobów i BOM, możesz do czasu dostawca usług wykonuje cofanie ubezpieczeń obwodu i powiadomienia z nami.

- Jeśli dostawca usług ma wstrzymano obsługę administracyjną elektrycznego (stan inicjowania obsługi administracyjnej dostawcy usługi jest ustawiona na **nie obsługi administracyjnej**) można usunąć obwodu. Spowoduje to zatrzymanie rozliczeniami dla układu.

Możesz usunąć z obwodu ExpressRoute, uruchamiając następujące polecenie:

    Remove-AzureDedicatedCircuit -ServiceKey "*********************************"



## <a name="next-steps"></a>Następne kroki

Po utworzeniu usługi elektrycznego, upewnij się, możesz wykonać następujące czynności:

- [Tworzenie i modyfikowanie routingu dla swojego obwodu ExpressRoute](expressroute-howto-routing-classic.md)
- [Łącze wirtualnej sieci z obwodem ExpressRoute](expressroute-howto-linkvnet-classic.md)
