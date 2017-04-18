<properties
    pageTitle="Zarządzanie Azure CDN przy użyciu programu PowerShell | Microsoft Azure"
    description="Dowiedz się, jak zarządzać Azure CDN za pomocą polecenia cmdlet programu PowerShell Azure."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/17/2016"
    ms.author="casoper"/>


# <a name="manage-azure-cdn-with-powershell"></a>Zarządzanie Azure CDN przy użyciu programu PowerShell

PowerShell zawiera jedną z metod najbardziej elastyczna do zarządzania profilami Azure CDN i punkty końcowe.  Można użyć programu PowerShell interakcyjnie lub pisząc skrypty do automatyzowania zadań związanych z zarządzaniem.  Ten samouczek zaprezentowano kilka typowych zadań można wykonać za pomocą programu PowerShell do zarządzania profilami Azure CDN i punkty końcowe.

## <a name="prerequisites"></a>Wymagania wstępne

Aby użyć programu PowerShell do zarządzania profilami Azure CDN i punkty końcowe, musi mieć zainstalowany moduł Azure programu PowerShell.  Aby dowiedzieć się, jak zainstalować Azure programu PowerShell i nawiązać połączenie przy użyciu Azure `Login-AzureRmAccount` polecenia cmdlet, zobacz, [jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md).

>[AZURE.IMPORTANT] Zaloguj się przy użyciu `Login-AzureRmAccount` przed wykonaniem poleceń cmdlet środowiska PowerShell Azure.

## <a name="listing-the-azure-cdn-cmdlets"></a>Wyświetlanie poleceń cmdlet Azure CDN

Można wyświetlić listę wszystkich poleceń cmdlet Azure CDN za pomocą `Get-Command` polecenia cmdlet.

```text
PS C:\> Get-Command -Module AzureRM.Cdn

CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Cmdlet          Get-AzureRmCdnCustomDomain                         2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnEndpoint                             2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnEndpointNameAvailability             2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnOrigin                               2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnProfile                              2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnProfileSsoUrl                        2.0.0      AzureRm.Cdn
Cmdlet          New-AzureRmCdnCustomDomain                         2.0.0      AzureRm.Cdn
Cmdlet          New-AzureRmCdnEndpoint                             2.0.0      AzureRm.Cdn
Cmdlet          New-AzureRmCdnProfile                              2.0.0      AzureRm.Cdn
Cmdlet          Publish-AzureRmCdnEndpointContent                  2.0.0      AzureRm.Cdn
Cmdlet          Remove-AzureRmCdnCustomDomain                      2.0.0      AzureRm.Cdn
Cmdlet          Remove-AzureRmCdnEndpoint                          2.0.0      AzureRm.Cdn
Cmdlet          Remove-AzureRmCdnProfile                           2.0.0      AzureRm.Cdn
Cmdlet          Set-AzureRmCdnEndpoint                             2.0.0      AzureRm.Cdn
Cmdlet          Set-AzureRmCdnOrigin                               2.0.0      AzureRm.Cdn
Cmdlet          Set-AzureRmCdnProfile                              2.0.0      AzureRm.Cdn
Cmdlet          Start-AzureRmCdnEndpoint                           2.0.0      AzureRm.Cdn
Cmdlet          Stop-AzureRmCdnEndpoint                            2.0.0      AzureRm.Cdn
Cmdlet          Test-AzureRmCdnCustomDomain                        2.0.0      AzureRm.Cdn
Cmdlet          Unpublish-AzureRmCdnEndpointContent                2.0.0      AzureRm.Cdn
```

## <a name="getting-help"></a>Uzyskiwanie pomocy

Możesz ją uzyskać z tych poleceń cmdlet przy użyciu `Get-Help` polecenia cmdlet.  `Get-Help`zawiera użycie i składnię i opcjonalnie pokazano przykłady.

