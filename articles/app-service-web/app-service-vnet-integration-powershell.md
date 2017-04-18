<properties
    pageTitle="Nawiązywanie połączenia z aplikacji z siecią wirtualną przy użyciu programu PowerShell"
    description="Instrukcje dotyczące nawiązywanie i Praca z wirtualnych sieci przy użyciu programu PowerShell"
    services="app-service"
    documentationCenter=""
    authors="ccompy"
    manager="wpickett"
    editor="cephalin"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/29/2016"
    ms.author="ccompy"/>

# <a name="connect-your-app-to-your-virtual-network-by-using-powershell"></a>Nawiązywanie połączenia z aplikacji z siecią wirtualną przy użyciu programu PowerShell #

## <a name="overview"></a>Omówienie ##

W usłudze Azure aplikacji aplikacji (sieć web, telefon komórkowy lub interfejsu API) można nawiązać Azure wirtualnej sieci (VNet) w ramach subskrypcji. Ta funkcja jest nazywana integracji VNet. Nie należy mylić z funkcji integracji VNet przy użyciu funkcji Środowisko usługi aplikacji, która umożliwia uruchamianie wystąpienie usługi aplikacji Azure w sieci wirtualnych.

Funkcja integracji VNet ma interfejsu użytkownika (UI) w portalu nowe używanego do integracji z sieci wirtualne, które są rozmieszczane przy użyciu modelu Klasyczny wdrożenia lub model wdrożenia Azure Menedżera zasobów. Jeśli chcesz dowiedzieć się więcej na temat funkcji, zobacz [Integracja aplikacji z Azure wirtualnej sieci](web-sites-integrate-with-vnet.md).

Ten artykuł dotyczy nie na temat sposobu używania interfejsu użytkownika, ale zamiast o włączaniu integracji przy użyciu programu PowerShell. Ponieważ poleceń dla każdego modelu wdrażania są różne, ten artykuł zawiera sekcja dla każdego modelu wdrożenia.  

Przed rozpoczęciem pracy z tego artykułu, upewnij się, że masz:

- Najnowsze SDK programu PowerShell Azure zainstalowany. Możesz zainstalować to z Instalatora platformy sieci Web.
- Aplikacja usługi aplikacji Azure działa na standardowy lub w wersji Premium.

## <a name="classic-virtual-networks"></a>Klasyczny wirtualnych sieci ##

W tej sekcji omówiono trzy zadania wirtualnych sieci korzystające z modelu Klasyczny wdrażania:

1. Nawiązywanie połączenia aplikacji z wcześniej istniejących wirtualnych sieci, które bramy i skonfigurowano obsługę połączeń punktu do witryny.
1. Aktualizowanie informacji o integracji wirtualną sieć dla aplikacji.
1. Rozłączanie aplikacji z siecią wirtualną.

### <a name="connect-an-app-to-a-classic-vnet"></a>Łączenie aplikacji z VNet klasyczny ###

Aby połączyć aplikacji wirtualnej sieci, wykonaj następujące trzy kroki:

1. Zadeklarować do aplikacji sieci web, że będzie do niej dołączania określonego wirtualnej sieci. Aplikacja wygeneruje certyfikat, który będzie przypisywany do wirtualnych sieci łączności punktu do witryny.
1. Przekazywanie certyfikatu aplikacji sieci web do wirtualnej sieci, a następnie pobrać pakiet VPN punktu do witryny identyfikatora URI.
1. Aktualizowanie połączenia wirtualnej sieci aplikacji sieci web z pakietem punktu do witryny identyfikatora URI.

Kroki pierwszym i trzecim są w pełni skryptowych, ale drugim krokiem wymaga jednorazową, ręcznego akcji za pośrednictwem portalu lub access umożliwiają wykonywanie akcji **umieszczenie** lub **Poprawka** na punkt końcowy Menedżera zasobów Azure wirtualnej sieci. Kontakt z pomocą techniczną Azure ma to włączone. Zanim zaczniesz, upewnij się, że jest sieć wirtualną klasyczny ma już włączone łączności punktu do witryny i wdrożonym bramę. Aby utworzyć bramy i łączność punktu do witryny, musisz korzystać z portalu, zgodnie z opisem w [Tworzenie bramy sieci VPN][createvpngateway].

Klasyczny wirtualną sieć musi znajdować się w tej samej subskrypcji jako usługi aplikacji usługi plan, który zawiera aplikację którego są Integracja z.

##### <a name="set-up-azure-powershell-sdk"></a>Konfigurowanie SDK programu PowerShell Azure #####

Otwórz okno programu PowerShell i skonfigurować konto Azure i subskrypcji usługi przy użyciu:

    Login-AzureRmAccount

Polecenie zostanie otwarty monit uzyskanie Azure poświadczenia. Po zalogowaniu się użyj jednej z następujących poleceń do Wybierz subskrypcję, do której chcesz użyć. Upewnij się, że korzystasz z subskrypcji, która wirtualnej sieci i plan usług aplikacji znajdują się w.

    Select-AzureRmSubscription –SubscriptionName [WebAppSubscriptionName]

lub

    Select-AzureRmSubscription –SubscriptionId [WebAppSubscriptionId]

