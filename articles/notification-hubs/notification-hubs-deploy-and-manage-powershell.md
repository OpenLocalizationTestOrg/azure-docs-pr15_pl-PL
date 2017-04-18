<properties 
    pageTitle="Wdrażanie i zarządzanie koncentratory powiadomień przy użyciu programu PowerShell" 
    description="Jak tworzyć i zarządzać nimi przy użyciu programu PowerShell automatyzacji koncentratory powiadomień" 
    services="notification-hubs" 
    documentationCenter="" 
    authors="ysxu" 
    manager="erikre" 
    editor="" />

<tags 
    ms.service="notification-hubs" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="powershell" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="06/29/2016" 
    ms.author="yuaxu"/>

# <a name="deploy-and-manage-notification-hubs-using-powershell"></a>Wdrażanie i zarządzanie koncentratory powiadomień przy użyciu programu PowerShell

##<a name="overview"></a>Omówienie

W tym artykule pokazano, jak używać Utwórz i Zarządzaj Azure powiadomienie koncentratory przy użyciu programu PowerShell. W tym temacie są wyświetlane następujące typowe zadania automatyzacji.

+ Tworzenie Centrum powiadomień
+ Ustawianie poświadczeń

Jeśli trzeba również utworzyć nowe nazw bus usług dla usługi koncentratorów powiadomień, zobacz [Zarządzanie Bus usługi przy użyciu programu PowerShell](../service-bus-messaging/service-bus-powershell-how-to-provision.md).

Zarządzanie koncentratory powiadomienia nie jest obsługiwana przez cmdlet dostępnych w programie Azure PowerShell. Z programu PowerShell najlepiej zostać utworzone odwołanie zestawu Microsoft.Azure.NotificationHubs.dll. Zestaw jest rozkładana z [pakietu Microsoft Azure powiadomienie koncentratory NuGet](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).


## <a name="prerequisites"></a>Wymagania wstępne

Przed rozpoczęciem tego artykułu, musisz mieć następujące czynności:

- Subskrypcję usługi Azure. Azure jest platformą subskrybowanej. Aby uzyskać więcej informacji na temat uzyskiwania subskrypcji zobacz [Opcje zakupu], [Oferuje członka]lub [Bezpłatnej wersji próbnej].

- Komputer z programem PowerShell Azure. Aby uzyskać instrukcje, zobacz [Instalowanie i konfigurowanie programu PowerShell Azure].

- Ogólne zrozumienie skryptów programu PowerShell, NuGet pakietów i .NET Framework.


## <a name="including-a-reference-to-the-net-assembly-for-service-bus"></a>W tym odwołanie do zestawu .NET dla usługi Bus

Zarządzanie koncentratory powiadomienie Azure nie jest jeszcze dołączone do poleceń cmdlet środowiska PowerShell w programie PowerShell Azure. Inicjowanie obsługi koncentratory powiadomienie, umożliwia klienta programu .NET opisane w tym [pakietu Microsoft Azure powiadomienie koncentratory NuGet](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).

Najpierw upewnij się, że skrypt można znaleźć zestawu **Microsoft.Azure.NotificationHubs.dll** , która jest instalowana jako pakiet NuGet w projekcie programu Visual Studio. W celu elastyczne, skrypt wykonuje następujące czynności:

1. Określa ścieżkę, w którym został wywołany.
2. Przechodzi ścieżkę, aż do znalezienia folder o nazwie `packages`. Zainstalować pakiety NuGet projekty Visual Studio powoduje utworzenie tego folderu.
3. Wyszukiwanie lokalizacji `packages` folder dla zestawu o nazwie **Microsoft.Azure.NotificationHubs.dll**.
4. Odwołania zestawu, dzięki czemu typy są dostępne w celu późniejszego użycia.

Oto implementacji poniższe czynności w obszarze skrypt programu PowerShell:

``` powershell

try
{
    # WARNING: Make sure to reference the latest version of Microsoft.Azure.NotificationHubs.dll
    Write-Output "Adding the [Microsoft.Azure.NotificationHubs.dll] assembly to the script..."
    $scriptPath = Split-Path (Get-Variable MyInvocation -Scope 0).Value.MyCommand.Path
    $packagesFolder = (Split-Path $scriptPath -Parent) + "\packages"
    $assembly = Get-ChildItem $packagesFolder -Include "Microsoft.Azure.NotificationHubs.dll" -Recurse
    Add-Type -Path $assembly.FullName

    Write-Output "The [Microsoft.Azure.NotificationHubs.dll] assembly has been successfully added to the script."
}

catch [System.Exception]
{
    Write-Error("Could not add the Microsoft.Azure.NotificationHubs.dll assembly to the script. Make sure you build the solution before running the provisioning script.")
}
```

## <a name="create-the-namespacemanager-class"></a>Tworzenie klasy NamespaceManager

Do zapewniania obsługi koncentratory powiadomienie, należy utworzyć wystąpienie klasy [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.azure.notificationhubs.namespacemanager.aspx) z zestawu SDK. 

Za pomocą polecenia cmdlet [Get-AzureSBAuthorizationRule] dostępnych w programie Azure PowerShell pobrać reguła autoryzacji, która jest używana do dostarczania parametry połączenia. Będą przechowywane odwołanie do `NamespaceManager` wystąpienia w `$NamespaceManager` zmiennej. Użyjemy `$NamespaceManager` do zapewniania obsługi koncentratora powiadomienie.

``` powershell
$sbr = Get-AzureSBAuthorizationRule -Namespace $Namespace
# Create the NamespaceManager object to create the hub
Write-Output "Creating a NamespaceManager object for the [$Namespace] namespace..."
$NamespaceManager=[Microsoft.Azure.NotificationHubs.NamespaceManager]::CreateFromConnectionString($sbr.ConnectionString);
Write-Output "NamespaceManager object for the [$Namespace] namespace has been successfully created."
```


