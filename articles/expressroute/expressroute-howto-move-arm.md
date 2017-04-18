<properties
   pageTitle="Przenoszenie obwodów ExpressRoute od klasycznych do Menedżera zasobów | Microsoft Azure"
   description="Tej stronie opisano, jak przenieść klasyczny elektrycznego model wdrożenia Menedżera zasobów."
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


# <a name="move-expressroute-circuits-from-the-classic-to-the-resource-manager-deployment-model"></a>Przenoszenie obwodów ExpressRoute z klasycznego modelu wdrożenia Menedżera zasobów

## <a name="configuration-prerequisites"></a>Wymagania wstępne dotyczące konfiguracji

- Potrzebujesz najnowszą wersję programu Azure PowerShell modułów (co najmniej w wersji 1.0).
- Upewnij się, że przejrzeniu [wymagania wstępne](expressroute-prerequisites.md), [routingu wymagania](expressroute-routing.md)i [przepływy pracy](expressroute-workflows.md) przed rozpoczęciem konfiguracji.
- Przed poprzedzających dalszych, przejrzyj informacje, które znajduje się w obszarze [przenoszenia obwód ExpressRoute z klasycznej do Menedżera zasobów](expressroute-move.md). Upewnij się, że możesz mieć w pełni zrozumiałe ograniczenia oraz ograniczenia możliwości.
- Jeśli chcesz przenieść obwód Azure ExpressRoute z modelu Klasyczny wdrożenia do modelu wdrożenia Menedżera zasobów Azure musi być elektrycznego skonfigurowana i działania w modelu Klasyczny wdrożenia.
- Upewnij się, że masz grupa zasobów, która została utworzona w modelu wdrożenia Menedżera zasobów.

## <a name="move-the-expressroute-circuit-to-the-resource-manager-deployment-model"></a>Przenieś elektrycznego ExpressRoute do modelu wdrożenia Menedżera zasobów

Należy przenieść obwód ExpressRoute do modelu wdrożenia Menedżera zasobów, aby można go używać różnych zarówno klasycznego, jak i modeli wdrażania Menedżera zasobów. Możesz to zrobić, uruchamiając następujące polecenia programu PowerShell.

### <a name="step-1-gather-circuit-details-from-the-classic-deployment-model"></a>Krok 1: Gromadzenie szczegóły elektrycznego z modelu Klasyczny wdrażania

Musisz najpierw zbieranie informacji na temat usługi obwód ExpressRoute.

Zaloguj się do Azure klasycznego środowiska i gromadzenie klucza usługi. Poniższy fragment programu PowerShell umożliwia uzyskiwanie potrzebnych informacji:

    # Sign in to your Azure account
    Add-AzureAccount

    # Select the appropriate Azure subscription
    Select-AzureSubscription "<Enter Subscription Name here>"

    # Import the PowerShell modules for Azure and ExpressRoute
    Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
    Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'

    # Get the service keys of all your ExpressRoute circuits
    Get-AzureDedicatedCircuit

Kopiowanie **klucza usługi** obwodu, który chcesz przenieść do modelu wdrożenia Menedżera zasobów.

### <a name="step-2-sign-in-to-the-resource-manager-environment-and-create-a-new-resource-group"></a>Krok 2: Logowanie się do środowiska Menedżera zasobów i tworzenie nowej grupy zasobów

Można utworzyć nową grupę zasobów za pomocą następujących wstawki kodu:

    # Sign in to your Azure Resource Manager environment
    Login-AzureRmAccount

    # Select the appropriate Azure subscription
    Get-AzureRmSubscription -SubscriptionName "<Enter Subscription Name here>" | Select-AzureRmSubscription

    #Create a new resource group if you don't already have one
    New-AzureRmResourceGroup -Name "DemoRG" -Location "West US"

Jeśli masz już, można również Użyj istniejącej grupy zasobów.

### <a name="step-3-move-the-expressroute-circuit-to-the-resource-manager-deployment-model"></a>Krok 3: Przenieś elektrycznego ExpressRoute do modelu wdrożenia Menedżera zasobów

Teraz możesz przystąpić do przenieść do elektrycznego ExpressRoute z klasycznego do modelu wdrożenia Menedżera zasobów. Przejrzyj informacje zawarte w obszarze [przenoszenia obwód ExpressRoute od klasycznych do modelu wdrożenia Menedżera zasobów](expressroute-move.md) przed wykonaniem dalszych.

Można to zrobić, uruchamiając następujące wstawki kodu:

    Move-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "DemoRG" -Location "West US" -ServiceKey "<Service-key>"

>[AZURE.NOTE] Po zakończeniu Przenieś nową nazwę, która jest wyświetlana w poprzednim polecenia cmdlet będą używane do adresowania zasobu. Obwód zasadniczo zostanie zmieniona.

## <a name="enable-an-expressroute-circuit-for-both-deployment-models"></a>Włączanie obwód ExpressRoute dla obu modeli wdrażania

Należy przenieść do elektrycznego ExpressRoute do modelu wdrożenia Menedżera zasobów przed kontrolowanie dostępu do modelu wdrożenia.

Uruchom następujące polecenie cmdlet, aby umożliwić dostęp do obu modeli wdrażania:

    # Get details of the ExpressRoute circuit
    $ckt = Get-AzureRmExpressRouteCircuit -Name "DemoCkt" -ResourceGroupName "DemoRG"

    #Set "Allow Classic Operations" to TRUE
    $ckt.AllowClassicOperations = $true

    # Update circuit
    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

Po tej operacji zakończyło się pomyślnie, można wyświetlić obwód w modelu Klasyczny wdrożenia.

Uruchom poniższe czynności, aby uzyskać szczegółowe informacje o układzie ExpressRoute:

    get-azurededicatedcircuit

Musi być widoczny na liście klucz usługi. Łącza do elektrycznego ExpressRoute przy użyciu polecenia standardowym klasycznym wdrożeniu modelu dla VNets klasyczny i standardowe polecenia ARM dla ARM VNETs mogą teraz zarządzać. Następujące artykuły przeprowadzi Cię przez proces Zarządzanie łączami do elektrycznego ExpressRoute:

- [Łącze wirtualnej sieci z obwodem ExpressRoute w modelu wdrożenia Menedżera zasobów](expressroute-howto-linkvnet-arm.md)
- [Łącze wirtualnej sieci z obwodem ExpressRoute w modelu Klasyczny wdrażania](expressroute-howto-linkvnet-classic.md)


## <a name="disable-the-expressroute-circuit-to-the-classic-deployment-model"></a>Wyłączanie obwód ExpressRoute do modelu Klasyczny wdrażania

Uruchom następujące polecenie cmdlet, aby wyłączyć dostęp do modelu Klasyczny wdrażania:

    # Get details of the ExpressRoute circuit
    $ckt = Get-AzureRmExpressRouteCircuit -Name "DemoCkt" -ResourceGroupName "DemoRG"

    #Set "Allow Classic Operations" to FALSE
    $ckt.AllowClassicOperations = $false

    # Update circuit
    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

Po tej operacji zakończyło się pomyślnie, nie można wyświetlić obwód w modelu Klasyczny wdrożenia.

## <a name="next-steps"></a>Następne kroki

Po utworzeniu usługi elektrycznego, upewnij się, możesz wykonać następujące czynności:

- [Tworzenie i modyfikowanie routingu dla swojego obwodu ExpressRoute](expressroute-howto-routing-arm.md)
- [Łącze wirtualnej sieci z obwodem ExpressRoute](expressroute-howto-linkvnet-arm.md)