##### <a name="variables-used-in-this-article"></a>Zmienne używane w tym artykule #####

Aby uprościć poleceń, możemy zostanie ustawiona zmiennej **$Configuration** programu PowerShell z określonej konfiguracji.

Ustaw zmienną następująco w programie PowerShell z poniższych parametrów:

    $Configuration = @{}
    $Configuration.WebAppResourceGroup = "[Your web app resource group]"
    $Configuration.WebAppName = "[Your web app name]"
    $Configuration.VnetSubscriptionId = "[Your vnet subscription id]"
    $Configuration.VnetResourceGroup = "[Your vnet resource group]"
    $Configuration.VnetName = "[Your vnet name]"

Lokalizacja aplikacji powinna być lokalizacji bez spacji. Na przykład USA Zachód jest westus.

    $Configuration.WebAppLocation = "[Your web app Location]"

Następny element to miejsce, w którym można zapisać certyfikatu. Należy zapisywalny dysk ścieżkę na komputerze lokalnym. Upewnij się uwzględnić cer na końcu.

    $Configuration.GeneratedCertificatePath = "[C:\Path\To\Certificate.cer]"

Aby wyświetlić, możesz ustawić, wpisz **$Configuration**.

    > $Configuration

    Name                           Value
    ----                           -----
    GeneratedCertificatePath       C:\vnetcert.cer
    VnetSubscriptionId             efc239a4-88f9-2c5e-a9a1-3034c21ad496
    WebAppResourceGroup            vnetdemo-rg
    VnetResourceGroup              testase1-rg
    VnetName                       TestNetwork
    WebAppName                     vnetintdemoapp
    WebAppLocation                 centralus

Pozostałą część tej sekcji założono, że zmienna utworzony w sposób opisany tylko.

##### <a name="declare-the-virtual-network-to-the-app"></a>Zadeklaruj wirtualną sieć do aplikacji #####

Użyj następującego polecenia aplikacji stwierdzić, że będzie korzystać z tego określonego wirtualnej sieci. Spowoduje to aplikację, aby wygenerować wymagane certyfikaty:

    $vnet = New-AzureRmResource -Name "$($Configuration.WebAppName)/$($Configuration.VnetName)" -ResourceGroupName $Configuration.WebAppResourceGroup -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -PropertyObject @{"VnetResourceId" = "/subscriptions/$($Configuration.VnetSubscriptionId)/resourceGroups/$($Configuration.VnetResourceGroup)/providers/Microsoft.ClassicNetwork/virtualNetworks/$($Configuration.VnetName)"} -Location $Configuration.WebAppLocation -ApiVersion 2015-07-01

Jeśli to polecenie zakończyło się powodzeniem, **$vnet** powinien mieć zmiennej **Właściwości** w nim. Zmienna **Właściwości** powinny zawierać zarówno odcisk palca certyfikatu i dane certyfikatu.

##### <a name="upload-the-web-app-certificate-to-the-virtual-network"></a>Przekazywanie certyfikatu aplikacji sieci web do wirtualnej sieci #####

Ręczne jednorazowego krokiem jest wymagane dla każdego połączenia wirtualnej sieci i subskrypcji. Oznacza to, że aplikacje subskrypcji A jest nawiązywane A wirtualnej sieci, potrzebujesz wykonać ten krok, tylko raz, niezależnie od tego, ile aplikacje można skonfigurować. Jeśli dodajesz nowej aplikacji do innej sieci wirtualnej, musisz ponownie wykonaj następujące czynności. Przyczyną jest zestaw certyfikatów jest generowany na poziomie subskrypcji w usłudze Azure aplikacji, oraz zestaw jest generowany raz dla każdej wirtualnej sieci, która połączy się z aplikacji.

Certyfikaty będą zostały już ustawione po wykonaniu tych czynności lub jeśli zintegrowany z tej samej sieci wirtualnych za pomocą portalu.

Pierwszym krokiem jest, aby wygenerować plik cer. Drugim krokiem jest, aby przekazać plik cer do wirtualnej sieci. Aby wygenerować plik cer z połączenia interfejsu API w poprzednim kroku, uruchom następujące polecenia.

    $certBytes = [System.Convert]::FromBase64String($vnet.Properties.certBlob)
    [System.IO.File]::WriteAllBytes("$($Configuration.GeneratedCertificatePath)", $certBytes)

Certyfikat będzie znajdować się w lokalizacji tego **$Configuration.GeneratedCertificatePath** określa.

Aby przekazać certyfikat ręcznie, za pomocą [Azure portal] [ azureportal] i **Przeglądanie wirtualnych sieci (klasyczny)** > **połączenia VPN** > **punktu do witryny** > **Zarządzanie certyfikaty**. W tym miejscu przekazywanie certyfikatu.

##### <a name="get-the-point-to-site-package"></a>Uzyskaj pakiet punktu do witryny #####

Następny krok konfigurowania połączenia wirtualnej sieci w aplikacji sieci web jest uzyskać pakiet punktu do witryny i przekazywanie ich do aplikacji sieci web.