## <a name="provisioning-a-new-notification-hub"></a>Nowe Centrum powiadomienie inicjowania obsługi administracyjnej 

Do zapewniania obsługi nowego koncentratora powiadomienie, za pomocą [Interfejsu API programu .NET dla koncentratorów powiadomienie].

W tej części skrypt możesz skonfigurować cztery zmienne lokalne. 

1. `$Namespace`: Ustaw ten nazwę obszaru nazw, miejsce, w którym chcesz utworzyć Centrum powiadomienie.
2. `$Path`: Ustaw następującą ścieżkę do nazwy nowego koncentratora powiadomienie.  Na przykład "MyHub".    
3. `$WnsPackageSid`: Ustaw ten pakiet SID dla Ciebie aplikacja systemu Windows z poziomu [Centrum deweloperów programu Windows](http://go.microsoft.com/fwlink/p/?linkid=266582&clcid=0x409).
4. `$WnsSecretkey`: Ustaw ten klucz tajny dla Ciebie aplikacja systemu Windows z poziomu [Centrum deweloperów programu Windows](http://go.microsoft.com/fwlink/p/?linkid=266582&clcid=0x409).

Zmienne są używane do połączenia do obszaru nazw i tworzenie koncentratora powiadomienie skonfigurowanym do obsługi powiadomień usługi powiadomień systemu Windows (WNS) przy użyciu poświadczeń WNS dla aplikacji dla systemu Windows. Aby uzyskać informacje dotyczące pakietu SID i klucz tajny wyświetlić, samouczek [Wprowadzenie koncentratory powiadomienie](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) . 

+ Używa wstawkę kodu skryptu `NamespaceManager` obiekt, aby sprawdzić, jeśli Centrum powiadomienie określone przez `$Path` istnieje.

+ Jeśli nie istnieje, skrypt utworzy `NotificationHubDescription` z WNS poświadczenia i przechodzić do `NamespaceManager` klasy `CreateNotificationHub` metody.

``` powershell

$Namespace = "<Enter your namespace>"
$Path  = "<Enter a name for your notification hub>"
$WnsPackageSid = "<your package sid>"
$WnsSecretkey = "<enter your secret key>"

$WnsCredential = New-Object -TypeName Microsoft.Azure.NotificationHubs.WnsCredential -ArgumentList $WnsPackageSid,$WnsSecretkey

# Query the namespace
$CurrentNamespace = Get-AzureSBNamespace -Name $Namespace

# Check if the namespace already exists
if ($CurrentNamespace)
{
    Write-Output "The namespace [$Namespace] in the [$($CurrentNamespace.Region)] region was found."

    # Create the NamespaceManager object used to create a new notification hub
    $sbr = Get-AzureSBAuthorizationRule -Namespace $Namespace
    Write-Output "Creating a NamespaceManager object for the [$Namespace] namespace..."
    $NamespaceManager = [Microsoft.Azure.NotificationHubs.NamespaceManager]::CreateFromConnectionString($sbr.ConnectionString);
    Write-Output "NamespaceManager object for the [$Namespace] namespace has been successfully created."

    # Check to see if the Notification Hub already exists
    if ($NamespaceManager.NotificationHubExists($Path))
    {
        Write-Output "The [$Path] notification hub already exists in the [$Namespace] namespace."  
    }
    else
    {
        Write-Output "Creating the [$Path] notification hub in the [$Namespace] namespace."
        $NHDescription = New-Object -TypeName Microsoft.Azure.NotificationHubs.NotificationHubDescription -ArgumentList $Path;
        $NHDescription.WnsCredential = $WnsCredential;
        $NamespaceManager.CreateNotificationHub($NHDescription);
        Write-Output "The [$Path] notification hub was created in the [$Namespace] namespace."
    }
}
else
{
    Write-Host "The [$Namespace] namespace does not exist."
}
```




## <a name="additional-resources"></a>Dodatkowe zasoby

- [Zarządzanie Bus usługi przy użyciu programu PowerShell](../service-bus-messaging/service-bus-powershell-how-to-provision.md)
- [Jak utworzyć Bus usługi kolejek, tematy i subskrypcji za pomocą skryptu programu PowerShell](http://blogs.msdn.com/b/paolos/archive/2014/12/02/how-to-create-a-service-bus-queues-topics-and-subscriptions-using-a-powershell-script.aspx)
- [Jak utworzyć Namespace Bus usługi i Centrum zdarzeń za pomocą skryptu programu PowerShell](http://blogs.msdn.com/b/paolos/archive/2014/12/01/how-to-create-a-service-bus-namespace-and-an-event-hub-using-a-powershell-script.aspx)

Niektóre gotowe skrypty są również dostępne do pobrania:
- [Usługa Bus skryptów programu PowerShell](https://code.msdn.microsoft.com/windowsazure/Service-Bus-PowerShell-a46b7059)
 

[Opcje zakupu]: http://azure.microsoft.com/pricing/purchase-options/
[Element członkowski ofert]: http://azure.microsoft.com/pricing/member-offers/
[Bezpłatna wersja próbna]: http://azure.microsoft.com/pricing/free-trial/
[Instalowanie i konfigurowanie programu PowerShell Azure]: ../powershell-install-configure.md
[Interfejs API programu .NET dla koncentratorów powiadomień]: https://msdn.microsoft.com/library/azure/mt414893.aspx
[Get-AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495122.aspx
[New-AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495165.aspx
[Get-AzureSBAuthorizationRule]: https://msdn.microsoft.com/library/azure/dn495113.aspx
 