```text
PS C:\> Get-Help Get-AzureRmCdnProfile

NAME
    Get-AzureRmCdnProfile

SYNOPSIS
    Gets an Azure CDN profile.


SYNTAX
    Get-AzureRmCdnProfile [-ProfileName <String>] [-ResourceGroupName <String>] [-InformationAction
    <ActionPreference>] [-InformationVariable <String>] [<CommonParameters>]


DESCRIPTION
    Gets an Azure CDN profile and all related information.


RELATED LINKS

REMARKS
    To see the examples, type: "get-help Get-AzureRmCdnProfile -examples".
    For more information, type: "get-help Get-AzureRmCdnProfile -detailed".
    For technical information, type: "get-help Get-AzureRmCdnProfile -full".

```

## <a name="listing-existing-azure-cdn-profiles"></a>Lista istniejących profilów Azure CDN

`Get-AzureRmCdnProfile` Polecenia cmdlet bez parametrów pobiera wszystkie istniejące profile CDN.

```powershell
Get-AzureRmCdnProfile
```

Te dane wyjściowe można potoku do polecenia cmdlet wyliczania.

```powershell
# Output the name of all profiles on this subscription.
Get-AzureRmCdnProfile | ForEach-Object { Write-Host $_.Name }

# Return only **Azure CDN from Verizon** profiles.
Get-AzureRmCdnProfile | Where-Object { $_.Sku.Name -eq "StandardVerizon" }
```

Można także zwracać jednego profilu, określając grupy zasobami i nazwę profilu.

```powershell
Get-AzureRmCdnProfile -ProfileName CdnDemo -ResourceGroupName CdnDemoRG
```

>[AZURE.TIP] Istnieje możliwość ma wiele profili CDN o takiej samej nazwie, ile znajdują się w różnych grup zasobów.  Pominięcie `ResourceGroupName` parametr zwraca wszystkie profile z odpowiednia nazwa.

## <a name="listing-existing-cdn-endpoints"></a>Punkty końcowe CDN istniejącej listy

`Get-AzureRmCdnEndpoint`można pobrać punktu końcowego poszczególnych lub wszystkich punktów końcowych w profilu.  

```powershell
# Get a single endpoint.
Get-AzureRmCdnEndpoint -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo

# Get all of the endpoints on a given profile. 
Get-AzureRmCdnEndpoint -ProfileName CdnDemo -ResourceGroupName CdnDemoRG

# Return all of the endpoints on all of the profiles.
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint

# Return all of the endpoints in this subscription that are currently running.
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint | Where-Object { $_.ResourceState -eq "Running" }
```

## <a name="creating-cdn-profiles-and-endpoints"></a>Tworzenie profilów CDN i punkty końcowe

`New-AzureRmCdnProfile`i `New-AzureRmCdnEndpoint` są używane do tworzenia profilów CDN i punkty końcowe.

```powershell
# Create a new profile
New-AzureRmCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -Sku StandardAkamai -Location "Central US"

# Create a new endpoint
New-AzureRmCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -Location "Central US" -EndpointName cdnposhdoc -OriginName "Contoso" -OriginHostName "www.contoso.com"

# Create a new profile and endpoint (same as above) in one line
New-AzureRmCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -Sku StandardAkamai -Location "Central US" | New-AzureRmCdnEndpoint -EndpointName cdnposhdoc -OriginName "Contoso" -OriginHostName "www.contoso.com"

```

## <a name="checking-endpoint-name-availability"></a>Sprawdzanie dostępności Nazwa punktu końcowego

`Get-AzureRmCdnEndpointNameAvailability`Zwraca obiekt informujący, jeśli nazwa punktu końcowego jest dostępna.

```powershell
# Retrieve availability
$availability = Get-AzureRmCdnEndpointNameAvailability -EndpointName "cdnposhdoc"

# If available, write a message to the console.
If($availability.NameAvailable) { Write-Host "Yes, that endpoint name is available." }
Else { Write-Host "No, that endpoint name is not available." }
```

## <a name="adding-a-custom-domain"></a>Dodawanie domeny niestandardowej