Zapisywanie szablonu następujące w pliku o nazwie GetNetworkPackageUri.json gdzieś na komputerze, na przykład C:\Azure\Templates\GetNetworkPackageUri.json.

    {
        "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "certData": {
                "type": "string"
            },
            "certThumbprint": {
                "type": "string"
            },
            "networkName": {
                "type": "string"
            }
        },
        "variables": {
            "legacyVnetName": "[concat('Group ', resourceGroup().name, ' ', parameters('networkName'))]"
            },
            "resources": [
            ],
        "outputs" : {
            "PackageUri" :
            {
            "value" : "[listPackage(resourceId('Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRootCertificates', parameters('networkName'), 'primary', parameters('certThumbprint')), '2014-06-01').packageUri]", "type" : "string"
            }
        }
    }


Ustawianie parametrów wejściowych:

    $parameters = @{"certData" = $vnet.Properties.certBlob ;
    certThumbprint = $vnet.Properties.certThumbprint ;
    "networkName" = $Configuration.VnetName }

Wywołaj skrypt:

    $output = New-AzureRmResourceGroupDeployment -Name unused -ResourceGroupName $Configuration.VnetResourceGroup -TemplateParameterObject $parameters -TemplateFile C:\PATH\TO\GetNetworkPackageUri.json


Zmienna **$output. Outputs.packageUri** teraz będzie zawierać pakietu URI podawane do aplikacji sieci web.

##### <a name="upload-the-point-to-site-package-to-your-app"></a>Przekazywanie pakietu punktu do witryny do aplikacji #####

Ostatnim krokiem jest zapewnienie ten pakiet aplikacji. Wystarczy uruchomić następnego polecenia:

    $vnet = New-AzureRmResource -Name "$($Configuration.WebAppName)/$($Configuration.VnetName)/primary" -ResourceGroupName $Configuration.WebAppResourceGroup -ResourceType "Microsoft.Web/sites/virtualNetworkConnections/gateways" -ApiVersion 2015-07-01 -PropertyObject @{"VnetName" = $Configuration.VnetName ; "VpnPackageUri" = $($output.Outputs.packageUri).Value } -Location $Configuration.WebAppLocation

Jeśli wiadomość zostanie wyświetlony monit o potwierdzenie, że będą zastąpione istniejący zasób, upewnij się zezwolić na jego.

Po pomyślnym tego polecenia aplikacji powinna teraz połączone wirtualnej sieci. Aby potwierdzić sukcesu, przejdź do konsoli aplikacji i wpisz następujące polecenie:

    SET WEBSITE_

W przypadku zmiennej środowiska o nazwie WEBSITE_VNETNAME, którą ma wartość, która odpowiada nazwie wirtualnych sieci docelowej wszystkich konfiguracji zakończyło się pomyślnie.

### <a name="update-classic-vnet-integration-information"></a>Aktualizowanie informacji integracji VNet klasyczny ###

Aby zaktualizować lub resync informacji, po prostu Powtórz czynności, które zostały wykonane podczas tworzenia integracja w pierwszej kolejności. Te kroki są:

1. Definiowanie informacje o konfiguracji.
1. Zadeklaruj wirtualnych sieci do aplikacji.
1. Uzyskaj pakiet punktu do witryny.
1. Przekazywanie pakietu punktu do witryny do aplikacji.

### <a name="disconnect-your-app-from-a-classic-vnet"></a>Rozłączanie aplikacji klasycznej VNet ###

Aby odłączyć aplikacji, potrzebne informacje o konfiguracji ustawiany podczas integracji wirtualnej sieci. Za pomocą tych informacji, ma polecenia Aby rozłączyć aplikacji wirtualnej sieci.

    $vnet = Remove-AzureRmResource -Name "$($Configuration.WebAppName)/$($Configuration.VnetName)" -ResourceGroupName $Configuration.WebAppResourceGroup -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-07-01

## <a name="resource-manager-virtual-networks"></a>Menedżer zasobów wirtualnych sieci ##

Menedżer zasobów wirtualnych sieci mają Azure interfejsów API Menedżera zasobów, które uprościć niektóre procesy w porównaniu z klasyczny wirtualnych sieci. Mamy skrypt, który pomoże Ci wykonać następujące czynności:

- Tworzenie wirtualnej sieci Menedżer zasobów i integracja aplikacji z nim.
- Tworzenie bramy, konfigurowanie łączności punktu do witryny w sieci wirtualnej wcześniej istniejących Menedżera zasobów, a następnie zintegrować aplikacji z nim.
- Integracja aplikacji z wcześniej istniejących wirtualną sieć Menedżera zasobów, zawierającą bramę i włączone łączności punktu do witryny.
- Rozłączanie aplikacji z siecią wirtualną.

### <a name="resource-manager-vnet-app-service-integration-script"></a>Skrypt integracji usługi aplikacji VNet Menedżera zasobów ###

