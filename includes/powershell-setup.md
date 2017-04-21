<properties services="virtual-machines" title="Setting up PowerShell" authors="JoeDavies-MSFT" solutions="" manager="timlt" editor="tysonn" />

<tags
   ms.service="virtual-machines"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm=""
   ms.workload="infrastructure"
   ms.date="05/12/2015"
   ms.author="rasquill" />

## <a name="setting-up-powershell"></a>Konfigurowanie programu PowerShell

Zanim będzie można używać Azure programu PowerShell, wykonaj następujące kroki.

### <a name="verify-powershell-versions"></a>Sprawdź wersje programu PowerShell

Zanim będzie można używać programu Windows PowerShell, musisz mieć programu Windows PowerShell, w wersji 3.0 lub 4.0. Aby znaleźć wersję programu Windows PowerShell, wpisz następujące polecenie w wierszu polecenia środowiska Windows PowerShell.

    $PSVersionTable

Powinien zostać wyświetlony podobny do tego.

    Name                           Value
    ----                           -----
    PSVersion                      3.0
    WSManStackVersion              3.0
    SerializationVersion           1.1.0.1
    CLRVersion                     4.0.30319.18444
    BuildVersion                   6.2.9200.16481
    PSCompatibleVersions           {1.0, 2.0, 3.0}
    PSRemotingProtocolVersion      2.2

Upewnij się, że wartość argumentu **PSVersion** jest 3.0 lub 4.0. Aby zainstalować wersję, zobacz [Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) lub [Windows Management Framework 4.0](http://www.microsoft.com/download/details.aspx?id=40855).

Trzeba również Azure programu PowerShell wersji 0.8.0 lub nowszej. Możesz sprawdzić wersję programu PowerShell Azure, zainstalowanym przy użyciu tego polecenia w wierszu polecenia programu PowerShell Azure.

    Get-Module azure | format-table version

Powinien zostać wyświetlony podobny do tego.

    Version
    -------
    0.8.16.1

Aby uzyskać instrukcje i łącza do najnowszej wersji zobacz [jak instalowanie i konfigurowanie programu PowerShell Azure](powershell-install-configure.md).


### <a name="set-your-azure-account-and-subscription"></a>Ustaw swoje konto Azure i subskrypcji

Jeśli nie masz jeszcze subskrypcji usługi Azure, można aktywować swojej [korzyści subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) lub znak w górę do [bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/).

Otwórz wiersz polecenia programu PowerShell Azure i zaloguj się do Azure za pomocą tego polecenia.

    Add-AzureAccount

Jeśli masz wiele subskrypcji Azure, można wyświetlić listę subskrypcji Azure za pomocą tego polecenia.

    Get-AzureSubscription

Zostanie wyświetlony następujący typ informacji:

    SubscriptionId            : fd22919d-eaca-4f2b-841a-e4ac6770g92e
    SubscriptionName          : Visual Studio Ultimate with MSDN
    Environment               : AzureCloud
    SupportedModes            : AzureServiceManagement,AzureResourceManager
    DefaultAccount            : johndoe@contoso.com
    Accounts                  : {johndoe@contoso.com}
    IsDefault                 : True
    IsCurrent                 : True
    CurrentStorageAccountName : 
    TenantId                  : 32fa88b4-86f1-419f-93ab-2d7ce016dba7

Można ustawić bieżącej subskrypcji Azure, uruchamiając następujące polecenia w wierszu polecenia programu PowerShell Azure. Zamień wszystko w obrębie ofert, łącznie z < i > znaki, z prawidłową nazwę.

    $subscr="<SubscriptionName from the display of Get-AzureSubscription>"
    Select-AzureSubscription -SubscriptionName $subscr -Current 

Aby uzyskać więcej informacji na temat Azure subskrypcje i kont Zobacz [jak: nawiązywanie połączenia z subskrypcji](powershell-install-configure.md#Connect).