`New-AzureRmCdnCustomDomain`powoduje dodanie niestandardowej nazwy domeny do istniejący punkt końcowy.

>[AZURE.IMPORTANT] Musisz skonfigurować CNAME u dostawcy DNS zgodnie z opisem w [sposób mapowania domeny niestandardowe do punktu końcowego sieci dostarczania zawartości (CDN)](./cdn-map-content-to-custom-domain.md).  Istnieje możliwość przetestowania mapowania przed modyfikowaniem przy użyciu punktu końcowego `Test-AzureRmCdnCustomDomain`.

```powershell
# Get an existing endpoint
$endpoint = Get-AzureRmCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -EndpointName cdnposhdoc

# Check the mapping
$result = Test-AzureRmCdnCustomDomain -CdnEndpoint $endpoint -CustomDomainHostName "cdn.contoso.com"

# Create the custom domain on the endpoint
If($result.CustomDomainValidated){ New-AzureRmCdnCustomDomain -CustomDomainName Contoso -HostName "cdn.contoso.com" -CdnEndpoint $endpoint }
```

## <a name="modifying-an-endpoint"></a>Modyfikowanie punktu końcowego

`Set-AzureRmCdnEndpoint`Modyfikuje istniejący punkt końcowy.

```powershell
# Get an existing endpoint
$endpoint = Get-AzureRmCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -EndpointName cdnposhdoc

# Set up content compression
$endpoint.IsCompressionEnabled = $true
$endpoint.ContentTypesToCompress = "text/javascript","text/css","application/json"

# Save the changed endpoint and apply the changes
Set-AzureRmCdnEndpoint -CdnEndpoint $endpoint
```

## <a name="purgingpre-loading-cdn-assets"></a>Czyszczenie i próbnej-loading CDN składników majątku

`Unpublish-AzureRmCdnEndpointContent`powoduje usunięcie przechowywanych w pamięci podręcznej składników majątku podczas `Publish-AzureRmCdnEndpointContent` wstępnie ładuje zasoby na obsługiwane punkty końcowe.

```powershell
# Purge some assets.
Unpublish-AzureRmCdnEndpointContent -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo -PurgeContent "/images/kitten.png","/video/rickroll.mp4"

# Pre-load some assets.
Publish-AzureRmCdnEndpointContent -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo -LoadContent "/images/kitten.png","/video/rickroll.mp4"

# Purge everything in /images/ on all endpoints.
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint | Unpublish-AzureRmCdnEndpointContent -PurgeContent "/images/*"
```

## <a name="startingstopping-cdn-endpoints"></a>Uruchamianie i zatrzymywanie CDN punkty końcowe
`Start-AzureRmCdnEndpoint`i `Stop-AzureRmCdnEndpoint` można uruchamiać i zatrzymywać poszczególnych punktów końcowych lub grup punktów końcowych.

```powershell
# Stop the cdndocdemo endpoint
Stop-AzureRmCdnEndpoint -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo

# Stop all endpoints
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint | Stop-AzureRmCdnEndpoint

# Start all endpoints
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint | Start-AzureRmCdnEndpoint
```

## <a name="deleting-cdn-resources"></a>Usuwanie CDN zasobów

`Remove-AzureRmCdnProfile`i `Remove-AzureRmCdnEndpoint` można używać do usuwania profilów i punkty końcowe.

```powershell
# Remove a single endpoint
Remove-AzureRmCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -EndpointName cdnposhdoc

# Remove all the endpoints on a profile and skip confirmation (-Force)
Get-AzureRmCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG | Get-AzureRmCdnEndpoint | Remove-AzureRmCdnEndpoint -Force

# Remove a single profile
Remove-AzureRmCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG
```

## <a name="next-steps"></a>Następne kroki

Dowiedz się, jak zautomatyzować CDN Azure za pomocą [.NET](./cdn-app-dev-net.md) lub [Node.js](./cdn-app-dev-node.md).

Aby uzyskać informacje o funkcjach sieci CDN, zobacz [Omówienie CDN](./cdn-overview.md).