Skopiuj poniższy skrypt i zapisz go w pliku. Jeśli nie chcesz użyć skryptu, zachęcamy do wyciąganie wniosków z go, aby dowiedzieć się, jak skonfigurować elementy wirtualnej sieci Menedżer zasobów.


    function ReadHostWithDefault($message, $default)
    {
        $result = Read-Host "$message [$default]"
        if($result -eq "")
        {
            $result = $default
        }
            return $result
        }

    function PromptCustom($title, $optionValues, $optionDescriptions)
    {
        Write-Host $title
        Write-Host
        $a = @()
        for($i = 0; $i -lt $optionValues.Length; $i++){
            Write-Host "$($i+1))" $optionDescriptions[$i]
        }
        Write-Host

        while($true)
        {
            Write-Host "Choose an option: "
            $option = Read-Host
            $option = $option -as [int]

            if($option -ge 1 -and $option -le $optionValues.Length)
            {
                return $optionValues[$option-1]
            }
        }
    }

    function PromptYesNo($title, $message, $default = 0)
    {
        $yes = New-Object System.Management.Automation.Host.ChoiceDescription "&Yes", ""
        $no = New-Object System.Management.Automation.Host.ChoiceDescription "&No", ""
        $options = [System.Management.Automation.Host.ChoiceDescription[]]($yes, $no)
        $result = $host.ui.PromptForChoice($title, $message, $options, $default)
        return $result
    }

    function CreateVnet($resourceGroupName, $vnetName, $vnetAddressSpace, $vnetGatewayAddressSpace, $location)
    {
        Write-Host "Creating a new VNET"
        $gatewaySubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix $vnetGatewayAddressSpace
        New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $resourceGroupName -Location $location -AddressPrefix $vnetAddressSpace -Subnet $gatewaySubnet
    }

    function CreateVnetGateway($resourceGroupName, $vnetName, $vnetIpName, $location, $vnetIpConfigName, $vnetGatewayName, $certificateData, $vnetPointToSiteAddressSpace)
    {
        $vnet = Get-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $resourceGroupName
        $subnet=Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet

        Write-Host "Creating a public IP address for this VNET"
        $pip = New-AzureRmPublicIpAddress -Name $vnetIpName -ResourceGroupName $resourceGroupName -Location $location -AllocationMethod Dynamic
        $ipconf = New-AzureRmVirtualNetworkGatewayIpConfig -Name $vnetIpConfigName -Subnet $subnet -PublicIpAddress $pip

        Write-Host "Adding a root certificate to this VNET"
        $root = New-AzureRmVpnClientRootCertificate -Name "AppServiceCertificate.cer" -PublicCertData $certificateData

        Write-Host "Creating Azure VNET Gateway. This may take up to an hour."
        New-AzureRmVirtualNetworkGateway -Name $vnetGatewayName -ResourceGroupName $resourceGroupName -Location $location -IpConfigurations $ipconf -GatewayType Vpn -VpnType RouteBased -EnableBgp $false -GatewaySku Basic -VpnClientAddressPool $vnetPointToSiteAddressSpace -VpnClientRootCertificates $root
    }

    function AddNewVnet($subscriptionId, $webAppResourceGroup, $webAppName)
    {
        Write-Host "Adding a new Vnet"
        Write-Host
        $vnetName = Read-Host "Specify a name for this Virtual Network"

        $vnetGatewayName="$($vnetName)-gateway"
        $vnetIpName="$($vnetName)-ip"
        $vnetIpConfigName="$($vnetName)-ipconf"

        # Virtual Network settings
        $vnetAddressSpace="10.0.0.0/8"
        $vnetGatewayAddressSpace="10.5.0.0/16"
        $vnetPointToSiteAddressSpace="172.16.0.0/16"

        $changeRequested = 0
        $resourceGroupName = $webAppResourceGroup

        while($changeRequested -eq 0)
        {
            Write-Host
            Write-Host "Currently, I will create a VNET with the following settings:"
            Write-Host
            Write-Host "Virtual Network Name: $vnetName"
            Write-Host "Resource Group Name:  $resourceGroupName"
            Write-Host "Gateway Name: $vnetGatewayName"
            Write-Host "Vnet IP name: $vnetIpName"
            Write-Host "Vnet IP config name:  $vnetIpConfigName"
            Write-Host "Address Space:$vnetAddressSpace"
            Write-Host "Gateway Address Space:$vnetGatewayAddressSpace"
            Write-Host "Point-To-Site Address Space:  $vnetPointToSiteAddressSpace"
            Write-Host
            $changeRequested = PromptYesNo "" "Do you wish to change these settings?" 1

            if($changeRequested -eq 0)
            {
                $vnetName = ReadHostWithDefault "Virtual Network Name" $vnetName
                $resourceGroupName = ReadHostWithDefault "Resource Group Name" $resourceGroupName
                $vnetGatewayName = ReadHostWithDefault "Vnet Gateway Name" $vnetGatewayName
                $vnetIpName = ReadHostWithDefault "Vnet IP name" $vnetIpName
                $vnetIpConfigName = ReadHostWithDefault "Vnet IP configuration name" $vnetIpConfigName
                $vnetAddressSpace = ReadHostWithDefault "Vnet Address Space" $vnetAddressSpace
                $vnetGatewayAddressSpace = ReadHostWithDefault "Vnet Gateway Address Space" $vnetGatewayAddressSpace
                $vnetPointToSiteAddressSpace = ReadHostWithDefault "Vnet Point-to-site Address Space" $vnetPointToSiteAddressSpace
            }
        }

        $ErrorActionPreference = "Stop";

        # We create the virtual network and add it here. The way this works is:
        # 1) Add the VNET association to the App. This allows the App to generate certificates, etc. for the VNET.
        # 2) Create the VNET and VNET gateway, add the certificates, create the public IP, etc., required for the gateway
        # 3) Get the VPN package from the gateway and pass it back to the App.

        $webApp = Get-AzureRmResource -ResourceName $webAppName -ResourceType "Microsoft.Web/sites" -ApiVersion 2015-08-01 -ResourceGroupName $webAppResourceGroup
        $location = $webApp.Location

        Write-Host "Creating App association to VNET"
        $propertiesObject = @{
         "vnetResourceId" = "/subscriptions/$($subscriptionId)/resourceGroups/$($resourceGroupName)/providers/Microsoft.Network/virtualNetworks/$($vnetName)"
        }
        $virtualNetwork = New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnetName)" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-08-01 -ResourceGroupName $webAppResourceGroup -Force

        CreateVnet $resourceGroupName $vnetName $vnetAddressSpace $vnetGatewayAddressSpace $location

        CreateVnetGateway $resourceGroupName $vnetName $vnetIpName $location $vnetIpConfigName $vnetGatewayName $virtualNetwork.Properties.CertBlob $vnetPointToSiteAddressSpace

        Write-Host "Retrieving VPN Package and supplying to App"
        $packageUri = Get-AzureRmVpnClientPackage -ResourceGroupName $resourceGroupName -VirtualNetworkGatewayName $vnetGatewayName -ProcessorArchitecture Amd64

        # Put the VPN client configuration package onto the App
        $PropertiesObject = @{
        "vnetName" = $VirtualNetworkName; "vpnPackageUri" = $packageUri
        }

        New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnetName)/primary" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections/gateways" -ApiVersion 2015-08-01 -ResourceGroupName $webAppResourceGroup -Force

        Write-Host "Finished!"
    }

    function AddExistingVnet($subscriptionId, $resourceGroupName, $webAppName)
    {
        $ErrorActionPreference = "Stop";

        # At this point, the gateway should be able to be joined to an App, but may require some minor tweaking. We will declare to the App now to use this VNET
        Write-Host "Getting App information"
        $webApp = Get-AzureRmResource -ResourceName $webAppName -ResourceType "Microsoft.Web/sites" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName
        $location = $webApp.Location

        $webAppConfig = Get-AzureRmResource -ResourceName "$($webAppName)/web" -ResourceType "Microsoft.Web/sites/config" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName
        $currentVnet = $webAppConfig.Properties.VnetName
        if($currentVnet -ne $null -and $currentVnet -ne "")
        {
            Write-Host "Currently connected to VNET $currentVnet"
        }

        # Display existing vnets
        $vnets = Get-AzureRmVirtualNetwork
        $vnetNames = @()
        foreach($vnet in $vnets)
        {
            $vnetNames += $vnet.Name
        }

        Write-Host
        $vnet = PromptCustom "Select a VNET to integrate with" $vnets $vnetNames

        # We need to check if this VNET is able to be joined to a App, based on following criteria
            # If there is no gateway, we can create one.
            # If there is a gateway:
                # It must be of type Vpn
                # It must be of VpnType RouteBased
                # If it doesn't have the right certificate, we will need to add it.
                # If it doesn't have a point-to-site range, we will need to add it.

        $gatewaySubnet = $vnet.Subnets | Where-Object { $_.Name -eq "GatewaySubnet" }

        if($gatewaySubnet -eq $null -or $gatewaySubnet.IpConfigurations -eq $null -or $gatewaySubnet.IpConfigurations.Count -eq 0)
        {
            $ErrorActionPreference = "Continue";
            # There is no gateway. We need to create one.
            Write-Host "This Virtual Network has no gateway. I will need to create one."

            $vnetName = $vnet.Name
            $vnetGatewayName="$($vnetName)-gateway"
            $vnetIpName="$($vnetName)-ip"
            $vnetIpConfigName="$($vnetName)-ipconf"

            # Virtual Network settings
            $vnetAddressSpace="10.0.0.0/8"
            $vnetGatewayAddressSpace="10.5.0.0/16"
            $vnetPointToSiteAddressSpace="172.16.0.0/16"

            $changeRequested = 0

            Write-Host "Your VNET is in the address space $($vnet.AddressSpace.AddressPrefixes), with the following Subnets:"
            foreach($subnet in $vnet.Subnets)
            {
                Write-Host "$($subnet.Name): $($subnet.AddressPrefix)"
            }

            $vnetGatewayAddressSpace = Read-Host "Please choose a GatewaySubnet address space"

            while($changeRequested -eq 0)
            {
                Write-Host
                Write-Host "Currently, I will create a VNET gateway with the following settings:"
                Write-Host
                Write-Host "Virtual Network Name: $vnetName"
                Write-Host "Resource Group Name:  $($vnet.ResourceGroupName)"
                Write-Host "Gateway Name: $vnetGatewayName"
                Write-Host "Vnet IP name: $vnetIpName"
                Write-Host "Vnet IP config name:  $vnetIpConfigName"
                Write-Host "Address Space:$($vnet.AddressSpace.AddressPrefixes)"
                Write-Host "Gateway Address Space:$vnetGatewayAddressSpace"
                Write-Host "Point-To-Site Address Space:  $vnetPointToSiteAddressSpace"
                Write-Host
                $changeRequested = PromptYesNo "" "Do you wish to change these settings?" 1

                if($changeRequested -eq 0)
                {
                    $vnetGatewayName = ReadHostWithDefault "Vnet Gateway Name" $vnetGatewayName
                    $vnetIpName = ReadHostWithDefault "Vnet IP name" $vnetIpName
                    $vnetIpConfigName = ReadHostWithDefault "Vnet IP configuration name" $vnetIpConfigName
                    $vnetGatewayAddressSpace = ReadHostWithDefault "Vnet Gateway Address Space" $vnetGatewayAddressSpace
                    $vnetPointToSiteAddressSpace = ReadHostWithDefault "Vnet Point-to-site Address Space" $vnetPointToSiteAddressSpace
                }
            }

            $ErrorActionPreference = "Stop";

            Write-Host "Creating App association to VNET"
            $propertiesObject = @{
             "vnetResourceId" = "/subscriptions/$($subscriptionId)/resourceGroups/$($vnet.ResourceGroupName)/providers/Microsoft.Network/virtualNetworks/$($vnetName)"
            }

            $virtualNetwork = New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnet.Name)" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName -Force

            # If there is no gateway subnet, we need to create one.
            if($gatewaySubnet -eq $null)
            {
                $gatewaySubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix $vnetGatewayAddressSpace
                $vnet.Subnets.Add($gatewaySubnet);
                Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
            }

            CreateVnetGateway $vnet.ResourceGroupName $vnetName $vnetIpName $location $vnetIpConfigName $vnetGatewayName $virtualNetwork.Properties.CertBlob $vnetPointToSiteAddressSpace

            $gateway = Get-AzureRmVirtualNetworkGateway -ResourceGroupName $vnet.ResourceGroupName -Name $vnetGatewayName
        }
        else
        {
            $uriParts = $gatewaySubnet.IpConfigurations[0].Id.Split('/')
            $gatewayResourceGroup = $uriParts[4]
            $gatewayName = $uriParts[8]

            $gateway = Get-AzureRmVirtualNetworkGateway -ResourceGroupName $vnet.ResourceGroupName -Name $gatewayName

            # validate gateway types, etc.
            if($gateway.GatewayType -ne "Vpn")
            {
                Write-Error "This gateway is not of the Vpn type. It cannot be joined to an App."
                return
            }

            if($gateway.VpnType -ne "RouteBased")
            {
                Write-Error "This gateways Vpn type is not RouteBased. It cannot be joined to an App."
                return
            }

            if($gateway.VpnClientConfiguration -eq $null -or $gateway.VpnClientConfiguration.VpnClientAddressPool -eq $null)
            {
                Write-Host "This gateway does not have a Point-to-site Address Range. Please specify one in CIDR notation, e.g. 10.0.0.0/8"
                $pointToSiteAddress = Read-Host "Point-To-Site Address Space"
                Set-AzureRmVirtualNetworkGatewayVpnClientConfig -VirtualNetworkGateway $gateway.Name -VpnClientAddressPool $pointToSiteAddress
            }

            Write-Host "Creating App association to VNET"
            $propertiesObject = @{
             "vnetResourceId" = "/subscriptions/$($subscriptionId)/resourceGroups/$($vnet.ResourceGroupName)/providers/Microsoft.Network/virtualNetworks/$($vnet.Name)"
            }

            $virtualNetwork = New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnet.Name)" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName -Force

            # We need to check if the certificate here exists in the gateway.
            $certificates = $gateway.VpnClientConfiguration.VpnClientRootCertificates

            $certFound = $false
            foreach($certificate in $certificates)
            {
                if($certificate.PublicCertData -eq $virtualNetwork.Properties.CertBlob)
                {
                    $certFound = $true
                    break
                }
            }

            if(-not $certFound)
            {
                Write-Host "Adding certificate"
                Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName "AppServiceCertificate.cer" -PublicCertData $virtualNetwork.Properties.CertBlob -VirtualNetworkGatewayName $gateway.Name
            }
        }

        # Now finish joining by getting the VPN package and giving it to the App
        Write-Host "Retrieving VPN Package and supplying to App"
        $packageUri = Get-AzureRmVpnClientPackage -ResourceGroupName $vnet.ResourceGroupName -VirtualNetworkGatewayName $gateway.Name -ProcessorArchitecture Amd64

        # Put the VPN client configuration package onto the App
        $PropertiesObject = @{
        "vnetName" = $vnet.Name; "vpnPackageUri" = $packageUri
        }

        New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnet.Name)/primary" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections/gateways" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName -Force

        Write-Host "Finished!"
    }

    function RemoveVnet($subscriptionId, $resourceGroupName, $webAppName)
    {
        $webAppConfig = Get-AzureRmResource -ResourceName "$($webAppName)/web" -ResourceType "Microsoft.Web/sites/config" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName
        $currentVnet = $webAppConfig.Properties.VnetName
        if($currentVnet -ne $null -and $currentVnet -ne "")
        {
            Write-Host "Currently connected to VNET $currentVnet"

            Remove-AzureRmResource -ResourceName "$($webAppName)/$($currentVnet)" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName
        }
          else
        {
          Write-Host "Not connected to a VNET."
        }
    }

    Write-Host "Please Login"
    Login-AzureRmAccount

    # Choose subscription. If there's only one we will choose automatically

    $subs = Get-AzureRmSubscription
    $subscriptionId = ""

    if($subs.Length -eq 0)
    {
        Write-Error "No subscriptions bound to this account."
        return
    }

    if($subs.Length -eq 1)
    {
        $subscriptionId = $subs[0].SubscriptionId
    }
    else
    {
        $subscriptionChoices = @()
        $subscriptionValues = @()

        foreach($subscription in $subs){
        $subscriptionChoices += "$($subscription.SubscriptionName) ($($subscription.SubscriptionId))";
        $subscriptionValues += ($subscription.SubscriptionId);
        }

        $subscriptionId = PromptCustom "Choose a subscription" $subscriptionValues $subscriptionChoices
    }

    Select-AzureRmSubscription -SubscriptionId $subscriptionId

    $resourceGroup = Read-Host "Please enter the Resource Group of your App"

    $appName = Read-Host "Please enter the Name of your App"

    $options = @("Add a NEW Virtual Network to an App", "Add an EXISTING Virtual Network to an App", "Remove a Virtual Network from an App");
    $optionValues = @(0, 1, 2)
    $option = PromptCustom "What do you want to do?" $optionValues $options

    if($option -eq 0)
    {
        AddNewVnet $subscriptionId $resourceGroup $appName
    }
    if($option -eq 1)
    {
        AddExistingVnet $subscriptionId $resourceGroup $appName
    }
    if($option -eq 2)
    {
        RemoveVnet $subscriptionId $resourceGroup $appName
    }

Zapisz kopię skrypt. W tym artykule nazywany V2VnetAllinOne.ps1, ale można użyć innej nazwy. Nie ma żadnych argumentów dla tego skryptu. Po prostu uruchomić go. Najpierw, które będzie wykonywać skrypt jest monit do zalogowania się. Po zalogowaniu się skrypt otrzymuje szczegółowe informacje o koncie i zwraca listę subskrypcji. Nie licząc wniosek o podanie poświadczeń, wykonywanie skryptu początkowe wygląda następująco:

    PS C:\Users\ccompy\Documents\VNET> .\V2VnetAllInOne.ps1
    Please Login

    Environment           : AzureCloud
    Account               : ccompy@microsoft.com
    TenantId              : 722278f-fef1-499f-91ab-2323d011db47
    SubscriptionId        : af5358e1-acac-2c90-a9eb-722190abf47a
    CurrentStorageAccount :

    Choose a subscription

    1) Pokaz subskrypcji (af5358e1-acac-2c90-a9eb-722190abf47a)
    2) Test MS (a5350f55-dd5a-41ec-2ddb-ff7b911bb2ef)
    3) Fioletowe pokaz subskrypcji (2d4c99a4-57f9-4d5e-a0a1-0034c52db59d)

    Wybierz jedną z opcji: 3

    Konto: ccompy@microsoft.com środowiska: subskrypcji AzureCloud: 2d4c99a4-57f9-4d5e-a0a1-0034c52db59d dzierżawy: 722278f-fef1-499f-91ab-2323d011db47

    Wprowadź Grupa zasobów aplikacji: hcdemo rg wprowadź nazwę aplikacji: v2vnetpowershell co chcesz zrobić?

    1) Dodawanie nowego wirtualnej sieci do aplikacji
    2) Dodawanie ISTNIEJĄCEGO wirtualną sieć do aplikacji
    3) Usuwanie wirtualnej sieci z aplikacji

Pozostałe w tej sekcji wyjaśniono, każdy z tych trzech opcji.

### <a name="create-a-resource-manager-vnet-and-integrate-with-it"></a>Tworzenie VNet Menedżera zasobów i integracja z nim ###

Aby utworzyć nowy wirtualnej sieci korzystającego z modelu wdrożenia Menedżera zasobów i zintegrować go z aplikacji, wybierz pozycję **1) Dodawanie nowego wirtualnej sieci do aplikacji**. To wyświetli monit o nazwę wirtualnej sieci. W moim przypadku jak widać w następujących ustawień używany nazwę v2pshell.

Skrypt zawiera szczegółowe informacje dotyczące wirtualnej sieci, który jest tworzony. Jeśli nie chcę, można zmienić wartości. W tym wykonanie przykład utworzona wirtualnej sieci, który ma następujące ustawienia:

    Virtual Network Name:         v2pshell
    Resource Group Name:          hcdemo-rg
    Gateway Name:                 v2pshell-gateway
    Vnet IP name:                 v2pshell-ip
    Vnet IP config name:          v2pshell-ipconf
    Address Space:                10.0.0.0/8
    Gateway Address Space:        10.5.0.0/16
    Point-To-Site Address Space:  172.16.0.0/12

    Do you wish to change these settings?
    [Y] Yes  [N] No  [?] Help (default is "N"):

Jeśli chcesz zmienić żadnych wartości, wpisz **Y** i wprowadź odpowiednie zmiany. Po zakończeniu edycji z ustawieniami wirtualnej sieci, wpisz **N** lub po prostu naciśnij klawisz Enter po wyświetleniu monitu o zmienianiu ustawień. Stamtąd na do momentu zakończenia skrypt informuje o niektórych jakie it "być wykonanie, dopóki nie rozpocznie się go utworzyć bramy wirtualnej sieci. Ten krok może trwać do godziny. Nie ma żadnych wskaźnik postępu podczas tej fazy, ale skrypt zostanie wyświetlona informacja, kiedy utworzono bramy.

Po zakończeniu działania skrypt będzie w nim wyświetlany **zakończone**. W tym momencie należy wirtualną sieć Menedżera zasobów i ma nazwę i wybrane ustawienia. Ten nowy wirtualną sieć zostanie również zintegrowany z aplikacji.

### <a name="integrate-your-app-with-a-preexisting-resource-manager-vnet"></a>Integracja aplikacji z wcześniej istniejących VNet Menedżera zasobów ###

Gdy jest integracji z siecią wirtualną wcześniej istniejących, jeśli podasz wirtualną sieć Menedżera zasobów i nie ma bramy lub łączności punktu do witryny, skrypt skonfiguruje który. Jeśli VNET już te czynności Konfigurowanie, skrypt pozwala przejść bezpośrednio do integracji aplikacji. Aby rozpocząć ten proces, po prostu wybierz **2) Dodawanie istniejącej sieci wirtualnych do aplikacji**.

Ta opcja działa tylko wtedy, gdy wcześniej istniejących wirtualną sieć Menedżera zasobów, który znajduje się w tej samej subskrypcji jako aplikacji. Po wybraniu opcji zostanie wyświetlona lista wirtualnych sieci swojego menedżera zasobów.   

    Select a VNET to integrate with

    1) v2demonetwork
    2) v2pshell
    3) v2vnetintdemo
    4) v2asenetwork
    5) v2pshell2

    Wybierz jedną z opcji: 5

Po prostu wybierz wirtualnej sieci, którą chcesz zintegrować z. Jeśli masz już bramy, która ma połączenie punkt do witryny włączone, skrypt po prostu integracja aplikacji z siecią wirtualną. Jeśli nie masz bramy, musisz określić podsieć bramy. Danej podsieci bramy musi być w obszarze adres wirtualnej sieci, i nie może być w innej podsieci. Jeśli masz wirtualną sieć bez bramy i uruchomić ten krok, co wyglądać podobnie do następującej:

    This Virtual Network has no gateway. I will need to create one.
    Your VNET is in the address space 172.16.0.0/16, with the following Subnets:
    default: 172.16.0.0/24
    Please choose a GatewaySubnet address space: 172.16.1.0/26

W tym przykładzie utworzona bramy wirtualnej sieci, która ma następujące ustawienia:

    Virtual Network Name:         v2pshell2
    Resource Group Name:          vnetdemo-rg
    Gateway Name:                 v2pshell2-gateway
    Vnet IP name:                 v2pshell2-ip
    Vnet IP config name:          v2pshell2-ipconf
    Address Space:                172.16.0.0/16
    Gateway Address Space:        172.16.1.0/26
    Point-To-Site Address Space:  172.16.0.0/12

    Do you wish to change these settings?
    [Y] Yes  [N] No  [?] Help (default is "N"):
    Creating App association to VNET

Jeśli chcesz zmienić dowolne z tych ustawień, możesz to zrobić. W przeciwnym razie naciśnij klawisz Enter, a skrypt utworzy Centrum i dołączanie aplikacji do wirtualnej sieci. Podczas tworzenia bramy jest nadal godziny, mimo że, dlatego upewnij się, że możesz pamiętać, że. Gdy wszystko jest gotowe, skrypt zostanie wyświetlony komunikat **zakończony**.

### <a name="disconnect-your-app-from-a-resource-manager-vnet"></a>Rozłączanie aplikacji VNet Menedżera zasobów ###

Odłączanie aplikacji wirtualnej sieci nie notowanie bramy lub wyłączanie łączności punktu do witryny. Być może używasz go do czegoś innego. Również nie odłącza go z innych aplikacji, poza podaną z nich. Aby wykonać tę akcję, zaznacz **3) usunąć wirtualnej sieci z aplikacji**. Po wykonaniu tej czynności pojawią się mniej więcej tak:

    Currently connected to VNET v2pshell

    Confirm
    Are you sure you want to delete the following resource:
    /subscriptions/edcc99a4-b7f9-4b5e-a9a1-3034c51db496/resourceGroups/hcdemo-rg/providers/Microsoft.Web/sites/v2vnetpowers
    hell/virtualNetworkConnections/v2pshell
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):

Mimo że skrypt mówi delete, wirtualną sieć nie powoduje usunięcia. Po prostu je usuwa integracja. Po upewnij się, że jest to, co chcesz zrobić, to polecenie jest przetwarzana bardzo szybko i pozwalają **PRAWDA** po jego zakończeniu.

<!--Links-->
[createvpngateway]: http://azure.microsoft.com/documentation/articles/vpn-gateway-point-to-site-create/
[azureportal]: http://portal.azure.com
